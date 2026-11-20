
```
docker-compose -f docker-compose.yml config
```

## 6.4、使用 Compose 命令构建和运行应用

先将刚才写的`Dockerfile`重命令为`Dockerfile.compose`：

```
cp Dockerfile Dockerfile.compose
```

在`helloworld`目录中，执行以下命令来启动应用程序：

```
docker-compose up
```

如果你想在后台执行该服务可以加上`-d`参数：`docker-compose up -d`

运行后的效果：

```
[+] Building 20.6s (15/16)                                                                                                                                                                  
 => [internal] load build definition from Dockerfile.compose                                                                                                                           0.0s
 => => transferring dockerfile: 527B                                                                                                                                                   0.0s
 => [internal] load .dockerignore                                                                                                                                                      0.0s
 => => transferring context: 2B                                                                                                                                                        0.0s
 => [internal] load metadata for docker.io/library/ubuntu:latest                                                                                                                       0.0s
 => [internal] load metadata for docker.io/library/golang:1.18                                                                                                                         0.0s
 => [stage-0 1/5] FROM docker.io/library/golang:1.18                                                                                                                                  20.5s
 => [stage-1 1/5] FROM docker.io/library/ubuntu:latest                                                                                                                                 0.0s
 => [internal] load build context                                                                                                                                                      0.1s
 => => transferring context: 5.43MB                                                                                                                                                    0.1s
 => [stage-0 2/5] ADD ./helloworld /go/src/helloworld/                                                                                                                                 0.1s
 => [stage-1 2/5] COPY ./helloworld/curl-amd64 /usr/bin/curl                                                                                                                           0.1s
 => [stage-0 3/5] WORKDIR /go/src/helloworld                                                                                                                                           0.1s
 => [stage-1 3/5] RUN chmod +x /usr/bin/curl                                                                                                                                           1.1s
 => [stage-0 4/5] RUN go env -w GOPROXY=https://proxy.golang.com.cn,https://goproxy.cn,direct                                                                                          1.1s
 => [stage-1 4/5] WORKDIR /app/                                                                                                                                                        0.0s
 => [stage-0 5/5] RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o app ./                                                                                                        19.0s
 => [stage-1 5/5] COPY --from=0 /go/src/helloworld/app ./                                                                                                                              0.1s
 => exporting to image                                                                                                                                                                 0.1s 
 => => exporting layers                                                                                                                                                                0.1s 
 => => writing image sha256:807800f9f2eef849451f4107c1b422550f2ec0afea34831cee05389aa5670b96                                                                                           0.0s 
 => => naming to docker.io/library/compose-web                                                                                                                                         0.0s 
 => => naming to docker.io/library/myhello:1.0.0                                                                                                                                       0.0s 
 => => naming to localhost:5000/myhello:1.0.0                                                                                                                                          0.0s
[+] Running 5/5
 ⠿ Network compose_mynetwork  Created                                                                                                                                                  0.1s
 ⠿ Network compose_net1       Created                                                                                                                                                  0.1s
 ⠿ Network compose_net2       Created                                                                                                                                                  0.1s
 ⠿ Container myredis          Started                                                                                                                                                  1.1s
 ⠿ Container myweb            Started
```

启动之后可以使用`docker`命令操作我们的容器，也可以用`docker-compose`操作（前提是在当前项目文件夹下，如果离开文件夹，`docker-compose`就会找不到项目；当然，也可以用`-f`和`-p`指定项目，但是略显麻烦）。

以上创建了一个网络，一个`Redis` 和 一个`web`；`postman`调用接口访问`redis`。

进入`redis`容器，查看`redis`数据：

```
fly@fly:~/wokspace/compose$ docker-compose exec -t redis bash
root@e964e083eb70:/data# redis-cli 
127.0.0.1:6379> auth 123456
OK
127.0.0.1:6379> set fly 100
OK
127.0.0.1:6379> get fly
"100"
127.0.0.1:6379> 

```

查看`docker`网络信息：

```
docker network ls
docker network inspect compose_mynetwork
```

示例：
