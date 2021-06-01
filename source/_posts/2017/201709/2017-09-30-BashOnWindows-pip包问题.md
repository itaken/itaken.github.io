---
title: "windows的ubuntu子系统pip安装的包无法运行"
date: 2017-09-30 14:02:48 +0800
img: /assets/images/201709/30-02.png

categories: 一个问题
tags: [ubuntu, bash, windows, python, 问题集]
---

>个人环境配置
```
OS 名称: Microsoft Windows 10 企业版
OS 版本: 10.0.15063 Build 15063
Python: 3.5.2
pip: 9.0.1
```

<mark>问题描述</mark>

在windows的ubuntu子系统中,使用`pip`安装了某个包, 提示安装成功了, 但是却无法使用命令行执行.

```bash
regel@DESKTOP-K146JU9:~$ pip3 install you-get
The directory '/home/regel/.cache/pip/http' or its parent directory is not owned by the current user and the cache has been disabled. Please check the permissions and owner of that directory. If executing pip with sudo, you may want sudo's -H flag.
The directory '/home/regel/.cache/pip' or its parent directory is not owned by the current user and caching wheels has been disabled. check the permissions and owner of that directory. If executing pip with sudo, you may want sudo's -H flag.
Collecting you-get
  Downloading you_get-0.4.915-py3-none-any.whl (186kB)
    100% |████████████████████████████████| 194kB 1.3MB/s
Installing collected packages: you-get
Successfully installed you-get-0.4.915
regel@DESKTOP-K146JU9:~$ you-get
you-get：未找到命令
```

<mark>解决方法</mark>

安装 `virtualenv`即可.

```bash
regel@DESKTOP-K146JU9:~$ sudo apt install virtualenv
regel@DESKTOP-K146JU9:~$ virtualenv ENV
```

如果执行`virtualenv ENV`后, python包还是无法运行,则需要**卸载重新安装**.

![virtualenv](/assets/images/201709/30-02.png)

---
## 参考文档
- [Why aren't pip-installed executables available from the command line?](https://superuser.com/questions/1204232/why-arent-pip-installed-executables-available-from-the-command-line)
- [pip installs packages successfully, but executables not found from command line](https://stackoverflow.com/questions/35898734/pip-installs-packages-successfully-but-executables-not-found-from-command-line)
