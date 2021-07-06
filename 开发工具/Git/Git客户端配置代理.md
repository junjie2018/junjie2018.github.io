## 操作步骤

1. 指令如下：

~~~ shell

# 全局

git config --global http.proxy 'http://192.168.13.59:1080'
git config --global https.proxy 'http://192.168.13.59:1080'

git config --global http.proxy 'http://127.0.0.1:1080'
git config --global https.proxy 'http://127.0.0.1:1080'

git config --global http.proxy 'socks5://127.0.0.1:1080'
git config --global https.proxy 'socks5://127.0.0.1:1080'

git config --global http.proxy 'http://192.168.13.113:1080'
git config --global https.proxy 'http://192.168.13.113:1080'

git config --global --unset http.proxy
git config --global --unset https.proxy

# 本地

git config --local http.proxy 'http://127.0.0.1:1080'
git config --local https.proxy 'http://127.0.0.1:1080'

git config --local http.proxy 'socks5://127.0.0.1:1080'
git config --local https.proxy 'socks5://127.0.0.1:1080'

git config --local --unset http.proxy
git config --local --unset https.proxy

~~~