1. 依赖文件

~~~

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>

~~~

2. application.yml文件

~~~

spring:
  datasource:
#    driver-class-name: org.postgresql.Driver                                                             # 原始的
    driver-class-name: com.p6spy.engine.spy.P6SpyDriver                                                   # 使用p6spy后的
    password: HelloWorld
#    url: jdbc:postgresql://192.168.13.68:5432/postgres?currentSchema=public&stringtype=unspecified       # 原始的
    url: jdbc:p6spy:postgresql://192.168.13.68:5432/postgres?currentSchema=public&stringtype=unspecified  # 使用p6spy后的
    username: postgres

~~~

## 参考资料

1. [spring boot整合使用JdbcTemplate之详解!!](https://blog.csdn.net/zty1317313805/article/details/79710488)