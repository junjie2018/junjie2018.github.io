## 说在前面

1. 真的有点喜极而泣的感觉，花了一下午，终于解决了这个问题，心理很舒服

## 解决步骤

1. 下载flannel的yml文件，在如下部分增加一条条目，并修改为需要使用的网卡：

![2020-07-23-18-22-46](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2020-07-23-18-22-46.png)

2. 卸载原有的flannel插件，并安装更改后的插件：

~~~

kubectl delete -f kube-flannel.yml
kubectl apply -f kube-flannel.yml

~~~

## 问题分析

1. 我的虚拟机是用两张网卡的，在搭建Kubernetes时，两张网卡可能会影响到我搭建，所以之前的搭建中我会关闭一张网卡
2. 本次实验中，我使用了kubeadm新的参数：apiserver-advertise-address=172.17.30.101。避免了在搭建阶段对我的影响
3. 但是该方案在搭建完成后，安装flannel时出现新的问题，我认为flannel会默认使用我enp0s3网卡，导致我无法进行实验

## 相关教程

1. [kubernetes--flannel（kubeadm安装）的不同node上的pod间无法通信](https://www.jianshu.com/p/1043ca1ee427)
2. [解决k8s无法通过svc访问其他节点pod的问题(同上)](https://www.jianshu.com/p/ed1ae8443fff)
3. [Kubernetes主机间curl cluster ip时通时不通(给了一点思路)](https://www.jianshu.com/p/816be3a6141f)
4. [解决Kubernetes中Pod无法正常域名解析问题分析与IPVS parseIP Error问题](http://www.mydlq.club/article/78/)
5. [Kubernetes中coredns无法正常域名解析问题分析(同上)](https://blog.csdn.net/networken/article/details/105604173)
