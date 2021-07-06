指令如下，这是我目前找到的最优雅，最快速的方法：

~~~

nmcli connection modify eth0 \
    ipv4.addresses 192.168.27.15/24 \
    ipv4.dns 192.168.27.22 \
    ipv4.method manual \
    ipv4.gateway 192.168.27.22 \
    connection.autoconnect yes 
nmcli c up eth0

nmcli connection modify eth0 \
    ipv4.addresses 172.20.11.206/24 \
    ipv4.dns 172.20.11.210 \
    ipv4.method manual \
    ipv4.gateway 172.20.11.210 \
    connection.autoconnect yes 
nmcli c up eth0

~~~

## 参考资料

1. [Centos8 配置静态IP](https://www.cnblogs.com/qianyuliang/p/11591970.html)