## 问题描述

在导入项目时，我按下图配置了Maven，同时我的setting.xml文件是从我同事那要的。结果我项目中多处报红。

![2021-06-17-09-37-40](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-06-17-09-37-40.png)

## 解决步骤

修改setting.xml文件中如下标签，该标签需要与在Idea中配置位置对应。

~~~ xml

<localRepository>D:\MavenRepository\repository</localRepository>

~~~