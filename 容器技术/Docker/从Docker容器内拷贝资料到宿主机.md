我这个需求产生于修复EasyApi的官方提供的渲染服务的Bug。就是我按照官方说明做了镜像后，无法正常启动，所以我决定先在本地试试能否正常启动该服务。

1. 首先确保你的容器处于正在运行的状态，如果没有办法保证，可以使用如下指令：

~~~

docker run -it --entrypoint top yapi-markdown-render:v3 --name tmp-for-test

~~~

2. 使用如下指令，完成copy

~~~

docker cp vibrant_joliot:/easyyapi .

~~~

## 参考资料

1. [docker中宿主机与容器（container）互相拷贝传递文件的方法](https://blog.csdn.net/dongdong9223/article/details/71425077)
2. [docker cmd 能够代替 entrypoint 的所有功能](https://segmentfault.com/q/1010000010668693)