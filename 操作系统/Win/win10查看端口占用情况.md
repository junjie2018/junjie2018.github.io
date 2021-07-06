和Linux需求一致，有时候服务启动起来了，需要检查是否正在监听着：

~~~

netstat -aon | findstr "9050"

~~~

## 参考资料

1. [windows下查看端口监听情况](https://blog.csdn.net/shmnh/article/details/12092699)