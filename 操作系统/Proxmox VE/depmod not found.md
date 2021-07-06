## 问题描述

1. 使用XShellssh到PVE服务器，账号使用的是junjie，然后使用su root切换root用户，再使用depmod指令，这时候会提示not found。

## 解决方法

1. 使用如下指令：

~~~ shell

/sbin/depmod `uname -r`

~~~

## 参考教程

1. [安装Realtek8168 网卡驱动](http://blog.sina.com.cn/s/blog_59929ec30100a7ua.html)