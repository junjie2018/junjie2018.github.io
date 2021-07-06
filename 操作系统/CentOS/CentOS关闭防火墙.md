这是一个非常简单的指令，但是我总是拼不对，干脆记下来算了。

~~~

systemctl stop firewalld
systemctl disable firewalld

~~~

20210502后续：

我今天才知道firewalld和iptables不是同一个东西，虽然他们都是基于Netfilter的内核数据包管理工具。

![2021-05-02-10-08-39](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-05-02-10-08-39.png)

因为firewalld默认是拒绝所有进入的流量（除了22号端口），而iptables默认是允许所有的流量的，所以我在进行实验的时候，经常需要关闭firewalld。（其实我还是有点疑惑，我看结构图，firewalld最终也是调回了iptables的命令，可以真正使用iptables -L去查看的时候，又看不到任何的规则）

在搭建OpenVPN时，很有趣，配置完iptables后，往往会关闭防火墙，我一直好奇，防火墙都被关闭了，那数据包又该如何如何按照规则分配呢，现在这个问题明晰了。

还有一个有趣的问题：我之前一直搜索的是如何关闭防火墙，所以搜索到了上面的指令，如果我搜索如何关闭iptables的话，可以得到下面的指令：

~~~

service iptables stop

~~~

## 参考资料

1. [Linux CentOS7关闭防火墙 CentOS6关闭开启防火墙 CentOS打开关闭防火墙](https://www.huaweicloud.com/articles/215b0cbccde290e8e820debc2bf3a397.html)