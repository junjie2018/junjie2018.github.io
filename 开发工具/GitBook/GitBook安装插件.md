参考我的配置文件：

~~~ json

{
    "plugins": [
        "-search",
        "-lunr",
        "-sharing",
        "-livereload",
        "-font-settings",
        "lightbox",
        "expandable-chapters",
        "chapter-fold",
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
            "elements": [".gitbook-link"]
        },
        "favicon":"favicon.ico"
    }
}

~~~


然后执行如下指令：

~~~

gitbook install .

~~~

## 配置说明

-search：去除默认的搜索插件
-sharing：去除默认的分享插件
-livereload：去除git serve热更新插件
-font-settings：去除页面上的A按钮插件

lightbox：单击查看图片
expandable-chapters：
hide-element：隐藏页面一些元素用的，目前和其他插件有一些冲突

custom-favicon：修改标题栏的图标（我的图标是在https://favicon.io/生成的，还不错）


## 参考资料

1. [GitBook插件整理](https://www.jianshu.com/p/427b8bb066e6)