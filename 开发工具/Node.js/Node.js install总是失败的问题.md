## 问题描述

~~~ 

C:\Users\wujj\Desktop\ElectronProject>npm install electron --save
(node:14808) Warning: Setting the NODE_TLS_REJECT_UNAUTHORIZED environment variable to '0' makes TLS connections and HTTPS requests insecure by disabling certificate verification.
(Use `node --trace-warnings ...` to show where the warning was created)

> core-js@3.9.1 postinstall C:\Users\wujj\Desktop\ElectronProject\node_modules\core-js
> node -e "try{require('./postinstall')}catch(e){}"


> electron@12.0.2 postinstall C:\Users\wujj\Desktop\ElectronProject\node_modules\electron
> node install.js

(node:27104) Warning: Setting the NODE_TLS_REJECT_UNAUTHORIZED environment variable to '0' makes TLS connections and HTTPS requests insecure by disabling certificate verification.
(Use `node --trace-warnings ...` to show where the warning was created)
RequestError: read ECONNRESET
    at ClientRequest.<anonymous> (C:\Users\wujj\Desktop\ElectronProject\node_modules\got\source\request-as-event-emitter.js:178:14)
    at Object.onceWrapper (events.js:422:26)
    at ClientRequest.emit (events.js:327:22)
    at ClientRequest.origin.emit (C:\Users\wujj\Desktop\ElectronProject\node_modules\@szmarczak\http-timer\source\index.js:37:11)
    at TLSSocket.socketErrorListener (_http_client.js:469:9)
    at TLSSocket.emit (events.js:315:20)
    at emitErrorNT (internal/streams/destroy.js:106:8)
    at emitErrorCloseNT (internal/streams/destroy.js:74:3)
    at processTicksAndRejections (internal/process/task_queues.js:80:21)
npm WARN bookmarker@1.0.0 No repository field.

npm ERR! code ELIFECYCLE
npm ERR! errno 1
npm ERR! electron@12.0.2 postinstall: `node install.js`
npm ERR! Exit status 1
npm ERR!
npm ERR! Failed at the electron@12.0.2 postinstall script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.

npm ERR! A complete log of this run can be found in:
npm ERR!     C:\Users\wujj\AppData\Roaming\npm-cache\_logs\2021-03-29T10_29_25_848Z-debug.log

~~~

试了很多种方法，改源、修改全局tsl，最后只有这个办法成功了，执行指令前，执行如下代码（我在Win 10系统）：

~~~

set NODE_TLS_REJECT_UNAUTHORIZED=0

~~~

## 参考文档

1. [npm install时报unable to verify the first certificate 证书无效的错误](https://juejin.cn/post/6844903950072676365)