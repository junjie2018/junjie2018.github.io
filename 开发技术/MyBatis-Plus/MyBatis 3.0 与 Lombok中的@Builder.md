## 问题描述

PG库中的字段类型为varchar，实体中字段类型为String，PO的简化后结构如下：

~~~ java

@Data
@Builder
@EqualsAndHashCode(callSuper = false)
@TableName(value = "t_dyf_field", autoResultMap = true)
class FieldVo extends BaseVo {
    private String id;
    private Integer dataType
}

~~~

通过MyBatis-Plus的BaseMapper中的selectOne方法查询实体的时候报了如下错误：

~~~


2021-04-19 11:44:45.580 INFO  StartupInfoLogger.java:61- [b3=,TraceId=,SpanId=,ParentSpanId=,Export=,Sampled=,Flags=] [Org=,User=] Started StartDyfProviderTest in 12.897 seconds (JVM running for 14.333)

Creating a new SqlSession

SqlSession [org.apache.ibatis.session.defaults.DefaultSqlSession@69aeeb5] was not registered for synchronization because synchronization is not active

JDBC Connection [com.alibaba.druid.proxy.jdbc.ConnectionProxyImpl@7058c450] will not be managed by Spring

==>  Preparing: SELECT id,org_id,domain_id,field_code,field_name,lang_code,field_type,data_type,default_value,note,group_id,expression,display_type,fill_note,example,unit,default_json_schema,is_required,is_delete,gmt_modify_time,gmt_create_time,modifier,creator FROM t_dyf_field WHERE is_delete=0 AND (id = ? AND org_id = ?)

==> Parameters: 17393408226095104(String), 100(String)

<==    Columns: id, org_id, domain_id, field_code, field_name, lang_code, field_type, data_type, default_value, note, group_id, expression, display_type, fill_note, example, unit, default_json_schema, is_required, is_delete, gmt_modify_time, gmt_create_time, modifier, creator

<==        Row: 17393408226095104, 100, 测试数据72, 测试数据71, 字段名, 测试数据52, 74920, 2659, 测试数据3, 测试数据36, 测试数据52, 测试数据27, 测试数据32, 测试数据18, 测试数据44, 测试数据5, {}, 46501, 0, 2021-04-19 11:00:54.086+08, 2021-04-19 11:00:54.086+08, 100, 100

Closing non transactional SqlSession [org.apache.ibatis.session.defaults.DefaultSqlSession@69aeeb5]

2021-04-19 11:44:48.480 ERROR  CloudControllerExceptionHandler.java:79- [b3=,TraceId=8ba0bed669734552,SpanId=8ba0bed669734552,ParentSpanId=,Export=true,Sampled=,Flags=] [Org=,User=] 运行时异常



org.springframework.dao.DataIntegrityViolationException: Error attempting to get column 'lang_code' from result set.  Cause: org.postgresql.util.PSQLException: 不良的类型值 int : 测试数据52

; 不良的类型值 int : 测试数据52; nested exception is org.postgresql.util.PSQLException: 不良的类型值 int : 测试数据52

	at org.springframework.jdbc.support.SQLStateSQLExceptionTranslator.doTranslate(SQLStateSQLExceptionTranslator.java:104) ~[spring-jdbc-5.2.12.RELEASE.jar:5.2.12.RELEASE]

	at org.springframework.jdbc.support.AbstractFallbackSQLExceptionTranslator.translate(AbstractFallbackSQLExceptionTranslator.java:72) ~[spring-jdbc-5.2.12.RELEASE.jar:5.2.12.RELEASE]

	at org.springframework.jdbc.support.AbstractFallbackSQLExceptionTranslator.translate(AbstractFallbackSQLExceptionTranslator.java:81) ~[spring-jdbc-5.2.12.RELEASE.jar:5.2.12.RELEASE]

	at org.springframework.jdbc.support.AbstractFallbackSQLExceptionTranslator.translate(AbstractFallbackSQLExceptionTranslator.java:81) ~[spring-jdbc-5.2.12.RELEASE.jar:5.2.12.RELEASE]

	at org.mybatis.spring.MyBatisExceptionTranslator.translateExceptionIfPossible(MyBatisExceptionTranslator.java:88) ~[mybatis-spring-2.0.5.jar:2.0.5]

	at org.mybatis.spring.SqlSessionTemplate$SqlSessionInterceptor.invoke(SqlSessionTemplate.java:440) ~[mybatis-spring-2.0.5.jar:2.0.5]

	at com.sun.proxy.$Proxy151.selectOne(Unknown Source) ~[?:?]

	at org.mybatis.spring.SqlSessionTemplate.selectOne(SqlSessionTemplate.java:159) ~[mybatis-spring-2.0.5.jar:2.0.5]

	at com.baomidou.mybatisplus.core.override.MybatisMapperMethod.execute(MybatisMapperMethod.java:90) ~[mybatis-plus-core-3.4.2.jar:3.4.2]

	at com.baomidou.mybatisplus.core.override.MybatisMapperProxy$PlainMethodInvoker.invoke(MybatisMapperProxy.java:148) ~[mybatis-plus-core-3.4.2.jar:3.4.2]

	at com.baomidou.mybatisplus.core.override.MybatisMapperProxy.invoke(MybatisMapperProxy.java:89) ~[mybatis-plus-core-3.4.2.jar:3.4.2]

	at com.sun.proxy.$Proxy155.selectOne(Unknown Source) ~[?:?]

	at com.baomidou.mybatisplus.extension.service.impl.ServiceImpl.getOne(ServiceImpl.java:201) ~[mybatis-plus-extension-3.4.2.jar:3.4.2]

	at com.baomidou.mybatisplus.extension.service.IService.getOne(IService.java:229) ~[mybatis-plus-extension-3.4.2.jar:3.4.2]

	at com.sdstc.dyf.admin.core.service.impl.FieldServiceImpl.selectField(FieldServiceImpl.java:83) ~[classes/:?]

	at com.sdstc.dyf.admin.core.service.impl.FieldServiceImpl$$FastClassBySpringCGLIB$$ed8fc6c7.invoke(<generated>) ~[classes/:?]

	at org.springframework.cglib.proxy.MethodProxy.invoke(MethodProxy.java:218) ~[spring-core-5.2.12.RELEASE.jar:5.2.12.RELEASE]

	at org.springframework.aop.framework.CglibAopProxy$DynamicAdvisedInterceptor.intercept(CglibAopProxy.java:687) ~[spring-aop-5.2.12.RELEASE.jar:5.2.12.RELEASE]

	at com.sdstc.dyf.admin.core.service.impl.FieldServiceImpl$$EnhancerBySpringCGLIB$$2162c38d.selectField(<generated>) ~[classes/:?]

	at com.sdstc.dyf.admin.core.controller.DyfController.getField(DyfController.java:53) ~[classes/:?]

	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method) ~[?:1.8.0_281]

	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62) ~[?:1.8.0_281]

	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43) ~[?:1.8.0_281]

	at java.lang.reflect.Method.invoke(Method.java:498) ~[?:1.8.0_281]

	at org.springframework.web.method.support.InvocableHandlerMethod.doInvoke(InvocableHandlerMethod.java:190) ~[spring-web-5.2.12.RELEASE.jar:5.2.12.RELEASE]

	at org.springframework.web.method.support.InvocableHandlerMethod.invokeForRequest(InvocableHandlerMethod.java:138) ~[spring-web-5.2.12.RELEASE.jar:5.2.12.RELEASE]

	at org.springframework.web.servlet.mvc.method.annotation.ServletInvocableHandlerMethod.invokeAndHandle(ServletInvocableHandlerMethod.java:105) ~[spring-webmvc-5.2.12.RELEASE.jar:5.2.12.RELEASE]

	at org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.invokeHandlerMethod(RequestMappingHandlerAdapter.java:878) ~[spring-webmvc-5.2.12.RELEASE.jar:5.2.12.RELEASE]

	at org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.handleInternal(RequestMappingHandlerAdapter.java:792) ~[spring-webmvc-5.2.12.RELEASE.jar:5.2.12.RELEASE]

	at org.springframework.web.servlet.mvc.method.AbstractHandlerMethodAdapter.handle(AbstractHandlerMethodAdapter.java:87) ~[spring-webmvc-5.2.12.RELEASE.jar:5.2.12.RELEASE]

	at org.springframework.web.servlet.DispatcherServlet.doDispatch(DispatcherServlet.java:1040) ~[spring-webmvc-5.2.12.RELEASE.jar:5.2.12.RELEASE]

	at org.springframework.web.servlet.DispatcherServlet.doService(DispatcherServlet.java:943) ~[spring-webmvc-5.2.12.RELEASE.jar:5.2.12.RELEASE]

	at org.springframework.web.servlet.FrameworkServlet.processRequest(FrameworkServlet.java:1006) ~[spring-webmvc-5.2.12.RELEASE.jar:5.2.12.RELEASE]

	at org.springframework.web.servlet.FrameworkServlet.doGet(FrameworkServlet.java:898) ~[spring-webmvc-5.2.12.RELEASE.jar:5.2.12.RELEASE]

	at javax.servlet.http.HttpServlet.service(HttpServlet.java:626) ~[tomcat-embed-core-9.0.41.jar:4.0.FR]

	at org.springframework.web.servlet.FrameworkServlet.service(FrameworkServlet.java:883) ~[spring-webmvc-5.2.12.RELEASE.jar:5.2.12.RELEASE]

	at javax.servlet.http.HttpServlet.service(HttpServlet.java:733) ~[tomcat-embed-core-9.0.41.jar:4.0.FR]

	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:231) ~[tomcat-embed-core-9.0.41.jar:9.0.41]

	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) ~[tomcat-embed-core-9.0.41.jar:9.0.41]

	at org.apache.tomcat.websocket.server.WsFilter.doFilter(WsFilter.java:53) ~[tomcat-embed-websocket-9.0.41.jar:9.0.41]

	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193) ~[tomcat-embed-core-9.0.41.jar:9.0.41]

	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) ~[tomcat-embed-core-9.0.41.jar:9.0.41]

	at brave.servlet.TracingFilter.doFilter(TracingFilter.java:68) ~[brave-instrumentation-servlet-5.12.7.jar:?]

	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193) ~[tomcat-embed-core-9.0.41.jar:9.0.41]

	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) ~[tomcat-embed-core-9.0.41.jar:9.0.41]

	at org.springframework.web.filter.FormContentFilter.doFilterInternal(FormContentFilter.java:93) ~[spring-web-5.2.12.RELEASE.jar:5.2.12.RELEASE]

	at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:119) ~[spring-web-5.2.12.RELEASE.jar:5.2.12.RELEASE]

	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193) ~[tomcat-embed-core-9.0.41.jar:9.0.41]

	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) ~[tomcat-embed-core-9.0.41.jar:9.0.41]

	at com.sdstc.scdp.log.filter.GlobalLogFilter.doFilterInternal(GlobalLogFilter.java:60) ~[scdp-log-1.0.2-20210417.042821-8.jar:?]

	at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:119) ~[spring-web-5.2.12.RELEASE.jar:5.2.12.RELEASE]

	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193) ~[tomcat-embed-core-9.0.41.jar:9.0.41]

	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) ~[tomcat-embed-core-9.0.41.jar:9.0.41]

	at brave.servlet.TracingFilter.doFilter(TracingFilter.java:87) ~[brave-instrumentation-servlet-5.12.7.jar:?]

	at org.springframework.cloud.sleuth.instrument.web.LazyTracingFilter.doFilter(TraceWebServletAutoConfiguration.java:139) ~[spring-cloud-sleuth-core-2.2.6.RELEASE.jar:2.2.6.RELEASE]

	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193) ~[tomcat-embed-core-9.0.41.jar:9.0.41]

	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) ~[tomcat-embed-core-9.0.41.jar:9.0.41]

	at org.springframework.boot.actuate.metrics.web.servlet.WebMvcMetricsFilter.doFilterInternal(WebMvcMetricsFilter.java:93) ~[spring-boot-actuator-2.3.8.RELEASE.jar:2.3.8.RELEASE]

	at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:119) ~[spring-web-5.2.12.RELEASE.jar:5.2.12.RELEASE]

	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193) ~[tomcat-embed-core-9.0.41.jar:9.0.41]

	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) ~[tomcat-embed-core-9.0.41.jar:9.0.41]

	at org.springframework.web.filter.CharacterEncodingFilter.doFilterInternal(CharacterEncodingFilter.java:201) ~[spring-web-5.2.12.RELEASE.jar:5.2.12.RELEASE]

	at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:119) ~[spring-web-5.2.12.RELEASE.jar:5.2.12.RELEASE]

	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193) ~[tomcat-embed-core-9.0.41.jar:9.0.41]

	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) ~[tomcat-embed-core-9.0.41.jar:9.0.41]

	at org.apache.catalina.core.StandardWrapperValve.invoke(StandardWrapperValve.java:202) ~[tomcat-embed-core-9.0.41.jar:9.0.41]

	at org.apache.catalina.core.StandardContextValve.invoke(StandardContextValve.java:97) ~[tomcat-embed-core-9.0.41.jar:9.0.41]

	at org.apache.catalina.authenticator.AuthenticatorBase.invoke(AuthenticatorBase.java:542) ~[tomcat-embed-core-9.0.41.jar:9.0.41]

	at org.apache.catalina.core.StandardHostValve.invoke(StandardHostValve.java:143) ~[tomcat-embed-core-9.0.41.jar:9.0.41]

	at org.apache.catalina.valves.ErrorReportValve.invoke(ErrorReportValve.java:92) ~[tomcat-embed-core-9.0.41.jar:9.0.41]

	at org.apache.catalina.core.StandardEngineValve.invoke(StandardEngineValve.java:78) ~[tomcat-embed-core-9.0.41.jar:9.0.41]

	at org.apache.catalina.connector.CoyoteAdapter.service(CoyoteAdapter.java:343) ~[tomcat-embed-core-9.0.41.jar:9.0.41]

	at org.apache.coyote.http11.Http11Processor.service(Http11Processor.java:374) ~[tomcat-embed-core-9.0.41.jar:9.0.41]

	at org.apache.coyote.AbstractProcessorLight.process(AbstractProcessorLight.java:65) ~[tomcat-embed-core-9.0.41.jar:9.0.41]

	at org.apache.coyote.AbstractProtocol$ConnectionHandler.process(AbstractProtocol.java:888) ~[tomcat-embed-core-9.0.41.jar:9.0.41]

	at org.apache.tomcat.util.net.NioEndpoint$SocketProcessor.doRun(NioEndpoint.java:1597) ~[tomcat-embed-core-9.0.41.jar:9.0.41]

	at org.apache.tomcat.util.net.SocketProcessorBase.run(SocketProcessorBase.java:49) ~[tomcat-embed-core-9.0.41.jar:9.0.41]

	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149) ~[?:1.8.0_281]

	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624) ~[?:1.8.0_281]

	at org.apache.tomcat.util.threads.TaskThread$WrappingRunnable.run(TaskThread.java:61) ~[tomcat-embed-core-9.0.41.jar:9.0.41]

	at java.lang.Thread.run(Thread.java:748) [?:1.8.0_281]

Caused by: org.postgresql.util.PSQLException: 不良的类型值 int : 测试数据52

	at org.postgresql.jdbc.PgResultSet.toInt(PgResultSet.java:3014) ~[postgresql-42.2.18.jar:42.2.18]

	at org.postgresql.jdbc.PgResultSet.getInt(PgResultSet.java:2188) ~[postgresql-42.2.18.jar:42.2.18]

	at org.postgresql.jdbc.PgResultSet.getInt(PgResultSet.java:2626) ~[postgresql-42.2.18.jar:42.2.18]

	at com.alibaba.druid.filter.FilterChainImpl.resultSet_getInt(FilterChainImpl.java:1173) ~[druid-1.2.4.jar:1.2.4]

	at com.alibaba.druid.filter.FilterAdapter.resultSet_getInt(FilterAdapter.java:1645) ~[druid-1.2.4.jar:1.2.4]

	at com.alibaba.druid.filter.FilterChainImpl.resultSet_getInt(FilterChainImpl.java:1169) ~[druid-1.2.4.jar:1.2.4]

	at com.alibaba.druid.filter.FilterAdapter.resultSet_getInt(FilterAdapter.java:1645) ~[druid-1.2.4.jar:1.2.4]

	at com.alibaba.druid.filter.FilterChainImpl.resultSet_getInt(FilterChainImpl.java:1169) ~[druid-1.2.4.jar:1.2.4]

	at com.alibaba.druid.proxy.jdbc.ResultSetProxyImpl.getInt(ResultSetProxyImpl.java:492) ~[druid-1.2.4.jar:1.2.4]

	at com.alibaba.druid.pool.DruidPooledResultSet.getInt(DruidPooledResultSet.java:292) ~[druid-1.2.4.jar:1.2.4]

	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method) ~[?:1.8.0_281]

	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62) ~[?:1.8.0_281]

	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43) ~[?:1.8.0_281]

	at java.lang.reflect.Method.invoke(Method.java:498) ~[?:1.8.0_281]

	at org.apache.ibatis.logging.jdbc.ResultSetLogger.invoke(ResultSetLogger.java:69) ~[mybatis-3.5.6.jar:3.5.6]

	at com.sun.proxy.$Proxy184.getInt(Unknown Source) ~[?:?]

	at org.apache.ibatis.type.IntegerTypeHandler.getNullableResult(IntegerTypeHandler.java:37) ~[mybatis-3.5.6.jar:3.5.6]

	at org.apache.ibatis.type.IntegerTypeHandler.getNullableResult(IntegerTypeHandler.java:26) ~[mybatis-3.5.6.jar:3.5.6]

	at org.apache.ibatis.type.BaseTypeHandler.getResult(BaseTypeHandler.java:85) ~[mybatis-3.5.6.jar:3.5.6]

	at org.apache.ibatis.executor.resultset.DefaultResultSetHandler.createUsingConstructor(DefaultResultSetHandler.java:710) ~[mybatis-3.5.6.jar:3.5.6]

	at org.apache.ibatis.executor.resultset.DefaultResultSetHandler.createByConstructorSignature(DefaultResultSetHandler.java:693) ~[mybatis-3.5.6.jar:3.5.6]

	at org.apache.ibatis.executor.resultset.DefaultResultSetHandler.createResultObject(DefaultResultSetHandler.java:657) ~[mybatis-3.5.6.jar:3.5.6]

	at org.apache.ibatis.executor.resultset.DefaultResultSetHandler.createResultObject(DefaultResultSetHandler.java:630) ~[mybatis-3.5.6.jar:3.5.6]

	at org.apache.ibatis.executor.resultset.DefaultResultSetHandler.getRowValue(DefaultResultSetHandler.java:397) ~[mybatis-3.5.6.jar:3.5.6]

	at org.apache.ibatis.executor.resultset.DefaultResultSetHandler.handleRowValuesForSimpleResultMap(DefaultResultSetHandler.java:354) ~[mybatis-3.5.6.jar:3.5.6]

	at org.apache.ibatis.executor.resultset.DefaultResultSetHandler.handleRowValues(DefaultResultSetHandler.java:328) ~[mybatis-3.5.6.jar:3.5.6]

	at org.apache.ibatis.executor.resultset.DefaultResultSetHandler.handleResultSet(DefaultResultSetHandler.java:301) ~[mybatis-3.5.6.jar:3.5.6]

	at org.apache.ibatis.executor.resultset.DefaultResultSetHandler.handleResultSets(DefaultResultSetHandler.java:194) ~[mybatis-3.5.6.jar:3.5.6]

	at org.apache.ibatis.executor.statement.PreparedStatementHandler.query(PreparedStatementHandler.java:65) ~[mybatis-3.5.6.jar:3.5.6]

	at org.apache.ibatis.executor.statement.RoutingStatementHandler.query(RoutingStatementHandler.java:79) ~[mybatis-3.5.6.jar:3.5.6]

	at com.baomidou.mybatisplus.core.executor.MybatisSimpleExecutor.doQuery(MybatisSimpleExecutor.java:69) ~[mybatis-plus-core-3.4.2.jar:3.4.2]

	at org.apache.ibatis.executor.BaseExecutor.queryFromDatabase(BaseExecutor.java:325) ~[mybatis-3.5.6.jar:3.5.6]

	at org.apache.ibatis.executor.BaseExecutor.query(BaseExecutor.java:156) ~[mybatis-3.5.6.jar:3.5.6]

	at com.baomidou.mybatisplus.core.executor.MybatisCachingExecutor.query(MybatisCachingExecutor.java:165) ~[mybatis-plus-core-3.4.2.jar:3.4.2]

	at com.baomidou.mybatisplus.core.executor.MybatisCachingExecutor.query(MybatisCachingExecutor.java:92) ~[mybatis-plus-core-3.4.2.jar:3.4.2]

	at org.apache.ibatis.session.defaults.DefaultSqlSession.selectList(DefaultSqlSession.java:147) ~[mybatis-3.5.6.jar:3.5.6]

	at org.apache.ibatis.session.defaults.DefaultSqlSession.selectList(DefaultSqlSession.java:140) ~[mybatis-3.5.6.jar:3.5.6]

	at org.apache.ibatis.session.defaults.DefaultSqlSession.selectOne(DefaultSqlSession.java:76) ~[mybatis-3.5.6.jar:3.5.6]

	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method) ~[?:1.8.0_281]

	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62) ~[?:1.8.0_281]

	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43) ~[?:1.8.0_281]

	at java.lang.reflect.Method.invoke(Method.java:498) ~[?:1.8.0_281]

	at org.mybatis.spring.SqlSessionTemplate$SqlSessionInterceptor.invoke(SqlSessionTemplate.java:426) ~[mybatis-spring-2.0.5.jar:2.0.5]

	... 75 more

~~~

定位问题时发现如下现象（按照发现顺序）：

1. 注释FieldVo中所有的字段，selectOne可以正常执行
2. 注释FieldVo中的Integer字段，selectOne可以正常执行
3. 注释FieldVo不继承BaseVO代码，selectOne可以正常执行

现象到这块就已经有点离奇了，后来一个同事发现，注释掉@Builder，其他部分保持不变，该代码也可以正常执行。开始意识到可能是Lombok与PG库驱动存在或者MyBatis-Plus存在某些冲突，果断按照这些关键字查找，寻找到如下资料：

[MybatisPlus 3.X 与lombok @Builder 冲突 解决方案](https://blog.csdn.net/zhuzhoulin/article/details/106710750)

我按照该文档修复了该问题，但是这个现象我是无法理解的，查看添加@Builder和不添加@Builder后生成的代码，发现区别仅仅是，当加了@Builder、@NoArgsConstructor、@AllArgsConstructor后会生成公共的无参数构造函数、全参的构造函数、和Builder类；如果只加了@Builder，会生成一个默认访问权限的全参构造函数、和Builder类，信息截止到目前，还是没有办法解释这个现象的。

我只能暂时记录下这个现象，未来有机会去解决。