---
title: "MySQL server has gone away"
date: 2017-05-02 11:39:35 +0800

categories: 一个问题
tags: [mysql, 问题集]
---

<mark>问题描述</mark>

>mysql报错 `2006, MySQL server has gone away`

<mark>解决方法</mark>

1. 修改 `/etc/mysql/mysql.conf.d/mysqld.cnf` 配置 [^2]
    - 设置`wait_timeout`
    >例如: 设置`wait_timeout=180`

    - 设置`max_allowed_packet`

    ```apache
    [mysqld]
    max_allowed_packet=16M
    ```
    - 使用 `mysqli_ping` [^1]


1. 重启 **mysql**
    ```bash
    $ sudo /etc/init.d/mysql restart
    ```

---
## 参考文档
- [MySQL: "Warning: MySQL server has gone away"](https://www.drupal.org/node/259580)
- [How do I fix the error “Mysql Server has gone away”?](https://piwik.org/faq/troubleshooting/faq_183/)
- [MySQL error 2006: mysql server has gone away](http://stackoverflow.com/questions/7942154/mysql-error-2006-mysql-server-has-gone-away)
- [B.5.2.9 MySQL server has gone away](https://dev.mysql.com/doc/refman/5.7/en/gone-away.html)
- [Why /etc/mysql/my.cnf is EMPTY?](https://askubuntu.com/questions/699903/why-etc-mysql-my-cnf-is-empty)
- [mysqli::ping](http://php.net/manual/en/mysqli.ping.php)
- [Ubuntu MySQL](https://help.ubuntu.com/lts/serverguide/mysql.html)


[^1]: http://php.net/manual/en/mysqli.ping.php
[^2]: https://help.ubuntu.com/lts/serverguide/mysql.html
