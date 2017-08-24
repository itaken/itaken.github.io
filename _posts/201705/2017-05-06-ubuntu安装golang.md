---
layout: post

title: "ubuntu 安装golang"
date: 2017-05-06 15:47:50 +0800

categories: post
tags: [ubuntu, golang]
---

1. 直接下载[**golang**安装包](https://golang.org/dl/), 或者使用`wget`或`aria2c`下载安装包.

    ```bash
    $ aria2c -c https://storage.googleapis.com/golang/go1.8.1.linux-amd64.tar.gz
    ```
    >**aria2c** 可以使用`sudo apt-get install aria2`命令直接安装, 国内可以使用**代理**下载
    ```bash
    $ aria2c -c --http-proxy=xxx https://storage.googleapis.com/golang/go1.8.1.linux-amd64.tar.gz
    ```
    >

1. 直接解压到`/usr/local/` 目录下

    ```bash
    $ sudo tar -C /usr/local -xzf go1.8.1.linux-amd64.tar.gz
    ```

1. 在`~/.profile`文件 **PATH** 中,添加`/usr/local/go/bin`路径

    查看 **version**,确定安装成功
    ```bash
    $ go version
    go version go1.8.1 linux/amd64
    ```

---
### 更多阅读
- [Getting Started](https://golang.org/doc/install?download=go1.8.1.linux-amd64.tar.gz)
