## 操作步骤

1. 指令如下

~~~

nmcli conn add con-name pppoe-home type pppoe ifname enp2s0 username 13022052202D396 password 123456

nmcli conn up pppoe-home

nmcli conn modify pppoe-home con-name pppoe



nmcli conn add con-name enp5s0 ifname enp4s0 type ethernet



~~~

我以前一直没有注意到，使用拨号上网的时候，竟然会多出来个这么个东西。

![2021-05-01-16-06-25](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-05-01-16-06-25.png)

## 遇到的问题

1. 遇到如下问题：

~~~

-- Logs begin at Sat 2021-05-01 03:10:09 EDT, end at Sat 2021-05-01 04:00:09 EDT. --
May 01 03:10:14 localhost.localdomain NetworkManager[809]: <info>  [1619853014.1813] manager: (enp2s0): new Ethernet device (/org/freedesktop/NetworkManager/Devices/3)
May 01 03:10:14 localhost.localdomain NetworkManager[809]: <info>  [1619853014.1829] device (enp2s0): state change: unmanaged -> unavailable (reason 'managed', sys-iface-state: 'external')
May 01 03:10:14 localhost.localdomain NetworkManager[809]: <info>  [1619853014.9689] device (enp2s0): state change: unavailable -> disconnected (reason 'none', sys-iface-state: 'managed')
May 01 03:59:43 MiWiFi-R4CM-srv NetworkManager[809]: <info>  [1619855983.6788] device (enp2s0): carrier: link connected
May 01 03:59:43 MiWiFi-R4CM-srv NetworkManager[809]: <info>  [1619855983.6798] device (enp2s0): Activation: starting connection 'pppoe-home' (d1402583-37ea-4cbd-890d-4061e6e66d1f)
May 01 03:59:43 MiWiFi-R4CM-srv NetworkManager[809]: <info>  [1619855983.6800] device (enp2s0): state change: disconnected -> prepare (reason 'none', sys-iface-state: 'managed')
May 01 03:59:43 MiWiFi-R4CM-srv NetworkManager[809]: <info>  [1619855983.6806] device (enp2s0): state change: prepare -> config (reason 'none', sys-iface-state: 'managed')
May 01 03:59:43 MiWiFi-R4CM-srv NetworkManager[809]: <info>  [1619855983.6813] device (enp2s0): state change: config -> ip-config (reason 'none', sys-iface-state: 'managed')
May 01 03:59:43 MiWiFi-R4CM-srv NetworkManager[809]: <warn>  [1619855983.6816] device (enp2s0): PPPoE failed to start: the PPP plugin /usr/lib64/NetworkManager/1.26.0-12.el8_3/libnm-ppp-plugin.so is not installed

~~~

解决方法从如下页面下载相应的dpm包，并进行安装。

~~~

http://mirror.centos.org/centos/8/BaseOS/x86_64/os/Packages/

dnf -y install NetworkManager-ppp-1.26.0-12.el8_3.x86_64.rpm

~~~

我感觉我解决VirtualBox的问题时，给我自己留下了不少的技术资产，我已经用相关的知识定位解决了好几个问题了。

## 复杂的问题

拨号成功后，我可以ping通8.8.8.8，也可以curl到www.baidu.com。目前我的网络拓补如下：

![2021-05-01-17-28-18](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-05-01-17-28-18.png)

这个时候发生了一些怪异的事情，我笔记本能ping通192.168.31.217，也能通过telnetl与其22端口建立连接，但是无法通过xshll建立ssh连接。这种现象我是第一次见，我很难在我的知识范围内找到一个合理的解释。

不过不的不说，我J4125那台机器上是多网卡的，而且现在enp2s0作为了上网卡，在很多单网卡的机器中，流量都会默认从这个网卡出去。我觉得抓包分析，可能会看到这样的现象，但是J4125上的usb网卡，本来就是临时挂载一下，所以我懒得花精力去修复这个问题。



![2021-05-01-16-14-31](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-05-01-16-14-31.png)

## 参考资料

1. [linux配置网络，nmcli配置法及直接修改配置文件法](https://www.bilibili.com/read/cv4560096/)
   
   里面简单提了下nmcli支持拨号上网。







