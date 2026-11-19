
# 掌握这些Docker镜像管理命令，让容器部署更得心应手

> 来源：[https://geekbang.feishu.cn/docx/Mmg9dxRXZoYhNUxU6iucFW9qnQb](https://geekbang.feishu.cn/docx/Mmg9dxRXZoYhNUxU6iucFW9qnQb)

> 👨🏻 作者：Lion莱恩呀

> 📃 作者介绍：专注分享高性能服务器后台开发知识，包括C/C++、Linux、网络协议、设计模式、中间件、云原生、数据库、分布式架构等领域知识。

> 🛖 👸号：Lion莱恩呀

> ⏰ 收录时间：2024-05-27 11:15:56 (UTC+8)

> 🔗 原文链接：h[ttps://mp.weixin.q](https://mp.weixin.qq.com/s?__biz=Mzk0ODUzNDc2Mw==&mid=2247485294&idx=1&sn=171c9261afe9d16b0983099402028ba7&scene=19#wechat_redirect)q.com/s?__biz=Mz...

![](img/3974d1bc5632d9dba09de935034118d2.png)

关注作者，获取更多相关知识

## docker images

列出本地镜像。

```
docker images [OPTIONS] [REPOSITORY[:TAG]]
```

OPTIONS说明：

- `-a`：列出本地所有的镜像（含中间映像层，默认情况下，过滤掉中间映像层）；
- `--digests`：显示镜像的摘要信息；
- `-f`：显示满足条件的镜像；
- `--format`：指定返回值的模板文件；
- `--no-trunc`：显示完整的镜像信息；
- `-q`：只显示镜像ID

示例：

```
# 查看所有悬空镜像
docker images --filter dangling=true
# 清除所有悬空镜像
docker image prune
```

## docker build

`docker build` 命令用于使用 `Dockerfile` 创建镜像。

```
docker build [OPTIONS] PATH | URL | -
```

### OPTIONS 说明

- `--add-host`：向 `hosts` 文件中添加自定义 `host:ip` 映射
- `--build-arg=[]`：设置镜像创建时的变量；
- `--cache-from`：指定镜像用作当前构建的缓存镜像
- `--cgroup-parent`：容器的可选父 cgroup
- `--compress`：使用 gzip 压缩构建上下文
- `--cpu-period`：限制 CPU CFS 周期；
- `--cpu-quota`：限制 CPU CFS 配额；
- `-c,–cpu-shares`：设置 cpu 使用权重；
- `--cpuset-cpus`：指定使用的 CPU id；
- `--cpuset-mems`：指定使用的内存 id；
- `--disable-content-trust`：跳过镜像校验，默认为 true；
- `-f,–file`：指定要使用的 `Dockerfile` 路径；
- `–force-rm`：不论构建是否成功，总是删除中间容器，默认 false。注意：中间容器；
- `–iidfile`：将镜像 ID 写入到文件
- `–isolation`：使用容器隔离技术；

> linux系统：只支持 `default`，即 linux 命名空间隔离技术

> windows系统：

> `default`：表示使用 `docker` 守护进程配置的选项，否则默认使用 `process` 选项

> `process`：命名空间隔离

> `hyperv`：基于 `Hyper-V` 管理程序基于分区的隔离

- `–label=[]`：设置镜像使用的元数据；
- `-m`：设置内存最大值；
- `–memory-swap`：设置 Swap 的最大值为内存 + swap，"-1" 表示不限 swap；
- `–network`：在构建期间为 `RUN` 指令设置网络模式（默认 `"default"`）与 `docker run` 指令无关
- `–no-cache`：创建镜像的过程不使用缓存；
- `–pull`：总是尝试去更新镜像的新版本；
- `–quiet, -q`：安静模式，成功后只输出镜像 ID；
- `–rm`：构建成功后，删除中间容器，默认 true。注意：中间容器，不是镜像；
- `–security-opt`：安全选项
- `–shm-size`：设置 `/dev/shm` 的大小，默认值是 64M；`/dev/shm` 是基于内存的 `tmpfs` 文件系统。
- `–ulimit`：Ulimit 配置。
- `–tag, -t`：镜像的名字及标签，通常 name:tag 或者 name 格式；可以在一次构建中为一个镜像设置多个标签。
- `–target`：设置要生成的目标生成阶段
- `–ulimit`：Ulimit 选项

示例：

```
docker build -t nodetodo:v1.0.0 -f Dockerfile .
```

## docker rmi

删除本地一个或多个镜像。

```
docker rmi [OPTIONS] IMAGE [IMAGE...]
```

OPTIONS说明：

- `-f`：强制删除；
- `--no-prune`：不移除该镜像的过程镜像，默认移除。

## docker tag

标记本地镜像，将其归入某一仓库。

```
docker rmi [OPTIONS] IMAGE [IMAGE...]
```

示例：

```
fly@fly:~$ docker tag nginx:latest myngix:1.22.0
fly@fly:~$ docker images 
REPOSITORY           TAG       IMAGE ID       CREATED       SIZE
myngix               1.22.0    ac8efec875ce   4 days ago    142MB
nginx                latest    ac8efec875ce   4 days ago    142MB
radial/busyboxplus   curl      71fa7369f437   8 years ago   4.23MB

```

tag 可以针对镜像进行版本管理和将镜像推到远程仓库。

## docker save

将指定镜像保存成 tar 归档文件。

```
docker save [OPTIONS] IMAGE [IMAGE...]
```

OPTIONS 说明：

- `-o`：输出到的文件。

```
docker save -o mynginx.tar mynginx:v1
# 解压文件，查看内容
tar vxf mynginx.tar -C mynginx
```

## docker load

导入使用 `docker save` 命令导出的镜像。

```
docker load [OPTIONS]
```

OPTIONS 说明：

- `--input , -i`：指定导入的文件，代替 `STDIN`。
- `--quiet , -q`：精简输出信息。

示例：

```
docker load -i mynginx.tar
```

## docker import

从归档文件中创建镜像。

```
docker import [OPTIONS] file|URL|- [REPOSITORY[:TAG]]
```

OPTIONS说明：

- `-c`：应用 `docker` 指令创建镜像。
- `-m`：提交时的说明文字。

示例：

```
docker import mynginx.tar mynginx:v3
```

## docker commit

从容器创建一个新的镜像。

```
docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]
```

OPTIONS说明：

- `-a`：提交的镜像作者。
- `-c`：使用 `Dockerfile` 指令来创建镜像。
- `-m`：提交时的说明文字。
- `-p`：在 `commit` 时，将容器暂停。

示例：

```
fly@fly:~$ docker commit mynginx myngix:v1.1.1
sha256:742e60dd0ca915729a29acbab55a606ecda5594753a0adfb85a2a1df902e9925
fly@fly:~$ docker images 
REPOSITORY           TAG       IMAGE ID       CREATED          SIZE
myngix               v1.1.1    742e60dd0ca9   14 seconds ago   142MB
myngix               1.22.0    ac8efec875ce   4 days ago       142MB
nginx                latest    ac8efec875ce   4 days ago       142MB
radial/busyboxplus   curl      71fa7369f437   8 years ago      4.23MB

```

## docker login/logout

`docker login`：登陆到一个 Docker 镜像仓库，如果未指定镜像仓库地址，默认为官方仓库 Docker Hub。

`docker logout`：登出一个 Docker 镜像仓库，如果未指定镜像仓库地址，默认为官方仓库 Docker Hub。

```
docker login [OPTIONS] [SERVER]
docker logout [OPTIONS] [SERVER]
```

OPTIONS说明：

- `-u`：登陆的用户名。
- `-p`：登陆的密码。

示例：

```
docker login -u 用户名 -p 密码
docker logout
```

## docker pull

从镜像仓库中拉取或者更新指定镜像。

```
docker pull [OPTIONS] NAME[:TAG|@DIGEST]
```

OPTIONS说明：

- `-a`：拉取所有 tagged 镜像。
- `--disable-content-trust`：忽略镜像的校验,默认开启。

示例：

```
fly@fly:~$ docker pull nginx
Using default tag: latest
latest: Pulling from library/nginx
Digest: sha256:ab589a3c466e347b1c0573be23356676df90cd7ce2dbf6ec332a5f0a8b5e59db
Status: Image is up to date for nginx:latest
docker.io/library/nginx:latest

```

## docker push

将本地的镜像上传到镜像仓库，注意，要先登陆到镜像仓库。

```
docker push [OPTIONS] NAME[:TAG]
```

OPTIONS说明：

- `--disable-content-trust`：忽略镜像的校验,默认开启。

## docker search

从 Docker Hub 查找镜像。

```
docker search [OPTIONS] TERM
```

OPTIONS说明：

- `--automated`：只列出 `automated build` 类型的镜像。
- `--no-trunc`：显示完整的镜像描述。
- `-f <过滤条件>`：列出收藏数不小于指定值的镜像。

```
# 从 Docker Hub 查找所有镜像名包含 nginx，并且收藏数大于 10 的镜像
docker search -f stars=10 nginx
```

参数说明：

- `NAME`：镜像仓库源的名称
- `DESCRIPTION`：镜像的描述
- `OFFICIAL`：是否 docker 官方发布
- `stars`：类似 Github 里面的 star，表示点赞、喜欢的意思。
- `AUTOMATED`：自动构建。

# 总结

掌握一系列重要的 Docker 镜像管理命令，让容器部署变得更加得心应手。无论是在开发、测试还是生产环境中，这些命令都能够更好地管理和优化 Docker 镜像，提升云原生技能，加速容器部署，提高工作效率。
