---
title: "jupyter可以启动,但是无法连接kernel的解决方案"
date: 2021-05-23 18:26:34 +0800
img: /assets/images/202105/23-01.png

categories: 一个问题
tags: [jupyter, 问题集]
---

<mark>问题描述</mark>

使用`pip`安装**jupyter**,可以打开web,但是无法连接kernel,无法使用.

错误示例:
```
Traceback (most recent call last):
  File "/path/to/user/.local/lib/python3.8/site-packages/tornado/web.py", line 1704, in _execute
    result = await result
  File "/path/to/user/.local/lib/python3.8/site-packages/tornado/gen.py", line 769, in run
    yielded = self.gen.throw(*exc_info)  # type: ignore
  ....
  File "/path/to/user/.local/lib/python3.8/site-packages/jupyter_core/paths.py", line 471, in secure_write
    raise RuntimeError("Permissions assignment failed for secure file: '{file}'."
RuntimeError: Permissions assignment failed for secure file: '/path/to/user/.local/share/jupyter/runtime/kernel-3538cdde-8a9a-4766-bc48-9b3c32a30dd2.json'. Got '0o677' instead of '0o0600'.
```

![jupyter无法运行](/assets/images/202105/23-01.png)

查看`tornado`,发现是**6.1**的版本, 依据网络上查到的**降低**tornado版本, 但是没有效果!
```
itaken@itaken-home:~$ pip list | grep torna
tornado                           6.1
itaken@itaken-home:~$ pip list | grep note
notebook                          6.4.0
```

`pip install tornado==5.1.1`提示`notebook 6.4.0 requires tornado>=6.1`, 无法降低版本:

```
itaken@itaken-home:~$ pip install tornado==5.1.1
Defaulting to user installation because normal site-packages is not writeable
Collecting tornado==5.1.1
  Downloading tornado-5.1.1.tar.gz (516 kB)
     |████████████████████████████████| 516 kB 1.2 MB/s
...
      Successfully uninstalled tornado-6.1
ERROR: pip's dependency resolver does not currently take into account all the packages that are installed. This behaviour is the source of the following dependency conflicts.
notebook 6.4.0 requires tornado>=6.1, but you have tornado 5.1.1 which is incompatible.
jupyter-server 1.6.4 requires tornado>=6.1.0, but you have tornado 5.1.1 which is incompatible.
Successfully installed tornado-6.1
```

即使强制降低了`tornado`版本,也是无法启动`notebook`:

```
itaken@itaken-home:~$ /usr/local/bin/python3 /path/to/user/.local/bin/jupyter notebook --notebook-dir="/path/to/user/jupyter-data" --allow-root
Traceback (most recent call last):
  File "/path/to/user/.local/bin/jupyter-notebook", line 5, in <module>
    from notebook.notebookapp import main
  File "/path/to/user/.local/lib/python3.8/site-packages/notebook/notebookapp.py", line 58, in <module>
    raise ImportError(_("The Jupyter Notebook requires tornado >= 5.0, but you have %s") % tornado.version)
ImportError: The Jupyter Notebook requires tornado >= 5.0, but you have 4.5.3
```

```
itaken@itaken-home:~$ /usr/local/bin/python3 /path/to/user/.local/bin/jupyter notebook --notebook-dir="/path/to/user/jupyter-data" --allow-root
[W 15:26:27.188 NotebookApp] Config option `runtime_dir` not recognized by `NotebookApp`.  Did you mean `notebook_dir`?
[W 15:26:29.170 NotebookApp] WARNING: The notebook server is listening on all IP addresses and not using encryption. This is not recommended.
...
[I 15:26:34.131 NotebookApp] 302 GET / (127.0.0.1) 2.110000ms
[W 15:26:41.407 NotebookApp] Config option `template_path` not recognized by `ExporterCollapsibleHeadings`.  Did you mean one of: `extra_template_paths, template_name, template_paths`?
[W 15:26:41.466 NotebookApp] Config option `template_path` not recognized by `ExporterCollapsibleHeadings`.  Did you mean one of: `extra_template_paths, template_name, template_paths`?
...
```

>问题: 因为安装了多个版本tornado, 重新安装`tornado 6.1`,在安装jupyter其他库,提示`No such file or directory`错误:

```
itaken@itaken-home:~$ pip install jupyter_contrib_nbextensions
Defaulting to user installation because normal site-packages is not writeable
Requirement already satisfied: jupyter_contrib_nbextensions in ./.local/lib/python3.8/site-packages (0.5.1)
...
Requirement already satisfied: ptyprocess in ./.local/lib/python3.8/site-packages (from terminado>=0.8.3->notebook>=4.0->jupyter_contrib_nbextensions) (0.7.0)
ERROR: Could not install packages due to an OSError: [Errno 2] No such file or directory: '/path/to/user/.local/lib/python3.8/site-packages/tornado-6.1.dist-info/METADATA'
```

直接使用`pip uninstall tornado`,`pip install tornado`重新安装失败, 需要`rm -rf /path/to/user/.local/lib/python3.8/site-packages/tornado-6.1.dist-info` 删除整个`tornado-6.1.dist-info`目录,然后在重新安装即可.

```
itaken@itaken-home:~$ pip uninstall tornado
Found existing installation: tornado 6.1
ERROR: Exception:
Traceback (most recent call last):
  File "/path/to/user/.local/lib/python3.8/site-packages/pip/_internal/cli/base_command.py", line 180, in _main
    status = self.run(options, args)
  ...
    with open(path, 'rb') as stream:
FileNotFoundError: [Errno 2] No such file or directory: '/path/to/user/.local/lib/python3.8/site-packages/tornado-6.1.dist-info/RECORD'
itaken@itaken-home:~$ pip install tornado
Defaulting to user installation because normal site-packages is not writeable
Collecting tornado
  Using cached tornado-6.1-cp38-cp38-manylinux2010_x86_64.whl (427 kB)
...
Installing collected packages: tornado
Successfully installed tornado-6.1
```

<mark>解决方案</mark>

实际的解决方案是,降低`nbconvert`的版本, 直接使用 `pip install nbconvert==5.6.1` 即可.

```
itaken@itaken-home:~$ pip list | grep nbc
nbclassic                         0.2.7
nbclient                          0.5.3
nbconvert                         6.0.7
```

```
itaken@itaken-home:~$ pip install nbconvert==5.6.1
Defaulting to user installation because normal site-packages is not writeable
Collecting nbconvert==5.6.1
  Downloading nbconvert-5.6.1-py2.py3-none-any.whl (455 kB)
     |████████████████████████████████| 455 kB 1.1 MB/s
Requirement already satisfied: defusedxml in ./.local/lib/python3.8/site-packages (from nbconvert==5.6.1) (0.7.1)
...
Requirement already satisfied: pyparsing>=2.0.2 in ./.local/lib/python3.8/site-packages (from packaging->bleach->nbconvert==5.6.1) (2.4.7)
Installing collected packages: nbconvert
  Attempting uninstall: nbconvert
    Found existing installation: nbconvert 6.0.7
    Uninstalling nbconvert-6.0.7:
      Successfully uninstalled nbconvert-6.0.7
ERROR: pip's dependency resolver does not currently take into account all the packages that are installed. This behaviour is the source of the following dependency conflicts.
notebook 6.4.0 requires tornado>=6.1, which is not installed.
jupyter-server 1.6.4 requires tornado>=6.1.0, which is not installed.
jupyter-nbextensions-configurator 0.4.1 requires tornado, which is not installed.
jupyter-contrib-nbextensions 0.5.1 requires tornado, which is not installed.
jupyter-contrib-core 0.3.3 requires tornado, which is not installed.
Successfully installed nbconvert-5.6.1

itaken@itaken-home:~$ pip list | grep nbc
nbclassic                         0.2.7
nbclient                          0.5.3
nbconvert                         5.6.1
itaken@itaken-home:~$ pip list | grep tor
async-generator                   1.10
decorator                         5.0.9
jupyter-nbextensions-configurator 0.4.1
tornado                           6.1
```

安装notebook拓展插件`nbextension`:

```
itaken@itaken-home:~$ jupyter contrib nbextension install --user
[I 18:21:01 InstallContribNbextensionsApp] jupyter contrib nbextension install --user
[I 18:21:01 InstallContribNbextensionsApp] Installing jupyter_contrib_nbextensions nbextension files to jupyter data directory
...
[I 18:21:02 InstallContribNbextensionsApp] - Validating: OK
[I 18:21:02 InstallContribNbextensionsApp] Installing jupyter_contrib_nbextensions items to config in /path/to/user/.jupyter
Enabling: jupyter_nbextensions_configurator
- Writing config: /path/to/user/.jupyter
    - Validating...
      jupyter_nbextensions_configurator 0.4.1 OK
Enabling notebook nbextension nbextensions_configurator/config_menu/main...
Enabling tree nbextension nbextensions_configurator/tree_tab/main...
```

启动**notebook**,开始python之旅吧!

```
itaken@itaken-home:~$ /usr/local/bin/python3 /path/to/user/.local/bin/jupyter notebook --notebook-dir="/path/to/user/jupyter-data" --allow-root --no-browser
[W 18:21:59.064 NotebookApp] WARNING: The notebook server is listening on all IP addresses and not using encryption. This is not recommended.
[I 18:21:59.239 NotebookApp] [jupyter_nbextensions_configurator] enabled 0.4.1
[I 18:21:59.242 NotebookApp] Serving notebooks from local directory: /path/to/user/jupyter-data
[I 18:21:59.242 NotebookApp] Jupyter Notebook 6.4.0 is running at:
[I 18:21:59.243 NotebookApp] http://192.168.1.3:8888/
[I 18:21:59.243 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[I 18:22:03.375 NotebookApp] 302 GET /tree? (192.168.31.30) 3.270000ms
[I 18:22:07.239 NotebookApp] 302 POST /login?next=%2Ftree%3F (192.168.31.30) 343.300000ms
[I 18:22:21.358 NotebookApp] Kernel started: 3b1b3a9d-3673-1111-a012-986f2b95166c, name: python3
...
```

---
## 参考文档
- [juipiter notebook server "connecting to kernel" problem #2664](https://github.com/jupyter/notebook/issues/2664)
- [本地创建的jupyter notebook 无法连接本地环境（即不能运行代码）](https://www.cnblogs.com/damin1909/p/12691147.html)
- [Jupyter notebook: No connection to server because websocket connection fails](https://stackoverflow.com/questions/54963043/jupyter-notebook-no-connection-to-server-because-websocket-connection-fails)
- [tornado 6.0 breaks notebook #4439](https://github.com/jupyter/notebook/issues/4439)
- [jupyter代码自动补全插件、安装后出现警告“Config option `template_path` not recognized by `LenvsLatexExporter`”的解决方案](https://blog.csdn.net/DTFT_/article/details/111242118)
- [jupyterLab打开后出现Config option `template_path` not recognized by `ExporterCollapsibleHeadings`相关问题](https://blog.csdn.net/outsider2019/article/details/109274996)
