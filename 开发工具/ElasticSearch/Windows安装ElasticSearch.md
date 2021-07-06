Windows安装ElasticSearch，用于学习环境

1. 下载二进制文件，解压后进入bin目录

2. 使用管理员身份运行`elasticsearch.bat`文件

3. 浏览器访问localhost:9200，看到如下内容，则成功：

~~~

{
  "name" : "WUJJ",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "5XBz9P4ZQXy-CBIqzZbCSw",
  "version" : {
    "number" : "7.12.1",
    "build_flavor" : "default",
    "build_type" : "zip",
    "build_hash" : "3186837139b9c6b6d23c3200870651f10d3343b7",
    "build_date" : "2021-04-20T20:56:39.040728659Z",
    "build_snapshot" : false,
    "lucene_version" : "8.8.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}

~~~