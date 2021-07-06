## 前言

1. 怎么说，犯了低级错误，但是在定位解决这个低级错误的时候又学到了很多知识

## 操作步骤

1. 拉取代码，将Jenkins运行起来：

~~~ shell

git clone https://gitee.com/junjie2019/kubernetes.git
cd kubernetes/Devops/Jenkins

kubectl apply -f PersistentVolumeClaim.yml
kubectl apply -f RBAC.yml
kubectl apply -f Jenkins.yml

~~~

1. 如图位置，进入Cloud配置页

![2020-07-24-17-17-46](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2020-07-24-17-17-46.png)
![2020-07-24-17-19-10](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2020-07-24-17-19-10.png)

3. 选择添加kubernetes云，并对如下关键数据配置，配置完成后点击测试连接

![2020-07-24-17-20-26](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2020-07-24-17-20-26.png)
![2020-07-24-17-22-14](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2020-07-24-17-22-14.png)

4. 配置slave信息，完成后点击保存

![2020-07-24-17-23-01](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2020-07-24-17-23-01.png)
![2020-07-24-17-23-30](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2020-07-24-17-23-30.png)

5. 创建测试项目，完成配置测试：

![2020-07-24-17-24-51](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2020-07-24-17-24-51.png)
![2020-07-24-17-25-11](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2020-07-24-17-25-11.png)

## 相关教程

1. [kubernetes中部署Jenkins并简单使用(完全按照这篇文档](https://www.cnblogs.com/coolops/p/13129955.html)

2. [在 Kubernetes 上动态创建 Jenkins Slave(知识点丰富，给了解决问题的思路)](https://www.chenshaowen.com/blog/creating-jenkins-slave-dynamically-on-kubernetes.html)

3. [Kubernetes下Jenkins CI的搭建(截图更晚上，补充了第一篇截图没有截到的地方)](https://hyrepo.com/tech/kubernetes-jenkins/)