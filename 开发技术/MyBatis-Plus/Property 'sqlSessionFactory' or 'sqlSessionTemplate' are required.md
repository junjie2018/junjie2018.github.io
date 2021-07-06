SpringBoot集成MyBatis-Plus后，启动时报如下错误：

~~~

org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'userMapper' defined in file [D:\Project\Mybatis\target\classes\fun\junjie\mybatis\mapper\UserMapper.class]: Invocation of init method failed; nested exception is java.lang.IllegalArgumentException: Property 'sqlSessionFactory' or 'sqlSessionTemplate' are required
  at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.initializeBean(AbstractAutowireCapableBeanFactory.java:1794) ~[spring-beans-5.2.12.RELEASE.jar:5.2.12.RELEASE]
  at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:594) ~[spring-beans-5.2.12.RELEASE.jar:5.2.12.RELEASE]
  at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:516) ~[spring-beans-5.2.12.RELEASE.jar:5.2.12.RELEASE]
  at org.springframework.beans.factory.support.AbstractBeanFactory.lambda$doGetBean$0(AbstractBeanFactory.java:324) ~[spring-beans-5.2.12.RELEASE.jar:5.2.12.RELEASE]
  at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:234) ~[spring-beans-5.2.12.RELEASE.jar:5.2.12.RELEASE]
  at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:322) ~[spring-beans-5.2.12.RELEASE.jar:5.2.12.RELEASE]
  at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:202) ~[spring-beans-5.2.12.RELEASE.jar:5.2.12.RELEASE]
  at org.springframework.beans.factory.support.DefaultListableBeanFactory.preInstantiateSingletons(DefaultListableBeanFactory.java:878) ~[spring-beans-5.2.12.RELEASE.jar:5.2.12.RELEASE]
  at org.springframework.context.support.AbstractApplicationContext.finishBeanFactoryInitialization(AbstractApplicationContext.java:879) ~[spring-context-5.2.12.RELEASE.jar:5.2.12.RELEASE]
  at org.springframework.context.support.AbstractApplicationContext.refresh(AbstractApplicationContext.java:551) ~[spring-context-5.2.12.RELEASE.jar:5.2.12.RELEASE]
  at org.springframework.boot.SpringApplication.refresh(SpringApplication.java:758) [spring-boot-2.3.7.RELEASE.jar:2.3.7.RELEASE]
  at org.springframework.boot.SpringApplication.refresh(SpringApplication.java:750) [spring-boot-2.3.7.RELEASE.jar:2.3.7.RELEASE]
  at org.springframework.boot.SpringApplication.refreshContext(SpringApplication.java:405) [spring-boot-2.3.7.RELEASE.jar:2.3.7.RELEASE]
  at org.springframework.boot.SpringApplication.run(SpringApplication.java:315) [spring-boot-2.3.7.RELEASE.jar:2.3.7.RELEASE]
  at org.springframework.boot.SpringApplication.run(SpringApplication.java:1237) [spring-boot-2.3.7.RELEASE.jar:2.3.7.RELEASE]
  at org.springframework.boot.SpringApplication.run(SpringApplication.java:1226) [spring-boot-2.3.7.RELEASE.jar:2.3.7.RELEASE]
  at fun.junjie.mybatis.MybatisApplication.main(MybatisApplication.java:12) [classes/:na]
Caused by: java.lang.IllegalArgumentException: Property 'sqlSessionFactory' or 'sqlSessionTemplate' are required
  at org.springframework.util.Assert.notNull(Assert.java:201) ~[spring-core-5.2.12.RELEASE.jar:5.2.12.RELEASE]
  at org.mybatis.spring.support.SqlSessionDaoSupport.checkDaoConfig(SqlSessionDaoSupport.java:122) ~[mybatis-spring-2.0.6.jar:2.0.6]
  at org.mybatis.spring.mapper.MapperFactoryBean.checkDaoConfig(MapperFactoryBean.java:73) ~[mybatis-spring-2.0.6.jar:2.0.6]
  at org.springframework.dao.support.DaoSupport.afterPropertiesSet(DaoSupport.java:44) ~[spring-tx-5.2.12.RELEASE.jar:5.2.12.RELEASE]
  at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.invokeInitMethods(AbstractAutowireCapableBeanFactory.java:1853) ~[spring-beans-5.2.12.RELEASE.jar:5.2.12.RELEASE]
  at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.initializeBean(AbstractAutowireCapableBeanFactory.java:1790) ~[spring-beans-5.2.12.RELEASE.jar:5.2.12.RELEASE]
  ... 16 common frames omitted

~~~

经过分析发现，是我依赖引入错误：

~~~

原来：
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus</artifactId>
    <version>3.4.3.1</version>
</dependency>

改为：
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.4.3.1</version>
</dependency>

~~~

## 参考资料

1. [java.lang.IllegalArgumentException: Property 'sqlSessionFactory' or 'sqlSessionTemplate' are require](https://blog.csdn.net/qq_37495786/article/details/97636255)