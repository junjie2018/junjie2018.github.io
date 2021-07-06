---
title: Ubuntu 18.04配置静态地址
lang: zh-cn
date: 2020-11-07 00:24:27
deploy: true

# CATEGORIES_PLACEHOLDER #

tags:
- ubuntu
- ubuntu18.04
- 静态地址

layout: page

toc: true
---

## 操作步骤

1. 编辑/etc/netplan/01-network-manager-all.yaml（你的机器上可能不是这个文件名，也有可能有多个文件，需要注意观察，并自行分析修改哪个文件），代码如下：

~~~ yaml
network:
  version: 2
  renderer: NetworkManager
  ethernets:
    enp34s0:
      dhcp4: no
      addresses: [192.168.31.29/24]
      optional: true
      gateway4: 192.168.31.1
      nameservers:
        addresses: [114.114.114.114, 8.8.8.8]
~~~

2. 运行如下指令：

~~~ shell
sudo netplan apply
~~~

## 个人总结

1. 整个过程其实并没有那么顺利，前几次试验都失败了，我意识到可能是路由和主机的接口出现了问题，我调整了一个路由器接口后发现enp34s0网口动态的获取到一个ip地址。

2. 我尝试过新起一份文件，同时修改网关的名称为ens33，我期待重新配置一个端口出来，结果并没有生效（我觉着这块我对底层理解的不太到位）。

3. 网卡的名称一定要与ifconfig查到的一致。

## 参考教程

1. [ubuntu 18.04 设置静态ip方法](https://www.cnblogs.com/yaohong/p/11593989.html)
