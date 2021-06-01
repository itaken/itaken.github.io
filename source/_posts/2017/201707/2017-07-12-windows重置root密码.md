---
title: "windows 重置MySQL root密码"
date: 2017-07-12 14:29:56 +0800

categories: 开发日常
tags: [windows, mysql, 问题集]
---

<mark>问题描述</mark>

>直接使用mysql压缩包解压, 使用root账号无法登录.

```bash
$mysql -u root
Enter password:   

ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: NO)  
```

<mark>解决方法</mark>

1. 在服务中关闭 **mysql服务**

1. 打开 **cmd**, 输入: `mysqld --skip-grant-tables`

1. 打开__新cmd__, 直接使用 **root** 账号登录, `mysql -u root`

1. 重置root密码

    ```bash
    mysql> update mysql.user set password=PASSWORD('root') where user='root';  
    Query OK, 2 rows affected (0.00 sec)  
    Rows matched: 2  Changed: 2  Warnings: 0  

    mysql> flush privileges;  
    Query OK, 0 rows affected (0.00 sec)
    ```

1. 关闭cmd, 重新开启mysql 即可.

---
## 参考文档
- [MySQL 修改用户密码及重置root密码](http://blog.csdn.net/leshami/article/details/39805839)
