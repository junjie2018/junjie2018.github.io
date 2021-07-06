之前有被这个问题困惑过，今天看书的时候，遇到了相关的资料，故记载下来。

可以比较U盘插入计算机前后dmesg命令输出的最后一行内容，也可以用lsblk。

## 实践dmesg

实践中使用dmesg后，新增内容如下：

~~~

[ 2650.933707] usb 1-2: new high-speed USB device number 4 using xhci_hcd
[ 2651.082193] usb 1-2: New USB device found, idVendor=0781, idProduct=5571, bcdDevice= 1.00
[ 2651.082196] usb 1-2: New USB device strings: Mfr=1, Product=2, SerialNumber=3
[ 2651.082198] usb 1-2: Product: Cruzer Fit
[ 2651.082199] usb 1-2: Manufacturer: SanDisk
[ 2651.082200] usb 1-2: SerialNumber: 4C530001160824101320
[ 2651.094526] usb-storage 1-2:1.0: USB Mass Storage device detected
[ 2651.094773] scsi host2: usb-storage 1-2:1.0
[ 2651.094891] usbcore: registered new interface driver usb-storage
[ 2651.095645] usbcore: registered new interface driver uas

~~~

我并没有看出任何有用的信息，稍后，信息变为如下内容，依旧没有看出任何信息：

~~~

[ 2650.933707] usb 1-2: new high-speed USB device number 4 using xhci_hcd
[ 2651.082193] usb 1-2: New USB device found, idVendor=0781, idProduct=5571, bcdDevice= 1.00
[ 2651.082196] usb 1-2: New USB device strings: Mfr=1, Product=2, SerialNumber=3
[ 2651.082198] usb 1-2: Product: Cruzer Fit
[ 2651.082199] usb 1-2: Manufacturer: SanDisk
[ 2651.082200] usb 1-2: SerialNumber: 4C530001160824101320
[ 2651.094526] usb-storage 1-2:1.0: USB Mass Storage device detected
[ 2651.094773] scsi host2: usb-storage 1-2:1.0
[ 2651.094891] usbcore: registered new interface driver usb-storage
[ 2651.095645] usbcore: registered new interface driver uas
[ 2652.102859] scsi 2:0:0:0: Direct-Access     SanDisk  Cruzer Fit       1.00 PQ: 0 ANSI: 6
[ 2652.103259] sd 2:0:0:0: Attached scsi generic sg0 type 0
[ 2652.104008] sd 2:0:0:0: [sda] 30842880 512-byte logical blocks: (15.8 GB/14.7 GiB)
[ 2652.105118] sd 2:0:0:0: [sda] Write Protect is off
[ 2652.105120] sd 2:0:0:0: [sda] Mode Sense: 43 00 00 00
[ 2652.105338] sd 2:0:0:0: [sda] Write cache: disabled, read cache: enabled, doesn't support DPO or FUA
[ 2652.129919]  sda: sda1 sda2
[ 2652.131186] sd 2:0:0:0: [sda] Attached SCSI removable disk

~~~

额，结合我lsblk的实验，可以看出U盘的设备名为sda1和sda2。

## lsblk实践

U盘插入前后lsblk输出：

~~~

# U盘插入前

NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
nvme0n1     259:0    0 465.8G  0 disk 
├─nvme0n1p1 259:1    0   512M  0 part /boot/efi
├─nvme0n1p2 259:2    0 464.3G  0 part /
└─nvme0n1p3 259:3    0   976M  0 part [SWAP]

# U盘插入后

NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda           8:0    1  14.7G  0 disk 
├─sda1        8:1    1  14.1G  0 part 
└─sda2        8:2    1   298M  0 part 
nvme0n1     259:0    0 465.8G  0 disk 
├─nvme0n1p1 259:1    0   512M  0 part /boot/efi
├─nvme0n1p2 259:2    0 464.3G  0 part /
└─nvme0n1p3 259:3    0   976M  0 part [SWAP]

~~~