其实相关的技术我很早前就用在了项目中，但是一直没有整理笔记，最近在重构我工具包的代码时，又使用到了相关的技术，所以顺便整理一下。我工具包有个工具类，其中的方法都是静态方法，但是这些静态方法会根据配置文件呈现出不同的功能。

配置文件的配置的获取我还是使用的是传统的方法，使用一个`@ConfigurationProperties(prefix = "xxx")`注解，那么我工具类中需要只用配置类的时候，我就需要先注入其中，我之前的写法如下：

~~~ java

@Component
@RequiredArgsConstructor
public class YamlUtils {

    private final ToolsConfig toolsConfig;
    private final ProjectConfig projectConfig;
    private static ToolsConfig toolsConfigStatic;
    private static ProjectConfig projectConfigStatic;

    @PostConstruct
    public void init() {
        toolsConfigStatic = toolsConfig;
        projectConfigStatic = projectConfig;
    }
}

~~~

我觉得非常的怪异，不是很满意。经过调整后，我的写法如下：

~~~ java

@Component
@RequiredArgsConstructor
public class YamlUtils {

    private final ApplicationContext applicationContext;

    private static ToolsConfig toolsConfig;
    private static ProjectConfig toolsConfig;

    @PostConstruct
    public void init() {
        projectConfig = applicationContext.getBean(ProjectConfig.class);
        toolsConfig = applicationContext.getBean(ToolsConfig.class);
    }
}

~~~

这种写法相对于之前的写法稍微优雅一点点，不需要同时提供一个对象字段和类字段了，而且两者的含义是一样的。但是其实我还是不太满意，我更期待的是`ApplicationContext`都不需要注入，而是通过一个工具类获取，或者存在支持静态字段注入的注解（目前没有找到相关的资料）。

## 参考资料

1. [从Spring 应用上下文获取 Bean 的常用姿势](https://segmentfault.com/a/1190000020918545)

2. [spring项目中获取ApplicationContext对象，然后手动获取bean](https://blog.csdn.net/ex_tang/article/details/82749844)
   
   提到了一个继承ApplicationContextAware的方案，感觉很高级，但是在实践中获取到的bean为null，我也不知道为什么。