---
title: "在windows 10中开启bash"
date: 2017-09-29 10:36:51 +0800
img: /assets/images/201709/29-01.png

categories: 一个问题
tags: [ubuntu, bash, windows, 问题集]
---

>个人环境配置
```
OS 名称: Microsoft Windows 10 企业版
OS 版本: 10.0.15063 Build 15063
```

## 在主页**更新和安全** 中选中**开发人员模式**

第一次开启需要联网,开启开发人员模式需要下载一些东西

![开发人员模式](/assets/images/201709/29-01.png)

## 在主页**应用** 中,点击打开**应用和功能**底部的**程序和功能**

![程序和功能](/assets/images/201709/29-02.png)

### 选中**启动和关闭windows功能**, 勾选**适用于linux 的windows的子系统**, 重启

![适用于linux 的windows的子系统](/assets/images/201709/29-03.png)

![重启](/assets/images/201709/29-04.png)

## cmd 输入 **bash**

>需要联网, 需要下载应用

![cmd](/assets/images/201709/29-05.png)

![bash](/assets/images/201709/29-06.png)

---
## 参考文档
- [在调试器里看Windows 10的Linux子系统](http://www.sohu.com/a/146088487_539864)
- [Win10安装Ubuntu子系统教程（附安装图形化界面）](https://www.windows10.pro/bash-on-ubuntu-on-windows/)
- [Windows10安装Linux bash（亲试）](http://blog.csdn.net/grey_csdn/article/details/77116021)
- [The Ubuntu Sub System (New Bash Shell) in Windows 10](https://helloacm.com/the-ubuntu-sub-system-new-bash-shell-in-windows-10/)
