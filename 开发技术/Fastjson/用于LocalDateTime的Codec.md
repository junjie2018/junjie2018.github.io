代码如下：

~~~ java

package com.sdstc.pdm.common.codec;

import com.alibaba.fastjson.parser.DefaultJSONParser;
import com.alibaba.fastjson.parser.deserializer.ObjectDeserializer;
import com.alibaba.fastjson.serializer.JSONSerializer;
import com.alibaba.fastjson.serializer.ObjectSerializer;

import java.io.IOException;
import java.lang.reflect.Type;
import java.time.LocalDateTime;
import java.time.ZoneOffset;

public class LocalDateTimeCodec implements ObjectSerializer, ObjectDeserializer {
    @Override
    public <T> T deserialze(DefaultJSONParser parser, Type type, Object fieldName) {
        if (type != LocalDateTime.class) {
            throw new RuntimeException("Wrong Type");
        }

        Long timestamp = Long.valueOf((String) parser.parse());

        //noinspection unchecked
        return (T) LocalDateTime.ofEpochSecond(
                timestamp / 1000, 0, ZoneOffset.ofHours(8));
    }

    @Override
    public int getFastMatchToken() {
        return 0;
    }

    @Override
    public void write(JSONSerializer serializer, Object object, Object fieldName, Type fieldType, int features) throws IOException {
        if (!(object instanceof LocalDateTime)) {
            throw new RuntimeException("Wrong Type");
        }

        LocalDateTime time = (LocalDateTime) object;

        serializer.write(time.toInstant(ZoneOffset.of("+8")).toEpochMilli());
    }
}


~~~

目前还是临时方案，我会持续迭代这个Codec。

## 参考资料

1. [localdatetime实现时间戳(相互转换)](https://blog.csdn.net/sayWhat_sayHello/article/details/80361486)