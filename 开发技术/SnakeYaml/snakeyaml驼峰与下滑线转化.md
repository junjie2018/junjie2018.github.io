我的需求是这样的，我需要序列化一个对象成yaml文档，序列化时要求驼峰转下划线，且保持字段声明顺序。我原本计划通过自定义snakeyaml的Representer实现这个需求，但是发现并不是很好这么搞（我没有充分研究，我觉得投入和产出不成比例），所以我决定绕一绕解决问题。

我最终选择的方案是，将对象通过fastjson转换成一个json字符串，转换的过程中保持字段声明顺序，且驼峰转下滑线。然后再用fastjson将json字符串转换成一个LinkedHashMap，再使用snakeyaml的dump方法导出该map。具体代码如下：

~~~ java

/**
 * 将对象序列化成Yaml文件，序列化时保持字段顺序与声明一致
 */
public static void dumpObject(TableRoot sourceObj, Path dirPath, String fileName) {

    try {

        if (!Files.exists(dirPath)) {
            Files.createDirectory(dirPath);
        }

        // 将原始的对象序列化成json（字段按照原对象的声明顺序）
        JSON.DEFAULT_GENERATE_FEATURE &= ~SerializerFeature.SortField.getMask();
        SerializeConfig serializeConfig = new SerializeConfig(true);
        serializeConfig.propertyNamingStrategy = PropertyNamingStrategy.SnakeCase;
        String jsonWithFieldOrder = JSON.toJSONString(sourceObj, serializeConfig);

        // 将jsonWithFieldOrder转换成Map，并输出到yaml文件
        FileWriter fileWriter = new FileWriter(Paths.get(dirPath.toString(), fileName).toString());
        Yaml yaml = new Yaml();
        fileWriter.write(yaml.dumpAsMap(JSON.parseObject(jsonWithFieldOrder, LinkedHashMap.class, Feature.OrderedField)));
        fileWriter.close();
    } catch (IOException e) {
        throw new RuntimeException("Dump Wrong...");
    }
}

~~~

这种解决问题的方案，成本非常的高，不过我用于工具的开发，问题倒不是很大。反序列话相对比较简单，代码如下：

~~~ java

/**
 * 将多个Yaml文件反序列化成对象列表
 */
public static List<TableRoot> loadObject() {

    File outputDir = new File(Paths.get(ToolsConfig.TABLES_INFO_DIR, ProjectConfig.getProjectName()).toString());
    if (!outputDir.isDirectory()) {
        throw new RuntimeException("Load Wrong...");
    }

    File[] yamlFiles = outputDir.listFiles();
    if (yamlFiles == null) {
        throw new RuntimeException("Load Wrong...");
    }

    try {
        List<TableRoot> result = new ArrayList<>();

        for (File file : yamlFiles) {
            String jsonMiddle = JSON.toJSONString(new Yaml().load(new FileReader(file)));
            TableRoot tableRoot = JSONObject.parseObject(jsonMiddle, TableRoot.class);
            result.add(tableRoot);
        }

        return result;
    } catch (FileNotFoundException e) {
        throw new RuntimeException("Load Wrong...");
    }
}

~~~

## 参考资料

1. [【需求】支持按照成员变量声明顺序，做序列化字段排序](https://github.com/alibaba/fastjson/issues/3115)

2. [Keep tags order using SnakeYAML](https://stackoverflow.com/questions/31534014/keep-tags-order-using-snakeyaml)
   
   针对该需求，这篇教程中似乎提到了更高级的解决方案，但是我目前没有使用该方案的需求。

3. [JAVA使用SnakeYAML解析与序列化YAML](https://cloud.tencent.com/developer/article/1585284)
   
   这篇文章里有些高级的知识需要理解下。
