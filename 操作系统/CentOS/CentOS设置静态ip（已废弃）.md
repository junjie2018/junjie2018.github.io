## CentOS 8版本

1. 在/etc/sysconfig/network-scripts下，找到你需要配置的网卡配置文件，打开编辑：

~~~


TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=static         #修改：将dhcp修改为stati表示使用静态ip
DEFROUTE=yes
IPADDR=192.168.128.129   #新增：设置IP地址
NETMASK=255.255.255.0    #新增：设置子网掩码
GATEWAY=192.168.128.1    #新增：设置网关
DNS1=114.114.114.114     #新增：设置dns
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=ens33
UUID=e4987998-a4ce-4cef-96f5-a3106a97f5bf
DEVICE=ens33

~~~

~~~

IPADDR=192.168.56.101
NETMASK=255.255.255.0
DNS1=114.114.114.114
GATEWAY=192.168.56.100

~~~

1. 使用`nmcli c reload`命令重启网络

## CentOS 7版本

同上，修改/etc/sysconfig/network-scripts下的问题，然后执行：

~~~

service network restart；

~~~

我没有尝试使用`nmcli c reload`，以后有机会尝试一下。

## 参考教程

1. [centos8 网络配置](https://blog.csdn.net/KLKFL/article/details/102994442)
2. [centos7设置静态IP地址](https://www.cnblogs.com/congcongdi/p/10149925.html)