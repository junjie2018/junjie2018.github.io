第三方镜像直接Run的话可能就没有办法进入到系统的终端，我用如下指令定义了我要执行的指令，从而覆盖Dockerfile中的CMD指令：

~~~

docker run -it python:3.8.6 bash

~~~

（我用该技术测试第三方image是否支持按住Git的软件）