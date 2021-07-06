代码如下：

~~~ js

var token

const loginRequest = {
    url: login_url,
    method: 'POST',
    header: 'Content-Type: application/json',
    body: {
        mode: 'raw',
        raw: JSON.stringify({"account":"junjie001@qq.com","pass":"Hello123","level":1,"inviteId":"","type":0})
    }
}

pm.sendRequest(loginRequest,function(err,res){
    
    if(err) {
        console.log(err)
    } else {
        
        var response = JSON.parse(res.text());

        token = response.data.token

        postman.setEnvironmentVariable('token', token)

        console.log(response.data.token);

    }
});


~~~

## 参考教程

1. [Postman脚本中发送请求](https://blog.csdn.net/baidu_37476464/article/details/106917355)
2. [js字符串转换为对象格式](https://blog.csdn.net/weixin_41646716/article/details/80939466)
3. [Postman：脚本应用_预请求脚本](https://www.huaweicloud.com/articles/eadfbf8e4d626f4efbcba3f47c0db141.html)