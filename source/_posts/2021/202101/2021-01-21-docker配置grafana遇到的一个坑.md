---
title: "在docker-compose启动grafana,出现权限错误的解决方案"
date: 2021-01-21 11:28:43 +0800
img: /assets/images/202101/21-01.png

categories: 一个问题
tags: [grafana, 问题集, docker]
---

>开发环境
```
Deepin 20.1
Docker version 19.03.8, build 1b4342cd4c
docker-compose version 1.21.0, build unknown
```

## `docker-compose.yml`文件示例

```
version: "3"

services:
    grafana:
        image: 'grafana/grafana:latest'
        user: "1000"
        container_name: 'grafana'
        ports:
            - '3000:3000'
        volumes:
            - './data:/var/lib/grafana'
            - './logs:/var/log/grafana'
```

<mark>问题描述</mark>

没有在grafana目录下新建`data`,`logs`文件,在运行docker-compose的时候会自动创建对应文件, 但是会提示权限问题.

```
grafana$ docker-compose up
WARNING: The Docker Engine you're using is running in swarm mode.

Compose does not use swarm mode to deploy services to multiple nodes in a swarm. All containers will be scheduled on the current node.

To deploy your application across the swarm, use `docker stack deploy`.

Creating network "grafana_default" with the default driver
Pulling grafana (grafana/grafana:latest)...
latest: Pulling from grafana/grafana
801bfaa63ef2: Pull complete
efdb3434c59e: Pull complete
8cbdb3f56d34: Pull complete
34f82d4bd2ec: Pull complete
af445b3382af: Pull complete
4f4fb700ef54: Pull complete
8aab09bbec8e: Pull complete
9e81c23e3db5: Pull complete
Digest: sha256:5f19b6c385e8bfb8e5c9ecc7cdd123a453af3cf01e7c20d20059e770f656286d
Status: Downloaded newer image for grafana/grafana:latest
Creating grafana ... done
Attaching to grafana
grafana    | GF_PATHS_DATA='/var/lib/grafana' is not writable.
grafana    | You may have issues with file permissions, more information here: http://docs.grafana.org/installation/docker/#migration-from-a-previous-version-of-the-docker-container-to-5-1-or-later
grafana    | mkdir: can't create directory '/var/lib/grafana/plugins': Permission denied
grafana exited with code 1
```

```
grafana$ docker-compose up
WARNING: The Docker Engine you're using is running in swarm mode.

Compose does not use swarm mode to deploy services to multiple nodes in a swarm. All containers will be scheduled on the current node.

To deploy your application across the swarm, use `docker stack deploy`.

Recreating grafana ... done
Attaching to grafana
grafana    | GF_PATHS_DATA='/var/lib/grafana' is not writable.
grafana    | You may have issues with file permissions, more information here: http://docs.grafana.org/installation/docker/#migration-from-a-previous-version-of-the-docker-container-to-5-1-or-later
grafana    | t=2021-01-21T03:26:19+0000 lvl=info msg="Starting Grafana" logger=server version=7.3.7 commit=1e261642f4 branch=HEAD compiled=2021-01-14T08:41:57+0000
grafana    | t=2021-01-21T03:26:19+0000 lvl=info msg="Config loaded from" logger=settings file=/usr/share/grafana/conf/defaults.ini
...
grafana    | t=2021-01-21T03:26:19+0000 lvl=eror msg="Server shutdown" logger=server reason="Service init failed: failed to connect to database: failed to create SQLite database file \"/var/lib/grafana/grafana.db\": open /var/lib/grafana/grafana.db: permission denied"
grafana    | Service init failed: failed to connect to database: failed to create SQLite database file "/var/lib/grafana/grafana.db": open /var/lib/grafana/grafana.db: permission denied
grafana exited with code 1
```

<mark>解决方案</mark>

修改grafana目录下的 data所有者,以及对应的子目录`plugins`.

```
grafana$ sudo chown itaken:itaken -R data
grafana$ sudo chown itaken:itaken -R logs
```

重新启动docker.

```
grafana$ docker-compose up
WARNING: The Docker Engine you're using is running in swarm mode.

Compose does not use swarm mode to deploy services to multiple nodes in a swarm. All containers will be scheduled on the current node.

To deploy your application across the swarm, use `docker stack deploy`.

Starting grafana ... done
Attaching to grafana
grafana    | t=2021-01-21T03:27:12+0000 lvl=info msg="Starting Grafana" logger=server version=7.3.7 commit=1e261642f4 branch=HEAD compiled=2021-01-14T08:41:57+0000
grafana    | t=2021-01-21T03:27:12+0000 lvl=info msg="Conf
...
```

在浏览器中输入`http://localhost:3000/`即可访问,默认初始管理员账号密码都是 `admin`!

![grafana](/assets/images/202101/21-01.png)

---
## 参考文档
- [New Docker Install with persistent storage, Permission problem](https://community.grafana.com/t/new-docker-install-with-persistent-storage-permission-problem/10896)
- [Configure a Grafana Docker image](https://grafana.com/docs/grafana/latest/administration/configure-docker/)
- [Run Grafana Docker image](https://grafana.com/docs/grafana/latest/installation/docker/#migration-from-a-previous-version-of-the-docker-container-to-5-1-or-later)
- [Grafana Logs "database is locked"](https://github.com/grafana/grafana/issues/16638)
