## 操作步骤

### 准备基础环境：

1. 克隆我的个人项目，启动三台虚拟机

~~~ shell

git clone https://github.com/junjie2018/vagrant.git
cd vagrant/cluster/ssh_kubernetes/kubernetes
vagrant up

~~~

2. 关闭工具机的防火墙，使开发机能直接访问虚拟机：

~~~ shell

sudo ufw disable

~~~

3. 开发机挂VPN，连接到三台虚拟机，并更新三台虚拟机的软件源为国内源

4. 为三台虚拟机安装docker，并设置Docker容器加速

~~~ shell

sudo apt-get install -y docker.io

~~~

### 安装Kubernetes

1. 关闭swap，并测试关闭是否成功

~~~ shell

sudo swapoff -a
free -h

~~~

2. 为三台虚拟机必要的基础工具（kubeadm需要用到的）

~~~ shell

sudo apt update && sudo apt install -y apt-transport-https
curl https://mirrors.aliyun.com/kubernetes/apt/doc/apt-key.gpg | sudo apt-key add - 

~~~

4. 配置镜像kubeadm仓库地址

~~~ shell

sudo tee /etc/apt/sources.list.d/kubernetes.list <<-'EOF'
deb https://mirrors.aliyun.com/kubernetes/apt/ kubernetes-xenial main
EOF
sudo apt update

~~~

4. 安装 kubelet、kubeadm、kubectl并验证安装是否成功

~~~ shell

sudo apt install -y kubelet kubeadm kubectl
kubelet --version

~~~

5. 初始化Kubernetes集群

~~~ shell

kubeadm init \
    --image-repository registry.aliyuncs.com/google_containers \
    --kubernetes-version v1.18.2 \
    --pod-network-cidr=10.244.0.0/16 \
    --apiserver-advertise-address=172.17.30.101

~~~

1. 安装flannel组件：

~~~ shell

# 需要外网，可以在浏览器上下载下来，再传到虚拟机上
wget https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
kubectl apply -f kube-flannel.yml

~~~

7. 漫长的等待后，验证是否成功：

~~~ shell

kubectl get pods --all-namespaces

~~~

## 其他知识

1. 忘记了node添加到master时的指令，可以通过如下指令获取

~~~ shell

kubeadm token create --print-join-command --ttl 0

~~~

## 相关教程

1. [ubuntu18.04安装kubernetes](https://www.jianshu.com/p/04f5b9791dc4?from=singlemessage)
2. [ubuntu18.04搭建 kubernetes（k8s）集群](https://blog.csdn.net/weixin_30840253/article/details/98305987)
3. [Kubernetes集群的简单搭建（flannel.yml文件，及相关内容）](https://www.jianshu.com/p/bbc52c760184)
4. [Docker 镜像加速](https://www.runoob.com/docker/docker-mirror-acceleration.html)
5. [kubenetes使用kubeadm查询添加节点到集群的命令](https://blog.csdn.net/fuck487/article/details/104663137)

## 后记

1. 我以为apiserver-advertise-address参数可以帮我解决双网卡的问题，没想到我还是踩到了双网卡的坑，见相关教程。