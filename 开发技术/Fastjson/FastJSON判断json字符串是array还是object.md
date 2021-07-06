没有比较优雅的方案，代码如下：

~~~ java

Object jsonObj = JSON.parse(json);
if (jsonObj instanceof JSONObject) {
    return ((JSONObject) jsonObj).toJavaObject(type);
} else if (jsonObj instanceof JSONArray) {
    return ((JSONArray) jsonObj).toJavaList(type);
} else {
    throw new RuntimeException("json数据格式错误");
}

~~~

## 参考资料

1. [FastJSON判断JSON字符串是JSONObject或JSONArray](https://blog.csdn.net/wtopps/article/details/83900858)
2. [如何判断一个JSON字符串是普通JSON(JSONObject)还是数组JSON(JSONArray)](https://github.com/alibaba/fastjson/issues/1107)