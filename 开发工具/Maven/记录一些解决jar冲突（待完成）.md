我想通过SpringBoot Actuator查看正在运行的应用的一些配置信息，当我引入如下依赖后，启动引用报如下错误：

~~~

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>

~~~

~~~

SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/D:/MavenRepository/repository/org/apache/logging/log4j/log4j-slf4j-impl/2.13.3/log4j-slf4j-impl-2.13.3.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/D:/MavenRepository/repository/ch/qos/logback/logback-classic/1.2.3/logback-classic-1.2.3.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.apache.logging.slf4j.Log4jLoggerFactory]
Exception in thread "main" java.lang.ExceptionInInitializerError
	at com.sdstc.dyf.provider.StartDyfProviderTest.main(StartDyfProviderTest.java:26)
Caused by: org.apache.logging.log4j.LoggingException: log4j-slf4j-impl cannot be present with log4j-to-slf4j
	at org.apache.logging.slf4j.Log4jLoggerFactory.validateContext(Log4jLoggerFactory.java:49)
	at org.apache.logging.slf4j.Log4jLoggerFactory.newLogger(Log4jLoggerFactory.java:39)
	at org.apache.logging.slf4j.Log4jLoggerFactory.newLogger(Log4jLoggerFactory.java:30)
	at org.apache.logging.log4j.spi.AbstractLoggerAdapter.getLogger(AbstractLoggerAdapter.java:54)
	at org.apache.logging.slf4j.Log4jLoggerFactory.getLogger(Log4jLoggerFactory.java:30)
	at org.slf4j.LoggerFactory.getLogger(LoggerFactory.java:363)
	at org.apache.commons.logging.LogAdapter$Slf4jAdapter.createLocationAwareLog(LogAdapter.java:130)
	at org.apache.commons.logging.LogAdapter.createLog(LogAdapter.java:91)
	at org.apache.commons.logging.LogFactory.getLog(LogFactory.java:67)
	at org.apache.commons.logging.LogFactory.getLog(LogFactory.java:59)
	at org.springframework.boot.SpringApplication.<clinit>(SpringApplication.java:196)
	... 1 more

~~~

因为我们的项目使用了Maven的SnapShot版本机制，在确定该问题不是同事造成的后，我认为该报错可能是因为jar包冲突。跟架构师了解到，我们的框架默认开启了spring-boot-starter-actuator，所以不需要在引入依赖，我暂时先放置这个问题吧。