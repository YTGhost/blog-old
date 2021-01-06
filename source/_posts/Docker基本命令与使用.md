---
title: Docker基本命令与使用
date: 2020-11-14 17:16:56
tags:
- Docker
categories:
- 学习笔记
---
Docker的一些基操~
<!--more-->
## 配置docker

安装docker：`yum install docker`

给docker配置阿里云镜像加速器：
<img src="https://image.hihia.top/Screenshot/image-20201113220124597.png" style="zoom:33%" />

## 镜像操作

检索：`docker search 关键字`

拉取：`docker pull 镜像名:tag`

查看本地镜像：`docker images`

删除指定的本地镜像：`docker rmi image-id`

## 容器操作

| 操作     | 命令                                                         | 说明                                                         |
| -------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 运行     | `docker run --name container-name -d image-name`             | `--name`：自定义容器名<br />`-d`：后台运行<br />`image-name`：指定镜像模板 |
| 列表     | `docker ps`                                                  | 加上-a；可以查看所有容器                                     |
| 停止     | `docker stop container-name/container-id `（查看运行中的容器） | 停止当前你运行的容器                                         |
| 启动     | `docker start container-name/container-id`                   | 启动容器                                                     |
| 删除     | `docker rm container-id`                                     | 删除指定容器                                                 |
| 端口映射 | `-p 6379:6379`<br />eg:`docker run -d -p 6379:6379 --name myredis docker.io/redis` | -p：主机端口（映射到）容器内部的端口                         |
| 容器日志 | `docker logs container-name/container-id`
| 进入容器 | `sudo docker exec -it <id> /bin/bash`


## 常用容器安装
### 安装mysql

```shell
#拉取镜像
docker pull mysql
# 运行
docker run -d -p 6380:3306 --name mysql01 -e MYSQL_ROOT_PASSWORD=my-secret-pw mysql:tag
```

### 安装redis
```shell
# 拉取镜像
docker pull redis
# 运行
docker run -d --name myredis -p 6379:6379 redis --requirepass "mypassword"
# 使用redis-cli连接命令， -h 服务器地址 -p 端口号 -a 密码
redis-cli -h host -p port -a password
```