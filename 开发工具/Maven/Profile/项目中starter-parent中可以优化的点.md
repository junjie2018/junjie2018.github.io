我注意到我们的starter-parent配置中有如下代码：

~~~ xml

<build>
    <resources>
        <resource>
            <directory>src/main/resources/</directory>
            <filtering>true</filtering>
        </resource>
        <resource>
            <directory>src/main/resources</directory>
            <!--先排除所有的配置文件-->
            <excludes>
                <exclude>application*.yml</exclude>
            </excludes>
        </resource>
        <resource>
            <directory>src/main/resources</directory>
            <!--引入所需环境的配置文件-->
            <filtering>true</filtering>
            <includes>
                <include>application.yml</include>
                <include>application-${spring.profiles.active}.yml</include>
            </includes>
        </resource>
    </resources>
</build>

~~~

这段配置是说，当我们打包处理资源文件时我们先不包含所有的application*.yml文件。然后application.yml文件，及application-${spring.profiles.active}.yml文件。

这段配置完全没有存在的必要，主要有一下的原因：

1. SpringBoot打包插件默认开启过滤器，会对进行@@字符串过滤的处理。
2. 我们项目中使用的是bootstrap.properties和bootstrap-xxx.yml，所以这个配置完全起不到任何作用