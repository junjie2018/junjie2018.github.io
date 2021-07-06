## 操作步骤

1. 搭建NFS服务器，想考相关教程

2. 配置Kubernetes使用NFS持久卷，指令如下：

~~~ shell

git clone https://gitee.com/junjie2019/kubernetes.git
cd kubernetes/PersistentVolume2

kubectl apply -f rabc.yml
kubectl apply -f deployment.yml
kubectl apply -f class.yml
kubectl apply -f test-claim.yml
kubectl apply -f test-pod.yml

~~~

## 相关教程

1. [利用NFS动态提供Kubernetes后端存储卷](https://jimmysong.io/kubernetes-handbook/practice/using-nfs-for-persistent-storage.html)

2. [K8S 之 使用NFS作为持久卷使用POD](https://blog.51cto.com/12965094/2490491?source=dra)