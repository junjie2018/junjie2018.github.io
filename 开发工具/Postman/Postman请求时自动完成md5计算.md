## 相关操作

1. 如图，点击Postman的Pre-request Script选项卡: 

![2020-10-09-15-14-51](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2020-10-09-15-14-51.png)

2. 填写如下代码：

~~~ javascript

var CryptoJS = require("crypto-js");

// select channel_app_id,sign_secret from mmp_brand_channel_app;
var channelIdToAppSecret = {
    "11111111":"11111111111111111111111111111111",
    "300000001":"300000001300000001300000001300000001",
    "300000002":"300000002300000002300000002300000002",
    "300000003":"300000003300000003300000003300000003",
    "300000004":"300000004300000004300000004300000004"
}

var body = JSON.parse(request.data)

var appId = body.appId
var appSecret = channelIdToAppSecret[appId]
var timestamp = Date.parse(new Date()) / 1000
var data = JSON.stringify(body.data)

var sign = CryptoJS.MD5(appId + appSecret + timestamp + data).toString().toUpperCase()

console.log(appId)
console.log(appSecret)
console.log(timestamp)
console.log(sign)
console.log(data)

postman.setEnvironmentVariable('timestamp', timestamp);
postman.setEnvironmentVariable('sign', sign);

~~~

3. 改写请求的body（请求参数和Header也支持双大括号语法）

~~~ json

{
    "brandId": 100000001,
    "channelId": 200000001,
    "appId": 300000001,
    "timestamp": "{{timestamp}}",
    "sign": "{{sign}}",
    "data":{"cardId":"1400000002","outTradeNo":"123555","appUserId":"zhenye"}
}

~~~

## 相关教程

1. [使用postman生成md5签名](https://www.jianshu.com/p/aa749d300f56)
2. [postman 发送MD5加密签名请求](https://www.cnblogs.com/liyujie1978/p/11172253.html)