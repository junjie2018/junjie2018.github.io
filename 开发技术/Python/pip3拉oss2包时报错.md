## 问题描述

我在CentOS上通过自己编译的Python的pip工具下载oss2模块时，出现如下报错：

~~~

[root@localhost ~]# pip3 install oss2
Collecting oss2
  Using cached oss2-2.14.0.tar.gz (224 kB)
    ERROR: Command errored out with exit status 1:
     command: /usr/local/python3/bin/python3.8 -c 'import sys, setuptools, tokenize; sys.argv[0] = '"'"'/tmp/pip-install-5yv_1h3n/oss2/setup.py'"'"'; __file__='"'"'/tmp/pip-install-5yv_1h3n/oss2/setup.py'"'"';f=getattr(tokenize, '"'"'open'"'"', open)(__file__);code=f.read().replace('"'"'\r\n'"'"', '"'"'\n'"'"');f.close();exec(compile(code, __file__, '"'"'exec'"'"'))' egg_info --egg-base /tmp/pip-pip-egg-info-pi9uat3k
         cwd: /tmp/pip-install-5yv_1h3n/oss2/
    Complete output (11 lines):
    Traceback (most recent call last):
      File "<string>", line 1, in <module>
      File "/usr/local/python3/lib/python3.8/site-packages/setuptools/__init__.py", line 23, in <module>
        from setuptools.dist import Distribution
      File "/usr/local/python3/lib/python3.8/site-packages/setuptools/dist.py", line 34, in <module>
        from setuptools import windows_support
      File "/usr/local/python3/lib/python3.8/site-packages/setuptools/windows_support.py", line 2, in <module>
        import ctypes
      File "/usr/local/python3/lib/python3.8/ctypes/__init__.py", line 7, in <module>
        from _ctypes import Union, Structure, Array
    ModuleNotFoundError: No module named '_ctypes'
    ----------------------------------------
ERROR: Command errored out with exit status 1: python setup.py egg_info Check the logs for full command output.
WARNING: You are using pip version 20.2.3; however, version 21.0.1 is available.
You should consider upgrading via the '/usr/local/python3/bin/python3.8 -m pip install --upgrade pip' command.

~~~

## 解决方案

1. 执行如下指令：

~~~

yum install libffi-devel -y

~~~

然后重新编译安装你的Python。

~~~

make clean && make && make install

~~~

## 参考资料

1. [ModuleNotFoundError: No module named '_ctypes'的解决方案](https://www.cnblogs.com/fanbi/p/12375023.html)