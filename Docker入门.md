# Docker 入门

## 确保 Docker 已经就绪

查看 docker 程序是否存在，功能是否正常：

	$ docker info

## 容器

**运行容器**

最简单的从镜像运行容器的命令：`docker run --name container-name -d image-name`

创建交互式容器：

	$ docker run -i -t ubuntu /bin/bash

创建守护式容器：

	$ docker run --name daemon_dave -d ubuntu /bin/sh -c "while true; do echo hello world; sleep 1; done"

使用 `-d` 选项告诉 Docker 将容器放到后台执行。

守护式容器（daemonized container）没有交互式会话，非常适合运行应用程序和服务。

**停止容器**

命令格式：`docker stop container-name/container-id`

	$ docker stop daemon_dave

**重新启动已经停止的容器**

命令格式：`docker start container-name/container-id`

	$ docker start bob_the_container

**自动重启容器**

如果由于某种错误而导致容器停止运行，可以通过 `--restart` 标志让 Docker 自动重新启动该容器。`--restart` 标志会检查容器的退出代码，并据此来决定是否要重启容器，默认的行为是 Docker 不会重启容器。

自动重启容器：

	$ docker run --restart=always --name daemon_dave -d ubuntu /bin/sh -C "while true; do echo hello world; sleep 1; done"

通过 `on-failure` 指定重启次数：

	--restart=on-failure:5

**删除容器**

删除指定的容器：`docker rm container-id`

使用 `-f` 来删除运行中的容器。

删除全部容器：`docker rm ${docker ps -a -q}`

	$ docker rm `docker ps -a -q`

**容器命名**

Docker 会为容器自动生成一个随机的名称。推荐使用 `--name` 选项为容器指定一个名称：

	$ docker run --name bob_the_container -i -t ubuntu /bin/bash

一个合法的容器名称只能包含以下字符：小写字母a-z、大写字母A-Z、数字0-9、下划线、圆点、横线，对应的正则为 `[a-zA-Z0-9_.-]`。

**附着到容器上**

	$ docker attach bob_the_container

**容器日志**

	$ docker logs daemon_dave

使用 `-f` 参数来监控 Docker 的日志：

	$ docker logs -f daemon_dave

使用 `-t` 标志为每条日志项加上时间戳：

	$ docker logs -t daemon_dave

可以跟踪容器日志的某一片段。例如 `$ docker logs --tail 10 daemon_dave` 获取日志的最后10行内容。`$ docker logs --tail 0 -f daemon_dave` 跟踪某个容器的最新日志而不必读取整个日志文件。
	
**Docker 日志驱动**

控制 Docker 守护进程和容器所用的日志驱动，通过 `--log-driver` 选项来实现。

**查看容器内的进程**

	$ docker top daemon_dave

**Docker 统计信息**

查看容器的统计信息：

	$ docker stats daemon_dave daemon_kate daemon_clare daemon_sarah

**在容器内部运行进程**

可以在容器内运行的进程有两种类型：后台任务和交互式任务。

在容器中运行后台任务：

	$ docker exec -d daemon_dave touch /etc/new_config_file

在容器中运行交互命令：

	$ docker exec -t -i daemon_dave /bin/bash

**深入容器**

使用 `docker inspect` 可以获得更多的容器信息：

	$ docker inspect daemon_dave

可以使用 `-f` 或 `--format` 标志来选定查看结果。

**端口映射**

Docker 容器中运行的软件所使用的端口，在本机和本机的局域网是不能访问的，所以需要将 Docker 容器中的端口映射到当前主机的端口上，这样本机和本机所在的局域网就能够访问软件了。

通过 `-p` 参数来实现端口映射。例如，映射 Redis 容器的 6379 端口到本机的 6378 端口：

	$ docker run -d -p 6378:6379 --name port-redis redis

注意，在非 Linux 下运行的 Docker 实际上是运行在虚拟机中的，即当前的本机不是当前的开发机器，而是虚拟机。所以还需要在虚拟机中再做一次端口映射，将虚拟机的端口映射到当前的开发机器。

## 镜像和仓库

本地镜像都保存在 Docker 宿主机的 /var/lib/docker 目录下。每个镜像都保存在 Docker 所采用的存储驱动目录下面，如 aufs 或者 devicemapper。也可以在 /var/lib/docker/containers 目录下面看到所有的容器。

Docker Hub 中有两种类型的仓库：用户仓库（user repository）和顶层仓库（top-level repository）。用户仓库的镜像都是由 Docker 用户创建的，而顶层仓库则是由 Docker 内部的人来管理的。

**列出镜像**

命令格式：`$ docker images`

**拉取镜像**

可以通过 `docker pull` 命令将镜像拉取到本地。

命令格式：`docker pull image-name`

拉取一个带标签的镜像：

	$ docker pull ubuntu:12.04

**查找镜像**

命令格式：`docker search image-name`

	$ docker search puppet

**删除镜像**

删除指定的镜像：`docker rmi image-id`

删除所有的镜像：`docker rmi ${docker images -q}`