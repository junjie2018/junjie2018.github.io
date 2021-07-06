> 20210504:
> 该方案在CentOS 7和CentOS 8上均可用

使用Ubuntu时，一直是直接在网上找一篇教程来使用，这次求稳去官方找了篇文档，进行操作，我将我使用到的指令整理如下：

1. 安装yum-utils（yum-utils提供了yun-config-manager工具），并设置文档的仓库（说实话，我对yum和dnf其实还不太熟悉）：

~~~ shell

yum install -y yum-utils
yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo

~~~

2. 安装docker

~~~

yum install docker-ce docker-ce-cli containerd.io

~~~

这个遇到到了一些小插曲，安装的时候报如下错误：

~~~

Last metadata expiration check: 0:02:47 ago on Sat 10 Apr 2021 02:02:50 PM CST.
Error: 
 Problem 1: problem with installed package podman-2.0.5-5.module_el8.3.0+512+b3b58dca.x86_64
  - package podman-2.0.5-5.module_el8.3.0+512+b3b58dca.x86_64 requires runc >= 1.0.0-57, but none of the providers can be installed
  - package podman-2.2.1-7.module_el8.3.0+699+d61d9c41.x86_64 requires runc >= 1.0.0-57, but none of the providers can be installed
  - package containerd.io-1.4.4-3.1.el8.x86_64 conflicts with runc provided by runc-1.0.0-68.rc92.module_el8.3.0+475+c50ce30b.x86_64
  - package containerd.io-1.4.4-3.1.el8.x86_64 obsoletes runc provided by runc-1.0.0-68.rc92.module_el8.3.0+475+c50ce30b.x86_64
  - package containerd.io-1.4.4-3.1.el8.x86_64 conflicts with runc provided by runc-1.0.0-70.rc92.module_el8.3.0+699+d61d9c41.x86_64
  - package containerd.io-1.4.4-3.1.el8.x86_64 obsoletes runc provided by runc-1.0.0-70.rc92.module_el8.3.0+699+d61d9c41.x86_64
  - cannot install the best candidate for the job
  - package runc-1.0.0-64.rc10.module_el8.3.0+479+69e2ae26.x86_64 is filtered out by modular filtering
 Problem 2: problem with installed package buildah-1.15.1-2.module_el8.3.0+475+c50ce30b.x86_64
  - package buildah-1.15.1-2.module_el8.3.0+475+c50ce30b.x86_64 requires runc >= 1.0.0-26, but none of the providers can be installed
  - package buildah-1.16.7-4.module_el8.3.0+699+d61d9c41.x86_64 requires runc >= 1.0.0-26, but none of the providers can be installed
  - package docker-ce-3:20.10.5-3.el8.x86_64 requires containerd.io >= 1.4.1, but none of the providers can be installed
  - package containerd.io-1.4.3-3.1.el8.x86_64 conflicts with runc provided by runc-1.0.0-70.rc92.module_el8.3.0+699+d61d9c41.x86_64
  - package containerd.io-1.4.3-3.1.el8.x86_64 obsoletes runc provided by runc-1.0.0-70.rc92.module_el8.3.0+699+d61d9c41.x86_64
  - package containerd.io-1.4.3-3.2.el8.x86_64 conflicts with runc provided by runc-1.0.0-70.rc92.module_el8.3.0+699+d61d9c41.x86_64
  - package containerd.io-1.4.3-3.2.el8.x86_64 obsoletes runc provided by runc-1.0.0-70.rc92.module_el8.3.0+699+d61d9c41.x86_64
  - package containerd.io-1.4.4-3.1.el8.x86_64 conflicts with runc provided by runc-1.0.0-70.rc92.module_el8.3.0+699+d61d9c41.x86_64
  - package containerd.io-1.4.4-3.1.el8.x86_64 obsoletes runc provided by runc-1.0.0-70.rc92.module_el8.3.0+699+d61d9c41.x86_64
  - cannot install the best candidate for the job
  - package containerd.io-1.4.3-3.1.el8.x86_64 conflicts with runc provided by runc-1.0.0-68.rc92.module_el8.3.0+475+c50ce30b.x86_64
  - package containerd.io-1.4.3-3.1.el8.x86_64 obsoletes runc provided by runc-1.0.0-68.rc92.module_el8.3.0+475+c50ce30b.x86_64
  - package containerd.io-1.4.3-3.2.el8.x86_64 conflicts with runc provided by runc-1.0.0-68.rc92.module_el8.3.0+475+c50ce30b.x86_64
  - package containerd.io-1.4.3-3.2.el8.x86_64 obsoletes runc provided by runc-1.0.0-68.rc92.module_el8.3.0+475+c50ce30b.x86_64
  - package containerd.io-1.4.4-3.1.el8.x86_64 conflicts with runc provided by runc-1.0.0-68.rc92.module_el8.3.0+475+c50ce30b.x86_64
  - package containerd.io-1.4.4-3.1.el8.x86_64 obsoletes runc provided by runc-1.0.0-68.rc92.module_el8.3.0+475+c50ce30b.x86_64
  - package runc-1.0.0-56.rc5.dev.git2abd837.module_el8.3.0+569+1bada2e4.x86_64 is filtered out by modular filtering
  - package runc-1.0.0-64.rc10.module_el8.3.0+479+69e2ae26.x86_64 is filtered out by modular filtering
(try to add '--allowerasing' to command line to replace conflicting packages or '--skip-broken' to skip uninstallable packages or '--nobest' to use not only best candidate packages)

~~~

经过查询资料了解，podman也是一种类似与Docker的容器服务，CentOS会默认安装podman，结果就导致了冲突，所以这个地方需要先将podman给卸载掉。

~~~

yum -y erase podman buildah

~~~

网上对podman的评价还是蛮高的，但是我不清楚我目前研究的Kubernetes技术栈是否有使用podman的计划，如果有的话，我会安排时间研究下这个东西。

3. 启动并设置开机运行Docker

~~~

systemctl start docker
systemctl enable docker

~~~

4. 配置镜像加速

Docker镜像加速，我目前主要用阿里云的。首次使用阿里云镜像加速时需要注册（具体细节我忘记了，可以百度下）

~~~

sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://coded9m2.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker

~~~

## 参考文档

1. [Install Docker Engine on CentOS](https://docs.docker.com/engine/install/centos/)
2. [CentOS8卸载podman安装docker](https://www.wyr.me/post/679)
3. [阿里云镜像加速器](https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors)
4. [Docker 镜像加速教程](https://www.runoob.com/docker/docker-mirror-acceleration.html)