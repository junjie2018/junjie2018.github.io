我觉得我遇到的这个问题属于PyCharm的BUG！！！通过oss2推送图片到阿里云OSS时，因为网络环境的问题，我需要设置代理，我在网上找到了一篇教程，是通过环境变量的方式，我果断的设置环境变量后尝试该方案，结果失败了。我以为是这个方案不适用Win环境，于是我在Linux环境测试，发现该方案可行。

我本人不信邪，我觉得从原理上讲，应该两个平台的表现是一致的，我有在Win平台寻找解决放案，后来发现了通过os.environ设置all_proxy的方式设置代理，是可行的。我意识到，可能是我设置了环境变量后，PyCharm没有立即获取到这些环境变量，于是，我设置了环境变量后，重启PyCharm，发现程序按照我的想法运行了。

我明白造成我问题的原因了，就是我设置了环境变量后，在PyCharm中启动程序时，没有读取到最新的环境变量。我觉得这个就是PyCharm的BUG。

## 其他讯息

其实这次解决问题，本来没有不会花费这么长时间的，我之前有做python中获取环境变量的实验，那个时候获取的始终是none，我没有想到是Pycharm的问题，而是以为我api调用错误，就搁置了这个问题，结果最后就导致这次解决问题花了很长时间。

这次的问题，还有一些奇怪的现象。我Win机器上开启了代理，oss2的请求会阻塞很长时间，然后报proxy错误，我不知道这个请求最终请求到哪了，为什么会出现这种现象。我觉得我应该学习一些相关的技术，这样下次遇到类似的问题时，我至少知道我的数据包去到哪了。

## 参考资料

1. [需要在没有外网但可以使用代理的服务器上，上传文件到oss，该如何设置代理](https://github.com/aliyun/aliyun-oss-python-sdk/issues/136)
2. [Request高级用法](https://docs.python-requests.org/zh_CN/latest/user/advanced.html#proxies)
3. [python 使用代理的几种方式](https://blog.csdn.net/whatday/article/details/112169945)
4. [python 获取环境变量](https://blog.csdn.net/zwt0909/article/details/53035861)

## 后续

这个问题其实还没有结束，解决了环境变量的问题后，还可能存在成功一次上传，部分成功，部分报SSL错误的情况或者Post成功Get失败。真的是离奇到爆炸。但是这次我开始耐心定位问题，我先通过如下指令，判断我1080端口是否正在监听：

~~~

# 要么用双引号，要么不用，单引号没有任何效果
netstat -aon|findstr 1080

~~~

当1080端口没有监听的时候，执行测试代码，发现报如下错误，这个错误是我预先知道的，是符合我们公司的网络环境的：

~~~

(Caused by SSLError(SSLCertVerificationError(1, '[SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed: unable to get local issuer certificate (_ssl.c:1125)')))

~~~

配置代码使用本地代理后，再次执行脚本，报如下错误：

~~~

(Caused by SSLError(SSLEOFError(8, 'EOF occurred in violation of protocol (_ssl.c:1125)')))

~~~

很明显，这是两种不同类型的SSL错误，我之前一直以为是一种，导致我以为是Request中某些玄学的因素导致无法正常请求。所以，也在尝试用一些玄学的办法解决这个问题~

目前已经尝试的解决方案：

1. Linux配置透明代理（失败）
2. ALL_PROXY走socks5协议（失败）
3. ALL_PROXY设置为其他机器（失败）
4. 在VPS环境测试，免使用代理进行测试（失败）
5. 为Bucket类传递Session参数（失败）

我现在真的觉得这种问题好哔狗，我清掉了OSS里的所有图片，重新运行脚本，这次运行的过程中因为我操作失误忘记清除max_retries=40的设置，结果整个脚本又跑成功了？？？但是这次运行花了很长一段时间，成功运行了脚本后，我又立即运行了一次，结果这次又出现了之前的错误！！！而且我还发现了一些怪异的现象，我如下代码断点，真正断点的时候，resp并没有获取到任何东西（如截图所示），我决定不在继续运行代码，在此等候代码执行，结果过了一会，resp的值又出现了。要命，难道这就是Python的异步？？？

~~~

        # Create the Request.
        req = Request(
            method=method.upper(),
            url=url,
            headers=headers,
            files=files,
            data=data or {},
            json=json,
            params=params or {},
            auth=auth,
            cookies=cookies,
            hooks=hooks,
        )
        prep = self.prepare_request(req)

        proxies = proxies or {}

        settings = self.merge_environment_settings(
            prep.url, proxies, stream, verify, cert
        )

        # Send the request.
        send_kwargs = {
            'timeout': timeout,
            'allow_redirects': allow_redirects,
        }
        send_kwargs.update(settings)
        resp = self.send(prep, **send_kwargs)

        return resp

~~~

![2021-04-23-20-46-48](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-04-23-20-46-48.png)

![2021-04-23-20-49-38](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-04-23-20-49-38.png)

我想锤人！！！我猜测是不是网络的问题，我的bucket在北京，我的vps在香港，而我在广州，请求需要从我这到香港，然后从香港到北京，这个过程太慢了，最后导致失败。我将bucket重建在深圳，结果脚本非常正确的运行了。真的哔狗啊，这种解决问题的方法已经超出我对事物的理解范围了，为什么post就可以无视请求时长的影响，而get就会受到印象呢？oss2又如何设置这个时长呢？我暂时不打算对这些东西深入研究了，知道有这些问题存在就好了。

## 后续20210427

没想到这件事情还有后续！！！我今天要使用脚本的时候准备做一些脚本清理工作，结果发现之前用深圳的bucket做实验的时候和用北京节点的bucket做实验的时候没有控制变量，北京用的是`https://oss-cn-beijing.aliyuncs.com`，而深圳用的是`oss-cn-shenzhen.aliyuncs.com`，区别是北京的多了一个https。当我深圳的endpoint也加上https，会报和北京bucket一样的错误（区别在于可能可以多运行一会，遇到该问题的几率小一点）！！！，而当我的北京endpoint去到这个https后，也不会报任何错误，而且速度还很快。

我这段代码是用的我很久前的代码，我以为是我当时写错了，我后来去网上搜索，发现很多代码都是加上https的，我之前写这段代码时也是参考的官方的文档，我认为大概率是官方文档调整了写法。

我觉得这是oss2的bug或者是oss2底层使用的库的bug，这个问题最困扰人的一点是加不加https，不是绝对的，加了不一定失败，所以定位这个问题时完全靠观察能力。这个问题深入底层的分析，需要掌握抓包、过滤包、解析https包的技术，我暂时没有掌握这些技术，所以先放弃了。

还有些东西需要记录下来，当抛出异常的时候，我貌似在鲨鱼上看到RST包了，不知道两者有没有关系。

## 个人小结

这次实验让我开始注意到网络环境对实验的影响了，我之前对这些东西无感，我觉得都是毫秒级的差距，应该感觉不出来，但是这次很明显的看到从香港vps上传东西到北京bucket，需要上十分钟，而到深圳，只需要一分钟不到。同时，网络请求的跳数太多，可能会对实验结果代理不可预知的问题。

## 其他知识

我发现其实我Shadowsocks客户端一打开，即使我调整为其为禁用，我1080端口也处于监听状态。额，我之前一直以为该端口会关闭，尴尬。

## 参考资料

1. [How to get around python requests SSL and proxy error?](http://5.9.10.113/65849454/how-to-get-around-python-requests-ssl-and-proxy-error)
   
   报错的位置和我很像，连代码的行数都很像，但是没有提供给我任何有用的讯息。