我在使用自编译的python3执行pyinstaller时遇到了如下问题：

~~~

[root@base launch]# pyinstaller -F launch.py 
31 INFO: PyInstaller: 4.3
31 INFO: Python: 3.8.9
62 INFO: Platform: Linux-3.10.0-1160.el7.x86_64-x86_64-with-glibc2.17
62 INFO: wrote /root/Software/launch/launch.spec
63 INFO: UPX is not available.
64 INFO: Extending PYTHONPATH with paths
['/root/Software/launch', '/root/Software/launch']
67 INFO: checking Analysis
67 INFO: Building Analysis because Analysis-00.toc is non existent
67 INFO: Initializing module dependency graph...
67 INFO: Caching module graph hooks...
72 INFO: Analyzing base_library.zip ...
1333 INFO: Processing pre-find module path hook distutils from '/usr/local/python3/lib/python3.8/site-packages/PyInstaller/hooks/pre_find_module_path/hook-distutils.py'.
1334 INFO: distutils: retargeting to non-venv dir '/usr/local/python3/lib/python3.8'
3008 INFO: Caching module dependency graph...
3101 INFO: running Analysis Analysis-00.toc
3110 INFO: Analyzing /root/Software/launch/launch.py
3169 INFO: Processing pre-safe import module hook urllib3.packages.six.moves from '/usr/local/python3/lib/python3.8/site-packages/PyInstaller/hooks/pre_safe_import_module/hook-urllib3.packages.six.moves.py'.
3907 INFO: Processing module hooks...
3907 INFO: Loading module hook 'hook-certifi.py' from '/usr/local/python3/lib/python3.8/site-packages/_pyinstaller_hooks_contrib/hooks/stdhooks'...
3910 INFO: Loading module hook 'hook-_tkinter.py' from '/usr/local/python3/lib/python3.8/site-packages/PyInstaller/hooks'...
3954 INFO: checking Tree
3954 INFO: Building Tree because Tree-00.toc is non existent
3954 INFO: Building Tree Tree-00.toc
3958 INFO: checking Tree
3958 INFO: Building Tree because Tree-01.toc is non existent
3958 INFO: Building Tree Tree-01.toc
3983 INFO: checking Tree
3984 INFO: Building Tree because Tree-02.toc is non existent
3984 INFO: Building Tree Tree-02.toc
3985 INFO: Loading module hook 'hook-difflib.py' from '/usr/local/python3/lib/python3.8/site-packages/PyInstaller/hooks'...
3987 INFO: Loading module hook 'hook-distutils.py' from '/usr/local/python3/lib/python3.8/site-packages/PyInstaller/hooks'...
3993 INFO: Loading module hook 'hook-distutils.util.py' from '/usr/local/python3/lib/python3.8/site-packages/PyInstaller/hooks'...
3994 INFO: Loading module hook 'hook-encodings.py' from '/usr/local/python3/lib/python3.8/site-packages/PyInstaller/hooks'...
4026 INFO: Loading module hook 'hook-heapq.py' from '/usr/local/python3/lib/python3.8/site-packages/PyInstaller/hooks'...
4028 INFO: Loading module hook 'hook-lib2to3.py' from '/usr/local/python3/lib/python3.8/site-packages/PyInstaller/hooks'...
4053 INFO: Loading module hook 'hook-multiprocessing.util.py' from '/usr/local/python3/lib/python3.8/site-packages/PyInstaller/hooks'...
4054 INFO: Loading module hook 'hook-pickle.py' from '/usr/local/python3/lib/python3.8/site-packages/PyInstaller/hooks'...
4055 INFO: Loading module hook 'hook-sysconfig.py' from '/usr/local/python3/lib/python3.8/site-packages/PyInstaller/hooks'...
4055 INFO: Loading module hook 'hook-xml.etree.cElementTree.py' from '/usr/local/python3/lib/python3.8/site-packages/PyInstaller/hooks'...
4055 INFO: Loading module hook 'hook-xml.py' from '/usr/local/python3/lib/python3.8/site-packages/PyInstaller/hooks'...
4091 INFO: Looking for ctypes DLLs
4111 INFO: Analyzing run-time hooks ...
4114 INFO: Including run-time hook '/usr/local/python3/lib/python3.8/site-packages/PyInstaller/hooks/rthooks/pyi_rth_multiprocessing.py'
4116 INFO: Including run-time hook '/usr/local/python3/lib/python3.8/site-packages/_pyinstaller_hooks_contrib/hooks/rthooks/pyi_rth_certifi.py'
4120 INFO: Looking for dynamic libraries
4379 INFO: Looking for eggs
4379 INFO: Python library not in binary dependencies. Doing additional searching...
Traceback (most recent call last):
  File "/usr/local/python3/bin/pyinstaller", line 8, in <module>
    sys.exit(run())
  File "/usr/local/python3/lib/python3.8/site-packages/PyInstaller/__main__.py", line 114, in run
    run_build(pyi_config, spec_file, **vars(args))
  File "/usr/local/python3/lib/python3.8/site-packages/PyInstaller/__main__.py", line 65, in run_build
    PyInstaller.building.build_main.main(pyi_config, spec_file, **kwargs)
  File "/usr/local/python3/lib/python3.8/site-packages/PyInstaller/building/build_main.py", line 737, in main
    build(specfile, kw.get('distpath'), kw.get('workpath'), kw.get('clean_build'))
  File "/usr/local/python3/lib/python3.8/site-packages/PyInstaller/building/build_main.py", line 684, in build
    exec(code, spec_namespace)
  File "/root/Software/launch/launch.spec", line 7, in <module>
    a = Analysis(['launch.py'],
  File "/usr/local/python3/lib/python3.8/site-packages/PyInstaller/building/build_main.py", line 242, in __init__
    self.__postinit__()
  File "/usr/local/python3/lib/python3.8/site-packages/PyInstaller/building/datastruct.py", line 160, in __postinit__
    self.assemble()
  File "/usr/local/python3/lib/python3.8/site-packages/PyInstaller/building/build_main.py", line 476, in assemble
    self._check_python_library(self.binaries)
  File "/usr/local/python3/lib/python3.8/site-packages/PyInstaller/building/build_main.py", line 581, in _check_python_library
    python_lib = bindepend.get_python_library_path()
  File "/usr/local/python3/lib/python3.8/site-packages/PyInstaller/depend/bindepend.py", line 956, in get_python_library_path
    raise IOError(msg)
OSError: Python library not found: libpython3.8mu.so.1.0, libpython3.8m.so.1.0, libpython3.8m.so, libpython3.8.so.1.0
    This would mean your Python installation doesn't come with proper library files.
    This usually happens by missing development package, or unsuitable build parameters of Python installation.

    * On Debian/Ubuntu, you would need to install Python development packages
      * apt-get install python3-dev
      * apt-get install python-dev
    * If you're building Python by yourself, please rebuild your Python with `--enable-shared` (or, `--enable-framework` on Darwin)
    
~~~

我执行了如下指令：

~~~

./configure --prefix=/usr/local/python3 --enable-shared

make clean && make && make install

~~~

此时执行时，又有如下报错：

~~~

[root@base launch]# pyinstaller -F launch.py 
/usr/local/python3/bin/python3.8: error while loading shared libraries: libpython3.8.so.1.0: cannot open shared object file: No such file or directory

~~~

我观察了Python3编译后的目录结构，发现了`libpython3.8.so.1.0`文件：

![2021-06-28-14-26-03](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-06-28-14-26-03.png)

我觉得很大可能性是make install指令没有将这个文件copy到相应的目录中，我手动完成了该操作：

~~~

cp libpython3.8.so.1.0 /usr/lib64

~~~

同样的，因为编译安装的时候我已经设置了环境变量了，这块我不需要进行相应的配置。

## 参考资料

1. [Python打包方法——Pyinstaller CentOS下踩坑记录](https://www.wandouip.com/t5i295394/)