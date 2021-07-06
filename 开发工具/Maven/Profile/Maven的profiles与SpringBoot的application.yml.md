我们的项目有一个比较便利的功能，如图，当我们选择了local后，我们启动SpringBoot项目时，将自动使用application-local.yml配置文件，当我们选择sit后，则会使用application-sit.yml配置文件。

![2021-07-01-22-00-16](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-07-01-22-00-16.png)

该功能是如何实现的呢，如图，在我们公司自己的starter-parent上有如下的配置：

![2021-07-01-22-07-06](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-07-01-22-07-06.png)

而在我们的bootstrap.properties中有如下配置：

![2021-07-01-22-08-05](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-07-01-22-08-05.png)

我认为项目在启动的时候，会将`@spring.profiles.active@`转换为相应的Maven Profile的值，从而实现使用相应的配置文件。

## 对上述分析的验证

我准备了如下配置文件：

~~~ 

# application.properties

spring.profiles.active=@spring.profiles.active@

# applicaiton-local.yml

tmp:
  key: local

# applicaiton-sit.yml

tmp:
  key: sit

~~~

我准备了如下配置类：

~~~ java

@Data
@Component
@ConfigurationProperties(prefix = "tmp")
public class TmpConfiguration {
    private String key;
}

~~~

在测试类中，我们发现当我们选择了不同的Maven Profile后，tmpConfiguration中的key值将会为local或者sit。

实验基本证实了我的想法。

## 还存在的问题

我在实验的过程中又产生了一些新的问题，如下：

1. `@spring.profiles.active@`只能写在application.properties中，不能写在application.yml中，如果写在application.yml中，则会在启动时报如下的错误（已解决，参考同目录下的笔记）：

~~~

Caused by: while scanning for the next token
found character '@' that cannot start any token. (Do not use @ for indentation)
 in 'reader', line 3, column 13:
        active: @spring.profiles.active@

~~~

那么如果我想达到和application.properties相同的配置效果，我又该如何写这个值呢？

2. 我目前不知道Maven Profile与SpringBoot项目的关系，application.properties中是如何获取我们在Maven Profile中配置的Profile呢？（已解决）
