---
title: "docker端口被占用"
date: 2018-01-11 23:28:38 +0800

categories: 一个问题
tags: [docker, 问题集]
---

>环境
>Docker version 1.13.1, build 092cba3

<mark>问题描述</mark>

```
$ docker run -d -p 80:8088 itaken/hello_nginx                                                                                             125 ↵
5087e5e17370020684fc26ec242f8a7ca637c1bd16840b0610ad3187884dbe34
docker: Error response from daemon: driver failed programming external connectivity on endpoint elastic_bell (ddd0d8c22e0e1bf44c11202640cc81dca685ac73b9d7d463ee6e0df8fe0cef3c): Error starting userland proxy: listen tcp 0.0.0.0:80: bind: address already in use.

$ docker ps                                                                                                                            125 ↵
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

关闭防火墙,重启docker也没有效果.

```
systemctl stop firewalld
systemctl restart docker
```

设置ipv4,无效

```
$  sudo sysctl -w net.ipv4.tcp_mtu_probing=1
net.ipv4.tcp_mtu_probing = 1
```

不设置端口,运行docker,查看dns,没发现问题
```
$ docker run -d itaken/hello_nginx
890efa3039c4971ae3c8088012ce505e639140e788c3804d5caac24e1ff7d635

$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS               NAMES
890efa3039c4        itaken/hello_nginx     "/usr/sbin/nginx -..."   About a minute ago   Up About a minute   80/tcp              loving_mclean

$ docker exec -it 89 bash

root@890efa3039c4:/# cat /etc/resolv.conf
# Generated by NetworkManager

nameserver 8.8.8.8
nameserver 8.8.4.4
root@890efa3039c4:/# exit
exit
```

<mark>解决方法</mark>

原来`80`端口被占用了, 改用其他端口即可.

```
$ sudo lsof -n -P -i :80 | grep LISTEN
nginx    1956     root    6u  IPv4  24377      0t0  TCP *:80 (LISTEN)
nginx    1956     root    7u  IPv6  24378      0t0  TCP *:80 (LISTEN)
nginx    1959 www-data    7u  IPv6  24378      0t0  TCP *:80 (LISTEN)
nginx    1960 www-data    6u  IPv4  24377      0t0  TCP *:80 (LISTEN)
nginx    1960 www-data    7u  IPv6  24378      0t0  TCP *:80 (LISTEN)

$ docker run -d -p 81:80 webdevops/php-nginx-dev
6f920f7e01c2cb1c2099cd1143e1e569bce50fc5bf8346789e98cc0db70f573e
```


---
## 参考文档
- [Error starting userland proxy: listen tcp 0.0.0.0:80: bind: address already in use](https://github.com/zodern/meteor-up/issues/625)
- [listen tcp 0.0.0.0:80: bind: address already in use](https://github.com/laradock/laradock/issues/16)
- [Error response from daemon: driver failed programming external connectivity on endpoint nginx-proxy (669659d666e6b6164716c6009cc1f1b413f2130e8d6238db341769bce23620fa): Error starting userland proxy: Bind for 0.0.0.0:80: unexpected error (Failure EADDRINUSE) Error: failed to start containers: nginx-proxy](https://github.com/jwilder/nginx-proxy/issues/839)
- ["driver failed programming external connectivity on endpoint" (1.7.0-rc1)](https://github.com/docker/compose/issues/3277#issuecomment-243353309)
- [Control Docker with systemd](https://docs.docker.com/config/daemon/systemd/#httphttps-proxy)
