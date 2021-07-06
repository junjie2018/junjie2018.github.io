`StringUtils.trim()`仅去除控制字符，且字符编码需要小于32
`StringUtils.strip()`可以去除\t\r\n等
`StringUtils.deleteWhitespace`将删除所有的空白

另外trim和strip还有xxxToNull和xxxToEmpty方法，如果字符串全部都为控制字符，则将得到Null或者空字符串。

trim和strip的区别在于能否去除全角和半角的空格字符。

## 参考资料

1. [StringUtils 去除空白](https://blog.csdn.net/qq_39964694/article/details/80332785)
2. [Java: trim()方法和strip()方法之间的区别](https://blog.csdn.net/weixin_42386551/article/details/109329861)