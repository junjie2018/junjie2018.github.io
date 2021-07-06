如果现在让我处理这个问题，我会选择使用自定义指令的方式（也不一定，自定义会让模板代码增加很多无关的标签），但是当时的话我自定义指令也并不是很熟悉，所以我选择了自定义Writer，让后将Writer传递给FreeMarker的渲染方法，代码如下：

[TemplateUtilsMax.java](https://github.com/junjie2018/AutoTools/blob/feat/jj/flat_domein_info/src/main/java/fun/junjie/autotools/utils/TemplateUtilsMax.java)

代码还处理调整期，未来还可能调整代码。