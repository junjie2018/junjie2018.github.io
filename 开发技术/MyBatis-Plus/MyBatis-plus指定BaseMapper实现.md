这个问题我暂时只能描述一下，因为这个问题发生的时候，我询问了同事，结果这是我们架构中已知的问题，所以很快就解决了，我并没有查找任何资料，也没有做任何分析。

该问题大致是这样的，我在开发时使用了MyBatis-Plus中从BaseMapper中继承而来的方法selectById（其他方法也有这个问题），该方法在本地运行的时候非常的正常，能够正确的检查出我想要的数据。但是，当把项目部署到dev环境的时候，该方法会报`no statement`的错误（具体错误细节我忘记了，大概是这个意思）。

同事告诉我，dev环境的配置文件需要进行如下配置：

~~~

mybatis-plus:
  global-config:
    super-mapper-class: com.baomidou.mybatisplus.core.mapper.BaseMapper

~~~

就是将BaseMapper指向我们自己实现的一个BaseMapper。我大概知道是怎么回事了，dev环境默认执行的BaseMapper的实现中并没有该方法的实现，所以在运行的时候会报`no statement`错误。