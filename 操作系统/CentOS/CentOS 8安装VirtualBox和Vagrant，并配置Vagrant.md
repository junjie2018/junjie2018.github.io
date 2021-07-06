## 安装VirtualBox和Vagrant

VirutalBox和Vagrant安装方式有很多，我比较推荐的是下载rpm包的方式安装。这样的VirtualBox和Vagrant的安装方式能够统一，如果对其他方案感兴趣，可以在我的废弃资料中找找，我之前有尝试过其他的方案。

1. 使用如下地址下载资源（可以尝试去官网找下最新的资源）：

~~~

https://download.virtualbox.org/virtualbox/6.1.18/VirtualBox-6.1-6.1.18_142142_el8-1.x86_64.rpm
https://releases.hashicorp.com/vagrant/2.2.15/vagrant_2.2.15_x86_64.rpm

~~~

2. 资源下载好后，拖到服务器中，运行如下指令安装：

~~~

dnf -y install VirtualBox-6.1-6.1.18_142142_el8-1.x86_64.rpm
dnf -y install vagrant_2.2.15_x86_64.rpm

~~~

3. 官网下载Vagrant的CentOS 8的Box（Vagrant Box下载是有技巧的，使用官网提供的简单下载按钮，容易断流），注意要选择平台为VirtualBox。Vagrant的Box可以理解为Vagrant封装的系统镜像。下载完成后拖到服务器中，用如下指令将Box加载到Vagrant中，我下载了CentOS 7和CentOS 8，因为有时候新系统可能会出些奇怪的问题，且资料还不好找，所以就换着来。

~~~

# 参考下载地址，下载后将文件名改为centos7、centos8
https://app.vagrantup.com/generic/boxes/centos7/versions/3.2.14/providers/virtualbox.box
https://app.vagrantup.com/generic/boxes/centos8/versions/3.2.14/providers/virtualbox.box

vagrant box add --name centos7 7a1ba0be-378a-4d19-bc12-fbb8a21a27f0
vagrant box add --name centos8 c7a9dc13-f8f4-43ac-86fd-7155a2ac7a4c

vagrant box list

~~~

1. 启动一个虚拟机完成VirtualBox和Vagrant测试。这个过程中可能会遇到VirtualBox无法启动的问题，可以参考我另一篇文章。

~~~ shell

mkdir Test
cd Test

vagrant init

tee Vagrant <<-'EOF'
Vagrant.configure("2") do |config|

  config.vm.box = "centos7"

  config.vm.provider "virtualbox" do |vb|
    vb.name = 'node'
    vb.memory = "1024"
    vb.cpus = 1
  end
end
EOF

vagrant up

~~~

2. 接下来你会看到如下日志：

~~~

Bringing machine 'default' up with 'virtualbox' provider...
==> default: Clearing any previously set forwarded ports...
==> default: Clearing any previously set network interfaces...
==> default: Preparing network interfaces based on configuration...
    default: Adapter 1: nat
==> default: Forwarding ports...
    default: 22 (guest) => 2222 (host) (adapter 1)
==> default: Running 'pre-boot' VM customizations...
==> default: Booting VM...
==> default: Waiting for machine to boot. This may take a few minutes...
    default: SSH address: 127.0.0.1:2222
    default: SSH username: vagrant
    default: SSH auth method: private key
==> default: Machine booted and ready!
==> default: Checking for guest additions in VM...
    default: The guest additions on this VM do not match the installed version of
    default: VirtualBox! In most cases this is fine, but in rare cases it can
    default: prevent things such as shared folders from working properly. If you see
    default: shared folder errors, please make sure the guest additions within the
    default: virtual machine match the version of VirtualBox you have installed on
    default: your host and reload your VM.
    default: 
    default: Guest Additions Version: 5.2.44
    default: VirtualBox Version: 6.1
==> default: Machine already provisioned. Run `vagrant provision` or use the `--provision`
==> default: flag to force provisioning. Provisioners marked to run always will still run.

~~~

## 使用Vagrant集群脚本

这个脚本是我之前学习一些集群工具时开发的，它的优点是可以轻松改几行代码就启动一个小集群，因为我目前比较清晰的了解我们目前的需求，所以有针对的对这个脚本进行了一些改造。


