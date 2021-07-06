## 问题描述：

1. 日志如下

~~~

Name:         jenkins-7d6c886b8-nkl4g
Namespace:    devops
Priority:     0
Node:         kubernetes3/172.17.30.103
Start Time:   Thu, 23 Jul 2020 06:13:34 +0000
Labels:       app=jenkins
              pod-template-hash=7d6c886b8
Annotations:  <none>
Status:       Pending
IP:           10.244.2.3
IPs:
  IP:           10.244.2.3
Controlled By:  ReplicaSet/jenkins-7d6c886b8
Containers:
  jenkins:
    Container ID:   
    Image:          192.168.30.174:80/test/jenkins:lts
    Image ID:       
    Ports:          8080/TCP, 50000/TCP
    Host Ports:     0/TCP, 0/TCP
    State:          Waiting
      Reason:       ImagePullBackOff
    Ready:          False
    Restart Count:  0
    Limits:
      cpu:     1
      memory:  1Gi
    Requests:
      cpu:      500m
      memory:   512Mi
    Liveness:   http-get http://:8080/login delay=60s timeout=5s period=10s #success=1 #failure=12
    Readiness:  http-get http://:8080/login delay=60s timeout=5s period=10s #success=1 #failure=12
    Environment:
      JAVA_OPTS:  -XshowSettings:vm -Dhudson.slaves.NodeProvisioner.initialDelay=0 -Dhudson.slaves.NodeProvisioner.MARGIN=50 -Dhudson.slaves.NodeProvisioner.MARGIN0=0.85 -Duser.timezone=Asia/Shanghai
    Mounts:
      /var/jenkins_home from jenkinshome (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from jenkins-sa-token-nt57q (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             False 
  ContainersReady   False 
  PodScheduled      True 
Volumes:
  jenkinshome:
    Type:       PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
    ClaimName:  jenkins-pvc
    ReadOnly:   false
  jenkins-sa-token-nt57q:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  jenkins-sa-token-nt57q
    Optional:    false
QoS Class:       Burstable
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:
  Type     Reason            Age                    From                  Message
  ----     ------            ----                   ----                  -------
  Warning  FailedScheduling  5m38s (x3 over 6m45s)  default-scheduler     persistentvolumeclaim "jenkins-pvc" not found
  Warning  FailedScheduling  4m54s                  default-scheduler     running "VolumeBinding" filter plugin for pod "jenkins-7d6c886b8-nkl4g": error getting PVC "devops/jenkins-pvc": could not find v1.PersistentVolumeClaim "devops/jenkins-pvc"
  Warning  FailedScheduling  4m54s (x2 over 4m54s)  default-scheduler     running "VolumeBinding" filter plugin for pod "jenkins-7d6c886b8-nkl4g": pod has unbound immediate PersistentVolumeClaims
  Normal   Scheduled         4m50s                  default-scheduler     Successfully assigned devops/jenkins-7d6c886b8-nkl4g to kubernetes3
  Normal   Pulling           3m26s (x4 over 4m48s)  kubelet, kubernetes3  Pulling image "192.168.30.174:80/test/jenkins:lts"
  Warning  Failed            3m26s (x4 over 4m48s)  kubelet, kubernetes3  Failed to pull image "192.168.30.174:80/test/jenkins:lts": rpc error: code = Unknown desc = Error response from daemon: Get https://192.168.30.174:80/v2/: http: server gave HTTP response to HTTPS client
  Warning  Failed            3m26s (x4 over 4m48s)  kubelet, kubernetes3  Error: ErrImagePull
  Normal   BackOff           3m12s (x6 over 4m47s)  kubelet, kubernetes3  Back-off pulling image "192.168.30.174:80/test/jenkins:lts"
  Warning  Failed            2m59s (x7 over 4m47s)  kubelet, kubernetes3  Error: ImagePullBackOff

~~~

## 处理步骤

1. 三台上配置/etc/docker/deamon.json文件，具体配置内容参考相关教程（我已提供相关教程）