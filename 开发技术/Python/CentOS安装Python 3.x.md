CentOS安装Python实际上有更简单的方法，就是通过yum安装，但是这种方法安装的版本和我win机器的版本不一致，所以我选择了用源码安装。

## 操作步骤

1. 安装编译环境

~~~ shell

yum -y groupinstall development
yum install -y zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel
yum install libffi-devel -y

~~~

2. 去官网下载源码，我下载的是GZipped source tarball：

~~~

https://www.python.org/downloads/release/python-389/

# 3.8.9版本的
wget https://www.python.org/ftp/python/3.8.9/Python-3.8.9.tgz

~~~

3. 解压源码，进行配置，然后编译：

~~~

tar xf Python-3.8.9.tgz && cd Python-3.8.9
./configure --prefix=/usr/local/python3
make && make install

~~~

4. 添加Python到环境变量中：

~~~

cd /etc/profile.d
echo 'export PATH=$PATH:/usr/local/python3/bin/' > python3.sh

source /etc/profile

~~~

## 其他知识

1. 添加环境变量的时候是单独为该程序在/etc/profile.d目录创建一个文件去修改环境变量，这样是方便以后查找和取消添加的环境变量。我之前是直接去更改/etc/profile文件，博文提到的这种方式更便于管理。

2. 某些场景下可能需要重装，比如我在安装后通过pip拉取工具的时候，报了`ModuleNotFoundError: No module named '_ctypes'`错误（相关笔记整理在Python分类下的其他文章中），我需要按照教程下载一些CentOS包，然后重新编译安装，指令如下：

~~~

make clean && make && make install

~~~

## 参考资料

1. [CentOS 7安装Python教程](https://www.jianshu.com/p/2728a846bf43)
2. [CentOS 7安装Python3 笔记](https://www.cnblogs.com/iverson-3/p/12289206.html)
   
   这是我很久前收藏的一篇博文，我应该没有按照这个方案去做。