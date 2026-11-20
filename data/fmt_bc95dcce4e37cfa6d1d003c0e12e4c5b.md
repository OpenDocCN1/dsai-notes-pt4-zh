
# Calico 跨主机容器网络通信原理

可以认为 Calico 把主机作为容器的默认网关使用，所有的报文发到主机，主机根据路由表进行转发。和经典的网络架构不同的是，Calico 并没有给默认网关配置一个 IP 地址，而是通过 ARP proxy 和修改容器路由表的机制实现。

主机上的 `cali7a4b00317e6` 网卡接收到报文之后，所有的报文会根据路由表转发，查看节点的路由表

```
root@all (1)[f:5]# route
192.168.26.103 | CHANGED | rc=0 &gt;&gt;
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
default         gateway         0.0.0.0         UG    0      0        0 ens32
10.244.31.64    vms106.liruilon 255.255.255.192 UG    0      0        0 tunl0
10.244.38.128   0.0.0.0         255.255.255.192 U     0      0        0 *
10.244.38.151   0.0.0.0         255.255.255.255 UH    0      0        0 cali39ccd735ea3
10.244.38.154   0.0.0.0         255.255.255.255 UH    0      0        0 calif39455d9c24
10.244.38.174   0.0.0.0         255.255.255.255 UH    0      0        0 cali7a4b00317e6
10.244.63.64    vms102.liruilon 255.255.255.192 UG    0      0        0 tunl0
10.244.169.64   vms105.liruilon 255.255.255.192 UG    0      0        0 tunl0
10.244.198.0    vms101.liruilon 255.255.255.192 UG    0      0        0 tunl0
10.244.239.128  vms100.liruilon 255.255.255.192 UG    0      0        0 tunl0
link-local      0.0.0.0         255.255.0.0     U     1002   0        0 ens32
172.17.0.0      0.0.0.0         255.255.0.0     U     0      0        0 docker0
192.168.26.0    0.0.0.0         255.255.255.0   U     0      0        0 ens32
```

访问 IP 为 `10.244.169.66`，可以看到匹配这一条路由

```
root@all (1)[f:5]# route | grep 169
192.168.26.103 | CHANGED | rc=0 &gt;&gt;
10.244.169.64   vms105.liruilon 255.255.255.192 UG    0      0        0 tunl0
root@all (1)[f:5]# ip route | grep 169
192.168.26.103 | CHANGED | rc=0 &gt;&gt;
10.244.169.64/26 via 192.168.26.105 dev tunl0 proto bird onlink
。。。。
```

`10.244.169.64/26` 表示一个 IP 地址范围，其中 `/26`(`255.255.255.192`) 表示子网掩码，确定了网络的大小。具体来说，这个 IP 地址范围包括从 `10.244.169.64` 到 `10.244.169.127` 的所有 IP 地址。

通过设备 `tunl0` 使用协议 `bird` 和 `onlink` 选项发送，下一跳 IP 地址为 `192.168.26.105`(`vms105.liruilon`...) 。

这里的 `bird` 和 `onlink` 是路由表中的两个选项:

+ `bird`：是一种路由协议，它可以帮助路由器动态地学习和适应网络拓扑结构的变化。Calico 使用 BGP 协议来实现网络功能，Bird 可以用于实现 BGP 路由器.

+ `onlink`：选项表示下一跳 IP 地址是直接可达的，也就是说，它是在同一子网内的。如果下一跳 IP 地址不在同一子网内，则需要使用网关来转发数据包。

然后我们来到下一跳 IP 地址对应的工作节点 `192.168.26.105`

```
┌──[root@vms100.liruilongs.github.io]-[~/ansible]
└─$kubectl get pods -n demo -o wide
NAME                               READY   STATUS    RESTARTS   AGE     IP              NODE                          NOMINATED NODE   READINESS GATES
demo-deployment-6cbdbd86d5-fbt9d   1/1     Running   0          4h36m   10.244.169.66   vms105.liruilongs.github.io   &lt;none&gt;           &lt;none&gt;
demo-deployment-6cbdbd86d5-nm467   1/1     Running   0          4h36m   10.244.38.174   vms103.liruilongs.github.io   &lt;none&gt;           &lt;none&gt;
┌──[root@vms100.liruilongs.github.io]-[~/ansible]
└─$
```

这个地址实际上是 目标 Pod 所在节点的 IP 地址，查看节点的路由信息。

```
┌──[root@vms100.liruilongs.github.io]-[~/ansible]
└─$ansible-console -i host.yaml --limit 192.168.26.105
Welcome to the ansible console.
Type help or ? to list commands.

root@all (1)[f:5]# ip route
192.168.26.105 | CHANGED | rc=0 &gt;&gt;
default via 192.168.26.2 dev ens32
10.244.31.64/26 via 192.168.26.106 dev tunl0 proto bird onlink
10.244.38.128/26 via 192.168.26.103 dev tunl0 proto bird onlink
10.244.63.64/26 via 192.168.26.102 dev tunl0 proto bird onlink
blackhole 10.244.169.64/26 proto bird
10.244.169.65 dev calid5e76ad523e scope link
10.244.169.66 dev cali39888f400bd scope link
10.244.169.70 dev cali0fdeca04347 scope link
10.244.169.73 dev cali7cf9eedbe64 scope link
10.244.169.77 dev calia011d753862 scope link
10.244.169.78 dev caliaa36e67b275 scope link
10.244.169.90 dev califc24aa4e3bd scope link
10.244.169.119 dev calibdca950861e scope link
10.244.169.120 dev cali40813694dd6 scope link
10.244.169.121 dev calie1fd05af50d scope link
10.244.198.0/26 via 192.168.26.101 dev tunl0 proto bird onlink
10.244.239.128/26 via 192.168.26.100 dev tunl0 proto bird onlink
169.254.0.0/16 dev ens32 scope link metric 1002
172.17.0.0/16 dev docker0 proto kernel scope link src 172.17.0.1
192.168.26.0/24 dev ens32 proto kernel scope link src 192.168.26.105
```

通过路由信息 会匹配 `10.244.169.66 dev cali39888f400bd scope link` 这个路由规则

```
root@all (1)[f:5]# ip route | grep 66
192.168.26.105 | CHANGED | rc=0 &gt;&gt;
10.244.169.66 dev cali39888f400bd scope link
root@all (1)[f:5]#
```

这个规则匹配的是一个 IP 地址，而不是网段。也就是说，主机上的每个容器都会有一个对应的路由表项。报文被发送到 `cali39888f400bd` 这个 `veth pair`

```
root@all (1)[f:5]# ip a | grep -A 4 cali39888f400bd
192.168.26.105 | CHANGED | rc=0 &gt;&gt;
16: cali39888f400bd@if4: &lt;BROADCAST,MULTICAST,UP,LOWER_UP&gt; mtu 1480 qdisc noqueue state UP
    link/ether ee:ee:ee:ee:ee:ee brd ff:ff:ff:ff:ff:ff link-netnsid 9
    inet6 fe80::ecee:eeff:feee:eeee/64 scope link
       valid_lft forever preferred_lft forever
root@all (1)[f:5]#
```

然后从 `cali39888f400bd` 一端发送给目标容器的 `eth0@if16` 。目标容器接收到报文之后，回复 ICMP 报文，应答报文原路返回。

```
┌──[root@vms100.liruilongs.github.io]-[~/ansible]
└─$kubectl exec -it demo-deployment-6cbdbd86d5-fbt9d -n demo -- bash
[root@demo-deployment-6cbdbd86d5-fbt9d /]# ip a
1: lo: &lt;LOOPBACK,UP,LOWER_UP&gt; mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: tunl0@NONE: &lt;NOARP&gt; mtu 1480 qdisc noop state DOWN group default qlen 1000
    link/ipip 0.0.0.0 brd 0.0.0.0
4: eth0@if16: &lt;BROADCAST,MULTICAST,UP,LOWER_UP&gt; mtu 1480 qdisc noqueue state UP group default
    link/ether 42:17:dd:38:6a:10 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 10.244.169.66/32 scope global eth0
       valid_lft forever preferred_lft forever
```

简单来回顾一下，跨主机的 Pod 如何通信？

本质是 Linux 中两个不同机器网络命名空间(network namespace)的通信，通过当前容器(网络命名空间)和宿主机内的一对虚拟网卡 `veth pair` ，把数据包发送到宿主节点上(这里涉及到 ARP 代理)，然后通过匹配路由表(这里是网段匹配)，下一跳到 目标 Pod 所在宿主节点，到达 目标 Pod 宿主节点 上之后，在通过 路由匹配(IP 匹配)到宿主节点上的虚拟网卡(和容器对应的一对虚拟网卡 `veth pair` )，然后从这个虚拟网卡到容器内部的虚拟网卡，实现请求，之后原路返回。

所以从网络命名空间角度理解，是容器 network namespace 到节点 root network namespace ，然后 节点 root network namespace 到 容器 network namespace

## network namespace 简单介绍

network namespace 在 Linux 内核 2.6 版本引入，作用是隔离 Linux 系统的网络设备，以及 IP 地址、端口、路由表、防火墙规则等网络资源。因此，每个网络 namespace 里都有自己的网络设备(如 IP 地址、路由表、端口范围、`/proc/net` 目录等)。

从网络的角度看，network namespace 使得容器非常有用，一个直观的例子就是：由于每个容器都有自己的(虚拟)网络设备，并且容器里的进程可以放心地绑定在端口上而不必担心端口冲突，这就使得在一个主机上同时运行多个监听 80 端口的 Web 服务器变为可能。

### 初识network namespace

network namespace 可以通过系统调用来创建，当前 network namespace 的增删改查功能已经集成到 Linux 的 `ip` 工具的 `netns` 子命令中。

创建一个名为 `netns_demo` 的 network namespace

```
┌──[root@liruilongs.github.io]-[~]
└─$ip netns add netns_demo
```

查看当前系统的 network namespace

```
┌──[root@liruilongs.github.io]-[~]
└─$ip netns list 
netns_demo
```

创建了一个 network namespace 时，系统会在 `/var/run/netns` 路径下面生成一个挂载点

```
┌──[root@liruilongs.github.io]-[~]
└─$ls /var/run/netns/netns_demo 
/var/run/netns/netns_demo
```

挂载点的作用

+ 方便对 namespace 的管理
+ 使 namespace 即使没有进程运行也能继续存在

一个 network namespace 被创建出来后，可以使用 `ip netns exec` 命令进入，做一些网络 查询/配置 的工作。

查看网络命名空间信息,没有任何配置，因此只有一块系统默认的本地回环设备 `lo`。

```
┌──[root@liruilongs.github.io]-[~]
└─$ip netns exec netns_demo ip a
1: lo: &lt;LOOPBACK&gt; mtu 65536 qdisc noop state DOWN qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
```

对于删除 network namespace，可以通过以下命令实现：

```
┌──[root@liruilongs.github.io]-[~]
└─$ip netns delete netns_demo
┌──[root@liruilongs.github.io]-[~]
└─$ip netns list
┌──[root@liruilongs.github.io]-[~]
└─$
```

注意，上面这条命令实际上并没有删除 `netns_demo` 这个 network namespace，它只是移除了这个 network namespace 对应的挂载点。只要里面还有进程运行着，network namespace 便会一直存在。

### 配置 network namespace

当尝试访问创建的 网络 命名空间的本地回环地址时，网络是不通的

```
┌──[root@liruilongs.github.io]-[~]
└─$ip netns exec netns_demo ip a
1: lo: &lt;LOOPBACK&gt; mtu 65536 qdisc noop state DOWN qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
```

这是因为本地回环地址对应的网卡设备连接默认是禁用的，需要下面的方式激活。

```
┌──[root@liruilongs.github.io]-[~]
└─$ip netns exec netns_demo ip link set dev lo up
```

激活之后，查看网络命名空间网卡的详细信息
