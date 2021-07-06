之前的实验中，我发现了如下的问题：

1. 没有相应的ifcfg，我手动copy的（已解决）
2. 有很多时候执行`nmcli c reload`并不会让/etc/sysconfig/network-scripts下的配置文件生效（已解决）
3. enp2s0的ifcfg配置文件不知道该如何写（已解决）
4. 四份ifcfg配置文件中的defaultroute都为yes
5. 存在断线的情况，断线后重连也无法ping通8.8.8.8（已解决）
6. ~~~我的网线是六类网线，但是显示的确实100M~~~
7. 我的网线是六类网线，B450M上显示1000m，J4125上显示100M，同一个网线的两端
8. 全部机器都重启后，笔记本只能ping通192.168.31.1，其他的都显示Ping：传输事故。常见故障（已解决）
9. 很奇怪的一件事，我笔记长时间未动后，无法正常ping，包括不能ping 172.16.100.1/24网段和ping 8.8.8.8。之前与172.16.100.1/24建立的连接不会受到任何印象，但是无法建立新的连接，表现为之前的ssh连接始终是可用的，但是无法创建新的ssh连接。重启笔记本网卡后，这个问题恢复了。而且，我是可以正常与J4125建立连接的。

我目前挨个挨个解决这些问题，有些问题可能还没有解决方案。

## 问题一

1. 删除/etc/sysconfig/network-scripts/下所有的配置文件，然后执行下nmcli c reload

2. 然后执行如下4条指令：

~~~

nmcli conn add con-name enp2s0 ifname enp2s0 type ethernet
nmcli conn add con-name enp3s0 ifname enp3s0 type ethernet
nmcli conn add con-name enp4s0 ifname enp4s0 type ethernet
nmcli conn add con-name enp5s0 ifname enp5s0 type ethernet

~~~

如此就在/etc/sysconfig/network-scripts/目录下就生成了4份ifcfg配置文件

3. 还原拨号上网

~~~

nmcli conn add con-name pppoe-home type pppoe ifname enp2s0 username 13022052202D396 password 123456

nmcli conn up pppoe-home
nmcli conn up pppoe-home # 关闭

~~~

这个地方有些小插曲，我执行了ppp-home添加后，又执行up操作，会提示我出错了，让我看日志信息。我注意到这个时候已经可以ping通了8.8.8.8，所以我分析，add后会理解执行拨号（不太确定哦），我再执行up时发生了冲突，所以报错了。

而且这个时候执行ifconfig，会发现第一次试验时截图中的ppp1变成了ppp0，我觉得这个应该是没有什么印象的。

4. 还原192.168.31.1，确保能够在笔记本上ssh

我这次用了如下指令，相比直接改配置文件更优雅一点：

~~~

nmcli connection modify enp3s0 ipv4.addresses 192.168.31.1/24 ipv4.method manual connection.autoconnect yes
nmcli connection modify enp4s0 ipv4.addresses 192.168.41.1/24 ipv4.method manual connection.autoconnect yes

~~~

这个地方也有小插曲，我执行完指令后注意到：192.168.31.1这个ip地址被分配给了enp5s0网卡，这不符合我的意愿，我注意到nmcli生成的配置文件中没有DEVICE，我手动为4份配置文件加上了DEVICE后，执行nmcli c reload，恢复到正常情况。（我简单验证了下，nmcli命令行貌似不支持DEVICE的设置）

对比配置文件，我发现connection.autoconnect yes等配置项似乎没有映射到我的配置文件。

1. 恢复笔记本上网

~~~

iptables -t nat -A POSTROUTING -s 192.168.31.0/24 -j MASQUERADE
iptables -t nat -A POSTROUTING -s 172.16.100.0/24 -j MASQUERADE

~~~

## 问题二

~~~我不知道这个问题产生的原因，我选择的是将该配置文件删除掉，并用nmcli重新加载，然后再重新生成该配置文件，再用nmcli重新加载。真的是非常奇奇怪怪的现象。~~~

~~~该现象发生的时候，使用nmcli connection modify也无法正常的修改ip地址。~~~

解决这个问题还有一个办法，就是使用nmcli c reload先加载一下，~~~然后使用nmcli c down先关闭网卡，~~~使用SSH的话，使用down然后再执行up会导致SSH断开链接，我发现直接使用up就可以了，然后再重启，我比较喜欢这个方案，更方便一些。

## 问题三

使用问题的方法，自动生成一份配置文件，不进行任何改写。

## 问题四

我目前做了如下的分析：

1. 我使用-j MASQUERADE前，一般都是先ssh到J4125上，然后执行这条指令，我认为此时我的笔记本与J4125已经建立了某种连接，所以导致我此时仍然可以正常访问该机器

2. 当192.168.31.1不可ping通的时候，8.8.8.8也不可ping通，之前认为的将所有的数据包转给了ens2s0网卡的分析应该是站不住脚的，因为这个时候8.8.8.8应该是可以ping通的。

3. VirtualBox的nat模式有个特点：就是虚拟机可以访问主机，访问主机所在网络中的其他机器，但是主机和其他机器无法访问主机。我目前的网络拓补和VirbualBox的nat模式很像，从原理来说，J4125上应该是访问不到我的笔记本的（难道这个是核心的原因么）。但是VMWare中的nat模式，主机和虚拟机是可以互相访问的，这个也没有足够的说服力。

我设计了一写实验：

1. 断开ssh连接，使用netstat -an监控tcp连接，直到两台机器之间没有相关的连接信息了，然后再来进行ping实验（实验结果是依然可以ping通）

2. 重启J4125，然后直接在机器上执行nat指令，在笔记本上进行ping实验（实验结果是依然可以ping通）

## 问题七

我发现网上有其他人也遇到类似的的问题了，在[这篇文章](https://askubuntu.com/questions/1287967/cant-get-rtl8125-realtek-driver-working-on-version-20-04)里，原文简单的摘抄如下：

> When you write "...Well the 9.003.05-1 version I found on ElRepo turned out to be buggy..." are you talking about the sub-par speed in one direction when connected to a 1 GbE network? Or something else?

## 问题八

需要再提供更多的关于这个问题的资料。3400G装的是Linux，这台机器可以正常的ping通192.168.31.1、192.168.41.1。我windows机器只能ping通192.168.31.1。

我将window机器的以太网断掉一段时间后，再重新连接，此时连192.168.31.1都无法正常的ping通。

我将3400G机器重启一下后，再重新连接，此时连192.168.41.1都无法正常的ping通。

这个时候将J4125重启一下，可以笔记本就可以正常的ping通192.168.31.1、192.168.41.1了。3400G机器也在J4125重启后可以正常的ping通。

如果重启笔记本，还是无法正常的ping通的，我可以确定不是iptables遭横的原因，我在该问题发生后，清除了所有的iptables，依旧无法ping通。

我现在基本把问题定位在J4125上了。

我注意到每次重连后，arp都无法正确的显示：

~~~

# 重连前

XiaoQiang (192.168.80.1) at ec:41:18:9f:60:45 [ether] on enp2s0
? (192.168.31.154) at 6c:2b:59:75:6b:e8 [ether] on enp3s0
? (192.168.41.203) at 2c:f0:5d:24:73:f8 [ether] on enp4s0

# 重连后

XiaoQiang (192.168.80.1) at ec:41:18:9f:60:45 [ether] on enp2s0
? (192.168.31.154) at 6c:2b:59:75:6b:e8 [ether] on enp3s0
? (192.168.41.203) at <incomplete> on enp4s0

~~~

同时我注意到，3400G重启后一直在发送ARP报文

![2021-05-03-14-17-52](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-05-03-14-17-52.png)

我是这么分析的，J4125的网卡驱动出了问题，没有正确处理设备的重连，导致3400G实际上根本就没有链接到该机器，也就导致ARP无法更新。怎么验证这个问题呢，我计划加上usb网卡，然后看看能够在usb网卡上重现这个问题。

很难受，我现在基本定位是网卡驱动除了问题，但是这样的话，这个问题就变得非常的复杂了。

我尝试了把驱动降为r8125-9.003.05，没有解决问题！！！

哈哈哈，终于搞定了，我把内核将到了5.4，然后这个问题恢复了，哈哈哈，很开心。

## 参考资料

1. [VMware-centos7 添加网卡后发现没有ifcfg-ens网卡配置文件 使用nmcli来配置ip地址并生成配置文件](https://blog.csdn.net/weixin_44654329/article/details/106575526)
   
   学习了如何为新网卡增加配置文件，及一些nmcli指令的应用

2. [linux配置网络，nmcli配置法及直接修改配置文件法](https://www.bilibili.com/read/cv4560096/)
   
   学习到了nmcli conn modify的使用