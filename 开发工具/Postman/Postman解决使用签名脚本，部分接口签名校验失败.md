## 问题描述

1. 完成了一个postman的脚本（可以在该分类下找到该脚本源码），用于在请求时自动计算签名，使用该脚本时发现绝大多数请求的签名校验成功，部分签名校验失败
2. 将这个请求的data部分替换掉，则签名校验成功（我们签名验证部分是需要将data计算在内的）

## 解决流程

1. postman打印所所有用于签名的字段，用java中的方法进行md5计算，计算结果与postman计算结果一致
2. 网关断点调试，发现计算出来的签名与postman计算出来的请求不一致
3. 截取网关请求，截取postman请求，截取计算签名时用的data的请求，发现data与postman处不一致

## 原因分析

1. postman脚本中拿data计算签名时，用到了JSON.parse(request.data)，JSON.parse()反序列化时存在精度丢失的问题
2. 而我们的请求中，memberId的字段太长了，且是数字类型，就刚好撞到这个问题上

## 解决方案

1. 将memberId字段换成String，或者截断一点（我采用的方案）
2. 不使用JSON.parse()，使用JsonPath直接拿数据（Postman貌似不支持引入JsonPath模块）
3. 不使用JSON.parse()，使用正则表达式提取data部分（不是很想搞，太麻烦了，哈哈）