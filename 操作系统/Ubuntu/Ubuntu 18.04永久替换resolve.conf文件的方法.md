## 处理步骤

1. 三台虚拟机上运行如下指令

~~~ shell
sudo tee -a /etc/resolvconf/resolv.conf.d/head <<-'EOF'

nameserver 172.17.30.1
nameserver 114.114.114.114
nameserver 114.114.115.115

EOF
~~~

2. 重启所有虚拟机

## 相关教程

1. [Kubernetes中的Pod无法访问外网-Ubuntu16.04 LTS](https://my.oschina.net/u/2306127/blog/1838098?spm=a2c6h.12873639.0.0.4e9e5cb0hf50Gt)

2. [完美解决K8s中的Pod无法解析外网域名问题（该方案行不通，给了思路）](https://blog.csdn.net/weixin_34248258/article/details/92926584)