# Docker专题(三)：Docker镜像

> 来源：[https://geekbang.feishu.cn/docx/Vtgvdr8JEoCKSsxGWClcYAfQngb](https://geekbang.feishu.cn/docx/Vtgvdr8JEoCKSsxGWClcYAfQngb)

> 👨🏻 作者：尼尔

> 📃 作者介绍：热爱技术的技术人

> 🛖 公众号：螺竹编程

> ⏰ 收录时间：2024-05-27 11:15:56 (UTC+8)

> 🔗 原文链接：h[ttps://mp.weixin.q](https://mp.weixin.qq.com/s/LzaiNSi_-HGZ4GHMq8ZC1A)q.com/s/LzaiNSi_...

![](img/cb3899b3a9e25047f5bb1ba172255050.png)

关注作者，获取更多相关知识

**Docker镜像介绍**

1

Docker镜像是Docker容器的基本组成部分，可以看作是一个轻量级、可执行的独立软件包，其中包含了运行某个应用所需的全部代码、运行时环境、系统工具、库文件等。

Docker镜像是通过将一个应用的代码和依赖项打包到一个可移植的文件系统中来创建的。这个文件系统包含了应用所需的所有内容，如代码、库、环境变量等，可以在任何Docker环境中运行。

**Docker镜像特点**

2

Docker镜像具有以下特点：

1.  **轻量级 **：Docker镜像只包含应用所需的最小系统资源和软件依赖项，因此比传统虚拟机镜像更加轻量级。

1.  **可移植性 **：Docker镜像可以在任何Docker环境中运行，无需考虑环境依赖和配置的问题。

1.  **可重复性 **：Docker镜像可以轻松地进行复制和共享，因为它们是基于Dockerfile文件进行构建的。

1.  **安全性 **：由于Docker镜像是隔离的，因此可以提供更高的安全性，特别是当多个应用程序运行在同一台主机上时。

**Docker镜像的获取**

3

获取Docker镜像主要有两种方式 ：

1.  **从Docker Hub拉取 **：Docker Hub是Docker官方的镜像仓库，提供了大量的预制镜像供用户使用。使用以下命令可以拉取官方的Ubuntu镜像。

1.  **从私有仓库拉取 **：除了Docker Hub，还可以从私有仓库拉取镜像。这通常需要仓库的URL和认证信息。

**Docker镜像的生成**

4

Docker镜像是通过 Dockerfile 文件和Docker命令来创建的。Dockerfile文件是一个文本文件，其中包含了一系列指令，用于描述如何构建Docker镜像。Docker命令则是用于构建、管理和运行Docker容器的命令。通过Dockerfile文件和Docker命令，可以轻松地创建和管理Docker镜像。

**Docker镜像相关命令**

5

以下是一些列出 Docker 镜像相关信息的常用命令：

1.  **列出本地所有镜像 **： docker images 该命令会列出本地所有的 Docker 镜像，包括镜像名称、标签、镜像 ID、大小等信息。

1.  **列出本地所有镜像的摘要信息 **： docker images -q 该命令会列出本地所有 Docker 镜像的摘要信息，仅包括镜像 ID。可以将其与其他命令搭配使用，例如删除所有本地镜像： docker rmi $(docker images -q)

1.  **列出指定镜像的信息 **： docker inspect <image_name> 该命令会列出指定 Docker 镜像的详细信息，包括元数据、配置信息、网络设置等。

1.  **列出指定镜像的所有标签 **： docker images <image_name> --format "{{.Repository}}:{{.Tag}}" 该命令会列出指定 Docker 镜像的所有标签，以 Repository:Tag 的形式输出。

1.  **列出指定镜像的所有层 **： docker history <image_name> 该命令会列出指定 Docker 镜像的所有层级结构（layers），包括每一层的创建时间、创建方式、大小等信息。

1.  **列出被删除但未被清理的镜像 **： docker images -f dangling=true 该命令会列出所有被删除但未被清理的 Docker 镜像。可以通过 docker image prune 命令将其清理掉。

1.  **列出镜像的虚悬依赖 **： docker images -f before=<image_name>