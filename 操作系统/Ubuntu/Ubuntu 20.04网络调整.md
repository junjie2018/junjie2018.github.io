## 我使用的方法

1. 禁用ssytemd-resolved（不用犹豫，这东西占用我53号端口，是肯定不行的）

~~~

sudo systemctl disable systemd-resolved
sudo systemctl stop systemd-resolved

~~~

2. 修改/etc/NetworkManager/NetworkManager.conf文件

~~~

[main]
dns=127.0.0.1

~~~

3. 删除符号链接

~~~

rm /etc/resolv.conf

~~~

4. 下载network-manager并重新启动

~~~

apt install -y network-manager
sudo systemctl restart NetworkManager

~~~


## 我认为可行的方法（实践证明，这种方法可行）

实际上我觉得，关闭systemd-resolved后，删除/etc/resolv.conf符号链接，然后手动创建一份/etc/resolv.conf也是可行的。

## 参考资料

1. [如何在Ubuntu中禁用systemd-resolved？](https://qastack.cn/ubuntu/907246/how-to-disable-systemd-resolved-in-ubuntu)
2. [怎么安装network-manager](https://forum.ubuntu.com.cn/viewtopic.php?t=155041)

