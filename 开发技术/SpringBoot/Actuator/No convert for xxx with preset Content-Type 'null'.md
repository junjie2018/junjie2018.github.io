今天配置好Actuator后，访问其接口时报如下错误：

~~~

{
    "code": 100101,
    "data": null,
    "message": "params invalidate: No converter for [class org.springframework.boot.actuate.beans.BeansEndpoint$ApplicationBeans] with preset Content-Type 'null'",
    "success": false
}

~~~

我一步一步知道我们的框架层，发现如下代码：

~~~ java

    @Override
    public void configureMessageConverters(List<HttpMessageConverter<?>> converters) {
        Iterator<HttpMessageConverter<?>> iterator = converters.iterator();
        while(iterator.hasNext()){
            HttpMessageConverter<?> converter = iterator.next();
            if(StringUtils.containsIgnoreCase(converter.getClass().getName(),"jackson2")){
                iterator.remove();
            }
        }
        FastJsonHttpMessageConverter fastJsonHttpMessageConverter = new FastJsonHttpMessageConverter();

        //自定义fastjson配置
        FastJsonConfig config = new FastJsonConfig();
        config.setSerializerFeatures(FastJsonUtil.serializerFeatures);

        fastJsonHttpMessageConverter.setFastJsonConfig(config);
        // 添加支持的MediaTypes;不添加时默认为*/*,也就是默认支持全部
        // 但是MappingJackson2HttpMessageConverter里面支持的MediaTypes为application/json
        // 参考它的做法, fastjson也只添加application/json的MediaType
        List<MediaType> fastMediaTypes = new ArrayList<>();
        fastMediaTypes.add(MediaType.APPLICATION_JSON_UTF8);
//        fastMediaTypes.add(MediaType.ALL); 这行代码为我边写
        fastJsonHttpMessageConverter.setSupportedMediaTypes(fastMediaTypes);
        converters.add(fastJsonHttpMessageConverter);

    }

~~~

我发现我们的`messageConverters`仅支持`MediaType.APPLICATION_JSON_UTF8`，我尝试将其改为`fastMediaTypes.add(MediaType.ALL)`，发现接口能正确的返回数据。

我简单的查看了一下源码，发现实现Actuator接口的实现并不是通过Controller，而是@EndPoint和@ReadOperation注解，我猜想可能在一个切面中会得到@ReadOperation的返回值，然后在HttpMessageConverts中寻找一个可以处理Content-Type为null的转换器进行处理，结果没有找到，结果就报错了。

针对这个案例，我不打算做框架层面任何修改，因为我们Maven管理方面走的是OverWrite，我修改了底层框架，很大概率会影响到线上的代码，有点担心会带来不好的影响。

## 参考资料

1. [springBoot或者springCloud 的集成 Prometheus监控-数据无法解析](https://segmentfault.com/a/1190000038413776)