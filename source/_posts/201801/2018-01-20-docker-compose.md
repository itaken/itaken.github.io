---
title: "docke-compose 简单使用"
date: 2018-01-20 21:27:11 +0800

categories: 开发日常
tags: [docker]
---

>本地环境
```
docker-compose version 1.8.0, build unknown
```

- `build` 本地创建镜像
- `command` 覆盖缺省命令
- `depends_on` 连接容器
- `ports` 暴露端口
- `volumes` 卷/挂载容器
- `image` pull镜像


- `up` 启动服务
- `stop` 停止服务
- `rm` 删除服务中的各个容器
- `logs` 观察各个容器的日志
- `ps` 列出服务相关的容器



---
## 参考文档
- [Sample apps with Compose](https://docs.docker.com/compose/samples-for-compose/)
- [Docker Compose](https://github.com/docker/compose)
- [Compose file version 3 reference](https://docs.docker.com/compose/compose-file/)
- [如何写docker-compose.yml，Docker compose file 参考文档](https://deepzz.com/post/docker-compose-file.html)
- [使用 docker-compose.yml 定义多容器应用程序](https://docs.microsoft.com/zh-cn/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/multi-container-applications-docker-compose)
- [Compose 模板文件](https://yeasy.gitbooks.io/docker_practice/content/compose/compose_file.html)
- [Compose 简介](https://yeasy.gitbooks.io/docker_practice/content/compose/introduction.html)
- [3.8.3 docker-compose.yml常用命令](http://book.itmuch.com/3%20%E4%BD%BF%E7%94%A8Docker%E6%9E%84%E5%BB%BA%E5%BE%AE%E6%9C%8D%E5%8A%A1/3.8.3%20docker-compose.yml%E5%B8%B8%E7%94%A8%E5%91%BD%E4%BB%A4.html)
