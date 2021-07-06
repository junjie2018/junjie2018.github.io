## @Configuration的proxyBeanMethods参数

entity包

User.java

~~~ java

@NoArgsConstructor
@AllArgsConstructor
public class User {
    private String name;
}

~~~

Pet.java

~~~ java

@NoArgsConstructor
@AllArgsConstructor
public class Pet {
    private String name;
}

~~~

config包

TmpConfiguration.java

~~~ java

@Configuration(proxyBeanMethods = false)
public class TmpConfigration {

    @Bean
    public User user() {
        return new User("张三");
    }

    @Bean
    public Pet pet() {
        return new Pet("旺财");
    }
}

~~~

三段测试代码：

~~~ java

// 实验一
for (String name : configurableApplicationContext.getBeanDefinitionNames()) {
    System.out.println(name);
}

~~~

实验一主要验证了加了@Bean注解的方法名，将会作为该Bean在容器中的名字。

~~~ java

// 实验二
System.out.println(configurableApplicationContext.getBean(TmpConfigration.class));
System.out.println(configurableApplicationContext.getBean(TmpConfigration.class));

System.out.println(configurableApplicationContext.getBean(User.class));
System.out.println(configurableApplicationContext.getBean(User.class));

System.out.println(configurableApplicationContext.getBean(Pet.class));
System.out.println(configurableApplicationContext.getBean(Pet.class));

~~~

实验二验证了proxyBeanMethods无论是true或者false，都不会影响从容器中通过get方法获取Bean（这些Bean始终都是单例）

~~~ java

// 实验三
TmpConfigration tmpConfigration = configurableApplicationContext.getBean(TmpConfigration.class);

System.out.println(tmpConfigration.user());
System.out.println(tmpConfigration.user());
System.out.println(tmpConfigration.user());

System.out.println(tmpConfigration.pet());
System.out.println(tmpConfigration.pet());
System.out.println(tmpConfigration.pet());

~~~

实验三验证了如果proxyBeanMethods设置为true，则即使通过Configuration Bean的方法获取Bean，这些Bean都是单例的。如果proxyBeanMethods设置为false，则通过Configuration Bean的方法获取Bean，这些Bean不是单例的。

### 小结

