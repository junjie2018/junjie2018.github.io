上一次实验中，让我最难以忘怀的每次重新搭建Kubernetes集群时都需要漫长的等待，即使我用了国内的仓库，也需要不停的等待。单纯的搭建K8s，使用阿里的仓库，时间是可以接受的。但是因为后面的装Flannel插件和Istio时及一些杂七杂八的东西时，时间真的受不了。使用国内的仓库，遇到最多的问题就是某个镜像的某些层拉取非常快，而其他的层就拉的非常的慢，我完全无法理解这个现象。

我当时解决该问题的主要方案是，在一台机器上拉取镜像，然后推到我自己的Harbor仓库，然后在其他结点上拉取我Harbor中的镜像，重新打上Tag。这个方案工作量比较大，而且当时我的Harbor也并非处于一个稳定的环境，经常连带虚拟机一起都被我销毁了，所以体验非常的糟糕。

重建Kubernetes集群，对我来说是一件非常频繁的事情，当集群出现了我意料之外的状况时，相比慢慢的排错，不如直接重建（主要是排错需要的知识储备不够）。所以我这次必须解决重建速度慢的问题。

我这次准备了两个方案解决速度慢的问题，一个是透明代理，一个是高质量的使用Harbor。实践中透明代理的速度并不理想，只能达到200~600kb，而且使用透明代理时，依旧存在镜像部分层下载速度巨慢的情况（我不确定是不是我电脑或者我的网络环境造成的）。Harbor提供了类似与Nexus的代理仓库的东西，但是目前的版本只支持Docker Hub和镜像源本身也是使用Harbor的两种场景，非常巧合，K8s搭建时，使用的镜像源是gcr.io，无法被代理（貌似说最新的2.2.1增加了对部分镜像源的支持）。

所以我最终选择的方案是：宿主机挂透明代理，一台虚拟机上拉取所有的镜像，然后push到我的Harbor。使用Kubedam初始化时指定仓库为我自己的Harbor仓库，这个方案有我之前方案的影子，只是细节处有些不同。我计划未来将这个方案脚本化，总之，我的目标还是能够快速的重建K8s、Istio集群。

## 搭建Harbor

1. 官网下载离线安装包，解压该离线包。

2. 从harbor.yml.tmp复制一份harbor.yml文件，修改hostname、数据存储目录、日志存储目录，注释掉https相关的配置

3. 执行./prepare，得到一份docker-compose.yml文件

4. 执行./install.sh，完成镜像的加载，容器的启动。

遇到的问题：

我在安装Harbor遇到的问题，绝大多数人应该都不会遇到。因为我考虑让Harbor的数据更安全，我通过Vagrant的同步目录同步到了主机上。在配置harbor.yml文件时，我将数据和日志文件指向了这个同步目录。结果就导致了harbor-db这个容器没有权限操作这个同步目录。

对这个问题深入研究后发现一些有意思的东西：harbor-db会创建一个progress用户和一个input用户组，来操作数据目录，同时它会把数据目录的所有者修改为progress:input。但是，Vagrant的同步目录是不支持修改用户和用户组的（执行了chown指令后没有任何效果），这最终到时harbor-db容器一直报无权限。

实际上对这个问题更深入研究，也会发现一些无法解释的问题。我将同步目录的权限全部设置为了777，并将所有者设置为root:root，而非默认的vagrant:vagrant。理论上讲，harbor-db容器的任何用户都可以操作这个目录了呀，但是它依旧会报错。我觉得这个报错可能不是系统报的，而是harbor-db的程序无法找到自己需要的用户和用户组为progress:input的数据目录，而报的错误。

我最后决定不使用同步目录了，以后对Harbor所在的虚拟机操作谨慎些，定时同步下吧。又或者，未来有时间将Harbor迁移到J4125那台机器上也行。

## 搭建Kubernetes

### 主节点

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

6. master节点

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

6. 初始化K8s（我执行的是如下指令，如果你没有自己的Harbor，建议参考参考资料部分提供的教程）

~~~

# 初次执行，需要将如下镜像拉取下来推送到自己的Harbor仓库
kubeadm config images list
kubeadm config images pull

docker tag k8s.gcr.io/kube-apiserver:v1.21.0 172.16.100.100:80/library/kube-apiserver:v1.21.0
docker tag k8s.gcr.io/kube-controller-manager:v1.21.0 172.16.100.100:80/library/kube-controller-manager:v1.21.0
docker tag k8s.gcr.io/kube-scheduler:v1.21.0 172.16.100.100:80/library/kube-scheduler:v1.21.0
docker tag k8s.gcr.io/kube-proxy:v1.21.0 172.16.100.100:80/library/kube-proxy:v1.21.0
docker tag k8s.gcr.io/pause:3.4.1 172.16.100.100:80/library/pause:3.4.1
docker tag k8s.gcr.io/etcd:3.4.13-0 172.16.100.100:80/library/etcd:3.4.13-0
docker tag k8s.gcr.io/coredns/coredns:v1.8.0 172.16.100.100:80/library/coredns/coredns:v1.8.0

docker push 172.16.100.100:80/library/kube-apiserver:v1.21.0
docker push 172.16.100.100:80/library/kube-controller-manager:v1.21.0
docker push 172.16.100.100:80/library/kube-scheduler:v1.21.0
docker push 172.16.100.100:80/library/kube-proxy:v1.21.0
docker push 172.16.100.100:80/library/pause:3.4.1
docker push 172.16.100.100:80/library/etcd:3.4.13-0
docker push 172.16.100.100:80/library/coredns/coredns:v1.8.0

kubeadm init \
    --image-repository 172.16.100.100:80/library \
    --kubernetes-version v1.21.0 \
    --pod-network-cidr=10.244.0.0/16 \
    --apiserver-advertise-address=172.16.100.101

~~~

1. 配置kubectl工具

~~~

mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

~~~

8. 获取加入集群的指令

~~~

kubeadm token create --print-join-command

~~~

### 从节点

1. 安装kubeadm、kubelet

~~~ shell

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
yum install -y  kubeadm kubelet

# 启动kubelet服务
systemctl enable kubelet && systemctl start kubelet

~~~

2. 执行加入集群的指令（我直接从教程里Copy过来的）

~~~

kubeadm join 192.168.99.104:6443 --token ncfrid.7ap0xiseuf97gikl 
    --discovery-token-ca-cert-hash sha256:47783e9851a1a517647f1986225f104e81dbfd8fb256ae55ef6d68ce9334c6a2

~~~

### 安装网络插件

1. 翻墙下载配置文件（我计划将kube-flannel.yml文件中的image改为我本地harbor中的镜像），该操作每台机器都需要执行：

~~~

https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml

kubectl apply -f kube-flannel.yml


wget https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
kubectl apply -f kube-flannel.yml

~~~


## 参考资料

1. [CentOS 搭建 K8S，一次性成功，收藏了！](https://segmentfault.com/a/1190000037682150)
   
   核心参考了该教程。

2. [Harbor下载地址](https://github.com/goharbor/harbor/releases)

3. [centos 7.0 查看selinux状态|关闭|开启](https://blog.csdn.net/edide/article/details/52389946)

4. [Linux开启与关闭Swap交换分区以及Putty命令行的基本操作](https://blog.dmyhm.net/?post=57)