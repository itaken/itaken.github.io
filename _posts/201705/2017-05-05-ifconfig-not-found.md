---
layout: post

title: "ubuntu 17.04 ifconfig command not found"
date: 2017-05-05 16:22:04 +0800

categories: post
tags: [ubuntu, ipconfig]
---

## 问题描述
>使用`ifconfig`查看网络信息,结果返回`command not found`

```bash
$ ifconfig -all                                                           1 ↵
zsh: command not found: ifconfig

$ echo $PATH                                                            127 ↵
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin:/snap/bin
```

## 解决方法

1. 使用 `ip a`

    ```bash
    $ ip a                                                                  255 ↵
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        inet 127.0.0.1/8 scope host lo
           valid_lft forever preferred_lft forever
        inet6 ::1/128 scope host
           valid_lft forever preferred_lft forever
     ...
    ```

1. 安装`net-tools`工具

    ```bash
    $ sudo apt-get install net-tools

    $ ifconfig                                                                1 ↵
    enp0s25: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
            inet 172.18.9.164  netmask 255.255.0.0  broadcast 172.18.255.255
            inet6 fe80::f4e7:b4a2:5612:f9af  prefixlen 64  scopeid 0x20<link>
            ether 28:d2:44:2d:77:48  txqueuelen 1000  (以太网)
            RX packets 2179231  bytes 1809075526 (1.8 GB)
            RX errors 0  dropped 0  overruns 0  frame 0
            TX packets 1223469  bytes 124231597 (124.2 MB)
            TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
            device interrupt 20  memory 0xf0600000-f0620000
      ...
    ```

---
更多阅读
- [ifconfig command not found](https://unix.stackexchange.com/questions/145447/ifconfig-command-not-found)
- [ifconfig command not found](http://stackoverflow.com/questions/24839810/ifconfig-command-not-found)
