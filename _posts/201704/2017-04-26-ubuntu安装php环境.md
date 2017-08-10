---
layout: post

title: "ubuntu 安装PHP 7.0环境 (LAMP环境配置)"
date: 2017-04-26 19:01:41 +0800

categories: php
tags: [ubuntu, php, lamp, apache, mysql]
---

1. 安装 `apache`
    ```bash
    $ sudo apt-get install apache2
    ```
    安装完毕，默认自动启动，可以使用`$ sudo /etc/init.d/apache2 restart`命令，进行重启。

1. 安装`php`
    可以选择安装最新版本 **php7**, 以及连接模块。
    >由于 Ubuntu Server 官方更新实在太慢，导致很多时候没法打上最新的安全补丁，我们这里推荐 [Ondřej Surý](https://deb.sury.org/) 的 [PPA](https://launchpad.net/~ondrej/+archive/ubuntu/php)

    ```bash
    $ sudo add-apt-repository ppa:ondrej/php
    $ sudo apt-get update
    $ sudo apt-get install libapache2-mod-php7.0 php7.0
    ```
    需要重启apache进行加载。
    >如果您的程序需要额外的 PHP 组件，可以通过 `apt-cache search php7.0` 命令来查找。

1. 安装`mysql` 5.7.x
    >经过多年生产环境的测试，我们推荐使用 [Percona Server](https://www.percona.com/software/mysql-database/percona-server) 代替原生的 MySQL

    ```bash
    $ wget https://repo.percona.com/apt/percona-release_0.1-4.$(lsb_release -sc)_all.deb
    $ sudo dpkg -i percona-release_0.1-4.$(lsb_release -sc)_all.deb
    $ sudo apt-get update
    $ sudo apt-get install percona-server-server-5.7
    ```
    >这里强烈推荐安装完 MySQL 后，执行一次安全设置，很简单的一条命令`mysql_secure_installation`执行后会让您选择密码强度，一般情况下选择 1 或者 **2**

1. 安装`phpmyadmin`
    web版数据库管理首推就是 **phpmyadmin**
    ```bash
    $ sudo apt-get install phpmyadmin
    ```
    **web server** 建议使用 `apache2`。
    在浏览器中打开`http://localhost/phpmyadmin` 进行数据库管理。

1. 设置项目目录权限
    ```bash
    $ chmod 777 /var/www/html
    ```

---
### 更多阅读
- [Ubuntu Server 16.04.x (Xenial Xerus) 安装 LEMP / LNMP 教程](https://segmentfault.com/a/1190000009330496)
- [ubuntu下安装Apache+PHP+Mysql](http://www.cnblogs.com/lynch_world/archive/2012/01/06/2314717.html)
