因为我的脚本会生成SUMMARY.md和Reademe.md，所以我一致认为`gitbook init`操作是没有意义的（其实我至今仍然认为这个操作没有任何意义），所以我选择直接使用`gitbook build`指令，结果发现我的折叠等插件根本没有生效。这个问题对我来说非常严重，如果笔记不能够折叠，看上就会有点乱糟糟的。

我花费了一个晚上定位修复这个问题，最后定位到了`gitbook init`操作上。`gitbook init`操作做了什么，目前我认为它主要生成了SUMMARY.md和README.md文件，而之所以我没有运行`gitbook init`导致我插件失效的原因就是，我的脚本是在Windows上开发的，我的SUMMARY.md生成的时候使用的是Windows的分割符，而gitbook貌似不支持这种分割符。

以往之所以能够成功，我觉得这里面更多的是运气吧，可能我运气好执行了`gitbook init`，又或者我的SUMMARY.md文件和`gitbook init`生成的几乎一致，所以导致我一直没有意识到这个问题。