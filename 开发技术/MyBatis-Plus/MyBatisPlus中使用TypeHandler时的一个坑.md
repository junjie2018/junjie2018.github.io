这个问题是我同事遇到的，这个问题大概是说，当一个字段注解了@TypeHandler后，在使用LambdaQueryWrapper时，你传递的参数不会给TypeHandler中指定的类转换，从而导致数据库报出异常。

我比较关注这个问题，是因为我最近也在大量的使用TypeHandler，我怕我自己也掉进去了。

## 参考资料

1. [在字段上设置typeHandler，使用LambdaQueryWrapper查询时没有生效](https://github.com/baomidou/mybatis-plus/issues/3346)

