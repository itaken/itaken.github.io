---
title: "ImportError: libcublas.so.8.0: cannot open shared object file: No such file or directory"
date: 2017-11-02 23:53:34 +0800
modified: 2017-11-08 21:41:46 +0800

categories: 一个问题
tags: [python, 问题集]
---

>环境配置
```
ubuntu 17.04
Python 3.5.3
```

<mark>问题描述</mark>

已经安装了 `caffe-cuda`, `libcaffe-cuda-dev`, `libcudart8.0`, `nvidia-cuda-dev` 包,但是还是报错.

```bash
$ python3 /path/to/test.py
Traceback (most recent call last):
  File "/usr/local/lib/python3.5/dist-packages/tensorflow/python/pywrap_tensorflow.py", line 58, in <module>
    from tensorflow.python.pywrap_tensorflow_internal import *
  File "/usr/local/lib/python3.5/dist-packages/tensorflow/python/pywrap_tensorflow_internal.py", line 28, in <module>
    _pywrap_tensorflow_internal = swig_import_helper()
  File "/usr/local/lib/python3.5/dist-packages/tensorflow/python/pywrap_tensorflow_internal.py", line 24, in swig_import_helper
    _mod = imp.load_module('_pywrap_tensorflow_internal', fp, pathname, description)
  File "/usr/lib/python3.5/imp.py", line 242, in load_module
    return load_dynamic(name, filename, file)
  File "/usr/lib/python3.5/imp.py", line 342, in load_dynamic
    return _load(spec)
ImportError: libcublas.so.8.0: cannot open shared object file: No such file or directory

Failed to load the native TensorFlow runtime.

See https://www.tensorflow.org/install/install_sources#common_installation_problems

for some common reasons and solutions.  Include the entire stack trace
above this error message when asking for help.
```

也安装了 `python3-pycuda` , `python3-pycuda-dbg` 但是还是不可以.

<mark>解决方法</mark>

需要安装 `cuda` 和 `cudnn`.

下载`CUDA Toolkit`, 以及 `NVIDIA cuDNN` 并安装.

---

如果提示 `ImportError: libcudnn.so.6: cannot open shared object file: No such file or directory` 错误(如下), 则需要下载安装`cudnn 6.0` (需要**注册**), 如果是其他版本的还是会报错.

```bash
$ python3 /path/to/test.py
Traceback (most recent call last):
  File "/usr/local/lib/python3.5/dist-packages/tensorflow/python/pywrap_tensorflow.py", line 58, in <module>
    from tensorflow.python.pywrap_tensorflow_internal import *
  File "/usr/local/lib/python3.5/dist-packages/tensorflow/python/pywrap_tensorflow_internal.py", line 28, in <module>
    _pywrap_tensorflow_internal = swig_import_helper()
  File "/usr/local/lib/python3.5/dist-packages/tensorflow/python/pywrap_tensorflow_internal.py", line 24, in swig_import_helper
    _mod = imp.load_module('_pywrap_tensorflow_internal', fp, pathname, description)
  File "/usr/lib/python3.5/imp.py", line 242, in load_module
    return load_dynamic(name, filename, file)
  File "/usr/lib/python3.5/imp.py", line 342, in load_dynamic
    return _load(spec)
ImportError: libcudnn.so.6: cannot open shared object file: No such file or directory

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "/path/to/test.py", line 1, in <module>
    import tensorflow as tf
  File "/usr/local/lib/python3.5/dist-packages/tensorflow/__init__.py", line 24, in <module>
    from tensorflow.python import *
  File "/usr/local/lib/python3.5/dist-packages/tensorflow/python/__init__.py", line 49, in <module>
    from tensorflow.python import pywrap_tensorflow
  File "/usr/local/lib/python3.5/dist-packages/tensorflow/python/pywrap_tensorflow.py", line 72, in <module>
    raise ImportError(msg)
ImportError: Traceback (most recent call last):
  File "/usr/local/lib/python3.5/dist-packages/tensorflow/python/pywrap_tensorflow.py", line 58, in <module>
    from tensorflow.python.pywrap_tensorflow_internal import *
  File "/usr/local/lib/python3.5/dist-packages/tensorflow/python/pywrap_tensorflow_internal.py", line 28, in <module>
    _pywrap_tensorflow_internal = swig_import_helper()
  File "/usr/local/lib/python3.5/dist-packages/tensorflow/python/pywrap_tensorflow_internal.py", line 24, in swig_import_helper
    _mod = imp.load_module('_pywrap_tensorflow_internal', fp, pathname, description)
  File "/usr/lib/python3.5/imp.py", line 242, in load_module
    return load_dynamic(name, filename, file)
  File "/usr/lib/python3.5/imp.py", line 342, in load_dynamic
    return _load(spec)
ImportError: libcudnn.so.6: cannot open shared object file: No such file or directory

Failed to load the native TensorFlow runtime.

See https://www.tensorflow.org/install/install_sources#common_installation_problems

for some common reasons and solutions.  Include the entire stack trace
above this error message when asking for help.
```

<mark>问题描述</mark>

安装完`cnda`以及`cdnn`, 运行程序还是报错:

`ImportError: libnvidia-fatbinaryloader.so.384.90: cannot open shared object file: No such file or directory` 错误

<mark>解决方法</mark>

是因为在 `/usr/local` 中没有 `nvidia` 数据, 直接创建超链接即可.

```bash
ln -s /usr/lib/nvidia-384 /usr/local/nvidia-384
```

调试后,运行 **tensorflow** 测试文件,

```python
#!/usr/bin/env python
import tensorflow as tf
hello = tf.constant('Hello, TensorFlow!')
sess = tf.Session()
print(sess.run(hello))
```

可以看到输出接口, 配置成功

```
$ python3 tensorflow_test.py
2017-11-08 21:23:35.237692: I tensorflow/core/platform/cpu_feature_guard.cc:137] Your CPU supports instructions that this TensorFlow binary was not compiled to use: SSE4.1 SSE4.2 AVX AVX2 FMA
2017-11-08 21:23:35.266193: E tensorflow/stream_executor/cuda/cuda_driver.cc:406] failed call to cuInit: CUDA_ERROR_UNKNOWN
2017-11-08 21:23:35.266292: I tensorflow/stream_executor/cuda/cuda_diagnostics.cc:145] kernel driver does not appear to be running on this host (itaken): /proc/driver/nvidia/version does not exist
b'Hello, TensorFlow!'
```

---
## 参考文档
- [CUDA Toolkit Download](https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1704&target_type=debnetwork)
- [NVIDIA cuDNN](https://developer.nvidia.com/cudnn)
- [Outdated Documentation (ImportError: libcudnn.so.6) #12416](https://github.com/tensorflow/tensorflow/issues/12416)
- [cuDNN Download](https://developer.nvidia.com/rdp/cudnn-download#a-collapse7-8)
- [ImportError: libnvidia-fatbinaryloader.so.375.51: cannot open shared object file #9071](https://github.com/tensorflow/tensorflow/issues/9071)
