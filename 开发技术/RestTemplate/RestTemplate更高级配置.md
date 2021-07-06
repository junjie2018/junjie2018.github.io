今天在一个没有配置RestTemplate的项目中使用RestTemplate，所以我需要自己配置一下，按照我之前的方式配置好了后，发现报了如下错误：

~~~

org.springframework.web.client.RestClientException: No HttpMessageConverter for com.sdstc.show.controller.external.DynamicFormController$PostDataRequest

	at org.springframework.web.client.RestTemplate$HttpEntityRequestCallback.doWithRequest(RestTemplate.java:961) ~[spring-web-5.2.4.RELEASE.jar:5.2.4.RELEASE]

	at org.springframework.web.client.RestTemplate.doExecute(RestTemplate.java:737) ~[spring-web-5.2.4.RELEASE.jar:5.2.4.RELEASE]

	at org.springframework.web.client.RestTemplate.execute(RestTemplate.java:674) ~[spring-web-5.2.4.RELEASE.jar:5.2.4.RELEASE]

	at org.springframework.web.client.RestTemplate.postForObject(RestTemplate.java:418) ~[spring-web-5.2.4.RELEASE.jar:5.2.4.RELEASE]

	at com.sdstc.show.controller.external.DynamicFormController.postData(DynamicFormController.java:49) ~[classes/:?]

	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method) ~[?:1.8.0_281]

	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62) ~[?:1.8.0_281]

	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43) ~[?:1.8.0_281]

	at java.lang.reflect.Method.invoke(Method.java:498) ~[?:1.8.0_281]

	at org.springframework.web.method.support.InvocableHandlerMethod.doInvoke(InvocableHandlerMethod.java:190) ~[spring-web-5.2.4.RELEASE.jar:5.2.4.RELEASE]

	at org.springframework.web.method.support.InvocableHandlerMethod.invokeForRequest(InvocableHandlerMethod.java:138) ~[spring-web-5.2.4.RELEASE.jar:5.2.4.RELEASE]

	at org.springframework.web.servlet.mvc.method.annotation.ServletInvocableHandlerMethod.invokeAndHandle(ServletInvocableHandlerMethod.java:106) ~[spring-webmvc-5.2.4.RELEASE.jar:5.2.4.RELEASE]

	at org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.invokeHandlerMethod(RequestMappingHandlerAdapter.java:879) ~[spring-webmvc-5.2.4.RELEASE.jar:5.2.4.RELEASE]

	at org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.handleInternal(RequestMappingHandlerAdapter.java:793) ~[spring-webmvc-5.2.4.RELEASE.jar:5.2.4.RELEASE]

	at org.springframework.web.servlet.mvc.method.AbstractHandlerMethodAdapter.handle(AbstractHandlerMethodAdapter.java:87) ~[spring-webmvc-5.2.4.RELEASE.jar:5.2.4.RELEASE]

	at org.springframework.web.servlet.DispatcherServlet.doDispatch(DispatcherServlet.java:1040) ~[spring-webmvc-5.2.4.RELEASE.jar:5.2.4.RELEASE]

	at org.springframework.web.servlet.DispatcherServlet.doService(DispatcherServlet.java:943) ~[spring-webmvc-5.2.4.RELEASE.jar:5.2.4.RELEASE]

	at org.springframework.web.servlet.FrameworkServlet.processRequest(FrameworkServlet.java:1006) ~[spring-webmvc-5.2.4.RELEASE.jar:5.2.4.RELEASE]

	at org.springframework.web.servlet.FrameworkServlet.doPost(FrameworkServlet.java:909) ~[spring-webmvc-5.2.4.RELEASE.jar:5.2.4.RELEASE]

	at javax.servlet.http.HttpServlet.service(HttpServlet.java:523) ~[jakarta.servlet-api-4.0.3.jar:4.0.3]

	at org.springframework.web.servlet.FrameworkServlet.service(FrameworkServlet.java:883) ~[spring-webmvc-5.2.4.RELEASE.jar:5.2.4.RELEASE]

	at javax.servlet.http.HttpServlet.service(HttpServlet.java:590) ~[jakarta.servlet-api-4.0.3.jar:4.0.3]

	at io.undertow.servlet.handlers.ServletHandler.handleRequest(ServletHandler.java:74) ~[undertow-servlet-2.0.29.Final.jar:2.0.29.Final]

	at io.undertow.servlet.handlers.FilterHandler$FilterChainImpl.doFilter(FilterHandler.java:129) ~[undertow-servlet-2.0.29.Final.jar:2.0.29.Final]

	at com.sdstc.core.configuration.web.RequestBodyFilter.doFilter(RequestBodyFilter.java:39) ~[sdstc-core-1.1.4.jar:?]

	at io.undertow.servlet.core.ManagedFilter.doFilter(ManagedFilter.java:61) ~[undertow-servlet-2.0.29.Final.jar:2.0.29.Final]

	at io.undertow.servlet.handlers.FilterHandler$FilterChainImpl.doFilter(FilterHandler.java:131) ~[undertow-servlet-2.0.29.Final.jar:2.0.29.Final]

	at org.springframework.web.filter.RequestContextFilter.doFilterInternal(RequestContextFilter.java:100) ~[spring-web-5.2.4.RELEASE.jar:5.2.4.RELEASE]

	at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:119) ~[spring-web-5.2.4.RELEASE.jar:5.2.4.RELEASE]

	at io.undertow.servlet.core.ManagedFilter.doFilter(ManagedFilter.java:61) ~[undertow-servlet-2.0.29.Final.jar:2.0.29.Final]

	at io.undertow.servlet.handlers.FilterHandler$FilterChainImpl.doFilter(FilterHandler.java:131) ~[undertow-servlet-2.0.29.Final.jar:2.0.29.Final]

	at org.springframework.web.filter.FormContentFilter.doFilterInternal(FormContentFilter.java:93) ~[spring-web-5.2.4.RELEASE.jar:5.2.4.RELEASE]

	at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:119) ~[spring-web-5.2.4.RELEASE.jar:5.2.4.RELEASE]

	at io.undertow.servlet.core.ManagedFilter.doFilter(ManagedFilter.java:61) ~[undertow-servlet-2.0.29.Final.jar:2.0.29.Final]

	at io.undertow.servlet.handlers.FilterHandler$FilterChainImpl.doFilter(FilterHandler.java:131) ~[undertow-servlet-2.0.29.Final.jar:2.0.29.Final]

	at org.springframework.web.filter.CharacterEncodingFilter.doFilterInternal(CharacterEncodingFilter.java:201) ~[spring-web-5.2.4.RELEASE.jar:5.2.4.RELEASE]

	at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:119) ~[spring-web-5.2.4.RELEASE.jar:5.2.4.RELEASE]

	at io.undertow.servlet.core.ManagedFilter.doFilter(ManagedFilter.java:61) ~[undertow-servlet-2.0.29.Final.jar:2.0.29.Final]

	at io.undertow.servlet.handlers.FilterHandler$FilterChainImpl.doFilter(FilterHandler.java:131) ~[undertow-servlet-2.0.29.Final.jar:2.0.29.Final]

	at io.undertow.servlet.handlers.FilterHandler.handleRequest(FilterHandler.java:84) ~[undertow-servlet-2.0.29.Final.jar:2.0.29.Final]

	at io.undertow.servlet.handlers.security.ServletSecurityRoleHandler.handleRequest(ServletSecurityRoleHandler.java:62) ~[undertow-servlet-2.0.29.Final.jar:2.0.29.Final]

	at io.undertow.servlet.handlers.ServletChain$1.handleRequest(ServletChain.java:68) ~[undertow-servlet-2.0.29.Final.jar:2.0.29.Final]

	at io.undertow.servlet.handlers.ServletDispatchingHandler.handleRequest(ServletDispatchingHandler.java:36) ~[undertow-servlet-2.0.29.Final.jar:2.0.29.Final]

	at io.undertow.servlet.handlers.RedirectDirHandler.handleRequest(RedirectDirHandler.java:68) ~[undertow-servlet-2.0.29.Final.jar:2.0.29.Final]

	at io.undertow.servlet.handlers.security.SSLInformationAssociationHandler.handleRequest(SSLInformationAssociationHandler.java:132) ~[undertow-servlet-2.0.29.Final.jar:2.0.29.Final]

	at io.undertow.servlet.handlers.security.ServletAuthenticationCallHandler.handleRequest(ServletAuthenticationCallHandler.java:57) ~[undertow-servlet-2.0.29.Final.jar:2.0.29.Final]

	at io.undertow.server.handlers.PredicateHandler.handleRequest(PredicateHandler.java:43) ~[undertow-core-2.0.29.Final.jar:2.0.29.Final]

	at io.undertow.security.handlers.AbstractConfidentialityHandler.handleRequest(AbstractConfidentialityHandler.java:46) ~[undertow-core-2.0.29.Final.jar:2.0.29.Final]

	at io.undertow.servlet.handlers.security.ServletConfidentialityConstraintHandler.handleRequest(ServletConfidentialityConstraintHandler.java:64) ~[undertow-servlet-2.0.29.Final.jar:2.0.29.Final]

	at io.undertow.security.handlers.AuthenticationMechanismsHandler.handleRequest(AuthenticationMechanismsHandler.java:60) ~[undertow-core-2.0.29.Final.jar:2.0.29.Final]

	at io.undertow.servlet.handlers.security.CachedAuthenticatedSessionHandler.handleRequest(CachedAuthenticatedSessionHandler.java:77) ~[undertow-servlet-2.0.29.Final.jar:2.0.29.Final]

	at io.undertow.security.handlers.AbstractSecurityContextAssociationHandler.handleRequest(AbstractSecurityContextAssociationHandler.java:43) ~[undertow-core-2.0.29.Final.jar:2.0.29.Final]

	at io.undertow.server.handlers.PredicateHandler.handleRequest(PredicateHandler.java:43) ~[undertow-core-2.0.29.Final.jar:2.0.29.Final]

	at io.undertow.server.handlers.PredicateHandler.handleRequest(PredicateHandler.java:43) ~[undertow-core-2.0.29.Final.jar:2.0.29.Final]

	at io.undertow.servlet.handlers.ServletInitialHandler.handleFirstRequest(ServletInitialHandler.java:269) ~[undertow-servlet-2.0.29.Final.jar:2.0.29.Final]

	at io.undertow.servlet.handlers.ServletInitialHandler.access$100(ServletInitialHandler.java:78) ~[undertow-servlet-2.0.29.Final.jar:2.0.29.Final]

	at io.undertow.servlet.handlers.ServletInitialHandler$2.call(ServletInitialHandler.java:133) ~[undertow-servlet-2.0.29.Final.jar:2.0.29.Final]

	at io.undertow.servlet.handlers.ServletInitialHandler$2.call(ServletInitialHandler.java:130) ~[undertow-servlet-2.0.29.Final.jar:2.0.29.Final]

	at io.undertow.servlet.core.ServletRequestContextThreadSetupAction$1.call(ServletRequestContextThreadSetupAction.java:48) ~[undertow-servlet-2.0.29.Final.jar:2.0.29.Final]

	at io.undertow.servlet.core.ContextClassLoaderSetupAction$1.call(ContextClassLoaderSetupAction.java:43) ~[undertow-servlet-2.0.29.Final.jar:2.0.29.Final]

	at io.undertow.servlet.handlers.ServletInitialHandler.dispatchRequest(ServletInitialHandler.java:249) ~[undertow-servlet-2.0.29.Final.jar:2.0.29.Final]

	at io.undertow.servlet.handlers.ServletInitialHandler.access$000(ServletInitialHandler.java:78) ~[undertow-servlet-2.0.29.Final.jar:2.0.29.Final]

	at io.undertow.servlet.handlers.ServletInitialHandler$1.handleRequest(ServletInitialHandler.java:99) ~[undertow-servlet-2.0.29.Final.jar:2.0.29.Final]

	at io.undertow.server.Connectors.executeRootHandler(Connectors.java:376) ~[undertow-core-2.0.29.Final.jar:2.0.29.Final]

	at io.undertow.server.HttpServerExchange$1.run(HttpServerExchange.java:830) ~[undertow-core-2.0.29.Final.jar:2.0.29.Final]

	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149) ~[?:1.8.0_281]

	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624) ~[?:1.8.0_281]

	at java.lang.Thread.run(Thread.java:748) [?:1.8.0_281]

~~~

HttpMessageConverter这东西我还是有点熟悉的，我立刻对RestTemplate加了如下配置：

~~~

        FastJsonHttpMessageConverter fastConverter = new FastJsonHttpMessageConverter();

        restTemplate.getMessageConverters().add(fastConverter);

~~~

结果又报错说无法找不到MediaType，额，还好我最近看过这部分知识，于是我进行了如下配置：

~~~

        FastJsonHttpMessageConverter fastConverter = new FastJsonHttpMessageConverter();

        List<MediaType> supportedMediaTypes = new ArrayList<>();
        supportedMediaTypes.add(MediaType.APPLICATION_JSON);
        supportedMediaTypes.add(MediaType.APPLICATION_ATOM_XML);
        supportedMediaTypes.add(MediaType.APPLICATION_FORM_URLENCODED);
        supportedMediaTypes.add(MediaType.APPLICATION_OCTET_STREAM);
        supportedMediaTypes.add(MediaType.APPLICATION_PDF);
        supportedMediaTypes.add(MediaType.APPLICATION_RSS_XML);
        supportedMediaTypes.add(MediaType.APPLICATION_XHTML_XML);
        supportedMediaTypes.add(MediaType.APPLICATION_XML);
        supportedMediaTypes.add(MediaType.IMAGE_GIF);
        supportedMediaTypes.add(MediaType.IMAGE_JPEG);
        supportedMediaTypes.add(MediaType.IMAGE_PNG);
        supportedMediaTypes.add(MediaType.TEXT_EVENT_STREAM);
        supportedMediaTypes.add(MediaType.TEXT_HTML);
        supportedMediaTypes.add(MediaType.TEXT_MARKDOWN);
        supportedMediaTypes.add(MediaType.TEXT_PLAIN);
        supportedMediaTypes.add(MediaType.TEXT_XML);
        fastConverter.setSupportedMediaTypes(supportedMediaTypes);

        restTemplate.getMessageConverters().add(fastConverter);

~~~

我们的框架高度定制化，有很多默认存在的东西，都因为定制的原因已经不叫原来的名字了，所以我们无法使用到这些bean，最后就导致出现了问题。