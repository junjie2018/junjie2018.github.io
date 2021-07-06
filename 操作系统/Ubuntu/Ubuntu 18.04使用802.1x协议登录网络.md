该教程为我工具机在公司访问网络用到的

## 操作步骤

1. 代码如下：

~~~ shell
nmcli con edit CONNECTION_NAME
nmcli> set ipv4.method auto
nmcli> set 802-1x.eap peap
nmcli> set 802-1x.identity sadegh.k@atu.com
nmcli> set 802-1x.phase2-auth mschapv2
nmcli> save
nmcli> quit
~~~

## 参考教程

1. [Ubuntu 18.04终端802.1X认证设置？](https://cloud.tencent.com/developer/ask/132407)
2. [802.1x with NetworkManager using nmcli](https://major.io/2016/05/03/802-1x-networkmanager-using-nmcli/)
3. [Ubuntu上通过802.1x认证联网](https://blog.csdn.net/iamcxl369/article/details/78900876)
