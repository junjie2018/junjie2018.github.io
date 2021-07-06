判断是否是静态方法的代码如下：

~~~ java

Method method = 类.getMethod(相关参数);
int modifiers = getModifiers();
Modifier.isStatic(modifiers )

~~~

调用静态方法的代码如下：

~~~ java

public static Object invokeConvertMethod(String enumClazzName, Object methodParam) {
    if (!methodCache.containsKey(enumClazzName)) {
        throw new RuntimeException("Wrong");
    }

    try {
        Method method = methodCache.get(enumClazzName);
        // todo 临时，因为还没有决定枚举要不要分String和Integer
        return method.invoke(null, String.valueOf(methodParam));
    } catch (IllegalAccessException | InvocationTargetException e) {
        e.printStackTrace();
        return null;
    }
}

~~~

## 参考资料

1. [Ｊava判断是否是static方法](https://blog.csdn.net/helloworlddm/article/details/102940406)
2. [Java 反射调用静态方法](https://blog.csdn.net/zhangzeyuaaa/article/details/42522015)