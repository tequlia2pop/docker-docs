# 安装 Docker

## 安装 Docker 的先决条件：

*   运行64位架构的计算机。
*   运行 Linux 3.8 或更高版本内核。
*   内核必须支持一种适合的存储驱动（storage driver），例如：
	*   Device Manager；
	*   AUFS；
	*	vfs；
	*	btrfs；
	*	ZFS（Docker 1.7中引入）
	*	默认存储驱动通常是 Device Manager 或 AUFS。
*   内核必须支持并开启 cgroup 和命名空间（namespace）功能。

## 在 Windows 中安装 Docker Toolbox

下载地址：http://www.docker.com/products/docker-toolbox
官方安装文档：https://docs.docker.com/docker-for-windows/

NOTE: 
*   确认电脑的 BIOS 设置中的 CPU 虚拟化技术支持已经开启。
*   只能运行在 Windows 7.1、8/8.1 或者更高版本上安装 Docker Toolbox。
*   重新安装时，建议删除用户主目录下的 `.docker` 目录。

安装成功后，可以尝试使用本机的 Docker 客户端连接虚拟机中运行的 Docker 守护进程，来测试 Docker Toolbox 是否已经正常安装：

	$ docker info

## 其他资料：
*   [Docker ToolBox在win下安装试用](https://my.oschina.net/liuyuanyuangogo/blog/608064)

