## 操作步骤

1. 拉取代码，将Ingress-Nginx运行起来：

~~~ shell

git clone https://gitee.com/junjie2019/kubernetes.git
cd kubernetes/Kubernetes/IngressNginx

kubectl apply -f mandatory.yml
kubectl apply -f service-nodeport.yml

~~~

2. 查看pods时会发现镜像拉不下来，可以查看相关的教程，解决这个问题

## 相关教程

1. [见异思迁：K8s 部署 Nginx Ingress Controller 之 kubernetes/ingress-nginx](https://www.cnblogs.com/dudu/p/12334613.html)

2. [NGINX Ingress Controller(无法下载插件，未使用该方案，该方案目前最新)](https://kubernetes.github.io/ingress-nginx/deploy/)

3. [ingress-nginx github地址](https://github.com/kubernetes/ingress-nginx)