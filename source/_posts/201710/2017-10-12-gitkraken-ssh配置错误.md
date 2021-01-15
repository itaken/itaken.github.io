---
title: "Failed to authenticate SSH session."
date: 2017-10-12 09:56:52 +0800
image: true

categories: 一个问题
tags: [git, 问题集]
---

<mark>问题描述</mark>

使用[gitkraken](https://www.gitkraken.com/) `git pull` 拉取更新内容错误, 提示: `Failed to authenticate SSH session: Invalid key data, not base64 encodeed.`

![authenticate error](/assets/images/201710/12-02.png)

因为使用putty 生成的key 在命令行下没有问题, 所以没有怀疑是因为**密钥**问题.

我还以为是密钥配置错误了, 交换公钥私钥, 后提示: `Configured SSH key is in an invalid format.`

![pull error](/assets/images/201710/12-01.png)

<mark>解决方法</mark>

使用`gitkraken`配置自己生成key, 然后根据规则修改密钥即可.

>果然, 还是因为**密钥**的问题. 因为两种生成key的方式不一样, _公钥不一样_.
