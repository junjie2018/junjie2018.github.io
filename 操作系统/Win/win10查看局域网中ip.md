我将家庭内部网络切换成了172.20.11网段，避免内网穿透时与其他网络环境中的网段冲突。切换后我需要知道我网络中的所有新Ip地址，故找到了这些相关的资料。

~~~

for /L %i IN (1,1,254) DO ping -w 2 -n 1 172.20.11.%i
arp -a

~~~

## 参考资料

1. [windows 查看局域网内所有已使用的IP](https://blog.csdn.net/Sweeping_monk_ren/article/details/90647421)