我一直以为CentOS在下载软件是会就近选择软件源，但是我今天更新软件时发现速度很慢，所以干脆一不做二不休，直接将软件源替换为国内的。

## 操作步骤

1. 备份/etc/yum.repos.d文件夹（备份是推荐带上年月日，最好再带上操作人）

~~~ shell

cp -r /etc/yum.repos.d /etc/yum.repos.bck20210410

~~~

2. 执行如下指令，更换配置文件中的属性：

~~~ shell

sed -e 's|^mirrorlist=|#mirrorlist=|g' \
         -e 's|^#baseurl=http://mirror.centos.org|baseurl=https://mirrors.tuna.tsinghua.edu.cn|g' \
         -i.bak \
         /etc/yum.repos.d/CentOS-*.repo

~~~

1. 更新缓存

~~~ shell

dnf makecache

~~~

4. 最后更新一下系统软件

~~~

dnf update

~~~

## 参考资料

1. [CentOS 镜像使用帮助](https://mirrors.tuna.tsinghua.edu.cn/help/centos/)
2. [CentOS 8 设置国内安装源](https://my.oschina.net/u/1011130/blog/3191990)