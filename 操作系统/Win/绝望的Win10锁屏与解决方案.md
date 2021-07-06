公司电脑锁屏后，重登时都会出现与工作站失去信任关系的问题，解决该问题必须重启机器。运维同事帮我解决了很多次，都没有效果，非常绝望。更绝望的是我有随手按Win + L的习惯。

与其指望自己不按Win + L键，不如把这个键给禁了。

## 操作步骤

1. 打开注册表（Win + R，输入regedit进入）

2. 进入如下目录：

~~~

计算机\HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Policies

~~~

3. 在左侧：鼠标右键Policies，新建项，命名为System

4. 点开System，在右侧，新建DWORD (32-bit) ，命名为DisableLockWorkstation

5. 右键点击新建的DisableLockWorkstation，点击修改，将值改为1

## 参考资料

1. [How to Disable the Lock Screen Shortcut Key (Win + L) in Windows](https://www.maketecheasier.com/disable-lock-screen-shortcut-key-windows/)

## 个人小结

本想截图把过程说清晰，但是我打开注册表时，Snapaste就无法使用。参考资料里有清楚的图片，如果不是太理解可以参考那些图片。参考资料可能会无法访问，可以直接谷歌“how to disable lock screen hotkey in win10”。

这个需求要的人应该非常的少，我百度了很长时间都没有找到解决方案。

3.20号，运维大大帮我解决这个问题了，非常开心~