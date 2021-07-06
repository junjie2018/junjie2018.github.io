源目录为source，目标目录为target，则如果target不存在，可直接使用如下指令完成复制：

~~~ shell

cp -r source target

~~~

如果target目录已经存在，则需要使用（我已经踩过该坑），此时执行`cp -r source target`，会将source复制到target中：

~~~ shell

cp -r source/* target

~~~

## 参考资料

1. [linux复制指定目录下的全部文件到另一个目录中，linux cp 文件夹](https://www.cnblogs.com/zdz8207/p/linux-cp-dir.html)