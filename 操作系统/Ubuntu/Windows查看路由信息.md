## 操作步骤

1. 指令如下

~~~
route print
route print | findstr "172"

route delete 172.17.0.0 

route add 172.17.0.0 MASK 255.255.0.0  172.17.30.1
route add 172.17.0.0 MASK 255.255.0.0  10.8.0.1

route delete 172.17.0.0 
route add 172.17.0.0 MASK 255.255.0.0  172.17.0.1
~~~