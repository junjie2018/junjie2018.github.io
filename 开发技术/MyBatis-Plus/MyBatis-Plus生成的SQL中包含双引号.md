![2021-06-16-09-36-28](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-06-16-09-36-28.png)

最近的工作又是在跟TypeHandler较劲，同样的错误我已经在两个场景中发现了，这次的场景是我修改了typeHandlerPackage配置，我想测一下我自己的typeHandler是否能实现我想要的效果，结构就出现了这个问题。

事后我分析，可能两次问题都是同一个原因：我在我自己的TypeHandler中的Mapped中配置了Object.class。