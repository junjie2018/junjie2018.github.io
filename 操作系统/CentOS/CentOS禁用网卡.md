我使用的是`ip link set enp0s3 up`，效果非常好。另外`ifdown enp0s8`指令不好使，文档里也说了，ifdown不支持以enp打头的网卡，这个坑被我踩了。

`nmcli c down enp0s3`是我之后要尝试的方式，我比较喜欢nmcli系列的指令。

## 参考资料

1. [Linux 中如何启用和禁用网卡？](https://juejin.cn/post/6844903845546426382)