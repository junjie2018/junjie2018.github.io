## 问题描述

执行如下指令时，报如下错误：

~~~

npm install -g yo
npm WARN registry Unexpected warning for https://registry.npmjs.org/: Miscellaneous Warning UNABLE_TO_VERIFY_LEAF_SIGNATURE: request to https://registry.npmjs.org/yo failed, reason: unable to verify the first certificate
npm WARN registry Using stale data from https://registry.npmjs.org/ due to a request error during revalidation.
npm ERR! code UNABLE_TO_VERIFY_LEAF_SIGNATURE
npm ERR! errno UNABLE_TO_VERIFY_LEAF_SIGNATURE
npm ERR! request to https://registry.npmjs.org/generator-code failed, reason: unable to verify the first certificate

npm ERR! A complete log of this run can be found in:
npm ERR!     C:\Users\wujj\AppData\Roaming\npm-cache\_logs\2021-03-22T10_23_21_944Z-debug.log

~~~

## 解决方案

1. 执行如下指令，关闭strict-ssl：

~~~

npm config set strict-ssl false

~~~

2. 执行如下指令，完成安装任务：

~~~

npm install -g yo

~~~

3. 执行如下指令，开启strict-ssl：

~~~

npm config set strict-ssl true

~~~

## 参考教程

1. [npm install 出现UNABLE_TO_VERIFY_LEAF_SIGNATURE的解决办法](https://blog.csdn.net/az44yao/article/details/85111637)