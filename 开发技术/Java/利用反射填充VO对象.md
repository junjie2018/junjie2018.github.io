应用场景是这样的。我秉承的信念是，入库时每个字段都是需要被精细控制，所以我往往需要写一大堆setXXX方法，该工具就可以我快速生成这些代码：

~~~ java

    public static void main(String[] args) {

        for (Field field : FormPo.class.getDeclaredFields()) {
            System.out.println(String.format(".%s(tmp.get%s())",
                    field.getName(),
                    StringUtils.capitalize(field.getName())));
        }

    }

~~~

有时间我会继续完善的。

20210617后续：

最近用Builder比较多，有时间改一下这个。