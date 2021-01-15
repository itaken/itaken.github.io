---
title: "在windows bash中git密钥配置"
date: 2017-09-29 14:51:27 +0800
img: /assets/images/201709/29-07.png

categories: 一个问题
tags: [ubuntu, bash, windows, 问题集]
---

<mark>问题描述</mark>

因为公司git使用ssh的公钥加密传输,而公钥已经做成`id_rsa`文件放在了C盘用户目录`.ssh`下, 使用cmd的时候可以直接提交,但是使用bash的git提交的时候, 提示输入密码.


<mark>解决方法</mark>

在bash的`~`目录下添加`.ssh`文件即可. 因为在c盘已经有目录文件了, 直接创建链接即可.

`ln -s -f /mnt/c/Users/user/.ssh/ ~`

![git ssh](/assets/images/201709/29-07.png)

---
## 参考文档
- [Working with Git on Windows](http://guides.beanstalkapp.com/version-control/git-on-windows.html)
- [Share private SSH keys with Bash on Windows](https://softwareengineering.stackexchange.com/questions/327979/share-private-ssh-keys-with-bash-on-windows)
- [Windows 10 SSH keys](https://stackoverflow.com/questions/31813080/windows-10-ssh-keys)
- [SSH key and the »Windows Subsystem for Linux](https://florianbrinkmann.com/en/3436/ssh-key-and-the-windows-subsystem-for-linux/)
- [SSH to a Linux host from Windows 10](http://www.kevinkuszyk.com/2016/11/24/ssh-to-a-linux-host-from-windows-10/)
- [SHARING SSH KEYS BETWEEN UBUNTU ON WINDOWS AND WINDOWS 10](https://kackman.net/2017/02/14/sharing-ssh-keys-ubuntu-windows/)
