## 操作步骤

1. 安装openssh服务端

~~~
sudo apt-get install openssh-server
~~~

2. 编辑配置文件/etc/ssh/sshd_config改PermitRootLogin为yes

~~~

# Authentication:
LoginGraceTime 120
PermitRootLogin yes
StrictModes yes

~~~

1. 重启服务

~~~

/etc/init.d/ssh restart

~~~

## 相关资料
1. [ubuntu 16.0 安装openssh和启动](https://blog.csdn.net/xiao_yuanjl/article/details/79147314)
2. [Ubuntu如何开启SSH服务](https://www.cnblogs.com/jiarenanhao/p/9938280.html)
3. [Ubuntu ssh开机自动启动的设置方法](https://blog.csdn.net/kaikai136412162/article/details/98026747)