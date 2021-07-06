典型的自己挖坑自己填。

问题是这样的，我的Postman登录我的账号后，进行请求时一直报代理错误，浏览器可以正常访问的URL，在Postman中请求都是代理错误，但是实际上我系统压根就没有开代理。

我此时已经开始怀疑我测试Python的Request类的代理时，添加的ALL_PROXY、HTTP_PROXY、HTTPS_PROXY环境变量影响到我的Postman了，果断去掉这几个环境变量，然后重启Postman，发现Postman恢复正常了。

我觉得这种问题很怪异，于是去看了看Postman的配置，结果发现我本机的Postman有如下配置：

![2021-04-22-20-13-56](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-04-22-20-13-56.png)

这个是真的巧合的不能再巧合了。

## 参考教程

1. [Postman设置网络代理](https://blog.csdn.net/qq_28284093/article/details/80162842)