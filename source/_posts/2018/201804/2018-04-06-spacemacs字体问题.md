---
title: "spacemacs cannot find source code pro"
date: 2018-04-06 20:47:05 +0800
img: /assets/images/201804/06-01.png

categories: 一个问题
tags: [emacs, 问题集]
---

<mark>问题描述</mark>

```
Cannot find any of the specified fonts (Source Code Pro)! Font settings may not be correct.
```

![spacemacs](/assets/images/201804/06-01.png)

<mark>解决方法</mark>

安装字体, 重启`spacemacs` 即可.

```bash
git clone --depth 1 --branch release https://github.com/adobe-fonts/source-code-pro.git ~/.fonts/adobe-fonts/source-code-pro

fc-cache -f -v ~/.fonts/adobe-fonts/source-code-pro
```

```bash
sudo dnf install adobe-source-code-pro-fonts
```

---

>### 如何安装 _spacemacs_ ?
>
```
mv .emacs .emacs.bak
mv .emacs.d .emacs.d.bak
git clone https://github.com/syl20bnr/spacemacs ~/.emacs.d
```
>


---
## 参考文档
- [Add font installation instructions for Linux ](https://github.com/adobe-fonts/source-code-pro/issues/17#issuecomment-8967116)
- [adobe-fonts/source-code-pro](https://github.com/adobe-fonts/source-code-pro/releases/tag/variable-fonts)
- [spacemacs: 安装与配置](https://bitmingw.com/2017/03/02/spacemacs-install-configuration/)
