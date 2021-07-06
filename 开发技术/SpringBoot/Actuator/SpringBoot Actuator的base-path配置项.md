SpringBoot Actuator默认的地址是在`server.servlet.context-path`的基础上加上`actuator/*`，这个细节，我之前一直没有关注到。`management.endpoints.web.base-path`配置实际上只会影响到`actuator`段。

举一个例子，如果项目按如下配置了context-path，那么actuator的地址就为：localhost:50001/demo/actuator

~~~

server:
  port: 50001
  servlet:
    context-path: /demo

~~~

此时如果修改了`management.endpoints.web.base-path`且值为tmp-actuator，那么actuator的地址就为：localhost:50001/demo/tmp-actuator

这样其实挺好的，这样的话我们的actuator请求可以非常轻松的通过网关，不需要在额外配合什么东西了。

## 参考资料

好东西，慢慢消化~~~

[Spring Boot (十九)：使用 Spring Boot Actuator 监控应用](http://www.ityouknow.com/springboot/2018/02/06/spring-boot-actuator.html)
