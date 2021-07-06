我在deploy auth-center项目的时候，在根项目执行deploy，报如下错误（没有太多的有用的信息）：

~~~

"C:\Program Files\Java\jdk1.8.0_281\bin\java.exe" -Dmaven.multiModuleProjectDirectory=D:\Project\auth-center "-Dmaven.home=D:\Software\IntelliJ IDEA 2019.1.4\plugins\maven\lib\maven3" "-Dclassworlds.conf=D:\Software\IntelliJ IDEA 2019.1.4\plugins\maven\lib\maven3\bin\m2.conf" "-javaagent:D:\Software\IntelliJ IDEA 2019.1.4\lib\idea_rt.jar=55517:D:\Software\IntelliJ IDEA 2019.1.4\bin" -Dfile.encoding=UTF-8 -classpath "D:\Software\IntelliJ IDEA 2019.1.4\plugins\maven\lib\maven3\boot\plexus-classworlds-2.6.0.jar" org.codehaus.classworlds.Launcher -Didea.version2019.1.4 -s D:\MavenRepository\settings-guoxiong.xml -Dmaven.repo.local=D:\MavenRepository\repository -DskipTests=true deploy -P local

[WARNING] 

[WARNING] Some problems were encountered while building the effective settings

[WARNING] 'servers.server.id' must be unique but found duplicate server with id maven-public @ D:\MavenRepository\settings-guoxiong.xml

[WARNING] 

[INFO] Scanning for projects...

[WARNING] 

[WARNING] Some problems were encountered while building the effective model for com.sdstc:authcenter-common:jar:1.0

[WARNING] The expression ${name} is deprecated. Please use ${project.name} instead.

[WARNING] 

[WARNING] Some problems were encountered while building the effective model for com.sdstc:authcenter-client:jar:1.0

[WARNING] The expression ${name} is deprecated. Please use ${project.name} instead.

[WARNING] 

[WARNING] Some problems were encountered while building the effective model for com.sdstc:authcenter-server:jar:1.0

[WARNING] 'build.plugins.plugin.version' for org.apache.maven.plugins:maven-install-plugin is missing. @ line 136, column 21

[WARNING] 'build.plugins.plugin.version' for org.apache.maven.plugins:maven-deploy-plugin is missing. @ line 152, column 21

[WARNING] 

[WARNING] Some problems were encountered while building the effective model for com.sdstc:authcenter-login:jar:1.0

[WARNING] The expression ${name} is deprecated. Please use ${project.name} instead.

[WARNING] 

[WARNING] It is highly recommended to fix these problems because they threaten the stability of your build.

[WARNING] 

[WARNING] For this reason, future Maven versions might no longer support building such malformed projects.

[WARNING] 

[INFO] ------------------------------------------------------------------------

[INFO] Reactor Build Order:

[INFO] 

[INFO] authcenter-common                                                  [jar]

[INFO] authcenter-client                                                  [jar]

[INFO] authcenter-login                                                   [jar]

[INFO] authcenter-server                                                  [jar]

[INFO] authcenter                                                         [pom]

[INFO] 

[INFO] --------------------< com.sdstc:authcenter-common >---------------------

[INFO] Building authcenter-common 1.0                                     [1/5]

[INFO] --------------------------------[ jar ]---------------------------------

[INFO] 

[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ authcenter-common ---

[INFO] Using 'UTF8' encoding to copy filtered resources.

[INFO] skip non existing resourceDirectory D:\Project\auth-center\authcenter-common\src\main\resources

[INFO] skip non existing resourceDirectory D:\Project\auth-center\authcenter-common\src\main\resources

[INFO] skip non existing resourceDirectory D:\Project\auth-center\authcenter-common\src\main\resources

[INFO] 

[INFO] --- maven-compiler-plugin:3.8.0:compile (default-compile) @ authcenter-common ---

[INFO] Nothing to compile - all classes are up to date

[INFO] 

[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ authcenter-common ---

[INFO] Using 'UTF8' encoding to copy filtered resources.

[INFO] skip non existing resourceDirectory D:\Project\auth-center\authcenter-common\src\test\resources

[INFO] 

[INFO] --- maven-compiler-plugin:3.8.0:testCompile (default-testCompile) @ authcenter-common ---

[INFO] No sources to compile

[INFO] 

[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ authcenter-common ---

[INFO] Tests are skipped.

[INFO] 

[INFO] --- maven-jar-plugin:3.2.0:jar (default-jar) @ authcenter-common ---

[INFO] 

[INFO] --- maven-install-plugin:2.4:install (default-install) @ authcenter-common ---

[INFO] Installing D:\Project\auth-center\authcenter-common\target\authcenter-common.jar to D:\MavenRepository\repository\com\sdstc\authcenter-common\1.0\authcenter-common-1.0.jar

[INFO] Installing D:\Project\auth-center\authcenter-common\pom.xml to D:\MavenRepository\repository\com\sdstc\authcenter-common\1.0\authcenter-common-1.0.pom

[INFO] 

[INFO] --- maven-deploy-plugin:2.7:deploy (default-deploy) @ authcenter-common ---

Uploading to project-repo: http://192.168.20.9:8081/repository/project-repo/com/sdstc/authcenter-common/1.0/authcenter-common-1.0.jar

Uploaded to project-repo: http://192.168.20.9:8081/repository/project-repo/com/sdstc/authcenter-common/1.0/authcenter-common-1.0.jar (237 kB at 2.1 MB/s)

Uploading to project-repo: http://192.168.20.9:8081/repository/project-repo/com/sdstc/authcenter-common/1.0/authcenter-common-1.0.pom

Uploaded to project-repo: http://192.168.20.9:8081/repository/project-repo/com/sdstc/authcenter-common/1.0/authcenter-common-1.0.pom (1.8 kB at 87 kB/s)

Downloading from project-repo: http://192.168.20.9:8081/repository/project-repo/com/sdstc/authcenter-common/maven-metadata.xml

Downloaded from project-repo: http://192.168.20.9:8081/repository/project-repo/com/sdstc/authcenter-common/maven-metadata.xml (302 B at 9.4 kB/s)

Uploading to project-repo: http://192.168.20.9:8081/repository/project-repo/com/sdstc/authcenter-common/maven-metadata.xml

Uploaded to project-repo: http://192.168.20.9:8081/repository/project-repo/com/sdstc/authcenter-common/maven-metadata.xml (302 B at 1.5 kB/s)

[INFO] 

[INFO] --------------------< com.sdstc:authcenter-client >---------------------

[INFO] Building authcenter-client 1.0                                     [2/5]

[INFO] --------------------------------[ jar ]---------------------------------

[INFO] 

[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ authcenter-client ---

[INFO] Using 'UTF8' encoding to copy filtered resources.

[INFO] skip non existing resourceDirectory D:\Project\auth-center\authcenter-client\src\main\resources

[INFO] skip non existing resourceDirectory D:\Project\auth-center\authcenter-client\src\main\resources

[INFO] skip non existing resourceDirectory D:\Project\auth-center\authcenter-client\src\main\resources

[INFO] 

[INFO] --- maven-compiler-plugin:3.8.0:compile (default-compile) @ authcenter-client ---

[INFO] Nothing to compile - all classes are up to date

[INFO] 

[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ authcenter-client ---

[INFO] Using 'UTF8' encoding to copy filtered resources.

[INFO] skip non existing resourceDirectory D:\Project\auth-center\authcenter-client\src\test\resources

[INFO] 

[INFO] --- maven-compiler-plugin:3.8.0:testCompile (default-testCompile) @ authcenter-client ---

[INFO] No sources to compile

[INFO] 

[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ authcenter-client ---

[INFO] Tests are skipped.

[INFO] 

[INFO] --- maven-jar-plugin:3.2.0:jar (default-jar) @ authcenter-client ---

[INFO] 

[INFO] --- maven-install-plugin:2.4:install (default-install) @ authcenter-client ---

[INFO] Installing D:\Project\auth-center\authcenter-client\target\authcenter-client.jar to D:\MavenRepository\repository\com\sdstc\authcenter-client\1.0\authcenter-client-1.0.jar

[INFO] Installing D:\Project\auth-center\authcenter-client\pom.xml to D:\MavenRepository\repository\com\sdstc\authcenter-client\1.0\authcenter-client-1.0.pom

[INFO] 

[INFO] --- maven-deploy-plugin:2.7:deploy (default-deploy) @ authcenter-client ---

Uploading to project-repo: http://192.168.20.9:8081/repository/project-repo/com/sdstc/authcenter-client/1.0/authcenter-client-1.0.jar

Uploaded to project-repo: http://192.168.20.9:8081/repository/project-repo/com/sdstc/authcenter-client/1.0/authcenter-client-1.0.jar (36 kB at 1.3 MB/s)

Uploading to project-repo: http://192.168.20.9:8081/repository/project-repo/com/sdstc/authcenter-client/1.0/authcenter-client-1.0.pom

Uploaded to project-repo: http://192.168.20.9:8081/repository/project-repo/com/sdstc/authcenter-client/1.0/authcenter-client-1.0.pom (2.3 kB at 63 kB/s)

Downloading from project-repo: http://192.168.20.9:8081/repository/project-repo/com/sdstc/authcenter-client/maven-metadata.xml

Downloaded from project-repo: http://192.168.20.9:8081/repository/project-repo/com/sdstc/authcenter-client/maven-metadata.xml (302 B at 14 kB/s)

Uploading to project-repo: http://192.168.20.9:8081/repository/project-repo/com/sdstc/authcenter-client/maven-metadata.xml

Uploaded to project-repo: http://192.168.20.9:8081/repository/project-repo/com/sdstc/authcenter-client/maven-metadata.xml (302 B at 1.7 kB/s)

[INFO] 

[INFO] ---------------------< com.sdstc:authcenter-login >---------------------

[INFO] Building authcenter-login 1.0                                      [3/5]

[INFO] --------------------------------[ jar ]---------------------------------

[INFO] 

[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ authcenter-login ---

[INFO] Using 'UTF8' encoding to copy filtered resources.

[INFO] skip non existing resourceDirectory D:\Project\auth-center\authcenter-login\src\main\resources

[INFO] skip non existing resourceDirectory D:\Project\auth-center\authcenter-login\src\main\resources

[INFO] skip non existing resourceDirectory D:\Project\auth-center\authcenter-login\src\main\resources

[INFO] 

[INFO] --- maven-compiler-plugin:3.8.0:compile (default-compile) @ authcenter-login ---

[INFO] Nothing to compile - all classes are up to date

[INFO] 

[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ authcenter-login ---

[INFO] Using 'UTF8' encoding to copy filtered resources.

[INFO] skip non existing resourceDirectory D:\Project\auth-center\authcenter-login\src\test\resources

[INFO] 

[INFO] --- maven-compiler-plugin:3.8.0:testCompile (default-testCompile) @ authcenter-login ---

[INFO] No sources to compile

[INFO] 

[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ authcenter-login ---

[INFO] Tests are skipped.

[INFO] 

[INFO] --- maven-jar-plugin:3.2.0:jar (default-jar) @ authcenter-login ---

[INFO] 

[INFO] --- maven-install-plugin:2.4:install (default-install) @ authcenter-login ---

[INFO] Installing D:\Project\auth-center\authcenter-login\target\authcenter-login.jar to D:\MavenRepository\repository\com\sdstc\authcenter-login\1.0\authcenter-login-1.0.jar

[INFO] Installing D:\Project\auth-center\authcenter-login\pom.xml to D:\MavenRepository\repository\com\sdstc\authcenter-login\1.0\authcenter-login-1.0.pom

[INFO] 

[INFO] --- maven-deploy-plugin:2.7:deploy (default-deploy) @ authcenter-login ---

Uploading to project-repo: http://192.168.20.9:8081/repository/project-repo/com/sdstc/authcenter-login/1.0/authcenter-login-1.0.jar

Uploaded to project-repo: http://192.168.20.9:8081/repository/project-repo/com/sdstc/authcenter-login/1.0/authcenter-login-1.0.jar (181 kB at 4.2 MB/s)

Uploading to project-repo: http://192.168.20.9:8081/repository/project-repo/com/sdstc/authcenter-login/1.0/authcenter-login-1.0.pom

Uploaded to project-repo: http://192.168.20.9:8081/repository/project-repo/com/sdstc/authcenter-login/1.0/authcenter-login-1.0.pom (3.5 kB at 55 kB/s)

Downloading from project-repo: http://192.168.20.9:8081/repository/project-repo/com/sdstc/authcenter-login/maven-metadata.xml

Downloaded from project-repo: http://192.168.20.9:8081/repository/project-repo/com/sdstc/authcenter-login/maven-metadata.xml (301 B at 15 kB/s)

Uploading to project-repo: http://192.168.20.9:8081/repository/project-repo/com/sdstc/authcenter-login/maven-metadata.xml

Uploaded to project-repo: http://192.168.20.9:8081/repository/project-repo/com/sdstc/authcenter-login/maven-metadata.xml (301 B at 1.8 kB/s)

[INFO] 

[INFO] --------------------< com.sdstc:authcenter-server >---------------------

[INFO] Building authcenter-server 1.0                                     [4/5]

[INFO] --------------------------------[ jar ]---------------------------------

[INFO] 

[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ authcenter-server ---

[INFO] Using 'UTF8' encoding to copy filtered resources.

[INFO] Copying 13 resources

[INFO] Copying 13 resources

[INFO] Copying 0 resource

[INFO] 

[INFO] --- maven-compiler-plugin:3.8.0:compile (default-compile) @ authcenter-server ---

[INFO] Nothing to compile - all classes are up to date

[INFO] 

[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ authcenter-server ---

[INFO] Using 'UTF8' encoding to copy filtered resources.

[INFO] Copying 19 resources

[INFO] 

[INFO] --- maven-compiler-plugin:3.8.0:testCompile (default-testCompile) @ authcenter-server ---

[INFO] Nothing to compile - all classes are up to date

[INFO] 

[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ authcenter-server ---

[INFO] Tests are skipped.

[INFO] 

[INFO] --- maven-jar-plugin:3.2.0:jar (default-jar) @ authcenter-server ---

[INFO] Building jar: D:\Project\auth-center\authcenter-server\target\authcenter.jar

[INFO] 

[INFO] --- spring-boot-maven-plugin:2.2.5.RELEASE:repackage (default) @ authcenter-server ---

[INFO] Replacing main artifact with repackaged archive

[INFO] 

[INFO] --- maven-install-plugin:2.4:install (default-install) @ authcenter-server ---

[INFO] Skipping artifact installation

[INFO] 

[INFO] --- maven-deploy-plugin:2.7:deploy (default-deploy) @ authcenter-server ---

[INFO] Skipping artifact deployment

[INFO] 

[INFO] ------------------------< com.sdstc:authcenter >------------------------

[INFO] Building authcenter 1.0                                            [5/5]

[INFO] --------------------------------[ pom ]---------------------------------

[INFO] 

[INFO] --- maven-install-plugin:2.4:install (default-install) @ authcenter ---

[INFO] Installing D:\Project\auth-center\pom.xml to D:\MavenRepository\repository\com\sdstc\authcenter\1.0\authcenter-1.0.pom

[INFO] 

[INFO] --- maven-deploy-plugin:2.7:deploy (default-deploy) @ authcenter ---

[INFO] ------------------------------------------------------------------------

[INFO] Reactor Summary for authcenter 1.0:

[INFO] 

[INFO] authcenter-common .................................. SUCCESS [  3.053 s]

[INFO] authcenter-client .................................. SUCCESS [  0.516 s]

[INFO] authcenter-login ................................... SUCCESS [  0.817 s]

[INFO] authcenter-server .................................. SUCCESS [  2.236 s]

[INFO] authcenter ......................................... FAILURE [  0.008 s]

[INFO] ------------------------------------------------------------------------

[INFO] BUILD FAILURE

[INFO] ------------------------------------------------------------------------

[INFO] Total time:  6.958 s

[INFO] Finished at: 2021-05-10T14:14:49+08:00

[INFO] ------------------------------------------------------------------------

[ERROR] Failed to execute goal org.apache.maven.plugins:maven-deploy-plugin:2.7:deploy (default-deploy) on project authcenter: Deployment failed: repository element was not specified in the POM inside distributionManagement element or in -DaltDeploymentRepository=id::layout::url parameter -> [Help 1]

[ERROR] 

[ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.

[ERROR] Re-run Maven using the -X switch to enable full debug logging.

[ERROR] 

[ERROR] For more information about the errors and possible solutions, please read the following articles:

[ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/MojoExecutionException

[ERROR] 

[ERROR] After correcting the problems, you can resume the build with the command

[ERROR]   mvn <goals> -rf :authcenter



Process finished with exit code 1

~~~

我并没有定位出这个问题，我解决该问题的方案是只Deploy子项目，不要用根项目Deploy。