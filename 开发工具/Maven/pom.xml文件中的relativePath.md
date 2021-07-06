今天用官方源初始化一个SpringBoot项目时发现parent标签下有一个relativePath标签，简单查询了一下，有如下解释：

1. 查找顺序：relativePath元素中的地址 > 本地仓库 > 远程仓库

2. 如果relateivePath设置为一个空值，将始终从仓库中获取，不从本地路径获取。

目前我主要到relateivePath标签主要应用于parent标签中，通过查资料对给标签有了进一步理解（查看该标签的描述信息）：

1. relativePath用于指定父pom.xml的相对位置

2. 父pom.xml默认位置为../pom.xml

3. Maven首先从当前项目的Reactor中寻找父Pom.xml,然后从文件系统中寻找，然后从本地仓库寻找，然后从远程仓库

4. relativePath允许配置一个特定的位置，如果项目是扁平的，或者纵深的（总之没有一个清晰的父Pom）

5. groupId、artifactId、version仍然被需要，而且必须与配置的路径上的pom.xml文件相符合
   
6. 这项特性只加强了项目的本地检查

## 参考资料

1. [maven 工程 pom.xml 中 relativePath 的作用](https://blog.csdn.net/jiangyu1013/article/details/94319284)

2. [Maven parent.relativePath 说明](https://blog.csdn.net/gzt19881123/article/details/105255138)