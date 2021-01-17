---
title: "搭建 phabricator 系统"
date: 2017-11-10 15:12:04 +0800
img: /assets/images/201711/10-01.png

categories: 开发日常
tags: [php]
---

```
GIT地址: https://github.com/phacility/phabricator
官网地址: https://phacility.com/phabricator/
基于Apache 2.0协议, 可自由修改分发
```

![phabricator](/assets/images/201711/10-01.png)

>**不支持** windows 系统, 所以需要在 linux 下搭建, 官方教程地址: [https://secure.phabricator.com/book/phabricator/article/installation_guide/](https://secure.phabricator.com/book/phabricator/article/installation_guide/)

---

* 搭建 **LAMP**(Apache+mysql+php), 或者 **LNMP**(Nginx+mysql+php) 环境,  开启 `ssl` 和 `rewrite`
* 安装 `git`, `git-core`, php插件 `php-curl`,  `php-apcu` (可选), `php-soap`, `php-cli`
* 定位到需要搭建的目录, 从 **github** 上拉取源代码

```bash
$ cd phabricator
phabricator/ $ git clone https://github.com/phacility/libphutil.git
phabricator/ $ git clone https://github.com/phacility/arcanist.git
phabricator/ $ git clone https://github.com/phacility/phabricator.git
```

* 配置 `vhost`,  以**apache**为例:

```
<VirtualHost *:80>
    ServerAdmin regelhh@gmail.com
    ServerName phabricator.local
    DocumentRoot /paht/to/phabricator/phabricator/webroot
    RewriteEngine on
    RewriteRule ^/rsrc/(.*)     -                       [L,QSA]
    RewriteRule ^/favicon.ico   -                       [L,QSA]
    RewriteRule ^(.*)$          /index.php?__path__=$1  [B,L,QSA]
    <Directory "/paht/to/phabricator/phabricator/webroot">
        Order allow,deny
        Allow from all
    </Directory>
</VirtualHost>
```

配置完毕后, 重启apache, 访问 `http://phabricator.local` 如果能够访问即添加成功.

* 导入数据到数据库, 可以直接使用 内置的方法实现.

```bash
phabricator/ $ ./bin/config set mysql.pass <password>
phabricator/ $ ./bin/storage upgrade
```

在 `./bin/config` 可以设置很多系统配置,  例如: **mysql.host** 设置mysql**主机**地址, **mysql.user** 设置mysql用户名,默认是**root**, **mysql.pass** 设置mysql密码,默认是**空**

![bin/config](/assets/images/201711/10-02.png)

访问 `http://phabricator.local` 即可看到注册页面, 第一个注册的用户就是 **admin**, 如下图是登陆成功后的管理员面板

![主面板](/assets/images/201711/10-03.png)

* **注意: 建议修改 `repository` 存放路径, 默认路径是 `/var/repo` 中**, 但是在 `/var` 目录下创建文件需要 **root** 权限, 该系统无法自动创建, 需要手动添加目录, 并修改权限.

![repository 存放路径](/assets/images/201711/10-04.png)

>注意: 一些涉及到安全的配置是无法使用web面板修改的, 需要使用 `./bin/config` 执行命令修改.

```bash
phabricator/ $ ./bin/config set repository.default-local-path <path>
```

![default-local-path](/assets/images/201711/10-05.png)

_如果 不修改 repository 存放路径, 也没有在 /var 目录下创建 repo, 或者 repo 目录无权限读写,  则添加的git项目是无法被拉取下来的._

* 添加 `repository`. 在面板的左侧边栏, 可以看到 `repositories`,  按照 `Repositories` > `Create Repository` > `Crete Git Repository` 添加即可

* 添加 repository 的 git 地址,在 repository 管理面板 `URLs` > `New URI`, 添加

![git 地址](/assets/images/201711/10-06.png)

* 设置 git 凭证, 也就是 添加 **ssh key 私钥**. 保存之后,下次新建项目可以直接选择已经添加的密钥. ( 注意: 密钥添加后无法通过web面板修改 )

![git 凭证](/assets/images/201711/10-07.png)

如果没有添加凭证, 则会在 repository面板中看到如下错误提示, 添加的 repository 没有生效.

![git 凭证](/assets/images/201711/10-08.png)

* 开启**守护进程**, 主要是作为定时任务功能, 比如: 拉取 git 更新 等.

```bash
phabricator/ $ ./bin/phd start
phabricator/ $ ps aux | grep php
```

![守护进程](/assets/images/201711/10-09.png)

如果 不开启守护进程, repository 会有错误信息, 提示守护进程没有开启

![守护进程](/assets/images/201711/10-10.png)

* 设置 `base-uri`, 如果没有设置, 会导致守护进程无法拉取 git 项目

![base-uri](/assets/images/201711/10-11.png)

```bash
phabricator/ $ ./bin/config set phabricator.base-uri <link>
```

![base-uri](/assets/images/201711/10-12.png)

![base-uri](/assets/images/201711/10-13.png)

* 大功告成, 安装完毕.


---
## 参考文档
- [官方教程](https://secure.phabricator.com/book/phabricator/article/installation_guide/)
