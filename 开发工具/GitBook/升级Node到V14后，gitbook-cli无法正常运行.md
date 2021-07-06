报错如下（这个报错不是我场景中的报错）：

~~~

Installing GitBook 3.2.3

/home/travis/.nvm/versions/node/v12.18.3/lib/node_modules/gitbook-cli/node_modules/npm/node_modules/graceful-fs/polyfills.js:287
      if (cb) cb.apply(this, arguments)
                 ^
TypeError: cb.apply is not a function
    at /home/travis/.nvm/versions/node/v12.18.3/lib/node_modules/gitbook-cli/node_modules/npm/node_modules/graceful-fs/polyfills.js:287:18
    at FSReqCallback.oncomplete (fs.js:169:5)

The command "gitbook install" failed and exited with 1 during .

~~~

我采用的方案：

~~~

cd /usr/lib/node_modules/gitbook-cli
npm i graceful-fs@4.1.4 --save
cd /usr/lib/node_modules/gitbook-cli/node_modules/npm
npm i graceful-fs@4.1.4 --save

~~~

我觉得node.js是我一生之敌，各种各样的问题太多了！！！

## 参考资料

1. [Gitbook build stopped to work in node 12.18.3](https://github.com/GitbookIO/gitbook-cli/issues/110)