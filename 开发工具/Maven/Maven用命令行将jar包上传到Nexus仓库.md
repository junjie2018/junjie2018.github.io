需求产生于一个非常特殊的场景，在做内部应用上云时，我们的流水线始终无法拉取下面的包（我们已经配置了VPN，且阿里云的Maven仓库已经做了全量同步），所以没有办法，只能通过下面的方式将jar包上传到阿里的仓库上（是这样的么，我好想忘记细节了）

## 指令如下

~~~

mvn deploy:deploy-file \
	-DgroupId=com.oracle \
    -DartifactId=ojdbc7 \
    -Dversion=12.1.0.2 \
    -Dfile=./ojdbc7-12.1.0.2.jar \
    -Durl=http://ci.pc.com.cn/nexus/content/repositories/releases/ \
    -DrepositoryId=releases

~~~

20210421后续：

有意思，我没想到这种需求也能再次遇到，今天在做GRpc相关的功能时，需要用到个grpc-starter.jar的包，这个包在Maven仓库里没有。我先尝试修改上面的指令，改动结果如下，企图简简单单的完成推送任务，但是一直报如下错误：

~~~ shell

mvn deploy:deploy-file \
	-DgroupId=com.sdstc.paas \
    -DartifactId=grpc-starter \
    -Dversion=1.0.0-RELEASE \
    -Dfile=./grpc-starter.jar \
    -Durl=http://192.168.20.9:8081/repository/mavenreleases/ \
    -DrepositoryId=maven-releases

~~~

~~~

[INFO] Scanning for projects...
[INFO]
[INFO] ------------------< org.apache.maven:standalone-pom >-------------------
[INFO] Building Maven Stub Project (No POM) 1
[INFO] --------------------------------[ pom ]---------------------------------
[INFO]
[INFO] --- maven-deploy-plugin:2.7:deploy-file (default-cli) @ standalone-pom ---
Uploading to maven-releases: http://192.168.20.9:8081/repository/mavenreleases/com/sdstc/paas/grpc-starter/1.0.0-RELEASE/grpc-starter-1.0.0-RELEASE.jar
Uploading to maven-releases: http://192.168.20.9:8081/repository/mavenreleases/com/sdstc/paas/grpc-starter/1.0.0-RELEASE/grpc-starter-1.0.0-RELEASE.pom
[INFO] ------------------------------------------------------------------------
[INFO] BUILD FAILURE
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  0.748 s
[INFO] Finished at: 2021-04-21T16:31:57+08:00
[INFO] ------------------------------------------------------------------------
[ERROR] Failed to execute goal org.apache.maven.plugins:maven-deploy-plugin:2.7:deploy-file (default-cli) on project standalone-pom: Failed to deploy artifacts: Could not find artifact com.sdstc.paas:grpc-starter:jar:1.0.0-RELEASE in maven-releases (http://192.168.20.9:8081/repository/mavenreleases/) -> [Help 1]
[ERROR]
[ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.
[ERROR] Re-run Maven using the -X switch to enable full debug logging.
[ERROR]
[ERROR] For more information about the errors and possible solutions, please read the following articles:
[ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/MojoExecutionException

~~~

我登录Nexus后并没有感觉到异常，maven-releases仓库都是存在的。而且maven-releases下有很多com.sdstc的包，我感觉推送到到这块应该是没有问题的（我被包名称上的Release给忽悠了，感觉这个包就应该推到release仓库）。后来我尝试在浏览器访问http://192.168.20.9:8081/repository/mavenreleases/地址，结果发现该地址报404错误。

我再次检查我的setting文件，在文件中找到了新的仓库地址，并检查了该仓库地址，是正常可以访问的，所以重新配置使用这个仓库，最终成功的完成了推送。成功配置如下：

~~~

mvn deploy:deploy-file \
    -DgroupId=com.sdstc.paas \
    -DartifactId=grpc-starter \
    -Dversion=1.0.0-RELEASE \
    -Dfile=./grpc-starter.jar \
    -Durl=http://192.168.20.9:8081/repository/project-repo/  
    -DrepositoryId=project-repo

~~~

在解决这个问题时，我学习到了将包安装在本地的指令，我想我下次可能会优先使用这个指令。我是在Win10上执行的，代码需要写成一行，但是，为了整理笔记时好看，我按Linux的方式重新调整了代码：

~~~

mvn install:install-file \
    -DgroupId=com.sdstc.paas \
    -DartifactId=grpc-starter \
    -Dversion=1.0.0-RELEASE \
    -Dfile=./grpc-starter.jar 
    -Durl=http://192.168.20.9:8081/repository/mavenreleases/  
    -DrepositoryId=maven-releases \
    -Dpackaging=jar

~~~

20210426后续：

没想到还有相关的需求？？？而且我们运维也是通过这种方式上传的（假的吧）,这次我用如下代码上传，遇到了如下的报错，我的解决方法是将jar包从本地的maven仓库中移出来，放到一个临时的文件夹中，然后再执行这行指令（玄学），然后就成功的推送了。

~~~

mvn deploy:deploy-file \
    -DgroupId=cn.hutool \
    -DartifactId=hutool-all \
    -Dversion=5.6.3 \
    -Dfile=./hutool-all-5.6.3.jar \
    -Durl=http://192.168.20.9:8081/repository/project-repo/  
    -DrepositoryId=project-repo

~~~

~~~

// 报错上传的日志

D:\MavenRepository\repository\cn\hutool\hutool-all\5.6.3>mvn deploy:deploy-file -DgroupId=cn.hutool -DartifactId=hutool-all -Dversion=5.6.3 -Dfile=./hutool-all-5.6.3.jar -Durl=http://192.168.20.9:8081/repository/project-repo/ -DrepositoryId=project-repo
[INFO] Scanning for projects...
[INFO]
[INFO] ------------------< org.apache.maven:standalone-pom >-------------------
[INFO] Building Maven Stub Project (No POM) 1
[INFO] --------------------------------[ pom ]---------------------------------
[INFO]
[INFO] --- maven-deploy-plugin:2.7:deploy-file (default-cli) @ standalone-pom ---
[INFO] ------------------------------------------------------------------------
[INFO] BUILD FAILURE
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  0.264 s
[INFO] Finished at: 2021-04-26T10:08:57+08:00
[INFO] ------------------------------------------------------------------------
[ERROR] Failed to execute goal org.apache.maven.plugins:maven-deploy-plugin:2.7:deploy-file (default-cli) on project standalone-pom: Cannot deploy artifact from the local repository: D:\MavenRepository\repository\cn\hutool\hutool-all\5.6.3\hutool-all-5.6.3.jar -> [Help 1]
[ERROR]
[ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.
[ERROR] Re-run Maven using the -X switch to enable full debug logging.
[ERROR]
[ERROR] For more information about the errors and possible solutions, please read the following articles:
[ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/MojoFailureException

// 正确上传的日志

C:\Users\wujj\Desktop\tmp3>mvn deploy:deploy-file -DgroupId=cn.hutool -DartifactId=hutool-all -Dversion=5.6.3 -Dfile=./hutool-all-5.6.3.jar -Durl=http://192.168.20.9:8081/repository/project-repo/ -DrepositoryId=project-repo
[INFO] Scanning for projects...
[INFO]
[INFO] ------------------< org.apache.maven:standalone-pom >-------------------
[INFO] Building Maven Stub Project (No POM) 1
[INFO] --------------------------------[ pom ]---------------------------------
[INFO]
[INFO] --- maven-deploy-plugin:2.7:deploy-file (default-cli) @ standalone-pom ---
Uploading to project-repo: http://192.168.20.9:8081/repository/project-repo/cn/hutool/hutool-all/5.6.3/hutool-all-5.6.3.jar
Uploaded to project-repo: http://192.168.20.9:8081/repository/project-repo/cn/hutool/hutool-all/5.6.3/hutool-all-5.6.3.jar (1.9 MB at 4.2 MB/s)
Uploading to project-repo: http://192.168.20.9:8081/repository/project-repo/cn/hutool/hutool-all/5.6.3/hutool-all-5.6.3.pom
Uploaded to project-repo: http://192.168.20.9:8081/repository/project-repo/cn/hutool/hutool-all/5.6.3/hutool-all-5.6.3.pom (393 B at 2.4 kB/s)
Downloading from project-repo: http://192.168.20.9:8081/repository/project-repo/cn/hutool/hutool-all/maven-metadata.xml
Uploading to project-repo: http://192.168.20.9:8081/repository/project-repo/cn/hutool/hutool-all/maven-metadata.xml
Uploaded to project-repo: http://192.168.20.9:8081/repository/project-repo/cn/hutool/hutool-all/maven-metadata.xml (299 B at 2.4 kB/s)
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  1.514 s
[INFO] Finished at: 2021-04-26T10:55:08+08:00
[INFO] ------------------------------------------------------------------------

~~~

我很讨厌这种充满玄学的问题，我觉得一个问题不知道其底层的缘由都是在给自己给别人埋坑，所以我计划花时间解决好这个问题，我目前的疑惑有：

1. 为什么在本地Maven仓库下不能成功，非要拷贝到一个临时文件夹中执行，是路径太长了么，还是Maven仓库中的一些文件影响了结果（我查看了日志，没有找到任何有用的信息）？

2. 我发现我最终推到Nexus仓库后的pom文件和该jar包原本的pom文件不一致，我觉得这是一个隐患（我有办法解决这个问题了）

![2021-04-26-11-05-53](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-04-26-11-05-53.png)

3. 我觉得存在技术，将本地Maven仓库的某个依赖整体推送到Nexus仓库。我在做阿里云的Maven时，阿里云就提供了这个工具，阿里云是将整个maven的本地仓库都推送过去，我觉得Nexus也提供相应的工具。（我还是没有找到这种工具）

## 问题一

我已经实验过了，不是目录太深、不是目录中带有数字、不是目录与版本号一致造成的问题。

当在本地仓库的jar包所在目录，新建一个临时目录，把jar包放进去，然后执行指令，也是可以成功的上传的。

知道问题的答案后，我不知道该用什么语言形容我的心情，只要你当前使用的maven工具的setting.xml文件中的localRepository和你正在上传的jar包不是同一个仓库，就可以成功的上传。我大致猜到了实现逻辑了，它会先通过命令的groupId、artificialId、version加上setting.xml文件中的localRepository，得到一个jar包的地址，然后与你当前希望上传的jar包地址进行对比，如果两者是一样的就会阻止上传。实际上报错中已经说明白了这个问题:Cannot deploy artifact from the local repository。只是我理解错了方向。

我没有找到解释为什么这样设计的资料。

## 问题二

我用如下指令解决了这个问题，但是前提是我还是得把pom文件和jar包拷贝到一个临时文件夹：

~~~

mvn deploy:deploy-file \
    -DgeneratePom=false \
    -DrepositoryId=project-repo \
    -Durl=http://192.168.20.9:8081/repository/project-repo/ \
    -DpomFile=./hutool-all-5.6.3.pom \
    -Dfile=./hutool-all-5.6.3.jar

~~~

## 参考资料

1. [将jar推送到本地maven库和nexus中的方法](https://blog.csdn.net/weixin_40584261/article/details/88365608)

2. [jar包上传maven私服出错Cannot deploy artifact from the local repository](https://blog.csdn.net/zzb5682119/article/details/54137986)

3. [使用Nexus3搭建Maven私服+上传第三方jar包到本地maven仓库](https://www.cnblogs.com/endv/p/11204704.html)

4. [Maven私服Nexus3.x环境构建操作记录](https://www.cnblogs.com/kevingrace/p/6201984.html)
   
   学习到一些Nexus仓库的基础知识，知道了如果存在proxy仓库，使用第三方包时可以通过这个仓库获取。我们的Nexus也确实存在一个proxy，但是我们的本地环境和dev环境的setting.xml文件中都没有配置这个仓库，难受。





