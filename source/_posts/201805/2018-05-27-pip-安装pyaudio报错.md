---
title: "Cannot install pyaudio, gcc error"
date: 2018-05-27 23:13:34 +0800

categories: 一个问题
tags: [python, 问题集]
---

>本地环境
```
Python 3.6.4
```

<mark>问题描述</mark>

```
$ pip install PyAudio                                      1 ↵
Collecting PyAudio
  Using cached https://files.pythonhosted.org/packages/ab/42/b4f04721c5c5bfc196ce156b3c768998ef8c0ae3654ed29ea5020c749a6b/PyAudio-0.2.11.tar.gz
Building wheels for collected packages: PyAudio
  Running setup.py bdist_wheel for PyAudio ... error
  Complete output from command /home/itaken/.pyenv/versions/3.6.4/bin/python3.6 -u -c "import setuptools, tokenize;__file__='/tmp/pip-install-mllu4p29/PyAudio/setup.py';f=getattr(tokenize, 'open', open)(__file__);code=f.read().replace('\r\n', '\n');f.close();exec(compile(code, __file__, 'exec'))" bdist_wheel -d /tmp/pip-wheel-s0ksir29 --python-tag cp36:
  running bdist_wheel
  running build
  running build_py
  creating build
  creating build/lib.linux-x86_64-3.6
  copying src/pyaudio.py -> build/lib.linux-x86_64-3.6
  running build_ext
  building '_portaudio' extension
  creating build/temp.linux-x86_64-3.6
  creating build/temp.linux-x86_64-3.6/src
  gcc -pthread -Wno-unused-result -Wsign-compare -DNDEBUG -g -fwrapv -O3 -Wall -Wstrict-prototypes -fPIC -I/home/itaken/.pyenv/versions/3.6.4/include/python3.6m -c src/_portaudiomodule.c -o build/temp.linux-x86_64-3.6/src/_portaudiomodule.o
  src/_portaudiomodule.c:29:23: fatal error: portaudio.h: 没有那个文件或目录
   #include "portaudio.h"
                         ^
  compilation terminated.
  error: command 'gcc' failed with exit status 1

  ----------------------------------------
  Failed building wheel for PyAudio
  Running setup.py clean for PyAudio
Failed to build PyAudio
Installing collected packages: PyAudio
  Running setup.py install for PyAudio ... error
    Complete output from command /home/itaken/.pyenv/versions/3.6.4/bin/python3.6 -u -c "import setuptools, tokenize;__file__='/tmp/pip-install-mllu4p29/PyAudio/setup.py';f=getattr(tokenize, 'open', open)(__file__);code=f.read().replace('\r\n', '\n');f.close();exec(compile(code, __file__, 'exec'))" install --record /tmp/pip-record-3vodewqa/install-record.txt --single-version-externally-managed --compile:
    running install
    running build
    running build_py
    creating build
    creating build/lib.linux-x86_64-3.6
    copying src/pyaudio.py -> build/lib.linux-x86_64-3.6
    running build_ext
    building '_portaudio' extension
    creating build/temp.linux-x86_64-3.6
    creating build/temp.linux-x86_64-3.6/src
    gcc -pthread -Wno-unused-result -Wsign-compare -DNDEBUG -g -fwrapv -O3 -Wall -Wstrict-prototypes -fPIC -I/home/itaken/.pyenv/versions/3.6.4/include/python3.6m -c src/_portaudiomodule.c -o build/temp.linux-x86_64-3.6/src/_portaudiomodule.o
    src/_portaudiomodule.c:29:23: fatal error: portaudio.h: 没有那个文件或目录
     #include "portaudio.h"
                           ^
    compilation terminated.
    error: command 'gcc' failed with exit status 1

    ----------------------------------------
Command "/home/itaken/.pyenv/versions/3.6.4/bin/python3.6 -u -c "import setuptools, tokenize;__file__='/tmp/pip-install-mllu4p29/PyAudio/setup.py';f=getattr(tokenize, 'open', open)(__file__);code=f.read().replace('\r\n', '\n');f.close();exec(compile(code, __file__, 'exec'))" install --record /tmp/pip-record-3vodewqa/install-record.txt --single-version-externally-managed --compile" failed with error code 1 in /tmp/pip-install-mllu4p29/PyAudio/
```

<mark>解决方法</mark>

```
sudo apt-get install libasound-dev portaudio19-dev libportaudio2 libportaudiocpp0
sudo apt-get install ffmpeg libav-tools
sudo pip install pyaudio
```

---
## 参考文档
- [Cannot install pyaudio, gcc error](https://stackoverflow.com/questions/20023131/cannot-install-pyaudio-gcc-error?utm_medium=organic&utm_source=google_rich_qa&utm_campaign=google_rich_qa)
