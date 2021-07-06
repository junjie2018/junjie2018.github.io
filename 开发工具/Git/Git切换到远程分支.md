我的需求是这样的，同事将代码提交到自己的分支上了，我需要查看他的代码，所以我需要切换到他的分支上，然后查看代码，我使用命令行操作git。

我之前将这个问题复杂化了，实际上我只需要在当前分支拉取一下代码，就可以拉取到他的分支的信息，然后使用git checkout切换到他的分支上。

## 参考资料

1. [git 拉取远程分支到本地](https://blog.csdn.net/carfge/article/details/79691360)
   
   简单接触了一下git fetch指令

2. [git获取远程服务器的指定分支](https://www.cnblogs.com/phpper/p/7136048.html)
   
   git pull <远程库名> <远程分支名>:<本地分支名>：单独拉下某个分支，我使用这个指令的场景还是蛮多的

   这条指令还可以拉取远程分支与当前分支合并。

   git pull指令相当于先做了git fetch，再做了git merge