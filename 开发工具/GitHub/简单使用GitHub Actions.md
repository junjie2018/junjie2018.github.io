如下为实验目录及文件：

![2021-07-05-09-27-58](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-07-05-09-27-58.png)

其中gihub-actions-demo.yml内容如下：

~~~ yml

name: GitHub Actions Demo
on: [push]
jobs:
  Explore-GitHub-Actions:
    runs-on: ubuntu-latest
    steps:
      - run: echo "🎉 The job was automatically triggered by a ${{ github.event_name }} event."
      - run: echo "🐧 This job is now running on a ${{ runner.os }} server hosted by GitHub!"
      - run: echo "🔎 The name of your branch is ${{ github.ref }} and your repository is ${{ github.repository }}."
      - name: Check out repository code
        uses: actions/checkout@v2
      - run: echo "💡 The ${{ github.repository }} repository has been cloned to the runner."
      - run: echo "🖥️ The workflow is now ready to test your code on the runner."
      - name: List files in the repository
        run: |
          ls ${{ github.workspace }}
      - run: echo "🍏 This job's status is ${{ job.status }}."

~~~

当push到仓库时，可以Actions选项卡中看到如下内容（实验中我共Push了三次，触发了Actions三次）：

![2021-07-05-09-29-50](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-07-05-09-29-50.png)

左侧为我们在yml中定义的所有工作流，右侧为该工作流执行结果。可以点击执行结果查看执行详情：

![2021-07-05-09-31-47](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-07-05-09-31-47.png)

此时右侧为我们定义的Job，点击Job查看Job执行详情：

![2021-07-05-09-32-21](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-07-05-09-32-21.png)

本次实验到此结束，这是我第一次接触Github Actions，感觉还是挺有趣的。

## 参考资料

1. [GitHub Actions 快速入门](https://docs.github.com/cn/actions/quickstart)