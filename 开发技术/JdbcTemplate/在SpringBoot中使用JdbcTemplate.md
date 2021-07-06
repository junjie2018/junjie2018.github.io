今天在调MyBatis-Plus Generator时，遇到一个奇怪的问题：我配置了表信息，断点调试的时候发现获取的tableInfo信息时，长度总为零。我敏锐的感觉到是数据库配置出现了问题。MyBatis-Plus Generator没有报任何错误，我使用PostgreSQL的经验比较少，很难定位到问题的原因，所以我决定用JdbcTemplate调试代码。

pom.xml文件如下，我基本上没怎么动，就是使用SpringBoot初始化器初始出一份代码，然后加上Generator相关的代码：

~~~ xml

<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>fun.junjie</groupId>
    <artifactId>mybatisplus-code-generator</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>mybatisplus-code-generator</name>
    <description>Demo project for Spring Boot</description>

    <properties>
        <java.version>1.8</java.version>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <spring-boot.version>2.3.7.RELEASE</spring-boot.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.4.1</version>
        </dependency>

        <dependency>
            <groupId>org.postgresql</groupId>
            <artifactId>postgresql</artifactId>
            <version>42.2.18</version>
        </dependency>

        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-generator</artifactId>
            <version>3.4.1</version>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.freemarker</groupId>
            <artifactId>freemarker</artifactId>
            <version>2.3.31</version>
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

    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>${spring-boot.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.8.1</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <version>2.3.7.RELEASE</version>
                <configuration>
                    <mainClass>fun.junjie.mybatisplus.code.generator.MybatisplusCodeGeneratorApplication</mainClass>
                </configuration>
                <executions>
                    <execution>
                        <id>repackage</id>
                        <goals>
                            <goal>repackage</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>

</project>

~~~

application.properties代码如下

~~~ 

spring.application.name=mybatisplus-code-generator

server.port=8888

spring.datasource.url=jdbc:postgresql://192.168.19.12:5432/dyf?currentSchema=dyf&stringtype=unspecified
spring.datasource.username=postgres
spring.datasource.password=dev.DB
spring.datasource.driver-class-name=org.postgresql.Driver

~~~

这份配置文件中有很多地方需要了解一下：我们数据库结构为：dyf库下的dyf模式。如果在配置的时候不加currentSchema=dyf，则无法检索出目标表。

测试代码如下：

~~~ java

package fun.junjie.mybatisplus.code.generator.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.Map;

/**
 * @author wujj
 */
@RestController
public class TestController {

    @Autowired
    private JdbcTemplate jdbcTemplate;

    @GetMapping("test")
    public void test() {
        Map<String, Object> stringObjectMap = jdbcTemplate.queryForMap("select * from t_dyf_app");
        System.out.println("");
    }
}

~~~

整个实验环境让我花费时间最多的就是，找到currentSchema=dyf配置。在使用该配置前，我尝试了使用spring.datasource.scheme=dyf配置，结果无法正常启动。

该配置在测试环境能正常运行了，但是在我做MyBatis-Plus Generator时，依旧没有作用。在MyBatis-Plus Generator中，需要在DataSourceConfig配置中通过setSchemaName指定dyf模式。因为我后来决定不使用MyBatis-Plus Generator，而是自己开发一个该工具，所以我暂时放弃了对该技术的研究。