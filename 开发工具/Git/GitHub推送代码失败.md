## 问题描述

向GitHub仓库推送代码时，先后出现了如下错误：

~~~ shell

git push -u origin main
fatal: unable to access 'https://github.com/junjie2018/blogs.git/': OpenSSL SSL_read: SSL_ERROR_SYSCALL, errno 10054

~~~

~~~ shell

git push -u origin main
fatal: unable to access 'https://github.com/junjie2018/blogs.git/': Failed to connect to github.com port 443: Timed out

~~~


## 解决方案

1. 为Git客户端端配置代理：

~~~ shell

git config --local http.proxy 'socks5://127.0.0.1:1080'
git config --local https.proxy 'socks5://127.0.0.1:1080'

~~~

问题给的信息是真的难以定位，只是习惯性的去配一下代码。