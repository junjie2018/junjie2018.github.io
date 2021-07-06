我的代码如下：

~~~ java

@Data
@NoArgsConstructor
public class TableRoot {
    private String tblName;
    private String tblDesc;
    private List<ColumnRoot> columns;

    public TableRoot(String tblName, String tblDesc) {
        this.tblName = tblName;
        this.tblDesc = tblDesc;
        this.columns = new ArrayList<>();
    }

    @Data
    @SuppressWarnings("WeakerAccess")
    private static class ColumnRoot {
        private String colName;
        private String colDesc;
        @JSONField(deserializeUsing = JavaTypeCodec.class, serializeUsing = JavaTypeCodec.class)
        private JavaType javaType;
    }
}


~~~

序列化TableRoot对象时发现无法正常序列化，提示权限不足，需要将ColumnRoot改为public。我ColumnRoot其实没有暴露到外部的需求，但是因为snakeyaml的特性不得不暴露到外部，不是很爽。

我目前没有找到资料解决这个问题。

## 参考资料

1. [can not access a member of class with modifiers "public"](https://vaadin.com/forum/thread/1414726/can-not-access-a-member-of-class-with-modifiers-public)