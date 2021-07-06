使用场景是这样的，一个记录配置信息的类，想使用静态方法、静态字段记录一些存储在配置文件中的信息。我选择定义这些静态的字段，然后再定义一些需要注入的字段，然后在一个方法中实现注入的字段往静态的字段中赋值的操作。

大概代码如下：

~~~ java
 
@Component
public class Config{
    @Autowire
    private String configItem1;
    
    @Autowire
    private String configItem2;

    public static String CONFIG_ITEM_1;
    public static String CONFIG_ITEM_2;

    @PostConstruct
    public void init(){
        CONFIG_ITEM_1 = configItem1;
        CONFIG_ITEM_2 = configItem2;
    }
}

~~~

目前这种写法我觉得非常的不优雅，原因有下：

1. 我定义了两种含义一样的字段，只是一个是静态的，一个是对象的

如何解决这个问题了，我想到的方案是使用将@Autowire放置在set方法上，这样甚至可以免去init方法的开发，但是这样不是没有问题的：目前我的编码风格已经习惯性使用构造函数注入，最不济也是将@Autowire写在字段上，为了使用静态的信息，我需要将@Autowire写在方法上，这样非常不符合我的编码习惯。

## 参考资料

1. [Spring 容器启动完成后，执行初始化加载工作](https://blog.csdn.net/xiaojin21cen/article/details/83418470)
2. [SpringBoot使用@Value给静态变量注入值](https://blog.csdn.net/mononoke111/article/details/81088472)