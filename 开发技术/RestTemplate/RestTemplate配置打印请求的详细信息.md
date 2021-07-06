代码如下：

RestTemplateConfig类：

~~~ java

@Configuration
public class RestTemplateConfig {

    @Bean
    public RestTemplate restTemplate(ClientHttpRequestFactory httpRequestFactory) {

        RestTemplate restTemplate = new RestTemplate(httpRequestFactory);

       // 设置拦截器，答应请求信息，方便Debug
       List<ClientHttpRequestInterceptor> interceptors = new ArrayList<>();

       interceptors.add(new LoggingClientHttpRequestInterceptor());

       restTemplate.setInterceptors(interceptors);

       //提供对传出/传入流的缓冲,可以让响应body多次读取(如果不配置,拦截器读取了Response流,再响应数据时会返回body=null)
       restTemplate.setRequestFactory(new BufferingClientHttpRequestFactory(httpRequestFactory));

        return restTemplate;
    }

    @Bean
    public ClientHttpRequestFactory simpleClientHttpRequestFactory() {
        SimpleClientHttpRequestFactory factory = new SimpleClientHttpRequestFactory();
        factory.setConnectTimeout(15000);
        factory.setReadTimeout(5000);
        return factory;
    }
}

~~~

LoggingClientHttpRequestInterceptor 类：

~~~ java

@Slf4j
public class LoggingClientHttpRequestInterceptor implements ClientHttpRequestInterceptor {


    @Override
    public ClientHttpResponse intercept(HttpRequest request, byte[] body, ClientHttpRequestExecution execution) throws IOException {
        traceRequest(request, body);
        ClientHttpResponse response = execution.execute(request, body);
        traceResponse(response);
        return response;
    }

    private void traceRequest(HttpRequest request, byte[] body) {
        log.info("=========================== request begin ===========================");
        log.info("uri : {}", request.getURI());
        log.info("method : {}", request.getMethod());
        log.info("headers : {}", request.getHeaders());
        log.info("request body : {}", new String(body, StandardCharsets.UTF_8));
        log.info("============================ request end ============================");
    }

    private void traceResponse(ClientHttpResponse httpResponse) throws IOException {
        StringBuilder inputStringBuilder = new StringBuilder();
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(httpResponse.getBody(), StandardCharsets.UTF_8));
        String line = bufferedReader.readLine();
        while (line != null) {
            inputStringBuilder.append(line);
            inputStringBuilder.append('\n');
            line = bufferedReader.readLine();
        }
        log.info("============================ response begin ============================");
        log.info("Status code  : {}", httpResponse.getStatusCode());
        log.info("Status text  : {}", httpResponse.getStatusText());
        log.info("Headers      : {}", httpResponse.getHeaders());
        log.info("Response body: {}", inputStringBuilder.toString());
        log.info("============================= response end =============================");
    }
}

~~~

这个功能不是手到擒来的（如果你按照上面的配置去搞，就是傻瓜版开启了这个功能），我开启这个功能时遇到了哪些问题？我找到教程前，已经有了自己的一份关于RestTemplate的配置，我不想对我的配置进行太大的改动，所以我只复制了我觉得重要的配置，即没有复制该行：restTemplate.setRequestFactory(new BufferingClientHttpRequestFactory(httpRequestFactory));

结果就导致了，日志信息明明看到了返回体有相关的信息，但是得到的对象总是为空。我仔细阅读了教程中的代码，发现漏了上面的一行。实际上我对底层的实现还是有点不太理解，目前先记录下来，把这些知识积累起来，这种知识积累多了后，可能就可以学习到更多的知识。

## 参考资料

1. [Spring RestTemplate配置拦截器打印请求URL和响应结果](https://blog.csdn.net/zhuzicc/article/details/105946400)
2. [Spring RestTemplate配置拦截器打印请求URL和响应结果](https://www.codenong.com/cs105946400/)