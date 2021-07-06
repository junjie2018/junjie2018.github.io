## 操作步骤

1. 关闭用户图形界面，使用tty登录

~~~

sudo systemctl set-default multi-user.target
sudo reboot

~~~

2. 开启用户图形界面

~~~

sudo systemctl set-default graphical.target
sudo reboot

~~~

## 参考资料

1. [Ubuntu18.04 关闭和开启图形界面](https://blog.csdn.net/zw__chen/article/details/86621103)