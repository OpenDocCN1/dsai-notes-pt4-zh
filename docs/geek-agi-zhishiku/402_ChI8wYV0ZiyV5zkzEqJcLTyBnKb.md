# 云原生时代：学会使用Docker容器运维命令，轻松管理应用

> 来源：[https://geekbang.feishu.cn/docx/ZVBMdNkXeov1kjx9QXKc5KDbnub](https://geekbang.feishu.cn/docx/ZVBMdNkXeov1kjx9QXKc5KDbnub)

> 👨🏻 作者：Lion莱恩呀

> 📃 作者介绍：专注分享高性能服务器后台开发知识，包括C/C++、Linux、网络协议、设计模式、中间件、云原生、数据库、分布式架构等领域知识。

> 🛖 👸号：Lion莱恩呀

> ⏰ 收录时间：2024-05-27 11:15:56 (UTC+8)

> 🔗 原文链接：h[ttps://mp.weixin.q](https://mp.weixin.qq.com/s?__biz=Mzk0ODUzNDc2Mw==&mid=2247485285&idx=1&sn=abb317990ab4d97445bfb80b45036225&scene=19#wechat_redirect)q.com/s?__biz=Mz...

![](img/9c168551ae3c44add79ca87d98ef2c2c.png)

关注作者，获取更多相关知识

# docker ps

列出容器。

```
docker ps [OPTIONS]
```

OPTIONS说明：

-a :显示所有的容器，包括未运行的。

-f :根据条件过滤显示的内容。

–format :指定返回值的模板文件。

-l :显示最近创建的容器。

-n :列出最近创建的n个容器。

–no-trunc :不截断输出。

-q :静默模式，只显示容器编号。

-s :显示总的文件大小。

示例：

```
fly@fly:~$ docker ps 
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS     NAMES
d948ec8c92a2   nginx     &quot;/docker-entrypoint.…&quot;   24 seconds ago   Up 22 seconds   80/tcp    mynginx
fly@fly:~$ docker ps -a
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS     NAMES
d948ec8c92a2   nginx     &quot;/docker-entrypoint.…&quot;   25 seconds ago   Up 23 seconds   80/tcp    mynginx
ae61ef82b947   nginx     &quot;/docker-entrypoint.…&quot;   39 hours ago     Created                   pedantic_poincare
fly@fly:~$ docker ps -f status=running
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS     NAMES
d948ec8c92a2   nginx     &quot;/docker-entrypoint.…&quot;   59 seconds ago   Up 57 seconds   80/tcp    mynginx

```

# docker exec

在运行的容器中执行命令。

```
docker exec [OPTIONS] CONTAINER COMMAND [ARG...]
```

OPTIONS说明：

-d :分离模式: 在后台运行

-i :即使没有附加也保持STDIN 打开

-t :分配一个伪终端

示例：

```
fly@fly:~$ docker exec mynginx ls
bin
boot
dev
docker-entrypoint.d
docker-entrypoint.sh
etc
home
lib
lib64
media
mnt
opt
proc
root
run
sbin
srv
sys
tmp
usr
var
```

```
fly@fly:~$ docker exec -it mynginx bash
root@d948ec8c92a2:/# ls
bin  boot  dev        docker-entrypoint.d  docker-entrypoint.sh  etc        home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@d948ec8c92a2:/# exit
exit

```

# docker inspect  docker 检查

获取容器/镜像的元数据。

```
docker inspect [OPTIONS] NAME|ID [NAME|ID...]
```

OPTIONS说明：

-f :指定返回值的模板文件。

-s :显示总的文件大小。

–type :为指定类型返回JSON

示例：

```
# 查看容器的示例id
docker inspect -f &#x27;{{.Id}}&#x27; &lt;id&gt;
# 检查镜像或者容器的参数，默认返回 JSON 格式
docker inspect &lt;id&gt;
```

# docker top  docker 顶部

查看容器中运行的进程信息，支持 ps 命令参数。

```
docker top [OPTIONS] CONTAINER [ps OPTIONS]
```

示例：

```
# 查看所有运行容器的进程信息。
for i in `docker ps |grep Up|awk &#x27;{print $1}&#x27;`;do echo \ &amp;&amp;docker top $i; done
```

```
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
165536              1835                1811                0                   05:35               ?                   00:00:00            nginx: master process nginx -g daemon off;
165637              1898                1835                0                   05:35               ?                   00:00:00            nginx: worker process
165637              1899                1835                0                   05:35               ?                   00:00:00            nginx: worker process
```

# docker attach  docker 附加

连接到正在运行中的容器。

```
docker attach [OPTIONS] CONTAINER

```

docker exec 与 docker attach 的区别：

1.  exec 在容器中执行命令，并且可以通过-i -t 创建虚拟终端的方式与容器交互。

1.  attach 进入容器某个正在执行的命令终端，不能交互操作。但是如果该容器的命令终端是一个可以
交互的终端，那么也可以交互。

**退出attach模式：**

1.  运行容器时指定 -i -t 参数，那么attach容器后可通过Ctrl + P + Ctrl + Q 退出attach状态。

```
docker run -d -it -P nginx
docker attach &lt;containerID&gt;
```

```
fly@fly:~$ docker run -d -it -P nginx
fe4450ec046e9423194c5f22b2dfdef242539b0fa4cc62d1698d96da06cb461e
fly@fly:~$ docker attach fe4450ec046e9423194c5f22b2dfdef242539b0fa4cc62d1698d96da06cb461e
2022/12/10 06:07:32 [notice] 29#29: signal 28 (SIGWINCH) received
2022/12/10 06:07:32 [notice] 1#1: signal 28 (SIGWINCH) received
2022/12/10 06:07:32 [notice] 30#30: signal 28 (SIGWINCH) received
2022/12/10 06:07:32 [notice] 29#29: signal 28 (SIGWINCH) received
2022/12/10 06:07:32 [notice] 1#1: signal 28 (SIGWINCH) received
2022/12/10 06:07:32 [notice] 30#30: signal 28 (SIGWINCH) received
read escape sequence
```

1.  仅指定-t参数，可以通过Ctrl + C 退出attach状态。

```
docker run -d -t -P nginx
docker attach &lt;containerID&gt;
```

1.  不指定-t参数，在attach的时候指定 --sig-proxy=false ，可以通过 Ctrl + C 退出attach状态。–sig-proxy 仅在没有指定-t参数的情况下生效。于–sig-proxy 默认值为true，默认将所有接收信号代理给了容器中的进程，所以Ctrl + C 的时候会将sigint (signal interrupt)终止信号发送到attach进程，从而转发到被attach的进程，导致容器中进程中断，从而退出容器。

```
docker run -d -i -P nginx
docker run -d -P nginx
docker attach --sig-proxy=false &lt;containerID&gt;
```

# docker wait  docker 等待

阻塞运行直到容器停止，然后打印出它的退出代码。

```
docker wait [OPTIONS] CONTAINER [CONTAINER...]
```

# docker export  docker 导出

将文件系统作为一个tar归档文件导出到STDOUT。

```
docker export [OPTIONS] CONTAINER
```

OPTIONS说明：

-o :将输入内容写到文件。

```
docker export mynginx -o mynginx.tar
# 解压归档文件，查看内容
tar vxf mynginx.tar -c mynginx

```

当容器出现问题时，可以将文件导出，保留现场。

# docker import  docker 导入

将文件系统导入指定容器。

```
docker import [OPTIONS] file|URL|- [REPOSITORY[:TAG]]
```

```
Import the contents from a tarball to create a filesystem image

Options:
  -c, --change list       Apply Dockerfile instruction to the created image
  -m, --message string    Set commit message for imported image
      --platform string   Set platform if server is multi-platform capable

```

# docker port  docker 端口

列出指定的容器的端口映射，或者查找将PRIVATE_PORT NAT到面向公众的端口。

```
docker port [OPTIONS] CONTAINER [PRIVATE_PORT[/PROTO]]
```

示例：

```
fly@fly:~$ docker port mynginx1 
80/tcp -&gt; 0.0.0.0:49154
80/tcp -&gt; :::49154
```

# docker cp

用于容器与主机之间的数据拷贝。

```
docker cp [OPTIONS] CONTAINER:SRC_PATH DEST_PATH|-
docker cp [OPTIONS] SRC_PATH|- CONTAINER:DEST_PATH
```

OPTIONS说明：

-L :保持源目标中的链接

示例，将容器的文件拷贝到主机：

```
fly@fly:~$ docker cp mynginx1:docker-entrypoint.sh /home/fly/
fly@fly:~$ ls
docker-entrypoint.sh
```

# docker diff  docker 差异

检查容器里文件结构的更改。docker diff命令会列出 3 种容器内文件状态变化（A - Add, D - Delete, C - Change ）的列表清单。

```
docker diff [OPTIONS] CONTAINER
```

示例：

```
fly@fly:~$ docker diff mynginx1
C /etc
C /etc/nginx
C /etc/nginx/conf.d
C /etc/nginx/conf.d/default.conf
C /run
A /run/nginx.pid
C /var
C /var/cache
C /var/cache/nginx
A /var/cache/nginx/proxy_temp
A /var/cache/nginx/scgi_temp
A /var/cache/nginx/uwsgi_temp
A /var/cache/nginx/client_temp
A /var/cache/nginx/fastcgi_temp
```

# docker rename  docker 重命名

重命名容器名。

```
docker rename CONTAINER NEW_NAME
```

示例：

```
fly@fly:~$ docker rename mynginx1 mynginx2
fly@fly:~$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED             STATUS             PORTS                                     NAMES
b6ccc26e22e6   nginx     &quot;/docker-entrypoint.…&quot;   15 minutes ago      Up 15 minutes      0.0.0.0:49154-&gt;80/tcp, :::49154-&gt;80/tcp   mynginx2
fe4450ec046e   nginx     &quot;/docker-entrypoint.…&quot;   33 minutes ago      Up 33 minutes      0.0.0.0:49153-&gt;80/tcp, :::49153-&gt;80/tcp   inspiring_hypatia
d948ec8c92a2   nginx     &quot;/docker-entrypoint.…&quot;   About an hour ago   Up About an hour   80/tcp                                    mynginx
```

# docker stats  docker 统计信息

容器监控，示例：

```
CONTAINER ID   NAME       CPU %     MEM USAGE / LIMIT     MEM %     NET I/O       BLOCK I/O     PIDS
b6ccc26e22e6   mynginx2   0.00%     4.344MiB / 1.907GiB   0.22%     1.01kB / 0B   0B / 16.4kB   3
```

# docker update  docker 更新

出docker update 是用来修改docker run 指定的运行参数。

示例：

```
docker update --restart always myubuntu3
docker update --restart no myubuntu3
```

# 总结

容器遵循最小原则，会尽可能不打包 非必须的程序进去。

docker attach如果没有指定-i -t或者–sig-proxy=false，那么使用CTL+C的方式会把容器也退出。