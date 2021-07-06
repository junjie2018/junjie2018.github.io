配好PG数据源后，可以愉快的在mapper.xml文件中使用文本格式化快捷键了，但是我发现字段显示为红色，且提示该字段不存在。我猜想是PG的scheme导致的这个问题，我们的表不是放在默认的public scheme下，而是放在别的schema下。

正好我之前解决这个问题的时候发现了currentScheme参数，所以我尝试在URL上配置了这个参数，结果非常满意，红色提示消失了。

~~~

jdbc:postgresql://192.168.19.12:5432/dev?currentScheme=auth

~~~

![2021-06-17-16-18-38](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-06-17-16-18-38.png)