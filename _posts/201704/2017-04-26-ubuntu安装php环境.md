---
layout: post

title: "ubuntu 安装PHP 7.0环境"
date: 2017-04-26 19:01:41 +0800

categories: php
tags: [ubuntu, php]
---

1. 安装 `apache`
  ```bash
  $ sudo apt-get install apache2
  ```
  安装完毕，默认自动启动，可以使用`$ sudo /etc/init.d/apache2 restart`命令，进行重启。

1. 安装`php`
  可以选择安装最新版本 **php7**, 以及连接模块。
  ```bash
  $ sudo apt-get install libapache2-mod-php7.0 php7.0
  ```
  需要重启apache进行加载。

1. 安装`mysql`
  ```bash
  $ sudo apt-get install mysql-server mysql-client
  ```

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
更多阅读
- [ubuntu下安装Apache+PHP+Mysql](http://www.cnblogs.com/lynch_world/archive/2012/01/06/2314717.html)
