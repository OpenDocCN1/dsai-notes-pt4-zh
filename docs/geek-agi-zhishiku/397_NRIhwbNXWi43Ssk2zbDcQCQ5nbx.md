# Docker专题(五)：Dockerfile

> 来源：[https://geekbang.feishu.cn/docx/J6FTdVkK5oCw5wxfyj2cC7VRnf7](https://geekbang.feishu.cn/docx/J6FTdVkK5oCw5wxfyj2cC7VRnf7)

> 👨🏻 作者：尼尔

> 📃 作者介绍：热爱技术的技术人

> 🛖 👸号：螺竹编程

> ⏰ 收录时间：2024-05-27 11:15:56 (UTC+8)

> 🔗 原文链接：h[ttps://mp.weixin.q](https://mp.weixin.qq.com/s/vaSO0WO56fMwEUKpGIYzcA)q.com/s/vaSO0WO5...

![](img/c9bc1771d5a5c71b4083095b8839b6d9.png)

关注作者，获取更多相关知识

**Dockerfile介绍**

1

Dockerfile 是用于构建 Docker 镜像的文本文件，其中包含一系列的指令和参数，用于描述如何构建镜像。Dockerfile 是 Docker 构建镜像的重要组成部分，可以让用户自定义镜像的构建过程，从而满足不同的需求。

使用 Dockerfile 构建镜像非常简单，只需要在 Dockerfile 所在的目录下执行以下命令即可： docker build -t <image_name> .

其中， -t 参数用于指定镜像的名称和版本号， . 表示使用当前目录下的 Dockerfile 构建镜像。

**Dockerfile组成部分**

2

一个典型的 Dockerfile 包含以下几个部分：

1.  基础镜像信息：指定所使用的基础镜像，例如 FROM ubuntu:latest 。

1.  维护者信息：指定 Dockerfile 的维护者，例如 MAINTAINER Name <email> 。

1.  执行操作指令：包括安装软件、复制文件、设置环境变量等操作，例如 RUN apt-get update && apt-get install -y nginx 。

1.  暴露端口指令：指定容器运行时需要开放的端口，例如 EXPOSE 80 。

1.  容器启动时执行指令：指定容器启动时需要执行的命令，例如 CMD ["nginx", "-g", "daemon off;"] 。

1.  其他指令：包括 COPY 、 ADD 、 ENTRYPOINT 、 WORKDIR 等指令，用于完成更多的操作。

Dockerfile常见指令与参数

3

以下是 Dockerfile 中常见的指令及其参数的详细解释：

1.  **FROM **： 指定基础镜像。 FROM 指令是 Dockerfile 中的第一条指令，它指定了新镜像的基础镜像。

1.  **RUN **： 执行命令并创建一个新的层来保存这些命令的执行结果。 RUN 指令用于执行命令，通常用于安装软件包、设置环境等。

1.  **CMD **： 指定容器启动时默认执行的命令。 CMD 指令定义了容器启动时执行的命令，可以是可执行文件的路径或 shell 命令。

1.  **ENTRYPOINT **： 配置容器启动时执行的命令。 ENTRYPOINT 定义了容器启动时要运行的可执行文件或命令。

1.  **EXPOSE **： 声明容器运行时监听的端口。 EXPOSE 指令用于声明容器在运行时监听的端口，这样 Docker 守护进程就可以将这些端口映射到宿主机上。

1.  **ENV **： 设置环境变量。 ENV 用于设置环境变量，这些变量可以在后续的 RUN 指令中使用，也可以在容器运行时使用。

1.  **ADD **： 将文件添加到镜像中。 ADD 可以复制新文件或者解压 tar 压缩文件到镜像中。

1.  **COPY **： 类似于 ADD，但只复制文件。 COPY 用于复制新文件或者目录到镜像中，与 ADD 的区别在于它不支持自动解压缩 tar 文件。

1.  **VOLUME **： 创建一个可以从本地主机或其他容器挂载的挂载点。 VOLUME 指令用于定义一个挂载点，可以用于持久化存储或共享数据。

1.  **USER **： 设置运行容器时的用户名或 UID。 USER 用于指定运行容器时的用户名或用户 ID。

1.  **WORKDIR **： 设置容器内的工作目录。 WORKDIR 用于设置容器启动后的工作目录。

1.  **ARG **： 定义构建参数。 ARG 允许用户在构建镜像时传递参数。

1.  **ONBUILD **： 指定当镜像被用作其他镜像的基础镜像时，自动执行的指令。 ONBUILD 指令用于设置触发器，当镜像作为另一个镜像的基础时，这些指令将被执行。

1.  **STOPSIGNAL **： 设置停止容器时使用的信号。 STOPSIGNAL 用于设置停止容器时使用的信号。

1.  **LABEL **： 设置镜像的元数据。 LABEL 用于设置镜像的元数据，可以是键值对。

1.  **SHELL **： 覆盖容器内使用的 shell。 SHELL 用于指定构建镜像时使用的 shell，通常与 RUN 指令结合使用。

1.

编写Dockerfile注意事项

4

在编写 Dockerfile 时，需要注意以下几点：

1.  每个指令都会创建一个新的中间镜像，因此应该尽量合并多个指令，减少中间镜像的数量。

1.  指令的顺序很重要，应该按照依赖关系和执行顺序来编写。

1.  镜像构建时会缓存之前的构建结果，如果某个步骤出现错误，可以通过 --no-cache 参数来禁用缓存。

1.  Dockerfile 中可以使用各种变量和参数，例如 $ENV_VAR 和 ${PARAM:-default} 等。