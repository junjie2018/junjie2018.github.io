该问题由同事定位并修复，我在一旁参观了整个过程。我们的代码如下：

~~~ java

@Select("<script>" +
        " SELECT * FROM t_user where is_delete = 0" +
        " <if test='request.account != null'>" +
        "   AND account like #{request.account} " +
        " </if>" +
        " <if test='request.registStatus != null'>" +
        "   AND regist_status = #{request.registStatus} " +
        " </if>" +
        " <if test='request.name != null'>" +
        "   AND name like #{request.name} " +
        " </if>" +
        " <if test='request.status != null'>" +
        "   AND status = #{request.status} " +
        " </if>" +
        " <if test='request.startTime != null'>" +
        "   AND gmt_create_time >= #{request.startTime} " +
        " </if>" +
        " <if test='request.endTime != null'>" +
        "   AND gmt_create_time >= #{request.endTime} " +
        " </if>" +
        " order by gmt_create_time" +
        "</script>")
IPage<User> getUserListPage(Page<User> objectPage, SearchExperienceAccountRequest request);

~~~

结果项目在启动的是否报了如下的错误：

~~~

2021-06-16 15:11:52.260 WARN  AbstractApplicationContext.java:558- Exception encountered during context initialization - cancelling refresh attempt: org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'accountController' defined in file [D:\Project\auth-center\authcenter-server\target\classes\com\sdstc\authcenter\controller\AccountController.class]: Unsatisfied dependency expressed through constructor parameter 0; nested exception is org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'accountServiceImpl' defined in file [D:\Project\auth-center\authcenter-server\target\classes\com\sdstc\authcenter\service\impl\AccountServiceImpl.class]: Unsatisfied dependency expressed through constructor parameter 0; nested exception is org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'companyMapper' defined in file [D:\Project\auth-center\authcenter-server\target\classes\com\sdstc\authcenter\mapper\CompanyMapper.class]: Unsatisfied dependency expressed through bean property 'sqlSessionFactory'; nested exception is org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'sqlSessionFactory' defined in class path resource [com/baomidou/mybatisplus/autoconfigure/MybatisPlusAutoConfiguration.class]: Bean instantiation via factory method failed; nested exception is org.springframework.beans.BeanInstantiationException: Failed to instantiate [org.apache.ibatis.session.SqlSessionFactory]: Factory method 'sqlSessionFactory' threw exception; nested exception is org.springframework.core.NestedIOException: Failed to parse mapping resource: 'file [D:\Project\auth-center\authcenter-server\target\classes\mapper\UserMapper.xml]'; nested exception is org.apache.ibatis.builder.BuilderException: Could not find value method on SQL annotation.  Cause: org.apache.ibatis.builder.BuilderException: Error creating document instance.  Cause: org.xml.sax.SAXParseException; lineNumber: 1; columnNumber: 523; 元素内容必须由格式正确的字符数据或标记组成。

~~~

问题的原因在于我们的代码中使用了`>`符号，这是一个特殊的字符，写在xml中需要进行转义。

![2021-06-16-15-37-47](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-06-16-15-37-47.png)

改正后的代码如下：

~~~ java

@Select("<script>" +
        " SELECT * FROM t_user where is_delete = 0" +
        " <if test='request.account != null'>" +
        "   AND account like #{request.account} " +
        " </if>" +
        " <if test='request.registStatus != null'>" +
        "   AND regist_status = #{request.registStatus} " +
        " </if>" +
        " <if test='request.name != null'>" +
        "   AND name like #{request.name} " +
        " </if>" +
        " <if test='request.status != null'>" +
        "   AND status = #{request.status} " +
        " </if>" +
        " <if test='request.startTime != null'>" +
        "   AND gmt_create_time &gt;= #{request.startTime} " +
        " </if>" +
        " <if test='request.endTime != null'>" +
        "   AND gmt_create_time &lt;= #{request.endTime} " +
        " </if>" +
        " order by gmt_create_time" +
        "</script>")
IPage<User> getUserListPage(Page<User> objectPage, SearchExperienceAccountRequest request);

~~~

这个问题我需要小心，原因是我之前在开发一款帮忙优化排版的脚本，我并没有注意到这个问题，其次我从业到现在，接触xml的次数太少了，踩过的坑也很少，我很容易犯相同的错误。

不过我对这种编码的方式也存在一些疑惑：

1. 这个查询可以通过LambdaQueryWrapper实现么

2. 这样的脚本是自己手动一行一行的编写的么，那我想测试该sql脚本的时候又该怎么办

我觉得使用这种方案，还是需要一些小工具的，让编写代码和检查代码都比较轻松，否则的话一点SQL出现了问题，我觉得没有谁有积极性去排查其中的问题。