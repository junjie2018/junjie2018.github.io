我确实有深拷贝的需求，我之前的方案是实现了一个静态工具类，在工具类中通过FastJson序列化和反序列化实现深拷贝（我主要用在工具类中，性能问题并不在意），但是毕竟不是非常的方便。最近找资料时发现了几款工具，打算测试一下这几款工具。

## commons-beanutils

依赖引入：

~~~ xml

<dependency>
    <groupId>commons-beanutils</groupId>
    <artifactId>commons-beanutils</artifactId>
    <version>1.9.4</version>
</dependency>

~~~

测试代码为：

~~~ java

public class Test {

    @Getter
    @Setter
    @NoArgsConstructor
    @AllArgsConstructor
    static class User {
        private String name;
        private Address address;
    }

    @NoArgsConstructor
    @AllArgsConstructor
    static class Address {
        private String country;
        private String province;
        private String city;
    }

    public static void main(String[] args) {
        User user1 = new User("test", new Address("a", "b", "c"));
        User user2 = new User();

        BeanUtils.copyProperties(user1, user2);

        System.out.println(user1);
        System.out.println(user2);

        System.out.println(user1.address);
        System.out.println(user2.address);
    }

}

~~~

输出为：

~~~

com.example.demo.test.Test$User@18e8568
com.example.demo.test.Test$User@33e5ccce
com.example.demo.test.Test$Address@5a42bbf4
com.example.demo.test.Test$Address@5a42bbf4

~~~

实际上只进行了浅拷贝。

## orika

依赖引入：

~~~ xml

<dependency>
    <groupId>ma.glasnost.orika</groupId>
    <artifactId>orika-core</artifactId>
    <version>1.5.4</version>
</dependency>

~~~

测试代码编写：

~~~ java

public class Test {

    @Getter
    @Setter
    @NoArgsConstructor
    @AllArgsConstructor
    static class User {
        private String name;
        private Address address;
    }

    @NoArgsConstructor
    @AllArgsConstructor
    static class Address {
        private String country;
        private String province;
        private String city;
    }

    public static void main(String[] args) {
        User user1 = new User("test", new Address("a", "b", "c"));
        User user2 = new User();

        MapperFactory mapperFactory = new DefaultMapperFactory.Builder()
                .useAutoMapping(true)
                .mapNulls(true)
                .build();
        MapperFacade mapperFacade = mapperFactory.getMapperFacade();

        mapperFacade.map(user1, user2);

        System.out.println("mapperFacade.map(user1, user2)");

        System.out.println(user1);
        System.out.println(user2);

        System.out.println(user1.address);
        System.out.println(user2.address);

        System.out.println("User user3 = mapperFacade.map(user1, User.class)");

        User user3 = mapperFacade.map(user1, User.class);
        System.out.println(user1);
        System.out.println(user3);

        System.out.println(user1.address);
        System.out.println(user3.address);

    }

}

~~~

输出为：

~~~

mapperFacade.map(user1, user2)

com.example.demo.test.Test$User@6043cd28
com.example.demo.test.Test$User@4d3167f4
com.example.demo.test.Test$Address@76a3e297
com.example.demo.test.Test$Address@ed9d034

User user3 = mapperFacade.map(user1, User.class)

com.example.demo.test.Test$User@6043cd28
com.example.demo.test.Test$User@1060b431
com.example.demo.test.Test$Address@76a3e297
com.example.demo.test.Test$Address@612679d6

~~~

可以看到是深拷贝。这个工具略微有点麻烦，需要开发成静态工具类，但是看推荐，说是效率极高。

## BeanUtils（Spring版）、BeanUtil（hutool）

这两款都是浅拷贝，这款就不呈现测试代码了。

## 测试总结

我觉得orika是有研究价值的，深度拷贝的应用还是蛮多的，我会花时间对这门技术进行系统学习。

## 参考资料

1. [java浅拷贝与深拷贝及拷贝工具推荐](https://blog.csdn.net/qq_31086797/article/details/106558379)