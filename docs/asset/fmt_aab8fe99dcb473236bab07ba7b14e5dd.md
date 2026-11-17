
# Linux 网络命名空间互通配置示例

## 配置红色命名空间

```
┌──[root@liruilongs.github.io]-[~]
└─$ip netns exec red bash
┌──[root@liruilongs.github.io]-[~]
└─$ip address add 192.168.20.2/24 dev eth0-r
┌──[root@liruilongs.github.io]-[~]
└─$ip r
# 对于目标网络 192.168.20.0/24 的数据包，它们将使用 eth0-r 设备进行本地通信。
192.168.20.0/24 dev eth0-r proto kernel scope link src 192.168.20.2
┌──[root@liruilongs.github.io]-[~]
└─$ip a
1: lo: &lt;LOOPBACK,UP,LOWER_UP&gt; mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
7: eth0-r@if6: &lt;BROADCAST,MULTICAST,UP,LOWER_UP&gt; mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether ca:b0:b2:80:25:43 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 192.168.20.2/24 scope global eth0-r
       valid_lft forever preferred_lft forever
    inet6 fe80::c8b0:b2ff:fe80:2543/64 scope link
       valid_lft forever preferred_lft forever
┌──[root@liruilongs.github.io]-[~]
└─$exit
exit
```

对另一个命名空间操作

```
┌──[root@liruilongs.github.io]-[~]
└─$ip netns exec green bash
┌──[root@liruilongs.github.io]-[~]
└─$ip address add 192.168.20.3/24 dev eth0-g
┌──[root@liruilongs.github.io]-[~]
└─$ip a
1: lo: &lt;LOOPBACK,UP,LOWER_UP&gt; mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
9: eth0-g@if8: &lt;BROADCAST,MULTICAST,UP,LOWER_UP&gt; mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether 36:5e:d9:8d:04:a8 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 192.168.20.3/24 scope global eth0-g
       valid_lft forever preferred_lft forever
    inet6 fe80::345e:d9ff:fe8d:4a8/64 scope link
       valid_lft forever preferred_lft forever
┌──[root@liruilongs.github.io]-[~]
└─$ip r
192.168.20.0/24 dev eth0-g proto kernel scope link src 192.168.20.3
┌──[root@liruilongs.github.io]-[~]
└─$exit
exit
```

## 两个命名空间之间的连通性测试

```
┌──[root@liruilongs.github.io]-[~]
└─$ip netns exec green bash
┌──[root@liruilongs.github.io]-[~]
└─$ping -c 2 192.168.20.2
PING 192.168.20.2 (192.168.20.2) 56(84) bytes of data.
64 bytes from 192.168.20.2: icmp_seq=1 ttl=64 time=0.252 ms
64 bytes from 192.168.20.2: icmp_seq=2 ttl=64 time=0.047 ms

--- 192.168.20.2 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1034ms
rtt min/avg/max/mdev = 0.047/0.149/0.252/0.102 ms

┌──[root@liruilongs.github.io]-[~]
└─$exit
exit
┌──[root@liruilongs.github.io]-[~]
└─$ip netns exec red bash
┌──[root@liruilongs.github.io]-[~]
└─$ping -c 2 192.168.20.3
PING 192.168.20.3 (192.168.20.3) 56(84) bytes of data.
64 bytes from 192.168.20.3: icmp_seq=1 ttl=64 time=0.241 ms
64 bytes from 192.168.20.3: icmp_seq=2 ttl=64 time=0.129 ms

--- 192.168.20.3 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1038ms
rtt min/avg/max/mdev = 0.129/0.185/0.241/0.056 ms
┌──[root@liruilongs.github.io]-[~]
└─$exit
exit
```

## 为根网络命名空间配置网桥和默认网关

将 IP 192.168.20.1/24 分配给根网络命名空间中的 `vnet-br0` 桥接口，以允许来自红色和绿色名称空间的外部通信，它将成为该网络的默认网关

```
┌──[root@liruilongs.github.io]-[~]
└─$ip address add 192.168.20.1/24 dev vnet-br0
```

将 192.168.20.1 配置为绿色和红色命名空间中的默认网关。将所有目标不在本地网络中的数据包发送到该网关进行进一步路由。

```
┌──[root@liruilongs.github.io]-[~]
└─$ip netns exec red bash
┌──[root@liruilongs.github.io]-[~]
└─$route add default gw 192.168.20.1
┌──[root@liruilongs.github.io]-[~]
└─$exit
exit
┌──[root@liruilongs.github.io]-[~]
└─$ip netns exec green bash
┌──[root@liruilongs.github.io]-[~]
└─$route add default gw 192.168.20.1
┌──[root@liruilongs.github.io]-[~]
└─$exit
exit
```

## 配置 NAT 规则

在 NAT 表中添加一个规则，将源 IP 地址为 192.168.20.0/24 的数据包进行源地址转换 (Source NAT)。

```
┌──[root@liruilongs.github.io]-[~]
└─$iptables -s 192.168.20.0/24 -t nat -A POSTROUTING -j MASQUERADE
```

执行该命令后，数据包从子网 192.168.20.0/24 发送到外部网络时，源 IP 地址将被替换为执行 NAT 的接口的 IP 地址。这通常用于实现网络地址转换 (NAT)，将内部网络的私有 IP 地址转换为公共 IP 地址，以便与外部网络进行通信。

## 根命名空间 Ping 测试

根命名空间做内网和公网地址 Ping 测

```
┌──[root@liruilongs.github.io]-[~]
└─$ping 192.169.26.149 -c 3
PING 192.169.26.149 (192.169.26.149) 56(84) bytes of data.
64 bytes from 192.169.26.149: icmp_seq=1 ttl=128 time=199 ms
64 bytes from 192.169.26.149: icmp_seq=2 ttl=128 time=199 ms
64 bytes from 192.169.26.149: icmp_seq=3 ttl=128 time=216 ms

--- 192.169.26.149 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2003ms
rtt min/avg/max/mdev = 199.020/204.755/215.909/7.888 ms
┌──[root@liruilongs.github.io]-[~]
└─$ping baidu.com -c 3
PING baidu.com (39.156.66.10) 56(84) bytes of data.
64 bytes from 39.156.66.10 (39.156.66.10): icmp_seq=1 ttl=128 time=11.9 ms
64 bytes from 39.156.66.10 (39.156.66.10): icmp_seq=2 ttl=128 time=11.9 ms
64 bytes from 39.156.66.10 (39.156.66.10): icmp_seq=3 ttl=128 time=12.1 ms

--- baidu.com ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2004ms
rtt min/avg/max/mdev = 11.919/11.999/12.142/0.100 ms
```

## 启用 IP 转发

在主机系统上启用 IPV4 转发以允许外部通信。执行该命令后，系统将开启 IP 转发功能，允许数据包在不同的网络接口之间进行转发。

```
┌──[root@liruilongs.github.io]-[~]
└─$sysctl -w net.ipv4.ip_forward=1
net.ipv4.ip_forward = 1
```

## 命名空间内网 Ping 测试

```
┌──[root@liruilongs.github.io]-[~]
└─$ip netns exec green ping baidu.com -c 3
PING baidu.com (110.242.68.66) 56(84) bytes of data.
64 bytes from 110.242.68.66 (110.242.68.66): icmp_seq=1 ttl=127 time=20.5 ms
64 bytes from 110.242.68.66 (110.242.68.66): icmp_seq=2 ttl=127 time=20.0 ms
64 bytes from 110.242.68.66 (110.242.68.66): icmp_seq=3 ttl=127 time=20.3 ms

--- baidu.com ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2003ms
rtt min/avg/max/mdev = 20.031/20.261/20.475/0.181 ms
┌──[root@liruilongs.github.io]-[~]
└─$ip netns exec red ping baidu.com -c 3
PING baidu.com (110.242.68.66) 56(84) bytes of data.
64 bytes from 110.242.68.66 (110.242.68.66): icmp_seq=1 ttl=127 time=20.2 ms
64 bytes from 110.242.68.66 (110.242.68.66): icmp_seq=2 ttl=127 time=20.3 ms
64 bytes from 110.242.68.66 (110.242.68.66): icmp_seq=3 ttl=127 time=20.1 ms

--- baidu.com ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2003ms
rtt min/avg/max/mdev = 20.085/20.197/20.278/0.082 ms
```

## 命名空间公网 Ping 测试

```
┌──[root@liruilongs.github.io]-[~]
└─$ip netns exec red ping 192.168.26.149 -c 3
PING 192.168.26.149 (192.168.26.149) 56(84) bytes of data.
64 bytes from 192.168.26.149: icmp_seq=1 ttl=64 time=0.241 ms
64 bytes from 192.168.26.149: icmp_seq=2 ttl=64 time=0.110 ms
64 bytes from 192.168.26.149: icmp_seq=3 ttl=64 time=0.075 ms

--- 192.168.26.149 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2074ms
rtt min/avg/max/mdev = 0.075/0.142/0.241/0.071 ms
┌──[root@liruilongs.github.io]-[~]
└─$ip netns exec green ping 192.168.26.149 -c 3
PING 192.168.26.149 (192.168.26.149) 56(84) bytes of data.
64 bytes from 192.168.26.149: icmp_seq=1 ttl=64 time=0.258 ms
64 bytes from 192.168.26.149: icmp_seq=2 ttl=64 time=0.097 ms
64 bytes from 192.168.26.149: icmp_seq=3 ttl=64 time=0.094 ms

--- 192.168.26.149 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2043ms
rtt min/avg/max/mdev = 0.094/0.149/0.258/0.076 ms
┌──[root@liruilongs.github.io]-[~]
└─$
```

![](img/522926f9857d57bf5850ca17630773e0.png)

在这里插入图片描述

到这里，我们实现和两个网络命名空间彼此通信，并且和根命名空间通信，同时可以和公网通信。

## 回顾

简单回顾一下我们干了什么：

+   在主机的根网络命名空间中创建一个 Linux 网桥，创建两个 Linux 网络命名空间
+   创建两个 veth pair，将其中一个端口连接到根命名空间中的网桥上，另一个端口放置在目标命名空间中。
+   在目标命名空间中配置 IP 地址，并将该端口启动起来。
+   在根命名空间中启用 IP 转发功能（通过设置 `net.ipv4.ip_forward=1`），分配 IP 地址，同时在命名空间配置默认网关。
+   配置 NAT 规则 SNAT，将目标命名空间中的流量转发的源 IP 地址转化为根命名空间中的 IP 地址。
+   目标命名空间中的流量将通过默认网关走网桥 IP 地址转发到根命名空间中，并通过根命名空间中的网络设备连接到互联网。

上面的 Demo 就是经典的容器组网模型，veth pair + bridge 的模式，用过 docker 的小伙伴会发现，默认情况下，安装完 docker 会自动创建一个桥接设备 `docker0`，用于 bridge 网络模式使用。
