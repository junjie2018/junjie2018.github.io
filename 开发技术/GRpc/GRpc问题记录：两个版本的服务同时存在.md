因为需要更换为新Service，所以导致项目中新旧同时存在，项目启动的时候没有任何问题，但是我用grpcui去连接的时候，报了如下的错误：

~~~

Failed to compute set of methods to expose: Symbol not found: com.sdstc.catalog.paas.dm.api.DataModel_12CatalogDMAdapterService

~~~

DataModel_12CatalogDMAdapterService是我开发的新服务，我的解决方法是将旧服务的代码全部注解掉。

暂时没有精力对这些问题深入研究，先记录下吧。