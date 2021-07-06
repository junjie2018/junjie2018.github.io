## 问题描述

1. service原为NodePort类型，改为默认类型时，有报错如下：

~~~

kubectl apply -f Jenkins.yml

The Service "jenkins" is invalid: spec.ports[1].nodePort: Forbidden: may not be used when `type` is 'ClusterIP'

~~~

2. 直接删除原Service，重新建一个Service

~~~

kubectl delete -f Jenkins.yml
kubectl apply -f Jenkins.yml

~~~

## 问题分析

1. 可能时不同yml文件走了merge逻辑 