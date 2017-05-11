---
layout: post

title: "ubuntu 安装docker"
date: 2017-05-10 15:12:23 +0800

categories: post
tags: [ubuntu, docker]
---

## 更新源

```bash
\# apt-get update
```

## 安装 `docker`

```bash
\# apt-get install -y docker.io

\# docker version
Client:
 Version:      1.12.6
 API version:  1.24
 Go version:   go1.6.2
 Git commit:   78d1802
 Built:        Tue Jan 31 23:35:14 2017
 OS/Arch:      linux/amd64
Cannot connect to the Docker daemon. Is the docker daemon running on this host?
```

## 启动`docker`

```bash
\# service docker start

\# docker version
Client:
 Version:      1.12.6
 API version:  1.24
 Go version:   go1.6.2
 Git commit:   78d1802
 Built:        Tue Jan 31 23:35:14 2017
 OS/Arch:      linux/amd64

Server:
 Version:      1.12.6
 API version:  1.24
 Go version:   go1.6.2
 Git commit:   78d1802
 Built:        Tue Jan 31 23:35:14 2017
 OS/Arch:      linux/amd64
```

## 查看`docker`状态

```bash
\# service docker status                                              1 ↵
● docker.service - Docker Application Container Engine
   Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: e
   Active: active (running) since 三 2017-05-10 15:06:48 CST; 4min 15s ago
     Docs: https://docs.docker.com
 Main PID: 4235 (dockerd)
    Tasks: 28
   Memory: 21.4M
      CPU: 494ms
   CGroup: /system.slice/docker.service
           ├─4235 /usr/bin/dockerd -H fd://
           └─4243 containerd -l unix:///var/run/docker/libcontainerd/docker-cont
   ...
```

## 拉取镜像 **hello-world**

```bash
\# docker pull hello-world
Using default tag: latest
latest: Pulling from library/hello-world
78445dd45222: Pull complete
Digest: sha256:c5515758d4c5e1e838e9cd307f6c6a0d620b5e07e6f927b07d05f6d12a1ac8d7
Status: Downloaded newer image for hello-world:latest

\# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
hello-world         latest              48b5124b2768        3 months ago        1.84 kB
```

## 拉取 **官方 nginx** 镜像
>国内建议使用 [网易蜂巢](https://c.163.com/hub#/m/home/) 的镜像.

```bash
\# docker pull hub.c.163.com/library/nginx:latest                     1 ↵
latest: Pulling from library/nginx

5de4b4d551f8: Pull complete
d4b36a5e9443: Pull complete
0af1f0713557: Pull complete
Digest: sha256:f84932f738583e0169f94af9b2d5201be2dbacc1578de73b09a6dfaaa07801d6
Status: Downloaded newer image for hub.c.163.com/library/nginx:latest
```

## 运行镜像

参数`-d` 表示后台运行一个镜像

```bash
\# docker run -d hub.c.163.com/library/nginx:latest
a1c8d69586a32c30d695c8cea4f7195c52e02c67f32293559a67a09edfbb6002
```
可以通过`docker ps`查看 运行的镜像.
```bash
\# docker ps
CONTAINER ID        IMAGE                                COMMAND                  CREATED             STATUS              PORTS               NAMES
a1c8d69586a3        hub.c.163.com/library/nginx:latest   "nginx -g 'daemon off"   11 seconds ago      Up 8 seconds        80/tcp              distracted_leakey
```
前台运行, 使用 `docker run hub.c.163.com/library/nginx:latest` 命令.

## 进入镜像操作

```bash
\# docker exec -it a1 bash                                            1 ↵
root@a1c8d69586a3:/\# ls
bin   dev  home  lib32	libx32	mnt  proc  run	 srv  tmp  var
boot  etc  lib	 lib64	media	opt  root  sbin  sys  usr
root@a1c8d69586a3:/\# exit
exit
```
>**a1**表示 镜像的ID

## 连接网络 [^2]

![docker 网络]({{ site.url }}/assets/images/201705/10-01.png)

端口映射
```bash
\# docker run -d -p 8080:80 hub.c.163.com/library/nginx
```

>使用`-P`可以映射所有端口, 命令: `docker run -d -P hub.c.163.com/library/nginx`

查看端口是否监听
```bash
$ netstat -na | grep 8080
tcp6       0      0 :::8080                 :::*                    LISTEN
```
在浏览器中直接输入[http://localhost:8080/](http://localhost:8080/)查看.

## 关闭容器
`# docker stop a1`


---
更多阅读
- [linux安装docker](http://www.imooc.com/video/14619)
- [网易蜂巢 镜像中心](https://c.163.com/hub#/m/home/)
- [ngnix 搜索结果](https://c.163.com/hub#/m/search/?keyword=nginx)
- [Ubuntu输入su提示认证失败的解决方法](http://studiogang.blog.51cto.com/505887/385223)

---
索引

[^2]: [docker网络](http://www.imooc.com/video/14623)
