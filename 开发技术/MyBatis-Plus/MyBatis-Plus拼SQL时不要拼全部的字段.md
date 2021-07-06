核心就在于使用select方法，参考代码如下：

~~~ java

LambdaQueryWrapper<AuthFun> queryWrapper = new LambdaQueryWrapper<AuthFun>()
        .eq(AuthFun::getTreeCode, TreeCode.SERVICE_PACKAGE.getValue())
        .in(AuthFun::getParentId, authFuncParentIds)
        .select(AuthFun::getId, AuthFun::getOperaList);

~~~

使用这个方法精准的限制被搜索到的字段是非常的有意义的，可以避免浪费太多的网络资源、减少对数据库的压力。

## 参考资料

1. [使用Mybatis-plus，查询表中某个字段的值，返回List集合_cc920095705的博客-程序员宅基地_mybatisplus查询指定字段](http://www.cxyzjd.com/article/cc920095705/109076444)