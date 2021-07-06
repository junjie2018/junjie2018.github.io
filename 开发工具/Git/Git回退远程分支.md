适用场景：在Uat或生产环境等分支不经常变更的场景，为了定位问题，增加了许多无用的代码，并提交到分支上了。待问题解决后，需要删除这些代码，建议适用分支回退功能。

~~~ shell

git checkout uat && git pull

# 本地和远程备份分支
git branch uat_backup && git push origin uat_backup

git reset --hard the_commit_id

# 删除远程分支，并用本地分支重建
git push origin :uat
git push origin uat

# 删除远程备份分支
git push origin :uat_backup

~~~

其实并不建议使用这个方案，这个方案要删除远程分支，可能因为权限等问题无法实现，且可能因为一些意外情况，造成这个分支永远被销毁了。

## 参考资料

1. [git 远程分支回滚](https://blog.csdn.net/u013399759/article/details/52212436)