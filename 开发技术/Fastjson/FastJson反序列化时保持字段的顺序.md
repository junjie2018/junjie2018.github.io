代码如下:

~~~ java

JSONObject obj = JSONObject.parseObject(jsonStr, Feature.OrderedField);

~~

整理这个笔记的时候发现了新的知识点。我之前一直再用`JSON.parsetObject()`方法，需要将返回结果再强转为`JSONObject`类型，很繁琐。其实上可以使用`JSONObject.parseObject()`方法。