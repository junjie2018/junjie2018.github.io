我在Debain上使用默认的库安装Node.js，版本过低，报npm冲突了。所以我不得不寻找其他的安装方法。

我最终使用的指令如下：

~~~ shell

curl -sL https://deb.nodesource.com/setup_12.x | bash -
sudo apt install nodejs

~~~

## 参考资料

1. [在Debian 10系统上安装Node.js和npm的三种不同方法](https://ywnz.com/linuxjc/5819.html)