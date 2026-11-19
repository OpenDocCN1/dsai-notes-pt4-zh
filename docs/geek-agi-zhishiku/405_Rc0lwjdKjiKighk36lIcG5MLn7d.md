
# Docker常用命令大全：一篇文章让你掌握所有技巧

> 来源：`https://geekbang.feishu.cn/docx/I0sXdfXLtomLxvxa4mgcUm0knEc`

> 👨🏻 作者：Lion莱恩呀

> 📃 作者介绍：专注分享高性能服务器后台开发知识，包括C/C++、Linux、网络协议、设计模式、中间件、云原生、数据库、分布式架构等领域知识。

> 🛖 👸号：Lion莱恩呀

> ⏰ 收录时间：2024-05-27 11:15:56 (UTC+8)

> 🔗 原文链接：`https://mp.weixin.qq.com/s?__biz=Mzk0ODUzNDc2Mw==&mid=2247485461&idx=1&sn=0718f0f38bbf14f1dd37524f213a955a&scene=19#wechat_redirect`

关注作者，获取更多相关知识

## 一、简介

Docker是一种开源的平台，用于开发，交付和运行应用程序。它利用容器化技术，可以将应用程序及其所有依赖（如库、环境变量、配置文件等）打包到一个标准化的单元中，称为容器。这使得应用程序可以在几乎任何环境中轻松部署，并且能够快速扩展和管理。Docker极大地简化了应用程序的交付和部署流程，同时提高了资源利用率和可移植性。因此，Docker被广泛应用于DevOps、持续集成/持续部署以及云计算等领域。

Docker的优势：

+   Docker容器与传统虚拟机相比具有更小的资源占用，启动更快，占用更少的内存和存储空间。

+   Docker容器可以在任何支持Docker的环境中运行，无论是开发、测试还是生产环境，都能够保持一致性。

+   Docker容器共享操作系统内核，因此在同一台物理机上可以运行多个容器，从而提高了硬件资源的利用率。

+   Docker容器可以轻松地部署、终止、扩展和迁移，使得应用程序的管理和维护更加灵活方便。

Docker的应用场景：

1.  应用程序的打包和交付：将应用程序及其依赖打包到Docker容器中，并确保在不同环境中始终一致。

1.  DevOps：快速构建、测试和部署应用程序，实现持续集成和持续交付。

1.  云计算：在云平台上快速部署，提高应用程序的可伸缩性、弹性和可移植性。

1.  微服务架构：每个微服务可以打包成一个独立的容器，便于管理和扩展。

1.  混合云部署：轻松在私有云、公有云和混合云环境中部署，在不同云环境之间实现便捷迁移。

为什么掌握常用命令很重要？

1.  熟练使用常用命令可以更快地执行常见任务，提高效率。

1.  可以简化工作流程，减少重复工作。

1.  更快速地进行故障排除和调试。

1.  直接地控制系统和应用程序，实现更高的灵活性和定制性。

1.  可以更好地管理远程服务器。

## 二、安装和启动Docker

在 Linux 上安装 Docker 引擎：

+   执行以下命令以安装 Docker 引擎：

```
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

+   启动 Docker 引擎：

```
sudo systemctl start docker
```

+   确保 Docker 引擎开机启动：

```
sudo systemctl enable docker
```

在 Windows 上安装 Docker 引擎：

+   下载 `Docker Desktop for Windows` 并安装。下载地址：`https://docs.docker.com/desktop/install/windows-install/`

+   安装完成后，在系统中启动 Docker Desktop：使用 Windows 搜索栏找到“服务”应用程序，并打开它。在服务列表中，找到“Docker Desktop Service”。右键单击“Docker Desktop Service”，然后选择“启动”。

Linux上验证Docker安装是否成功：

```
docker --version
```

如果 Docker 安装成功，将会显示 Docker 引擎的版本信息。

也可以执行以下命令查看 Docker 信息：

```
docker info
```

这将显示有关 Docker 引擎的详细信息，包括版本、容器数量、镜像数量等。

## 三、容器操作常用命令

### 3.1、创建容器

`docker run` 是一个创建和运行容器的常用命令。它是在 Docker 中创建新容器的主要方式之一。

基本语法：

```
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
```

+   `OPTIONS`: 可以是一系列的选项，用于配置容器的各种行为和特性。

+   `IMAGE`: 要基于哪个镜像创建容器。

+   `COMMAND`: 可选参数，指定容器启动后要执行的命令。

+   `ARG`: 可选参数，作为 `COMMAND` 的参数传递给容器。

常用选项包括：

+   `-d`, `--detach`：在后台运行容器。

+   `-it`：以交互模式运行容器，通常与`-t`和`-i`一起使用。允许用户输入命令。

+   `--name`：为容器指定一个名称。

+   `-p`：映射容器端口到主机端口。

+   `-v`：挂载数据卷到容器。

示例：在后台运行一个名为 `my_container` 的容器，将容器的端口80映射到主机的端口8080，使用镜像 `my_image`。

```
docker run -d --name my_container -p 8080:80 my_image
```

在使用`docker run`命令时，Docker 将会使用指定的镜像在新容器中运行一个指定的命令。如果镜像在本地不存在，Docker 将会尝试从默认的镜像仓库中下载。

`docker run` 命令是容器化应用程序的基础之一，它提供了灵活的选项，可以根据需要配置容器的各种行为和特性。

### 3.2、查看容器详细信息

在 Docker 中，有两个常用的命令可以用来查看容器的详细信息：`docker ps` 和 `docker inspect`。

`docker ps` 命令 用于列出正在运行的容器，并显示其基本信息，例如容器的ID、名称、创建时间、状态等。其基本语法如下：

```
docker ps [OPTIONS]
```

`OPTIONS`常用的选项：

+   `-a`：显示所有的容器，包括正在运行和已经停止的容器。

+   `-q`：仅显示容器的ID，而不显示其他信息。

+   `--filter`：根据指定的条件过滤要显示的容器，例如根据状态、标签等。

+   `--format`：指定输出的格式，可以自定义输出的信息。

+   `-l`：显示最近创建的容器，包括已经停止的容器。

+   `-n`：显示最近创建的`N`个容器，`N`为数字。

示例:

```
docker ps
```

输出：

```
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                    NAMES
2a7f84d16df0        nginx:latest        &quot;nginx -g &#x27;daemon of…&quot;   About a minute ago   Up 59 seconds       0.0.0.0:8080-&gt;80/tcp     my-nginx
d682412e8da5        mysql:5.7           &quot;docker-entrypoint.s…&quot;   3 days ago           Up 3 days           0.0.0.0:3306-&gt;3306/tcp   my-mysql
e11c52904a83        redis:latest        &quot;docker-entrypoint.s…&quot;   2 weeks ago          Exited (137) 2 weeks  0.0.0.0:6379-&gt;6379/tcp   my-redis
```

`docker inspect` 命令 用于获取容器的详细信息，包括容器的配置、网络设置、挂载的卷等。基本语法如下：

```
docker inspect [OPTIONS] CONTAINER
```

其中，`CONTAINER` 是要查看的容器的名称或ID。`OPTIONS`可用选项如下：

+   `--format`, `-f`：指定输出信息的格式。

+   `--size`, `-s`：输出容器占用的存储空间大小。

示例：

```
docker inspect nginx:latest
```

输出：

```
[
    {
        &quot;Id&quot;: &quot;2a7f84d16df0&quot;,
        &quot;Name&quot;: &quot;/my-nginx&quot;,
        &quot;State&quot;: {
            &quot;Status&quot;: &quot;running&quot;,
            &quot;Running&quot;: true
        },
        &quot;Config&quot;: {
            &quot;Image&quot;: &quot;nginx:latest&quot;,
            &quot;Cmd&quot;: [
                &quot;nginx&quot;,
                &quot;-g&quot;,
                &quot;daemon off;&quot;
            ],
            &quot;Ports&quot;: [
                {
                    &quot;PrivatePort&quot;: 80,
                    &quot;PublicPort&quot;: 8080
                }
            ]
        },
        &quot;NetworkSettings&quot;: {
            &quot;IPAddress&quot;: &quot;172.17.0.2&quot;
        }
    }
]
```

### 3.3、启动/停止/重启容器

（1）启动容器:

```
docker start [OPTIONS] CONTAINER [CONTAINER...]
```

此命令用于启动已经存在的容器。可以指定一个或多个容器的名称或ID来启动它们。例如：

```
docker start my-container
```

`docker start` 命令的常用选项：

+   `-i`, `--interactive=false`：默认值是 false，启动后进入容器执行命令的交互模式。

+   `--attach=[]`：默认值是 []，在启动后进入容器的标准输入、输出和错误输出（类似于ssh）。

+   `-a`, `--attach=[]`：默认值是 []，在启动后进入容器的标准输入、输出和错误输出（类似于ssh）。

+   `-d`, `--detach=true`：默认值是 true，启动容器后立即返回。

+   `--sig-proxy=true`：默认值是 true，代理接收到的信号到容器内进程。

+   `--name=""`：指定容器的名称。

+   `--add-host=[]`：为容器添加自定义的主机信息。

+   `-h`, `--hostname=""`：容器的主机名。

+   `--help=false`：帮助信息。

（2）停止容器:

```
docker stop [OPTIONS] CONTAINER [CONTAINER...]
```

该命令用于停止正在运行的容器。可以指定一个或多个容器的名称或ID来停止它们。例如：

```
docker stop my-container
```

`docker stop` 命令是比较简单的命令，通常情况下并不需要额外的选项，所以它的选项并不多，常用的选项：

+   `-t`, `--time=10`：指定等待容器停止的时间，默认为10秒。

一般情况下，使用 `docker stop` 命令加上容器的名称或ID即可完成对容器的停止操作。当容器停止后也可以使用 `docker kill` 命令直接强制停止容器，但不推荐这种做法，因为它会直接终止容器中的进程，可能导致数据丢失或者不一致。

（3）重启容器:

```
docker restart [OPTIONS] CONTAINER [CONTAINER...]
```

该命令用于重启正在运行的容器。可以指定一个或多个容器的名称或ID来重启它们。例如：

```
docker restart my-container
```

`docker restart` 命令并没有常用的选项，因为它的功能比较简单，只需指定要重启的容器即可。

### 3.4、进入容器

`docker exec` 命令用于在正在运行的容器中执行命令。其基本语法为：

```
docker exec [OPTIONS] CONTAINER COMMAND [ARG...]
```

其中，`CONTAINER` 指定要在其中执行命令的容器的名称或ID，`COMMAND [ARG...]` 指定要执行的命令及参数。

常用的选项包括：

+   `-d`, `--detach=false`：在后台模式中运行命令

+   `-i`, `--interactive=false`：保持 STDIN 打开，即使未附加

+   `-t`, `--tty=false`：分配伪终端

例如，要在名为 `mycontainer` 的容器中执行 `ls -l` 命令，可以使用下面的命令：

```
docker exec -it mycontainer ls -l
```

这将在 `mycontainer` 容器中执行 `ls -l` 命令，并且通过 `-i` 和 `-t` 选项指定了交互模式和分配伪终端。

使用 `docker exec` 命令可以方便地在运行中的容器中执行命令，而不需要停止容器或者通过 `docker attach` 命令附加到容器中。

### 3.5、删除容器

`docker rm` 命令用于删除一个或多个容器。其基本语法为：

```
docker rm [OPTIONS] CONTAINER [CONTAINER...]
```

其中，`CONTAINER` 指定要删除的容器的名称或ID，可以同时指定多个容器。

常用的选项包括：

+   `-f`, `--force`：强制删除正在运行的容器

+   `-v`, `--volumes`：删除容器关联的存储卷

例如，要删除名为 `mycontainer` 的容器，可以使用以下命令：

```
docker rm mycontainer
```

如果要删除多个容器，可以在命令中列出它们的名称或ID，如：

```
docker rm container1 container2 container3
```

注意：使用 `docker rm` 命令可以删除处于停止状态的容器，如果要删除正在运行的容器，则需要使用 `-f` 或 `--force` 选项。

另外，使用 `-v` 或 `--volumes` 选项可以同时删除容器关联的存储卷。

## 四、镜像操作常用命令

### 4.1、拉取镜像

`docker pull` 命令用于从Docker Registry（如Docker Hub）拉取镜像到本地。其基本语法为：

```
docker pull [OPTIONS] NAME[:TAG|@DIGEST]
```

其中，`NAME` 指定要拉取的镜像的名称， `TAG` 指定要拉取的镜像的标签，可以省略，默认为 `latest` ， `DIGEST` 是镜像的摘要。

常用的选项包括：

+   `-a`, `--all-tags=false`：拉取镜像的所有标签

+   `--disable-content-trust=true`：禁用镜像内容信任（Image Content Trust）

例如，要拉取名为 `ubuntu` 的镜像，可以使用以下命令：

```
docker pull ubuntu
```

如果要拉取特定标签的镜像，可以使用 `:` 分隔名称和标签，如：

```
docker pull ubuntu:18.04
```

`docker pull` 命令会从指定的Registry（默认为Docker Hub）下载镜像，如果指定的镜像不存在于本地，就会将其拉取到本地。如果要拉取镜像的所有标签，可以使用 `-a` 选项。

### 4.2、查看本地镜像列表

`docker images` 命令用于列出本地系统中的镜像列表。其基本语法为：

```
docker images [OPTIONS] [REPOSITORY[:TAG]]
```

其中， `OPTIONS` 包括一些常见的选项，如：

+   `-a`, `--all`：显示所有镜像，包括中间映像

+   `--digests`：显示镜像的摘要

+   `--no-trunc`：显示完整的镜像ID

`REPOSITORY` 指定要筛选的镜像存储库的名称， `TAG` 指定要筛选的镜像标签。

例如，如果要列出系统中的所有镜像，可以使用以下命令：

```
docker images
```

输出：



# 四、镜像操作常用命令

```
REPOSITORY          TAG       IMAGE ID       CREATED        SIZE
ubuntu              latest    2ca708c1c9cc   5 days ago     72.8MB
alpine              3.13      a0656fbd4a9b   5 weeks ago    5.6MB
nginx               latest    4bb7d98f3a41   2 months ago   133MB
mysql               5.7       63ed815ced08   3 months ago   448MB
```

如果要显示特定仓库的镜像，可以指定 `REPOSITORY` ：

```
docker images ubuntu
```

## 4.3、构建镜像

`docker build` 命令是用于从 `Dockerfile` 构建镜像的命令。`Dockerfile` 是一个文本文件，包含了构建镜像所需的所有指令和参数。

基本的 `docker build` 命令语法如下：

```
docker build [OPTIONS] PATH | URL | -
```

其中，`PATH` 是指包含 `Dockerfile` 的目录路径，`URL` 是指包含 `Dockerfile` 的 Git 仓库地址，`-` 表示使用标准输入。

常用的选项包括：

+   `-t, --tag`：为新构建的镜像设置标签

+   `--file`：指定要使用的 `Dockerfile` 路径

+   `--build-arg`：设置构建参数

+   `--no-cache`：不使用缓存，从头开始构建

例如，在包含 `Dockerfile` 的目录中，可以使用以下命令构建镜像并为其设置标签：

```
docker build -t myimage:1.0 .
```

这将在当前目录中查找 `Dockerfile`，并根据其内容构建一个名为 `myimage` ，标签为 `1.0` 的镜像。

使用 `docker build` 命令可以根据 `Dockerfile` 构建出镜像，使得用户能够定制化构建过程，从而创建符合自身需求的镜像。

## 4.4、推送镜像到仓库

`docker push` 命令用于将本地构建的镜像推送（上传）到 Docker 仓库或其他容器镜像仓库中。这个命令将在 Docker 仓库中创建一个新的存储库，并将本地构建的镜像上传到该存储库中。

基本的 `docker push` 命令语法如下：

```
docker push [OPTIONS] NAME[:TAG]
```

其中，`NAME` 是要推送的镜像的名称，`TAG` 是该镜像的标签。

常用的选项包括：

+   `--all-tags`：推送镜像的所有标签

+   `--disable-content-trust`：禁用内容信任

例如，要将本地镜像 `myimage:1.0` 推送到 Docker Hub 上的仓库 `username/myimage` ，可以使用以下命令：

```
docker push username/myimage:1.0
```

注意：在执行这个命令之前，需要先使用 `docker login` 命令进行登录认证。

`docker login` 命令用于向 Docker 仓库进行身份验证，以便用户可以推送或拉取镜像。在执行 `docker push` 或 `docker pull` 命令之前，用户需要先使用 `docker login` 命令登录到 Docker 仓库。

基本的 `docker login` 命令语法如下：

```
docker login [OPTIONS] [SERVER]
```

其中，`SERVER` 是 Docker 仓库的地址。当未指定 `SERVER` 参数时，默认使用 Docker Hub。

当执行 `docker login` 命令后，用户需要提供其用户名和密码。一些 Docker 仓库还需要使用访问令牌（`access token`）或其他身份验证方法。

常用的选项包括：

+   `-u, --username`：指定用户名

+   `-p, --password`：指定密码

+   `--password-stdin`：从标准输入输入密码

例如，如果要登录到 Docker Hub，可以使用以下命令：

```
docker login
```

系统将提示用户输入用户名和密码。

如果要使用用户名和密码作为命令参数提供，可以使用以下命令：

```
docker login --username=myusername --password=mypassword
```

对于某些情况，可以通过 `--password-stdin` 选项将密码从标准输入输入入，例如：

```
echo &quot;mypassword&quot; | docker login --username=myusername --password-stdin
```

通过 `docker login` 命令可以登录到 Docker 仓库，从而可以进行镜像的推送和拉取操作。

通过 `docker push` 命令可以将本地构建的镜像推送到指定的仓库中，使得其他用户或系统可以通过仓库中的镜像进行部署、运行等操作。

## 4.5、删除镜像

`docker rmi` 命令用于删除本地的一个或多个镜像。使用该命令可以释放磁盘空间并删除不再需要的镜像。

基本的 `docker rmi` 命令语法如下：

```
docker rmi [OPTIONS] IMAGE [IMAGE...]
```

其中，`IMAGE` 是要删除的一个或多个镜像的名称或ID。

常用的选项包括：

+   `-f, --force`：强制删除，即使镜像正在被使用。

+   `--no-prune`：不删除未被任何容器引用的镜像。

+   `-a, --all`：删除所有镜像。

例如，要删除一个名为 `myimage` 的镜像，可以使用以下命令：

```
docker rmi myimage
```

如果要删除多个镜像，可以一次性指定多个镜像的名称：

```
docker rmi image1 image2
```

如果镜像正在被容器使用，需要使用 `-f` 选项强制删除：

```
docker rmi -f myimage
```

如果要删除所有镜像，可以使用 `-a` 选项：

```
docker rmi -a
```

通过 `docker rmi` 命令，用户可以方便地删除本地不再需要的镜像，释放磁盘空间。

# 五、网络和数据卷操作常用命令

## 5.1、管理网络

`docker network` 命令用于管理 Docker 中的网络。通过该命令可以创建、删除、列出和管理 Docker 的网络。

`docker network` 命令的一般语法：

```
docker network COMMAND [OPTIONS]
```

一些常用的 `docker network` 子命令包括：

+   `create`：创建一个新的网络

+   `ls` 或 `list`：列出所有可用的网络

+   `inspect`：显示有关给定网络的详细信息

+   `connect`：将容器连接到网络

+   `disconnect`：将容器从网络中断开连接

+   `rm` 或 `remove`：删除一个或多个网络

例如，创建一个名为 `my-network` 的新网络：

```
docker network create my-network
```

要列出所有可用的网络，可以使用以下命令：

```
docker network ls
```

要查看特定网络的详细信息，可以使用 `inspect` 子命令，例如：

```
docker network inspect my-network
```

连接容器到网络可以使用 `connect` 子命令，例如：

```
docker network connect my-network container1
```

从网络中断开容器连接可以使用 `disconnect` 子命令，例如：

```
docker network disconnect my-network container1
```

要删除不再需要的网络，可以使用 `rm` 子命令，例如：

```
docker network rm my-network
```

通过 `docker network` 命令可以轻松地管理 Docker 中的网络，包括创建新的网络、列出现有的网络、连接和断开容器到网络，以及删除不再需要的网络。

## 5.2、创建数据卷

`docker volume create` 命令用于在 Docker 中创建新的数据卷。数据卷是用于持久化存储容器数据的一种方式，它可以在容器之间共享数据，并且可以独立于容器的生命周期而存在。通过 `docker volume create` 命令，用户可以创建具有自定义名称和选项的新数据卷。

基本的 `docker volume create` 命令语法如下：

```
docker volume create [OPTIONS] [VOLUME]
```

其中，`VOLUME` 是要创建的数据卷的名称。

常用的选项包括：

+   `--driver`：指定数据卷的后端存储驱动（例如，本地、云存储等）

+   `-d, --driver-opt`：设置驱动选项

+   `--label`：为数据卷添加标签

+   `--opt`：设置驱动特定的选项

例如，要创建一个名为 `myvolume` 的新数据卷，可以使用以下命令：

```
docker volume create myvolume
```

如果要指定特定的后端存储驱动，可以使用 `--driver` 选项：

```
docker volume create --driver local --opt type=none --opt device=/root/sanbox/fly myvolume
```

通过 `docker volume create` 命令可以方便地创建新的数据卷，用于在容器之间共享和持久化存储数据。

## 5.3、挂载数据卷

`docker run -v` 命令用于在运行 Docker 容器时挂载数据卷，从而实现容器和数据卷之间的关联。挂载数据卷允许容器访问和使用数据卷中的文件。通过 `docker run -v` 命令可以在运行容器时将一个或多个数据卷挂载到容器中。

基本的 `docker run -v` 命令语法如下：

```
docker run -v [VOLUME:CONTAINER_PATH] CONTAINER_IMAGE
```

其中，`VOLUME` 是要挂载的数据卷的名称，`CONTAINER_PATH` 是容器中将数据卷挂载到的路径，`CONTAINER_IMAGE` 是要运行的容器的镜像名称。

如果要挂载本地目录作为数据卷，可以使用绝对路径：

```
docker run -v /root/sanbox:/container/path CONTAINER_IMAGE
```

如果要使用一个预先创建的数据卷，可以指定其名称：

```
docker run -v myvolume:/container/path CONTAINER_IMAGE
```

挂载多个数据卷时，可以多次使用 `-v` 选项：

```
docker run -v volume1:/path1 -v volume2:/path2 CONTAINER_IMAGE
```

通过 `docker run -v` 命令可以在运行容器时将一个或多个数据卷挂载到容器中，从而实现数据的持久化存储和共享。

# 六、容器日志和统计信息查看

## 6.1、查看容器日志

`docker logs` 命令用于查看 Docker 容器的日志输出。通过这个命令可以方便地查看容器内部的标准输出和标准错误输出，以便进行故障排查、性能分析或日志记录。

基本的 `docker logs` 命令语法如下：

```
docker logs [OPTIONS] CONTAINER
```

其中，`CONTAINER` 是要查看日志的容器的名称或 ID。

常用的选项包括：

+   `-f, --follow`：实时跟踪容器的日志输出

+   `--since`：显示自指定时间戳或相对于当前时间的时间范围内的日志

+   `--tail`：仅显示指定行数的日志

+   `--timestamps`：在日志中显示时间戳

例如，要查看名为 `mycontainer` 的容器的日志，可以使用以下命令：

```
docker logs mycontainer
```

输出：

```
2024-02-21 12:00:00: Application started
2024-02-21 12:05:00: Error: Unable to connect to database
2024-02-21 12:10:00: Warning: Disk space running low
2022-02-21 12:15:00: Connection established to database
```

如果要实时跟踪容器的日志输出，可以使用 `-f` 选项：

```
docker logs -f mycontainer
```

如果要仅显示最近的 20 行日志，可以使用 `--tail` 选项：

```
docker logs --tail 20 mycontainer
```

## 6.2、查看容器资源占用信息

`docker stats` 命令用于查看当前正在运行的容器的资源占用情况，包括 CPU 使用情况、内存使用情况、网络 I/O 和磁盘 I/O 等。通过这个命令可以实时监控容器的资源消耗情况，以便进行性能分析和资源调优。

基本的 `docker stats` 命令语法如下：

```
docker stats [OPTIONS] [CONTAINER...]
```

其中，`CONTAINER` 是要查看资源占用情况的容器的名称或 ID。

常用的选项包括：

+   `--all`：显示所有容器的资源占用情况，包括已停止的容器。

+   `--format`：指定输出格式。

例如，要查看名为 `mycontainer` 的容器的资源占用情况，可以使用以下命令：

```
docker stats mycontainer
```

如果要查看所有正在运行的容器的资源占用情况，可以使用 `--all` 选项：

```
docker stats --all
```

输出通常会类似于以下内容：

```
CONTAINER ID   NAME          CPU %     MEM USAGE / LIMIT     MEM %      NET I/O       BLOCK I/O         PIDS
2ca708c1       mycontainer   0.50%     300MiB / 1GiB         30.00%     3.24kB / 0B   64.3MB / 1.23MB   5
```

这里显示了容器的 ID、名称、CPU 使用率、内存使用情况、网络 I/O、磁盘 I/O 和进程数等信息。

通过 `docker stats` 命令可以实时监控容器的资源占用情况，了解各个容器的资源消耗情况，以便进行性能分析和资源管理。

# 七、Docker Compose常用命令

## 7.1、编排多个容器

`docker-compose` 是 `Docker Compose` 官方提供的工具，用于定义和运行多个容器的应用程序。`docker-compose` 工具使用一个名为 `docker-compose.yml` 的文件来定义应用程序的服务、网络和卷等配置，然后通过命令来管理多个容器的编排和运行。其中，`docker-compose up` 是 `docker-compose` 命令中的一个常用命令，用于启动和运行整个应用程序的容器。

`docker-compose up` 命令的基本语法如下：

```
docker-compose up [options] [SERVICE...]
```

其中，`SERVICE` 是要启动的服务的名称。如果不指定服务名称，则默认会启动 `docker-compose.yml` 文件中定义的所有服务。

`docker-compose up` 命令的常用选项包括：

+   `-d`：在后台运行容器

+   `--build`：构建镜像并启动容器

+   `--force-recreate`：强制重新创建容器

+   `--no-deps`：忽略依赖关系，仅启动指定的服务

例如，有一个名为 `docker-compose.yml` 的 `docker-compose` 配置文件，其中定义了多个服务，包括数据库服务和Web应用服务，要通过 `docker-compose up` 启动所有服务，可以使用以下命令：

```
docker-compose up
```

如果希望在后台运行容器，可以使用 `-d` 选项：

```
docker-compose up -d
```

如果只想启动其中的某几个服务，可以在命令中指定特定的服务名称：

```
docker-compose up webapp db
```

在启动过程中，`docker-compose up` 命令会读取 `docker-compose.yml` 文件，根据配置启动并编排多个容器，并将它们连接到正确的网络和卷上。这使得多个容器之间的交互和通信变得更加方便，并且整个应用程序可以更容易地进行编排和管理。

## 7.2、指定docker-compose文件

在某些情况下，可能会存在多个 `docker-compose.yml` 文件，或者希望使用不同的文件来定义不同的环境或配置。为了解决这些情况，`docker-compose` 提供了 `-f` 选项，允许指定要使用的 `docker-compose.yml` 文件。

`docker-compose -f` 命令的基本语法如下：

```
docker-compose -f &lt;文件路径&gt; [其他选项] &lt;子命令&gt; [参数]
```

其中，`<文件路径>` 是要使用的 `docker-compose.yml` 文件的路径。

例如，有一个名为 `docker-compose-dev.yml` 的 `docker-compose` 配置文件用于定义开发环境的服务，要使用该文件来运行 `docker-compose` ，可以使用以下命令：

```
docker-compose -f docker-compose-dev.yml up
```



这样会使用指定的文件来定义要运行的服务，而不是默认的 `docker-compose.yml` 文件。

`-f` 选项可以与其他 `docker-compose` 的子命令一起使用，比如 `build`、`down`、`ps` 等。

## 7.3、停止并移除服务

`docker-compose down` 是 Docker Compose 中的一个常用命令，用于停止并移除由 `docker-compose up` 启动的所有服务。它会关闭所有运行的容器，并且删除它们所使用的网络和卷等资源。

`docker-compose down` 命令的基本语法如下：

```
docker-compose down [options]
```

其中，`options` 是一些可选的选项，包括：

+ `-v, --volumes`：同时删除容器的卷。

+ `--rmi type`：移除镜像（all：移除所有镜像，local：移除本地镜像，none：不移除任何镜像）。

例如，要停止并移除由当前目录下的 `docker-compose.yml` 文件定义的所有服务，可以使用以下命令：

```
docker-compose down
```

输出：

```
Stopping myapp_web_1 ... done
Stopping myapp_db_1 ... done
Removing myapp_web_1 ... done
Removing myapp_db_1 ... done
Removing network myapp_default
```

如果要同时删除容器的卷，可以使用 `-v` 选项：

```
docker-compose down -v
```

此命令将停止并删除所有相关的容器，网络和卷，并释放系统资源。

# 总结

Docker 是当今流行的容器化平台，常用命令在使用 Docker 时非常重要。

1. `docker pull`：从镜像仓库拉取镜像，这是部署和创建容器的第一步。

1. `docker run`：运行容器，可以指定所需的镜像、端口、卷挂载等配置参数。

1. `docker ps`：列出所有正在运行的容器，提供了快速查看容器状态的方式。

1. `docker exec`：在正在运行的容器中执行命令，这可以方便地进入容器内部进行操作。

1. `docker stop` 和 `docker start`：停止和启动容器，对于管理运行中的容器非常重要。

1. `docker rm`：删除已停止的容器，释放资源。

1. `docker rmi`：删除镜像，释放空间。

1. `docker logs`：查看容器的日志输出，有助于排查问题。

1. `docker-compose`：用于编排和管理多个容器的工具，可以通过编写配置文件来定义和运行多个服务。

这些命令对于构建、运行和管理容器非常重要，掌握它们能够帮助更高效地利用 Docker 平台进行开发、测试和部署。同时，良好地理解这些命令也能够帮助更好地排查和解决在 Docker 中遇到的问题。

学习资料：

+ Docker官方文档(`https://docs.docker.com/`)提供了 Docker 平台的官方文档，其中包括了 Docker 引擎、Docker Compose、Docker Swarm 等各种工具的详细使用说明，以及最佳实践、常见问题解决方案等内容。通过阅读 Docker 官方文档可以更好地理解 Docker 平台的原理、功能和用法，从而更好地利用 Docker 进行容器化开发和部署。

+ 书籍：有许多优秀的书籍专门介绍了 Docker 技术和实践，例如《Docker 实战》、《Docker 实战指南》等。
