# 云原生时代，必备的Docker容器管理命令大揭秘！

> 来源：[https://geekbang.feishu.cn/docx/GEQAdW0OboaNbZxTy0icepHZn5d](https://geekbang.feishu.cn/docx/GEQAdW0OboaNbZxTy0icepHZn5d)

> 👨🏻 作者：Lion莱恩呀

> 📃 作者介绍：专注分享高性能服务器后台开发知识，包括C/C++、Linux、网络协议、设计模式、中间件、云原生、数据库、分布式架构等领域知识。

> 🛖 👸号：Lion莱恩呀

> ⏰ 收录时间：2024-05-27 11:15:56 (UTC+8)

> 🔗 原文链接：h[ttps://mp.weixin.q](https://mp.weixin.qq.com/s?__biz=Mzk0ODUzNDc2Mw==&mid=2247485267&idx=1&sn=557ece859b833fc603e4c977aa67f875&scene=19#wechat_redirect)q.com/s?__biz=Mz...

![](img/da4c6d292c848024cf0fc0d88433a62b.png)

关注作者，获取更多相关知识

> 摘要：在云原生时代，Docker已成为必不可少的容器管理工具。通过掌握Docker常用的容器管理命令，可以提高应用部署和运维的效率。本文将揭秘云原生时代必备的Docker容器管理命令，包括容器的创建、启动、停止、删除以及日常运维中常用的命令和技巧。通过学习本文，读者将能够轻松应对云原生时代的容器管理挑战，提高工作效率，实现更高质量的应用部署和运维管理。

# 一、docker create  一、docker创建

创建一个新的容器但不启动它。语法同docker run。

```
docker create  [OPTIONS] CONTAINER
```

示例：

在一个终端执行docker create，在另一个终端执行docker events。

```
# 第一个终端
fly@fly:~$ docker create nginx
ae61ef82b9477f32607a6f4a438d1f02711ad0c77309808f075fa0a8c7e7a35a

# 第二个终端
fly@fly:~$ docker events 
2022-12-08T14:58:30.633969517Z container create ae61ef82b9477f32607a6f4a438d1f02711ad0c77309808f075fa0a8c7e7a35a (image=nginx, maintainer=NGINX Docker Maintainers &lt;docker-maint@nginx.com&gt;, name=pedantic_poincare)

```

# 二、docker run

创建一个新的容器并运行一个命令。

```
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
```

帮助：

```
fly@fly:~$ docker run --help
```

## 2.1、常用选项

+   –add-host：容器中hosts文件添加 host:ip 映射记录。

```
docker run --rm -it --add-host db-static:86.75.30.9 ubuntu cat /etc/hosts
```

+   -a, --attach：附加到 STDIN, STDOUT 或 STDERR。

```
docker run --rm -a stdin -a stdout -i -t ubuntu /bin/bash
```

+   –cidfile：将容器id写入到指定文件。

```
docker run --rm --cidfile /tmp/idfile ubuntu
```

+   -d, --detach：后台运行容器并打印容器id。

+   –detach-keys：指定将容器切回后台的按键，默认：CTRL-p+CTRL-q。

+   –disable-content-trust：跳过镜像验证，默认为TRUE。

+   –domainname：设置容器网络服务域名。

+   –entrypoint：覆盖镜像默认程序入口。

```
docker run --rm -it --entrypoint /bin/bash nginx
```

+   -e, --env：设置环境变量。

+   –env-file：从文件中读取环境变量。

+   –expose：暴露一个或多个端口。

+   –group-add：为容器用户添加更多用户组。

```
docker run --rm --group-add audio --group-add nogroup --group-add 777 ubuntu id
```

+   -h, --hostname string：设置主机名。

+   –init 在容器内运行init，转发信号并捕获进程。

```
# ps 对比容器的元祖进程 
docker run --rm -it ubuntu bash 
docker run --rm -it --init ubuntu bash
```

+   -i, --interactive：保持标准输入设备处于打开状态。

+   -l, --label list：设置容器元数据labels。

+   –label-file list：从文件中读取以换行作为分隔符的label。

+   –link：添加一个其他容器的链接，及修改当容器的/etc/hosts文件。

+   –log-driver：容器日志驱动，none、local、json-file、syslog、journald、gelf、fluentd、awslogs、splunk、etwlogs、gcplogs、logentries。

+   –log-opt：日志驱动选项。

+   –mount：将文件系统挂载到容器，与 -v 类似。

```
# 挂载volume 
docker run -t -i --rm --mount type=volume,target=/data ubuntu bash 
# 创建目录 
mkdir /tmp/data 
# 修改目录所有权，仅当使用userns的情况下需要 
sudo chown 165536:root /tmp/data 
# bind 磁盘已存在目录 
docker run -t -i --rm --mount type=bind,src=/tmp/data,dst=/data ubuntu bash
```

+   –name：指定容器名称。

+   –privileged：授予容器超级权限。

+   -p, --publish：发布容器端口的主机端口。

+   -P, --publish-all：将所有暴露的端口发布到主机随机端口。

+   –pull：运行前拉取图像，（always|missing|never）,默认 missing。

+   –pids-limit：限制容器中pid个数。

+   –read-only ：将容器跟文件系统装载为只读。

+   –restart：当容器退出时的重启策略，默认为no。

```
no：当容器退出时，不要自动重新启动
on-failure[:max-retries]：仅当容器以非零退出状态退出时才重新启动。
always：无论退出状态如何，始终重新启动容器。无论容器的当前状态如何，容器也将始终在守护程序启动时启动
unless-stopped：论退出状态如何，始终重新启动容器，包括守护程序启动时，除非容器在Docker守护程序停止之前处于停止状态。
```

+   –rm：当容器退出时，自动删除容器。

+   –sig-proxy：将接收到的信号代理到进程（默认为true）。

+   –stop-signal：停止容器的信号（默认为“SIGTERM”）。

+   –stop-timeout：容器停止超时时长，单位s。

+   –tmpfs：装载tmpfs目录。

```
docker run -dit --rm --tmpfs /run1:rw,noexec,nosuid,size=65536k ubuntu bash
```

+   -t, --tty：分配一个伪终端设备。

+   -u, --user：用户名或用户ID (格式: <name|uid>[:<group|gid>])。

+   -v, --volume ：绑定数据卷。

+   –volume-driver：容器的可选卷驱动程序。

+   –volumes-from：从指定的容器装载数据卷。

+   -w, --workdir：容器内的工作目录。

## 2.2、系统

+   –cap-add：添加linux功能

+   –cap-drop：删除linux功能

```
docker run --cap-add=ALL --cap-drop=MKNOD ...
docker run --cap-add=SYS_ADMIN ...
docker run --cap-add=CAP_SYS_ADMIN ...
```

```
#示例：
fly@fly:~$ docker run --cap-add=ALL --cap-drop=MKNOD nginx
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2022/12/09 14:44:55 [notice] 1#1: using the &quot;epoll&quot; event method
2022/12/09 14:44:55 [notice] 1#1: nginx/1.23.2
2022/12/09 14:44:55 [notice] 1#1: built by gcc 10.2.1 20210110 (Debian 10.2.1-6) 
2022/12/09 14:44:55 [notice] 1#1: OS: Linux 5.4.0-135-generic
2022/12/09 14:44:55 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2022/12/09 14:44:55 [notice] 1#1: start worker processes
2022/12/09 14:44:55 [notice] 1#1: start worker process 29
2022/12/09 14:44:55 [notice] 1#1: start worker process 30
^C2022/12/09 14:45:05 [notice] 1#1: signal 2 (SIGINT) received, exiting
2022/12/09 14:45:05 [notice] 30#30: exiting
2022/12/09 14:45:05 [notice] 29#29: exiting
2022/12/09 14:45:05 [notice] 30#30: exit
2022/12/09 14:45:05 [notice] 29#29: exit
2022/12/09 14:45:05 [notice] 1#1: signal 17 (SIGCHLD) received from 30
2022/12/09 14:45:05 [notice] 1#1: worker process 29 exited with code 0
2022/12/09 14:45:05 [notice] 1#1: worker process 30 exited with code 0
2022/12/09 14:45:05 [notice] 1#1: exit
```

+   –isolation：指定容器的隔离技术。

> linux系统：只支持default，即linux命名空间隔离技术。

> windows系统：default:表示使用docker守护进程配置的选项，否则默认使用process选项。

> process:命名空间隔离

> hyperv:基于Hyper-V 管理程序基于分区的隔离

+   –platform：设置平台，如果服务器支持多个平台。

+   –runtime：用于此容器使用的运行时。

+   –security-opt：通过指定–security opt标志来覆盖每个容器的默认标签方案。

+   –shm-size：设置/dev/shm设备的大小，/dev/shm是一个基于内存的临时文件系统。

+   –storage-opt：容器的存储驱动程序选项

+   –sysctl：设置系统参数，默认 map[]。

+   –ulimit：设置Ulimit选项，默认[]。

## 2.3、网络

1.  –ip：ip4地址。

1.  –ip6：ip6地址。

1.  –dns 设置自定义dns服务器，将修改容器中 /etc/resolv.conf 文件。

1.  –dns-option：设置DNS选项，将修改容器中 /etc/resolv.conf 文件。

1.  –dns-search：设置自定义DNS搜索域名，将修改容器中 /etc/resolv.conf 文件。

1.  –link-local-ip：设置一个或多个容器的以太网设备的链路本地IPv4/IPv6地址。

1.  –mac-address：容器Mac地址。

1.  –network：将容器加入到指定网络。

1.  –network-alias：为容器添加网络范围的别名。

## 2.4、健康检查

1.  –health-cmd：健康检查命令。

1.  –health-interval：健康检查频率（ms|s|m|h）,默认0s。

1.  –health-retries：健康检查重试次数，连续失败指定次数则判断为不健康。

1.  –health-start-period：设置启动容器的初始化时间，在此期间的健康检查不作为参考依据，（ms|s|m|h）,默认0s。

1.  –health-timeout：每一次检查超时时间（ms|s|m|h）,默认0s。

1.  –no-healthcheck：禁用健康检查。

示例：

```
docker run --rm --health-cmd &quot;curl http://localhost&quot; --health-interval 2s --health-retries 5 --health-start-period 10s --health-timeout 1s nginx
```

```
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2022/12/09 14:51:51 [notice] 1#1: using the &quot;epoll&quot; event method
2022/12/09 14:51:51 [notice] 1#1: nginx/1.23.2
2022/12/09 14:51:51 [notice] 1#1: built by gcc 10.2.1 20210110 (Debian 10.2.1-6) 
2022/12/09 14:51:51 [notice] 1#1: OS: Linux 5.4.0-135-generic
2022/12/09 14:51:51 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2022/12/09 14:51:51 [notice] 1#1: start worker processes
2022/12/09 14:51:51 [notice] 1#1: start worker process 29
2022/12/09 14:51:51 [notice] 1#1: start worker process 30
127.0.0.1 - - [09/Dec/2022:14:51:53 +0000] &quot;GET / HTTP/1.1&quot; 200 615 &quot;-&quot; &quot;curl/7.74.0&quot; &quot;-&quot;
127.0.0.1 - - [09/Dec/2022:14:51:55 +0000] &quot;GET / HTTP/1.1&quot; 200 615 &quot;-&quot; &quot;curl/7.74.0&quot; &quot;-&quot;
127.0.0.1 - - [09/Dec/2022:14:51:57 +0000] &quot;GET / HTTP/1.1&quot; 200 615 &quot;-&quot; &quot;curl/7.74.0&quot; &quot;-&quot;

```

# 三、docker start/stop/restart  三、docker启动/停止/重启

+   docker start :启动一个或多个已经被停止的容器。

```
docker start [OPTIONS] CONTAINER [CONTAINER...]
```

+   docker stop :停止一个运行中的容器。

```
docker stop [OPTIONS] CONTAINER [CONTAINER...]
```

+   docker restart :重启容器。

```
docker restart [OPTIONS] CONTAINER [CONTAINER...]
```

示例：

```
docker restart mynginx
docker stop mynginx
docker start mynginx
```

# 四、docker kill

杀掉一个运行中的容器。

```
docker kill [OPTIONS] CONTAINER [CONTAINER...]
```

OPTIONS说明：

-s :向容器发送一个信号。

| 命令 | 默认信号量 | 默认信号量的值 |
| --- | --- | --- |
| kill | TERM | 15 |
| docker kill  docker 杀戮 | KILL | 9 |
| docker stop  docker 停止 | TERM | 15 |

示例：

```
docker kill -s KILL mynginx
docker kill -s TERM mynginx
```

如果容器终止时的状态对用户而言很重要，用户可能会想要了解docker kill 和docker stop之间的区别。docker kill 的行为和标准的kill 命令行程序并不相同。kill 程序的默认工作方式是向指定的进程发送

TERM信号（即信号值为15）。这个信号表示程序应该被终止，但是不要强迫程序终止。当这个信号被处理时，大多数程序将执行某种清理工作，但是该程序也可以执行其他操作，包括忽略该信号。

而docker kill 对正在运行的程序使用的是KILL信号，这使得该进程没办法处理终止过程。这就意味着一些诸如包含运行进程ID之类的文件可能会残留在文件系统中。根据应该用程序管理状态的能力，如果再次启动容器，这可能会也可能不会造成问题。docker stop 命令则像kill 命令那样工作，发送的是一个TERM型号。

# 五、docker rm

删除一个或多个容器。

```
docker rm [OPTIONS] CONTAINER [CONTAINER...]
```

OPTIONS说明：

-f :通过 SIGKILL 信号强制删除一个运行中的容器。

-l :移除容器间的网络连接，而非容器本身。

-v :删除与容器关联的卷。

示例：

删除所有的容器。

```
docker rm `docker ps -f status=exited -q`
```

```
fly@fly:~$ docker rm `docker ps -f status=exited -q`
002e50006cc4
14ccc9a0dc5a
36f6736b9c62
cf6976e1333f
1f2d1a8d657f
361a3588db08
5b94ae9400df
fly@fly:~$ 
fly@fly:~$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

```

# 六、docker pause/unpause  六、docker 暂停/取消暂停

1.  docker pause :暂停容器中所有的进程。

1.  docker unpause :恢复容器中所有的进程。

```
docker pause CONTAINER [CONTAINER...]
docker unpause CONTAINER [CONTAINER...]
```

#

# 总结

（1）docker的create命令和run命令区别在于create只创建容器，而run命令除了创建容器还会连接网络和启动容器；start命令会连接网络和启动容器。即docker run = docker create + docker start。

（2）docker stop发送的信号值默认是15，但是nginx构建的时候会指定为3；docker kill发送的信号值是9，强制删除。

（3）docker run使用最多的参数：

+   -d, --detach：后台运行容器并打印容器id。

+   -i, --interactive：保持标准输入设备处于打开状态。

+   -t, --tty：分配一个伪终端设备。

+   –rm：当容器退出时，自动删除容器。

+   -p, --publish：发布容器端口和主机端口的映射。

+   -P, --publish-all：将所有暴露的端口发布到主机随机端口。

+   –entrypoint：覆盖镜像默认程序入口。

+   –name：指定容器名称。

+   –health-cmd：健康检查命令。