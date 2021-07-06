## 问题描述

1. 在配置ShardingSphere-Jdbc时，按照官网官网最新配置，遇到了jdbcUrl is required with driverClassName异常

## 解决步骤

1. 修改配置中的url为jdbc-url，代码如下：

~~~
spring:
  shardingsphere:
    props:
      sql:
        show: true

    datasource:
      names: mmp
      mmp:
        # jdbc-url: jdbc:mysql://192.168.53.100:3306/mmp_watsons_junjie?useUnicode=true&characterEncoding=UTF8&serverTimezone=CTT
        jdbc-url: jdbc:mysql://192.168.53.100:3306/mmp_watsons_junjie?useUnicode=true&characterEncoding=UTF8&serverTimezone=CTT
        type: com.zaxxer.hikari.HikariDataSource
        driver-class-name: com.mysql.cj.jdbc.Driver
        username: promotion_app
        password: promotion_app
~~~

20210617后续：

很尴尬啊，这篇笔记是很久前整理的，我忘记我当时修改什么东西了

## 参考资料
1. [jdbcUrl is required with driverClassName](https://www.cnblogs.com/jpfss/p/11083472.html)