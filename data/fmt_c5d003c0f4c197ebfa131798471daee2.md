
# Docker专题(二)：Docker架构、核心组件与概念

> 来源：[https://geekbang.feishu.cn/docx/JRmad8mQso2AApxUjsocsGMxnvb](https://geekbang.feishu.cn/docx/JRmad8mQso2AApxUjsocsGMxnvb)

> 👨🏻 作者：尼尔

> 📃 作者介绍：热爱技术的技术人

> 🛖 👸号：螺竹编程

> ⏰ 收录时间：2024-05-27 11:15:56 (UTC+8)

> 🔗 原文链接：[https://mp.weixin.qq.com/s/DxoNxRGuDcE8lC7PJW-MCg](https://mp.weixin.qq.com/s/DxoNxRGuDcE8lC7PJW-MCg)

![](img/66be8315f59358c7f997cedba49cfd26.png)

关注作者，获取更多相关知识

## Docker架构

![](img/4d4e9698cfedd057cd82f2eec8e0a097.png)

在Docker架构中，有三个主要组件：`Docker Client`、`Docker Host`和`Docker Registry`。

### Docker Client

`Docker Client`是与用户交互的命令行工具或图形界面，用于与`Docker`进行通信。用户可以使用`Docker Client`执行各种`Docker`命令，例如构建、运行、停止和管理容器，以及管理`Docker`镜像和网络等。`Docker Client`与`Docker`守护进程（`Docker daemon`）通信，发送用户的命令并接收执行结果。

### Docker Host

`Docker Host`是安装了`Docker`引擎的物理或虚拟机。它负责运行`Docker`守护进程，并管理`Docker`容器和镜像。`Docker Host`可以是开发人员的本地机器，也可以是云服务提供商的虚拟机实例。`Docker Host`包含`Docker daemon`，`Docker Daemon`（`Docker`守护进程）是`Docker`引擎的守护进程，负责管理`Docker`对象（如容器、镜像、网络和卷等），接收来自`Docker Client`的命令并执行相应操作。

### Docker Registry

`Docker Registry`是一个集中存储和分发`Docker`镜像的服务。它充当了`Docker`镜像的仓库，并提供了一个集中的位置供开发者上传、下载和共享镜像。`Docker Hub`是最常用的公共`Docker Registry`，提供了大量的官方和社区维护的镜像。此外，还可以搭建私有的`Docker Registry`，用于内部开发和部署需求。

## Docker核心组件与概念

### 镜像（`Image`）

`Image`是一个只读的模板，包含了运行应用程序所需的所有文件系统、库、工具和配置。可以通过`Docker`镜像构建容器。镜像可以从`Docker Hub`或其他镜像仓库中获取，也可以通过`Dockerfile`定义自己的镜像。

### 容器（`Container`）

`Container`是从镜像创建的运行实例。容器是一个独立的、轻量级的、可执行的软件包，包含了运行应用程序的所有内容。容器隔离了应用程序及其依赖项，使其可以在不同的环境中以一致的方式运行。

### Docker引擎（`Docker Engine`）

`Docker Engine`是`Docker`的核心组件，负责构建和运行容器，同时它是运行在`Docker Host`上的软件集合。`Docker Engine`使用Linux内核的容器特性（如`cgroups`和命名空间）来提供容器化功能。`Docker Engine`包括以下几个关键组件：

#### Docker Daemon

`Docker Daemon`是`Docker`引擎的守护进程，负责管理`Docker`对象（如容器、镜像、网络和卷等）。它接收来自`Docker Client`的命令，并执行相应的操作，如创建、启动、停止和销毁容器，以及构建和拉取镜像等。`Docker Daemon`与`Docker Client`进行通信，并通过`REST API`提供服务。

#### Docker Runtime

`Docker Runtime`是负责运行和管理`Docker`容器的组件。它可以使用不同的容器运行时（如`Docker Engine`、`containerd`、`CRI-O`等）来创建和管理容器。容器运行时负责启动容器、隔离容器的文件系统、进程空间和网络，并提供容器与宿主机之间的通信和资源管理。

#### Docker Storage

`Docker Storage`负责管理`Docker`容器的持久化存储。它可以使用不同的存储驱动（如`overlay2`、`aufs`、`btrfs`等）来管理容器的镜像和数据卷。存储驱动负责在宿主机上创建、读取和写入容器的文件系统，并将数据存储在宿主机上的相应位置。

#### Docker Network

`Docker Network`负责创建和管理`Docker`容器之间的网络连接。它可以创建虚拟网络、子网和网络接口，并为容器分配IP地址。`Docker Network`还提供网络隔离和路由功能，使容器可以在独立的网络环境中运行，并与其他容器或外部网络通信。

### Docker文件（`Dockerfile`）

`Dockerfile`是一个文本文件，包含了一系列的指令和配置，用于自动化构建`Docker`镜像。通过编写`Dockerfile`，开发者可以定义镜像的内容、依赖项和运行配置，以及执行构建过程。
