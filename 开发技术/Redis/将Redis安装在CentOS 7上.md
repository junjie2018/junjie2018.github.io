
## 操作步骤

1. 从官网下载源码包

~~~

yum install -y gcc 

mkdir -p ~/software/redis && cd ~/software/redis
wget http://download.redis.io/releases/redis-5.0.8.tar.gz?_ga=2.113561735.1252722932.1596592540-1218236159.1596592540

mv redis-5.0.8.tar.gz* redis-5.0.8.tar.gz # 不知道行不行

~~~

2. 解压并编译安装

~~~ shell

# yum install -y gcc
tar -zxvf redis-5.0.8.tar.gz

cd redis-5.0.8
make
make install PREFIX=/usr/local/redis

~~~

3. 设置环境变量

~~~ shell

sudo tee -a /etc/profile <<-'EOF'
REDIS_HOME=/usr/local/redis
PATH=${REDIS_HOME}:${PATH}
EOF
source /etc/profile

~~~

## 常用指令

1. 常用指令如下

~~~

redis-cli -h host.com.cn -p
auth password
select 12

keys C*
del xxxx

~~~

## 相关教程

1. [Centos7安装Redis](https://www.cnblogs.com/heqiuyong/p/10463334.html)
2. [redis使用redis-cli查看所有的keys及清空所有的数据](https://www.cnblogs.com/jiftle/p/9534540.html)
3. [通过redis-cli批量删除多个指定模式的key(几乎没用到)](https://blog.csdn.net/qq_22771739/article/details/86767935)