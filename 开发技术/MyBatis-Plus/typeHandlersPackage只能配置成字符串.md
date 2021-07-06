我是有需求将type-handler-package配置成字符串数组，但是官方给的文档中，该处只能配置成字符串，我觉得查看源码肯定可以找到配置成字符串数组的方案，只是这样做成本太高了，并不值得这么做。

20210621后续：

我在网上有看到type-handler-package配置成多个值，值之间用分号隔开，我没有验证过这个方案。

## 参考资料

1. [typehandlerspackage官方文档](https://baomidou.com/config/#typehandlerspackage)

2. [spring boot 与mybatis整合，type-aliases-package、type-handlers-package等配置不起作用,导致类加载失败](https://blog.csdn.net/doctor_who2004/article/details/70163144)
   
   这篇文章似乎在讲源码怎么看，我简单看了下，目前不想花精力去研究这个。