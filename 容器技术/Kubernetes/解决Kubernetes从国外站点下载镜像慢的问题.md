## 解决步骤

1. 拿到原镜像地址：

~~~ shell 

kubectl describe po xxx -n xxx # quay.io/kubernetes-ingress-controller/nginx-ingress-controller:0.30.0

~~~

2. 替换为国内的站点

~~~ shell

sudo docker pull quay.mirrors.ustc.edu.cn/kubernetes-ingress-controller/nginx-ingress-controller:0.30.0

~~~

3. 推送到harbor，方便其他虚拟机进行下载

~~~ shell

sudo docker tag quay.mirrors.ustc.edu.cn/kubernetes-ingress-controller/nginx-ingress-controller:0.30.0 192.168.30.174:80/test/nginx-ingress-controller:0.30.0
sudo docker push 192.168.30.174:80/test/nginx-ingress-controller:0.30.0

~~~

4. 其他虚拟机拉下代码，重新打回原tag

~~~ shell

sudo docker pull 192.168.30.174:80/test/nginx-ingress-controller:0.30.0
sudo docker tag  192.168.30.174:80/test/nginx-ingress-controller:0.30.0 quay.io/kubernetes-ingress-controller/nginx-ingress-controller:0.30.0

~~~

## 相关资料

1. [在国内如何拉取 quay.io 的镜像](https://imlc.me/zai-guo-nei-ru-he-la-qu-quay.io-de-jing-xiang)

2. [烂泥：docker.io、gcr.io、quay.io镜像加速(20200413更新)(未实践)](https://www.ilanni.com/?p=14534)

3. [【docker 镜像源】解决quay.io和gcr.io国内无法访问的问题(未实践)](https://www.cnblogs.com/zealousness/p/11116858.html)