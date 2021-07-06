事情的起因是这样的，我创建Ubuntu虚拟机时没有调整硬盘大小，结果磁盘只有32G，这个孔家大小在编译OpenWrt时完全不够用。按照我以往的做法，我会直接重新一个新的虚拟机，并调整硬盘大小为100G。但是，在接触PVE的过程中，我对Linux的磁盘有些熟悉了，我想尝试这自己将这个磁盘调整为我想要的大小。于是有了这篇文章。

## 操作步骤

1. 在PVE上将虚拟机的磁盘大小调整为132G。

2. 查看磁盘信息：我在查看磁盘信息的时候，终端显示了一些红色的提示信息，大致就是说部分空间没有用上，下面的一步可以修改该问题：

~~~

fdisk -l

~~~

3. 修复`fdisk -l`指令中的提示信息。完成该步后，`fdisk -l`就不会有任何错误提示信息，我也是偶然发现可以用这个修复的。

~~~ shell

# 我的输入
root@junjie:~# parted /dev/sda
GNU Parted 3.3
Using /dev/sda
Welcome to GNU Parted! Type 'help' to view a list of commands.

# 我的输入
(parted) print                                                            
Warning: Not all of the space available to /dev/sda appears to be used, you can fix the GPT to use all of the space (an extra 209715200
blocks) or continue with the current setting? 

# 我的输入
Fix/Ignore? fix                                                           
Model: QEMU QEMU HARDDISK (scsi)
Disk /dev/sda: 142GB
Sector size (logical/physical): 512B/512B
Partition Table: gpt
Disk Flags: 

Number  Start   End     Size    File system  Name  Flags
 1      1049kB  2097kB  1049kB                     bios_grub
 2      2097kB  1076MB  1074MB  ext4
 3      1076MB  34.4GB  33.3GB

~~~

4. 为分区增加空间，完成该不，通过`fdisk -l`指令已经可以看到分区的大小发生了变化。

~~~ shell

# 我的输入
root@junjie:~# parted /dev/sda
GNU Parted 3.3
Using /dev/sda
Welcome to GNU Parted! Type 'help' to view a list of commands.

# 我的输入
(parted) print                                                            
Model: QEMU QEMU HARDDISK (scsi)
Disk /dev/sda: 142GB
Sector size (logical/physical): 512B/512B
Partition Table: gpt
Disk Flags: 

Number  Start   End     Size    File system  Name  Flags
 1      1049kB  2097kB  1049kB                     bios_grub
 2      2097kB  1076MB  1074MB  ext4
 3      1076MB  34.4GB  33.3GB

# 我的输入
(parted) resizepart 3 100%                                                

# 我的输入
(parted) quit                                                             
Information: You may need to update /etc/fstab.

~~~

5. 修改物理卷大小（这一步我没有看出任何变化信息）

~~~

pvresize /dev/sda2

~~~

6. 修改逻辑卷大小（ubuntu--vg-ubuntu--lv文件视各自情况而定，你的文件可能不叫这个名字）

~~~

lvresize --extents +100%FREE --resizefs /dev/mapper/ubuntu--vg-ubuntu--lv

~~~

这一步带来的变化如图所示：

![2021-05-22-12-01-13](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-05-22-12-01-13.png)

7. 最后重启验证下

![2021-05-22-12-18-51](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-05-22-12-18-51.png)

## 参考资料

1. [proxmox ve (PVE) 调整虚拟机(VM)的磁盘大小](https://www.d3tt.com/view/255)