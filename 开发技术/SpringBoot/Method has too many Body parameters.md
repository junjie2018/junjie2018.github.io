## 问题描述

报错如下：

~~~ 

Caused by: java.lang.IllegalStateException: Method has too many Body parameters: public abstract void com.sdstc.authcenter.client.UserConfigClient.updateUserListConfig(java.lang.String,com.sdstc.authcenter.request.UpdateListConfigRequest)
	at feign.Util.checkState(Util.java:127) ~[feign-core-10.1.0.jar:?]
	at feign.Contract$BaseContract.parseAndValidateMetadata(Contract.java:117) ~[feign-core-10.1.0.jar:?]
	at org.springframework.cloud.openfeign.support.SpringMvcContract.parseAndValidateMetadata(SpringMvcContract.java:188) ~[spring-cloud-openfeign-core-2.1.1.RELEASE.jar:2.1.1.RELEASE]
	at feign.Contract$BaseContract.parseAndValidatateMetadata(Contract.java:66) ~[feign-core-10.1.0.jar:?]
	at feign.ReflectiveFeign$ParseHandlersByName.apply(ReflectiveFeign.java:154) ~[feign-core-10.1.0.jar:?]
	at feign.ReflectiveFeign.newInstance(ReflectiveFeign.java:52) ~[feign-core-10.1.0.jar:?]
	at feign.Feign$Builder.target(Feign.java:251) ~[feign-core-10.1.0.jar:?]
	at org.springframework.cloud.openfeign.HystrixTargeter.target(HystrixTargeter.java:36) ~[spring-cloud-openfeign-core-2.1.1.RELEASE.jar:2.1.1.RELEASE]
	at org.springframework.cloud.openfeign.FeignClientFactoryBean.getTarget(FeignClientFactoryBean.java:284) ~[spring-cloud-openfeign-core-2.1.1.RELEASE.jar:2.1.1.RELEASE]
	at org.springframework.cloud.openfeign.FeignClientFactoryBean.getObject(FeignClientFactoryBean.java:247) ~[spring-cloud-openfeign-core-2.1.1.RELEASE.jar:2.1.1.RELEASE]
	at org.springframework.beans.factory.support.FactoryBeanRegistrySupport.doGetObjectFromFactoryBean(FactoryBeanRegistrySupport.java:171) ~[spring-beans-5.2.4.RELEASE.jar:5.2.4.RELEASE]
	at org.springframework.beans.factory.support.FactoryBeanRegistrySupport.getObjectFromFactoryBean(FactoryBeanRegistrySupport.java:101) ~[spring-beans-5.2.4.RELEASE.jar:5.2.4.RELEASE]
	at org.springframework.beans.factory.support.AbstractBeanFactory.getObjectForBeanInstance(AbstractBeanFactory.java:1818) ~[spring-beans-5.2.4.RELEASE.jar:5.2.4.RELEASE]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.getObjectForBeanInstance(AbstractAutowireCapableBeanFactory.java:1266) ~[spring-beans-5.2.4.RELEASE.jar:5.2.4.RELEASE]
	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:260) ~[spring-beans-5.2.4.RELEASE.jar:5.2.4.RELEASE]
	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:202) ~[spring-beans-5.2.4.RELEASE.jar:5.2.4.RELEASE]
	at org.springframework.beans.factory.config.DependencyDescriptor.resolveCandidate(DependencyDescriptor.java:276) ~[spring-beans-5.2.4.RELEASE.jar:5.2.4.RELEASE]
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.addCandidateEntry(DefaultListableBeanFactory.java:1510) ~[spring-beans-5.2.4.RELEASE.jar:5.2.4.RELEASE]
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.findAutowireCandidates(DefaultListableBeanFactory.java:1467) ~[spring-beans-5.2.4.RELEASE.jar:5.2.4.RELEASE]
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.doResolveDependency(DefaultListableBeanFactory.java:1250) ~[spring-beans-5.2.4.RELEASE.jar:5.2.4.RELEASE]
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.resolveDependency(DefaultListableBeanFactory.java:1207) ~[spring-beans-5.2.4.RELEASE.jar:5.2.4.RELEASE]
	at org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor$AutowiredFieldElement.inject(AutowiredAnnotationBeanPostProcessor.java:640) ~[spring-beans-5.2.4.RELEASE.jar:5.2.4.RELEASE]
	... 19 more


~~~

Feign客户端代码：

~~~

// Version 1
@PostMapping("/updateUserListConfig")
void updateUserListConfig(
    @RequestAttribute(APICons.REQUEST_USER_ID) @NotEmpty String userId,
    @RequestBody @Validated UpdateListConfigRequest request); 

// Version 2
@PostMapping("/updateUserListConfig")
void updateUserListConfig(
    String userId,
    @RequestBody @Validated UpdateListConfigRequest request); 

~~~

以上两种写法都不可以，会导致Feign消费端报错。

## 解决方案

如下写法不会报错：

~~~ java

@PostMapping("/updateUserListConfig")
void updateUserListConfig(
    @RequestParam("tmp") String userId, 
    @RequestBody @Validated UpdateListConfigRequest request);

~~~

截止目前，已经从技术上解决这个报错的问题了。接下来我谈一谈我在项目中如何修复该问题，及我的思考。

我们之所以在项目中使用@RequestAttribute是因为通过网关进入到服务内部时，网关会根据Token取得用户的一些信息，并塞到请求里的Header中，然后请求到下一个服务。我们的Controller通过@RequestAttribute获得的属性可以快速进行逻辑处理。理论上，服务会将这个Header信息存储起来，在下一跳请求中，塞上这个Header（具体的技术细节可以参考如何塞traceId）。下一个服务于是就可以同样使用该Header信息，并使用@RequestAttribute注解。

我们的问题在于，我们将这样的一份Controller复制成了Client，只删除了方法体，而没有删除@RequestAttribute。最后导致了Feign服务端导入失败。从技术上讲，复制的时候删除所有@RequestAttribute参数，这个问题也不会出现了。

我最后怎么改，我将其全部改为了@RequestParam，因为我得到消息说，我们内部服务之间的请求可能不包含Header信息，而且，我们有些接口可能是从MQ消费者请求的，这些MQ消费者是没有任何Token信息的，它们发出的请求也就没有Header了。

很糟糕的开发体验哦，一种非常好的框架，结果不能发挥出其强大的作用。如何避免这个问题呢？我觉得区分内部接口和外部接口可能会有点作用（需要在路径、Client命名上进行区分），所有可能被网关直接、间接访问到的接口都是外部接口，外部接口是一定可以使用@RequestAttribute属性的；除此之外的接口都是内部接口，内部接口主要服务于MQ等。

这可能会带来一些冗余，一份相同功能的接口将会存在两个版本，其区别仅仅是Controller接受参数的方式不同。我这边能给到的建议只是：Controller层只做参数的简单处理，Service层做真正的逻辑，这样Service层的复用性增加，可以稍微减少冗余带来的不爽快的体验。（实际开发中，肯定是假设所有的接口都是外部接口，等到MQ有需求后，再去开发一个内部接口，开发内部接口时，如果有Service有机会复用，这个时候重构下代码就好了）

这里面可能还存在一个问题：可能有一部分外部接口需要调到内部接口，这种事情应该绝对不可以发生，这个需要从规则层面限制。还有一个问题：可能一个接口会被网关调用的接口间接调用，但是他实际上不应该暴露给网关，我们该处理这个问题（这是一个非常常见的需求），如何处理这个问题？我对这个问题的提出的解决方法是，每个controller中的方法都写完整的路径名（不要使用类上的@RequestMapping来提升一丁点的复用性），通过在路径名最前方上加internel。然后在网关上配置，确保internel不会暴露给外部。 *（这个地方应该设计以下关键字，使用internel关键字可能会让人疑惑）*

项目管理方面，服务提供方，大概需要一个服务于外部接口的Feign客户端，加一个服务于内部接口的Feign客户端。

服务部署时，我们也可以将提供外部接口的服务和提供内部接口的服务分开部署，尽管它们是同一套代码。

这件事没想到还有后续：在后续对项目的研究中，我发现我们的确是存在两种Controller，一种服务于外部，没有找到相关的Feign Client；一种是服务于内部，其Controller以I打头，会存在相关的Feign Client，服务于内部的的Controller的方法的返回值，没有使用ResponseVo包装，其方法没有使用@RequestAttribute注解标注。这样的设计，可能最终会降低服务于外部的接口的复用性，且最后导致接口管理混乱。

## 小结

综上，接口存在以下几种：

1. 被网关直接调用的接口
2. 被网关间接调用的接口，其本身也可能被网关直接调用
3. 被网关间接调用的接口，其本身不应该被网关调用
4. 服务于MQ消费者的接口


对上述内容总结如下：

1. 从网关进入的请求，请求的接口，及这些接口请求到的内部服务的接口都视为外部接口，其他的接口视为内部接口
2. 网关的一个请求将会形成请求树，这个树上的每个节点都需要是外部接口
3. 外部接口可以使用@RequestAttribute注解，返回值需要用RequestVo包装
4. 服务提供方提供两份Feign客户端代码用于调用服务于外部的接口，一份用于调用服务于内部的接口。
5. 部署服务提供方时需要部署两份，一份为外部接口提供服务，一份为内部接口提供服务

我将会持续关注这部分，收集相关的需求，并完善这部分的文档。