---
layout: post

title: "github关联本地创建的git库"
date: 2017-02-19 13:51:08 +0800

categories: post
tags: [github, git]
---

1. 本地创建项目文件夹, 例如: **jekyll-document-to-pdf**

1. 初始化 git 库
    ```bash
    $ cd jekyll-document-to-pdf
    $ git init
    ```

1. 添加改动文件 (添加所有文件)
    ```bash
    $ git add .
    ```

1. 本地提交 git代码库
    ```bash
    $ git commit -m "初次提交"
    $ git push origin master
    ```

    > 如果没有**关联远程库**, 直接**push**, 则会报错

    ```bash
    fatal: 'origin' does not appear to be a git repository
    fatal: Could not read from remote repository.

    Please make sure you have the correct access rights
    and the repository exists.
    ```

1. 在 github上创建 代码库
    ![创建代码库]({{ site.url }}/assets/images/201702/19-01.png)

1. 关联本地库
    ```bash
    $ git remote add origin https://github.com/itaken/jekyll-document-to-pdf.git
    ```

1. 提交, 完毕.
    ```bash
    $ git push -u origin master
    ```
    ![提交代码]({{ site.url }}/assets/images/201702/19-02.png)

---
更多阅读
- [添加远程库](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013752340242354807e192f02a44359908df8a5643103a000)
