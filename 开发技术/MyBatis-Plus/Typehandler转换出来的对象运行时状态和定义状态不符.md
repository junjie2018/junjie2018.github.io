我在开发新功能时发现如下的问题，我代码编写如下：

![2021-05-18-09-09-37](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-05-18-09-09-37.png)

在运行时候value字段的值是：

![2021-05-18-09-10-23](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-05-18-09-10-23.png)

是一个LinkedTreeMap，而不是我期待的Language对象。

我意识到肯定是JsonbTypeHandler影响了我最终的结果，但是，当我将JsonbTypeHandler换成ObjectTypeHandler时，依旧无法达到我想要的效果。

我还未开始系统研究MyBatis-Plus，这块的问题只能先放置。我最终选择的是通过FastjsonTypeHandler先绕开这个问题（FastJsonTypeHandler也不能很好的符合我的需求）。

20210520后续：

我后来开发的处理该问题的方案

~~~ java

@Data
@Builder
@NoArgsConstructor
@AllArgsConstructor
@EqualsAndHashCode(callSuper = false)
@TableName(value = "t_dyf_option", autoResultMap = true)
public class OptionPo extends BasePo {

    private static final long serialVersionUID = 1L;

    private String orgId;

    private String fieldCode;

    private Integer sort;

    @TableField(typeHandler = LanguageTypeHandler.class)
    private List<Language> value;

    @Data
    @NoArgsConstructor
    @AllArgsConstructor
    public static class Language {
        private String languageCode;
        private String languageValue;
    }

    @Slf4j
    @MappedTypes({Object.class})
    @MappedJdbcTypes(JdbcType.VARCHAR)
    public static class LanguageTypeHandler extends AbstractJsonTypeHandler<Object> {

        private final Class<?> type = Language.class;

        @Override
        protected Object parse(String json) {

            JSON.parseObject("", type);

            Object jsonObj = JSON.parse(json);
            if (jsonObj instanceof JSONObject) {
                return ((JSONObject) jsonObj).toJavaObject(type);
            } else if (jsonObj instanceof JSONArray) {
                return ((JSONArray) jsonObj).toJavaList(type);
            } else {
                throw new RuntimeException("数据库数据格式错误");
            }
        }

        @Override
        protected String toJson(Object obj) {
            return JSON.toJSONString(obj,
                    SerializerFeature.WriteMapNullValue,
                    SerializerFeature.WriteNullListAsEmpty,
                    SerializerFeature.WriteNullStringAsEmpty);
        }
    }
}

~~~

考虑到后面需要开发自动化工具，所以我将Handler提取到了框架中，新代码如下：

JsonTypeHandler.java

~~~ java

public class JsonTypeHandler extends AbstractJsonTypeHandler<Object> {

    protected Class<?> type = Object.class;

    @Override
    protected Object parse(String json) {

        JSON.parseObject("", type);

        Object jsonObj = JSON.parse(json);
        if (jsonObj instanceof JSONObject) {
            return ((JSONObject) jsonObj).toJavaObject(type);
        } else if (jsonObj instanceof JSONArray) {
            return ((JSONArray) jsonObj).toJavaList(type);
        } else {
            throw new RuntimeException("json数据格式错误");
        }

    }

    @Override
    protected String toJson(Object obj) {
        return JSON.toJSONString(obj,
                SerializerFeature.WriteMapNullValue,
                SerializerFeature.WriteNullListAsEmpty,
                SerializerFeature.WriteNullStringAsEmpty);
    }
}


~~~

OptionPo.java

~~~ java

package com.sdstc.dyf.admin.core.po;

import com.sdstc.dyf.admin.core.handler.JsonTypeHandler;
import com.sdstc.scdp.mybatis.plus.handler.JsonbTypeHandler;
import com.baomidou.mybatisplus.annotation.TableName;

import java.util.List;

import com.sdstc.scdp.mybatis.plus.po.BasePo;
import com.baomidou.mybatisplus.annotation.TableField;

import lombok.*;
import lombok.extern.slf4j.Slf4j;
import org.apache.ibatis.type.JdbcType;
import org.apache.ibatis.type.MappedJdbcTypes;
import org.apache.ibatis.type.MappedTypes;

/**
 * <p>
 *
 * </p>
 *
 * @author wujj
 * @since 2021-05-17
 */
@Data
@Builder
@NoArgsConstructor
@AllArgsConstructor
@EqualsAndHashCode(callSuper = false)
@TableName(value = "t_dyf_option", autoResultMap = true)
public class OptionPo extends BasePo {

    private static final long serialVersionUID = 1L;

    private String orgId;
    
    private String fieldCode;

    private Integer sort;

    @TableField(typeHandler = LanguageTypeHandler.class)
    private List<Language> value;

    @Data
    @NoArgsConstructor
    @AllArgsConstructor
    public static class Language {
        private String languageCode;
        private String languageValue;
    }

    @Slf4j
    @MappedTypes({Object.class})
    @MappedJdbcTypes(JdbcType.VARCHAR)
    public static class LanguageTypeHandler extends JsonTypeHandler {

        public LanguageTypeHandler() {
            this.type = Language.class;
        }

    }
}


~~~

目前的实现上肯定不是非常的优雅，比如用FastJson解析的时候，一定需要先判断是Array还是Object，但是能达到我们想要的效果。

自动化工具遇到类型为jsonb的字段时，会自动生成相应的对象和Handler。

## 参考资料

1. [MyBatis通过TypeHandler自动编解码对象的Json属性](https://my.oschina.net/KevinBlandy/blog/4500819)
2. [关于mybatis中typeHandler的两个案例](https://blog.csdn.net/u012702547/article/details/54572679)

3. [【Mybatis】用TypeHandler将数据库中存储的json字符串处理为对象，包括对象含List以及复杂对象的情况, 并满足泛型可转成多种对象](https://blog.csdn.net/SeptDays/article/details/96357404)
   
   没有用到该资料，先记录在这块。