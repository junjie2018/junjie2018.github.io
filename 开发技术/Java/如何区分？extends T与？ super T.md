## <? extends T>

<? extends T>表示类型的上界，也就是说，参数化的类型可能是T或者T的子类型。例如下面的写法都是合法的赋值语句：

~~~ java

List<? extends Number> list = new ArrayList<Number>();
List<? extends Number> list = new ArrayList<Integer>();
List<? extends Number> list = new ArrayList<Float>();

~~~

### 读数据分析

1. 不管给list如何赋值，可以保证list里面存放的一定是Number类型或其子类，因此可以从list列表里读取Number类型的值。
2. 不能从list中读取Integer，因为list里面可能存放的是Float值，同理，也不可以从list里面读取Float。

### 写数据分析

1. 不能向list中写Number，因为list中有可能存放的是Float
1. 不能向list中写Integer，因为list中有可能存放的是Float
2. 不能向list中写Float，因为list中有可能存放的是Integer

从上面的分析可以发现，只能从List<? extends T>读取T，因为无法确认它实际执行列表的类型，从而无法确定列表里面存放的实际的类型，所以无法向列表里面添加元素。

*（个人表示怀疑吧，如果只能读取，那完全不知道这个容器存在的意义）*

## <? super T>

<? super T>表示类型下届，也就是说，参数化的类型是此类型的超类型。

~~~ java

List<? super Float> list = new ArrayList<Float>();
List<? super Float> list = new ArrayList<Number>();
List<? super Float> list = new ArrayList<Object>();

~~~

<? super T>被设计为用来写数据的泛型（只能写入T或者T的子类类型），不能用来读，分析如下。

### 读数据分析

无法保证list里面一定存放的是Float类型或Number类型，因此有可能存放的是Object类型，唯一能确定的是list里面存放的是Object或其他子类，但是无法确定子类的类型。正是由于无法确认list里面存放数据的类型，因此无法从list里面读取数据。

### 写数据分析

1. 可以向list里面写入Float类型的数据（不管list里面实际存放的是Float、Number或Object，写入Float都是允许的）；同理，也可以向list里面添加Float子类类型的元素。
2. 不可以向list里面添加Number或Object类型的数据，因为list中可能存放的是Float类型的数据。

## 使用案例

代码如下：

~~~ java

public static <T> void copy(List<? super T> dest, List<? extends T> src) {
    for (int i = 0; i < src.size(); i++) {
        dest.set(i, src.get(i));
    }
}

~~~