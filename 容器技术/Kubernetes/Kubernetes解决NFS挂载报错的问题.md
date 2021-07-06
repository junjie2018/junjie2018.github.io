## 问题描述：

1. 日志如下

~~~

Name:           nfs-client-provisioner-56596b5cc5-kvsff
Namespace:      default
Priority:       0
Node:           kubernetes3/172.17.30.103
Start Time:     Thu, 23 Jul 2020 04:06:48 +0000
Labels:         app=nfs-client-provisioner
                pod-template-hash=56596b5cc5
Annotations:    <none>
Status:         Pending
IP:             
IPs:            <none>
Controlled By:  ReplicaSet/nfs-client-provisioner-56596b5cc5
Containers:
  nfs-client-provisioner:
    Container ID:   
    Image:          quay.io/external_storage/nfs-client-provisioner:latest
    Image ID:       
    Port:           <none>
    Host Port:      <none>
    State:          Waiting
      Reason:       ContainerCreating
    Ready:          False
    Restart Count:  0
    Environment:
      PROVISIONER_NAME:  fuseim.pri/ifs
      NFS_SERVER:        192.168.30.174
      NFS_PATH:          /home/junjie/nfs
    Mounts:
      /persistentvolumes from nfs-client-root (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from nfs-client-provisioner-token-9dv54 (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             False 
  ContainersReady   False 
  PodScheduled      True 
Volumes:
  nfs-client-root:
    Type:      NFS (an NFS mount that lasts the lifetime of a pod)
    Server:    192.168.30.174
    Path:      /home/junjie/nfs
    ReadOnly:  false
  nfs-client-provisioner-token-9dv54:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  nfs-client-provisioner-token-9dv54
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:
  Type     Reason       Age    From                  Message
  ----     ------       ----   ----                  -------
  Warning  FailedMount  4m28s  kubelet, kubernetes3  MountVolume.SetUp failed for volume "nfs-client-root" : mount failed: exit status 32
Mounting command: systemd-run
Mounting arguments: --description=Kubernetes transient mount for /var/lib/kubelet/pods/fb44a773-c2ba-41fa-8b2a-9f9c52115495/volumes/kubernetes.io~nfs/nfs-client-root --scope -- mount -t nfs 192.168.30.174:/home/junjie/nfs /var/lib/kubelet/pods/fb44a773-c2ba-41fa-8b2a-9f9c52115495/volumes/kubernetes.io~nfs/nfs-client-root
Output: Running scope as unit: run-r3196c404e0bd48b89adbb143ac8ba079.scope
mount: /var/lib/kubelet/pods/fb44a773-c2ba-41fa-8b2a-9f9c52115495/volumes/kubernetes.io~nfs/nfs-client-root: bad option; for several filesystems (e.g. nfs, cifs) you might need a /sbin/mount.<type> helper program.
  Normal   Scheduled    4m28s  default-scheduler     Successfully assigned default/nfs-client-provisioner-56596b5cc5-kvsff to kubernetes3
  Warning  FailedMount  4m28s  kubelet, kubernetes3  MountVolume.SetUp failed for volume "nfs-client-root" : mount failed: exit status 32
Mounting command: systemd-run
Mounting arguments: --description=Kubernetes transient mount for /var/lib/kubelet/pods/fb44a773-c2ba-41fa-8b2a-9f9c52115495/volumes/kubernetes.io~nfs/nfs-client-root --scope -- mount -t nfs 192.168.30.174:/home/junjie/nfs /var/lib/kubelet/pods/fb44a773-c2ba-41fa-8b2a-9f9c52115495/volumes/kubernetes.io~nfs/nfs-client-root
Output: Running scope as unit: run-rbed921e0b7cd49a99857c958a1f04a1a.scope
mount: /var/lib/kubelet/pods/fb44a773-c2ba-41fa-8b2a-9f9c52115495/volumes/kubernetes.io~nfs/nfs-client-root: bad option; for several filesystems (e.g. nfs, cifs) you might need a /sbin/mount.<type> helper program.

~~~

## 处理步骤

1. 三台虚拟机上运行如下指令

~~~ shell

sudo apt install -y nfs-common

~~~

## 相关教程

1. [KUBERNETES挂载NFS报错：MOUNTVOLUME.SETUP FAILED FOR VOLUME “HTTPD-STORAGE” : MOUNT FAILED: EXIT STATUS 32](https://www.centosdoc.com/docker/131.html)