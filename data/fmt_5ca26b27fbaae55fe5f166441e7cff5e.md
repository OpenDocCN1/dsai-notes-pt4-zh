
# Docker网络高级实战：隔离网络、检查路由表和自定义网络驱动

> 来源：[https://geekbang.feishu.cn/docx/VHxUdgHiQotomFx2NvRc3WpZn3d](https://geekbang.feishu.cn/docx/VHxUdgHiQotomFx2NvRc3WpZn3d)

> 👨🏻 作者：Lion莱恩呀

> 📃 作者介绍：专注分享高性能服务器后台开发知识，包括C/C++、Linux、网络协议、设计模式、中间件、云原生、数据库、分布式架构等领域知识。

> 🛖 👸号：Lion莱恩呀

> ⏰ 收录时间：2024-05-27 11:15:56 (UTC+8)

> 🔗 原文链接：h[ttps://mp.weixin.q](https://mp.weixin.qq.com/s?__biz=Mzk0ODUzNDc2Mw==&mid=2247485815&idx=1&sn=525d9f8aba9e547453610ee94d56fde8&scene=19#wechat_redirect)q.com/s?__biz=Mz...

![](img/55a165e581628f115317195039126230.png)

关注作者，获取更多相关知识

## 一、相关概念

### 1.1、网桥

在一台未经特殊网络配置的Ubuntu机器上安装完Docker之后，在宿主机上通过`ifconfig`命令可以看到多了一块名为`docker0`的网卡。该网卡即是`docker0`网桥，网桥在这里的作用相当于交换机，为连接在其上的设备转发数据帧。

网桥上的`veth`网卡设备相当于交换机上的端口，可以将多个容器或虚拟机连接其上，这些端口工作在二层（链路层），所以是不需要配置IP信息的。

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
        RX packets 124134  bytes 8441977 (8.4 MB)
        RX errors 0  dropped 257  overruns 0  frame 0
        TX packets 271  bytes 49957 (49.9 KB)
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
        TX packets 17  bytes 1462 (1.4 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

`docker`的所有容器，都是通过网桥进行通信。

![](img/0b77d085dde214440f2618aa2325346c.png)

```
$ docker inspect registry
[
    {
        "Id": "22634e72e7a3f6fb38991d9e380e514e3ff4b89f418256a54c9f0a0cd5746f1f",
        "Created": "2022-12-13T03:42:35.725091364Z",
        "Path": "/entrypoint.sh",
        "Args": [
            "/etc/docker/registry/config.yml"
        ],
        ......
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "e7822213c3aa7566f3ca1b410f57aeceb01a4666c177640d1574fab0024a00fb",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {
                "5000/tcp": [
                    {
                        "HostIp": "0.0.0.0",
                        "HostPort": "5000"
                    },
                    {
                        "HostIp": "::",
                        "HostPort": "5000"
                    }
                ]
            },
            "SandboxKey": "/var/run/docker/netns/e7822213c3aa",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "51543b28d5845fc74095634960f21fd2a5ffed373695ace323ef6a0cd9080955",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:02",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "f06e508f708d688af9608cf4310b086b09a546aa28cd9ed6a80446b077e0888e",
                    "EndpointID": "51543b28d5845fc74095634960f21fd2a5ffed373695ace323ef6a0cd9080955",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                }
            }
        }
    }
]
```

在`NetworkSettings`可以看到一个`SandboxID`和`SandboxKey`。使用`lsns`查看网络命名空间，可以看到`SandboxKey`（`/var/run/docker/netns/e7822213c3aa`）。
