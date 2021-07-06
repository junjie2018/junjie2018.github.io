在CentOS上自行编译python3后，安装依赖时出现了如下问题：

~~~

[root@base launch]# pip3 install pyinstaller
Collecting pyinstaller
  Downloading pyinstaller-4.3.tar.gz (3.7 MB)
     |████████████████████████████████| 3.7 MB 1.1 MB/s 
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
ERROR: Exception:
Traceback (most recent call last):
  File "/usr/local/python3/lib/python3.8/site-packages/pip/_internal/cli/base_command.py", line 228, in _main
    status = self.run(options, args)
  File "/usr/local/python3/lib/python3.8/site-packages/pip/_internal/cli/req_command.py", line 182, in wrapper
    return func(self, options, args)
  File "/usr/local/python3/lib/python3.8/site-packages/pip/_internal/commands/install.py", line 323, in run
    requirement_set = resolver.resolve(
  File "/usr/local/python3/lib/python3.8/site-packages/pip/_internal/resolution/legacy/resolver.py", line 183, in resolve
    discovered_reqs.extend(self._resolve_one(requirement_set, req))
  File "/usr/local/python3/lib/python3.8/site-packages/pip/_internal/resolution/legacy/resolver.py", line 388, in _resolve_one
    abstract_dist = self._get_abstract_dist_for(req_to_install)
  File "/usr/local/python3/lib/python3.8/site-packages/pip/_internal/resolution/legacy/resolver.py", line 340, in _get_abstract_dist_for
    abstract_dist = self.preparer.prepare_linked_requirement(req)
  File "/usr/local/python3/lib/python3.8/site-packages/pip/_internal/operations/prepare.py", line 482, in prepare_linked_requirement
    abstract_dist = _get_prepared_distribution(
  File "/usr/local/python3/lib/python3.8/site-packages/pip/_internal/operations/prepare.py", line 91, in _get_prepared_distribution
    abstract_dist.prepare_distribution_metadata(finder, build_isolation)
  File "/usr/local/python3/lib/python3.8/site-packages/pip/_internal/distributions/sdist.py", line 38, in prepare_distribution_metadata
    self._setup_isolation(finder)
  File "/usr/local/python3/lib/python3.8/site-packages/pip/_internal/distributions/sdist.py", line 96, in _setup_isolation
    reqs = backend.get_requires_for_build_wheel()
  File "/usr/local/python3/lib/python3.8/site-packages/pip/_vendor/pep517/wrappers.py", line 160, in get_requires_for_build_wheel
    return self._call_hook('get_requires_for_build_wheel', {
  File "/usr/local/python3/lib/python3.8/site-packages/pip/_vendor/pep517/wrappers.py", line 265, in _call_hook
    raise BackendUnavailable(data.get('traceback', ''))
pip._vendor.pep517.wrappers.BackendUnavailable: Traceback (most recent call last):
  File "/usr/local/python3/lib/python3.8/site-packages/pip/_vendor/pep517/_in_process.py", line 86, in _build_backend
    obj = import_module(mod_path)
  File "/usr/local/python3/lib/python3.8/importlib/__init__.py", line 127, in import_module
    return _bootstrap._gcd_import(name[level:], package, level)
  File "<frozen importlib._bootstrap>", line 1014, in _gcd_import
  File "<frozen importlib._bootstrap>", line 991, in _find_and_load
  File "<frozen importlib._bootstrap>", line 961, in _find_and_load_unlocked
  File "<frozen importlib._bootstrap>", line 219, in _call_with_frames_removed
  File "<frozen importlib._bootstrap>", line 1014, in _gcd_import
  File "<frozen importlib._bootstrap>", line 991, in _find_and_load
  File "<frozen importlib._bootstrap>", line 975, in _find_and_load_unlocked
  File "<frozen importlib._bootstrap>", line 671, in _load_unlocked
  File "<frozen importlib._bootstrap_external>", line 783, in exec_module
  File "<frozen importlib._bootstrap>", line 219, in _call_with_frames_removed
  File "/tmp/pip-build-env-sme_dbro/overlay/lib/python3.8/site-packages/setuptools/__init__.py", line 18, in <module>
    from setuptools.dist import Distribution
  File "/tmp/pip-build-env-sme_dbro/overlay/lib/python3.8/site-packages/setuptools/dist.py", line 38, in <module>
    from setuptools import windows_support
  File "/tmp/pip-build-env-sme_dbro/overlay/lib/python3.8/site-packages/setuptools/windows_support.py", line 2, in <module>
    import ctypes
  File "/usr/local/python3/lib/python3.8/ctypes/__init__.py", line 7, in <module>
    from _ctypes import Union, Structure, Array
ModuleNotFoundError: No module named '_ctypes'

WARNING: You are using pip version 20.2.3; however, version 21.1.3 is available.
You should consider upgrading via the '/usr/local/python3/bin/python3.8 -m pip install --upgrade pip' command.

~~~

我执行了如下指令：

~~~

yum install libffi-devel -y
make clean && make && make install

~~~

因为环境变量之前在第一次编译安装时我已经配置过了，所以这块不需要进行任何配置。

## 参考资料

1. [ModuleNotFoundError: No module named '_ctypes'的解决方案](https://www.cnblogs.com/fanbi/p/12375023.html)