---
title: "ubuntu 16.04 无法启动mongodb"
date: 2017-03-02 11:14:07 +0800

categories: 一个问题
tags: [linux,ubuntu, mongodb, 问题集]
---

<mark>问题描述</mark>

今天, 打开好久没有用的mongodb, 发现无法启动.

```bash
$ sudo service mongodb start
```

提示错误: `Failed to start mongod.service: Unit mongodb.service is masked.`

<mark>解决方法</mark>

google搜索, 找到了如下解决方案. [^1]

```bash
$ sudo systemctl unmask mongodb
Removed /etc/systemd/system/mongodb.service.

$ sudo service mongodb enable
Usage: /etc/init.d/mongodb {start|stop|force-stop|restart|force-reload|status}

$ sudo service mongodb start
```

终于解决了, 完美 :))

---
## 参考文档
- [how-to-install-mongodb-on-ubuntu-16-04](https://www.digitalocean.com/community/tutorials/how-to-install-mongodb-on-ubuntu-16-04)
-  [mongodb-3-2-doesnt-start-on-lubuntu-16-04-lts-as-a-service](http://askubuntu.com/questions/770054/mongodb-3-2-doesnt-start-on-lubuntu-16-04-lts-as-a-service)
-  [running-mongodb-on-ubuntu-16-04-lts](http://stackoverflow.com/questions/37014186/running-mongodb-on-ubuntu-16-04-lts)


[^1]: http://stackoverflow.com/questions/37014186/running-mongodb-on-ubuntu-16-04-lts
