是这样的，我在合并分支的时候，删除了原来的源码，从Git上拉取了一份新的代码，然后再Idea导入该源码并启动该项目，结果再启动的过程中一致报如下错误：

~~~

***************************
APPLICATION FAILED TO START
***************************

Description:

The bean 'http://metadata.FeignClientSpecification' could not be registered. A bean with that name has already been defined and overriding is disabled.

Action:

Consider renaming one of the beans or enabling overriding by setting spring.main.allow-bean-definition-overriding=true

~~~

之前运气比较好，我在启动项目时一定会指定`spring.profiles.active=local`，所以我也是第一看到这个错误。我之前的项目中也从来没有进行过`spring.main.allow-bean-definition-overriding=true`配置，所以我主观上也不会想到在配置文件中进行该项配置。

我在什么时候意识到是我自己出了问题，我在发现我昨天成功运行的分支突然也运行不了，后来才发现是自己忘记配置`spring.profiles.active=local`了（经过同事提醒）。在这次定位问题的过程中我也接触了一些新的东西，比如：

![2021-05-27-12-14-21](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-05-27-12-14-21.png)

实际上我是不需要配置`spring.profiles.active=local`的，我可以在profiles中选择local，就可以使用到local环境。经过同事了解到其原理：当选择了local后，我们在运行时会自动给带上`spring.profiles.active=local`参数。其实我是有点不理解的是，我运行的时候是通过Idea上的运行按钮，profiles配置属于Maven的配置，这两者之间是如何联系在一起的呢？