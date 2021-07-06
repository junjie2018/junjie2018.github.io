我核心想实现的是：自动的完成笔记源文件到Gitbook源文件到Gitbook编译后文件推送到github仓库的工作。

该工作如果手动完成，有如下步骤：

1. 调整md文件中的图片引用，防止更改过md文件的层级关系，无法找到图片的位置
2. 将图片上传到OSS，然后记录图片对应的OSS的位置，这个值在生成Gitbook源文件时要用到
3. 调整md文件中图片引用为OSS中的位置，生成Gitbook源文件
4. 生成GitBook需要的Summary文件
5. 下载GitBook所需要的插件
6. 得到静态的GitBook编译后文件，将编译后的文件推送到github仓库

为了更充分使用这次Github Actions学习机会，我计划开发自己的Github Actions，我的Github Actions设计如下：

1. Adjust Pictures In MD Files
   
   这个Actions的目标是调整md文件中图片引用，确保修改了笔记文件的目录层级，不会找不到相应的图片。

2. Push Pictures To OSS
   
   这个Actions的目标是将md文件中用到图片资源推送到阿里云OSS上

3. Generate Summary.md Book.json And Readme.md
   
   这个Actions如其名所述，用于生成book.json、Summary.md、Readme.md文件用的

4. Build Static Htmls
   
   这个Actions调用Gitbook相关的方法，生成所需要的静态文件。

5. Push Static Htmls To Github
   
   将编译后的静态文件推送到Github中。

在实践中，因为不想倒腾中间产物，为了我将3、4、5步合并成了一个Actions。

## 方案小结

本次实验结果已经完全满足了我最开始的设想，我可以通过我自己开发的Actions完成我博客的自动化部署。但是由于对GitHub Actions知识积累不足，导致我在如下方面做的并不是太好：

1. 没有利用官方提供的action，我现在并不知道官方提供了哪些action，所以我全部都是靠“蛮力”解决的（即自己控制Action的每一步）

2. 冗余数据太多了。我并不是以官方提供的文件夹为工作目录，而是以所使用的Docker镜像的根文件夹为目录，也就是说我开发的每个Action都会首先将源码拉取到当前镜像的Docker根目录中，整个构建工作，该拉取任务供执行了3次。

3. 目录位置混乱，因为GitHub Action在启动我的Action时会指定一个工作目录，导致我不得不在我的脚本中使用觉得路径，所以最终导致我的脚本可读性非常化，目录使用有点混乱。

4. 没有利用Action之间的文件的流转，和第二点一样，我没有研究到这些技术，所以目前选择的是直接在工作区准备一份全量的数据。

5. 脚本没有介个GitHub Action的错误码，我发现有时我的脚本错误，可能不会导致GitHub Action退出，这个问题非常的严重，我后面一定需要花时间优化一下。

6. 密码等重要数据硬编码在Action中，因为目前blogs为私有项目，将密码存在该仓库里分享并不是很大，所以我并没有花时间去研究GitHub相关的技术。

7. Action运行不稳定。因为Action中依赖了许多其他的插件和工具等，这些插件我并没有指定版本，这导致我的Action在运行的时候，有一定的概率发生错误。

我将持续关注这些技术，然后优化我的方案，知道我的方案足够优雅。