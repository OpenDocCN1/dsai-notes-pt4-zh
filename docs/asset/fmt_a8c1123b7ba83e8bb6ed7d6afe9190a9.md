
```
$ sudo lsns --output-all 
        NS TYPE   PATH              NPROCS   PID  PPID COMMAND                                        UID USER                NETNSID NSFS
4026531835 cgroup /proc/1/ns/cgroup    211     1     0 /sbin/init auto automatic-ubiquity noprompt      0 root                        
4026531836 pid    /proc/1/ns/pid       210     1     0 /sbin/init auto automatic-ubiquity noprompt      0 root                        
4026531837 user   /proc/1/ns/user      211     1     0 /sbin/init auto automatic-ubiquity noprompt      0 root                        
4026531838 uts    /proc/1/ns/uts       207     1     0 /sbin/init auto automatic-ubiquity noprompt      0 root                        
4026531839 ipc    /proc/1/ns/ipc       210     1     0 /sbin/init auto automatic-ubiquity noprompt      0 root                        
4026531840 mnt    /proc/1/ns/mnt       202     1     0 /sbin/init auto automatic-ubiquity noprompt      0 root                        
4026531860 mnt    /proc/21/ns/mnt        1    21     2 kdevtmpfs                                        0 root                        
4026531992 net    /proc/1/ns/net       210     1     0 /sbin/init auto automatic-ubiquity noprompt      0 root             unassigned 
4026532548 mnt    /proc/534/ns/mnt       1   534     1 /lib/systemd/systemd-udevd                       0 root                        
4026532549 uts    /proc/534/ns/uts       1   534     1 /lib/systemd/systemd-udevd                       0 root                        
4026532622 mnt    /proc/773/ns/mnt       1   773     1 /lib/systemd/systemd-timesyncd                 102 systemd-timesync            
4026532623 uts    /proc/773/ns/uts       1   773     1 /lib/systemd/systemd-timesyncd                 102 systemd-timesync            
4026532624 mnt    /proc/854/ns/mnt       1   854     1 /lib/systemd/systemd-networkd                  100 systemd-network             
4026532625 mnt    /proc/856/ns/mnt       1   856     1 /lib/systemd/systemd-resolved                  101 systemd-resolve             
4026532635 mnt    /proc/1083/ns/mnt      1  1083     1 /usr/sbin/ModemManager                           0 root                        
4026532644 mnt    /proc/1494/ns/mnt      1  1494  1466 registry serve /etc/docker/registry/config.yml   0 root                        
4026532645 uts    /proc/1494/ns/uts      1  1494  1466 registry serve /etc/docker/registry/config.yml   0 root                        
4026532646 ipc    /proc/1494/ns/ipc      1  1494  1466 registry serve /etc/docker/registry/config.yml   0 root                        
4026532647 pid    /proc/1494/ns/pid      1  1494  1466 registry serve /etc/docker/registry/config.yml   0 root                        
4026532649 net    /proc/1494/ns/net      1  1494  1466 registry serve /etc/docker/registry/config.yml   0 root                      0 /run/docker/netns/e7822213c3aa
4026532690 mnt    /proc/909/ns/mnt       1   909     1 /lib/systemd/systemd-logind                      0 root                        
4026532691 mnt    /proc/888/ns/mnt       1   888     1 /usr/sbin/irqbalance --foreground                0 root                        
4026532747 uts    /proc/909/ns/uts       1   909     1 /lib/systemd/systemd-logind                      0 root
```

# 1.2、CNM

CNM（Container Network Model）译为容器网络模型，定义了构建容器虚拟化网络的模型。该模型主要包含三个核心组件 沙盒、端点、网络。

+ 沙盒：一个沙盒包含了一个容器网络栈的信息。沙盒可以对容器的接口、路由和DNS设置进行管理。一个沙盒可以有多个端点和网络。
+ 端点：一个端点可以加入一个沙盒和一个网络。一个端点只可以属于一个网络并且只属于一个沙盒。
+ 网络：一个网络是一组可以直接互相联通的端点。一个网络可以包含多个端点。

# 1.3、libnetwork

`libnetwork` 实现了CNM 的Docker网络组件库。`libnetwork`内置了几种网络驱动：

+ `bridge`驱动。此驱动为Docker的默认设置，使用这个驱动的时候，`libnetwork`将创建出来的docker容器连接到Docker网桥上。作为最常规的模式，`bridge`模式已经可以满足docker容器最基本的使用需求了。
+ `host`驱动。使用这种驱动的时候，`libnetwork`将不为docker容器创建网络协议栈，即不会创建独立的network namespace。Docker容器中的进程处于宿主机的网络环境中，相当于docker容器与宿主机共同使用一个network namespace，使用宿主机的网卡、IP和端口等信息。但是，容器的其他方面，如文件系统、进程列表等还是和宿主机隔离的。集群的时候使用。
+ `overlay`驱动。通过网络虚拟化技术，在同一个物理网络上构建出一张或多张虚拟的逻辑网络。适合大规模的云计算虚拟化环境。
+ `remote`驱动。这个驱动实际上并没有做真正的网络服务实现，而是调用了用户自行实现的网络驱动插件，使`libnetwork`实现了驱动的可插件化，更好的满足了用户的多种需求。用户只要根据`libnetwork`提供的协议标准，实现其所要求的各个接口并向Docker daemon进行注册。
+ `null`驱动。使用这种驱动的时候，Docker容器拥有自己的network namespace，但是并不为Docker容器进行任何网络配置。也就是说，这个Docker容器除了network namespace 自带的loopback网卡外，没有其他任何网卡、ip、路由等信息，需要用户为Docker容器添加网卡、配置IP等。这种模式如果不进行特定的配置是无法正常使用的，但优点也非常明显，它给了用户最大的自由度来定义容器的网络环境。

# 二、实操

## 2.1、`docker network`常用命令

```
# 连接一个容器到一个网络 
docker network connect #Connect a container to a network 
# 创建一个网络 
docker network create #Create a network 
# 将容器从一个网络中断开 
docker network disconnect #Disconnect a container from a network 
# 查看网络的详细信息 
docker network inspect #Display detailed information on one or more networks 
# 查看网络列表 
docker network ls #List networks 
# 移除所有为使用的网络 
docker network prune #Remove all unused networks 
# 移除一个或多个网络 
docker network rm #Remove one or more networks
```

（1）创建网络时，可以指定子网IP地址、网关、IP范围等信息，IP地址斜杆后面的数字表示固定了前面IP的多少位不能改变。（一个IP地址是32bit的）

```
docker network create --subnet=172.11.0.1/16 --gateway=172.11.11.11 --ip-range=172.11.1.2/24 mynet
```

（2）启动容器时绑定网络，注意`--network`要在容器名的前面。

```
docker run -d --network brigh nginx
```

## 2.2、运行一个docker容器，查看CNM三个概念

```
# 运行一个容器 
docker run -d -it --name myubuntu ubuntu /bin/bash 
# 查看容器详信息 
docker inspect myubuntu
```

## 2.3、查看`docker0`在内核路由表上的记录

```
$ route -n
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
0.0.0.0         192.168.8.1     0.0.0.0         UG    100    0        0 ens33
172.17.0.0      0.0.0.0         255.255.0.0     U     0      0        0 docker0
192.168.0.0     0.0.0.0         255.255.240.0   U     0      0        0 ens33
192.168.8.1     0.0.0.0         255.255.255.255 UH    100    0        0 ens33
```

通过以上命令可以找到一条静态路由，该路由表示所有目的IP地址为`172.17.0.0/16`的数据包都从`docker0`网卡发出。

## 2.4、查看网络列表

（1）查看宿主机已有网络列表：

```
$ docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
f06e508f708d   bridge    bridge    local
75f3915547ea   host      host      local
a1b056f00f28   none      null      local
```

docker 安装后默认创建了以上的三个驱动的网络。

查看`bridge`网络详细信息：
