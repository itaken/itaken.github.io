---
title: "如何查看windows 的ubuntu子系统目录"
date: 2017-09-30 12:27:56 +0800
img: /assets/images/201709/30-01.png

categories: 一个问题
tags: [ubuntu, bash, windows, 问题集]
---

>个人环境配置
```
OS 名称: Microsoft Windows 10 企业版
OS 版本: 10.0.15063 Build 15063
```

<mark>问题描述</mark>

根据网络搜索结果, ubuntu子系统应该在`C:\Users\{user}\AppData\Local\lxss\`目录下, 但是在`Local`目录下没有发现`lxss`文件夹.

<mark>解决方法</mark>

`cd` 到 Local目录下, 然后执行 `attrib -s -h lxss`即可.

```bash
C:\Users\user\AppData\Local>attrib -s -h lxss
```

![目录文件夹](/assets/images/201709/30-01.png)

---
## 参考文档
- [WIndows directory of the linux subsystem](https://github.com/Microsoft/BashOnWindows/issues/402)
- [Where is the Ubuntu file system root directory in Windows NT subsystem and vice versa?](https://askubuntu.com/questions/759880/where-is-the-ubuntu-file-system-root-directory-in-windows-nt-subsystem-and-vice)
