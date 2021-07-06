我没有打算用DockerCompose，直接使用指令的方式也挺简单的。

## 机器环境

1. 系统CentOS 7.9
2. 两块硬盘，一块装系统，一块挂载在/data目录下

## 网络环境

~~~

# 创建自定义bridge
docker network create ToolNet

# 将已有的容器链接到创建的网络（我没有使用该指令）
docker network connect ToolNet [容器名称]

# 查看网络信息
docker network inspect ToolNet

~~~

## MySQL

~~~ shell

# 准备配置文件
mkdir -p /root/Software/Configuration/MySQL/conf
tee /root/Software/Configuration/MySQL/conf/my.cnf <<-'EOF'
[client]
default-character-set=utf8mb4
[mysql]
default-character-set=utf8mb4
[mysqld]
sql-mode=STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION
character-set-server=utf8mb4
EOF

# 启动Docker
sudo docker run -d \
    --restart always -p 3306:3306 \
    --name mysql57 \
    --network ToolNet \
    -v /root/Software/Configuration/MySQL/conf/my.cnf:/etc/mysql/my.cnf \
    -v /data/MySQL/logs:/logs \
    -v /data/MySQL/data:/var/lib/mysql \
    -e MYSQL_ROOT_PASSWORD=HelloWorld \
    mysql:5.7.34

~~~

## MongoDB

这个配置的细节处我还是不太了解，但是现在先这么搞着，方便使用

~~~

docker run -d \
    --restart always -p 27017:27017 \
    --name mongodb \
    --network ToolNet \
    -v /data/MongoDB:/data/backup \
    -e MONGO_INITDB_ROOT_USERNAME=root \
    -e MONGO_INITDB_ROOT_PASSWORD=HelloWorld \
    mongo:4.4.6

docker run -d \
    --restart always -p 8081:8081 \
    --name mongodb-express \
    --network ToolNet \
    -e ME_CONFIG_MONGODB_SERVER="mongodb" \
    -e ME_CONFIG_BASICAUTH_USERNAME="junjie" \
    -e ME_CONFIG_BASICAUTH_PASSWORD="junjie" \
    -e ME_CONFIG_MONGODB_ADMINUSERNAME="root" \
    -e ME_CONFIG_MONGODB_ADMINPASSWORD="HelloWorld" \
    mongo-express:0.54.0

~~~

## YApi

~~~ shell

mkdir -p /root/Software/Configuration/YApi/conf
tee /root/Software/Configuration/YApi/conf/config.json <<-'EOF'
{
  "port": "3000",
  "adminAccount": "junjie2025@gmail.com",
  "timeout":120000,
  "db": {
    "servername": "mongodb",
    "DATABASE": "yapi",
    "port": 27017,
    "user": "root",
    "pass": "HelloWorld",
    "authSource": "admin"
  }
}
EOF

docker run -it \
  --restart always \
  --network ToolNet \
  --entrypoint npm \
  --workdir /yapi/vendors \
  -v /root/Software/Configuration/YApi/conf/config.json:/yapi/config.json \
  registry.cn-hangzhou.aliyuncs.com/anoyi/yapi \
  run install-server

docker run -d \
  --name yapi \
  --network ToolNet \
  --workdir /yapi/vendors \
  -p 3000:3000 \
  -v /root/Software/Configuration/YApi/conf/config.json:/yapi/config.json \
  registry.cn-hangzhou.aliyuncs.com/anoyi/yapi \
  server/app.js

~~~

可选操作，删除builder容器，这个好像没有什么用

~~~

docker container rm -f boring_faraday

~~~

最后登录YApi时的账号密码为：junjie2025@gmail.com、ymfe.org

## EasyYApi

使用如下我自己开发Dockerfile，为什么要使用我自己的仓库了，是因为官方提供的仓库中的package.json中有个插件的版本会导致Bug，需要升级一下。但是我没有找到通过命令行方式升级某个插件版本的方法，所以就自己fork的了一个仓库。

我对我这份Dockerfile还挺满意，我计划将YApi的安装也通过这种自己编辑Dockerfile的方式实现。

~~~ dockerfile

FROM node:12-alpine as builder
WORKDIR /easyyapi
RUN apk add --no-cache git
RUN git clone https://github.com/junjie2018/yapi-markdown-render.git .
RUN npm install

FROM node:12-alpine
ENV TZ="Asia/Shanghai"
WORKDIR /easyyapi
COPY --from=builder /easyyapi /easyyapi
EXPOSE 3001
ENTRYPOINT ["npm", "start", "3001"]

~~~

使用如下指令，构建该镜像，并启动该镜像

~~~

docker build -t yapi-markdown-render:v1 .

# 我还没测试，因为我测试Dockerfile时，该服务已经启动起来了，我懒得重搞，哈哈
docker run -d \
  --name yapi-markdown-render \
  --network ToolNet \
  -p 3001:3001 \
  yapi-markdown-render:v1

~~~

## PostgresSQL

~~~

sudo docker run -d \
    --restart always -p 5432:5432 \
    --name postgres13 \
    --network ToolNet \
    -v /data/PostgresSQL:/var/lib/postgresql/data \
    -e POSTGRES_PASSWORD=HelloWorld \
    postgres:13.3-alpine

~~~

## 踩坑记录

1. 我之前有这么一行配置：

![2021-05-18-10-18-14](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-05-18-10-18-14.png)

这行配置的意思是说，将容器的/var/lib/mysql目录挂载到主机的目录上，我在抄写的时候，少了一层目录，导致无法无法正常启动数据库。

## 相关教程

### MySQL教程

1. [docker 安装 mysql5.7](https://www.cnblogs.com/binz/p/12295374.html)

2. [MySQL5.7 启动报错:initialize specified but the data directory has files in it. Aborting.](https://blog.csdn.net/liyf155/article/details/61420126)

3. [Docker自定义网络和运行时指定IP](https://blog.csdn.net/sbxwy/article/details/78962809)

4. [Docker容器间通信方法](https://juejin.cn/post/6844903847383547911)
   非常重要的教程，在这篇教程里学会了自定义网络的使用。

### MongoDB教程

1. [Docker 下的 MongoDB + Mongo-Express 环境搭建](https://cloud.tencent.com/developer/article/1394986)
   
   学习了四个账号密码环境变量的配置。

2. [Docker搭建MongoDB](https://www.huaweicloud.com/articles/8c36d50a4643d77b4f0abb27555b8aa7.html)
   
   - 学习了`-v /mnt/mongo/backup:/data/backup`配置，但是并不是太满意
   
   - 学习了`docker exec mongo sh -c 'exec var=`date +%Y%m%d%H%M` && mongodump -h localhost --port 27017 -u test -p test1 -d dbname -o /data/backup/$var_test1.dat'`，实际使用中，没有满足我的需求

3. [官方Mongo镜像说明](https://hub.docker.com/_/mongo)

4. [官方MongoExpress镜像说明](https://hub.docker.com/_/mongo-express?tab=description&page=1&ordering=last_updated)

5. [Docker 安装 MongoDB](https://www.runoob.com/docker/docker-install-mongodb.html)

### YAPI教程

1. [顶尖 API 文档管理工具 (YAPI)](https://www.jianshu.com/p/a97d2efb23c5)
   
   我主要参考的是这个教程。

2. [官方GitHub](https://github.com/YMFE/yapi)

3. [使用docker安装yapi](https://www.jianshu.com/p/c7ffdaac12f5)
   [顶尖 API 文档管理工具 (YAPI)](https://www.jianshu.com/p/a97d2efb23c5)
   [Ryan-Miao/docker-yapi](https://github.com/Ryan-Miao/docker-yapi)
   [fjc0k/docker-YApi](https://github.com/fjc0k/docker-YApi)
   [silsuer/yapi](https://hub.docker.com/r/silsuer/yapi)
   [jayfong/yapi](https://hub.docker.com/r/jayfong/yapi)
   [fjc0k/docker-YApi](https://github.com/fjc0k/docker-YApi)

   基本只是参考，并没有走这个方案。

### EasyYApi资料

1. [官方：easyyapi/yapi-markdown-render](https://github.com/easyyapi/yapi-markdown-render)
2. [我改的：junjie2018/yapi-markdown-render](https://github.com/junjie2018/yapi-markdown-render)
3. [Markdown配置部分资料](https://easyyapi.com/documents/yapi_render.html)
4. [使用 Dockerfile 定制镜像](https://yeasy.gitbook.io/docker_practice/image/build)

### PostgreSQL教程

1. [Docker安装PostgreSQL](https://segmentfault.com/a/1190000021492980)

2. [DockerHub PostgreSQL文档](https://hub.docker.com/_/postgres?tab=description&page=1&ordering=last_updated)