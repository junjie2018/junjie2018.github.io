## 操作步骤

1. 拉取代码，将Nexus运行起来：

~~~ shell

git clone https://gitee.com/junjie2019/kubernetes.git
cd kubernetes/Devops/Jenkins

kubectl apply -f PersistentVolumeClaim.yml
kubectl apply -f Nexus.yml

kubectl create configmap setting.xml \
  -- from-file=setting.xml \
  -n devops

~~~

## 相关资料

1. [Kubernetes部署Nexus3](https://www.jianshu.com/p/cc4817e014df)