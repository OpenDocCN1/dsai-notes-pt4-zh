# 跨入云原生世界：掌握Docker基础命令，实现无缝容器化部署

> 来源：[https://geekbang.feishu.cn/docx/FtJedMlvHoDlLFxkfErcyuwknPg](https://geekbang.feishu.cn/docx/FtJedMlvHoDlLFxkfErcyuwknPg)

> 👨🏻 作者：Lion莱恩呀

> 📃 作者介绍：专注分享高性能服务器后台开发知识，包括C/C++、Linux、网络协议、设计模式、中间件、云原生、数据库、分布式架构等领域知识。

> 🛖 公众号：Lion莱恩呀

> ⏰ 收录时间：2024-05-27 11:15:56 (UTC+8)

> 🔗 原文链接：h[ttps://mp.weixin.q](https://mp.weixin.qq.com/s?__biz=Mzk0ODUzNDc2Mw==&mid=2247485230&idx=1&sn=058025df5a21f27eab43fabff0d9cd68&scene=19#wechat_redirect)q.com/s?__biz=Mz...

![](img/3d51e8495b9c29ab821d1392d6493467.png)

关注作者，获取更多相关知识

# 一、Docker 环境信息命令

## 1.1、docker info

显示 Docker 系统信息，包括镜像、容器数、仓库镜像、架构等。

```
docker info
```

```
docker info [OPTIONS]
```

示例：

```
fly@fly:~$ docker info 
Client:
 Context:    default
 Debug Mode: false

Server:
 Containers: 4
  Running: 4
  Paused: 0
  Stopped: 0
 Images: 2
 Server Version: 20.10.12
 Storage Driver: overlay2
  Backing Filesystem: extfs
  Supports d_type: true
  Native Overlay Diff: true
  userxattr: false
 Logging Driver: json-file
 Cgroup Driver: cgroupfs
 Cgroup Version: 1
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local logentries splunk syslog
 Swarm: inactive
 Runtimes: io.containerd.runtime.v1.linux runc io.containerd.runc.v2
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: 
 runc version: 
 init version: 
 Security Options:
  apparmor
  seccomp
   Profile: default
  userns
 Kernel Version: 5.4.0-135-generic
 Operating System: Ubuntu 20.04.5 LTS
 OSType: linux
 Architecture: x86_64
 CPUs: 2
 Total Memory: 1.907GiB
 Name: fly
 ID: PALW:CS67:UTR6:Q3TR:QUH7:U2LI:KGEJ:U4KL:OG3L:R2WT:2I5X:R33I
 Docker Root Dir: /var/lib/docker/165536.165536
 Debug Mode: false
 Registry: https://index.docker.io/v1/
 Labels:
 Experimental: false
 Insecure Registries:
  127.0.0.0/8
 Registry Mirrors:
...
 Live Restore Enabled: false

WARNING: No swap limit support

```

## 1.2、docker version

显示 Docker 版本信息。

```
docker version 
```

```
docker version [OPTIONS]
# OPTIONS说明：
# -f :指定返回值的模板文件。
```

```
fly@fly:~$ docker version
Client:
 Version:           20.10.12
 API version:       1.41
 Go version:        go1.16.2
 Git commit:        20.10.12-0ubuntu2~20.04.1
 Built:             Wed Apr  6 02:14:38 2022
 OS/Arch:           linux/amd64
 Context:           default
 Experimental:      true

Server:
 Engine:
  Version:          20.10.12
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.16.2
  Git commit:       20.10.12-0ubuntu2~20.04.1
  Built:            Thu Feb 10 15:03:35 2022
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.5.9-0ubuntu1~20.04.5
  GitCommit:        
 runc:
  Version:          1.1.0-0ubuntu1~20.04.2
  GitCommit:        
 docker-init:
  Version:          0.19.0
  GitCommit:        

```

# 二、系统日志信息常用命令

## 2.1、docker events

从服务器获取实时事件（监听docker事件）。

```
docker events
```

```
docker events [OPTIONS]
OPTIONS说明：
-f ：根据条件过滤事件；
--since ：从指定的时间戳后显示所有事件;
--until ：流水时间显示到指定的时间为止；
```

示例：

```
# 第一个终端执行
docker events
# 第二个终端操作容器，比如docker run -d nginx
docker start/stop/restart
# 查看第一个终端输出

```

```
# 启动容器事件
2022-12-08T14:18:01.476501682Z container create 36f6736b9c6289da6db95898507796a378cd03e29b51166039c5a5a593377fb0 (image=nginx, maintainer=NGINX Docker Maintainers &lt;docker-maint@nginx.com&gt;, name=stupefied_newton)
2022-12-08T14:18:01.578704583Z network connect fdb00a147400b60e2c0706129ed2b143a13c02c5675b5fc2871c5d4ed42aa478 (container=36f6736b9c6289da6db95898507796a378cd03e29b51166039c5a5a593377fb0, name=bridge, type=bridge)
2022-12-08T14:18:03.310080179Z container start 36f6736b9c6289da6db95898507796a378cd03e29b51166039c5a5a593377fb0 (image=nginx, maintainer=NGINX Docker Maintainers &lt;docker-maint@nginx.com&gt;, name=stupefied_newton)

# stop事件
2022-12-08T14:20:19.252053300Z container kill 36f6736b9c6289da6db95898507796a378cd03e29b51166039c5a5a593377fb0 (image=nginx, maintainer=NGINX Docker Maintainers &lt;docker-maint@nginx.com&gt;, name=stupefied_newton, signal=3)
2022-12-08T14:20:19.365758840Z container die 36f6736b9c6289da6db95898507796a378cd03e29b51166039c5a5a593377fb0 (exitCode=0, image=nginx, maintainer=NGINX Docker Maintainers &lt;docker-maint@nginx.com&gt;, name=stupefied_newton)
2022-12-08T14:20:19.468679557Z network disconnect fdb00a147400b60e2c0706129ed2b143a13c02c5675b5fc2871c5d4ed42aa478 (container=36f6736b9c6289da6db95898507796a378cd03e29b51166039c5a5a593377fb0, name=bridge, type=bridge)
2022-12-08T14:20:19.535147938Z container stop 36f6736b9c6289da6db95898507796a378cd03e29b51166039c5a5a593377fb0 (image=nginx, maintainer=NGINX Docker Maintainers &lt;docker-maint@nginx.com&gt;, name=stupefied_newton)

```

## 2.2、docker logs

docker logs，通常用于排查容器里面的问题。比如容器里部署的程序执行不起来，由于docker做了隔离，就需要使用docker logs来排除问题。

```
docker logs [OPTIONS] CONTAINER
OPTIONS说明：
-f : 跟踪日志输出
--since :显示某个开始时间的所有日志
-t : 显示时间戳
--tail :仅列出最新N条容器日志
-n :显示最后多少行的日志
```

示例：

```
fly@fly:~$ docker logs -f mynginx
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2022/12/06 15:04:21 [notice] 1#1: using the &quot;epoll&quot; event method
2022/12/06 15:04:21 [notice] 1#1: nginx/1.23.2
2022/12/06 15:04:21 [notice] 1#1: built by gcc 10.2.1 20210110 (Debian 10.2.1-6) 
2022/12/06 15:04:21 [notice] 1#1: OS: Linux 5.4.0-135-generic
2022/12/06 15:04:21 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2022/12/06 15:04:21 [notice] 1#1: start worker processes
2022/12/06 15:04:21 [notice] 1#1: start worker process 29
2022/12/06 15:04:21 [notice] 1#1: start worker process 30

fly@fly:~$ docker logs -n 10 mynginx
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2022/12/06 15:04:21 [notice] 1#1: using the &quot;epoll&quot; event method
2022/12/06 15:04:21 [notice] 1#1: nginx/1.23.2
2022/12/06 15:04:21 [notice] 1#1: built by gcc 10.2.1 20210110 (Debian 10.2.1-6) 
2022/12/06 15:04:21 [notice] 1#1: OS: Linux 5.4.0-135-generic
2022/12/06 15:04:21 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2022/12/06 15:04:21 [notice] 1#1: start worker processes
2022/12/06 15:04:21 [notice] 1#1: start worker process 29
2022/12/06 15:04:21 [notice] 1#1: start worker process 30

```

## 2.3、docker history

查看指定镜像的创建历史。

```
docker history [OPTIONS] IMAGE
OPTIONS说明：
-H :以可读的格式打印镜像大小和日期，默认为true；
--no-trunc :显示完整的提交记录；
-q :仅列出提交记录ID
```

示例：

```
fly@fly:~$ docker history nginx:latest 
IMAGE          CREATED      CREATED BY                                      SIZE      COMMENT
ac8efec875ce   2 days ago   /bin/sh -c #(nop)  CMD [&quot;nginx&quot; &quot;-g&quot; &quot;daemon…   0B        
&lt;missing&gt;      2 days ago   /bin/sh -c #(nop)  STOPSIGNAL SIGQUIT           0B        
&lt;missing&gt;      2 days ago   /bin/sh -c #(nop)  EXPOSE 80                    0B        
&lt;missing&gt;      2 days ago   /bin/sh -c #(nop)  ENTRYPOINT [&quot;/docker-entr…   0B        
&lt;missing&gt;      2 days ago   /bin/sh -c #(nop) COPY file:e57eef017a414ca7…   4.62kB    
&lt;missing&gt;      2 days ago   /bin/sh -c #(nop) COPY file:abbcbf84dc17ee44…   1.27kB    
&lt;missing&gt;      2 days ago   /bin/sh -c #(nop) COPY file:5c18272734349488…   2.12kB    
&lt;missing&gt;      2 days ago   /bin/sh -c #(nop) COPY file:7b307b62e82255f0…   1.62kB    
&lt;missing&gt;      2 days ago   /bin/sh -c set -x     &amp;&amp; addgroup --system -…   61.2MB    
&lt;missing&gt;      2 days ago   /bin/sh -c #(nop)  ENV PKG_RELEASE=1~bullseye   0B        
&lt;missing&gt;      2 days ago   /bin/sh -c #(nop)  ENV NJS_VERSION=0.7.7        0B        
&lt;missing&gt;      2 days ago   /bin/sh -c #(nop)  ENV NGINX_VERSION=1.23.2     0B        
&lt;missing&gt;      2 days ago   /bin/sh -c #(nop)  LABEL maintainer=NGINX Do…   0B        
&lt;missing&gt;      2 days ago   /bin/sh -c #(nop)  CMD [&quot;bash&quot;]                 0B        
&lt;missing&gt;      2 days ago   /bin/sh -c #(nop) ADD file:1f1efd56601ebc26a…   80.5MB    
```

# 三、容器的生命周期管理命令

## 3.1、docker create

创建一个新的容器但不启动它，语法同docker run。

示例：

```
docker create nginx
```

## 3.2、docker run

创建一个新的容器并运行一个命令。

```
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
```

常用选项：

1.  –add-host：容器中hosts文件添加 host:ip 映射记录。

```
docker run --rm -it --add-host db-static:86.75.30.9 ubuntu cat /etc/hosts
```

1.  -a, --attach：附加到 STDIN, STDOUT 或 STDERR。

```
docker run --rm -a stdin -a stdout -i -t ubuntu /bin/bash
```

1.  –cidfile：将容器id写入到指定文件。

```
docker run --rm --cidfile /tmp/idfile ubuntu
```

1.  -d, --detach：后台运行容器并打印容器id。

1.  –detach-keys：指定将容器切回后台的按键，默认：CTRL-p+CTRL-q。

1.  –disable-content-trust：跳过镜像验证，默认为TRUE。

1.  –domainname：设置容器网络服务域名。

1.  –entrypoint：覆盖镜像默认程序入口。

```
docker run --rm -it --entrypoint /bin/bash nginx
```

1.  -e, --env：设置环境变量。

1.  –env-file：从文件中读取环境变量。

1.  –expose：暴露一个或多个端口。

1.  –group-add：为容器用户添加更多用户组。

```
docker run --rm --group-add audio --group-add nogroup --group-add 777 ubuntu id
```

1.  -h, --hostname string：设置主机名。

1.  –init 在容器内运行init，转发信号并捕获进程。

```
# ps 对比容器的元祖进程 
docker run --rm -it ubuntu bash 
docker run --rm -it --init ubuntu bash
```

1.  -i, --interactive：保持标准输入设备处于打开状态。

1.  -l, --label list：设置容器元数据labels。

1.  –label-file list：从文件中读取以换行作为分隔符的label。

1.  –link：添加一个其他容器的链接，及修改当容器的/etc/hosts文件。

1.  –log-driver：容器日志驱动，none、local、json-file、syslog、journald、gelf、fluentd、awslogs、splunk、etwlogs、gcplogs、logentries。

1.  –log-opt：日志驱动选项。

1.  –mount：将文件系统挂载到容器，与 -v 类似。

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

1.  –name：指定容器名称。

1.  –privileged：授予容器超级权限。

1.  -p, --publish：发布容器端口都主机端口。

1.  -P, --publish-all：将所有暴露的端口发布到主机随机端口。

1.  –pull：运行前拉取图像，（always|missing|never）,默认 missing。

1.  –pids-limit：限制容器中pid个数。

1.  –read-only ：将容器跟文件系统装载为只读。

1.  –restart：当容器退出时的重启策略，默认为no。

1.  –rm：当容器退出时，自动删除容器。

1.  –sig-proxy：将接收到的信号代理到进程（默认为true）。

1.  –stop-signal：停止容器的信号（默认为“SIGTERM”）。

1.  –stop-timeout：容器停止超时时长，单位s。

1.  –tmpfs：装载tmpfs目录。

```
docker run -dit --rm --tmpfs /run1:rw,noexec,nosuid,size=65536k ubuntu bash
```

1.  -t, --tty：分配一个伪终端设备。

1.  -u, --user： 用户名或用户ID (格式: <name|uid>[:<group|gid>])。

1.  -v, --volume ：绑定数据卷。

1.  –volume-driver：容器的可选卷驱动程序。

1.  –volumes-from：从指定的容器装载数据卷。

1.  -w, --workdir：容器内的工作目录。

# 总结

docker run主要做三件事情：创建容器、连接网络、启动容器。