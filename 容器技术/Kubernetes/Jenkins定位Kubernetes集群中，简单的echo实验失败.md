## 问题描述

1. 按照教程走完所有流程后，始终得不到项目的日志输出，项目状态一致处于运行中
2. 查看slave pod的describe和log都没有任何提示
3. 查看master pod的log得到如下日志

~~~ 
VM settings:
    Max. Heap Size (Estimated): 247.50M
    Ergonomics Machine Class: server
    Using VM: OpenJDK 64-Bit Server VM

Running from: /usr/share/jenkins/jenkins.war
webroot: EnvVars.masterEnvVars.get("JENKINS_HOME")
2020-07-24 02:35:59.320+0000 [id=1]	INFO	org.eclipse.jetty.util.log.Log#initialized: Logging initialized @671ms to org.eclipse.jetty.util.log.JavaUtilLog
2020-07-24 02:35:59.470+0000 [id=1]	INFO	winstone.Logger#logInternal: Beginning extraction from war file
2020-07-24 02:35:59.540+0000 [id=1]	WARNING	o.e.j.s.handler.ContextHandler#setContextPath: Empty contextPath
2020-07-24 02:35:59.641+0000 [id=1]	INFO	org.eclipse.jetty.server.Server#doStart: jetty-9.4.27.v20200227; built: 2020-02-27T18:37:21.340Z; git: a304fd9f351f337e7c0e2a7c28878dd536149c6c; jvm 1.8.0_242-b08
2020-07-24 02:36:00.642+0000 [id=1]	INFO	o.e.j.w.StandardDescriptorProcessor#visitServlet: NO JSP Support for /, did not find org.eclipse.jetty.jsp.JettyJspServlet
2020-07-24 02:36:00.749+0000 [id=1]	INFO	o.e.j.s.s.DefaultSessionIdManager#doStart: DefaultSessionIdManager workerName=node0
2020-07-24 02:36:00.749+0000 [id=1]	INFO	o.e.j.s.s.DefaultSessionIdManager#doStart: No SessionScavenger set, using defaults
2020-07-24 02:36:00.757+0000 [id=1]	INFO	o.e.j.server.session.HouseKeeper#startScavenging: node0 Scavenging every 660000ms
2020-07-24 02:36:01.618+0000 [id=1]	INFO	hudson.WebAppMain#contextInitialized: Jenkins home directory: /var/jenkins_home found at: EnvVars.masterEnvVars.get("JENKINS_HOME")
2020-07-24 02:36:01.890+0000 [id=1]	INFO	o.e.j.s.handler.ContextHandler#doStart: Started w.@21be3395{Jenkins v2.235.2,/,file:///var/jenkins_home/war/,AVAILABLE}{/var/jenkins_home/war}
2020-07-24 02:36:01.930+0000 [id=1]	INFO	o.e.j.server.AbstractConnector#doStart: Started ServerConnector@62043840{HTTP/1.1, (http/1.1)}{0.0.0.0:8080}
2020-07-24 02:36:01.931+0000 [id=1]	INFO	org.eclipse.jetty.server.Server#doStart: Started @3283ms
2020-07-24 02:36:01.932+0000 [id=20]	INFO	winstone.Logger#logInternal: Winstone Servlet Engine running: controlPort=disabled
2020-07-24 02:36:03.435+0000 [id=25]	INFO	jenkins.InitReactorRunner$1#onAttained: Started initialization
2020-07-24 02:36:03.892+0000 [id=26]	INFO	jenkins.InitReactorRunner$1#onAttained: Listed all plugins
2020-07-24 02:36:09.728+0000 [id=26]	INFO	jenkins.InitReactorRunner$1#onAttained: Prepared all plugins
2020-07-24 02:36:09.738+0000 [id=26]	INFO	jenkins.InitReactorRunner$1#onAttained: Started all plugins
2020-07-24 02:36:09.941+0000 [id=25]	INFO	jenkins.InitReactorRunner$1#onAttained: Augmented all extensions
2020-07-24 02:36:11.228+0000 [id=25]	INFO	jenkins.InitReactorRunner$1#onAttained: System config loaded
2020-07-24 02:36:11.229+0000 [id=25]	INFO	jenkins.InitReactorRunner$1#onAttained: System config adapted
2020-07-24 02:36:11.229+0000 [id=25]	INFO	jenkins.InitReactorRunner$1#onAttained: Loaded all jobs
2020-07-24 02:36:11.235+0000 [id=26]	INFO	jenkins.InitReactorRunner$1#onAttained: Configuration for all jobs updated
2020-07-24 02:36:11.624+0000 [id=39]	INFO	hudson.model.AsyncPeriodicWork#lambda$doRun$0: Started Download metadata
2020-07-24 02:36:11.919+0000 [id=39]	INFO	hudson.model.AsyncPeriodicWork#lambda$doRun$0: Finished Download metadata. 202 ms
2020-07-24 02:36:12.815+0000 [id=26]	INFO	o.s.c.s.AbstractApplicationContext#prepareRefresh: Refreshing org.springframework.web.context.support.StaticWebApplicationContext@6effd93a: display name [Root WebApplicationContext]; startup date [Fri Jul 24 10:36:12 CST 2020]; root of context hierarchy
2020-07-24 02:36:12.816+0000 [id=26]	INFO	o.s.c.s.AbstractApplicationContext#obtainFreshBeanFactory: Bean factory for application context [org.springframework.web.context.support.StaticWebApplicationContext@6effd93a]: org.springframework.beans.factory.support.DefaultListableBeanFactory@62c07203
2020-07-24 02:36:12.827+0000 [id=26]	INFO	o.s.b.f.s.DefaultListableBeanFactory#preInstantiateSingletons: Pre-instantiating singletons in org.springframework.beans.factory.support.DefaultListableBeanFactory@62c07203: defining beans [authenticationManager]; root of factory hierarchy
2020-07-24 02:36:13.227+0000 [id=26]	INFO	o.s.c.s.AbstractApplicationContext#prepareRefresh: Refreshing org.springframework.web.context.support.StaticWebApplicationContext@1a5fd292: display name [Root WebApplicationContext]; startup date [Fri Jul 24 10:36:13 CST 2020]; root of context hierarchy
2020-07-24 02:36:13.227+0000 [id=26]	INFO	o.s.c.s.AbstractApplicationContext#obtainFreshBeanFactory: Bean factory for application context [org.springframework.web.context.support.StaticWebApplicationContext@1a5fd292]: org.springframework.beans.factory.support.DefaultListableBeanFactory@449df8fb
2020-07-24 02:36:13.228+0000 [id=26]	INFO	o.s.b.f.s.DefaultListableBeanFactory#preInstantiateSingletons: Pre-instantiating singletons in org.springframework.beans.factory.support.DefaultListableBeanFactory@449df8fb: defining beans [filter,legacy]; root of factory hierarchy
2020-07-24 02:36:13.445+0000 [id=26]	INFO	o.c.j.p.k.KubernetesClientProvider$SaveableListenerImpl#onChange: Invalidating Kubernetes client: kubernetes null
2020-07-24 02:36:13.450+0000 [id=26]	INFO	jenkins.InitReactorRunner$1#onAttained: Completed initialization
2020-07-24 02:36:13.552+0000 [id=19]	INFO	o.c.j.p.k.KubernetesClientProvider$SaveableListenerImpl#onChange: Invalidating Kubernetes client: kubernetes null
2020-07-24 02:36:13.860+0000 [id=19]	INFO	hudson.WebAppMain$3#run: Jenkins is fully up and running
2020-07-24 02:37:20.320+0000 [id=11]	WARNING	hudson.security.csrf.CrumbFilter#doFilter: Found invalid crumb 8912d904c747a67dab090ad588545030425547e720e49e7a2af899038c6844b3. If you are calling this URL with a script, please use the API Token instead. More information: https://jenkins.io/redirect/crumb-cannot-be-used-for-script
2020-07-24 02:37:20.320+0000 [id=11]	WARNING	hudson.security.csrf.CrumbFilter#doFilter: No valid crumb was included in request for /ajaxBuildQueue by wujunjie. Returning 403.
2020-07-24 02:37:20.373+0000 [id=15]	WARNING	hudson.security.csrf.CrumbFilter#doFilter: Found invalid crumb 8912d904c747a67dab090ad588545030425547e720e49e7a2af899038c6844b3. If you are calling this URL with a script, please use the API Token instead. More information: https://jenkins.io/redirect/crumb-cannot-be-used-for-script
2020-07-24 02:37:20.373+0000 [id=15]	WARNING	hudson.security.csrf.CrumbFilter#doFilter: No valid crumb was included in request for /ajaxExecutors by wujunjie. Returning 403.
2020-07-24 02:38:41.563+0000 [id=24]	INFO	o.c.j.p.k.KubernetesCloud#provision: Excess workload after pending Kubernetes agents: 1
2020-07-24 02:38:41.565+0000 [id=24]	INFO	o.c.j.p.k.KubernetesCloud#provision: Template for label joker-jnlp: jenkins-slave
2020-07-24 02:38:43.128+0000 [id=24]	INFO	o.internal.platform.Platform#log: ALPN callback dropped: HTTP/2 is disabled. Is alpn-boot on the boot class path?
2020-07-24 02:38:51.948+0000 [id=38]	INFO	hudson.slaves.NodeProvisioner#lambda$update$6: jenkins-slave-wrpbz provisioning successfully completed. We have now 2 computer(s)
2020-07-24 02:38:52.176+0000 [id=67]	INFO	o.c.j.p.k.KubernetesLauncher#launch: Created Pod: devops/jenkins-slave-wrpbz
2020-07-24 02:38:52.324+0000 [id=71]	INFO	o.internal.platform.Platform#log: ALPN callback dropped: HTTP/2 is disabled. Is alpn-boot on the boot class path?
2020-07-24 02:38:52.348+0000 [id=71]	WARNING	i.f.k.c.d.i.WatchConnectionManager$1#onFailure: Exec Failure: HTTP 403, Status: 403 - events is forbidden: User "system:serviceaccount:devops:jenkins-sa" cannot watch resource "events" in API group "" in the namespace "devops"
java.net.ProtocolException: Expected HTTP 101 response but was '403 Forbidden'
	at okhttp3.internal.ws.RealWebSocket.checkResponse(RealWebSocket.java:229)
	at okhttp3.internal.ws.RealWebSocket$2.onResponse(RealWebSocket.java:196)
	at okhttp3.RealCall$AsyncCall.execute(RealCall.java:203)
	at okhttp3.internal.NamedRunnable.run(NamedRunnable.java:32)
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624)
	at java.lang.Thread.run(Thread.java:748)
2020-07-24 02:38:52.354+0000 [id=67]	INFO	o.c.j.p.k.KubernetesLauncher#eventWatch: Cannot watch events on devops/jenkins-slave-wrpbz
Also:   java.lang.Throwable: waiting here
		at io.fabric8.kubernetes.client.utils.Utils.waitUntilReady(Utils.java:144)
		at io.fabric8.kubernetes.client.dsl.internal.WatchConnectionManager.waitUntilReady(WatchConnectionManager.java:341)
		at io.fabric8.kubernetes.client.dsl.base.BaseOperation.watch(BaseOperation.java:755)
		at io.fabric8.kubernetes.client.dsl.base.BaseOperation.watch(BaseOperation.java:739)
		at io.fabric8.kubernetes.client.dsl.base.BaseOperation.watch(BaseOperation.java:70)
		at org.csanchez.jenkins.plugins.kubernetes.KubernetesLauncher.eventWatch(KubernetesLauncher.java:230)
		at org.csanchez.jenkins.plugins.kubernetes.KubernetesLauncher.launch(KubernetesLauncher.java:138)
		at hudson.slaves.SlaveComputer.lambda$_connect$0(SlaveComputer.java:296)
		at jenkins.util.ContextResettingExecutorService$2.call(ContextResettingExecutorService.java:46)
		at jenkins.security.ImpersonatingExecutorService$2.call(ImpersonatingExecutorService.java:71)
		at java.util.concurrent.FutureTask.run(FutureTask.java:266)
io.fabric8.kubernetes.client.KubernetesClientException: events is forbidden: User "system:serviceaccount:devops:jenkins-sa" cannot watch resource "events" in API group "" in the namespace "devops"
	at io.fabric8.kubernetes.client.dsl.internal.WatchConnectionManager$1.onFailure(WatchConnectionManager.java:203)
	at okhttp3.internal.ws.RealWebSocket.failWebSocket(RealWebSocket.java:571)
	at okhttp3.internal.ws.RealWebSocket$2.onResponse(RealWebSocket.java:198)
	at okhttp3.RealCall$AsyncCall.execute(RealCall.java:203)
	at okhttp3.internal.NamedRunnable.run(NamedRunnable.java:32)
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624)
	at java.lang.Thread.run(Thread.java:748)
2020-07-24 02:38:54.672+0000 [id=67]	INFO	o.c.j.p.k.KubernetesLauncher#launch: Pod is running: devops/jenkins-slave-wrpbz
2020-07-24 02:39:25.039+0000 [id=67]	INFO	o.c.j.p.k.KubernetesLauncher#launch: Waiting for agent to connect (30/100): jenkins-slave-wrpbz
2020-07-24 02:39:55.272+0000 [id=67]	INFO	o.c.j.p.k.KubernetesLauncher#launch: Waiting for agent to connect (60/100): jenkins-slave-wrpbz
~~~

## 解决步骤

1. 使用如下RABC配置

~~~ yml

apiVersion: v1
kind: ServiceAccount
metadata:
  name: jenkins-sa
  namespace: devops
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: jenkins-crd
  labels:
    name: jenkins
subjects:
  - kind: ServiceAccount
    name: jenkins-sa
    namespace: devops
roleRef:
  kind: ClusterRole
  name: cluster-admin
  apiGroup: rbac.authorization.k8s.io

~~~
