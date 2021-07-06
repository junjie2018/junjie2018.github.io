## 说明

1. 这不是一个优雅的方案，只是为了临时用一下
2. 我感觉阿里云也是用的这个方案，但是可能它用的层次更高一些，我只是简单的照搬

## 操作步骤

### master

1. 切换到root用户，执行如下指令：

~~~ shell

mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

~~~

### node

1. vagrant用户，指令如下

~~~ shell

cd ~ && mkdir .kube
scp vagrant@172.17.30.101:~/.kube/config ~/.kube/config

~~~

1. root用户，指令如下

~~~ shell

cd ~ && mkdir .kube
cp /home/vagrant/.kube/config ~/.kube/
sudo chown $(id -u):$(id -g) $HOME/.kube/config

~~~


## 相关教程

1. [ubuntu18.04安装kubernetes(用于查看安装Kubernetes后，如何保证master机器能够使用kubectl)](https://www.jianshu.com/p/04f5b9791dc4?from=singlemessage)

2. [通过kubectl连接Kubernetes集群(我的方案就是阿里云方案的照搬)](https://www.alibabacloud.com/help/zh/doc-detail/86494.htm)

## 更多研究资料（我没有用这些，但是我认为这些方案更好一些）

1. [kubectl客户端工具远程连接k8s集群（有关于证书的资料）](https://www.lagou.com/lgeduarticle/8941.html)
2. [配置kubectl在Mac(本地)远程连接Kubernetes集群](https://www.cnblogs.com/wubolive/p/11225486.html)
3. [使用 kubectl 连接远程 Kubernetes 集群(有kubectl配置资料)](https://www.dazhuanlan.com/2019/12/31/5e0b0fd05cbf8/)
4. [使用 kubectl 连接远程 Kubernetes 集群(有kubectl配置资料)](http://jalan.space/2018/08/25/2018/kubectl/)
5. [配置远程工具访问kubernetes集群(有kubectl配置资料)](https://blog.csdn.net/shenshouer/article/details/52960364)