---
layout: post

title: "使用jekyll来管理github pages"
date: 2017-02-16 10:20:01

categories: post
tags: github jekyll gem
---

```
  Ubuntu: 16.10
  git: 2.9.3
```

\1. 将 github pages **clone** 到本地
```shell
git clone https://github.com/itaken/itaken.github.io.git
```

\2. 进入你本地的github pages目录
```shell
$ jekyll new .
```

报错, 提示文件夹不为空:

```
  Conflict: /var/www/git/mine/itaken.github.io exists and is not empty.
```

使用下面语句

```shell
$ jekyll new . --force
```

\3. 开启服务 [^1]

```shell
$ jekyll serve
```

打开: http://127.0.0.1:4000/

\4. 开始书写或修改博客

\5. 提交到 git
```shell
$ git add .
$ git commit -m "提交信息"
$ git push origin master
```

### 参考文档 ###

[jekyll quick start](http://jekyllbootstrap.com/usage/jekyll-quick-start.html)

[jekyll markdown internal links](http://stackoverflow.com/questions/4629675/jekyll-markdown-internal-links)

[Jekyll: How to get markdown parsing inside blocks using Kramdown?](http://stackoverflow.com/questions/22291211/jekyll-how-to-get-markdown-parsing-inside-blocks-using-kramdown)

[^1]: [jekyllrb usage](https://jekyllrb.com/docs/usage/)
