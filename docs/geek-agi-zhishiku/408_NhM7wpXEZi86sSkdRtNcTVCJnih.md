# 掌握云原生：Dockerfile实践指南

> 来源：[https://geekbang.feishu.cn/docx/Vmb5dSbBKoBmD9xd9eoclTFznyg](https://geekbang.feishu.cn/docx/Vmb5dSbBKoBmD9xd9eoclTFznyg)

> 👨🏻 作者：Lion莱恩呀

> 📃 作者介绍：专注分享高性能服务器后台开发知识，包括C/C++、Linux、网络协议、设计模式、中间件、云原生、数据库、分布式架构等领域知识。

> 🛖 👸号：Lion莱恩呀

> ⏰ 收录时间：2024-05-27 11:15:56 (UTC+8)

> 🔗 原文链接：h[ttps://mp.weixin.q](https://mp.weixin.qq.com/s?__biz=Mzk0ODUzNDc2Mw==&mid=2247485468&idx=1&sn=4513e344ef600b7c894a7e85daab6e85&scene=19#wechat_redirect)q.com/s?__biz=Mz...

![](img/7538149d92becdf922a62175d962f853.png)

关注作者，获取更多相关知识

# 一、多阶段构建

制作docker镜像时，遵循越小越好，尽量剔除不需要的内容。比如编译环境，程序编译完成之后就不需要了；所以引入多阶段构建剔除不需要的内容。

Docker 17.05版本以后，新增了Dockerfile多阶段构建。所谓多阶段构建，实际上是允许一个Dockerfile中出现多个 FROM 指令。

多个 FROM 指令的意义：多个 FROM 指令并不是为了生成多根的层关系，最后生成的镜像，仍以最后一条 FROM 为准，之前的FROM 会被抛弃，那么之前的FROM 又有什么意义呢？

每一条 FROM 指令都是一个构建阶段，多条 FROM 就是多阶段构建，虽然最后生成的镜像只能是最后一个阶段的结果，但是，能够将前置阶段中的文件拷贝到后边的阶段中，这就是多阶段构建的最大意义。

最大的使用场景是将编译环境和运行环境分离，比如，之前我们需要构建一个Go语言程序，那么就需要用到go命令等编译环境。

（1）准备工作：

```
# 创建一个目录
mkdir example3
cd example3
# 下载nginx源码包，作为素材
curl https://nginx.org/download/nginx-1.21.6.tar.gz &gt; ./nginx-1.21.6.tar.gz
# 下载app代码
git clone https://gitee.com/nickdemo/helloworld
```

（2）多阶段构建dockerfile：

```
FROM golang:1.18
WORKDIR helloworld
COPY ./helloworld ./
RUN go env -w GOPROXY=https://proxy.golang.com.cn,https://goproxy.cn,direct
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o app .
RUN pwd

FROM alpine:latest
MAINTAINER fly
ENV env1=v1
ENV env2=v2
LABEL myhello 1.0.0
LABEL env prod
COPY --from=0 /go/helloworld/app ./
EXPOSE 80
CMD [&quot;./app&quot;,&quot;--param1=p1&quot;,&quot;--param2=p2&quot;]

```

（3）也可以通过as关键词，为构建阶段指定别名，可以提高可读性：

```
FROM golang:1.18 as s0
WORKDIR helloworld
COPY ./helloworld ./
RUN go env -w GOPROXY=https://proxy.golang.com.cn,https://goproxy.cn,direct
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o app .
RUN pwd

FROM alpine:latest
MAINTAINER fly
ENV env1=v1
ENV env2=v2
LABEL myhello 1.0.0
LABEL env prod
COPY --from=s0 /go/helloworld/app ./
EXPOSE 80
CMD [&quot;./app&quot;,&quot;--param1=p1&quot;,&quot;--param2=p2&quot;]

```

（4）构建镜像：

```
docker build -t hello:1.0.0 -f Dockerfile .
```

执行结果：

```
fly@fly:~/workspace/example01$ docker build -t hello:1.0.0 -f Dockerfile .
Sending build context to Docker daemon  5.477MB
Step 1/15 : FROM golang:1.18
1.18: Pulling from library/golang
32de3c850997: Pull complete 
fa1d4c8d85a4: Pull complete 
c796299bbbdd: Pull complete 
81283a9569ad: Pull complete 
c768848b86a2: Pull complete 
160a777925fe: Pull complete 
1be94824532a: Pull complete 
...
Successfully built 429a1b2d8c89
Successfully tagged hello:1.0.0

```

（5）docker images查看镜像。可以看到生成的镜像只有十几兆，不像上一篇中的至少1G多。同时可以看到一个 <none> 的的中间镜像，即s0阶段的镜像；这个镜像是多阶段构建抛弃的不需要的内容。

```
REPOSITORY           TAG       IMAGE ID       CREATED         SIZE
hello                1.0.0     429a1b2d8c89   4 minutes ago   15.9MB
&lt;none&gt;               &lt;none&gt;    608d482ea60f   4 minutes ago   1.08GB
golang               1.18      fffd0d9a59da   2 days ago      965MB
alpine               latest    49176f190c7e   4 weeks ago     7.05MB

```

（6）运行镜像：

```
docker run -d -p 81:80 --name myhello hello:1.0.0
```

（7）测试镜像：

```
curl http://localhost:81/ping
```

测试结果

```
fly@fly:~/workspace/example01$ curl http://localhost:81/ping
ip = 172.17.0.2
```

# 二、ADD 与 COPY 指令

（1）ADD 与 COPY 不能拷贝上下文以外的文件。

COPY 命令语法格式：

```
COPY &lt;src&gt; &lt;dest&gt;                         //将上下文中源文件，拷贝到目标文件
COPY prefix* /destDir/                 //将所有prefix 开头的文件拷贝到 destDir 目录下
COPY prefix?.log /destDir/         //支持单个占位符，例如 ：prefix1.log、prefix2.log 等
```

ADD 命令语法：

```
ADD &lt;src&gt; &lt;dest&gt;
```

ADD 命令除了不能用在 multistage 的场景下，ADD 命令可以完成 COPY 命令的所有功能，并且还可

完成两类的功能：

1.  解压压缩文件并把它们添加到镜像中，对于宿主机本地压缩文件，ADD命令会自动解压并添加到镜像。

1.  从 url 拷贝文件到镜像中，需要注意：url 所在文件如果是压缩包，ADD 命令不会自动解压缩。

（2）对于目录而言，COPY 和 ADD 命令具有相同的特点：只复制目录中的内容而不包含目录自身。例如：

```
COPY srcDir /destDir/ //只会将源文件夹srcDir下的文件拷贝到 destDir 目录下
```

（3）COPY 区别于ADD在于Dockerfile中使用multi-stage。

```
FROM golang:1.18 as stage0
ADD ./helloworld /go/src/helloworld/
WORKDIR /go/src/helloworld
RUN go env -w GOPROXY=https://proxy.golang.com.cn,https://goproxy.cn,direct
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o app .
FROM alpine:latest
ENV env1=env1value
ENV env2=env2value
MAINTAINER nick
LABEL hello 1.0.0
WORKDIR /app/
COPY --from=stage0 /go/src/helloworld/app ./
EXPOSE 80
CMD [&quot;./app&quot;,&quot;--param1=p1&quot;,&quot;--param2=p2&quot;]
```

## 示例

（1）下载nginx tar.gz包作为素材。

```
curl https://nginx.org/download/nginx-1.21.6.tar.gz &gt; ./nginx-1.21.6.tar.gz
```

（2）编写Dockerfile文件。

```
FROM golang:1.18 as stage0
# ADD ./helloworld /go/src/helloworld/
COPY ./helloworld /go/src/helloworld/
WORKDIR /go/src/helloworld
RUN go env -w GOPROXY=https://proxy.golang.com.cn,https://goproxy.cn,direct
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o app .
FROM alpine:latest
ENV env1=env1value
ENV env2=env2value
MAINTAINER nick
LABEL hello 1.0.0
ADD https://nginx.org/download/nginx-1.21.6.tar.gz /soft/
COPY nginx-1.21.6.tar.gz /soft/copy/
ADD nginx-1.21.6.tar.gz /soft/add/
WORKDIR /app/
COPY --from=stage0 /go/src/helloworld/app ./
EXPOSE 80
CMD [&quot;./app&quot;,&quot;--param1=p1&quot;,&quot;--param2=p2&quot;]
```

（3）构建镜像。

```
docker build -t hello:1.0.0 -f Dockerfile .
```

（4）运行容器。

```
docker run -d --name hello hello:1.0.0
```

（5）查看add和copy的文件。

```
docker exec -it hello /bin/sh
```

注意：目标文件位置要注意路径后面是否带 “/” ，带斜杠表示目录，不带斜杠表示文件名文件名里带有空格，需要再 ADD（或COPY）指令里用双引号的形式标明：

```
ADD &quot;hello world.txt&quot; &quot;/tmp/hello world.txt&quot;
```

# 三、CMD指令

CMD 指令有三种格式：

```
# shell 格式
CMD &lt;command&gt;
# exec格式，推荐格式
CMD [&quot;executable&quot;,&quot;param1&quot;,&quot;param2&quot;]
# 为ENTRYPOINT 指令提供参数
CMD [&quot;param1&quot;,&quot;param2&quot;]
```

1.  CMD 指令提供容器运行时的默认值，这些默认值可以是一条指令，也可以是一些参数。

1.  一个dockerfile中可以有多条CMD指令，但只有最后一条CMD指令有效。

1.  CMD参数格式是在CMD指令与ENTRYPOINT指令配合时使用，CMD指令中的参数会添加到ENTRYPOINT指令中。

1.  使用shell 和exec 格式时，命令在容器中的运行方式与RUN 指令相同。不同在于，RUN指令在构建镜像时执行命令，并生成新的镜像。

1.  CMD指令在构建镜像时并不执行任何命令，而是在容器启动时默认将CMD指令作为第一条执行的命令。如果在命令行界面运行docker run 命令时指定命令参数，则会覆盖CMD指令中的命令。

## 示例

```
FROM golang:1.18 as stage0
ADD ./helloworld /go/src/helloworld/
WORKDIR /go/src/helloworld
RUN go env -w GOPROXY=https://proxy.golang.com.cn,https://goproxy.cn,direct
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o app .
FROM alpine:latest
ENV env1=env1value
ENV env2=env2value
MAINTAINER nick
LABEL hello 1.0.0
WORKDIR /app/
COPY --from=stage0 /go/src/helloworld/app ./
EXPOSE 80
CMD [&quot;./app&quot;,&quot;--param1=p1&quot;,&quot;--param2=p2&quot;]
```

构建镜像和运行：

```
docker build -t hello:1.0.0 .
# 指定启动命令和参数
docker run -d -p 80:80 hello:1.0.0 ./app --param1=1 --param2=2
# 指定启动命令为sh
docker run -dit -p 80:80 hello:1.0.0 sh
```

访问：

```
curl http://localhost/print/startup
```

执行结果：

```
$ curl http://localhost/print/startup
start up params:     param1 = 1 and param2 = 2 
```

# 四、ENTRYPOINT指令

ENTRYPOINT指令有两种格式：

```
# shell 格式
ENTRYPOINT &lt;command&gt;
# exec 格式，推荐格式
ENTRYPOINT [&quot;executable&quot;,&quot;param1&quot;,&quot;param2&quot;]
```

1.  ENTRYPOINT指令和CMD指令类似，都可以让容器每次启动时执行相同的命令，但它们之间又有不同。一个Dockerfile中可以有多条ENTRYPOINT指令，但只有最后一条ENTRYPOINT指令有效。

1.  当使用shell格式时，ENTRYPOINT指令会忽略任何CMD指令和docker run 命令的参数，并且会运行在bin/sh -c中。

1.  推荐使用exec格式，使用此格式，docker run 传入的命令参数将会覆盖CMD指令的内容并且附加到ENTRYPOINT指令的参数中。

1.  CMD可以是参数，也可以是指令，ENTRYPOINT只能是命令；docker run 命令提供的运行命令参数可以覆盖CMD，但不能覆盖ENTRYPOINT。

1.  可以通过docker run --entrypoint 替换容器的入口程序。

## 示例

```
FROM golang:1.18 as stage0
ADD ./helloworld /go/src/helloworld/
WORKDIR /go/src/helloworld
RUN go env -w GOPROXY=https://proxy.golang.com.cn,https://goproxy.cn,direct
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o app .
FROM alpine:latest
ENV env1=env1value
ENV env2=env2value
MAINTAINER nick
LABEL hello 1.0.0
WORKDIR /app/
COPY --from=stage0 /go/src/helloworld/app ./
EXPOSE 80
ENTRYPOINT [&quot;./app&quot;,&quot;--param1=p1&quot;,&quot;--param2=p2&quot;]
```

构建和运行：

```
docker build -t hello:1.0.0 .
# 指定启动参数
docker run -d -p 80:80 hello:1.0.0 --param1=1 --param2=2
# 指定入口程序为sh
docker run -dit -p 80:80 --entrypoint sh hello:1.0.0
```

# 五、build-arg

dockerfile 预定义参数：HTTP_PROXY，http_proxy，HTTPS_PROXY，https_proxy，FTP_PROXY，ftp_proxy，NO_PROXY，no_proxy，ALL_PROXY，all_proxy。预定义参数，可直接在dockerfile中使用，无需使用arg来声明。

```
FROM golang:1.18 as s0
ADD ./helloworld /go/src/helloworld/
WORKDIR /go/src/helloworld
# RUN go env -w GOPROXY=https://proxy.golang.com.cn,https://goproxy.cn,direct
RUN go env -w GOPROXY=$http_proxy
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o app .
FROM alpine:latest as s1
ARG wd label tag
RUN echo $wd,$label,$tag
ENV env1=env1value
ENV env2=env2value
MAINTAINER nick
LABEL $label $tag
WORKDIR $wd
COPY --from=stage0 /go/src/helloworld/app ./
EXPOSE 80
ENTRYPOINT [&quot;./app&quot;,&quot;--param1=p1&quot;,&quot;--param2=p2&quot;]
```

构建：

```
docker build -t hello:1.0.0 --build-arg &quot;http_proxy=https://proxy.golang.com.cn,https://goproxy.cn,direct&quot; --build-arg &quot;wd=/home/app/&quot; --build-arg &quot;label=myhello&quot; --build-arg &quot;tag=1.0.0&quot; --no-cache .
```

# 六、target与cache-from

对于多阶段构建，可以通过–target指定需要重新构建的阶段。–cache-from 可以指定一个镜像作为缓存源，当构建过程中dockerfile指令与缓存镜像源指令匹配，则直接使用缓存镜像中的镜像层，从而加快构建进程。可以将缓存镜像推送到远程注册中心，提供给不同的构建过程使用，在使用前需要先pull到本地。

```
FROM golang:1.18 as s0
ADD ./helloworld /go/src/helloworld/
WORKDIR /go/src/helloworld
RUN go env -w GOPROXY=https://proxy.golang.com.cn,https://goproxy.cn,direct
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o app .
FROM alpine:latest as s1
ENV env1=env1value
ENV env2=env2value
MAINTAINER nick
LABEL hello 1.0.0
WORKDIR /app
COPY --from=s0 /go/src/helloworld/app ./
EXPOSE 80
ENTRYPOINT [&quot;./app&quot;,&quot;--param1=p1&quot;,&quot;--param2=p2&quot;]
```

构建：

```
docker build -t prehello:1.0.0 --target s0 .
# or
docker build -t hello:1.0.0 --cache-from prehello:1.0.0 --target s1 .
```

# 七、onbuild

onbuild指令将指令添加到镜像中，当镜像作为另一个构建的基础镜像时，将触发这些指令执行。触发指令将在下游构建的上下文中执行。注意：onbuild指令不会影响当前构建，但会影响下游构建。

示例：

（1）生成自定义基础镜像：

```
FROM golang:1.18
ONBUILD ADD ./helloworld /go/src/helloworld/
ONBUILD WORKDIR /go/src/helloworld
ONBUILD RUN go env -w
GOPROXY=https://proxy.golang.com.cn,https://goproxy.cn,direct
ONBUILD RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o app .
```

构建：

```
docker build -t mygolang:1.0.0 .
```

（2）通过自定义基础镜像构建新的镜像：

```
FROM mygolang:1.0.0 as s0
FROM alpine:latest as s1
ENV env1=env1value
ENV env2=env2value
MAINTAINER nick
LABEL hello 1.0.0
WORKDIR /app
COPY --from=s0 /go/src/helloworld/app ./
EXPOSE 80
ENTRYPOINT [&quot;./app&quot;,&quot;--param1=p1&quot;,&quot;--param2=p2&quot;]
```

构建：

```
docker build -t hello:1.0.0 .
```

# 总结

在本文中，我们深入探讨了Dockerfile中多阶段构建、ADD、COPY、CMD、ENTRYPOINT、build-arg、target与cache-from以及onbuild等关键指令的实践方法。通过示例更好地理解这些指令在云原生环境中的应用场景和最佳实践，并能够利用它们来构建高效、可靠的云原生应用。