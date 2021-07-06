我开发了一个外卖提醒的脚本，每次运行的时候，会发送一条消息到我们的钉钉群，提醒我们点外卖，我将它放到了我的实验机上。开发的定时任务脚本如下：

~~~

# 编辑/etc/crontab增加如下内容
vi /etc/crontab

# 增加的内容
30,32,35 11 * * 1-6 /root/Software/launch/dist/launch
45,50 17 * * 1-6 /root/Software/launch/dist/launch

# 加载任务
crontab /etc/crontab

# 查看任务
crontab -l
crontab -u root -l

~~~

我想要的效果是：周一到周六每天中午11.30、11.32、11.35提醒一次，晚上17.45、17.50提醒一次。

## 参考资料

1. [CentOS 7 定时计划任务设置](https://blog.csdn.net/qianxing111/article/details/80091187)