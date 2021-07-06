快速使用的指令如下：

~~~

nodeppt new slide.md

nodeppt serve slide.md

nodeppt build slide.md

~~~

有意思的是，使用了nodeppt serve指令后，可以实时的编辑并渲染ppt。

Demo代码如下：

~~~ markdown

title: Demo
speaker: JJ
plugins:
    - echarts

<slide class="bg-black-blue aligncenter" image="https://source.unsplash.com/C1HhAQrbykQ/ .dark">

# Demo {.text-landing.text-shadow}

By JJ {.text-intro}

[:fa-github: Github](https://github.com/ksky521/nodeppt){.button.ghost}

<slide class="bg-black-blue" image="https://source.unsplash.com/C1HhAQrbykQ/ .dark">

第一页

<slide class="bg-black-blue" image="https://source.unsplash.com/C1HhAQrbykQ/ .dark">

第二页

~~~

## 参考资料

1. [nodeppt 2.0](https://github.com/ksky521/nodePPT)