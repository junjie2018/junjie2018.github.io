我昨天一直在思考Maven Profile与SpringBoot配置文件的关系，想知道Maven Profile中的配置是如何传递给SpringBoot配置文件的，是通过环境变量么？

我最终获取到了一个项目启动时的Idea运行指令（相关笔记在Idea分类下寻找），我发现这条指令平平无奇，并没有传递任何参数到应用程序。

我又观察target下classes目录中的配置文件，发现该目录下的application.properties中原有的配置已经被替换为如下内容：

![2021-07-02-16-28-50](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-07-02-16-28-50.png)

所以我作出了如下分析，Idea启动前会自动的调用Maven的打包功能，从而生产target目录下的classes等文件，这个过程中我们在Maven项目中配置的Maven过滤器会发挥作用，将文件中引用的特定符号转换成指定的值。

其实我的代码中并没有配置Maven过滤器插件，为什么application.properties中的`@spring.profiles.active@`会被Maven过滤器插件转换为Profile的值呢，我个人认为是因为我引入了SpringBoot的打包插件，该插件默认配置了Maven打包插件（我没有证据）。

我看过网上关于如何使用Maven过滤器插件的文章，它们都是针对application*.yml进行处理，我猜想SpringBoot的打包插件也是这样的（实际上我做了实验，非application*.yml文件的确不会被处理）。


为了验证我的，我设计了如下实验：先准备一份application-tmp.properties，包含`@spring.profiles.active@`，然后运行package指令，在target目录下观察application-tmp.properties文件，的确发现进行了字符串的替换。

![2021-07-02-18-48-18](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-07-02-18-48-18.png)

综上，我可以解释另一篇笔记中提到的疑问：我如何在application.yml中使用该机制，我只需要在编辑了applicaiton.yml文件后，运行一下package指令就好了（也可以不运行，貌似启动指令会根据Maven Profile帮我们生成新的application.yml文件），如下：

![2021-07-02-20-32-52](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-07-02-20-32-52.png)

## 参考资料

1. [在Spring Boot YML配置文件中使用MAVEN变量@var@](https://blog.csdn.net/u012280292/article/details/80970225)
2. [Maven Profile 与 SpringBoot Profile 多环境打包指派指定环境](https://blog.csdn.net/jianxia801/article/details/99688644)