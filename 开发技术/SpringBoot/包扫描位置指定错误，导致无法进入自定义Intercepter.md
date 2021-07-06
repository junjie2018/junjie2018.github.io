公司开新项目，我参考以往的代码起了一个项目，项目启动很成功，但是请求时报如下错误：

![2021-06-26-16-12-06](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-06-26-16-12-06.png)

我们开发了相应的拦截器，负责解析请求中传递的token，将token中的信息塞到属性中（当且仅当请求不通过网关直接请求服务时）。但是从如上报错信息中可以看出我们的拦截器并没有起到作用。在我们的拦截器UserInfoInterceptor中打上断点，结果发现请求确实没有进入我们的拦截器。

观察以往项目，发现我们的新项目结构和以往的包结构是有出入的，以往的项目包结构为com.abc.project，而新项目为com.abc.project.server，新项目想较以往的项目多了一层包结构。然后一些奇奇怪怪的原因，就导致我们的包无法被扫描到，从而无法加载UserInfoInterceptor到SpringBean中。

后来我将我们的启动类改为如下配置，该问题恢复了。

![2021-06-26-16-23-56](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-06-26-16-23-56.png)

我对这个问题做了一些分析，`com.sdstc.pdm.server`包是肯定会被自动扫描到的，`com.sdstc.core`包是无法被自动扫描到，实际上`com.sdstc.pdm.server`也确实被自动扫描到了，而`com.sdstc.core`没有。想要实现`com.sdstc.core`包被自动扫描到，我们可以通过开发starter实现这个效果。

