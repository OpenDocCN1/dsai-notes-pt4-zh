
```
Sending build context to Docker daemon  2.048kB
Step 1/12 : FROM golang:1.18
 ---> fffd0d9a59da
Step 2/12 : MAINTAINER fly
 ---> Using cache
 ---> 563fafbaa215
Step 3/12 : ENV env1=v1
 ---> Using cache
 ---> 00cf872c162e
Step 4/12 : ENV env2=v2
 ---> Using cache
 ---> 675573fda93f
Step 5/12 : LABEL myhello 1.0.0
 ---> Using cache
 ---> 7fe90336bfc0
Step 6/12 : LABEL env prod
 ---> Using cache
 ---> 1f518a4de736
Step 7/12 : RUN git clone https://gitee.com/nickdemo/helloworld.git
 ---> Using cache
 ---> 497dfec57d8f
Step 8/12 : WORKDIR helloworld
 ---> Using cache
 ---> 581cbde35690
Step 9/12 : RUN go env -w GOPROXY="https://proxy.golang.com.cn,https://goproxy.cn,direct"
 ---> Running in aa3d7b78e3ea
Removing intermediate container aa3d7b78e3ea
 ---> 228d20762041
Step 10/12 : RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o app .
 ---> Running in 0b4899bd2333
go: downloading github.com/gomodule/redigo v1.8.9
go: downloading github.com/spf13/viper v1.12.0
go: downloading github.com/fsnotify/fsnotify v1.5.4
go: downloading github.com/mitchellh/mapstructure v1.5.0
go: downloading github.com/spf13/afero v1.8.2
go: downloading github.com/spf13/cast v1.5.0
go: downloading github.com/spf13/jwalterweatherman v1.1.0
go: downloading github.com/spf13/pflag v1.0.5
go: downloading golang.org/x/sys v0.0.0-20220520151302-bc2c85ada10a
go: downloading golang.org/x/text v0.3.7
go: downloading github.com/subosito/gotenv v1.3.0
go: downloading github.com/hashicorp/hcl v1.0.0
go: downloading gopkg.in/ini.v1 v1.66.4
go: downloading github.com/magiconair/properties v1.8.6
go: downloading github.com/pelletier/go-toml/v2 v2.0.1
go: downloading gopkg.in/yaml.v3 v3.0.0
go: downloading github.com/pelletier/go-toml v1.9.5
Removing intermediate container 0b4899bd2333
 ---> 8a0543eb5966
Step 11/12 : EXPOSE 80
 ---> Running in 5de109cc2133
Removing intermediate container 5de109cc2133
 ---> 11f4f6f09a9e
Step 12/12 : CMD ["./app","--param1=p1","--param2=p2"]
 ---> Running in 49caece0cdf7
Removing intermediate container 49caece0cdf7
 ---> 308a6e93a8ff
Successfully built 308a6e93a8ff
Successfully tagged hello:1.0.0
```

（6）查看`docker images`。发现有了`hello:1.0.0`。

```
fly@fly:~/wokspace/example1$ docker images
REPOSITORY               TAG       IMAGE ID       CREATED              SIZE
hello                    1.0.0     308a6e93a8ff   About a minute ago   1.08GB
golang                   1.18      fffd0d9a59da   41 hours ago         965MB
nginx                    latest    ac8efec875ce   2 weeks ago          142MB
nongtengfei/mynginx      1.0.0     ac8efec875ce   2 weeks ago          142MB
localhost:5000/mynginx   1.0.0     ac8efec875ce   2 weeks ago          142MB
registry                 2         81c944c2288b   5 weeks ago          24.1MB
```

（7）运行镜像。

```
docker run -p 80:80 --name myhello hello:1.0.0
```

执行结果：

```
fly@fly:~/wokspace/example1$ docker run -p 80:80 --name myhello hello:1.0.0 
into main
```

也可以后台执行：

```
docker run -p 80:80 -d --name myhello hello:1.0.0
```

然后使用`curl`访问：

```
$ curl http://localhost:80/print/env
env list : env1 = v1 and env2 = v2
```

### 问题检查

上述制作的镜像有什么问题？

1. 镜像非常大，什么都没做就是1.08GB。不方便传输。

1. 使用了`git`软件，这个在`golang`的基础镜像默认有的，但是如果是`gcc`环境，可能就没有这个`git`软件。

1. 构建时传输了上下文，但是上下文基本没有用到。

## 2.2、基本语法实践 — `gcc`

（1）创建文件夹，存放`Dockerfile`。注意，默认`Dockerfile`文件的首字母要大写，当不指定`Dockerfile`时`docker`会自动去匹配。

```
mkdir example2
cd example2
vim Dockerfile
```

（2）`Dockerfile`输入如下内容：

```
FROM gcc
MAINTAINER fly
COPY ./hello.c ./
RUN pwd
LABEL myhello 1.0.0
LABEL env prod
RUN gcc hello.c -o hello
CMD ["./hello"]
```

（3）创建`hello.c`并输入内容：

（`hello.c`）

```
#include <stdio.h>
int main(int argc,char **argv)
{
        printf("hello world\n");
        return 0;
}
```

（4）构建镜像：

```
docker build -t myhelloc .
```

执行结果：

```
Sending build context to Docker daemon  3.072kB
Step 1/8 : FROM gcc
latest: Pulling from library/gcc
32de3c850997: Already exists 
fa1d4c8d85a4: Already exists 
c796299bbbdd: Already exists 
81283a9569ad: Already exists 
60b38700e7fb: Pull complete 
0db15631b022: Pull complete 
becc68bc09a5: Pull complete 
c369162968fc: Pull complete 
ce5ec26c51fc: Pull complete 
Digest: sha256:6c101c7698a6ebe5cd153182889ffc9ab2f7192bf96a06fe292806116fdaafba
Status: Downloaded newer image for gcc:latest
 ---> e94a76bddd72
Step 2/8 : MAINTAINER fly
 ---> Running in e193e6e7dac1
Removing intermediate container e193e6e7dac1
 ---> afa92db551f4
Step 3/8 : COPY ./hello.c ./
 ---> 87b51ccd7de3
Step 4/8 : RUN pwd
 ---> Running in 569f3ddfaf53
/
Removing intermediate container 569f3ddfaf53
 ---> 2ee756f47004
Step 5/8 : LABEL myhello 1.0.0
 ---> Running in 2deaebd60838
Removing intermediate container 2deaebd60838
 ---> 1610bf7d0792
Step 6/8 : LABEL env prod
 ---> Running in ce840c209a93
Removing intermediate container ce840c209a93
 ---> b6158d8d3aac
Step 7/8 : RUN gcc hello.c -o hello
 ---> Running in 6f01803e28b1
Removing intermediate container 6f01803e28b1
 ---> 6c9223360ad5
Step 8/8 : CMD ["./hello"]
 ---> Running in 9127f8f3e413
Removing intermediate container 9127f8f3e413
 ---> a88d99f3d8e8
Successfully built a88d99f3d8e8
Successfully tagged myhelloc:latest
```

（5）执行镜像：

```
docker run myhelloc
```

执行结果：

```
fly@fly:~/wokspace/example2$ docker run myhelloc
hello world
```

如果是后台执行，可以通过`docker log`查询到输出的`hello world`。

# 总结

1. 构建镜像时，选择上下文要注意，不要包含有不要用到的内容，通常使用项目的根目录作为上下文即可。

1. `COPY`命令时，基本上是`"./*"`开始，没有绝对路径，因为构建的时候上下文已经发送到守护进程，不在宿主机了。

1. `go`语言和`c`语言编译的区别，主要在`FROM`部分，依赖的基础环境；另外，编译命令也不一样。
