我在尝试通过JdbcTeamplate获取数据库中列的元数据，有如下一段代码：

~~~ java

ResultSet columns = dbMetaData.getColumns(null, null, "t_dyf_%", null);

while(columns.next()){
    String tableName = columns.getString("COLUMN_NAME");
    System.out.println(tableName);
}

~~~

我并不知道columns.getString(columnTable)方法能够传递哪些参数，我查看了这个方法的源码，也没有说清楚。我懒得找文章，最后我通过断点的方式查看了该方法支持的所有参数：

![2021-05-20-09-16-36](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-05-20-09-16-36.png)

![2021-05-20-09-17-34](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-05-20-09-17-34.png)

这件事情中唯一让我感觉到意外的是，我断点查看columns的实现时，显示的是`HikariProxyResultSet`，但是在这个类中的断点不会执行到，需要在`PgResultSet`中进行断点。