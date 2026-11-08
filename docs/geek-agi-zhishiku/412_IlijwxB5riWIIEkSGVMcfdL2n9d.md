# 【容器编排魔法】Docker容器编排神器Componse完全解析！

> 来源：[https://geekbang.feishu.cn/docx/GXSsdJNUto3O3exD8YvcDAXxnNe](https://geekbang.feishu.cn/docx/GXSsdJNUto3O3exD8YvcDAXxnNe)

> 👨🏻 作者：Lion莱恩呀

> 📃 作者介绍：专注分享高性能服务器后台开发知识，包括C/C++、Linux、网络协议、设计模式、中间件、云原生、数据库、分布式架构等领域知识。

> 🛖 👸号：Lion莱恩呀

> ⏰ 收录时间：2024-05-27 11:15:56 (UTC+8)

> 🔗 原文链接：h[ttps://mp.weixin.q](https://mp.weixin.qq.com/s?__biz=Mzk0ODUzNDc2Mw==&mid=2247485884&idx=1&sn=f38d6230232d9faac60e9d84e50a7579&scene=19#wechat_redirect)q.com/s?__biz=Mz...

![](img/278cedadd31f2fd57106fee97c84a8d7.png)

关注作者，获取更多相关知识

# 一、前言

Docker Compose是Docker官方提供的一个用于定义和运行多个Docker容器的工具。它采用YAML文件来配置应用的服务，在一个文件中定义一组相关联的容器化应用，然后通过一条命令即可使用该文件来创建、启动、停止以及删除整个应用的容器，非常适合用于开发、测试和部署环境。

相关文档：

+   docker-compose 命令行

+   docker-compose 配置文件

# 二、Compose 简介

Docker Compose 是 Docker 官方编排（Orchestration）项目之一，负责快速的部署分布式应用。

Compose 项目是 Docker 官方的开源项目，负责实现对 Docker 容器集群的快速编排。其代码目前在github上开源。

Compose 定位是 【定义和运行多个 Docker 容器的应用（Defining and running multi-container Docker applications）】，其前身是开源项目 Fig。

我们知道使用一个 Dockerfile 模板文件，可以让用户很方便的定义一个单独的应用容器。然而，在日常工作中，经常会碰到需要多个容器相互配合来完成某项任务的情况。例如要实现一个 Web 项目，除了Web 服务容器本身，往往还需要再加上后端的数据库服务容器，甚至还包括负载均衡容器等。

Compose 恰好满足了这样的需求。它允许用户通过一个单独的 docker-compose.yml 模板文件（YAML 格式）来定义一组相关联的应用容器为一个项目（project）。

Compose 中有两个重要的概念：

+   服务 (service)：一个应用的容器，实际上可以包括若干运行相同镜像的容器实例。

+   项目 (project)：由一组关联的应用容器组成的一个完整业务单元，在 docker-compose.yml 文件中定义。

Compose的默认管理对象是项目，通过子命令对项目中的一组容器进行便捷地生命周期管理。

Compose 项目由 Python 编写，实现上调用了 Docker 服务提供的 API 来对容器进行管理。因此，只要所操作的平台支持 Docker API，就可以在其上利用 Compose 来进行编排管理。

# 三、compose的安装和卸载

## 3.1、安装

在 Linux 上的也安装十分简单，从 GitHub Release处直接下载编译好的二进制文件即可。

例如，在 Linux 64 位系统上直接下载对应的二进制包。

（1）先把docker-compose文件下载到当前目录：

```
wget https://github.com/docker/compose/releases/download/v2.12.2/docker-compose-linux-x86_64
```

（2）然后拷贝到/usr/bin/ 。

```
sudo cp -arf docker-compose-linux-x86_64 /usr/bin/docker-compose
sudo chmod +x /usr/bin/docker-compose
```

## 3.2、卸载

如果是二进制包方式安装的，删除二进制文件即可。

```
sudo rm /usr/bin/docker-compose
```

## 3.3、使用

选项：

```
Usage:  docker compose [OPTIONS] COMMAND

Docker Compose

Options:
      --ansi string                Control when to print ANSI control characters (&quot;never&quot;|&quot;always&quot;|&quot;auto&quot;) (default &quot;auto&quot;)
      --compatibility              Run compose in backward compatibility mode
      --env-file string            Specify an alternate environment file.
  -f, --file stringArray           Compose configuration files
      --profile stringArray        Specify a profile to enable
      --project-directory string   Specify an alternate working directory
                                   (default: the path of the, first specified, Compose file)
  -p, --project-name string        Project name

Commands:
  build       Build or rebuild services
  convert     Converts the compose file to platform&#x27;s canonical format
  cp          Copy files/folders between a service container and the local filesystem
  create      Creates containers for a service.
  down        Stop and remove containers, networks
  events      Receive real time events from containers.
  exec        Execute a command in a running container.
  images      List images used by the created containers
  kill        Force stop service containers.
  logs        View output from containers
  ls          List running compose projects
  pause       Pause services
  port        Print the public port for a port binding.
  ps          List containers
  pull        Pull service images
  push        Push service images
  restart     Restart service containers
  rm          Removes stopped service containers
  run         Run a one-off command on a service.
  start       Start services
  stop        Stop services
  top         Display the running processes
  unpause     Unpause services
  up          Create and start containers
  version     Show the Docker Compose version information

Run &#x27;docker compose COMMAND --help&#x27; for more information on a command.
```

compose的关键选项：

+   -p：指定项目名称。

+   -f：指定配置文件。

建议：单独的项目放在一个文件夹下，而不指定-p和-f参数，这样用起来比较方便；因为如果指定这两个选项，那么在项目操作过程中需要每次都要指定这两个参数，否则compose指令就识别不到项目。所以直接以默认的方式（以当前文件夹作为项目名称，当前文件夹下的docker-compose.yml作为配置文件）执行。

# 四、yml 配置指令参考

以下内容只作为参考；具体参见官方文档。

（1） **version **；Compose 文件格式有3个版本,分别为1, 2.x 和 3.x 目前主流的为 3.x 其支持 docker 1.13.0 及其以上的版本。

（2） **build **；指定镜像构建参数，其作用与docker build 命令一样。

比如：

```
build: ./dir
```

或

```
build:
        # 上下文路径
        context: .
        # 指定构建镜像的 Dockerfile 文件名
        dockerfile: Dockerfile-compose
        # 添加构建参数，这是只能在构建过程中访问的环境变量。
        args:
        - http_proxy: https://proxy.golang.com.cn,https://goproxy.cn,direct
        # 设置构建镜像的标签。
        labels:
        - &quot;myhello:1.0&quot;
        - &quot;com.example.description=Accounting webapp&quot;
        # target：多层构建，可以指定构建哪一层。
        # tags：指定镜像的tags
        tags:
        - &quot;myhello:1.0.0&quot;
        - &quot;localhost:5000/myhello:1.0.0&quot;
```

（3） **command **；覆盖容器启动的默认命令。

```
command: [&quot;bundle&quot;, &quot;exec&quot;, &quot;thin&quot;, &quot;-p&quot;, &quot;3000&quot;]
```

（4） **container_name **；指定自定义容器名称，而不是生成的默认名称。

```
container_name: my-web-container
```

（5） **depends_on **；设置依赖关系。

+   docker-compose up ：以依赖性顺序启动服务。在以下示例中，先启动 db 和 redis ，才会启动web。

+   docker-compose up SERVICE ：自动包含 SERVICE 的依赖项。在以下示例中，docker-compose up web 还将创建并启动 db 和 redis。

+   docker-compose stop ：按依赖关系顺序停止服务。在以下示例中，web 在 db 和 redis 之前停止。

```
version: &quot;3.7&quot;
services:
        web:
                build: .
                depends_on:
                        - db
                        - redis
        redis:
                image: redis
        db:
                image: postgres
```

注意：web 服务不会等待 redis、db 完全启动 之后才启动。

（6） **entrypoint **；覆盖容器默认的 entrypoint。

```
entrypoint: /code/entrypoint.sh
```

也可以是以下格式：

```
entrypoint:
        - php
        - -d
        - zend_extension=/usr/local/lib/php/extensions/no-debug-non-zts-        20100525/xdebug.so
        - -d
        - memory_limit=-1
        - vendor/bin/phpunit
```

（7） **env_file **；从文件添加环境变量，可以是单个值或列表的多个值。

```
env_file: .env
```

也可以是列表格式：

```
env_file:
        - ./common.env
        - ./apps/web.env
        - /opt/secrets.env
```

（8） **environment **；添加环境变量。您可以使用数组或字典、任何布尔值，布尔值需要用引号引起来，以确保 YML 解析器不会将其转换为 True 或 False。

```
environment:
        RACK_ENV: development
        SHOW: &#x27;true&#x27;
```

（9） **expose **；暴露端口，但不映射到宿主机，只被连接的服务访问。

仅可以指定内部端口为参数：

```
expose:
        - &quot;3000&quot;
        - &quot;8000&quot;
```

（10） **extra_hosts **；添加主机名映射。类似 docker run --add-host。

```
extra_hosts:
        - &quot;somehost:162.242.195.82&quot;
        - &quot;otherhost:50.31.209.229&quot;
```

以上会在此服务的内部容器中 /etc/hosts 创建一个具有 ip 地址和主机名的映射关系：

```
162.242.195.82 somehost
50.31.209.229 otherhost
```

（11） **healthcheck **；用于检测 docker 服务是否健康运行。

```
healthcheck:
        test: [&quot;CMD&quot;, &quot;curl&quot;, &quot;-f&quot;, &quot;http://localhost&quot;] # 设置检测程序
        interval: 1m30s # 设置检测间隔
        timeout: 10s # 设置检测超时时间
        retries: 3 # 设置重试次数
        start_period: 40s # 启动后，多少秒开始启动检测程序
```

（12） **image **；指定容器运行的镜像。

以下格式都可以：

```
image: redis
image: ubuntu:14.04
image: tutum/influxdb
image: example-registry.com:4000/postgresql
image: a4bc65fd # 镜像id
```

（13） **logging **；服务的日志记录配置。

driver：指定服务容器的日志记录驱动程序，默认值为json-file。有以下三个选项。

```
driver: &quot;json-file&quot;
driver: &quot;syslog&quot;
driver: &quot;none&quot;
```

仅在 json-file 驱动程序下，可以使用以下参数，限制日志得数量和大小。

```
logging:
        driver: json-file
        options:
                max-size: &quot;200k&quot; # 单个文件大小为200k
                max-file: &quot;10&quot; # 最多10个文件
```

当达到文件限制上限，会自动删除旧得文件。

syslog 驱动程序下，可以使用 syslog-address 指定日志接收地址。

```
logging:
        driver: syslog
        options:
                syslog-address: &quot;tcp://192.168.0.106:123&quot;
```

（14） **restart。**

+   no：当容器退出时，不要自动重新启动

+   on-failure[:max-retries]：仅当容器以非零退出状态退出时才重新启动。

+   always：无论退出状态如何，始终重新启动容器。无论容器的当前状态如何，容器也将始终在守护程序启动时启动

+   unless-stopped：论退出状态如何，始终重新启动容器，包括守护程序启动时，除非容器在Docker守护程序停止之前处于停止状态。

```
restart: &quot;no&quot;
restart: always
restart: on-failure
restart: unless-stopped
```

（15） **volumes **；将主机的数据卷或着文件挂载到容器里。

```
version: &quot;3.7&quot;
services:
        db:
                image: postgres:latest
                volumes:
                        - &quot;/localhost/postgres.sock:/var/run/postgres/postgres.sock&quot;
                        - &quot;/localhost/data:/var/lib/postgresql/data&quot;
```

（16） **networks **；为容器指定网络。

```
services:
        frontend:
                image: awesome/webapp
                networks:
                        front-tier:
                                ipv4_address: 172.16.238.10
                                ipv6_address: 2001:3984:3989::10
networks:
        front-tier:
                ipam:
                        driver: default
                        config:
                                - subnet: &quot;172.16.238.0/24&quot;
                                - subnet: &quot;2001:3984:3989::/64&quot;
```

# 五、Compose 命令说明

## 5.1、命令对象与格式

官方文档。

对于 Compose 来说，大部分命令的对象既可以是项目本身，也可以指定为项目中的服务或者容器。如果没有特别的说明，命令对象将是项目，这意味着项目中所有的服务都会受到命令影响。

执行 docker-compose [COMMAND] --help 或者 docker-compose help [COMMAND] 可以查看具体某个

命令的使用格式。

docker-compose 命令的基本的使用格式是

```
docker-compose [-f=&lt;arg&gt;...] [options] [COMMAND] [ARGS...]
```

## 5.2、命令选项

+   -f, --file FILE 指定使用的 Compose 模板文件，默认为 docker-compose.yml ，可以多次指定。

+   -p, --project-name NAME 指定项目名称，默认将使用所在目录名称作为项目名。

## 5.3、命令使用详细说明

**（1）build。 **格式为：

```
docker-compose build [options] [SERVICE...]
```

构建或重新构建项目中service的镜像。构建之后镜像不会被应用到容器，必须将项目down之后，再重新up方能使用最新镜像。构建的镜像会被加上项目名称的前缀。

选项包括：

+   --force-rm ：删除构建过程中的临时容器。

+   --no-cache ：构建镜像过程中不使用 cache（这将加长构建过程）。

+   --pull ：始终尝试通过 pull 来获取更新版本的镜像。

+   --build-arg ：设置构建参数

（2）config。验证 Compose 文件格式是否正确，若正确则显示配置，若格式错误显示错误原因。

（3）down。此命令将会停止 up 命令所启动的容器，并移除网络。

（4）exec。进入指定的容器。

（5）help。获得一个命令的帮助。

（6）images。列出 Compose 文件中包含的镜像。

（7）kill。通过发送 SIGKILL 信号来强制停止服务容器。格式为：

```
docker-compose kill [options] [SERVICE...]
```

支持通过 -s 参数来指定发送的信号，例如通过如下指令发送 SIGINT 信号。

```
docker-compose kill -s SIGINT
```

（8）logs。查看服务容器的输出。格式为：

```
docker-compose logs [options] [SERVICE...] 
```

默认情况下，docker-compose 将对不同的服务输出使用不同的颜色来区分。

可以通过 --no-color 来关闭颜色。

该命令在调试问题的时候十分有用。

（9）pause。暂停一个服务容器。格式为：

```
docker-compose pause [SERVICE...] 
```

（10）port。打印某个容器端口所映射的公共端口。格式为：

```
docker-compose port [options] SERVICE PRIVATE_PORT
```

选项：

+   --protocol=proto 指定端口协议，tcp（默认值）或者 udp。

+   --index=index 如果同一服务存在多个容器，指定命令对象容器的序号（默认为 1）。

（11）ps。列出项目中目前的所有容器。格式为：

```
docker-compose ps [options] [SERVICE...]
```

选项：- q 只打印容器的 ID 信息。

（12）pull。拉取服务依赖的镜像。格式为：

```
docker-compose pull [options] [SERVICE...] 
```

--igno re-pull-failures选项：忽略拉取镜像过程中的错误。

（13）push。推送服务依赖的镜像到 Docker 镜像仓库。

（14）restart。重启项目中的服务。格式为：

```
docker-compose restart [options] [SERVICE...] 
```

- t, --timeout TIMEOUT 选项 ： 指定重启前停止容器的超时（默认为 10 秒）。

（15）rm。删除所有（停止状态的）服务容器。推荐先执行 docker-compose stop 命令来停止容器。格式为：

```
docker-compose rm [options] [SERVICE...] 
```

选项：

+   -f, --force 强制直接删除，包括非停止状态的容器。一般尽量不要使用该选项。

+   -v 删除容器所挂载的数据卷。

**（16）run **。在指定服务上执行一个命令，类似docker run指令。格式为：

```
docker-compose run [options] [-p PORT...] [-e KEY=VAL...] SERVICE [COMMAND] [ARGS...] 
```

示例：

```
docker-compose run redis pwd
```

将会基于指定的service的镜像启动一个新的容器，并且运行指定命令。

默认情况下，如果存在关联，则所有关联的服务将会自动被启动，除非这些服务已经在运行中。

该命令类似启动容器后运行指定的命令，相关卷、链接等等都将会按照配置自动创建。

两个不同点：

+   给定命令将会覆盖原有的自动运行命令；

+   不会自动创建端口，以避免冲突。

如果不希望自动启动关联的容器，可以使用 --no-deps 选项，例如：

```
# 该操作为伪操作
docker-compose run --no-deps web pwd
```

将不会启动 web 容器所关联的其它容器。

选项：

+   -d 后台运行容器。

+   --name NAME 为容器指定一个名字。

+   --entrypoint CMD 覆盖默认的容器启动指令。

+   --e KEY=VAL 设置环境变量值，可多次使用选项来设置多个环境变量。

+   -u, --user=“” 指定运行容器的用户名或者 uid。

+   --no-deps 不自动启动关联的服务容器。

+   --rm 运行命令后自动删除容器， d 模式下将忽略。

+   -p, --publish=[] 映射容器端口到本地主机。

+   --service-ports 配置服务端口并映射到本地主机。

+   -T 不分配伪 tty，意味着依赖 tty 的指令将无法运行。

（17 ）start。启动已经存在的服务。格式为：

```
docker-compose start [SERVICE...]
```

（18）stop。停止已经处于运行状态的服务，但不删除它。通过 docker-compose start 可以再次启动这些服务。格式为：

```
docker-compose stop [options] [SERVICE...] 
```

-t, --timeout TIMEOUT 选项： 停止容器时候的超时（默认为 10 秒）。

（19）top。查看各个服务内运行的进程。

（20）unpause。恢复处于暂停状态中的服务。格式为：

```
docker-compose unpause [SERVICE...] 
```

（21）version。打印版本信息。格式为：

```
docker-compose version
```

**（22）up。 **该命令十分强大，它将尝试自动完成包括构建镜像，（重新）创建服务，启动服务，并关联服务相关容

器的一系列操作。

格式为：

```
docker-compose up [options] [SERVICE...] 
```

链接的服务都将会被自动启动，除非已经处于运行状态。

可以说，大部分时候都可以直接通过该命令来启动一个项目。

默认情况， docker-compose up 启动的容器都在前台，控制台将会同时打印所有容器的输出信息，可以很方便进行调试。

当通过 Ctrl-C 停止命令时，所有容器将会停止。

如果使用 docker-compose up -d ，将会在后台启动并运行所有的容器。一般推荐生产环境下使用该选项。

默认情况，如果服务容器已经存在， docker-compose up 将会尝试停止容器，然后重新创建（保持使用 volumes-from 挂载的卷），以保证新启动的服务匹配 docker-compose.yml 文件的最新内容。如果用户不希望容器被停止并重新创建，可以使用 docker-compose up --no-recreate 。这样将只会启动处于停止状态的容器，而忽略已经运行的服务。如果用户只想重新部署某个服务，可以使用 docker-compose up --no-deps -d < SERVICE_NAME > 来重新创建服务并后台停止旧服务，启动新服务，并不会影响到其所依赖的服务。

选项：

+   -d 在后台运行服务容器。

+   –no-color 不使用颜色来区分不同的服务的控制台输出。

+   –no-deps 不启动服务所链接的容器。

+   –force-recreate 强制重新创建容器，不能与 --no-recreate 同时使用。

+   –no-recreate 如果容器已经存在了，则不重新创建，不能与 --force-recreate 同时使用。

+   –no-build 不自动构建缺失的服务镜像。

+   -t, --timeout TIMEOUT 停止容器时候的超时（默认为 10 秒）。

#

# 六、compose使用案例

## 6.1、准备

获取代码：

```
git clone https://gitee.com/long-xu/helloworld.git
```

## 6.2、Dockerfile 文件

在 helloworld 目录中，创建一个名为的文件 Dockerfile，内容如下：

```
FROM golang:1.18
ADD ./ /go/src/helloworld/
WORKDIR /go/src/helloworld
RUN go env -w GOPROXY=$http_proxy
# RUN go env -w GOPROXY=https://proxy.golang.com.cn,https://goproxy.cn,direct
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o app .
FROM ubuntu:latest
COPY ./curl-amd64 /usr/bin/curl
RUN chmod +x /usr/bin/curl
ENV env1=env1value
ENV env2=env2value
MAINTAINER nick
WORKDIR /app/
COPY --from=0 /go/src/helloworld/app ./
EXPOSE 80
ENTRYPOINT [&quot;./app&quot;]
CMD [&quot;--param1=p1&quot;,&quot;--param2=p2&quot;]
```

构建镜像：

```
docker build -t hello:1.0.0 -f Dockerfile --build-args http_proxy=https://proxy.golang.com.cn,https://goproxy.cn,direct .
```

## 6.3、docker-compose.yml

注意：以两个空格分层级。

```
version: &#x27;3.7&#x27;
# 定义项目中的服务
services:
  # 名为web的服务
  web:
    # 根据配置构建镜像
    build:
      # 构建镜像的上下文
      context: .
      #构建镜像的dockerfile
      dockerfile: Dockerfile.compose
      # 构建参数
      args:
        http_proxy: https://proxy.golang.com.cn,https://goproxy.cn,direct
      # 设置labels元数据
      labels:
        myhello: 1.0
        com.example.description: &quot;Accounting webapp&quot;
      # 为构建的镜像指定tags
      tags:
      - &quot;myhello:1.0.0&quot;
      - &quot;localhost:5000/myhello:1.0.0&quot;
    # 指定容器名称
    container_name: &quot;myweb&quot;
    # 设置容器依赖关系,表示web服务依赖于redis服务
    depends_on:
    - redis
    # 运行容器的端口映射
    ports:
    - &quot;80:80&quot;
    # 覆盖容器的cmd指令
    command: [&quot;--param1=1&quot;,&quot;--param2=2&quot;]
    # 设置环境变量
    environment:
      env1: &quot;a&quot;
      env2: &quot;b&quot;
    # 健康检查
    healthcheck:
      # 检查命令
      #test: [&quot;CMD&quot;,&quot;curl&quot;,&quot;http://localhost/health&quot;]
      test: [&quot;CMD-SHELL&quot;,&quot;statuscode=`curl -o /dev/null -s -w %{http_code} http://localhost/health`; [ $$statuscode -le 400 ] || exit 1&quot;]
      # 检查时间间隔
      interval: 5s
      # 检查超时时间
      timeout: 1s
      # 重试次数，即连续失败指定次数则判定为不健康
      retries: 5
      # 设置容器初始化时间，再次期间不报告健康检查状态
      start_period: 5s
    # 使用定义的网络
    networks:
    - mynetwork
    - net1
  # 名为 redis的服务
  redis:
    # 服务启动镜像
    image: &quot;redis:latest&quot;
    container_name: &quot;myredis&quot;
    # 容器暴露端口
    expose:
    - 6379
    # 指定容器启动命令，即覆盖cmd指令
    command: redis-server --requirepass 123456
    # 数据卷设置
    volumes:
    - /data
    networks:
    - mynetwork
    - net2
# 定义网络
networks:
  net1: {}
  net2: {}
  mynetwork:
    ipam:
      driver: default
      config:
      - subnet: &quot;172.16.238.0/24&quot;

```

检查配置文件的有效性：

```
docker-compose -f docker-compose.yml config
```

## 6.4、使用 Compose 命令构建和运行应用

先将刚才写的Dockerfile重命令为Dockerfile.compose：

```
cp Dockerfile Dockerfile.compose
```

在helloworld目录中，执行以下命令来启动应用程序：

```
docker-compose up
```

如果你想在后台执行该服务可以加上 -d 参数：docker-compose up -d

运行后的效果：

```
[+] Building 20.6s (15/16)                                                                                                                                                                  
 =&gt; [internal] load build definition from Dockerfile.compose                                                                                                                           0.0s
 =&gt; =&gt; transferring dockerfile: 527B                                                                                                                                                   0.0s
 =&gt; [internal] load .dockerignore                                                                                                                                                      0.0s
 =&gt; =&gt; transferring context: 2B                                                                                                                                                        0.0s
 =&gt; [internal] load metadata for docker.io/library/ubuntu:latest                                                                                                                       0.0s
 =&gt; [internal] load metadata for docker.io/library/golang:1.18                                                                                                                         0.0s
 =&gt; [stage-0 1/5] FROM docker.io/library/golang:1.18                                                                                                                                  20.5s
 =&gt; [stage-1 1/5] FROM docker.io/library/ubuntu:latest                                                                                                                                 0.0s
 =&gt; [internal] load build context                                                                                                                                                      0.1s
 =&gt; =&gt; transferring context: 5.43MB                                                                                                                                                    0.1s
 =&gt; [stage-0 2/5] ADD ./helloworld /go/src/helloworld/                                                                                                                                 0.1s
 =&gt; [stage-1 2/5] COPY ./helloworld/curl-amd64 /usr/bin/curl                                                                                                                           0.1s
 =&gt; [stage-0 3/5] WORKDIR /go/src/helloworld                                                                                                                                           0.1s
 =&gt; [stage-1 3/5] RUN chmod +x /usr/bin/curl                                                                                                                                           1.1s
 =&gt; [stage-0 4/5] RUN go env -w GOPROXY=https://proxy.golang.com.cn,https://goproxy.cn,direct                                                                                          1.1s
 =&gt; [stage-1 4/5] WORKDIR /app/                                                                                                                                                        0.0s
 =&gt; [stage-0 5/5] RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o app ./                                                                                                        19.0s
 =&gt; [stage-1 5/5] COPY --from=0 /go/src/helloworld/app ./                                                                                                                              0.1s
 =&gt; exporting to image                                                                                                                                                                 0.1s 
 =&gt; =&gt; exporting layers                                                                                                                                                                0.1s 
 =&gt; =&gt; writing image sha256:807800f9f2eef849451f4107c1b422550f2ec0afea34831cee05389aa5670b96                                                                                           0.0s 
 =&gt; =&gt; naming to docker.io/library/compose-web                                                                                                                                         0.0s 
 =&gt; =&gt; naming to docker.io/library/myhello:1.0.0                                                                                                                                       0.0s 
 =&gt; =&gt; naming to localhost:5000/myhello:1.0.0                                                                                                                                          0.0s
[+] Running 5/5
 ⠿ Network compose_mynetwork  Created                                                                                                                                                  0.1s
 ⠿ Network compose_net1       Created                                                                                                                                                  0.1s
 ⠿ Network compose_net2       Created                                                                                                                                                  0.1s
 ⠿ Container myredis          Started                                                                                                                                                  1.1s
 ⠿ Container myweb            Started
```

启动之后可以使用docker命令操作我们的容器，也可以用docker-compose操作（前提是在当前项目文件夹下，如果离开文件夹，docker-compose就会找不到项目；当然，也可以用-f和-p指定项目，但是略显麻烦）。

以上创建了一个网络，一个Redis 和 一个web；postman调用接口访问redis。

进入redis容器，查看redis数据：

```
fly@fly:~/wokspace/compose$ docker-compose exec -t redis bash
root@e964e083eb70:/data# redis-cli 
127.0.0.1:6379&gt; auth 123456
OK
127.0.0.1:6379&gt; set fly 100
OK
127.0.0.1:6379&gt; get fly
&quot;100&quot;
127.0.0.1:6379&gt; 

```

查看docker 网络信息：

```
docker network ls
docker network inspect compose_mynetwork
```

示例：

```
fly@fly:~/wokspace/compose$ docker network ls
NETWORK ID     NAME                DRIVER    SCOPE
b0d6cdc5d9c2   bridge              bridge    local
4919d219e8db   compose_mynetwork   bridge    local
d16736c9b2b2   compose_net1        bridge    local
68c35403e62f   compose_net2        bridge    local
75f3915547ea   host                host      local
a1b056f00f28   none                null      local
fly@fly:~/wokspace/compose$ docker network inspect compose_mynetwork
[
    {
        &quot;Name&quot;: &quot;compose_mynetwork&quot;,
        &quot;Id&quot;: &quot;4919d219e8dbb590184068aee81a300ef743c2ce394814a731b6039afb375fae&quot;,
        &quot;Created&quot;: &quot;2023-03-13T08:16:41.008573796Z&quot;,
        &quot;Scope&quot;: &quot;local&quot;,
        &quot;Driver&quot;: &quot;bridge&quot;,
        &quot;EnableIPv6&quot;: false,
        &quot;IPAM&quot;: {
            &quot;Driver&quot;: &quot;default&quot;,
            &quot;Options&quot;: null,
            &quot;Config&quot;: [
                {
                    &quot;Subnet&quot;: &quot;172.16.238.0/24&quot;
                }
            ]
        },
        &quot;Internal&quot;: false,
        &quot;Attachable&quot;: false,
        &quot;Ingress&quot;: false,
        &quot;ConfigFrom&quot;: {
            &quot;Network&quot;: &quot;&quot;
        },
        &quot;ConfigOnly&quot;: false,
        &quot;Containers&quot;: {
            &quot;e964e083eb705147ae4bafe90b737efabb873d6ee13f574279650f67118f4f12&quot;: {
                &quot;Name&quot;: &quot;myredis&quot;,
                &quot;EndpointID&quot;: &quot;2fc0d07deff5002a28491cd9daa5308dbb8253914f3d8d104bcc196ad43f4a3b&quot;,
                &quot;MacAddress&quot;: &quot;02:42:ac:10:ee:02&quot;,
                &quot;IPv4Address&quot;: &quot;172.16.238.2/24&quot;,
                &quot;IPv6Address&quot;: &quot;&quot;
            },
            &quot;fdacf2b5ce8eb9c5c8a90ec52333b5e68674cb30f5569feede7a4ed58238e5af&quot;: {
                &quot;Name&quot;: &quot;myweb&quot;,
                &quot;EndpointID&quot;: &quot;0118db8855a2b668f8c2cfc13042c28e339cde3e2d2ed18f63447d4752c608bf&quot;,
                &quot;MacAddress&quot;: &quot;02:42:ac:10:ee:03&quot;,
                &quot;IPv4Address&quot;: &quot;172.16.238.3/24&quot;,
                &quot;IPv6Address&quot;: &quot;&quot;
            }
        },
        &quot;Options&quot;: {},
        &quot;Labels&quot;: {
            &quot;com.docker.compose.network&quot;: &quot;mynetwork&quot;,
            &quot;com.docker.compose.project&quot;: &quot;compose&quot;,
            &quot;com.docker.compose.version&quot;: &quot;2.12.2&quot;
        }
    }
]

```

# 总结

1.  重点1，学会compose.yml文件如何配置。

1.  重点2，compose命令的使用。

1.  compose针对的目标对象是项目service。