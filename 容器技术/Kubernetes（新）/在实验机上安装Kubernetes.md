我为试验机设置了全新的网络环境，完全不必担心镜像下载速度过慢、镜像无法下载的问题。所以相应的教程也非常的清晰明了。

另外需要说明的是，我的所有节点都是根据我制作的模板生成的，我在模板中安装了许多的工具。可以参考我模板配置相关的说明。

## 基础环境准备：

我这次是失误了，这些工作应该在模板节点上做的。这样生成的所有节点都已经被配置好了。

1. 关闭防火墙

~~~ shell

systemctl disable firewalld
systemctl stop firewalld

~~~

2. 关闭selinux

~~~ shell

# 临时禁用selinux
setenforce 0

# 永久关闭 修改/etc/sysconfig/selinux文件设置
sed -i 's/SELINUX=permissive/SELINUX=disabled/' /etc/sysconfig/selinux
sed -i "s/SELINUX=enforcing/SELINUX=disabled/g" /etc/selinux/config

~~~

3. 关闭交换分区

~~~ shell

# 禁用交换分区
swapoff -a

# 永久禁用，打开/etc/fstab注释掉swap那一行。
sed -i 's/.*swap.*/#&/' /etc/fstab

~~~

4. 修改内核参数

~~~ shell

cat <<EOF >  /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
sysctl --system

~~~

5. 重启后检查各台机器的状态

~~~

reboot
sestatus
swapon -s

~~~

## 安装K8S

注意事项：

1. 我推荐指定apiserver-advertise-address参数，是因为有些虚拟机是双网卡的，一张是nat，用于虚拟机访问外部网络，一张是private，用于虚拟机内部访问。如果让kubeadm自己选，可能会选错网卡。
2. 我推荐不指定版本号，用最新的，比较香
3. 不需要指定仓库，我这边基础设施上已经做了处理了，可以很快的拉取到各个镜像。

~~~ shell

# 可不必执行
kubeadm config images list
kubeadm config images pull

kubeadm init \
    --pod-network-cidr=10.244.0.0/16 \
    --apiserver-advertise-address=172.20.11.201

# 用于还原kubeadm init对系统进行的任何改变（用于重装）
kubeadm reset

~~~

## 添加节点


## 参考资料

1. [kubeadm reset](https://kubernetes.io/zh/docs/reference/setup-tools/kubeadm/kubeadm-reset/)