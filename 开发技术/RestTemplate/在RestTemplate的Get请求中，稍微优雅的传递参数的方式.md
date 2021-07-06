自从用了RestTemplate，我已经很少在Java代码中使用HttpClient之类的东西了。RestTemplate的便利性，能够帮助我快速的开发一些小工具。

我接下来需要研究的RestTemplate的技术是：看能不能脱离SpringBoot项目使用RestTemplate。现阶段使用这个工具时我还需要初始化一个SpringBoot项目，有点麻烦。

RestTemplate的Get请求，优雅的传递参数的方法如下：

~~~ java

UriComponents apiInterfaceListUrl = UriComponentsBuilder.fromUriString(YAPI)
        .path(YApi.API_INTERFACE_LIST.getPath())
        .queryParam("token", TOKEN)
        .queryParam("limit", RESPONSE_ITEM_LIMIT)
        .build();

ApiInterfaceListResponse apiInterfaceListResponse = restTemplate.getForObject(
        apiInterfaceListUrl.toUriString(),
        ApiInterfaceListResponse.class);

~~~

queryParam方法是允许传递一个`Map<String, String>`类型的参数的，但是我没有找到一个很好的工具将一个对象转换成`Map<String, String>`，自己开发这个工具，我暂时又没有足够的动力，所以暂时先直接传递字符串了。

## 参考教程

1. [使用RestTemplate发送get请求,获取不到参数的问题](https://blog.csdn.net/zzzgd_666/article/details/82791180)