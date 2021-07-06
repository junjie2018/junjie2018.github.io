我选择的是将自己创建的Actions和源码放在一起，因为这些Actions使用的复用性挺低的，放在一起避免了创建另一个仓库，非常舒服：

目录结构如下：

![2021-07-05-20-03-51](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-07-05-20-03-51.png)

其中Dockerfile文件如下：

~~~ dockerfile

FROM alpine:3.10

# Copies your code file from your action repository to the filesystem path `/` of the container
COPY entrypoint.sh /entrypoint.sh
RUN chmod +x entrypoint.sh

# Code file to execute when the docker container starts up (`entrypoint.sh`)
ENTRYPOINT ["/entrypoint.sh"]

~~~

action.yml文件如下：

~~~ yml

# action.yml
name: 'Hello World'
description: 'Greet someone and record the time'
inputs:
  who-to-greet:  # id of input
    description: 'Who to greet'
    required: true
    default: 'World'
outputs:
  time: # id of output
    description: 'The time we greeted you'
runs:
  using: 'docker'
  image: 'Dockerfile'
  args:
    - ${{ inputs.who-to-greet }}

~~~

entrypoint.sh如下：

~~~ shell

#!/bin/sh -l

echo "Hello $1"
time=$(date)
echo "::set-output name=time::$time"

~~~

Reademe.md没有太多营养，这儿就不呈现了。

github-actions-demo.yml如下：

~~~ yml

on: [push]

jobs:
  hello_world_job:
    runs-on: ubuntu-latest
    name: A job to say hello
    steps:
      - name: Checkout
        uses: actions/checkout@v2
      - name: Hello world action step
        id: hello
        uses: ./.github/actions/AdjustPicturesInMDFiles
        with:
          who-to-greet: 'Mona the Octocat'
      # Use the output from the `hello` step
      - name: Get the output time
        run: echo "The time was ${{ steps.hello.outputs.time }}"

~~~


## 参考资料

1. [创建 Docker 容器操作](https://docs.github.com/cn/actions/creating-actions/creating-a-docker-container-action)
   
   我参考了官方的案例，但是官方的案例中存在一点小小的错误，所以Dockerfile文件应该参考我的。