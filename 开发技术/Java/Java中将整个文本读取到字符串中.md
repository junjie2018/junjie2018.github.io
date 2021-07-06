python中有个readLines的api，这个api可以一次性的将整个文件的文本读取到一个字符串中，我理所当然的认为java中也有类似的api，结果找了一圈没有找到相关的资料，最后我用如下的方式实现了相同的效果：

~~~ java

    File tplFile = tplFilePath.toFile();

    try (FileInputStream fis = new FileInputStream(tplFile)) {
        byte[] tplBytes = new byte[(int) tplFile.length()];
        //noinspection ResultOfMethodCallIgnored
        fis.read(tplBytes);
        String tplContent = new String(tplBytes);
        tplContentsMap.put(tplFile.getName(), StringUtils.strip(tplContent.trim()));
        loadFragmentContents(tplFile.getName(), tplContent);

    } catch (IOException e) {
        e.printStackTrace();
    }

~~~

这种方式并不是很直观，我以后看看有没有相应的工具包。

## 参考资料

1. [java 读取文件——按照行取出（使用BufferedReader和一次将数据保存到内存两种实现方式）](https://www.cnblogs.com/0201zcr/p/5009975.html)