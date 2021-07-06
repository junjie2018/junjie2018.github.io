## 问题描述

因为我们客户端在上传数据时没有进行输入检查（服务端也没有检查），导致我们数据库t_material_info表中插入了一些错误数据。t_material_info表的extra_property列的类型json，可能会被插入如下的值：

~~~ json

{
    "":"",
    "field":"key"
}

~~~

现在的需求是修改t_material_info表的extra_property列，如果该列的json数据包含了key为空字符串的字段，则去掉该字段。

我开发的SQL如下：

~~~ sql

UPDATE t_material_info 
SET extra_property = jsonb ( extra_property ) - '' 
WHERE
  ID IN ( SELECT ID FROM t_material_info WHERE extra_property :: json ->> '' = '' )

~~~

因为考虑到该SQL需要在生产环境执行，所以做了一些调整（将原来的一条SQL拆成了两条），我们生产环境处理该问题的逻辑是，先查出所有需要修改的数据，然后update语句的where条件中指明需要修改的记录的id，这样可以最大限度防止SQL错误影响到线上的数据。当然我们可以结合一些工具，批量的生成这些update语句。