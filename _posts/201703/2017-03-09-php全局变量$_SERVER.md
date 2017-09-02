---
layout: post

title: "PHP $_SERVER变量"
date: 2017-03-09 15:27:08 +0800

categories: php
tags: [php, 网络安全]
---

>在[GPC][1]开启的情况下$\_SERVER是不受GPC影响的。

1. $\_SERVER['**SERVER_NAME**'] `当前运行脚本所在的服务器的主机名`

    ```
    $_SERVER['SERVER_NAME'] 在apache2 下
    如果你没有设置ServerName或者没有把UseCanonicalName 设置为 On的话，
    这个值就会是客户端提供的hostname ，对于apache来说，它只取第一个host，
    但对于php来说，不管多少个，它全要了。
    ```
1. $\_SERVER['**HTTP_X_FORWARDED_FOR**'] `浏览当前页面的用户计算机的网关` [^1]

    - 没有使用代理服务 器的情况：
        REMOTE_ADDR = 您的 IP
        HTTP_VIA = 没数值或不显示
        HTTP_X_FORWARDED_FOR = 没数值或不显示

    - 使用透明代理服务器的情 况：Transparent Proxies
        REMOTE_ADDR = 最后一个代理服务器 IP
        HTTP_VIA = 代理服务器 IP
        HTTP_X_FORWARDED_FOR = 您的真实 IP ，经过多个代理服务器时，这个值类似如下：203.98.182.163, 203.98.182.163, 203.129.72.215。
        这类代理服务器还是将您的信息转发给您的访问对象，无法达到隐藏真实身份的目的。

    - 使用普通匿名代理服务器的情况：Anonymous Proxies
        REMOTE_ADDR = 最后一个代理服务器 IP
        HTTP_VIA = 代理服务器 IP
        HTTP_X_FORWARDED_FOR = 代理服务器 IP ，经过多个代理服务器时，这个值类似如下：203.98.182.163, 203.98.182.163, 203.129.72.215。
        隐藏了您的真实IP，但是向访问对象透露了您是使用代理服务器访问他们的。

    - 使用欺骗性代理服务器的情况：Distorting Proxies
        REMOTE_ADDR = 代理服务器 IP
        HTTP_VIA = 代理服务器 IP
        HTTP_X_FORWARDED_FOR = 随机的 IP ，经过多个代理服务器时，这个值类似如下：203.98.182.163, 203.98.182.163, 203.129.72.215。
        告诉了访问对象您使用了代理服务器，但编造了一个虚假的随机IP代替您的真实IP欺骗它。

    - 使用高匿名代理服务器的情况：High Anonymity Proxies (Elite proxies)
        REMOTE_ADDR = 代理服务器 IP
        HTTP_VIA = 没数值或不显示
        HTTP_X_FORWARDED_FOR = 没数值或不显示。
        完全用代理服务器的信息替代了您的所有信息，就象您就是完全使用那台代理服务器直接访问对象。

1. $\_SERVER['PHP_SELF'] `当前执行脚本的文件名`


1. $\_SERVER['HTTP_USER_AGENT'] `返回客户端ua`

---
### 更多阅读
- [浅谈审计中如何快速定位引入单引号的地方](http://www.am0s.com/codesec/229.html)
- [$_SERVER](http://php.net/manual/en/reserved.variables.server.php)
- [What is the difference between HTTP_CLIENT_IP and HTTP_X_FORWARDED_FOR?](http://stackoverflow.com/questions/7445592/what-is-the-difference-between-http-client-ip-and-http-x-forwarded-for)

---
### 索引

[^1]: [php中HTTP_X_FORWARDED_FOR 和 REMOTE_ADDR的使用](http://www.cnblogs.com/andhm/archive/2010/12/18/1910030.html)

[1]: http://php.net/manual/zh/security.magicquotes.what.php
