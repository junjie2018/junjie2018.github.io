20210420后续：

实际上我有点不想用Python去开发一些小工具了，Python好久不摸，API忘得太快了，Ide又给不到什么有用的信息，在观望一下吧。

## 小小说明

这是我个人很久以前开发的一个辅助开发工具，最近有遇到类似的需求了，故记录下来。

事情的起因是这样的：我们业务上经常需要和第三方对接，第三方提供的往往是一个json文件。我们在编写代码时，可能会构建一个map，然后把各个字段填充进去，我并不是很喜欢这种方案，感觉并不是太优雅。我比较喜欢用@Builder的方案，这样代码段落会比较清晰，看上去也很优雅。

所以我就完成了这个辅助工具，这个工具可以解析一份json文件，然后输出类的定义代码。我在实现的时候，考虑了嵌套关系，所以生成的代码和实际情况是比较符合的，可以直接copy到项目中使用。

这儿需要说明的是，因为当时编写的时候，只考虑了服务于接口开发，代码中充斥了很多和我们应用场景细细相关的东西，比如我解析时，是从data层开始解析的；最后生成的对象，也是很多个小对象包含在一个大的Data对象中；同时，我这边只考虑了String的场景。如果你有需求，可以自行调整一下代码。

## 代码展示

代码如下：

~~~ python

import json

outer_class = []
inner_class = {}


class Field:
    def __init__(self, field_tame, field_type, is_list=False):
        self.fieldName = field_tame
        self.fieldType = field_type
        self.is_list = is_list


def outer_dispose(data_element):
    for key in data_element:
        if isinstance(data_element[key], list):
            # 暂时不考虑list嵌套的问题
            if len(data_element[key]) > 0:
                if isinstance(data_element[key][0], dict):
                    outer_class.append(Field(key, key, True))

                    # 处理内部类型
                    inner_class[key] = []
                    inner_dispose(key, data_element[key][0])
                else:
                    outer_class.append(Field(key, 'string', True))
            else:
                outer_class.append(Field(key, 'string', True))
        elif isinstance(data_element[key], dict):
            outer_class.append(Field(key, key))

            # 处理内部类型
            inner_class[key] = []
            inner_dispose(key, data_element[key])
        else:
            outer_class.append(Field(key, 'string'))


def inner_dispose(class_name, element):
    for key in element:
        if isinstance(element[key], list):
            if len(element[key]) > 0:
                if isinstance(element[key][0], dict):
                    inner_class[class_name].append(Field(key, key, True))

                    # 处理内部类型
                    inner_class[key] = []
                    inner_dispose(key, element[key][0])
                else:
                    inner_class[class_name].append(Field(key, 'string', True))
            else:
                inner_class[class_name].append(Field(key, 'string', True))
        elif isinstance(element[key], dict):
            inner_class[class_name].append(Field(key, key))

            # 处理内部类型
            inner_class[key] = []
            inner_dispose(key, element[key])
        else:
            inner_class[class_name].append(Field(key, 'string'))


def generateLine(prefix, field):
    result = ''
    if field.is_list:
        result += '%sprivate List<%s> %s;\n' % (prefix, field.fieldType.capitalize(), field.fieldName)
    else:
        result += '%sprivate %s %s;\n' % (prefix, field.fieldType.capitalize(), field.fieldName)
    return result


def generateOuterClass(outer_class, inner_class_content):
    result = ''
    result += '@Data\n'
    result += '@NoArgsConstructor\n'
    result += '@AllArgsConstructor\n'
    result += 'public class %s {\n' % 'Temp'

    for field in outer_class:
        result += generateLine('\t', field)

    result += '\n\n'
    result += inner_class_content

    result += '}\n'

    return result


def generateInnerClass(inner_class):
    result = ''
    for class_name in inner_class:
        result += '\t@Data\n'
        result += '\t@Builder\n'
        result += '\t@NoArgsConstructor\n'
        result += '\t@AllArgsConstructor\n'
        result += '\tpublic static class %s {\n' % class_name.capitalize()

        for field in inner_class[class_name]:
            result += generateLine('\t\t', field)
        result += '\t}\n\n'
    return result


if __name__ == '__main__':
    with open('input.json', encoding='utf8') as file:
        root_element = json.load(file)
        data_element = root_element['data']
        if isinstance(data_element, list):
            if len(data_element) > 0:
                outer_dispose(data_element[0])
        elif isinstance(data_element, dict):
            outer_dispose(data_element)
        else:
            raise Exception()

    inner_class_content = generateInnerClass(inner_class)
    print(generateOuterClass(outer_class, inner_class_content))

~~~

这份代码目前收录在[python]，如果你有兴趣，可以关注该项目最新的动态，或许我已经对这份代码进行了调整，且调整后更符合你的需求。

[python]:https://github.com/junjie2018/python