## 操作步骤

1. 指令如下

~~~

# 创建docker用户组
sudo groupadd docker

# 将当前用户添加到docker组
sudo gpasswd -a ${USER} docker

# 重启服务
sudo service docker restart

# 切换当前会话到新组
newgrp - docker

~~~

## 相关资料
1. [Ubuntu16.04 添加 Docker用户组](https://www.cnblogs.com/dyufei/p/8094549.html)