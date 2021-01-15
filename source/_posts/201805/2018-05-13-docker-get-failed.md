---
title: "docker拉取镜像失败"
date: 2018-05-13 19:35:47 +0800

categories: 一个问题
tags: [docker, 问题集]
---

<mark>问题描述</mark>

```
$ docker-compose up --build
Pulling composer (composer:latest)...
ERROR: Get https://registry-1.docker.io/v2/library/composer/manifests/latest: Get https://auth.docker.io/token?scope=repository%3Alibrary%2Fcomposer%3Apull&service=registry.docker.io: net/http: TLS handshake timeout


$ docker-compose up --build
Pulling web (nginx:alpine)...
ERROR: Get https://registry-1.docker.io/v2/: net/http: TLS handshake timeout
```

使用`docker-machine rm`, 提示错误.

```
$ docker-machine rm default                                                                                                              1 ↵
zsh: command not found: docker-machin
```

使用`docker logout`也无效

```
$ docker logout                                                                                                                        127 ↵
Not logged in to https://index.docker.io/v1/
$ docker-compose up --build
Pulling web (nginx:alpine)...
ERROR: Get https://registry-1.docker.io/v2/: net/http: TLS handshake timeout
```

<mark>解决方法</mark>

一般都是网络问题,设置代理, 或使用国内镜像站点.

在`/etc/docker/daemon.json`中,添加
```
{
  "registry-mirrors": [
    "https://registry.docker-cn.com"
  ]
}
```

重启服务

```
$ sudo systemctl daemon-reload
$ sudo systemctl restart docker
```

```
$ docker-compose up --build                                                                                                              1 ↵
Pulling web (nginx:alpine)...
alpine: Pulling from library/nginx
ff3a5c916c92: Already exists
b430473be128: Pulling fs layer
7d4e05a01906: Download complete
8aeac9a3205f: Download complete
...
```


---
## 参考文档
- [TLS handshake timeout](https://github.com/docker/kitematic/issues/1125)
- [镜像加速器](https://yeasy.gitbooks.io/docker_practice/content/install/mirror.html)
- [Use case: the China registry mirror](https://docs.docker.com/registry/recipes/mirror/#use-case-the-china-registry-mirror)
