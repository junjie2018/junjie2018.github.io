1. 依赖配置如下：

~~~

<dependency>
    <groupId>p6spy</groupId>
    <artifactId>p6spy</artifactId>
    <version>3.9.1</version>
</dependency>

~~~

2. application.yml配置如下

~~~

spring:
  datasource:
#    driver-class-name: org.postgresql.Driver                                                             # 原始的
    driver-class-name: com.p6spy.engine.spy.P6SpyDriver                                                   # 使用p6spy后的
    password: HelloWorld
#    url: jdbc:postgresql://192.168.13.68:5432/postgres?currentSchema=public&stringtype=unspecified       # 原始的
    url: jdbc:p6spy:postgresql://192.168.13.68:5432/postgres?currentSchema=public&stringtype=unspecified  # 使用p6spy后的
    username: postgres

~~~

3. sps.properties配置

~~~

module.log=com.p6spy.engine.logging.P6LogFactory,com.p6spy.engine.outage.P6OutageFactory
# 自定义日志打印
logMessageFormat=fun.junjie.autotools.config.P6spyLogFormat
# 使用日志系统记录sql
appender=com.p6spy.engine.spy.appender.Slf4JLogger
## 配置记录Log例外
excludecategories=info,debug,result,batc,resultset
# 设置使用p6spy driver来做代理
deregisterdrivers=true
# 日期格式
dateformat=yyyy-MM-dd HH:mm:ss
# 实际驱动
#driverlist=com.mysql.jdbc.Driver
#drive=com.p6spy.engine.spy.P6SpyDriver
driverlist=com.p6spy.engine.spy.P6SpyDriver
# 是否开启慢SQL记录
outagedetection=true
# 慢SQL记录标准 秒
outagedetectioninterval=2

~~~

4. P6spyLogFormat开发

~~~

package fun.junjie.autotools.config;

import com.p6spy.engine.spy.appender.MessageFormattingStrategy;
import io.micrometer.core.instrument.util.StringUtils;

public class P6spyLogFormat implements MessageFormattingStrategy {

    @Override
    public String formatMessage(final int connectionId, final String now, final long elapsed, final String category, final String prepared, final String sql, final String url) {

        return StringUtils.isNotEmpty(sql) ? new StringBuilder().append(" Execute SQL：>>>>").append(sql.replaceAll("[\\s]+", " ")).append(String.format(" <<<< Execute time: %s ms", elapsed)).toString() : null;
    }
}


~~~

## 小结

我配置p6spy是为了打印我通过JdbcTemplate获取表元数据时执行的SQL，但是，我发现这个工具根本不好使。我最后采用的方案是自己搭建一个PG数据库，然后开启SQL日志。

我没有深入研究p6spy，毕竟它目前不满足我的需求。

## 参考资料

1. [使用P6Spy监控你的Spring boot数据库操作](https://my.oschina.net/hutaishi/blog/3020251)
2. [springboot 配置 P6spy](https://zhuanlan.zhihu.com/p/256243688)