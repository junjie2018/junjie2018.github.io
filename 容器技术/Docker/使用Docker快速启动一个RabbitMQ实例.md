## 操作步骤

1. 指令如下

~~~ shell

sudo docker pull docker.io/rabbitmq:3.8-management
sudo docker run \
    --name rabbitmq -d \
    -p 15672:15672 -p 5672:5672 \
    docker.io/rabbitmq:3.8-management

~~~

2. 添加账号

~~~ shell

sudo docker exec -i -t rabbitmq bin/bash
rabbitmqctl add_user root 123456
rabbitmqctl set_permissions -p / root ".*" ".*" ".*"
rabbitmqctl set_user_tags root administrator
rabbitmqctl list_users

~~~

3. 管理页面

~~~

http://192.168.30.174:15672

~~~

## 相关教程

1. [docker安装RabbitMq](https://juejin.im/post/6844903970545090574)