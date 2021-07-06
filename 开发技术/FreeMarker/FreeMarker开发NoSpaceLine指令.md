我在开发模板代码时，我期待我的模板代码足够清晰，所以我指令和指令之间会空较多的行，用于代码排版，如下所示：

~~~ freemarker

<#list columnInfos as columnInfo>
    <@noSpaceLine>

    <#-- 忽略审计字段 -->
    <#if columnInfo.columnName == "org_id"
        || columnInfo.columnName == "creator"
        || columnInfo.columnName == "modifier"
        || columnInfo.columnName == "gmt_create_time"
        || columnInfo.columnName == "gmt_modify_time">
        <#continue>
    </#if>

    <#if columnInfo.enumInfo??>

    /**
     * ${columnInfo.columnComment}
     *
     * @see ${properties.enumsPackage}.${columnInfo.enumInfo.enumClass}#value
     */

    <#if columnInfo.columnName == "id">
    @NotBlank
    </#if>

    private ${columnInfo.enumInfo.enumValueType} ${columnInfo.beanObject};

    <#elseif columnInfo.internalClassInfo??>

    /**
     * ${columnInfo.columnComment}
     */
    private JSONObject ${columnInfo.beanObject};

    <#else>

    /**
     * ${columnInfo.columnComment}
     */
    <#if columnInfo.columnName == "id">
    @NotBlank
    </#if>

    private ${columnInfo.fieldType} ${columnInfo.beanObject};

    </#if>

    </@noSpaceLine>

</#list>

~~~

其实javadoc的注释、注解和代码之间我不希望有的空行。如果不自行开发指令的话，渲染出来的代码会有很大的空隙，非常影响阅读，我阅读过FreeMarker的文档，没有找到任何好用的指令，所以最后决定自行开发。

相关的代码体现在我的工具包中：

[NoSpaceLineDiretive.java](https://github.com/junjie2018/AutoTools/blob/feat/jj/flat_domein_info/src/main/java/fun/junjie/autotools/directives/NoSpaceLineDiretive.java
)

思路核心为：在指令中拿取指令体，然后渲染出指令体，处理掉渲染后的内容中的空白行，再调用env.getOut().write()写回处理后的内容。

## 参考资料

1. [Freemarker自定义指令和方法](https://blog.csdn.net/xiaoxufox/article/details/88839132)
   学会