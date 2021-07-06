## 安装Rancher

1. 执行如下指令

~~~

docker run -d --restart=unless-stopped   -p 80:80 -p 443:443   --privileged   rancher/rancher:latest

~~~

## 导入集群

1. 首先在Cluster选项卡下选择Add Cluster。因为我已经配置过了，这个按钮不见了。

![2021-05-15-09-30-32](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-05-15-09-30-32.png)

2. 在第一栏的Register Existing Cluster里选择Other。我看有的教程选择的是GKE，我这个是最新版，选择GKE不行。

3. 之后会让你执行三条指令，我是如下处理的：

~~~

# 换[USER_ACCOUNT]为root

kubectl create clusterrolebinding cluster-admin-binding --clusterrole cluster-admin --user root

# 不执行

kubectl apply -f https://192.168.13.68/v3/import/x26bbfhcn855gd4h24n59jdcd84l2xvjvv28n2t6fgpt4f77dkrfpf_c-whs58.yaml  


# 执行（在node1节点上执行的）

curl --insecure -sfL https://192.168.13.68/v3/import/x26bbfhcn855gd4h24n59jdcd84l2xvjvv28n2t6fgpt4f77dkrfpf_c-whs58.yaml | kubectl apply -f -    

~~~

## 遇到的问题

1. 我执行第二条的时候，报了如下的错误：

~~~

Unable to connect to the server: x509: certificate is valid for 127.0.0.1, 172.17.0.2, 172.17.0.4, not 192.168.13.68

~~~

我猜想是因为我的集群里证书都是我自己造的，所以有这个问题。

2. 我执行第三条的时候，也遇到了问题，大意说没有给kubectl传递任何东西。我还是怀疑是证书的问题。所以我选择了在浏览器中下载，推到node1上，然后执行（这个可能是个例，不一定都存在这个问题）。

## 参考资料

1. [Rancher部署并导入K8S集群](https://www.cnblogs.com/kevingrace/p/14617757.html)