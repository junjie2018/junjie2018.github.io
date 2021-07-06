最近服务的消费者提出了新的需求，要求我们的时间字段的格式都必须为时间戳，我原本以为是一个简单的问题，结果发现Fastjson在处理LocalDateTime时不是那么简单（我还没有思考过为什么使用LocalDateTime而不是Date等类）。找了一圈，没有很好的方案，所以我开发了如下的代码，我打算在下次遇到该需求的时候再重构代码：

~~~ java

    // todo 临时方案，以后需要优化
    @Data
    public static class DataPoDTO {

        private String formId;

        private String orgId;

        private Map<String, Object> extraData;

        protected String id;

        protected Integer delete = 0;

        protected LocalDateTime gmtModifyTime;

        protected LocalDateTime gmtCreateTime;

        protected String modifier;

        protected String creator;

        public Long getGmtModifyTime() {
            return gmtModifyTime.toEpochSecond(ZoneOffset.of("+8"));
        }

        public Long getGmtCreateTime() {
            return gmtCreateTime.toEpochSecond(ZoneOffset.of("+8"));
        }
    }

~~~

我们从数据库中查出来的数据，先COPY到DataPoDTO中，然后再进行序列化。

## 相关资料收集

整理一下这些资料，便于以后深入研究：

1. [从LocalDateTime序列化探讨全局一致性序列化](https://juejin.cn/post/6854573211528249357)
   
   这篇文章提到了序列化工具和反序列化工具全局一致性，但是作者的案例使用的是JackJson。

2. [FastJson输出时间类型强转为时间戳](https://blog.csdn.net/boom_man/article/details/87983511)
   
   提到了`fastJsonConfig.setSerializeFilters()`，对我当前的问题来说，这个解决方案太重了，但是如果是全局一致性的话，可以考虑使用这个方案。

3. [Java8 LocalDateTime获取时间戳（毫秒/秒）、LocalDateTime与String互转、Date与LocalDateTime互转](https://blog.csdn.net/u014044812/article/details/79231738)
   
   [localdatetime实现时间戳(相互转换)](https://blog.csdn.net/sayWhat_sayHello/article/details/80361486)
   
   包含了LocalDateTime时间戳转换的知识。