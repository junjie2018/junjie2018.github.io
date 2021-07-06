我开发的点外卖提醒机器人，在半夜提醒点外卖了，我的定时脚本如下：

~~~

30,32,35 11 * * 1-6 /root/Software/launch/dist/launch
45,50 17 * * 1-6 /root/Software/launch/dist/launch

~~~

我定时任务肯定是没有任何问题的，所以感觉是服务器时间出问题了，所以打算修复一下这个问题：

~~~

# 检查当前时区
timedatectl status | grep 'Time zone'

# 输出为：
Time zone: America/New_York (EDT, -0400)

# 设置硬件时钟调整为本地时钟一致
timedatectl set-local-rtc 1

# 设置时区为上海
timedatectl set-timezone Asia/Shanghai

# 查看时间和时区
date & timedatectl status | grep 'Time zone'


# 输出为：
Tue Jun 29 09:27:36 CST 2021
Time zone: Asia/Shanghai (CST, +0800)

~~~

## 参考资料

1. [CentOS 7同步时间的2种方法](https://www.xiaoz.me/archives/12989)