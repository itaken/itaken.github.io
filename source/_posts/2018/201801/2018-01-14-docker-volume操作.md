---
title: "docke volume操作"
date: 2018-01-14 10:43:05 +0800

categories: 开发日常
tags: [docker]
---

>环境
>Docker version 1.13.1, build 092cba3

### 运行镜像

```
$ docker run -d --name nginx -v /usr/share/nginx/html nginx
fc64b9d72ef5155625ca0bcec0ebd835399c4d3dc4e50b7c22a60bc718b20d0b
```

### 查看`nginx`信息

```
$ docker inspect nginx
```

### 查看挂载信息(`Mounts`)

```
"Mounts": [
    {
        "Type": "volume",
        "Name": "12830e60c099267ddc4b02f51345a62d0bde59c2fbc7a26eda0916f9bb26995e",
        "Source": "/var/lib/docker/volumes/12830e60c099267ddc4b02f51345a62d0bde59c2fbc7a26eda0916f9bb26995e/_data",
        "Destination": "/usr/share/nginx/html",
        "Driver": "local",
        "Mode": "",
        "RW": true,
        "Propagation": ""
    }
],
```

### 查看真实路径下文件内容

```
$ sudo ls /var/lib/docker/volumes/12830e60c099267ddc4b02f51345a62d0bde59c2fbc7a26eda0916f9bb26995e/_data
50x.html  index.html
```

可以对文件进行修改

```
$ docker exec -it nginx /bin/bash
root@fc64b9d72ef5:/# cd /usr/share/nginx/html/
root@fc64b9d72ef5:/usr/share/nginx/html# ls
50x.html  index.html
```

## 挂载容器

### 创建容器

```
$ docker create -v $PWD/data:/var/mydata --name data_container ubuntu
d34178763293222952df90f7e7729a4451acd745ca6a05198f4f6ec78307de03
```

_说明: `$PWD`当前路径_

### 挂载容器(持久化)

```
$ docker run -it --volumes-from data_container ubuntu /bin/bash

root@bf99e01d43e0:/# mount | grep mydata
/dev/sda2 on /var/mydata type ext4 (rw,relatime,errors=remount-ro,data=ordered)
root@bf99e01d43e0:/# cd /var/mydata/
root@bf99e01d43e0:/var/mydata# echo "HELLO WORLD" > hello.html
root@bf99e01d43e0:/var/mydata# ls
hello.html
```

`--volumes-from`挂载的容器

```
$ cd data
$ ll
总用量 4.0K
-rw-r--r-- 1 root root 12 5月  12 11:14 hello.html
$ cat hello.html
HELLO WORLD
```
