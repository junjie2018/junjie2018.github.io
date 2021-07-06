这是我自己开发的工具，从entity到typeHandler、到枚举代码如下：

~~~ java

/**
 * 是否是模型必须
 */
@TableField(typeHandler = IsModelRequiredTypeHandler.class)
private IsModelRequired modelRequired;
private Integer isModelRequired;

~~~

~~~ java

@Slf4j
@MappedTypes({Object.class})
@MappedJdbcTypes(JdbcType.VARCHAR)
public class IsModelRequiredTypeHandler extends AbstractEnumTypeHandler<IsModelRequired> {
    @Override
    protected IsModelRequired parseValue(String inputParam) {
        return IsModelRequired.convert(inputParam);
    }

    @Override
    protected String toValue(IsModelRequired isModelRequired) {
        return isModelRequired.getValue();
    }
}

~~~

~~~ java

/**
 * 是否是模型必须
 */
@AllArgsConstructor(access = AccessLevel.PROTECTED)
public enum IsModelRequired {


    /**
     * 否
     */
    NOT_MODEL_REQUIRED("0"),

    /**
     * 是
     */
    MODEL_REQUIRED("1"),

    ;

    @Getter
    private String value;

    public static IsModelRequired convert(String inputValue) {
        for (IsModelRequired enumItem : IsModelRequired.values()) {
            if (enumItem.getValue().equals(inputValue)) {
                return enumItem;
            }
        }
        throw new RuntimeException("Enum Transfer Wrong.");
    }
}

~~~

## 参考资料

1. [如何在MyBatis中优雅的使用枚举](https://segmentfault.com/a/1190000010755321)