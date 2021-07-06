我从源码导入了一个Maven项目，因为我默认的Maven配置中使用的是setting.xml文件，但是我没有这份配置文件（我的文件为setting-default.xml），但是我忽视了这个问题。

当我配置了Maven Profile，然后启动项目的一个测试用例时，项目正常启动了，但是并没有获取到Maven Profile的值。这个现象其实很奇怪：

1. Maven都没有进行导入，为什么测试方法能正常启动
2. 为什么正常启动后，不能获取到Maven Profile的值呢