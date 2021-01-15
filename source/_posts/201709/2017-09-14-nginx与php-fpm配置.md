---
title: "Ubuntu nginx与php-fpm配置, 无法解析php文件"
date: 2017-09-14 16:16:14 +0800
img: /assets/images/201709/14-02.png

categories: 一个问题
tags: [ubuntu, nginx, php, 问题集]
---

<mark>问题描述</mark>

使用nginx作为web服务器,php-fpm作为php处理器.

修改`/usr/local/nginx/conf/nginx.conf`配置文件,添加php解析配置.

```
location ~ \.php$ {
    root           /var/www/html;
    ## With php7.0-cgi alone:
    # fastcgi_pass   127.0.0.1:9000;
    # With php7.0-fpm:
    fastcgi_pass   unix:/run/php/php7.0-fpm.sock;
    #fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
    include        fastcgi.conf;
    #fastcgi_index  index.php;
    include        fastcgi_params;
}
```

查看`php-fpm`和`nginx`的服务状态

![服务状态](/assets/images/201709/14-02.png)

发现**服务正常**,但是打开php文件,无法解析而是直接变成了下载, 目录下的index.php则是提示出错了.

![出错了](/assets/images/201709/14-03.png)

查看nginx的`/usr/local/nginx/logs/error.log`错误日志, 发现`nginx`与`php-fpm`通信失败:

```
cat /usr/local/nginx/logs/error.log
...
2017/09/14 15:59:14 [crit] 27323#0: *1 connect() to unix:/run/php/php7.0-fpm.sock failed (13: Permission denied) while connecting to upstream, client: 127.0.0.1, server: localhost, request: "GET /phpmyadmin/ HTTP/1.1", upstream: "fastcgi://unix:/run/php/php7.0-fpm.sock:", host: "localhost"
2017/09/14 16:01:50 [crit] 27323#0: *5 connect() to unix:/run/php/php7.0-fpm.sock failed (13: Permission denied) while connecting to upstream, client: 127.0.0.1, server: localhost, request: "GET /phpmyadmin/ HTTP/1.1", upstream: "fastcgi://unix:/run/php/php7.0-fpm.sock:", host: "localhost"
```

<mark>解决方法</mark>

查看`/run/php/php7.0-fpm.sock`文件的权限,发现只有`www-data`用户和`www-data`用户组拥有读写的功能:

```bash
/run/php $ ll
总用量 4.0K
-rw-r--r-- 1 root     root     5 9月  14 15:58 php7.0-fpm.pid
srw-rw---- 1 www-data www-data 0 9月  14 15:58 php7.0-fpm.sock
```

发现nginx运行的用户是`root`,或者`nobody`, 非`www-data`.

```bash
$ ps aux | grep nginx
root      3570  0.0  0.0  24984   444 ?        Ss   16:30   0:00 nginx: master process /usr/bin/nginx
nobody    3572  0.0  0.0  25452  3316 ?        S    16:30   0:00 nginx: worker process
```

那么,问题简单了,nginx用`www-data`用户运行,或者修改`/run/php/php7.0-fpm.sock`文件的权限,我选择了后者, 直接修改sock文件的权限即可.

```bash
/run/php $ sudo chmod 666 php7.0-fpm.sock
/run/php $ ll
总用量 4.0K
-rw-r--r-- 1 root     root     5 9月  14 15:58 php7.0-fpm.pid
srw-rw-rw- 1 www-data www-data 0 9月  14 15:58 php7.0-fpm.sock
```

---
## 参考文档
- [HOW TO INSTALL PHP 7.0 (PHP-FPM) ON UBUNTU 16.04 LTS (XENIAL XERUS)](https://www.tqhosting.com/kb/464/How-to-install-PHP-70-PHP-FPM-on-Ubuntu-1604-LTS-Xenial-Xerus.html)
- [nginx和php-fpm基础环境的安装和配置](https://segmentfault.com/a/1190000003067656)
