1. 下载二进制文件，并解压二进制文件

~~~

tar -zxvf helm-v3.5.4-linux-amd64.tar.gz

~~~

2. 将二进制文件移动$PATH目录下

~~~

mv linux-amd64/helm /usr/local/bin/helm

~~~

## Helm常用指令

1. 添加一个chart仓库，并查看charts列表

~~~

helm repo add bitnami https://charts.bitnami.com/bitnami
helm search repo bitnami

~~~

2. 查看chart详细信息，并安装、卸载chart

~~~

helm show chart bitnami/mysql
helm show all bitnami/mysql

helm install bitnami/mysql --generate-name

helm uninstall mysql-1612624192
helm uninstall mysql-1612624192 --keep-history

~~~

3. 查看哪些chart被发布了

~~~

helm ls
helm list

~~~

4. 获取帮助信息

~~~

helm get -h

~~~

## 参考资料

1. [官方教程：安装Helm](https://helm.sh/zh/docs/intro/install/)
2. [Helm二进制文件下载地址](https://github.com/helm/helm/releases)
3. [快速入门指南](https://helm.sh/zh/docs/intro/quickstart/)