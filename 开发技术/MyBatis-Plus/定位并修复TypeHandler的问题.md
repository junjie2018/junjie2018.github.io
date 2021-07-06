问题的表现：

1. 我们数据库中使用了timestamptz类型
2. 我们代码中使用了LocalDateTime类型
3. 我们的代码使用了LocalDateTimeTypeHandler注解

数据能够正常的入库，但是当从数据库中读取数据的时候，会报timestamp转换异常。我断点发现，报异常的时候，代码并没有进入LocalDateTimeTypeHandler，而是走了PGResultSet，我断定是LocalDateTimeTypeHandler无法进入导致的错误。我以该现象为关键字，寻找解决方案，最后有如下解决方案：

~~~

mybatis-plus:
  type-handlers-package: com.sdstc.core.mybatisplus.type

~~~

## 参考资料

1. [mybatis plus坑之 - @TableField(typeHandler) 查询时不生效为null](https://blog.csdn.net/sgambler/article/details/106921634)

2. [更新时自定义的TypeHandler不生效](https://github.com/baomidou/mybatis-plus/issues/794)
   
   我没有看这个教程，但是这篇教程似乎讨论的更深入
   