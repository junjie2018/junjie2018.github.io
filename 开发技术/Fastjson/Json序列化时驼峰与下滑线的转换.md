我开启了一个新的项目，尝试传递如下json，然后用如下对象接受，结果无法正常接受，这个是符合我预期的。

~~~

{
    "test_json":"testJson"
}

@Data
private static class TestJson {
    private String testJson;
}

@PostMapping("/testJson")
public TestJson testJson(@RequestBody TestJson testJson) {
    return testJson;
}

~~~

按照教程，做了如下配置后`spring.jackson.property-naming-strategy= CAMEL_CASE_TO_LOWER_CASE_WITH_UNDERSCORES`，我的请求和返回如下：

~~~

# 请求
{
    "test_json":"testJson"
}

# 返回
{
    "test_json":"testJson"
}

~~~

目前的表现和我们项目框架已经不一样了，我们项目框架中，即使不做任何配置，有如下请求和返回：

~~~

# 请求
{
    "test_json":"testJson"
}

# 返回
{
    "code": 200,
    "data": {
        "testJson": "testJson"
    },
    "message": "成功"
}

~~~

截止目前，我产生了一下需要探索的项：

1. 如何看项目做了哪些配置，比如`spring.jackson.property-naming-strategy= CAMEL_CASE_TO_LOWER_CASE_WITH_UNDERSCORES`
2. 是不是因为在项目中实验的时候，返回值在第二层，所以导致返回的testJson没有下滑线


## 解答一

开启SpringBoot Actuator，然后请求`localhost:8888/actuator/configprops`，返回结果中搜索jackson就可以看到相关的配置：

![2021-04-25-15-42-17](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-04-25-15-42-17.png)

从这份配置甚至可以猜到其可以对输入输出分别配置，从而达到我们项目框架的效果，但是看了其配置后发现它根本没有进行这项的配置：

![2021-04-25-15-45-38](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-04-25-15-45-38.png)

我觉得我们项目可能更多的是通过代码进行配置。现在就是不清楚通过JavaConfig进行的配置能够通过configprops反应出来么（已经证实：我们项目中使用的并不是jackson，所以看不到相关的配置。）。

经过向架构师的咨询，了解到我们项目的HttpMessageConvert使用的并不是Jackson，而是Fastjson，配置代码如下：

~~~ java

	@Bean
	public HttpMessageConverters fastJsonHttpMessageConverters() {
		return new HttpMessageConverters(getFastJsonHttpMessageConverter());
	}

	public HttpMessageConverter<?> getFastJsonHttpMessageConverter() {
		// 1.定义一个converters转换消息的对象
		FastJsonHttpMessageConverter fastConverter = new FastJsonHttpMessageConverter();

		List<MediaType> supportedMediaTypes = new ArrayList<>();
		supportedMediaTypes.add(MediaType.APPLICATION_JSON);
		supportedMediaTypes.add(MediaType.APPLICATION_ATOM_XML);
		supportedMediaTypes.add(MediaType.APPLICATION_FORM_URLENCODED);
		supportedMediaTypes.add(MediaType.APPLICATION_OCTET_STREAM);
		supportedMediaTypes.add(MediaType.APPLICATION_PDF);
		supportedMediaTypes.add(MediaType.APPLICATION_RSS_XML);
		supportedMediaTypes.add(MediaType.APPLICATION_XHTML_XML);
		supportedMediaTypes.add(MediaType.APPLICATION_XML);
		supportedMediaTypes.add(MediaType.IMAGE_GIF);
		supportedMediaTypes.add(MediaType.IMAGE_JPEG);
		supportedMediaTypes.add(MediaType.IMAGE_PNG);
		supportedMediaTypes.add(MediaType.TEXT_EVENT_STREAM);
		supportedMediaTypes.add(MediaType.TEXT_HTML);
		supportedMediaTypes.add(MediaType.TEXT_MARKDOWN);
		supportedMediaTypes.add(MediaType.TEXT_PLAIN);
		supportedMediaTypes.add(MediaType.TEXT_XML);
		fastConverter.setSupportedMediaTypes(supportedMediaTypes);

		// 2.添加fastjson的配置信息，比如: 是否需要格式化返回的json数据
		FastJsonConfig fastJsonConfig = new FastJsonConfig();
		// 修改配置返回内容的过滤
		// WriteNullListAsEmpty ：List字段如果为null,输出为[],而非null
		// WriteNullStringAsEmpty ： 字符类型字段如果为null,输出为"",而非null
		// DisableCircularReferenceDetect ：消除对同一对象循环引用的问题，默认为false（如果不配置有可能会进入死循环）
		// WriteNullBooleanAsFalse：Boolean字段如果为null,输出为false,而非null
		// WriteMapNullValue：是否输出值为null的字段,默认为false
		// PrettyFormat：结果是否格式化

		fastJsonConfig.setSerializerFeatures(SerializerFeature.DisableCircularReferenceDetect,
				SerializerFeature.WriteNullStringAsEmpty, SerializerFeature.WriteNullListAsEmpty,
				SerializerFeature.WriteNullBooleanAsFalse);

		// 配置全局long转String
		SerializeConfig serializeConfig = SerializeConfig.globalInstance;
		serializeConfig.put(Long.class, ToStringSerializer.instance);
		serializeConfig.put(Long.TYPE, ToStringSerializer.instance);
		fastJsonConfig.setSerializeConfig(serializeConfig);
		fastJsonConfig.setDateFormat("yyyy-MM-dd HH:mm:ss");
		// 3.在converter中添加配置信息
		fastConverter.setFastJsonConfig(fastJsonConfig);
		// 4.将converter赋值给HttpMessageConverter
		// HttpMessageConverter<?> converter = fastConverter;
		// 5.返回HttpMessageConverters对象
		return fastConverter;
	}

~~~

经过实验，我了解到FastJson是默认支持下滑线式的json key转成java中的驼峰，当需要序列化成json时，需要如下代码实现驼峰装下滑线（这段代码我暂时没有优化，等我需要的时候，我会优化下）：

~~~ java

    @Data
    @AllArgsConstructor
    private static class TestJson {
        private String testJson;
    }

    public static void main(String[] args) {
       
        // 下滑线转驼峰
        String jsonInput = "{\"test_json\":\"testJson\"}";

        TestJson testJson = JSON.parseObject(jsonInput, TestJson.class);


        // 驼峰转下划线
        TestJson testJson2 = new TestJson("testJson");


        SerializeConfig config = new SerializeConfig();
        config.propertyNamingStrategy = PropertyNamingStrategy.SnakeCase;

        String s = JSON.toJSONString(testJson2, config);
    }

~~~

## 解答二

包装后，请求与返回如下：

~~~

# 请求
{
    "test_json":"testJson"
}

# 返回
{
    "test_json": {
        "test_json": "testJson"
    }
}

~~~

其实从这些表现就可以发现`spring.jackson.property-naming-strategy= CAMEL_CASE_TO_LOWER_CASE_WITH_UNDERSCORES`配置的工作原理。它大概是请求时建立了test_json和testJson到testJson的映射，而返回的时候，它会将对象的所有字段都转换成下划线式的。


https://blog.csdn.net/java_cxrs/article/details/105850597
https://www.jianshu.com/p/cb02796dfbd2

## 参考资料

1. [springboot(15)修改HTTP默认序列化工具](https://blog.csdn.net/sz85850597/article/details/80412137)
2. [Java Json 数据下划线与驼峰格式进行相互转换](https://blog.csdn.net/wtopps/article/details/81087157)
3. [FastJson下划线转驼峰](https://blog.csdn.net/qq_41959009/article/details/103738291)
4. [springboot与web前端的下划线与驼峰的json转换配置](https://www.jianshu.com/p/cb02796dfbd2)
5. 