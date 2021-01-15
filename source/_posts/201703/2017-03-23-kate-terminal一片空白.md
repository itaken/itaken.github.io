---
title: "kate terminal面板无法使用问题处理"
date: 2017-03-23 10:03:21 +0800
img: /assets/images/201703/23-01.png

categories: 一个问题
tags: [terminal, 问题集]
---

<mark>问题描述</mark>

> kate编辑器有`Terminal`面板, 但是安装完毕kate之后,发现该面板一片空白, 无法使用.

<mark>解决方法</mark>

1. 安装`konsole`

    ```bash
    $ sudo apt-get install konsole
    ```
2. 重启`kate`, 即可以直接使用terminal了.
    ![kate terminal](/assets/images/201703/23-01.png)

---
## 参考文档
- [ Reply to topic No terminal in Kate](https://forum.kde.org/viewtopic.php?f=9&t=117640)
- [Teminal does not show in Kate](http://askubuntu.com/questions/650978/teminal-does-not-show-in-kate)
