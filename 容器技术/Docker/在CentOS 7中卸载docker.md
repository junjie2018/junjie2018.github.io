之所以选择卸载Docker，是因为我觉得简单工具的安装使用Docker只是提供了安装便利性，工具运行时并没有直接安装在物理机或虚拟机上那么高性能（直觉，未验证）。

~~~

yum list installed | grep docker

# 这种方式貌似会删除较多的东西，还是建议一个一个的删除，安全些
yum -y remove docker*

rm -rf /var/lib/docker

# 如果不执行这一步的话，docker的适配器不会消失
reboot

~~~

## 参考资料

1. [https://blog.csdn.net/liujingqiu/article/details/74783780](Centos 7 如何卸载docker)