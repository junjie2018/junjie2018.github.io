这个技术存在的意义是可以让我对服务端的数据包管理更严格，避免一些不必要的问题造成我无法正常使用。

1. 使用如下指令得到数据包的文件：

~~~ shell

tcpdump -tttt -s0 -X -vv tcp port 8080 -w captcha.cap
tcpdump -i eth0 -w captcha.cap
tcpdump -i enp34s0 -w captcha.cap

~~~

2. 下载capcha.cap文件到开发机，用鲨鱼进行分析（我这块很简单，东西down下来后，直接可以点开）。