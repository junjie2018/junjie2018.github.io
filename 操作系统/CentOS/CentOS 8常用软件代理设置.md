关于代理的问题，现在已经让我非常的头疼了，我计划还是研究一下软路由，尽量让我的工具机流量全部走外网。另外，我可能还需要升级一下我搭梯子的能力，我感觉我现在的梯子没有跑满我的带宽，速度还是有点慢。

## dnf代理设置

打开/etc/dnf/dnf.conf，在[main]后添加：

~~~ shell

proxy=http://192.168.31.154:1080
proxy_username=
proxy_password=


tee -a /etc/dnf/dnf.conf <<-'EOF'
proxy=http://192.168.31.154:1080
proxy_username=
proxy_password=
EOF

20200416补充：

我习惯性的写https_proxy = http://192.168.31.154:1080/，结果遭遇了网络管制，我百思不得解，最后终于尝试返现是将proxy写成了http_proxy，但是dnf本身却没有报任何错误。

~~~

## wget代理设置

wget完整的配置文件位于/etc/wgetrc，不建议改这个文件，可以拷贝一份放在自己的家目录下：

~~~ shell

tee -a ~/.wgetrc <<-'EOF'
https_proxy = http://192.168.31.154:1080/
http_proxy = http://192.168.31.154:1080/
ftp_proxy = http://192.168.31.154:1080/
EOF

tee -a ~/.wgetrc <<-'EOF'
https_proxy = http://192.168.13.113:1080/
http_proxy = http://192.168.13.113:1080/
ftp_proxy = http://192.168.13.113:1080/
EOF

tee ~/.wgetrc <<-'EOF'
https_proxy = http://192.168.13.59:1080/
http_proxy = http://192.168.13.59:1080/
ftp_proxy = http://192.168.13.59:1080/
EOF

# 这种写法不允许，需要研究下

tee -a ~/.wgetrc <<-'EOF'
https_proxy = socks://127.0.0.1:1080/
http_proxy = socks://127.0.0.1:1080/
ftp_proxy = socks://127.0.0.1:1080/
EOF

~~~

## git代理设置

~~~

git config --global http.proxy 'http://192.168.31.154:1080'
git config --global https.proxy 'http://192.168.31.154:1080'

~~~

## 参考文档

1. [为 dnf 设置代理](https://segmentfault.com/a/1110800022310405)
2. [为wget命令设置代理](https://www.cnblogs.com/frankyou/p/6693256.html)