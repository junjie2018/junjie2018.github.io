我以为这个是一个比较简单的工作，像Linux中调用一下nohup就好了，但是没想到方案中有很多代码我都不太懂：

~~~ bat

@echo off 
if "%1" == "h" goto begin 
mshta vbscript:createobject("wscript.shell").run("%~nx0 h",0)(window.close)&&exit 
:begin
java -jar -Dspring.profiles.active=local gateway.jar > log.txt

~~~

## 参考教程

1. [bat脚本实现后台运行cmd命令](https://www.huaweicloud.com/articles/a19beb66651cdf0314aac575c006f210.html)
2. [批处理文件 bat 后台运行](https://blog.csdn.net/diyu122222/article/details/77871585)