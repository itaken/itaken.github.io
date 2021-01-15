---
title: "ubuntu 使用gogs搭建本地git服务器"
date: 2017-09-03 20:31:10 +0800
img: /assets/images/201709/03-02.png

categories: 开发日常
tags: [ubuntu, git]
---

>本人环境配置
```
ubuntu: 17.04
go: 1.8.1
mysql: 5.7.18-15
OpenSSL: 1.0.2g
git: 2.11.0
Gogs: 0.11.29.0727
```

1. 下载 [gogs 二进制包](https://gogs.io/docs/installation/install_from_binary), 或者 [添加源](https://packager.io/gh/pkgr/gogs/builds/672/install/ubuntu-16.04) 的方式安装

1. 二进制包,解压并cd到目录. 执行 `$ ./gogs web`, 开启web服务.
```bash
$ ./gogs web
2017/09/03 20:16:47 [ WARN] Custom config '/home/itaken/Softwares/gogs/custom/conf/app.ini' not found, ignore this if you're running first time
2017/09/03 20:16:47 [TRACE] Custom path: /home/itaken/Softwares/gogs/custom
2017/09/03 20:16:47 [TRACE] Log path: /home/itaken/Softwares/gogs/log
2017/09/03 20:16:47 [TRACE] Log Mode: Console (Trace)
2017/09/03 20:16:47 [ INFO] Gogs 0.11.29.0727
2017/09/03 20:16:47 [ INFO] Cache Service Enabled
2017/09/03 20:16:47 [ INFO] Session Service Enabled
2017/09/03 20:16:47 [ INFO] SQLite3 Supported
2017/09/03 20:16:47 [ INFO] Run Mode: Development
2017/09/03 20:16:47 [ INFO] Listen: http://0.0.0.0:3000
[Macaron] 2017-09-03 20:16:58: Started GET / for 127.0.0.1
...
```
![gogs web server](/assets/images/201709/03-02.png)

1. 手动创建 `gogs`数据库. 创建`gogs`用户(建议), 或者使用`root`用户配置 **gogs**
```bash
$ mysql -u root -p
> create user 'gogs'@'localhost' identified by '密码';
> grant all privileges on gogs.* to 'gogs'@'localhost';
> flush privileges;
> exit;
```

1. 打开 `http://0.0.0.0:3000`, 完成配置即可看到git服务页面.

    控制面板页面:

    ![web](/assets/images/201709/03-01.png)

---
## 参考文档
- [使用 Gogs 搭建自己的 Git 服务器](https://blog.mynook.info/post/host-your-own-git-server-using-gogs/)
- [Gogs package](https://packager.io/gh/pkgr/gogs)
- [Gogs 一款极易搭建的自助 Git 服务](https://try.gogs.io/)
- [gogits/gogs](https://github.com/gogits/gogs)
