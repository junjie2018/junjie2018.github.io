场景是这样的，我们local和dev共用一套token，之前的脚本中我将通过如下方法设置到了当前环境中，所以当我切换环境的时候我不得不重新运行一次登录脚本。

~~~ javascript

postman.setEnvironmentVariable('token', token)

~~~

因为我需要高频率的在local和dev环境间切换，所以我希望我的登录脚本能够将获取的token信息保存在全局变量中，然后在当前环境中通过一个变量中引用全局变量，从而实现一次登录应用不同的环境。

我登录脚本代码如下：

~~~ javascript

var token
var login_address

const loginRequestDev = {
    url: 'http://dev.4dshoetech.local/backend/authcenter/login/login',
    method: 'POST',
    header: 'Content-Type: application/json',
    body: {
        mode: 'raw',
        raw: JSON.stringify({"account":"junjie001@qq.com","pass":"Hello123","level":1,"inviteId":"","type":0})
    }
}

const loginRequestSit = {
    url: 'http://sit.4dshoetech.local/backend/authcenter/login/login',
    method: 'POST',
    header: 'Content-Type: application/json',
    body: {
        mode: 'raw',
        raw: JSON.stringify({"account":"502","pass":"123456","level":1,"inviteId":"","type":0})
    }
}



pm.sendRequest(loginRequestDev,function(err,res){
    
    if(err) {
        console.log(err)
    } else {
        
        var response = JSON.parse(res.text());

        token = response.data.token

        pm.globals.set("dev_token", token);
    
        console.log(response.data.token);
    }
});

pm.sendRequest(loginRequestSit,function(err,res){
    
    if(err) {
        console.log(err)
    } else {
        
        var response = JSON.parse(res.text());

        token = response.data.token

        pm.globals.set("sit_token", token);

        console.log(response.data.token);
    }
});


~~~

我global和local、dev、sit配置如下：

![2021-06-16-09-01-33](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-06-16-09-01-33.png)

![2021-06-16-09-01-50](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-06-16-09-01-50.png)

![2021-06-16-09-02-32](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-06-16-09-02-32.png)

在使用token时，只需要如下配置，就可以实现只运行一次登录脚本，不同环境中使用：

![2021-06-16-09-03-23](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-06-16-09-03-23.png)

## 参考资料

1. [Postman配置全局变量与环境变量详细教程](https://blog.csdn.net/Hello_World_QWP/article/details/87859427)
2. [postman 设置全局变量](https://zhuanlan.zhihu.com/p/60822607)