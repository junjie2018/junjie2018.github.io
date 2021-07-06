## 问题描述

考虑到最近GitHub经常无法正常使用，而我在开发博客脚本需要从GitHub上拉取推送代码，所以我打算在代码中使用代理，我的代码如下：

~~~ python

Repo.clone_from("https://github.com/junjie2018/blogs.git",
                to_path=blogs_path,
                branch="main",
                config="https.proxy=socks://localhost:1080")

~~~

这份代码的问题是，只会偶尔一次两次成功，我并不确定是什么原因产生的。我有尝试过将socks换为http、https，情况也是一样的，都是偶尔一两次成功。我是在Windows环境下进行测试的。

## 解决方案

这方面的资料太少了，我决定先绕开这个问题，在执行脚本是前，我设置Git客户端的全局代理。

~~~ shell

git config --global http.proxy 'socks5://127.0.0.1:1080'
git config --global https.proxy 'socks5://127.0.0.1:1080'

~~~