想看一下spring-boot-starter-parent的源码，看看版本裁决时用了哪些变量，但是发现Idea无法调整到这些源码，这就很奇怪了。

我没有找到解决跳转问题的办法，先记录一下。我pom文件中parent标签内容如下：

~~~ xml

<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.5.2</version>
    <relativePath/> <!-- lookup parent from repository -->
</parent>

<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.3.4.RELEASE</version>
</parent>

~~~

通过重新定义spring-boot-starter-parent，从而改变所依赖的组件的版本是一个蛮重要的技术（个人认为），所以以后还是要解决这个问题的。我目前选择的绕开这个问题的方法是，在仓库里手动找到这个pom文件，然后再打开这个文件。

spring-boot-starter-parent项目的父依赖为spring-boot-dependencies，版本裁决时用到的变量都存在了这个项目中。

## 关于版本裁决的利用

针对我们自行开发的starter，如果这些starter中引入了其他的组件，我们想对这些组件进行版本管理，我觉得可行的方案是，我们自行开发一个starter-parent，该starter的parent为spring-boot-starter-parent，其中增加了许多企业内部使用的组件的版本。甚至该starter-parent中可以对spring-boot-starter-parent中的一些变量进行覆盖，从而控制它们的版本号。

我简单开发了一个Demo用于验证我这种思想，项目结构如下，其中demo为父项目，jj-starter-parent为父依赖，jj-demo依赖于jj-starter-parent。

![2021-07-01-23-11-53](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-07-01-23-11-53.png)

demo的pom.xml文件为：

~~~ xml

<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <packaging>pom</packaging>
    <modules>
        <module>jj-parent-starter</module>
        <module>jj-demo</module>
    </modules>

    <groupId>com.example</groupId>
    <artifactId>demo</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>demo</name>
    <description>Demo project for Spring Boot</description>
    <properties>
        <java.version>1.8</java.version>
    </properties>

</project>

~~~

jj-starter-parent的pom.xml文件为：

~~~ xml

<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.5.2</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.example</groupId>
    <artifactId>jj-parent-starter</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>pom</packaging>
    
</project>

~~~

jj-demo的pom.xml文件为：

~~~ xml

<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <groupId>com.example</groupId>
        <artifactId>jj-parent-starter</artifactId>
        <version>0.0.1-SNAPSHOT</version>
        <relativePath>../jj-parent-starter/pom.xml</relativePath> <!-- lookup parent from repository -->
    </parent>

    <modelVersion>4.0.0</modelVersion>

    <artifactId>jj-demo</artifactId>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-configuration-processor</artifactId>
            <optional>true</optional>
        </dependency>

        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.20</version>
            <scope>provided</scope>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-autoconfigure</artifactId>
            <scope>compile</scope>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.junit.vintage</groupId>
                    <artifactId>junit-vintage-engine</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>

~~~

实验中，效果和我预计的一样，基本上是实验成功的。

## 项目中的starter-parent开发思路

待续。。。