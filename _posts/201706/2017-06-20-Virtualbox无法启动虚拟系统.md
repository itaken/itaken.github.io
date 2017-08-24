---
layout: post

title: "virtualbox 报错 Kernel driver not installed 修复"
date: 2017-06-20 15:43:17 +0800
image: true

categories: post
tags: [ubuntu, virtualbox]
---

<mark>问题描述</mark>

今天更新了virtualbox, 导致安装在虚拟机的系统无法打开, 提示**Kernel driver not installed (rc=-1908)** 错误

![virtual error]({{ site.url }}/assets/images/201706/20-01.png)

<mark>解决方法</mark>

重新安装 **virtualbox-dkms** 即可.

```bash
$ sudo apt-get install linux-headers-generic build-essential dkms
$ sudo apt-get remove --purge virtualbox-dkms
$ sudo apt-get install virtualbox-dkms
```

---
### 更多阅读
- [How to fix Virtualbox error “Kernel driver not installed (rc=-1908)” on Ubuntu](http://www.binarytides.com/fix-vbox-kernel-driver-error/)
- [VBox on 14.04: Kernel driver not installed (rc=-1908) \[duplicate\]](https://askubuntu.com/questions/498900/vbox-on-14-04-kernel-driver-not-installed-rc-1908)
