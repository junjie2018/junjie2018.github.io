## 对模板的配置

我在模板上执行了如下指令（凭记忆回忆的）：

1. 配置下网络环境（我在配置模板机时，网络环境还没有搭起来，所以只能走全局代理的方式了）

~~~

# 公司
export all_proxy=http://192.168.13.113:1080

# 家（通过OpenWrt代理的）
nmcli connection modify eth0 \
    ipv4.addresses 172.20.11.200/24 \
    ipv4.dns 172.20.11.210 \
    ipv4.method manual \
    ipv4.gateway 172.20.11.210 \
    connection.autoconnect yes 
nmcli c up eth0

~~~

1. 安装docker（相关内容，我有更完整的文档说明，可以参考下）。

~~~

yum install -y yum-utils
yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo

yum install docker-ce docker-ce-cli containerd.io

systemctl start docker
systemctl enable docker

~~~

3. 安装lrzsz

~~~

yum install -y lrzsz

~~~

4. 关闭firewalld、selinux、swap，配置内核参数（应该做而忘记做了的）

~~~

# 关闭防火墙
systemctl disable firewalld
systemctl stop firewalld


# 临时禁用selinux
setenforce 0

sed -i 's/SELINUX=permissive/SELINUX=disabled/' /etc/sysconfig/selinux
sed -i "s/SELINUX=enforcing/SELINUX=disabled/g" /etc/selinux/config


# 禁用交换分区
swapoff -a

sed -i 's/.*swap.*/#&/' /etc/fstab


# 修改内核参数
cat <<EOF >  /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
sysctl --system


# 重启后检查配置是否生效
reboot
systemctl status firewalld
sestatus
swapon -s

~~~

我目前就记起了这四步，之后想起来了再整理。

## 安装K8S的工具

~~~ shell

# 配置k8s阿里云源
cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64/
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg https://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
EOF

# 安装kubeadm、kubectl、kubelet
yum install -y kubectl kubeadm kubelet

# 启动kubelet服务
systemctl enable kubelet && systemctl start kubelet

~~~

## 对虚拟机的配置

1. 修改hostname

~~~

hostnamectl set-hostname base
hostnamectl set-hostname node1
hostnamectl set-hostname node2
hostnamectl set-hostname node3
hostnamectl set-hostname node4
hostnamectl set-hostname node5

~~~

2. 编辑/etc/hosts文件（不编辑这个文件，Kubernetes集群初始化时会出现问题）

~~~

tee /etc/hosts <<-'EOF'
127.0.0.1   localhost
192.168.13.68   base
192.168.13.195  node1
192.168.13.83   node2
192.168.13.32   node3
192.168.13.105  node4
192.168.13.236  node5
EOF


tee /etc/hosts <<-'EOF'
127.0.0.1   localhost
172.20.11.201   base
172.20.11.202   node1
172.20.11.203   node2
172.20.11.204   node3
172.20.11.205   node4
172.20.11.206   node5
EOF

~~~

1. 配置网络环境：

~~~

nmcli connection modify eth0 \
    ipv4.addresses 192.168.13.195/24 \
    ipv4.dns 192.168.13.77 \
    ipv4.method manual \
    ipv4.gateway 192.168.13.77 \
    connection.autoconnect yes 
nmcli c up eth0

~~~

完成以上工作，就完成了一个集群的基本配置了。