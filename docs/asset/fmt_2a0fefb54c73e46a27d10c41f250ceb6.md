
# Docker私有化部署Dify

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/JACMd7xCyo4Tepxp1ZncKOQ5neb](https://qqs7y1hozd1.feishu.cn/docx/JACMd7xCyo4Tepxp1ZncKOQ5neb)

## 云服务和私有化部署

+   使用Dify的云服务版，云服务版就是在[Dify官网](https://dify.ai/)编排应用。

+   很多小伙伴身处金融、政府这样对数据高度敏感的行业，直接使用云服务不符合他们的需求。数据安全性是他们最为看重的，于是私有化部署应运而生。Dify当然也提供私有化部署的方式，顾名思义，私有化部署是指在自己的服务器或者桌面电脑上部署Dify。

> Dify提供两种私有化部署的方式：

+   Docker Compose部署

+   源码启动

+   我们推荐Docker Compose部署的方式，Docker Compose部署方式快速、简单，几行命令就能搞定。

+   源码启动步骤相对繁琐，不推荐。

## 什么是Docker？

我先跟你讲一个场景，原先没有Docker的时候，我们想要在服务器上部署一个Java Web应用，除了Jar包本身，还需要在服务器上安装JDK、MySQL、Redis、Nginx等，然后配置好各种配置文件，最后启动Jar包。如果你是部署1台服务器还不算麻烦，那如果部署100台服务器呢？并且如果服务器的Linux版本不一样，或者JDK版本不一样，你的Java Web应用部署在A服务器上可以运行，也许部署在B服务器上就会报错。这些问题是不是很头疼？于是Docker应运而生。有了Docker之后，你只需要创建一个包含Java Web应用程序、JDK、MySQL、Redis、Nginx等的容器镜像，并将其部署到任何支持Docker的环境中，无需担心版本冲突或环境不一致的问题。Docker就是一个包含了所有依赖的容器，我的理解就是他是一个小型系统，不受系统环境限制，可以独立运行。

## 安装Docker

### Mac和Windows

在Mac和Windows上安装Docker非常简单，只需要到[Docker官网](https://www.docker.com/products/docker-desktop/)下载Docker Desktop，双击安装即可。

![](img/d82aa6d097d1f95d57b5e897becd5255.png)

Docker Desktop安装完成打开后的界面如下：

![](img/19c66699eb9717a60f0eec5088b29620.png)

可以在设置打开Docker开机自启动。

我们在Mac终端执行命令`docker --version`查看Docker版本:

![](img/4e620db1b1704f566486763c4fe3908c.png)

### Linux服务器

对于无桌面环境的Linux服务器，RedHat、Ubuntu、CentOS等都是参照[官网安装指南](https://docs.docker.com/engine/install/)进行安装，这里不作过多介绍。

## 下载Dify源码

使用`git clone`命令下载Dify源码：

```
git clone https://github.com/langgenius/dify.git
```

## 启动Dify

### Docker启动Dify

进入 Dify 源代码的 `docker` 目录，执行一键启动命令：

```
cd dify/dockerdocker compose up -d
```

如果您的系统安装了 Docker Compose V2 而不是 V1，请使用 `docker compose` 而不是 `docker-compose`。通过`$ docker compose version`检查版本号：

![](img/6cfaffe236a2b794b0a38b14593f682a.png)

### 部署结果

```
[+] Running 7/7 ✔ Container docker-web-1       Started                                                                                                                                                                                       1.0s  ✔ Container docker-redis-1     Started                                                                                                                                                                                       1.1s  ✔ Container docker-weaviate-1  Started                                                                                                                                                                                       0.9s  ✔ Container docker-db-1        Started                                                                                                                                                                                       0.0s  ✔ Container docker-worker-1    Started                                                                                                                                                                                       0.7s  ✔ Container docker-api-1       Started                                                                                                                                                                                       0.8s  ✔ Container docker-nginx-1     Started
```

### 检查容器是否正常运行

```
docker compose ps
```

包括 3 个业务服务 `api` / `worker` / `web`，以及 4 个基础组件 `weaviate` / `db` / `redis` / `nginx`，都要处于启动状态。注意下面每一个服务的`status`都要是`Up`状态才算真正启动成功。

```
NAME                IMAGE                              COMMAND                  SERVICE             CREATED             STATUS              PORTS
docker-api-1        langgenius/dify-api:0.3.2          "/entrypoint.sh"         api                 4 seconds ago       Up 2 seconds        80/tcp, 5001/tcp
docker-db-1         postgres:15-alpine                 "docker-entrypoint.s…"   db                  4 seconds ago       Up 2 seconds        0.0.0.0:5432->5432/tcp
docker-nginx-1      nginx:latest                       "/docker-entrypoint.…"   nginx               4 seconds ago       Up 2 seconds        0.0.0.0:80->80/tcp
docker-redis-1      redis:6-alpine                     "docker-entrypoint.s…"   redis               4 seconds ago       Up 3 seconds        6379/tcp
docker-weaviate-1   semitechnologies/weaviate:1.18.4   "/bin/weaviate --hos…"   weaviate            4 seconds ago       Up 3 seconds        
docker-web-1        langgenius/dify-web:0.3.2          "/entrypoint.sh"         web                 4 seconds ago       Up 3 seconds        80/tcp, 3000/tcp
docker-worker-1     langgenius/dify-api:0.3.2          "/entrypoint.sh"         worker              4 seconds ago       Up 2 seconds        80/tcp, 5001/tcp
```

## 访问Dify

在浏览器中输入 `http://localhost` 即可访问 Dify。

## 升级Dify

进入 Dify源代码的 `docker` 目录，按顺序执行以下命令：

```
cd dify/docker
git pull origin main
docker compose down
docker compose pull
docker compose up -d
```

## 常见问题

### Dify启动失败

我是在Ubuntu系统中，Docker版本相对较低的情况下遇到如下问题： 在浏览器中输入 `http://localhost` 访问 Dify，首页正常显示，当点击设置管理员用户页面一直在加载中，如下图所示：

打开浏览器控制台，发现报错：

```
/console/api/setup 502
```

看起来是Dify Api服务报错了，使用如下命令查看容器是否启动成功：

```
docker compose ps
```

果然发现有几个服务的状态`status`栏并不是`Up`状态，而是处于`Restarting`状态。

通过`docker logs docker-api-1`命令查看Dify Api服务的日志，发现报错信息如下：

![](img/76074c8942ad31bc1338dca45d467ff6.png)

这是由于Docker运行权限不够，需要被赋予更高的权限。

解决方法：

在`docker-compose.yaml`文件中，给`services`下每一个`service`最后一行都加上`privileged: true`

![](img/f68c6f72b91d954925641dd5c91cd8e2.png)

然后重新启动Dify即可。

### 在Ubuntu系统下，升级Dify

由于在第一个问题中，Ubuntu系统中，本地修改了`docker-compose.yaml`文件，在使用`git pull origin main`会提示本地有文件没有提交：

```
error: Your local changes to the following files would be overwritten by merge:        docker/docker-compose.yamlPlease, commit your changes or stash them before you can merge.
```

解决方法：

```
git add docker-compose.yamlgit commit -m "local change"
```

执行`git commit`时，需要配置 Git 的用户信息。Git 需要知道提交是由谁进行的。

```
这里用户名和邮箱随便写即可
git config --global user.email "xxx"
git config --global user.name "xxx"
```

提交本地修改之后，再执行`git pull origin main`，由于本地修改了`docker-compose.yaml`文件，此时会提示文件有冲突，需要手动解决冲突。

```
vi docker-compose.yaml
```

手动解决冲突后，保存`docker-compose.yaml`文件，然后再本地commit修改。

```
git add docker-compose.yaml
git commit -m "解决docker-compose.yaml的冲突"
```

冲突解决后再按照如下步骤进行Dify升级

```
cd dify/docker
git pull origin main
docker compose down
docker compose pull
docker compose up -d
```

### 升级到指定Tag版本

```
cd dify/docker

docker compose down

//查看当前分支
git branch

git fetch --all

//查看所有tag版本
git tag

//拉取指定分支
git checkout 0.6.11

docker compose pull

docker compose up -d
```

### Docker无法拉取镜像

修改Docker镜像源，寻找新的可以的Docker镜像源。

```
// daemon.json可能会不存在，直接 vi /etc/docker/daemon.json可以创建
sudo vi /etc/docker/daemon.json

{
  "registry-mirrors": ["新的镜像源地址"]
}

systemctl restart docker //重启docker

docker info //查看镜像源有没有配置成功
```
