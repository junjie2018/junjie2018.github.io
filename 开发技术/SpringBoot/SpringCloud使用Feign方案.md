## 操作步骤
![2020-08-18-15-17-06](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2020-08-18-15-17-06.png)

1. 如图两个项目mmp-wuid和mmp-member-brand项目

2. mmp-wuid项目下有如下module，其中admin、api、fiegn都依赖于feign

- mmp-global-admin-api
- mmp-global-api
- mmp-global-dmain
- mmp-global-api-feign

3. mmp-global-brand项目下，需要远程与mmp-wuid项目通信，所以mmp-global-admin-api模块依赖了mmp-wuid项目下mmp-global-api-feign项目