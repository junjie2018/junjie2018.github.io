如下代码：

![2021-06-16-16-48-15](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-06-16-16-48-15.png)

对应生成的YApi文档为：

![2021-06-16-16-48-44](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-06-16-16-48-44.png)

这其实不是我想要的，因为我们的@RequestAttribute会从Header中获取一个值赋给该字段，而这个Header来源于网关对token的解析。

参考了官方的文档，似乎没有零侵入的方案，所以我采用了自己实现注解的方案，我开发的方案如下：

~~~ java

package com.sdstc.authcenter.annotations;

import java.lang.annotation.*;

/**
 * 用于EasyYAPI隐藏@RequestAttribute
 */
@Documented
@Retention(RetentionPolicy.SOURCE)
@Target(ElementType.PARAMETER)
public @interface EasyYAPIIgnoreParam {
    boolean hide() default true;
}

~~~

easyyapi的配置如下：

~~~

param.ignore=@com.sdstc.authcenter.annotations.EasyYAPIIgnoreParam#hide

~~~

代码中的写法如下：

~~~ java

@PostMapping("/createAuthAppRole")
public ResponseVo<String> createAuthAppRole(
        @RequestHeader(APICons.HEADER_TENANT_ID) String tenantId,
        @EasyYAPIIgnoreParam @RequestAttribute(APICons.REQUEST_USER_ID) String userId,
        @RequestBody @Valid CreateAuthAppRoleRequest request) {
    return ResponseVo.createSuccessByData(authAppRoleService.createAuthAppRole(userId, tenantId, request));
}

~~~

目前YApi的表现和期待的一样，我将持续关注这个问题。

## 方案优化

当然，这件事到此并没有结束，在我后来的研究中我发现我们的项目时注解了`@RequestAttribute(APICons.REQUEST_USER_ID)`的参数需要被忽略，所以我完全可以针对这个注解进行EasyYapi的配置，所以我删除了我开发的注解，并进行了如下的配置。

~~~

param.ignore=@org.springframework.web.bind.annotation.RequestAttribute

~~~

代码中的写法如下：

~~~ java

@PostMapping("/createAuthAppRole")
public ResponseVo<String> createAuthAppRole(
        @RequestHeader(APICons.HEADER_TENANT_ID) String tenantId,
        @EasyYAPIIgnoreParam @RequestAttribute(APICons.REQUEST_USER_ID) String userId,
        @RequestBody @Valid CreateAuthAppRoleRequest request) {
    return ResponseVo.createSuccessByData(authAppRoleService.createAuthAppRole(userId, tenantId, request));
}

~~~

目前的这个方案我还是挺满意的，哈哈。

## 参考资料

1. [EasyYapi官方文档：param_ignore](https://easyyapi.com/setting/rules/param_ignore.html)
2. [java在注解中绑定方法参数的解决方案](https://blog.csdn.net/yingxiake/article/details/51152444)