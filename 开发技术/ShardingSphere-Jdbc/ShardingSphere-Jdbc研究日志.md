ShardingSphere-Jdbc研究告一段落，目前总结的配置文件和增加的类已经基本能够满足我们的业务需求。而且我已经对我使用到的每个技术点都进行了相对充分的测试，确保了我的理解是正确的。我将对ShardingSphere-Jdbc的研究总结如下。

## 业务需求


我们的业务上只有分表需求，并没有分库需求，所以主要研究了在分表方面的使用。

在分表方面，我们的需求为：

1. 按照某个字段的模进行分表
2. 按照创建时间进行分表（这个地方需要注意，存在创建表的需求）

研究时编写的代码我已提交到：[shardingsphere-jdbc]，有兴趣的同学可以自行查看

[shardingsphere-jdbc]:https://github.com/junjie2018/shardingsphere-jdbc

## 发现或解决的问题

截止目前，发现的和解决的问题如下所列，并不是所有的问题都解决了，但是眼下的方案能让我们的以下在一定时间内正常运行：

1. 分页问题（使用临时方案解决）
2. 查询时不包括分表键的问题（使用临时方案解决）
3. 绑定表
4. 按时间分表及相关定时任务

### 分页问题

使用ShardingSphere-Jdbc时，有分页需求时，需要从多个数据源中拿数据，然后再拼接为最终的结果。很直观的，不可能从每个数据节点（数据节点：一个数据库 + 表名定义唯一一个数据节点）执行一次limit，然后将结果拼接再一次，选出目标范围内的数据。**ShardingSphere-Jdbc也的确不是这样做的，它做了一些SQL的改写**。

ShardingSphere-Jdbc如何改写：举个例子，如果我们要得到第3页的10个数据，原始limit的写法为limit 3, 10。ShardingSphere-Jdbc对其的改写为：limit 0, 33，它从每个数据节点拿到前33个数据，拼接后，再显示按照需求显示第3页的10个数据。

我们不必担心这个过程中内存会使用量会非常高，按照官方文档的说法，其底层采用了流式处理方案，并不是将各个数字节点的数据缓存到内存后再做分页。

不过，如果分页偏移量很大，则可能引发新的问题，那就是网络开销，按照ShardingSphere-Jdbc的实现，如果我们有三个数据节点，需求为要第一万页的数据，则其至少需要传输三万乘以每页大小个数据。

ShardingSphere-Jdbc官方文档中简单提到了一个解决方案，分别如下：

1. 构建行记录数量与行偏移量

这个方案我并没有查到相关的资料，也不知道如何实践。

2. 通过某个可排序的字段完成分页

代码演示如下：

~~~ sql
SELECT * FROM t_order WHERE id > 100000 AND id <= 100010 ORDER BY id
~~~

其实这个是没有办法在我们生产中实践的，因为我们生产中的id是由雪花算法产生的，并且和行不是一一对应的，所以100010 >= id > 100000在分页层面并没有任何含义。

我认为我们可以使用一个row字段，来模拟这种解决方案，但又会引入新的问题，那就是在微服务架构中，我们还需要确保row字段全局唯一而且还要递增，这本身就是一个技术难题（我们的项目中，目前没有积累解决这种问题的方案）。这个方案还存在一个问题，那就是应用面窄，一旦增加了排序的需求，row字段就没有任何意义的，又会回归到ShardingSphere默认的解决方法。

不过我们的项目实际上只采用了分表，没有采用分库，甚至整个微服务都是用同一个库（我们再尽量避免分布式事务方面的问题）。所以通过一些技巧，还是有办法拿到全局唯一的row值。

我们项目一直不愿意引入新的技术方案，怕增加研发成本，这限制了我们解决问题的思路。这个问题应该可以使用ES解决，但是没有机会进行探索这个方案。

### 查询时不包括分表键的问题

![2020-10-14-12-09-06](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2020-10-14-12-09-06.png)

我们分表策略为按照某个字段，进行模运算。但是我们实际业务中，不一定按照这个字段进行查询，这就导致了会出现按照非分表键进行查询的问题，按照文档所说，这个查询将会遍历全部的分表。

该如何解决了，我们采用的方案为：对这些非分表键加索引。这个方案看上去不是很优雅，但是可以在一定程度上环节这个问题。

还有一种高大上的解决方案，我们可以使用多个键进行分表，且算法需要保证单独使用各个键、使用全部这些键时能够定位到同一张表，关于这个算法如何实现能不能实现，其实我是不知道的。

或许业务精心设计后，这个问题也是可以避免的，但是在我们的项目中，这样做的成本会非常高。

### 绑定表

绑定表是指指分片规则一致的主表和子表。例如：t_order表和t_order_item表，均按照order_id分片，则此两张表互为绑定表关系。绑定表之间的多表关联查询不会出现笛卡尔积关联，关联查询效率将大大提升。

这儿挪用一下官方的案例，一个查询如下：

~~~
SELECT i.* FROM t_order o JOIN t_order_item i ON o.order_id=i.order_id WHERE o.order_id in (10, 11);
~~~

那么按照笛卡尔积，则应该改写SQL为：

~~~
SELECT i.* FROM t_order_0 o JOIN t_order_item_0 i ON o.order_id=i.order_id WHERE o.order_id in (10, 11);
SELECT i.* FROM t_order_0 o JOIN t_order_item_1 i ON o.order_id=i.order_id WHERE o.order_id in (10, 11);
SELECT i.* FROM t_order_1 o JOIN t_order_item_0 i ON o.order_id=i.order_id WHERE o.order_id in (10, 11);
SELECT i.* FROM t_order_1 o JOIN t_order_item_1 i ON o.order_id=i.order_id WHERE o.order_id in (10, 11);
~~~

配置了绑定表关系后，改写所得的SQL为：

~~~
SELECT i.* FROM t_order_0 o JOIN t_order_item_0 i ON o.order_id=i.order_id WHERE o.order_id in (10, 11);
SELECT i.* FROM t_order_1 o JOIN t_order_item_1 i ON o.order_id=i.order_id WHERE o.order_id in (10, 11);
~~~

在我们的业务中有相关的需求，所以我们需要使用到绑定表。

### 按时间分表及相关定时任务

我们一些记录表，是按照时间分片的，在ShardingSphere-Jdbc中，我们需要自行提供分片算法，思路清晰时，这个算法实现起来比较简单。

这儿说说我在实现时遇到的一个坑：因为我们的schedule项目需要操作分表，故项目引入了ShardingSphere-JDBC，同时我想在执行建表语句前，检查一下该表是否存在，故我使用了show tables like ''，解决发现返回的结果为空。后来经过实验，我发现，原生的数据源的的确确支持show语句，但是ShardingSphere-JDBC配置的数据源是不支持这个语句的。（官方文档也有谈到对各种语句的支持，我没有深入研究）

最后我的解决方案为，创建一张纯记录表，记录目前定时任务已经创建了哪些表，这样使用select语句就可以知道表是否创建了。

## 值得研究的方向

1. 与Liquibase的结合

我是比较感兴趣这个技术点的，但是，我对Liquibase的掌握，目前也仅仅限制与一些简单的Demo。

为什么我会对这两者的结合感兴趣了，ShardingSphere-Jdbc支持插入语句，我们可以从旧表中读取数据，然后再插入到新表中，完成数据的迁移。而完成这个工作的最好工具就是Liquibase。

2. 系统的使用这款工具

我们的业务需求真的只使用了这个工具的很小一部分功能，它还提供了很多非常强大的功能。我们对这款工具的使用是存在隐患的，如前文所述，有很多业务场景下的解决方案都不是很优雅，而且，很多生产中可能会遇到的问题，我们都没有进行假设，并准备应对方案（这本来应该是DBA干的，我们开发客串了）。

## 参考资料

1. [Apache ShardingSphere](https://shardingsphere.apache.org/index_zh.html)
2. [apache/shardingsphere](https://github.com/apache/shardingsphere/tree/master/examples)