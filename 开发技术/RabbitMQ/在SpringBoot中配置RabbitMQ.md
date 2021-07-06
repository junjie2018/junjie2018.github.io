## 操作步骤

1. 引入依赖

~~~ xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-amqp</artifactId>
    <version>2.3.2.RELEASE</version>
</dependency>
~~~

2. 准备配置文件

~~~ yml
spring:
  rabbitmq:
    host: 192.168.30.174
    port: 5672
    username: admin
    password: 123456
    virtual-host: /
~~~

3. 准备配置类

~~~ java
package cn.watsons.mmp.brand.api.config;

import org.springframework.amqp.core.Queue;
import org.springframework.amqp.rabbit.config.SimpleRabbitListenerContainerFactory;
import org.springframework.amqp.rabbit.connection.CachingConnectionFactory;
import org.springframework.amqp.rabbit.core.RabbitTemplate;
import org.springframework.amqp.support.converter.SimpleMessageConverter;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;


@Configuration
public class RabbitConfig {
    @Bean
    public Queue Queue() {
        return new Queue("hello");
    }
}
~~~

4. 准备测试文件

~~~ java
package cn.watsons.mmp.brand.api.rabbit;

import cn.watsons.mmp.brand.api.BrandMemberApiApplication;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.amqp.core.AmqpTemplate;
import org.springframework.amqp.rabbit.core.RabbitTemplate;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;

import java.util.Date;

@RunWith(SpringRunner.class)
@SpringBootTest(classes = {BrandMemberApiApplication.class})
public class RabbitMQTest {
    @Autowired
    private AmqpTemplate rabbitTemplate;

    @Test
    public void send() {
        String context = "hello " + new Date();
        System.out.println("Sender : " + context);

        this.rabbitTemplate.convertAndSend("hello", context);
    }
}

~~~

## 相关教程

1. [Spring Boot(八)：RabbitMQ 详解](http://www.ityouknow.com/springboot/2016/11/30/spring-boot-rabbitMQ.html)