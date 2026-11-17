
```
$ ifconfig 
docker0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.17.0.1  netmask 255.255.0.0  broadcast 172.17.255.255
        inet6 fe80::42:40ff:fe65:67d8  prefixlen 64  scopeid 0x20<link>
        ether 02:42:40:65:67:d8  txqueuelen 0  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 5  bytes 526 (526.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

ens33: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.11.72  netmask 255.255.240.0  broadcast 192.168.15.255
        inet6 fe80::20c:29ff:fe70:53ec  prefixlen 64  scopeid 0x20<link>
        ether 00:0c:29:70:53:ec  txqueuelen 1000  (Ethernet)
        RX packets 1112461  bytes 75138488 (75.1 MB)
        RX errors 0  dropped 2915  overruns 0  frame 0
        TX packets 1015  bytes 139048 (139.0 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 104  bytes 8112 (8.1 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 104  bytes 8112 (8.1 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

veth0fdda42: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet6 fe80::c4fe:c7ff:feb0:8678  prefixlen 64  scopeid 0x20<link>
        ether c6:fe:c7:b0:86:78  txqueuelen 0  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 21  bytes 1742 (1.7 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

veth6a2fa56: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet6 fe80::8cab:c2ff:fe6c:4291  prefixlen 64  scopeid 0x20<link>
        ether 8e:ab:c2:6c:42:91  txqueuelen 0  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 11  bytes 866 (866.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

vethdc1f5f2: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet6 fe80::48f1:13ff:feed:4d31  prefixlen 64  scopeid 0x20<link>
        ether 4a:f1:13:ed:4d:31  txqueuelen 0  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 11  bytes 866 (866.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

（4）分别进入两个`nginx`容器尝试相互访问：

1. 通过`docker exec`进入容器。
2. 通过`docker inspect`查看容器IP。
3. 容器内部相互`curl`访问`http://IP:80`端口。

## 2.5、网络隔离效果展示

我们都知道，不同网络的机器是不能通信的。

（1）创建两个`bridge`网络。

```
docker network create backend
docker network create frontend
```

（2）通过一些命令展示创建后的网络的一些属性，以及计算机上的变化。

```
# 查看宿主机网卡变化 
ifconfig 
# 查看网络属性 
docker network ls 
docker network inspect backend 
docker network inspect frontend 
# 查看路由规则 
route -n
```

（3）运行两个`nginx`容器，分别指定到两个网络。

```
docker run -d -p 83:80 --name mynginx1 --net backend nginx 
docker run -d -p 84:80 --name mynginx2 --net frontend nginx
```

> 通过`docker exec`进入容器。
> 通过`docker inspect`查看容器IP。
> 容器内部相互`curl`访问`http://IP:80`端口。

上述操作，表明两个不在一个网络的两个容器间是无法相互访问的。那么我们增加一个网络，将两个容器接入到新增的网络中。查看容器详细信息，关注网络部分。

```
docker network create middle 
docker network connect middle mynginx1 
docker network connect middle mynginx2
```

从容器详情可以看出，容器多了一个网卡。容器每接入一个网络，就会生成一个对应的网卡。那么我们不难想象到，我们每新建一个容器，会产生两个网卡，第一个是容器内部的网卡用来与网桥通讯的；第二个是网桥上的无IP网卡，用来充当网桥端口的，供容器接入的。

（4）通过共同网络去实现容器间相互访问。

> 通过`docker exec`进入容器。
> 通过`docker inspect`查看容器IP。
> 容器内部相互`curl`访问`http://IP:80`端口。

## 2.6、`host`驱动网络

创建`host`驱动网络，向网络添加容器，演示网卡数，容器网络等情况。

（1）创建`host`驱动的网络实例，注意：一个宿主机只允许有一个`host`驱动的网络实例。

```
docker network create --driver host host1
```

（2）运行一个基于`host`的网络实例的`nginx`容器。

```
# 运行容器 
docker run -d --net host nginx 
# 访问本机80端口 
curl http://localhost
```

（3）查看各种信息。

```
# ifconfig 查看宿主机网卡，并没有因为创建了host网络上的容器而增加网卡信息 
ifconfig 
# 查看host网络实例信息 
docker network inspect host 
# 查看容器网络信息 
docker inspect <容器ID> 
```

容器在接入`host`网络后，已经不具备独立的IP地址了。甚至于运行容器 `-p` 参数都被禁用了。我们可以尝试运行一个`nginx` 指定`-p`参数，并查看容器运行日志。

# 总结

掌握Docker网络高级实战对于构建安全、高效且可扩展的云原生应用程序至关重要。

+   理解Docker网络隔离机制，并使用标签和策略实施网络隔离。

+   通过检查内核路由表和使用命令行工具，了解Docker容器与主机网络之间的连接。

+   自定义Docker网络驱动程序，以满足特定网络需求，例如多主机网络或软件定义网络（SDN）。

欢迎关注我的👸号

每日推送文章

![](img/287599dd0d0c07c3a1f4c07d7e1f2712.png)

长按上图，识别图中二维码即可关注
