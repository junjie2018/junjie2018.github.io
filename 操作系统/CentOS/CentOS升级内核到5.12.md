## 操作步骤

1. 常用指令

~~~

uanme -a                # 查看当前内核版本
cat /etc/redhat-release # 查看当前系统版本
rpm -qa | grep kernel   # 查看已安装的内核
yum repolist            # 查看yum当前配置了哪些源

~~~

2. 安装elrepo仓库，并查看仓库配置信息

~~~

rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
yum install https://www.elrepo.org/elrepo-release-8.el8.elrepo.noarch.rpm

cat /etc/yum.repos.d/elrepo.repo

~~~

这一步中，我将elrepo仓库全部配置成enable了，按照教程，如果不配置的话需要在yum的搜索、查询、安装指令上加上`--enablerepo=elrepo-kernel`。额，实际上我执行这些指令的时候也加上了这行配置。

3. 查看新的内核包，及查看kernel-lt和kernel-ml的信息。

~~~

yum search --enablerepo=elrepo-kernel lt # 这条指令没有给到我有用的信息，返回的内容太多了
yum info --enablerepo=elrepo-kernel kernel-lt kernel-ml

~~~

4. 删除旧版本的内核开发工具，并安装新版本的内核，及内核开发工具

~~~

yum remove kernel-tools kernel-tools-libs kernel-headers kernel-devel

yum install --enablerepo=elrepo-kernel -y kernel-ml kernel-ml-headers kernel-ml-devel kernel-ml-tools kernel-ml-tools-libs kernel-devel-ml

yum install --enablerepo=elrepo-kernel -y kernel-lt kernel-lt-headers kernel-lt-devel kernel-lt-tools kernel-lt-tools-libs kernel-devel

~~~

因为我那个折腾人的螃蟹2.5G网卡的驱动，必须要求内核在5.6以上，所以我选择了ml内核。

5. 设置grub（这部分我按教程瞎操作的，我对grub、grub2还不太熟悉）

~~~

grub2-set-default 0
grub2-mkconfig -o /boot/grub2/grub.cfg
reboot

~~~

6. 移除旧版内核（如果能指定版本更好，否则可能有清除到一些有用的工具包）

~~~

yum remove kernel-core-4.18.0 kernel-devel-4.18.0 kernel-tools-libs-4.18.0 kernel-headers-4.18.0
yum remove kernel-lt kernel-lt-devel kernel-lt-tools-libs kernel-lt-headers
yum remove kernel-ml kernel-ml-devel kernel-ml-tools-libs kernel-ml-headers

~~~

## 踩过的坑

1. kernel-ml输入成kernel-mt，导致搜索不到相关的开发包

## 参考资料

1. [Welcome to the ELRepo Project](http://elrepo.org/tiki/tiki-index.php)
   
   我在这个网站学习了设置CentOS 8的ElRepo仓库。

2. [CentOS7.6更新内核](http://gaussli.com/2019/03/06/linux-centos7-6%E6%9B%B4%E6%96%B0%E5%86%85%E6%A0%B8/)
   
   在该教程中学习了很多知识，比如：
   
   1. 查看当前安装的内核版本指令
   2. elrepo是一个企业级Linux的仓库
   3. lt表示longterm（长时间支持版本），ml表示mainline（主线版本）

3. [Centos 8升级内核版本](https://www.cnblogs.com/yanglang/p/13282202.html)
   
   我一开始使用的是这个教程，但是这个教程只给了内核的安装，没有讲到内核工具包，所以我又上其他教程中补充了一些资料。

