这个报错非常的迷惑人，让人感觉是自己的sql查出来了多条记录，但是代码中将多条数据塞到了一个对象中，实际上并不是这样的，MyBatis拼出来的SQL最终检查出来的也是一条记录。

这个问题最终是因为Entity上加了@Builder后，缺少默认构造函数导致的，我已经第二次踩这个坑了。

## 参考资料

1. [mybatis-plus java.lang.IndexOutOfBoundsException: Index: 23, Size: 23](https://blog.csdn.net/qq_41829492/article/details/89392669)