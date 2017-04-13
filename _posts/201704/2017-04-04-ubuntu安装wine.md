---
layout: post

title: "ubuntu 安装wine"
date: 2017-04-04 19:58:09 +0800

categories: post
tags: [ubuntu, wine]
---

1. 直接 apt-get install
    ```bash
    $ sudo apt-get install wine
    正在读取软件包列表... 完成
    正在分析软件包的依赖关系树
      ...
    ```
    >一般都是稳定版

1. 添加源  add-apt-repository [^1]
    ```bash
    $ sudo add-apt-repository ppa:wine/wine-builds                          100 ↵
     !!! PLEASE NOTE THAT THIS REPOSITORY IS DEPRECATED !!!

    For more information, please see:
      ...

    $ sudo apt update
    命中:1 http://archive.ubuntukylin.com:10006/ubuntukylin xenial InRelease
      ...

    $ sudo apt install wine-staging
    $ sudo apt install winehq-devel wine-stable winetricks
    ```

1. 安装包安装 [^2]
    ```bash
    $ wget http://dl.winehq.org/wine/source/2.0/wine-2.0.tar.bz2
    $ tar xjf wine-2.0.tar.bz2
    $ cd wine-2.0
    $ sudo ./configure --enable-win64
    $ sudo make && sudo make install
    ```
    >32-bit 直接使用 `$ sudo ./configure`

    wine 2.1  ( .xz 文件 [^3] )
    ```bash
    $ tar -xJf wine-2.1.tar.xz
    $ cd wine-2.1
    $ sudo ./configure
    $ sudo make && sudo make install
    ```

---
索引

[^1]: [How to install and configure Wine?](http://askubuntu.com/questions/316025/how-to-install-and-configure-wine)
[^2]: [Installation of Wine 2.0 on Ubuntu, Linux Mint and Debian](http://www.tecmint.com/install-wine-on-ubuntu-and-linux-mint/)
[^3]: [How do I uncompress a tarball that uses .xz?](http://askubuntu.com/questions/92328/how-do-i-uncompress-a-tarball-that-uses-xz)
