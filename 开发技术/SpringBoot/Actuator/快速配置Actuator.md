我竟然发现我一直没有整理这个，今天急用，找不到相关的笔记。

pom配置如下：

~~~

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>

~~~

application.yml配置如下：

~~~

management:
  endpoints:
    web:
      exposure:
        include: '*'

~~~

观察日志：

![2021-06-28-16-27-25](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-06-28-16-27-25.png)

我有几次这样配置了，接口还是无法访问，重启多次后才恢复，我不确定是哪块出问题了，所以建议看到日志文件后再访问接口。