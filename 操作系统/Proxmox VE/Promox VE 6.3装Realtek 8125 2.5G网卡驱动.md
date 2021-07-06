## 操作步骤

1. 准备编译工具

~~~

apt update && apt install pve-headers-$(uname -r) build-essential

~~~

2. 从如下地址下载驱动源码

~~~

https://www.realtek.com/en/component/zoo/category/network-interface-controllers-10-100-1000m-gigabit-ethernet-pci-express-software

~~~

3. 解压并编译

~~~

tar xvf r8125-9.004.01.tar
cd r8125-9.004.01
make

~~~

4. 安装驱动

~~~

cd src
mkdir -p /lib/modules/5.4.78-2-pve/kernel/drivers/net/ethernet/realtek/
install -m 644 -o root r8125.ko /lib/modules/5.4.78-2-pve/kernel/drivers/net/ethernet/realtek/

/sbin/depmod `uname -r`
/sbin/modprobe r8125

~~~

5. 检验安装结果并重启

~~~

lsmod | grep r8125
modinfo r8125

reboot

~~~

## 个人小结

1.我贴出来的代码，是在我的机器上实际使用的代码（除了驱动版本号与代码中的不符合），教程中提到了装dkms，但是我无法安装该工具，遂放弃。

## 参考教程

1. [给PVE6添加Realtek 8125 2.5G网卡驱动](https://www.jianshu.com/p/0fecc79eb79d)

