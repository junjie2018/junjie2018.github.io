我在本地测试Dockerfile时，习惯性的将ENTRYPOINT写成如下形式：

~~~ dockerfile

ENTRYPOINT ["./entrypoint.sh"]

~~~

但是如果这样写，在GitHub Actions中将会报错：

~~~ dockerfile

docker: Error response from daemon: OCI runtime create failed: container_linux.go:380: starting container process caused: exec: "./entrypoint.sh": stat ./entrypoint.sh: no such file or directory: unknown.

~~~

需要用如下的写法：

~~~ dockerfile

ENTRYPOINT ["/entrypoint.sh"]

~~~

## 原因分析

如下Github Actions在启动Docker容器时会传递如下的参数，而我使用的python:3.8.6镜像的默认位置是根目录，所以就无法使用相对路径找到我的脚本文件。

![2021-07-06-12-14-08](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-07-06-12-14-08.png)