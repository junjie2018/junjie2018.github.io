问题是这样的，我们的项目需要使用到GRpc的9090端口，所以申请运维帮我们暴露一下该端口，等我们自己测试该端口时，发现该端口无法正常使用（原80端口是正常的），检查了POD、Service后，可以确认该端口应该处于开启状态的。我后来注意到，运维帮我们配置的Service如下：

~~~

spec:
  clusterIP: 10.254.73.68
  ports:
  - name: http-tcp
    port: 80
    protocol: TCP
    targetPort: 8080
  - name: http-tcp-9090
    port: 9090
    protocol: TCP
    targetPort: 9090
  selector:
    app: dyf-provider
  sessionAffinity: None
  type: ClusterIP

~~~

我记得我学习Istio时了解到，Istio会根据你Service暴露端口时取的名字，做一些什么操作，我觉得是`http-tcp-9090`前的http影响到我们了，所以我去掉了`http`，测试该端口服务正常。我们最后尝试将tpc换成grpc，该端口也是正常使用的。

最后的最后，为了确认不是巧合，我们又换成了http，该端口服务果然不可以访问了，哈哈，可以肯定就是这个http前缀影响了这个端口的正常使用。