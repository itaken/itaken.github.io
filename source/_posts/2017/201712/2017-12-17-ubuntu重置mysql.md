---
title: "ubuntu 重置mydql"
date: 2017-12-17 21:03:14 +0800

categories: 一个问题
tags: [mysql, 问题集]
---

>环境配置
```
ubuntu 17.04
mysql 5.7.20
```

<mark>问题描述</mark>

今天, mysql连不上了, 不管是否输入密码, 应该是忘记密码了.

```bash
$ mysql -uroot -p
Enter password:
ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: NO)
```

查找到方案1: `mysqladmin`, 然而并没用, 密码并没有.

```bash
$ mysqladmin -u root password itaken123                                    1 ↵
mysqladmin: connect to server at 'localhost' failed
error: 'Access denied for user 'root'@'localhost' (using password: NO)'
```

重新配置mysql, 这个是**可行的**, 但是因为屏幕宽高太小,从而错过了配置密码, 扶额.

```bash
$ sudo dpkg-reconfigure mysql-server-5.7                                  1 ↵
debconf: 无法初始化前端界面：Dialog
debconf: (对话框界面要求屏幕画面必须为至少 13 行高及 31 列宽.)
debconf: 返回前端界面：Readline
Checking if update is needed.
Checking server version.
Running queries to upgrade MySQL server.
Checking system database.
mysql.columns_priv                                 OK
mysql.db                                           OK
mysql.engine_cost                                  OK
```

方法三: 使用`mysqld_safe`, 然而在我的环境中没有效果.

```bash
$ sudo mysqld_safe --skip-grant-tables --skip-networking                  1 ↵
2017-12-17T13:07:48.802498Z mysqld_safe Logging to syslog.
2017-12-17T13:07:48.813166Z mysqld_safe Logging to '/var/log/mysql/error.log'.
2017-12-17T13:07:48.823180Z mysqld_safe Directory '/var/run/mysqld' for UNIX socket file don't exists.
```

<mark>解决方法</mark>

简单粗暴的重装,任何事情都可以使用最简单的方法处理. 幸亏数据有备份.

```bash
$ sudo apt purge mysql-server mysql-server-5.7 mysql-client-5.7 mysql-client-core-5.7 mysql-server-core-5.7 -y
$ sudo apt install mysql-client-5.7 mysql-server-5.7 -y
```

## 参考文档
- [ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/var/run/mysqld/mysqld.sock' (2)](https://stackoverflow.com/questions/11657829/error-2002-hy000-cant-connect-to-local-mysql-server-through-socket-var-run)
