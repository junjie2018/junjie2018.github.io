C盘炸了，只剩下3G了，我基本不在C盘装任何东西，经同时提示发现是hiberfil.sys、pagefile.sys、swapfile.sys文件占用太大空间，这些文件没有什么价值，可以清理掉。

## 清理hiberfil.sys

打开cmd工具，执行powercfg -h off，然后去删除hiberfil.sys文件（此时hiberfil.sys就消失了）

## 调整pagefile.sys文件的大小

截图如下：

![2021-04-19-18-46-14](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-04-19-18-46-14.png)

![2021-04-19-18-46-44](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-04-19-18-46-44.png)

![2021-04-19-18-47-20](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-04-19-18-47-20.png)

![2021-04-19-18-47-44](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-04-19-18-47-44.png)

至于swapfile.sys，我暂时懒得管了，C盘已经腾出来不少空间了~

## 参考资料

1. [hiberfil.sys 可以删吗？【C盘清理】](https://jingyan.baidu.com/article/e75aca85229cab142edac6a9.html)

2. [如何删除pagefile.sys](https://jingyan.baidu.com/article/ab0b5630a859cdc15afa7d29.html)