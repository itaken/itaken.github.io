---
title: "dockerfile创建镜像"
date: 2018-01-11 23:06:17 +0800
img: /assets/images/201801/11-01.png

categories: 开发日常
tags: [docker]
---


### 创建 HELLO WORLD

在目录下,创建`Dockerfile`, 写入如下命令

```
FROM alpine:latest
MAINTAINER itaken
CMD echo "HELLO WORLD"
```

![docker](/assets/images/201801/11-01.png)

_说明: `alpine`是非常小的linx镜像_

### 使用`docker build`创建镜像

```bash
$ docker build -f Dockerfile -t itaken/hello_docker .

Sending build context to Docker daemon 2.048 kB
Step 1/3 : FROM alpine:latest
latest: Pulling from library/alpine
ff3a5c916c92: Pull complete
Digest: sha256:7df6db5aa61ae9480f52f0b3a06a140ab98d427f86d8d5de0bedab9b8df6b1c0
Status: Downloaded newer image for alpine:latest
 ---> 3fd9065eaf02
Step 2/3 : MAINTAINER itaken
 ---> Running in 4cea7327d9c9
 ---> 99d8f67ca624
Removing intermediate container 4cea7327d9c9
Step 3/3 : CMD echo "HELLO WORLD"
 ---> Running in abb196bfc38d
 ---> cb57a2eb20fa
Removing intermediate container abb196bfc38d
Successfully built cb57a2eb20fa
```

_说明:`t`表示标签, `.`表示路径_

### 查看image是否成功

```
$ docker images
REPOSITORY                TAG                 IMAGE ID            CREATED             SIZE
itaken/hello_docker              latest              cb57a2eb20fa        6 minutes ago       4.15 MB

$ docker run hello_docker
HELLO WORLD
```

### 创建复杂点的镜像

```bash
FROM ubuntu
MAINTAINER itaken
RUN apt update
RUN apt install -y nginx
COPY index.html /var/www/html
ENTRYPOINT ["/usr/sbin/nginx", "-g", "daemon off;"]
EXPOSE 80
```
创建`index.html`,写入`HELLO WORLD`

>语法说明
- FROM  base image
- RUN 执行命令
- ADD 添加文件
- COPY 拷贝文件
- CMD 执行命令
- EXPOSE 暴露端口
- WORKDIR 指定路径
- MAINTAINER 维护者
- ENV 设定环境变量
- ENTRYPOINT 容器入口
- USER 指定用户
- VOLUME mount point

### 查看是否成功

```
$ docker run -d -p 81:80 itaken/hello_nginx
3042061e3c2f40771289161227c04c23c20cd36c155f7f2596140bd6ba101a35

$ curl http://localhost:81
HELLO WORLD
```

---
## 参考文档
- [Dockerfile 指令详解](https://yeasy.gitbooks.io/docker_practice/content/image/dockerfile/)
