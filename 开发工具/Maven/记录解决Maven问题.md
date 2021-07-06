还原命案现场，deploy的时候出现了如下错误：

~~~

[INFO] ------------------------------------------------------------------------
[INFO] Reactor Summary for thirdplatformcenter 1.0.0:
[INFO] 
[INFO] thirdplatformcenter ................................ FAILURE [  0.667 s]
[INFO] thirdplatform-common ............................... SKIPPED
[INFO] thirdplatform-client ............................... SKIPPED
[INFO] thirdplatform-server ............................... SKIPPED
[INFO] ------------------------------------------------------------------------
[INFO] BUILD FAILURE
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  1.074 s
[INFO] Finished at: 2021-04-26T18:25:18+08:00
[INFO] ------------------------------------------------------------------------
[ERROR] Failed to execute goal org.apache.maven.plugins:maven-deploy-plugin:2.7:deploy (default-deploy) on project thirdplatformcenter: Failed to deploy artifacts: Could not find artifact com.sdstc:thirdplatformcenter:pom:1.0.0 in project-repo (http://192.168.20.9:8081/repository/projectrepo/) -> [Help 1]
[ERROR] 
[ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.
[ERROR] Re-run Maven using the -X switch to enable full debug logging.
[ERROR] 
[ERROR] For more information about the errors and possible solutions, please read the following articles:
[ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/MojoExecutionException


~~~

很熟悉是吧，我在两个不同的场景遇到了相同的报错，而且它们核心的原因都是一样的：仓库的地址找不到。

我先说说这次发生错误的起因后果，我们的项目依赖了一个内部的框架项目：

~~~

    <parent>
        <groupId>com.sdstc</groupId>
        <artifactId>sdstc-spring-boot-parent-start</artifactId>
        <version>1.0.0</version>
        <relativePath/>
    </parent>

~~~

框架项目的maven中配置了仓库信息：

~~~

    <!-- 本地快照 和release 发布 的配置 -->
    <distributionManagement>
        <repository>
            <!-- ID要和MAVEN中conif/setting.xml 中的server保持一致 -->
            <id>project-repo</id>
            <name>project-repo</name>
            <!-- project-repo的url地址 -->
            <url>${REMOTE_REPO}</url>
        </repository>
    </distributionManagement>

    <repositories>
        <repository>
            <id>central</id>
            <name>central</name>
            <!-- 配置仓库的地址 -->
            <url>${REMOTE_PUBLIC}</url>
        </repository>
    </repositories>

~~~

这些仓库信息的具体值是在我们的setting.xml文件中配置的：

~~~

    <profile> 
      <id>remote-repo</id>  
      <properties> 
        <REMOTE_REPO>http://192.168.20.9:8081/repository/projectrepo/</REMOTE_REPO> 
      </properties> 
    </profile>  
    <profile> 
      <id>remote-public</id>  
      <properties> 
        <REMOTE_PUBLIC>http://192.168.20.9:8081/repository/mavenpublic</REMOTE_PUBLIC> 
      </properties> 
    </profile> 

~~~

问题在于哪？在于我们的remote-repo在传承的过程中，将`http://192.168.20.9:8081/repository/project-repo/`地址改为了`http://192.168.20.9:8081/repository/projectrepo/`，而后者是一个不可访问的地址。

解决这个问题的方法有哪些了？第一将地址改为正确的，第二在项目的pom.xml文件中加如下配置（后者用在我定位问题和测试我的思路，前者是我最终的采用的方案）：

~~~

    <distributionManagement>
        <repository>
            <!-- ID要和MAVEN中conif/setting.xml 中的server保持一致 -->
            <id>project-repo</id>
            <name>project-repo</name>
            <!-- project-repo的url地址 -->
            <url>http://192.168.20.9:8081/repository/project-repo/</url>
        </repository>
    </distributionManagement>

~~~

问题解决了，我其实产生了很多新的问题，我之前想尽办法为Maven配置代理的目的是什么？其实上我们的maven-public仓库是配置了阿里云的（感谢小勇，教会我看这个），我使用公司的setting.xml文件时应该是可以拉取到我想要的jar包的。但是当时问题确实太巧合了，我无论如何都拉不到我想要的包，我降了好几个版本都不行，所以我认为是因为仓库原因。

![2021-04-26-18-37-22](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-04-26-18-37-22.png)

另外，我本地不能使用中央仓库不能使用阿里云仓库，这本来就不符合我的开发习惯，就像透明代理一样，是一种我必须解决的技术问题。

后续：

这个问题更有趣的一点是：我们的pom.xml文件是从pdf文件中复制下来，而pdf中这两个仓库地址刚好处于换行的地方，所以在复制的时候被视为单词的换行符，给自动合并了，这是一个什么鬼？？？后面是Adobe PDF中复制出来的结果（貌似Adobe PDF中所有的横线都会变成空格）。

![2021-04-26-18-50-04](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-04-26-18-50-04.png)

![2021-04-26-18-51-50](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-04-26-18-51-50.png)

~~~

<REMOTE_REPO>http://192.168.20.9:8081/repository/project
repo/</REMOTE_REPO>
</prop erties>
</
<
<id>remote public</id>
<
<REMOTE_PUBLIC>http://192.168.20.9:8081/repository/maven
public</REMOTE_PUBLIC>
</
</
</

~~~