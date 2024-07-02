# Docker 生态一览

本文的目标是分享容器技术的整体视图。不涉及技术细节，而是对容器和 Docker 进行全局视图。

从 Docker 第一个版本诞生以来发生了很多的变化，这让试图学习这项技术的工程师和开发人员感到困惑。

本文将说明容器生态系统中的不同概念，它们之间的关系，Docker 介绍以及 2018 年以来最重要的里程碑。

所有的图片均来自 docker.com 网站。

## Docker

Docker 并不是容器技术本身，只是容器技术的一种实现方式，

容器并不是一个新兴的技术，Docker 的普及给人了一种它是唯一使用容器的错觉，但事实上不是这样的，下面这些也使用了容器技术：

* Chroot Jail
* FreeBSD Jails
* Linux-VServer
* Solaris Containers
* OpenVZ
* Process LMCTFY
* Docker
* RKT
* Chroot Jail

Docker 不是最早的一个容器化技术，但的确是最出名的一个。

该技术于 2013 年推出，在过去的几年不断变化和发展。

![docker-infrastructure.png](https://blog-1252349778.cos.ap-beijing.myqcloud.com/2018/docker-infrastructure.png)

这些是 Docker 平台主要的组件。

在架构上，Docker 位于应用程序和基础设施之间。它封装了行业标准的容器运行时 containerd，一个名为 docker Swarm 的本地编排工具，社区版（Docker 的开源版本）和提供商业管理服务的企业版。

## Docker 和 LXC

LXC 是 Docker 的第一个执行环境时，从 0.9 版本开始被 libcontainerd 取代

## Docker 和 libcontainerd

libcontaienrd 是 linux 基础设施的 Docker 接口，比如 cgroup、namespace。。。



![docker-component.png](https://blog-1252349778.cos.ap-beijing.myqcloud.com/2018/docker-component.png)

docker 创建容器的过程：

* docker 引擎创建镜像
* 传递给 containerd
* containerd 创建 containerd-shim
* containerd-shim 使用 runc 运行一个容器
* contaienrd-shim 允许运行时在启动后退出

这个模型的两个主要好处是：

- 运行守护进程减少容器
- 重启或者升级引擎不影响正在运行的容器