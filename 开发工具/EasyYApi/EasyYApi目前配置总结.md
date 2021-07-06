## Postman

我先从Postman整理起，虽然Postman相关的配置是我最后才实践的，但是确实提升了我不少编码幸福度。我比较喜欢使用Postman进行接口的测试，没有原因就是单纯的习惯了。我之前的方案是先用EasyYapi导出成一个文件，再在Postman中导入这个文件，步骤比较多，挺麻烦的。

新方案为：

1. 先生成一个Postman Token，然后配置到Idea中，这样我就可以直接通过EasyYapi将接口导入到我的Postman中了（Postman Token获取地址：https://go.postman.co/integrations/services/pm_pro_api）

![2021-06-28-16-03-14](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-06-28-16-03-14.png)

2. 配置postman.prerequest为如下内容，这样导出的每个接口在请求时都会自动加上token和Sdtc-Tenant-Id的Header，这样就不需要我手动填写了，非常舒服

~~~


pm.request.headers.remove("Sdtc-Tenant-Id")
pm.request.headers.add({key: "token",value: "{{token}}"})
pm.request.headers.add({key: "Sdtc-Tenant-Id",value: "12345678910"})

~~~

当然我Postman上还有一个登录用的脚本，可以同时请求dev和sit环境的登录接口，将token设置到环境变量中（具体实现可以在postman分类下寻找相关笔记）。

这套方案我目前还是有一些不太舒服的地方，EasyYapi每次导出到Postman时都是新建一个分类，而不是与旧的分类进行融合。这会导致我们Postman看上去非常的乱。

![2021-06-28-15-48-44](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-06-28-15-48-44.png)

实际上我是比较希望它们进行融合的，因为我希望完成所有的接口开发后，我能拥有一套带数据的Postman接口，用于日后的维护工作（我不喜欢@Mock等注解，会增加维护成本）

那么我目前是怎么处理这个问题的呢，如下，我手动维护了一份包含数据的测试接口，当我接口发生改变重新推到了Postman后，我手动的删除旧的Collection中的Request，将新的Request拖到我维护的这个Collection中。目前这个方案运行良好。

![2021-06-30-15-52-53](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-06-30-15-52-53.png)

## YApi

### markdown.render.server配置

上来的第一问题就是markdown.render.server配置，我发现官网默认提供的地址被DNS污染了（我经过测试得出了这个结论），所以我不得不自行搭建一个yapi-markdown-render，在搭建yapi-markdown-render中我主要遇到的问题是：使用官方的源码，无法启动项目，因为某个依赖出现了错误，我解决这个问题的方法Fork一份源码，然后修改package.json文件，将该依赖的版本适当的改小，然后项目就可以正常的启动了，非常舒服。

[我Fork的源码地址](https://github.com/junjie2018/yapi-markdown-render)

我后来又开发了自己的Dockerfile，以在Docker中启动yapi-markdown-render，相关的笔记我放在了Docker分类下的**利用Docker快速启动开发环境**中（文章标题及目录未来可能调整），我不打算在这儿赘述了。

具体到Idea项目中，我需要在`.easy.api.config`文件的配置如下：

~~~

markdown.render.server=http://192.168.13.68:3001/render

~~~

很尴尬，因为后来DNS又恢复了，我已经注释掉这行配置了。

### method.return.main配置

如下代码：

~~~ java

    /**
    * xxx接口
    *
    * @return 这是返回数据
    */
    @PostMapping("/test")
    public ResponseVo<String> test() {

        return ResponseVo.createSuccessByData("tmp");

    }

~~~

默认情况下，渲染出来的YApi文档如下：

![2021-06-30-16-08-42](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-06-30-16-08-42.png)

我们不知道返回的data类型是什么，含义是什么，当我们在`.easy.api.config`配置了如下代码后（具体配置要视各自项目情况而定）：

~~~

method.return.main[groovy:it.returnType().isExtend("com.sdstc.core.vo.ResponseVo")]=data

~~~

YApi渲染出来的页面如下：

![2021-06-30-16-10-56](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-06-30-16-10-56.png)

### param.ignore配置

~~~ java

    @PostMapping("/test")
    public ResponseVo<List<String>> createPdmProjMemberGroups(@RequestAttribute(APICons.REQUEST_USER_ID) String userId) {

        return ResponseVo.createSuccessByData("tmp");

    }

~~~

默认情况下，YAPI渲染出来时，会多一个userId的参数，这个会让人很迷惑，实际上userId是我们的网关通过解析Token，然后塞到了Header中，我们在UserIntercept中拿到这个值，然后塞到Attributes中，这儿不截图了。

所以我进行了如下的配置：

~~~

param.ignore=@org.springframework.web.bind.annotation.RequestAttribute

~~~

### postman.prerequest

这个配置是偷懒用的，我在postman分类下一篇笔记已经谈到过其用意，我目前的配置如下：

~~~

postman.prerequest=```
pm.request.headers.remove("Sdtc-Tenant-Id")
pm.request.headers.add({key: "token",value: "{{token}}"})
pm.request.headers.add({key: "Sdtc-Tenant-Id",value: "12345678910"})
```
~~~

### 备注与备注中分行

目标是这样的：

![2021-06-30-16-18-59](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-06-30-16-18-59.png)

代码中这么写：

~~~ java

    /**
     * 更新项目团队（包含团队成员信息）
     * <p>
     * groupInfo中如果包含projectGroupId，则将对该团队进行更新<br>
     * groupInfo中如果不包含projectGroupId，则创建该团队信息<br>
     * 对于属于当前项目的projectGroupId，而在编辑时没有传递的，一律删除<br>
     */
    @PostMapping("/projectGroup/updatePdmProjMemberGroup")
    @Transactional
    public ResponseVo updatePdmProjMemberGroup(
            @RequestHeader(APICons.HEADER_TENANT_ID) String tenantId,
            @RequestAttribute(APICons.REQUEST_USER_ID) String userId,
            @RequestBody @Valid UpdatePdmProjMemberGroupsRequest request) {

        pdmProjMemberGroupService.updatePdmProjMemberGroups(userId, tenantId, request);

        return ResponseVo.createSuccess();

    }

~~~

### json.rule.convert

目标是让Request和Response中的LocalDateTime类型，呈现在YApi上为Integer类型，具体的配置如下：

~~~

json.rule.convert[java.time.LocalDateTime]=java.lang.Integer

~~~

默认情况下是String类型，这个不符合我们的项目场景。