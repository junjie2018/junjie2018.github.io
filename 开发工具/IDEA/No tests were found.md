这个错误发生的非常让人哭笑不得，现象就是运行测试时如论如何都无法正确的运行，总是显示No tests were found。

![2021-06-16-09-08-57](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-06-16-09-08-57.png)

最后检查代码发现，是因为编写了如下的代码：

![2021-06-16-09-09-13](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-06-16-09-09-13.png)

我分析我当时是先注入什么对象的，结果因为其他事情给耽搁了，所以就导致写了一半，最后就发生了这个Bug。

## 参考资料

1. [JAVA——JUNIT运行错误[No tests were found]](https://blog.csdn.net/weixin_43272781/article/details/111027076)
   
   我解决问题的时候，这篇教程没有帮到我，当时我定位问题后，发现问题正是文章里罗列的排查项，有点意思。