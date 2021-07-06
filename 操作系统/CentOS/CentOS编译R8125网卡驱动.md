我现在最后悔的事情就是买软路由的时候选了R8125 2.5G网卡，这个网卡是在是太折腾人了。CentOS、PVE上都需要手动的编译安装驱动，ESXI上表现的总是有问题（ESXI 6.7本身也有很多Bug）。不过在折腾这张网卡的时候，也接触了一些较高级的知识，算因祸得福吧。

## 操作步骤

1. 准备相关工具：

~~~

dnf group install "Development Tools"

~~~

2. [官网](https://www.realtek.com/en/component/zoo/category/network-interface-controllers-10-100-1000m-gigabit-ethernet-pci-express-software  )下载驱动，拖到机器中执行如下指令，我选择的是

![2021-05-01-10-07-16](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-05-01-10-07-16.png)

~~~

tar -jxvf r8125-9.005.01.tar.bz2
cd r8125-9.005.01
./autorun.sh

~~~

3. 编译完成后（解决完各种问题后），执行如下指令，完成驱动安装和检查：

~~~

modprode r8125
lsmod | grep r8125
modinfo r8125
reboot

~~~

## 解决的各种问题

我按照时间顺序记录，建议倒序阅读，因为后面的方案可以解决前面的所有问题！！！

1. 第一个问题

~~~

[root@MiWiFi-R4CM-srv r8125-9.005.01]# ./autorun.sh

Check old driver and unload it.
Build the module and install
make[2]: *** /lib/modules/4.18.0-240.el8.x86_64/build: No such file or directory.  Stop.
make[1]: *** [Makefile:171: clean] Error 2
make: *** [Makefile:48: clean] Error 2

~~~

果断定位是kernel-devel工具没装，按照解决VirtualBox相关问题时的方法安装后，发现该报错消失。

2. 第二个问题

~~~

/root/r8125-9.005.01/src/r8125_n.c:62:10: fatal error: linux/pci-aspm.h: No such file or directory
 #include <linux/pci-aspm.h>
          ^~~~~~~~~~~~~~~~~~
compilation terminated.
make[3]: *** [scripts/Makefile.build:316: /root/r8125-9.005.01/src/r8125_n.o] Error 1
make[2]: *** [Makefile:1544: _module_/root/r8125-9.005.01/src] Error 2
make[1]: *** [Makefile:163: modules] Error 2
make: *** [Makefile:41: modules] Error 2

~~~

查了一些资料后意识到是内核版本的问题，果断升级（花了很长时间），参考我相关的资料进行内核升级。

## 参考资料

1. [给PVE6添加Realtek 8125 2.5G网卡驱动](https://www.jianshu.com/p/0fecc79eb79d)
   
   学习了modprode指令的使用，其实我现在也不知道是不是这个指令起的作用，但是原教程给的load module指令，不可用，而且几乎查不到相关的资料。

2. [CentOS8 手工编译安装 Realtek 8125 2.5G网卡驱动](https://blog.csdn.net/lggirls/article/details/103521357)
   
   我遇到的问题，这篇教程一个都没有提，糟心。我只学到了大概了流程。