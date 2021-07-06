是这样的，我参考之前的写法，在Request对象中用JSONObject去接受了前端请求传递了一个对象（我并不关心这个对象的结构）。结果接口有如下返回值：

~~~ json

{
    "code": 100101,
    "data": null,
    "message": "params invalidate: JSON mapping problem: com.sdstc.show.controller.external.DynamicFormController$PostDataRequest[\"extraData\"]->com.google.gson.JsonObject[\"asString\"]; nested exception is com.fasterxml.jackson.databind.JsonMappingException: JsonObject (through reference chain: com.sdstc.show.controller.external.DynamicFormController$PostDataRequest[\"extraData\"]->com.google.gson.JsonObject[\"asString\"])",
    "success": false
}

~~~

抱着试试看的心态，我去掉了@Data注解，该问题消失了。额，但是我是@Data的忠实用户，所以我最后用如下方式改写：

~~~ java

    @Data
    private static class PutDataRequest {
        private Map<?, ?> extraData;
    }

~~~

关于@Data，从细节上讲，我有很多想不明白的地方，当我们从Controller接受对象的时候，我们可以不要这个注解，接受没有任何问题；但是，当我们从RestTemplate接受的时候，如果没有这个注解，接受到的所有字段都为null。更奇怪的时，本次开发时，Controller和RestTemplate的HttpMessageConvert都是FastJsonHttpMessageConverter。

现在唯一能让我信服的理由就是，Controller可能用了字段名反射，而RestTemplate走的是Getter和Setter。

20210607后续：

其实这个问题的原因在于JsonObject和JSONObject根本不是同一个东西