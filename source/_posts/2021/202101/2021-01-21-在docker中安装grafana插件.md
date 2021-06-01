---
title: "在docker中安装grafana插件"
date: 2021-01-21 15:23:52 +0800

categories: 开发日常
tags: [linux, grafana, docker]
---

>开发环境
```
Deepin 20.1
Docker version 19.03.8, build 1b4342cd4c
docker-compose version 1.21.0, build unknown
```

## 查看容器

```
itaken@itaken-home:~$ docker container ps
CONTAINER ID        IMAGE                    COMMAND             CREATED             STATUS              PORTS                    NAMES
33f4a08b17e2        grafana/grafana:latest   "/run.sh"           About an hour ago   Up About an hour    0.0.0.0:3000->3000/tcp   grafana
```

## 步入容器,安装插件

```
itaken@itaken-home:~$ docker exec -it 33f4a08b17e2
"docker exec" requires at least 2 arguments.
See 'docker exec --help'.

Usage:  docker exec [OPTIONS] CONTAINER COMMAND [ARG...]

Run a command in a running container
itaken@itaken-home:~$ docker exec -it 33f4a08b17e2 /bin/bash
bash-5.0$ ll
bash: ll: command not found
bash-5.0$ la
bash: la: command not found
bash-5.0$ cd ..
bash-5.0$ ll
bash: ll: command not found
bash-5.0$ la
bash: la: command not found
bash-5.0$ ls
apk              ca-certificates  grafana          man              misc             udhcpc           zoneinfo
bash-5.0$

bash-5.0$ grafana-cli plugins install grafana-piechart-panel
installing grafana-piechart-panel @ 1.6.1
from: https://grafana.com/api/plugins/grafana-piechart-panel/versions/1.6.1/download
into: /var/lib/grafana/plugins

✔ Installed grafana-piechart-panel successfully

Restart grafana after installing plugins . <service grafana-server restart>

bash-5.0$ service grafana-server restart
bash: service: command not found
bash-5.0$ exit
exit
```


---
## 参考文档
- [How can I run a docker exec command inside a docker container](https://www.edureka.co/community/10588/how-can-i-run-a-docker-exec-command-inside-a-docker-container)
- [Pie Chart](https://grafana.com/grafana/plugins/grafana-piechart-panel?pg=plugins&plcmt=featured-undefined)
