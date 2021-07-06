今天第一次接触GRpc，之前有做游戏开发时用的ProtoBuf，对这个东西印象很好，GRpc的底层貌似就是ProtoBuf。

大概说下我做了哪些操作吧：

1. 为类加上@GRpcService注解，这个注解是我们自己开发的，我还没有去了解这些注解的细节。

2. 启动应用程序，在日志里搜索port，可以看到GRpcService监听的是9090端口

3. 下载[grpcui](https://github.com/fullstorydev/grpcui)，解压后配置一些环境变量，然后打开cmd执行grpcui -help

4. 执行如下指令，将打开一个新的Web页面，该页面就是用来测试GRpc的工具。

~~~

grpcui -plaintext localhost:9090

~~~

GRpc服务的代码如何编写？我刚开始在这个方面犯错了，错误表现为在grpcui点击了invoke按钮后，invoke按钮一直是灰色的。后来按照同事提供的方式编写了如下代码：

~~~ java

    @Override
    public void create(com.sdstc.catalog.paas.dm.api.EnterModel.CreateRequest request,
                       io.grpc.stub.StreamObserver<com.sdstc.catalog.paas.dm.api.EnterModel.CreateResponse> responseObserver) {

        System.out.println("This is Create");

        printObject(request);
        responseObserver.onNext(EnterModel.CreateResponse.newBuilder()
            .setCode(200)
            .setRelateId("1111")
            .build());
        responseObserver.onCompleted();
    }

~~~

我犯错的原因是，我没有执行`responseObserver.onCompleted()`。

我们的GRpc用在什么场景中？我们有一个目录服务，我们公司所有上传的项目最终都会将内容提交到这个项目中。之前我们的方案中，用户提交的内容先提交到各自服务，然后再调用目录服务的接口，将这些数据提交到目录服务中（是这样么，我不确定之前是不是这样实现的）。

这样做有什么坏处呢？我们服务间通信使用的是Restful风格的接口，Http请求的时候是有可能失败的，我们是需要进行重试处理的。我们虽然有自己的重试服务，重试工作会比较简单，但是我们每个服务依旧需要提供Client去处理重试逻辑，这会增加每个开发人员的工作量。而且重试只是最终保证了我们的数据时正确的，当数据驻留在消息服务器上的时候，可能会给我们的项目Debug时带来一些困扰。

还有什么问题呢？目录服务是一个中心服务，向其提交数据有两种方式，暴露接口，让中心服务自己拉取；调用中心服务的接口，将数据主动推送过去。很多时候，我们的服务是需要同时支持这两种方式的，这无形中增加了各个服务的开发量。而且由各个服务决定数据的形式，目录服务本身也会进行一些开发，进行适配。

总之，在我看来，之前的这种方案，隐含了很多的开发任务，和系统运行的不稳定因素。

我们新方案的思路是怎样的呢？前端直接将数据提交到目录服务，目录服务完成处理后，调各个服务的接口将用户的数据通知到各个服务。然后服务拿到这些数据后，再按照设计进行入库、或更新。有时候目录中心需要全量的服务数据，这个也是通过GRpc的服务实现的。

我对这套设计的理解就这些了，还在探索中。