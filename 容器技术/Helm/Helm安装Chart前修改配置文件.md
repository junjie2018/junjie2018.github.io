简单指令如下：

~~~ shell

# 展示可配置项
helm show values bitnami/wordpress

# 使用指定文件覆盖默认配置
echo '{mariadb.auth.database: user0db, mariadb.auth.username: user0}' > values.yaml
helm install -f values.yaml bitnami/wordpress --generate-name

# 查看指定release中--set设置的值
helm get values <release-name>

# 清除--set中设置的值
helm upgrade --reset-values

~~~

## 两种方式传递配置数据

- --values(或-f)：使用YAML文件覆盖配置。可以指定多次，优先使用最右边的文件。
- --set：通过命令行的方式对指定项进行覆盖。

如果同时使用两种方式，则--set中的值会被合并到--values中，但是--set中的值优先级更高。

## --set的格式和限制

--set选项使用0或多个name/value对。最简单的用法类似于：--set name=value，等价于如下YAML格式：

~~~ yaml

name: value

~~~

多个值使用逗号分割，因此--set a=b,c=d的YAML表示是：

~~~ yaml

a: b
c: d

~~~

支持更复杂的表达式。例如，--set outer.inner=value 被转换成了：

~~~ yaml

outer:
  inner: value

~~~

列表使用花括号（{}）来表示。例如，--set name={a, b, c} 被转换成了：

~~~ yaml

name:
  - a
  - b
  - c

~~~

可以使用数组下标的语法来访问列表中的元素。例如 --set servers[0].port=80 就变成了：

~~~ yaml

servers:
  - port: 80

~~~

多个值也可以通过这种方式来设置。--set servers[0].port=80,servers[0].host=example 变成了：

~~~ yaml

servers:
  - port: 80
    host: example

~~~

如果需要在 --set 中使用特殊字符，你可以使用反斜线来进行转义；--set name=value1\,value2 就变成了：

~~~ yaml

name: "value1,value2"

~~~

类似的，你也可以转义点序列（英文句号）。这可能会在 chart 使用 toYaml 函数来解析 annotations，labels，和 node selectors 时派上用场。--set nodeSelector."kubernetes\.io/role"=master 语法就变成了：

~~~ yaml

nodeSelector:
  kubernetes.io/role: master

~~~

深层嵌套的数据结构可能会很难用--set表达。我们希望Chart的设计者们在设计values.yaml文件的格式时，考虑到--set的使用。

## 参考资料

1. [安装前自定义 chart](https://helm.sh/zh/docs/intro/using_helm/)