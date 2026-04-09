#  大雨：轻松上手 Docker：一站式指南助你成为容器化技术高手

> 来源：[https://waytoagi.feishu.cn/docx/X1AjdriYFoY437x3oWicxGEAnSf](https://waytoagi.feishu.cn/docx/X1AjdriYFoY437x3oWicxGEAnSf)

> 🔗 原文链接： h[ttps://www.iaiuse.](https://www.iaiuse.com/posts/442760ea.html)com/posts/442760...

> 发表时间：2024-3-30

> 一个小技巧，日常常用的命令存到 Notion 等常用工具中，好记性不如烂笔头。

> 如果遇到问题暂时没解决，千万不要熬夜，睡一觉醒来会发现有惊喜。

#  写在前面

AI 时代让我们每个人都有了探索世界的新方式。本文将引领你从选择合适的云服务器开始，深入到 Docker 的安装与应用，再到 Docker Compose 的高效多容器管理。我们不仅讲解如何通过 Portainer 等工具管理 Docker，还涉及文件备份、系统监控等实用技巧。此外，你将了解 Docker 与虚拟机的区别，掌握如何利用 Docker 在开发过程中实现快速、一致的环境部署。这不仅是一个指南，更是你 Docker 旅程中的伙伴。让我们一起探索 Docker 的强大能力，开启高效、有序的开发新篇章！

+   本文价值一个疯狂星期四

+   因为为了写它专门买了个服务器

+   工具推荐

+   finalshell*（有很多李鬼）

+   debian

#  1、准备工作，买服务器，安装 Docker

> 如果已经有云服务器，请直接跳到后面的安装部分，这里主要展示安全组和操作系统选择。

##  1.1、阿里云购买

[https://ecs-buy.ali](https://ecs-buy.aliyun.com/ecs/#/simple)yun.com/ecs/#/simple

直接点这个链接会提示我们登录，如果没有注册可以注册下，去年 11 的时候有活动 99 元一年的服务器（销售给我打电话，说赶快买，错过要等 1 年，结果从去年双 11 到现在，还是这个价格）

![](img/c063462a1d3927dfc70df3a34f1a1690.png)

注册成功了，输入上面这个链接，购买 ECS。

我已经买过 1 个了，这次演示就重新买了。因为没有找到 debian 的选项。选择自定义，选了最低配置，大家可以从这里入手，后面有需要再升配就好了，升配不用重装，网页上点点就好了。

![](img/0fc93f00a2c45501041982b1460c2e41.png)

选择操作系统，选择自己熟悉的，我比较习惯 Debian。选择最新版的就行

![](img/8d00f31eacfb4c1353fe1698a6416f95.png)

设置自动快照

![](img/7f033fda9cb98b53dc778c18eef01a5c.png)

建议设置，就是我们自己瞎搞八搞系统整坏了，恢复下昨天的正常版本，这样操作起来就完全无忧了。

设置安全组，为了后面方便使用，我们可以试下。自定义安全组 -20240329

![](img/f731a3c97642bfc79c0d1af8f7ea125b.png)

设置密码

![](img/39dc3b67855728c58d77701547d4dc1e.png)

整体检查一下，下单付款

![](img/930447f03004a1b9eb0d05777ab28a75.png)

要说不说，弄个 ECS 是挺贵的，主要一个起步，展示 Docker 如何使用，大家有环境就不用买它的了。

##  1.2、使用 FinallShell 连接服务器

购买完成以后，开始进入 ECS 界面找到刚刚的服务器，找到它的 IP 地址，121.40.138.224

![](img/0e68fd32f5420a1ba5498f4bfd594463.png)

> 注意：FinallShell 有很多李鬼站点

打开 [FinallShell ](http://www.hostbuf.com/)。添加新的连接

![](img/016188dc9de448b101d13006e52c0136.png)

添加服务器信息

![](img/e184bf907408655ec657e653551d8885.png)

确定后保存，然后双击连接，第一次连接会提示，接受并保存

![](img/0950c9e757834511d5ff362526b2b245.png)

登录成功

![](img/c31d0368b54c82c08a6ede088fdfc562.png)

升级安装插件（可选，如果报错就不折腾了，主要是为了弄 Docker）

```
apt update &amp;&amp; apt full-upgrade -y &amp;&amp; apt autoremove &amp;&amp; apt autoclean
apt install sudo htop git wget curl screen emacs vim ufw net-tools screen -y
```

![](img/7fe46bda756361114b68f186ca1a3758.png)

##  1.3、升级系统

系统升级一下

```
apt update
apt upgrade -y
apt install curl vim wget gnupg dpkg apt-transport-https lsb-release ca-certificates
```

![](img/046c5e8e09d1189aa1a4a1df50fe0941.png)

##  1.4、安装 DockerCE 和 Docker-Compose

> CE 就是社区版的意思

然后加入 Docker 的 GPG 公钥和 apt 源

```
curl -sSL https://download.docker.com/linux/debian/gpg | gpg --dearmor &gt; /usr/share/keyrings/docker-ce.gpg
echo &quot;deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-ce.gpg] https://download.docker.com/linux/debian $(lsb_release -sc) stable&quot; &gt; /etc/apt/sources.list.d/docker.list
```

![](img/c8a2e2683707b987c962c29d51913d88.png)

然后更新系统后即可安装 Docker CE：

```
apt update
apt install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

![](img/3d7bf43b6ed159fa6a13c62d2c77ee0b.png)

此时可以使用 docker version 命令检查是否安装成功

```
root@iZbp13svrytn8818ps7qwpZ:~
Client: Docker Engine - Community
 Version:           26.0.0
 API version:       1.45
 Go version:        go1.21.8
 Git commit:        2ae903e
 Built:             Wed Mar 20 15:18:01 2024
 OS/Arch:           linux/amd64
 Context:           default

Server: Docker Engine - Community
 Engine:
  Version:          26.0.0
  API version:      1.45 (minimum version 1.24)
  Go version:       go1.21.8
  Git commit:       8b79278
  Built:            Wed Mar 20 15:18:01 2024
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.6.28
  GitCommit:        ae07eda36dd25f8a1b98dfbf587313b99c0190bb
 runc:
  Version:          1.1.12
  GitCommit:        v1.1.12-0-g51d5e94
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
```

看下 docker-compose 版本

```
root@iZbp13svrytn8818ps7qwpZ:~
Docker Compose version v2.25.0
```

> 注意，这里安装的是 docker-compose 插件，命令行是用 docker compose 而不是 docker-compose

#  2、从最简单的例子开始—安装 Portainer

##  2.1、安装基础功能

> 本文旨在通过最基础的案例开始操作 Docker，后面会选择 Portainer 来做演示

开始命令行输入如下命令，开始安装：

```
docker run -d -p 9001:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock portainer/portainer-ce
```

![](img/b4b7ae0a1fea03b81ff00a8ea48b469b.png)

这个命令会做以下几件事：

+   -d ：后台运行容器。

+   -p 9001:9000 ：将容器的 9000 端口映射到宿主机的 9001 端口，Portainer 将通过宿主机的这个端口提供 Web 访问界面。

+   --name=portainer ：为容器指定一个名字，这里是 portainer 。

+   --restart=always ：确保 Docker 守护进程重启时容器自动启动。

+   -v /var/run/docker.sock:/var/run/docker.sock ：将宿主机的 Docker 套接字文件挂载到容器内的相同位置，使 Portainer 能够与 Docker 守护进程通信，管理 Docker 容器。

> 测试一下服务是否启动，因为它开放了 9001 端口

```
curl 127.0.0.1:9001
```

![](img/2a33e98be3b6b41e50c582c843d98b5c.png)

如果它没有启动或者启动失败，可能会是下面这样的结果，比如访问 8000 端口

```
curl 127.0.0.1:8000
```

![](img/bfa0777ca31a5a3a794d56b720401da2.png)

##  2.2、通过网页访问，打开安全组策略

默认情况下，我们在外面无法访问阿里云的机器，前面购买的时候打开了几个端口，但是没有 Portainer 的 9001 端口，需要打开安全策略，它可以通过阿里云的安全组进入，也可以从那个机器进入，方便期间，我们从 ECS 机器界面进入：

![](img/47873d768133499444f2caa8b1183775.png)

前面购买的时候给它创建了一个新的安全组，方便管理，点击管理规则开始设置

![](img/a79401247ab36216004e6d5ab7a43284.png)

加入 9001 端口的访问

![](img/06b0822c1092a681892c650e973dcdb5.png)

这个时候，我们就可以在自己电脑上，通过浏览器访问了

![](img/ae62b81389cd212c8f592a7546521902.png)

到此，第一个 docker 容器就安装好了。关于什么是容器，我们最后再来讲，接下来我们要删除这个容器。

##  2.3、停止容器删除容器

前面介绍了 docker run 命令，接下来我们看下，如何停止容器，为我们后续的演示做准备。

```
docker ps
```

这里命令用来查看现在系统中有哪些容器在运行

![](img/4e384301c30d5c4ee93b64b4bb23633a.png)

从这里我们可以看出来，有一个容器，它名字叫 portainer ，我们现在要停止它。

```
docker stop portainer
```

![](img/79ae799589d0c4732faa04c1dc5248df.png)

这个时候我们可以再次运行上面的命令，就发现已经没有容器了

```
docker ps
```

![](img/17d64c2aa4e762c5db4cb30c13b56ff1.png)

如果我们想看所有容器，包括停止的容器，可以在前面的命令后面加上 -a

```
docker ps -a
```

![](img/6b2a3816484f5b6634a18112838b37ee.png)

接下来，我们需要删除它，后面要重新建立

```
docker rm portainer
```

![](img/dcb92fde831ebd1b0506605bd92666eb.png)

这个时候我们就会发现， docker ps -a 也没有了

我们这样创建的容器有个问题，如果容器删除了，里面的数据也删除了，这就很麻烦了，所以我们需要把容器里面的数据，放在外面，不放在容器里面，这个时候就有一个概念叫文件映射。

##  2.4、文件映射（卷挂载）

> 关于什么是宿主机，什么是容器我们后面再展开。

文件映射我了解的有几种方式，一种是通过 docker 命令创建一个卷（volumn），另外就直接对应宿主机的路径，简单起见我们就直接用宿主机路径。通俗点理解就是桌面快捷方式。看起来在桌面，其实只是快捷方式，真正的东西在其他地方。

```
docker run -d -p 9001:9000 \
--name=portainer \
--restart=always \
-v /var/run/docker.sock:/var/run/docker.sock \
-v /opt/dokdt/portainer/data:/data \
portainer/portainer-ce
```

在这里，我们让容器里面的 /data，映射到主机的 /opt/dokdt/portainer/data。

![](img/e9d325a2c4578ea9cc477530858915d8.png)

通过 ls 命令，我们能看到，它自己创建了后面的这些文件夹，并把 protainer.db 文件放了进来。

这样设置以后，我们删除容器，数据还是保留在宿主机上。

##  2.5、Linux 命令行小技巧

+   docker rm p ，我们在命令输入这些，按住 tab 键，它会自动帮我们补齐后面的部分

+   我们在输入一个很长的命令行，比如前面的 docker run xxx 很长一段，想要回到最前面。

+   也就是 docker 这里，可以用快捷键 ctrl + a

+

![](img/d9e0cca84d7bf363f1a384401c6ca0db.png)

从前面这个案例我们发现，如果要调整命令行并不容易，参数很多，不好编辑，另外如果我们希望用 2 个容器分别来放数据库和 portainer 呢？这个时候有人就想到了，一个更加方便编辑的文件格式—yaml 格式，然后通过解析这个文件，形成 docker run 命令，这样就简化了整个过程。这就是 docker-compose。

> 同样的，我们先删除容器 docker stop portainer && docker rm portainer

#  3、Docker-Compose 多容器管理

##  3.1、单容器示例

网上的很多项目都提供了 docker-compose 的文件来操作，这里介绍的内容也是类似的。下面是一个 docker-compose 文件内容

```
version: &#x27;3.8&#x27;

services:
  portainer:
    image: portainer/portainer-ce
    container_name: portainer
    ports:
      - &quot;9001:9000&quot;
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - /opt/dokdt/portainer/data:/data
    restart: always
```

这个文件就叫 docker-compose.yml

```
docker compose up -d
```

![](img/43c5449ccb254dc9182d657cefa67c24.png)

这样就启动了容器，我们如果要调整，就修改这个文件，再执行一次 docker compose 就可以了。

有了这样一个文件，我们就有机会对一个服务器做整体编排了。比如一个应用，它需要 redis，需要 mysql，我们就可以放一个 yml 文件中去管理维护。比如最近比较流行的 fastgpt。

##  3.2、多容器示例

```
version: &#x27;3.3&#x27;
services:
  pg:
    image: ankane/pgvector:v0.5.0 
    
    container_name: pg
    restart: always
    ports: 
      - 5432:5432
    networks:
      - fastgpt
    environment:
      
      - POSTGRES_USER=username
      - POSTGRES_PASSWORD=password
      - POSTGRES_DB=postgres
    volumes:
      - ./pg/data:/var/lib/postgresql/data
  mongo:
    image: registry.cn-hangzhou.aliyuncs.com/fastgpt/mongo:5.0.18
    container_name: mongo
    restart: always
    ports:
      - 27017:27017
    networks:
      - fastgpt
    command: mongod --keyFile /data/mongodb.key --replSet rs0
    environment:
      - MONGO_INITDB_ROOT_USERNAME=myusername
      - MONGO_INITDB_ROOT_PASSWORD=mypassword
    volumes:
      - ./mongo/data:/data/db
    entrypoint:
      - bash
      - -c
      - |
        openssl rand -base64 128 &gt; /data/mongodb.key
        chmod 400 /data/mongodb.key
        chown 999:999 /data/mongodb.key
        echo &#x27;const isInited = rs.status().ok === 1
        if(!isInited){
          rs.initiate({
              _id: &quot;rs0&quot;,
              members: [
                  { _id: 0, host: &quot;mongo:27017&quot; }
              ]
          })
        }&#x27; &gt; /data/initReplicaSet.js
        # 启动MongoDB服务
        exec docker-entrypoint.sh &quot;$@&quot; &amp;

        
        until mongo -u myusername -p mypassword --authenticationDatabase admin --eval &quot;print(&#x27;waited for connection&#x27;)&quot; &gt; /dev/null 2&gt;&amp;1; do
          echo &quot;Waiting for MongoDB to start...&quot;
          sleep 2
        done

        
        mongo -u myusername -p mypassword --authenticationDatabase admin /data/initReplicaSet.js

        
        wait $!
  fastgpt:
    container_name: fastgpt
    image: registry.cn-hangzhou.aliyuncs.com/fastgpt/fastgpt:v4.7 
    
    ports:
      - 3000:3000
    networks:
      - fastgpt
    depends_on:
      - mongo
      - pg
    restart: always
    environment:
      
      - DEFAULT_ROOT_PSW=1234
      
      - OPENAI_BASE_URL=http://oneapi:3000/v1
      
      - CHAT_API_KEY=sk-fastgpt
      
      - DB_MAX_LINK=30
      
      - TOKEN_KEY=any
      
      - ROOT_KEY=root_key
      
      - FILE_TOKEN_KEY=filetoken
      
      - MONGODB_URI=mongodb://myusername:mypassword@mongo:27017/fastgpt?authSource=admin
      
      - PG_URL=postgresql://username:password@pg:5432/postgres
    volumes:
      - ./config.json:/app/data/config.json
      - ./fastgpt/tmp:/app/tmp
  mysql:
    image: mysql:8.0.36
    container_name: mysql
    restart: always
    ports:
      - 3306:3306
    networks:
      - fastgpt
    command: --default-authentication-plugin=mysql_native_password
    environment:
      
      MYSQL_ROOT_PASSWORD: oneapimmysql
      MYSQL_DATABASE: oneapi
    volumes:
      - ./mysql:/var/lib/mysql
  oneapi:
    container_name: oneapi
    image: ghcr.io/songquanpeng/one-api:latest
    ports:
      - 3001:3000
    depends_on:
      - mysql
    networks:
      - fastgpt
    restart: always
    environment:
      
      - SQL_DSN=root:oneapimmysql@tcp(mysql:3306)/oneapi
      
      - SESSION_SECRET=oneapikey
      
      - MEMORY_CACHE_ENABLED=true
      
      - BATCH_UPDATE_ENABLED=true
      
      - BATCH_UPDATE_INTERVAL=10
      
      - INITIAL_ROOT_TOKEN=fastgpt
    volumes:
      - ./oneapi:/data
networks:
  fastgpt:
```

这个文件很复杂，完全搞懂需要点时间，我们挑重点部分来讲就可以了。它涵盖了多个服务，包括数据库（PostgreSQL 和 MongoDB）、一个 AI 模型服务（ fastgpt ）、MySQL 数据库以及一个 API 服务（ oneapi ）。为了帮助初学者更好地理解这个文件，我们将逐个解释其中包含的几个关键点： networks 、 environment 、 command 、 entrypoint 和 depends_on 。

###  Networks

+   **作用 **：在 Docker Compose 文件中定义网络，允许容器间通过名称而非 IP 地址进行通信。这提高了服务间通信的可读性和管理便捷性。

+   **示例中的应用 **： fastgpt 网络连接了所有服务，确保它们可以互相发现和通信。

###  Environment

+   **作用 **：用于设置环境变量，这些变量可以被容器内运行的应用程序读取。环境变量常用于配置应用行为或传递敏感信息（如数据库密码）。

+   **示例中的应用 **：每个服务通过 environment 配置数据库凭据、API 密钥等信息，如 POSTGRES_USER 和 POSTGRES_PASSWORD 为 PostgreSQL 服务设置用户名和密码。

###  Command

+   **作用 **：覆盖容器启动后默认执行的命令。这对于自定义容器的启动行为或传递额外参数至应用非常有用。

+   **示例中的应用 **：在 mongo 服务中， command 用于启动 MongoDB 服务并配置键文件和复制集设置。

###  Entrypoint

+   **作用 **：指定容器启动时执行的首个命令，可以是一个命令或一个脚本。 entrypoint 与 command 的区别在于 entrypoint 定义了容器的入口点，而 command 则提供了传递给 entrypoint 的参数。

+   **示例中的应用 **： mongo 服务使用 entrypoint 执行一个 bash 脚本，该脚本生成加密密钥，修改文件权限，初始化 MongoDB 的复制集，等待 MongoDB 启动完成后再执行后续操作。

###  Depends_on

+   **作用 **：定义服务之间的依赖关系。使用 depends_on 可以指定一个服务在其他服务启动后再启动，但不保证服务完全就绪。

+   **示例中的应用 **： fastgpt 和 oneapi 服务分别使用 depends_on 确保在它们依赖的数据库服务（ mongo 、 pg 、 mysql ）启动之后再启动。

通过理解这些关键概念，我们可以更好地掌握 Docker Compose 文件的结构和功能，从而有效地编排复杂的多容器应用。这个示例展示了一个典型的开发环境配置，包括数据库服务、应用服务和他们之间的依赖关系，是学习 Docker Compose 的一个很好的起点。

##  3.3、补充说明

docker compose 默认会调用 docker-compose.yml 这个文件，如果我们有多个 yml 文件怎么办呢？通过-f 参数就可以了

```
docker compose -f ./fastgpt.yml up -d
```

##  3.4、Linux 命令行小技巧

+   本机和服务器之间要传递文件，FinallShell 可以直接操作。

![](img/b4b85d0d03d4a7011338c9f13146271e.png)

+

+   vim 小技巧
vim 大部分情况下不需要用，特别是 finallshell 还这么方便的情况下。平常用到的几个小技巧。
gg ： 回到第一行
G： 到最后一行
x： 删除一个字符 xn： n 是几就删除几个字符
dd：删除一行
u： 还原上一次操作
i：进入编辑状态
esc：退出编辑状态%s/原始内容/要替换的内容/g
这里的 g 是全局替换的意思

> 总的来说，对于我们普通人没啥必要学，实在手痒可以玩一下

Portainer 是一个非常好用的 Docker 管理工具，如果不是很习惯命令行方式，它可以帮忙降低使用难度，这部分内容如果有需求可以留言给我，再做补充。

对于我们爱好者来说，系统搭建好了，就好了，其实事情才刚刚开始。

#  4、Docker 问题排查和日常维护

##  4.1、日志查看和问题排查

###  Docker logs

容器启动以后，如果出现异常，我们可以通过日志来判断

```
docker logs portainer
```

![](img/c3bdb06bd283d91f08fc04366d88d4a8.png)

通过这个命令我们可以看到容器的日志，排查问题所在。

如果我想要进入 docker 内部要如何做呢？

> 这里有一个场景，比如我们不确定一个配置文件是否正确映射了，我们可以进入容器去查看文件内容。

###  Docker exec

要进入 Docker 容器内部，通常使用 docker exec 命令，这允许您在运行中的容器内执行命令。最常见的用途之一是启动一个交互式的 bash 会话，这样您就可以在容器内部以交互方式运行命令，就像在一个常规的 Linux 环境中一样。

####  示例命令

假设您的容器名为 fastgpt ，要进入该容器，可以使用以下命令：

```
docker exec -it fashgpt /bin/bash
```

这里的参数解释如下：

+   docker exec ：Docker 的命令，用于在运行中的容器内执行命令。

+   -it :

+   -i 或 --interactive 保持标准输入（STDIN）开放，即使不附加到终端也是如此，允许您与会话互动。

+   -t 或 --tty 分配一个伪终端，这使得您能够像在常规终端中一样与容器进行交互。

+   mycontainer ：目标容器的名称或 ID。

+   /bin/bash ：您希望在容器内执行的命令，这里是启动 bash。如果容器中没有 bash，您也可以尝试使用 /bin/sh 或其他可用的 shell。

####  进入容器内部

执行上述命令后，您的命令行界面将变为容器内部的 bash 会话，您可以在容器内运行命令、查看文件系统等，就好像您正在操作一个独立的 Linux 系统一样。要退出容器会话，可以简单地键入 exit 命令或使用 Ctrl+D 快捷键。

####  注意事项

+   并非所有的容器都会包含 bash 或 sh，这取决于容器镜像是如何构建的。如果容器镜像基于非常轻量级的基础镜像（如 alpine ），可能只有 /bin/sh 可用。

+   使用 docker exec 进入容器主要用于调试和管理任务。对于希望与容器持续交互的应用场景，应考虑其他方法，如在容器启动时直接运行交互式应用。

通过 docker exec 命令，Docker 提供了一种便捷的方式来直接与运行中的容器进行交互，这在开发和调试过程中尤其有用。

容器开始运行以后，会产生很多数据，所以备份这些数据就是我们需要考虑的问题了，另外还有一个文件是硬盘爆了咋办？现在有很多工具帮忙看，云厂商有工具，类似宝塔这样的工具也能提供能力。下面的方法比较原始一些。

##  4.2、文件备份和系统监控

有了上面的知识，我们只需要备份文件映射的部分就可以了。

```
#!/bin/bash


BACKUP_DATE=$(date +&quot;%Y-%m-%d&quot;)


BACKUP_PATH=&quot;/opt/dokdt/portainer_backup_$BACKUP_DATE.tar.gz&quot;



docker run --rm \
    -v /opt/dokdt/portainer/data:/data \
    -v $BACKUP_PATH:/backup.tar.gz \
    busybox tar czvf /backup.tar.gz /data

echo &quot;Portainer数据备份完成，备份文件位于: $BACKUP_PATH&quot;
```

这个脚本现在不用自己写了，丢给 GPT 去弄就好了。把我们的需求描述清楚，比如说我们只保留最近 7 天的备份？也是可以的，这个功能一般国内云厂商都有类似服务，省点我们自己写也可以。

系统监控可以用 uptime 这样的开源项目，也是个 docker

##  4.3、Linux 命令行小技巧

随时时间推移，硬盘数据会越来越多，如果没有其他的手段，下面介绍几个古早就有的工具。

###  top 命令，按 q 退出

![](img/ac7d653dd5a8187f04347b47677c5554.png)

这个命令里面，简单点就是按 c/m 看内容和 cpu 占比等

###  free 命令看内存

![](img/dc79d305a22eebe0ec5ed1dbb6a00db5.png)

###  df 显示文件系统的磁盘空间使用情况

![](img/2dc8163a98fca5c9088f15f18cd1ce15.png)

###  du 估算文件和目录占用的磁盘空间

**用途 **：估算文件和目录占用的磁盘空间。

![](img/7ba10366d234a1d6082e1278d169c009.png)

#  5、Docker 和主机的关系，和 VM 的区别

Docker 的出现是为了解决“在我的机器上能运行，但是在你的机器上却不能运行”的常见问题，这个问题通常被称为“环境地狱”。在 Docker 之前，软件开发和部署面临着许多挑战，包括环境不一致、依赖冲突、部署复杂等。Docker 通过提供一个轻量级的、可移植的、自给自足的容器来封装应用和其依赖，使得应用能够在任何支持 Docker 的环境中无缝运行。

##  5.1、Docker 出现的原因和要解决的问题

+   **环境一致性 **：确保开发、测试、生产环境保持一致，减少“在我机器上运行正常”问题的出现。

+   **依赖管理 **：通过容器封装应用及其依赖，简化依赖管理。

+   **部署简化 **：提供快速、一致的部署流程，提高部署效率和可靠性。

+   **资源隔离 **：确保应用之间的资源隔离，提高系统的安全性和稳定性。

+   **资源利用率 **：与传统虚拟机相比，容器化能更高效地利用系统资源。

##  5.2、Docker、VM 和主机之间的关系、区别和联系

+   **与虚拟机（VM）的区别 **：

+   **资源开销 **：VM 包括应用、必要的二进制文件和库，以及整个操作系统，而 Docker 容器共享宿主机的操作系统，只包含应用和其依赖，因此更加轻量级。

+   **启动速度 **：容器可以在几秒钟内启动，而虚拟机可能需要几分钟。

+   **性能 **：容器几乎没有额外的性能开销，而虚拟机则因为需要额外的操作系统，会有一定的性能损耗。

+   **与主机的关系 **：

+   Docker 容器运行在宿主机上，它们通过 Docker 引擎与宿主机的操作系统交互，利用宿主机的内核功能（如 cgroups 和 namespaces）实现资源隔离和管理。

+   容器视角下的“主机”是指容器内部的环境，这为应用提供了一个隔离的运行时环境，虽然与宿主机共享内核，但在许多方面表现得就像是一个独立的机器。

总的来说，Docker 的出现极大地简化了软件的打包、分发、安装和运行过程，通过容器化技术，促进了 DevOps 文化的发展，加速了软件开发和部署的现代化进程。

> 说得这么好，很显然，这部分内容都是 GPT 写的。

#  6、后记

一个特殊的机缘，可能是 rootless，用了 podman，目前主力工具是 podman，为了写这篇笔记专门装了 docker。纰漏之处在所难免。

反向代理一直用的 nginx，但是 nginx proxy manager 遇到多好几次坑，看起来镜像 1G，但是占用空间有 10G，而且 inode 很快就消耗光了，硬生生逼着用了 caddy。暂时还好。

如果有什么需要进一步解答的，可以留言。

以下是一个总结表格，列出了一些常用的 Docker 命令及其用途：

| 命令 | 用途 |
| --- | --- |
| docker run [options] image [command] | 创建一个新的容器并运行一个命令 |
| docker start container | 启动一个或多个已停止的容器 |
| docker stop container | 停止一个运行中的容器 |
| docker restart container | 重启容器 |
| docker rm container | 删除一个或多个容器 |
| docker ps [options] | 列出容器 |
| docker exec [options] container command | 在运行的容器中执行命令 |
| docker logs [options] container | 获取容器的日志 |