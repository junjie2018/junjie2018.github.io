指令如下：

~~~ shell

sudo curl -L "https://github.com/docker/compose/releases/download/1.29.1/docker-compose-Linux-x86_64" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
docker-compose --version

~~~

~~~

mv docker-compose-Linux-x86_64 /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
docker-compose --version

~~~

## 参考资料

1. [Docker Compose](https://www.runoob.com/docker/docker-compose.html)
   
   教程里的版本太旧了，我用了最新版。

2. [docker/compose](https://github.com/docker/compose/releases)