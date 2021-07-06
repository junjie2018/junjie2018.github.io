添加依赖：

~~~ xml

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
    <optional>true</optional>
</dependency>

~~~

开发application.yml:

~~~ yaml

project-config:
  template-dir: 'D:\Download\spring-demo-master\spring-demo-master\cn\AutoTools\src\main\resources\templates'
  temp-dir: 'C:\Users\wujj\Desktop\Temp'
  default-primary-key-name: 'id'
  table-info-dir: 'D:\Download\spring-demo-master\spring-demo-master\cn\AutoTools\src\main\resources\tables'
  enum-comment-pattern: '^([\\u4e00-\\u9fa5]{1,})（(([A-Za-z0-9-]+：[\\u4e00-\\u9fa5]{1,}，?)+)）$'
  number-pattern: '^[0-9]*$'

~~~

开发配置Bean：

~~~ java

@Data
@Component
@ConfigurationProperties(prefix = "project-config")
public class ProjectConfig {
    /**
     * 模板文件根目录
     */
    private String templateDir;

    /**
     * 临时文件夹
     */
    private String tempDir;

    /**
     * 默认的主键字段名
     */
    private String defaultPrimaryKeyName;

    /**
     * 储存表信息Yaml文件的文件夹
     */
    private String tableInfoDir;

    /**
     * 枚举的模式
     */
    private String enumCommentPattern;

    /**
     * 数字的模式
     */
    private String numberPattern;
}

~~~

## @Value与@ConfigurationProperties的对比

![2021-06-17-11-45-57](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-06-17-11-45-57.png)

之前没有关注到这个层面的问题，之后的使用中需要注意一下这个问题。

## 参考资料

1. [SpringBoot 获取yml配置文件信息](https://blog.csdn.net/u012448904/article/details/103912606)