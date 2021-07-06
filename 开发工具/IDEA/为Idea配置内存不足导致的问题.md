## 问题描述

~~~

#
# There is insufficient memory for the Java Runtime Environment to continue.
# Native memory allocation (malloc) failed to allocate 949216 bytes for Chunk::new
# An error report file with more information is saved as:
# D:\Project\dyf\hs_err_pid20632.log
[thread 15576 also had an error]
#
# Compiler replay data is saved as:
# D:\Project\dyf\replay_pid20632.log
Disconnected from the target VM, address: '127.0.0.1:63994', transport: 'socket'

Process finished with exit code 1

~~~

Idea在执行程序的时候，报如上的错误，处理过程如下：

![2021-04-20-19-13-03](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-04-20-19-13-03.png)

这个方案实际上并没有帮到我，我每次设置后都会重启，重启后就不会出现该问题了。但是该设置仍然是700M。

20210615后续：

后来已经证明是我操作系统出现了问题，当内存超过60%时，就无法正常的分配内存。