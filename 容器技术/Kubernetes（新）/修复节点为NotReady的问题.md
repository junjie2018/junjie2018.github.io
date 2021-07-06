这个问题我之前从未遇到过，先说说我怎么发现这个问题的，我使用Helm安装chart时，发现Release一直处于Pending状态，所以我顺手查看了下nodes信息，发现除了Master节点，其他节点全部都处于NotReady状态。

我采用如下方案修复了该问题，将所有节点重启，等待Master节点上的Kubectl指令可用（即Master上的服务启动成功）后，然后对所有从节点执行如下指令：

~~~

systemctl restart docker 
systemctl restart kubelet

~~~

我分析是如下原因造成的，Master节点启动太慢了，导致从节点上的kubelet错误了连接时机。如果下次我重启PVE机器时该问题还会复现，我计划设置PVE的虚拟机启动延时。

## 参考资料

1. [k8s node节点重启后遇到的问题及解决](https://blog.csdn.net/weixin_44723434/article/details/99678843)
2. [安全地清空一个节点](https://kubernetes.io/zh/docs/tasks/administer-cluster/safely-drain-node/)