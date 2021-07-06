如下插件配置：

~~~ json

{
    "plugins": [
        "-search",
        "-sharing",
        "-livereload",
        "-font-settings",
        "lightbox",
        "expandable-chapters",
        "github",
        "splitter",
        "back-to-top-button",
        "code",
        "hide-element",
        "custom-favicon"
    ],
    "pluginsConfig": {
        "code": {
            "copyButtons": true
        },
        "github": {
            "url": "https://github.com/junjie2018"
        },
        "hide-element": {
            "elements": [".gitbook-link","chapter"]
        },
        "favicon":"favicon.ico"
    }
}

~~~

我在使用中发现，如果我不清除search、sharing、livereload插件，则hide-element插件会起到作用。但是一旦清除了它们，则该插件不会起到任何作用。

是为什么发生这个问题呢？其实如上的配置，其实会导致我页面报错，页面报错，可能会导致我hide-element的方法没有被执行到，从而无法影藏我想隐藏的内容。

![2021-07-03-17-05-15](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-07-03-17-05-15.png)

该如何处理这个问题呢？其实search有一个lunr的插件，这个插件应该是search的后端（具体实现不是太了解），如果我们需要删除search插件，则需要同时删除lunr插件，否则就会导致页面报错。

![2021-07-03-17-10-41](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-07-03-17-10-41.png)