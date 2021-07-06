我们一部分接口在local环境并调不通，我将这些接口写在了manage层，我希望我有个开关当我在本地环境运行的时候，这些接口可以不被调用，我想到了使用SpringBoot的环境。

如下代码：

~~~ java

@Value("${spring.profiles.active}")
private String env;

public void test(){
    if("local".equal(env)){
       return; 
    }
}

~~~

我最后放弃这么搞了，因为这种方案在我们项目组中并不是很流行，我担心会给其他同事带来困惑。

## 参考资料

1. [SpringBoot获取当前运行环境三种方式](https://blog.csdn.net/qq_27818541/article/details/105719962)