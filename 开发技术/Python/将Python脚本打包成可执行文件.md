还不错，打包出来后只有6M多一点，哈哈，虽然我只写了几行代码。

20210628后续：

该工具还可以将脚本打包成Linux可执行的文件，记录一下步骤：

1. 安装工具`pip3 install pyinstaller`
2. 检查工具版本`pyinstaller -v`
3. 执行`pyinstaller -F launch.py`指令
4. 在dist目录下找生成的可执行文件

## 参考资料

1. [使用pycharm将python项目打包成exe运行文件](https://blog.csdn.net/FORLOVEHUAN/article/details/100050665)