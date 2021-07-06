1. 从Artifact Hub中、repo中搜索chart

~~~ shell

# 从hub中
helm search hub wordpress

# 从repo中
helm repo add brigade https://brigadecore.github.io/charts
helm search repo brigade

~~~

2. 安装chart

~~~ shell

# release名字和chart名字
helm install happy-panda bitnami/wordpress
helm install bitnami/wordpress --generate-name

# 多种来源进行安装
helm install foo foo-0.1.1.tgz
helm install foo path/to/foo
helm install foo https://example.com/charts/foo-1.2.3.tgz

# 查看Release状态
helm status happy-panda

~~~

3. 安装前自定义chart（简单版）

~~~ shell

# 查看chart中的可配置项
helm show values bitnami/wordpress

# 使用yaml格式文件覆盖任意配置
echo '{mariadb.auth.database: user0db, mariadb.auth.username: user0}' > values.yaml
helm install -f values.yaml bitnami/wordpress --generate-name

~~~

4. 升级release和失败时恢复

~~~ shell

# 升级chart的新版本或修改release的配置
helm upgrade -f panda.yaml happy-panda bitnami/wordpress

# 可以用于失败时回滚
helm rollback happy-panda 1

# 查看一个特定release的修订版本号
helm history [RELEASE]

~~~

5. 卸载相关

~~~

helm uninstall happy-panda
helm list

helm uninstall --keep-history
helm list --uninstalled

helm list --all

~~~

6. 仓库管理

~~~

helm repo list
helm repo add dev https://example.com/dev-charts

# 更新仓库
helm repo update

# 移除仓库
helm repo remove dev

~~~

7. 创建Chart

~~~

helm create deis-workflow

# 验证格式
helm lint

# 打包分发
helm package deis-workflow

# 安装chart
helm install deis-workflow ./deis-workflow-0.1.0.tgz

~~~

## 参考资料

1. [使用Helm](https://helm.sh/zh/docs/intro/using_helm/)