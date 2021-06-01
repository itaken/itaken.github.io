---
title: "spacemacs 如何在顶部标题栏显示文件路径"
date: 2018-04-10 00:23:38 +0800
img: /assets/images/201804/10-01.png

categories: 一个问题
tags: [emacs, 问题集]
---

<mark>问题描述</mark>

如何在标题栏显示文件路径?

<mark>解决方法</mark>

使用网络上的方法,添加相关代码到配置项即可.

`SPACE + f e + d` 打开配置,添加如下代码

```
;; Disable loading of “default.el” at startup,
;; in Fedora all it does is fix window title which I rather configure differently
(setq inhibit-default-init t)

;; SHOW FILE PATH IN FRAME TITLE
(setq-default frame-title-format "%b (%f)")
```

![spacemacs](/assets/images/201804/10-01.png)


---
## 参考文档
- [Display path of file in status bar](https://stackoverflow.com/questions/2903426/display-path-of-file-in-status-bar?utm_medium=organic&utm_source=google_rich_qa&utm_campaign=google_rich_qa)
