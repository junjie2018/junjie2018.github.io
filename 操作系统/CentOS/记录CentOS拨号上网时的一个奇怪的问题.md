CentOS拨号上网时导致的无法通过192.168.31.217进行ssh的问题。我的解决方案非常的暴力，我拆掉了J4125上的USB网卡，然后将我的笔记本和J4125的enp3s0网卡直连。我对enp3s0网卡进行了如下配置：

~~~
# /etc/sysconfig/network-scripts/ifcfg-enp3s0

TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=static
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=enp3s0
UUID=2a6cd38f-6405-4c45-9bd0-2648e1f26a15
DEVICE=enp3s0
ONBOOT=yes

IPADDR=192.168.31.1
NETMASK=255.255.255.0
GATEWAY=192.168.31.1

~~~

ifcfg-enp3s0这份文件原本是不存在的，我是从系统为我的USB网卡生成的文件中copy出来的，然后改了改。

接下来配置Windows，Windows配置很简单，就是给一个静态的IP地址就好了。只是我在配置的时候遇到了一些怪异的现象，我填好了ip地址后，退出来后。然后在终端执行ipconfig，会发现我配置的ip地址并没有生效，会变成`169.254.69.105`，奇奇怪怪，我完全不知道这个ip地址是怎么产生的。

我多次尝试后，地址依然会变成这个，我只好一一禁用我的网卡，貌似知道我禁用了无线网卡后，该问题才恢复，我其实一开始就断开了我无线的连接，没想到无线网卡还会影响到这个ip的配置，不过我之前无线分配的ip和我这次想设置的ip确实是一样的，可能是发生了什么冲突。

我好奇的是从网络拓补上来讲，目前的网络拓补和使用USB网卡时的网络拓补基本一样的，这次却可以成功的SSH到J4125上。
