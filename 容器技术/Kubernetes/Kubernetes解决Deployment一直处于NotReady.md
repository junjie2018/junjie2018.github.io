## 处理步骤

1. 查看pods状态，发现pod没有成功创建：

~~~ shell
kubectl get po
~~~

2. 查看deployment状态，发现deployment一直处于NotReady状态，查看其描述，得不到任何讯息

~~~ shell

kubectl get deploy
kubectl describe deploy nfs-client-provisioner

~~~

![2020-07-23-12-01-41](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2020-07-23-12-01-41.png)

3. 查看ReplicationSet状态，发现出错，查看其描述，找到出错的原因

~~~

kubectl get rs
kubectl describe rs nfs-client-provisioner-56596b5cc5

~~~

![2020-07-23-12-02-08](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2020-07-23-12-02-08.png)