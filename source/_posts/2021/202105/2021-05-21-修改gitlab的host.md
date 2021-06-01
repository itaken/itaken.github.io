---
title: "修改docker gitlab的host"
date: 2021-05-21 23:27:04 +0800
img: /assets/images/202105/21-01.png

categories: 一个问题
tags: [git, gitlab, 问题集]
---

<mark>问题描述</mark>

使用docker 本地安装 gitlab, 准备自定义host.

`docker-compose.yml`文件如下:

```yml
version: '3'
services:
  web:
    image: 'gitlab/gitlab-ce:latest'
    container_name: gitlab-web
    restart: always
    hostname: gitlab.local
    privileged: true
    ports:
      - '2000:80'
      - '2022:22'
      - '2443:443'
    volumes:
      - './config:/etc/gitlab'
      - './logs:/var/log/gitlab'
      - './data:/var/opt/gitlab'
    logging:
      driver: json-file
    deploy:
      resources:
        limits:
          cpus: '1.0'
          memory: 512M
        reservations:
          cpus: '0.5'
          memory: 128M
```

<mark>解决方案</mark>


修改`gitlab.rb`文件,开启`external_url "http://gitlab.local"`,保存文件,移除container,重新建立.

```
itaken@itaken-home:/path/to/gitlab/config$ sudo vim gitlab.rb

itaken@itaken-home:/path/to/gitlab/config$ sudo docker ps
CONTAINER ID        IMAGE                            COMMAND                  CREATED             STATUS                    PORTS                                                                                    NAMES
e89f9b90d047        gitlab/gitlab-ce:latest          "/assets/wrapper"        2 weeks ago         Up 39 minutes (healthy)   0.0.0.0:2022->22/tcp, 0.0.0.0:2000->80/tcp, 0.0.0.0:2443->443/tcp                     gitlab-gitlab-ce

itaken@itaken-home:/path/to/gitlab/config$ sudo docker rm -f gitlab/gitlab-ce:latest
Error: No such container: gitlab/gitlab-ce:latest
itaken@itaken-home:/path/to/gitlab/config$ sudo docker container ps
CONTAINER ID        IMAGE                            COMMAND                  CREATED             STATUS                    PORTS                                                                                    NAMES
e89f9b90d047        gitlab/gitlab-ce:latest          "/assets/wrapper"        2 weeks ago         Up 39 minutes (healthy)   0.0.0.0:2022->22/tcp, 0.0.0.0:2000->80/tcp, 0.0.0.0:2443->443/tcp                     gitlab-gitlab-ce
itaken@itaken-home:/path/to/gitlab/config$ sudo docker rm -f e89f9b90d047
e89f9b90d047

itaken@itaken-home:/path/to/gitlab$ docker-compose up
...
```

进入gitlab,发现项目的host已经修改了!

![修改host](/assets/images/202105/21-01.png)

---
## 参考文档
- [GitLab on Docker - How to set the external URL for GitLab](https://stackoverflow.com/questions/59177615/gitlab-on-docker-how-to-set-the-external-url-for-gitlab)
