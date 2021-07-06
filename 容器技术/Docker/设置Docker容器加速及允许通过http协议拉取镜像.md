## 操作步骤

1. 指令如下

~~~ shell

sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://cr58lvy7.mirror.aliyuncs.com"],
  "insecure-registries":["192.168.30.174:80"]
}
EOF

sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "insecure-registries":["172.16.100.100:80"]
}
EOF

sudo systemctl daemon-reload
sudo systemctl restart docker

~~~

## 相关教程

1. [Docker 镜像加速](https://www.runoob.com/docker/docker-mirror-acceleration.html)
2. [镜像加速器](https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors)