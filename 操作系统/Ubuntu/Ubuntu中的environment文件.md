最近按照教程配置了一份environment文件，在里面配置了代理，发现apt、curl都可以使用代理了，之前的方案是使用all_proxy，而且apt需要单独配置，先关注下这个问题。

执行后需要使用`source /etc/environment`生效