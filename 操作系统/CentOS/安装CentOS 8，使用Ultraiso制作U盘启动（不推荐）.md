因为之前使用Ubuntu系统的时候，总是会遇到一些奇奇怪怪的问题，而且这些问题并不是很好查到资料（Ubuntu更新的太快了，网上的教程往往落后于系统发展，而且我们有时候遇到的问题都是别人未遇到过的）。

所以我一直有计划将我工具机的系统更新为CentOS。之前使用工具机时，我会在工具机里再装虚拟机，为了在我开发机上能通过IP直接访问到工具机上的虚拟机，我又在工具机上安装了OpenVPN，这样我的开发机就可以通过挂OpenVPN，访问工具机上的虚拟网络了，很方便一些软件的测试。另外，我还在工具机上装了形形色色的环境和工具软件，时间长了，我也忘记我工具机的具体环境是怎样的了。

所以我计划乘着这次重装CentOS系统，我将这些教程再整理一遍，方便我未来还原我工具机的环境。

## CentOS系统的安装

### 操作步骤

核心操作步骤：

1. 下载CentOS 7的镜像文件

2. 用Ultraiso制作启动盘

3. 在物理机上进行安装

这一部分比较简单，具体细节不贴出来了，如果有需要的建议自行百度“物理机上装CentOS”，我参考的教程如下：

1. [物理机安装linux系统（centos7.6）](https://www.cnblogs.com/zhuimengdeyuanyuan/p/13440561.html)

### 遇到的问题

本着学新不学旧方针，我觉得将Centos 7换成Centos 8，结果在装CentOS 8时就遇到了 `dracut initqueue timeout` 问题。我参考了如下的教程解决该问题：

1. [Linux（CentOs 7）系统重装笔记(一)](https://www.cnblogs.com/congcongdi/p/9929185.html)
2. [dracut-initqueue timeout](https://www.cnblogs.com/dennysong/p/10872575.html)
3. [安装CentOS7出现dracut-initqueue timeout的解决办法](https://blog.csdn.net/xx5595480/article/details/79286199)

我解决该问题的步骤为（这些步骤请参考以上教程食用）：

1. 在一次完整的失败安装后，安装并不会退出，而会出现一个命令行工具，在命令行中执行如下代码（我估计我的U盘是sda打头的）：

~~~ shell

cd /dev/
ls | grep sda

~~~

执行该代码后，会显示出两个结果（此时我U盘保持插着的状态），拔下U盘，再次执行该指令，该指令执行结果为空。可以确定U盘为sda4（猜的成分更多）

2. 重启电脑，进入安装界面，先按TAB后按e，将指令改为：

~~~ shell

vmlinuz initrd=initrd.img inst.stage2=hd:/dev/sda4 quiet

~~~

3. 如此这个问题就解决了。

20210404补充：

之前用Ultraiso装Centos 8时出现了一些奇奇怪怪的问题，我用了相对复杂的方案解决这个问题了，但是我越想越不对劲，我认为是引导出问题了，所以换成了rufus-3.13做U盘启动，结果成功解决该问题。即上述的问题，在使用rufus-3.13做U盘启动的时候，并不会出现。

20210410补充：

Ultraiso和rufus-3.13方案都存在不足，我已经更新为Win32DiskImager方案，目前该方案问题最少。