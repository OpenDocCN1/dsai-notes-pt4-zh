
# K8s 集群高可用 master 节点 ETCD 挂掉如何恢复?_etcd挂了

> 来源：[https://geekbang.feishu.cn/docx/KhPndxUKxoqFFvxJtUncQTkbnxd](https://geekbang.feishu.cn/docx/KhPndxUKxoqFFvxJtUncQTkbnxd)

> 👨🏻 作者：山河已无恙

> 📃 作者介绍：SRE，云原生相关技术干货分享，RHCA/RHCE/RHCAS、CKA 认证，CSDN/51CTO博客专家，InfoQ写作平台/Alibaba Cloud社区签约作者、Huawei Cloud社区云享专家、Tencent Cloud社区内容共创官

> ⏰ 收录时间：2024-05-27 11:20:58 (UTC+8)

> 🔗 原文链接：https://liruilong.blog.csdn.net/article/details/136773827

![](img/6e876d29a561187963014d8a1da32ea9.png)

关注作者，获取更多AI知识

## 写在前面

---

+ 很常见的集群运维场景，整理分享

+ 博文内容为 K8s 集群高可用 master 节点故障如何恢复的过程

+ 理解不足小伙伴帮忙指正

**不**必太纠结于当下，也不必太忧虑未来，当你经历过一些事情的时候，眼前的风景已经和从前不一样了。——村上春树

---

### 遇到了什么问题

今天做实验发现，[集群](https://so.csdn.net/so/search?q=%E9%9B%86%E7%BE%A4&spm=1001.2101.3001.7020)其中一个 master 节点上的 `etcd` 和 `apiserver` 都挂掉了

```
┌──[root@vms100.liruilongs.github.io]-[~]
└─$kubectl get nodes
NAME                          STATUS   ROLES           AGE    VERSION
vms100.liruilongs.github.io   Ready    control-plane   415d   v1.25.1
vms101.liruilongs.github.io   Ready    control-plane   415d   v1.25.1
vms102.liruilongs.github.io   Ready    control-plane   415d   v1.25.1
vms103.liruilongs.github.io   Ready    &lt;none&gt;          415d   v1.25.1
vms105.liruilongs.github.io   Ready    &lt;none&gt;          415d   v1.25.1
vms106.liruilongs.github.io   Ready    &lt;none&gt;          415d   v1.25.1
┌──[root@vms100.liruilongs.github.io]-[~]
└─$
```

`vms100.liruilongs.github.io` 这个节点上的 `apiserver` 和 `etcd`

```
┌──[root@vms100.liruilongs.github.io]-[~]
└─$kubectl get pod -A -o wide | grep apiserver
kube-system          kube-apiserver-vms100.liruilongs.github.io            0/1     CrashLoopBackOff   1448 (3m23s ago)   415d   192.168.26.100   vms100.liruilongs.github.io   &lt;none&gt;           &lt;none&gt;
kube-system          kube-apiserver-vms101.liruilongs.github.io            1/1     Running            272 (3h18m ago)    415d   192.168.26.101   vms101.liruilongs.github.io   &lt;none&gt;           &lt;none&gt;
kube-system          kube-apiserver-vms102.liruilongs.github.io            1/1     Running            246 (3h18m ago)    415d   192.168.26.102   vms102.liruilongs.github.io   &lt;none&gt;           &lt;none&gt;
┌──[root@vms100.liruilongs.github.io]-[~]
└─$kubectl get pod -A -o wide | grep etcd
kube-system          etcd-vms100.liruilongs.github.io                      0/1     CrashLoopBackOff   1244 (3m6s ago)    415d   192.168.26.100   vms100.liruilongs.github.io   &lt;none&gt;           &lt;none&gt;
kube-system          etcd-vms101.liruilongs.github.io                      1/1     Running            167 (3h18m ago)    415d   192.168.26.101   vms101.liruilongs.github.io   &lt;none&gt;           &lt;none&gt;
kube-system          etcd-vms102.liruilongs.github.io                      1/1     Running            173 (3h18m ago)    415d   192.168.26.102   vms102.liruilongs.github.io   &lt;none&gt;           &lt;none&gt;
┌──[root@vms100.liruilongs.github.io]-[~]
└─$
```

查看 `keepalived` 对应的静态Pod运行正常

```
┌──[root@vms100.liruilongs.github.io]-[~]
└─$kubectl get pod -A -o wide | grep keep
kube-system          keepalived-vms100.liruilongs.github.io                1/1     Running            63 (3h50m ago)    415d   192.168.26.100   vms100.liruilongs.github.io   &lt;none&gt;           &lt;none&gt;
kube-system          keepalived-vms101.liruilongs.github.io                1/1     Running            54 (3h51m ago)    415d   192.168.26.101   vms101.liruilongs.github.io   &lt;none&gt;           &lt;none&gt;
kube-system          keepalived-vms102.liruilongs.github.io                1/1     Running            60 (3h51m ago)    415d   192.168.26.102   vms102.liruilongs.github.io   &lt;none&gt;           &lt;none&gt;
┌──[root@vms100.liruilongs.github.io]-[~]
└─$
```

所以可能是 `etcd` 数据不同步，或者什么原因导致 `etcd` 挂掉了。因为每个 master 节点的 `apiserver` 只和本节点的 `etcd` 进行通信（每个 `etcd` 的写请求会转发到 `etcd` 的领导节点），`etcd` 挂掉，`apiserver` 无法提供能力，所以也会挂掉。

通过 `etcdctl` 可以发现 `vms100.liruilongs.github.io` 上的 `etcd` 彻底死掉了

```
┌──[root@vms100.liruilongs.github.io]-[~]
└─$ETCDCTL_API=3 etcdctl  --endpoints https://127.0.0.1:2379 \
 --cert=&quot;/etc/kubernetes/pki/etcd/server.crt&quot; \
 --key=&quot;/etc/kubernetes/pki/etcd/server.key&quot;  \
 --cacert=&quot;/etc/kubernetes/pki/etcd/ca.crt&quot; \
 member list -w table
Error: dial tcp 127.0.0.1:2379: connect: connection refused
```

### 如何排查

这里我们换一个 `etcd` 节点执行命令

查看 `etcd` 集群成员

```
┌──[root@vms100.liruilongs.github.io]-[~]
└─$ssh vms101.liruilongs.github.io
Last login: Sat Mar  2 09:52:01 2024 from 192.168.26.100
┌──[root@vms101.liruilongs.github.io]-[~]
└─$ETCDCTL_API=3 etcdctl  --endpoints https://127.0.0.1:2379 \
 --cert=&quot;/etc/kubernetes/pki/etcd/server.crt&quot;  \
 --key=&quot;/etc/kubernetes/pki/etcd/server.key&quot; \
  --cacert=&quot;/etc/kubernetes/pki/etcd/ca.crt&quot; \
  member list -w table
+------------------+---------+-----------------------------+-----------------------------+-----------------------------+
|        ID        | STATUS  |            NAME             |         PEER ADDRS          |        CLIENT ADDRS         |
+------------------+---------+-----------------------------+-----------------------------+-----------------------------+
|  ee392e5273e89e2 | started | vms100.liruilongs.github.io | https://192.168.26.100:2380 | https://192.168.26.100:2379 |
| 70059e836d19883d | started | vms101.liruilongs.github.io | https://192.168.26.101:2380 | https://192.168.26.101:2379 |
| b8cb9f66c2e63b91 | started | vms102.liruilongs.github.io | https://192.168.26.102:2380 | https://192.168.26.102:2379 |
+------------------+---------+-----------------------------+-----------------------------+-----------------------------+
```

查看节点状态

```
┌──[root@vms101.liruilongs.github.io]-[~]
└─$ETCDCTL_API=3 etcdctl  --endpoints https://127.0.0.1:2379 \
  --cert=&quot;/etc/kubernetes/pki/etcd/server.crt&quot; \
  --key=&quot;/etc/kubernetes/pki/etcd/server.key&quot;  \
  --cacert=&quot;/etc/kubernetes/pki/etcd/ca.crt&quot; \
   endpoint status --cluster  -w table
Failed to get the status of endpoint https://192.168.26.100:2379 (context deadline exceeded)
+-----------------------------+------------------+---------+---------+-----------+-----------+------------+
|          ENDPOINT           |        ID        | VERSION | DB SIZE | IS LEADER | RAFT TERM | RAFT INDEX |
+-----------------------------+------------------+---------+---------+-----------+-----------+------------+
| https://192.168.26.101:2379 | 70059e836d19883d |   3.5.4 |   88 MB |     false |       603 |   22208417 |
| https://192.168.26.102:2379 | b8cb9f66c2e63b91 |   3.5.4 |   88 MB |      true |       603 |   22208417 |
+-----------------------------+------------------+---------+---------+-----------+-----------+------------+
```

确定 `ETCD` 节点故障

```
┌──[root@vms101.liruilongs.github.io]-[~]
└─$ETCDCTL_API=3 etcdctl  --endpoints https://127.0.0.1:2379  \
 --cert=&quot;/etc/kubernetes/pki/etcd/server.crt&quot;  \
 --key=&quot;/etc/kubernetes/pki/etcd/server.key&quot;  \
 --cacert=&quot;/etc/kubernetes/pki/etcd/ca.crt&quot; \
 endpoint  health --cluster  -w table
https://192.168.26.101:2379 is healthy: successfully committed proposal: took = 3.753357ms
https://192.168.26.102:2379 is healthy: successfully committed proposal: took = 2.989943ms
https://192.168.26.100:2379 is unhealthy: failed to connect: dial tcp 192.168.26.100:2379: connect: connection refused
Error: unhealthy cluster
```

查看 `etcd` 的容器日志



# `etcd` 节点故障处理

## 问题现象

```
┌──[root@vms100.liruilongs.github.io]-[~]
└─$docker ps -a | grep etcd
0f2f98ebf8c3   a8a176a5d5d6                                        "etcd --advertise-cl…"   4 minutes ago   Exited (2) 4 minutes ago                  k8s_etcd_etcd-vms100.liruilongs.github.io_kube-system_e8c17bb99f9bd8119cdd769556041e18_1252
a4b39d16a753   registry.aliyuncs.com/google_containers/pause:3.8   "/pause"                 4 hours ago     Up 4 hours                                k8s_POD_etcd-vms100.liruilongs.github.io_kube-system_e8c17bb99f9bd8119cdd769556041e18_54
┌──[root@vms100.liruilongs.github.io]-[~]
└─$docker logs 0f2f98ebf8c3
{"level":"info","ts":"2024-03-16T14:46:54.644Z","caller":"etcdmain/etcd.go:73","msg":"Running: ","args":["etcd","--advertise-client-urls=https://192.168.26.100:2379","--cert-file=/etc/kubernetes/pki/etcd/server.crt","--client-cert-auth=true","--data-dir=/var/lib/etcd","--experimental-initial-corrupt-check=true","--experimental-watch-progress-notify-interval=5s","--initial-advertise-peer-urls=https://192.168.26.100:2380","--initial-cluster=vms100.liruilongs.github.io=https://192.168.26.100:2380","--key-file=/etc/kubernetes/pki/etcd/server.key","--listen-client-urls=https://127.0.0.1:2379,https://192.168.26.100:2379","--listen-metrics-urls=http://127.0.0.1:2381","--listen-peer-urls=https://192.168.26.100:2380","--name=vms100.liruilongs.github.io","--peer-cert-file=/etc/kubernetes/pki/etcd/peer.crt","--peer-client-cert-auth=true","--peer-key-file=/etc/kubernetes/pki/etcd/peer.key","--peer-trusted-ca-file=/etc/kubernetes/pki/etcd/ca.crt","--snapshot-count=10000","--trusted-ca-file=/etc/kubernetes/pki/etcd/ca.crt"]}
{"level":"info","ts":"2024-03-16T14:46:54.645Z","caller":"etcdmain/etcd.go:116","msg":"server has been already initialized","data-dir":"/var/lib/etcd","dir-type":"member"}
{"level":"info","ts":"2024-03-16T14:46:54.645Z","caller":"embed/etcd.go:131","msg":"configuring peer listeners","listen-peer-urls":["https://192.168.26.100:2380"]}
{"level":"info","ts":"2024-03-16T14:46:54.645Z","caller":"embed/etcd.go:479","msg":"starting with peer TLS","tls-info":"cert = /etc/kubernetes/pki/etcd/peer.crt, key = /etc/kubernetes/pki/etcd/peer.key, client-cert=, client-key=, trusted-ca = /etc/kubernetes/pki/etcd/ca.crt, client-cert-auth = true, crl-file = ","cipher-suites":[]}
{"level":"info","ts":"2024-03-16T14:46:54.645Z","caller":"embed/etcd.go:139","msg":"configuring client listeners","listen-client-urls":["https://127.0.0.1:2379","https://192.168.26.100:2379"]}
{"level":"info","ts":"2024-03-16T14:46:54.645Z","caller":"embed/etcd.go:308","msg":"starting an etcd server","etcd-version":"3.5.4","git-sha":"08407ff76","go-version":"go1.16.15","go-os":"linux","go-arch":"amd64","max-cpu-set":4,"max-cpu-available":4,"member-initialized":true,"name":"vms100.liruilongs.github.io","data-dir":"/var/lib/etcd","wal-dir":"","wal-dir-dedicated":"","member-dir":"/var/lib/etcd/member","force-new-cluster":false,"heartbeat-interval":"100ms","election-timeout":"1s","initial-election-tick-advance":true,"snapshot-count":10000,"snapshot-catchup-entries":5000,"initial-advertise-peer-urls":["https://192.168.26.100:2380"],"listen-peer-urls":["https://192.168.26.100:2380"],"advertise-client-urls":["https://192.168.26.100:2379"],"listen-client-urls":["https://127.0.0.1:2379","https://192.168.26.100:2379"],"listen-metrics-urls":["http://127.0.0.1:2381"],"cors":["*"],"host-whitelist":["*"],"initial-cluster":"","initial-cluster-state":"new","initial-cluster-token":"","quota-size-bytes":2147483648,"pre-vote":true,"initial-corrupt-check":true,"corrupt-check-time-interval":"0s","auto-compaction-mode":"periodic","auto-compaction-retention":"0s","auto-compaction-interval":"0s","discovery-url":"","discovery-proxy":"","downgrade-check-interval":"5s"}
panic: freepages: failed to get all reachable pages (page 7744: multiple references)

goroutine 109 [running]:
go.etcd.io/bbolt.(*DB).freepages.func2(0xc00009c480)
        /go/pkg/mod/go.etcd.io/bbolt@v1.3.6/db.go:1056 +0xe9
created by go.etcd.io/bbolt.(*DB).freepages
        /go/pkg/mod/go.etcd.io/bbolt@v1.3.6/db.go:1054 +0x1cd
┌──[root@vms100.liruilongs.github.io]-[~]
└─$
```

## 如何解决

这里最快的办法是重新同步一下这个节点的数据，即把这个故障节点移出集群，清理完故障节点旧数据在重新添加，操作步骤

### 操作步骤

+ 清理数据目录，移动静态`Pod`的`yaml`文件：停止故障节点服务，然后删除`etcd`数据目录。

+ 移除故障节点：使用`member remove`命令剔除错误节点，可以在健康的节点执行命令。

+ 添加节点：使用`member add`命令添加故障节点。

+ 重新启动：移动故障节点`yaml`文件，进行启动

> 注：静态`Pod`通过加载指定目录的`yaml`文件来调度，`kubelet`会定时扫描，删除移动`yaml`文件，静态`Pod`会自动停止，同理。添加`yaml`文件会自动创建静态`Pod`

### 移动静态`Pod`的`yaml`文件

```
┌──[root@vms100.liruilongs.github.io]-[~]
└─$mv  /etc/kubernetes/manifests/{etcd.yaml,kube-apiserver.yaml}  /tmp/
```

### 删除`etcd`数据目录

```
┌──[root@vms100.liruilongs.github.io]-[~]
└─$rm -rf /var/lib/etcd/*
```

### 确认节点的`etcd`和apiservier都已经停止

```
┌──[root@vms100.liruilongs.github.io]-[~]
└─$kubectl get pod -A -o wide | grep apiserver
kube-system          kube-apiserver-vms101.liruilongs.github.io            1/1     Running   272 (4h15m ago)   415d   192.168.26.101   vms101.liruilongs.github.io   <none>           <none>
kube-system          kube-apiserver-vms102.liruilongs.github.io            1/1     Running   246 (4h15m ago)   415d   192.168.26.102   vms102.liruilongs.github.io   <none>           <none>
┌──[root@vms100.liruilongs.github.io]-[~]
└─$kubectl get pod -A -o wide | grep etcd
kube-system          etcd-vms101.liruilongs.github.io                      1/1     Running   167 (4h15m ago)   415d   192.168.26.101   vms101.liruilongs.github.io   <none>           <none>
kube-system          etcd-vms102.liruilongs.github.io                      1/1     Running   173 (4h15m ago)   415d   192.168.26.102   vms102.liruilongs.github.io   <none>           <none>
┌──[root@vms100.liruilongs.github.io]-[~]
└─$
```

获取故障节点ID，下面的操作我们在健康的`etcd`节点执行，或者可以修改`--endpoints`



# etcd 集群节点故障恢复

## 查看当前集群成员

```
┌──[root@vms101.liruilongs.github.io]-[~]
└─$ETCDCTL_API=3 etcdctl --endpoints https://192.168.26.101:2379 --cert="/etc/kubernetes/pki/etcd/server.crt" --key="/etc/kubernetes/pki/etcd/server.key" --cacert="/etc/kubernetes/pki/etcd/ca.crt" member list -w table
+------------------+---------+-----------------------------+-----------------------------+-----------------------------+
|        ID        | STATUS  |            NAME             |         PEER ADDRS          |        CLIENT ADDRS         |
+------------------+---------+-----------------------------+-----------------------------+-----------------------------+
|  ee392e5273e89e2 | started | vms100.liruilongs.github.io | https://192.168.26.100:2380 | https://192.168.26.100:2379 |
| 70059e836d19883d | started | vms101.liruilongs.github.io | https://192.168.26.101:2380 | https://192.168.26.101:2379 |
| b8cb9f66c2e63b91 | started | vms102.liruilongs.github.io | https://192.168.26.102:2380 | https://192.168.26.102:2379 |
+------------------+---------+-----------------------------+-----------------------------+-----------------------------+
```

## 移除故障节点

```
┌──[root@vms101.liruilongs.github.io]-[~]
└─$ETCDCTL_API=3 etcdctl --endpoints https://127.0.0.1:2379 --cert="/etc/kubernetes/pki/etcd/server.crt" --key="/etc/kubernetes/pki/etcd/server.key" --cacert="/etc/kubernetes/pki/etcd/ca.crt" member remove ee392e5273e89e2
Member  ee392e5273e89e2 removed from cluster 4816f346663d82a7
```

## 重新添加节点

```
┌──[root@vms101.liruilongs.github.io]-[~]
└─$ETCDCTL_API=3 etcdctl --endpoints https://127.0.0.1:2379 --cert="/etc/kubernetes/pki/etcd/server.crt" --key="/etc/kubernetes/pki/etcd/server.key" --cacert="/etc/kubernetes/pki/etcd/ca.crt" member add vms100.liruilongs.github.io --peer-urls=https://192.168.26.100:2380
Member 456f71fdc1ad9917 added to cluster 4816f346663d82a7

ETCD_NAME="vms100.liruilongs.github.io"
ETCD_INITIAL_CLUSTER="vms100.liruilongs.github.io=https://192.168.26.100:2380,vms101.liruilongs.github.io=https://192.168.26.101:2380,vms102.liruilongs.github.io=https://192.168.26.102:2380"
ETCD_INITIAL_ADVERTISE_PEER_URLS="https://192.168.26.100:2380"
ETCD_INITIAL_CLUSTER_STATE="existing"
```

## 回到 100 节点机器，移动 Yaml 文件，恢复节点

```
┌──[root@vms100.liruilongs.github.io]-[~]
└─$mv /tmp/{etcd.yaml,kube-apiserver.yaml} /etc/kubernetes/manifests/
```

## 确认 Pod 状态

```
┌──[root@vms100.liruilongs.github.io]-[~]
└─$kubectl get pod -A -o wide | grep etcd
kube-system          etcd-vms100.liruilongs.github.io                      1/1     Running   0                 16s    192.168.26.100   vms100.liruilongs.github.io   <none>           <none>
kube-system          etcd-vms101.liruilongs.github.io                      1/1     Running   167 (4h32m ago)   415d   192.168.26.101   vms101.liruilongs.github.io   <none>           <none>
kube-system          etcd-vms102.liruilongs.github.io                      1/1     Running   173 (4h32m ago)   415d   192.168.26.102   vms102.liruilongs.github.io   <none>           <none>
┌──[root@vms100.liruilongs.github.io]-[~]
└─$kubectl get pod -A -o wide | grep apiserver
kube-system          kube-apiserver-vms100.liruilongs.github.io            1/1     Running   0                 24s    192.168.26.100   vms100.liruilongs.github.io   <none>           <none>
kube-system          kube-apiserver-vms101.liruilongs.github.io            1/1     Running   272 (4h32m ago)   415d   192.168.26.101   vms101.liruilongs.github.io   <none>           <none>
kube-system          kube-apiserver-vms102.liruilongs.github.io            1/1     Running   246 (4h32m ago)   415d   192.168.26.102   vms102.liruilongs.github.io   <none>           <none>
┌──[root@vms100.liruilongs.github.io]-[~]
└─$
```

## 查看 `etcd` 集群状态

```
┌──[root@vms101.liruilongs.github.io]-[~]
└─$ETCDCTL_API=3 etcdctl --endpoints https://127.0.0.1:2379 --cert="/etc/kubernetes/pki/etcd/server.crt" --key="/etc/kubernetes/pki/etcd/server.key" --cacert="/etc/kubernetes/pki/etcd/ca.crt" member list -w table
+------------------+-----------+-----------------------------+-----------------------------+-----------------------------+
|        ID        |  STATUS   |            NAME             |         PEER ADDRS          |        CLIENT ADDRS         |
+------------------+-----------+-----------------------------+-----------------------------+-----------------------------+
| 54952f3b494c0286 | unstarted |                             | https://192.168.26.100:2380 |                             |
| 70059e836d19883d |   started | vms101.liruilongs.github.io | https://192.168.26.101:2380 | https://192.168.26.101:2379 |
| b8cb9f66c2e63b91 |   started | vms102.liruilongs.github.io | https://192.168.26.102:2380 | https://192.168.26.102:2379 |
+------------------+-----------+-----------------------------+-----------------------------+-----------------------------+
```

这里我们发现 新添加的节点状态不正常，一直是 `unstarted`

我们在 故障节点执行 `etcd` 命令。发现故障节点并没有添加到集群，而是作为一个单节点运行。

```
┌──[root@vms100.liruilongs.github.io]-[~]
└─$ETCDCTL_API=3 etcdctl --endpoints https://127.0.0.1:2379 --cert="/etc/kubernetes/pki/etcd/server.crt" --key="/etc/kubernetes/pki/etcd/server.key" --cacert="/etc/kubernetes/pki/etcd/ca.crt" member list -w table
+-----------------+---------+-----------------------------+-----------------------------+-----------------------------+
|       ID        | STATUS  |            NAME             |         PEER ADDRS          |        CLIENT ADDRS         |
+-----------------+---------+-----------------------------+-----------------------------+-----------------------------+
| ee392e5273e89e2 | started | vms100.liruilongs.github.io | https://192.168.26.100:2380 | https://192.168.26.100:2379 |
+-----------------+---------+-----------------------------+-----------------------------+-----------------------------+
┌──[root@vms100.liruilongs.github.io]-[~]
└─$ETCDCTL_API=3 etcdctl --endpoints https://127.0.0.1:2379 --cert="/etc/kubernetes/pki/etcd/server.crt" --key="/etc/kubernetes/pki/etcd/server.key" --cacert="/etc/kubernetes/pki/etcd/ca.crt" endpoint status --cluster -w table
+-----------------------------+-----------------+---------+---------+-----------+-----------+------------+
|          ENDPOINT           |       ID        | VERSION | DB SIZE | IS LEADER | RAFT TERM | RAFT INDEX |
+-----------------------------+-----------------+---------+---------+-----------+-----------+------------+
| https://192.168.26.100:2379 | ee392e5273e89e2 |   3.5.4 |  815 kB |      true |         2 |       2261 |
+-----------------------------+-----------------+---------+---------+-----------+-----------+------------+
┌──[root@vms100.liruilongs.github.io]-[~]
└─$
```

也没有同步 当前集群的数据

```
┌──[root@vms100.liruilongs.github.io]-[~]
└─$kubectl get pod -A -o wide --server=https://vms100.liruilongs.github.io:6443
No resources found
```

遇到这种[情况，大部分](https://so.csdn.net/so/search?q=%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6&spm=1001.2101.3001.7020)原因是 某个节点的 `etcd` 配置文件 的问题，我的这个问题是 故障节点的 `etcd` 配置文件，没有集群信息相关配置 ，所以这里把集群相关配置写入配置

## 原本的配置文件



# etcd 集群故障排查记录

## 查看 etcd 配置文件

```
┌──[root@vms100.liruilongs.github.io]-[~]
└─$cat /etc/kubernetes/manifests/etcd.yaml
apiVersion: v1
kind: Pod
metadata:
  annotations:
    kubeadm.kubernetes.io/etcd.advertise-client-urls: https://192.168.26.100:2379
  creationTimestamp: null
  labels:
    component: etcd
    tier: control-plane
  name: etcd
  namespace: kube-system
spec:
  containers:
  - command:
    - etcd
    - --advertise-client-urls=https://192.168.26.100:2379
    - --cert-file=/etc/kubernetes/pki/etcd/server.crt
    - --client-cert-auth=true
    - --data-dir=/var/lib/etcd
    - --experimental-initial-corrupt-check=true
    - --experimental-watch-progress-notify-interval=5s
    - --initial-advertise-peer-urls=https://192.168.26.100:2380
    - --initial-cluster=vms100.liruilongs.github.io=https://192.168.26.100:2380
    - --key-file=/etc/kubernetes/pki/etcd/server.key
    - --listen-client-urls=https://127.0.0.1:2379,https://192.168.26.100:2379
    - --listen-metrics-urls=http://127.0.0.1:2381
    - --listen-peer-urls=https://192.168.26.100:2380
    - --name=vms100.liruilongs.github.io
    - --peer-cert-file=/etc/kubernetes/pki/etcd/peer.crt
    - --peer-client-cert-auth=true
    - --peer-key-file=/etc/kubernetes/pki/etcd/peer.key
    - --peer-trusted-ca-file=/etc/kubernetes/pki/etcd/ca.crt
    - --snapshot-count=10000
    - --trusted-ca-file=/etc/kubernetes/pki/etcd/ca.crt
    image: registry.aliyuncs.com/google_containers/etcd:3.5.4-0
。。。。。。。。。。。。。。。。。
```

## 添加缺失的集群配置

集群信息不全的，添加后的配置文件

```
┌──[root@vms100.liruilongs.github.io]-[~]
└─$cat /etc/kubernetes/manifests/etcd.yaml
apiVersion: v1
kind: Pod
metadata:
  annotations:
    kubeadm.kubernetes.io/etcd.advertise-client-urls: https://192.168.26.100:2379
  creationTimestamp: null
  labels:
    component: etcd
    tier: control-plane
  name: etcd
  namespace: kube-system
spec:
  containers:
  - command:
    - etcd
    - --advertise-client-urls=https://192.168.26.100:2379
    - --cert-file=/etc/kubernetes/pki/etcd/server.crt
    - --client-cert-auth=true
    - --data-dir=/var/lib/etcd
    - --experimental-initial-corrupt-check=true
    - --experimental-watch-progress-notify-interval=5s
    - --initial-advertise-peer-urls=https://192.168.26.100:2380
    - --initial-cluster=vms100.liruilongs.github.io=https://192.168.26.100:2380,vms101.liruilongs.github.io=https://192.168.26.101:2380
    - --initial-cluster-state=existing
    - --key-file=/etc/kubernetes/pki/etcd/server.key
    - --listen-client-urls=https://127.0.0.1:2379,https://192.168.26.100:2379
    - --listen-metrics-urls=http://127.0.0.1:2381
    - --listen-peer-urls=https://192.168.26.100:2380
    - --name=vms100.liruilongs.github.io
    - --peer-cert-file=/etc/kubernetes/pki/etcd/peer.crt
    - --peer-client-cert-auth=true
    - --peer-key-file=/etc/kubernetes/pki/etcd/peer.key
    - --peer-trusted-ca-file=/etc/kubernetes/pki/etcd/ca.crt
    - --snapshot-count=10000
    - --trusted-ca-file=/etc/kubernetes/pki/etcd/ca.crt
```

## 恢复节点并检查状态

然后我们以上面相同的方式从新恢复一次，发现节点直接没有起来

```
┌──[root@vms100.liruilongs.github.io]-[~]
└─$kubectl get pod -A -o wide | grep apiserver
kube-system          kube-apiserver-vms100.liruilongs.github.io            0/1     CrashLoopBackOff   1 (18s ago)       39s    192.168.26.100   vms100.liruilongs.github.io   &lt;none&gt;           &lt;none&gt;
kube-system          kube-apiserver-vms101.liruilongs.github.io            1/1     Running            272 (5h29m ago)   415d   192.168.26.101   vms101.liruilongs.github.io   &lt;none&gt;           &lt;none&gt;
kube-system          kube-apiserver-vms102.liruilongs.github.io            1/1     Running            246 (5h29m ago)   415d   192.168.26.102   vms102.liruilongs.github.io   &lt;none&gt;           &lt;none&gt;
┌──[root@vms100.liruilongs.github.io]-[~]
└─$kubectl get pod -A -o wide | grep etcd
kube-system          etcd-vms100.liruilongs.github.io                      0/1     CrashLoopBackOff   3 (21s ago)       53s    192.168.26.100   vms100.liruilongs.github.io   &lt;none&gt;           &lt;none&gt;
kube-system          etcd-vms101.liruilongs.github.io                      1/1     Running            167 (5h29m ago)   415d   192.168.26.101   vms101.liruilongs.github.io   &lt;none&gt;           &lt;none&gt;
kube-system          etcd-vms102.liruilongs.github.io                      1/1     Running            173 (5h29m ago)   415d   192.168.26.102   vms102.liruilongs.github.io   &lt;none&gt;           &lt;none&gt;
```

## 查看日志

```
┌──[root@vms100.liruilongs.github.io]-[~]
└─$kubectl logs etcd-vms100.liruilongs.github.io -n kube-system
.............................
{&quot;level&quot;:&quot;fatal&quot;,&quot;ts&quot;:&quot;2024-03-16T16:25:19.981Z&quot;,&quot;caller&quot;:&quot;etcdmain/etcd.go:204&quot;,&quot;msg&quot;:&quot;discovery failed&quot;,&quot;error&quot;:&quot;error validating peerURLs {ClusterID:4816f346663d82a7 Members:[&amp;{ID:b8cb9f66c2e63b91 RaftAttributes:{PeerURLs:[https://192.168.26.102:2380] IsLearner:false} Attributes:{Name:vms102.liruilongs.github.io ClientURLs:[https://192.168.26.102:2379]}} &amp;{ID:3fbbbed942c51f7b RaftAttributes:{PeerURLs:[https://192.168.26.100:2380] IsLearner:false} Attributes:{Name: ClientURLs:[]}} &amp;{ID:70059e836d19883d RaftAttributes:{PeerURLs:[https://192.168.26.101:2380] IsLearner:false} Attributes:{Name:vms101.liruilongs.github.io ClientURLs:[https://192.168.26.101:2379]}}] RemovedMemberIDs:[]}: member count is unequal&quot;,&quot;stacktrace&quot;:&quot;go.etcd.io/etcd/server/v3/etcdmain.startEtcdOrProxyV2\n\t/go/src/go.etcd.io/etcd/release/etcd/server/etcdmain/etcd.go:204\ngo.etcd.io/etcd/server/v3/etcdmain.Main\n\t/go/src/go.etcd.io/etcd/release/etcd/server/etcdmain/main.go:40\nmain.main\n\t/go/src/go.etcd.io/etcd/release/etcd/server/main.go:32\nruntime.main\n\t/go/gos/go1.16.15/src/runtime/proc.go:225&quot;}
```

## 日志分析

根据日志信息，可以看到有用的信息 `RemovedMemberIDs:[]}: member count is unequal` ,成员数量不相等,在分析日志



# etcd 节点启动故障排查与修复

## 查看异常日志

```
{
    "level": "info",
    "ts": "2024-03-16T16:25:19.961Z",
    "caller": "etcdmain/etcd.go:73",
    "msg": "Running: ",
    "args": [
        "etcd",
        "--advertise-client-urls=https://192.168.26.100:2379",
        "--cert-file=/etc/kubernetes/pki/etcd/server.crt",
        "--client-cert-auth=true",
        "--data-dir=/var/lib/etcd",
        "--experimental-initial-corrupt-check=true",
        "--experimental-watch-progress-notify-interval=5s",
        "--initial-advertise-peer-urls=https://192.168.26.100:2380",
        "--initial-cluster=vms100.liruilongs.github.io=https://192.168.26.100:2380,vms101.liruilongs.github.io=https://192.168.26.101:2380",
        "--initial-cluster-state=existing",
        "--key-file=/etc/kubernetes/pki/etcd/server.key",
        "--listen-client-urls=https://127.0.0.1:2379,https://192.168.26.100:2379",
        "--listen-metrics-urls=http://127.0.0.1:2381",
        "--listen-peer-urls=https://192.168.26.100:2380",
        "--name=vms100.liruilongs.github.io",
        "--peer-cert-file=/etc/kubernetes/pki/etcd/peer.crt",
        "--peer-client-cert-auth=true",
        "--peer-key-file=/etc/kubernetes/pki/etcd/peer.key",
        "--peer-trusted-ca-file=/etc/kubernetes/pki/etcd/ca.crt",
        "--snapshot-count=10000",
        "--trusted-ca-file=/etc/kubernetes/pki/etcd/ca.crt"
    ]
}
..............................................................................
{
    "level": "warn",
    "ts": "2024-03-16T16:25:19.981Z",
    "caller": "etcdmain/etcd.go:146",
    "msg": "failed to start etcd",
    "error": "error validating peerURLs {ClusterID:4816f346663d82a7 Members:[&{ID:b8cb9f66c2e63b91 RaftAttributes:{PeerURLs:[https://192.168.26.102:2380] IsLearner:false} Attributes:{Name:vms102.liruilongs.github.io ClientURLs:[https://192.168.26.102:2379]}} &{ID:3fbbbed942c51f7b RaftAttributes:{PeerURLs:[https://192.168.26.100:2380] IsLearner:false} Attributes:{Name: ClientURLs:[]}} &{ID:70059e836d19883d RaftAttributes:{PeerURLs:[https://192.168.26.101:2380] IsLearner:false} Attributes:{Name:vms101.liruilongs.github.io ClientURLs:[https://192.168.26.101:2379]}}] RemovedMemberIDs:[]}: member count is unequal"
}
{
    "level": "fatal",
    "ts": "2024-03-16T16:25:19.981Z",
    "caller": "etcdmain/etcd.go:204",
    "msg": "discovery failed",
    "error": "error validating peerURLs {ClusterID:4816f346663d82a7 Members:[&{ID:b8cb9f66c2e63b91 RaftAttributes:{PeerURLs:[https://192.168.26.102:2380] IsLearner:false} Attributes:{Name:vms102.liruilongs.github.io ClientURLs:[https://192.168.26.102:2379]}} &{ID:3fbbbed942c51f7b RaftAttributes:{PeerURLs:[https://192.168.26.100:2380] IsLearner:false} Attributes:{Name: ClientURLs:[]}} &{ID:70059e836d19883d RaftAttributes:{PeerURLs:[https://192.168.26.101:2380] IsLearner:false} Attributes:{Name:vms101.liruilongs.github.io ClientURLs:[https://192.168.26.101:2379]}}] RemovedMemberIDs:[]}: member count is unequal",
    "stacktrace": "go.etcd.io/etcd/server/v3/etcdmain.startEtcdOrProxyV2\n\t/go/src/go.etcd.io/etcd/release/etcd/server/etcdmain/etcd.go:204\ngo.etcd.io/etcd/server/v3/etcdmain.Main\n\t/go/src/go.etcd.io/etcd/release/etcd/server/etcdmain/main.go:40\nmain.main\n\t/go/src/go.etcd.io/etcd/release/etcd/server/main.go:32\nruntime.main\n\t/go/gos/go1.16.15/src/runtime/proc.go:225"
}
```

可以看到它提示可能错误与 `vms102.liruilongs.github.io` 节点相关

## 对比配置文件

然后我们看一下 `vms102.liruilongs.github.io` 的配置文件

```
┌──[root@vms102.liruilongs.github.io]-[~]
└─$cat /etc/kubernetes/manifests/etcd.yaml
apiVersion: v1
kind: Pod
metadata:
  annotations:
    kubeadm.kubernetes.io/etcd.advertise-client-urls: https://192.168.26.102:2379
  creationTimestamp: null
  labels:
    component: etcd
    tier: control-plane
  name: etcd
  namespace: kube-system
spec:
  containers:
  - command:
    - etcd
    - --advertise-client-urls=https://192.168.26.102:2379
    - --cert-file=/etc/kubernetes/pki/etcd/server.crt
    - --client-cert-auth=true
    - --data-dir=/var/lib/etcd
    - --experimental-initial-corrupt-check=true
    - --experimental-watch-progress-notify-interval=5s
    - --initial-advertise-peer-urls=https://192.168.26.102:2380
    - --initial-cluster=vms100.liruilongs.github.io=https://192.168.26.100:2380,vms102.liruilongs.github.io=https://192.168.26.102:2380,vms101.liruilongs.github.io=https://192.168.26.101:2380
    - --initial-cluster-state=existing
    - --key-file=/etc/kubernetes/pki/etcd/server.key
    - --listen-client-urls=https://127.0.0.1:2379,https://192.168.26.102:2379
    - --listen-metrics-urls=http://127.0.0.1:2381
    - --listen-peer-urls=https://192.168.26.102:2380
    - --name=vms102.liruilongs.github.io
    - --peer-cert-file=/etc/kubernetes/pki/etcd/peer.crt
    - --peer-client-cert-auth=true
    - --peer-key-file=/etc/kubernetes/pki/etcd/peer.key
    - --peer-trusted-ca-file=/etc/kubernetes/pki/etcd/ca.crt
    - --snapshot-count=10000
    - --trusted-ca-file=/etc/kubernetes/pki/etcd/ca.crt
```

通过配置文件比对，可以发现，之前配置的故障节点的配置任然有问题，少了一个 `vms102.liruilongs.github.io=https://192.168.26.102:2380` 节点信息。

```
"--initial-cluster=vms100.liruilongs.github.io=https://192.168.26.100:2380,vms101.liruilongs.github.io=https://192.168.26.101:2380",
"--initial-cluster=vms100.liruilongs.github.io=https://192.168.26.100:2380,vms102.liruilongs.github.io=https://192.168.26.102:2380,vms101.liruilongs.github.io=https://192.168.26.101:2380",
```

## 修复配置并验证集群状态

修改完配置，按照上面相同的流程重新恢复节点，节点恢复

通过 `etcdctl` 命令检查

```
┌──[root@vms100.liruilongs.github.io]-[~]
└─$ETCDCTL_API=3 etcdctl  --endpoints https://127.0.0.1:2379  --cert="/etc/kubernetes/pki/etcd/server.crt"  --key="/etc/kubernetes/pki/etcd/server.key"  --cacert="/etc/kubernetes/pki/etcd/ca.crt" member list -w table
+------------------+---------+-----------------------------+-----------------------------+-----------------------------+
|        ID        | STATUS  |            NAME             |         PEER ADDRS          |        CLIENT ADDRS         |
+------------------+---------+-----------------------------+-----------------------------+-----------------------------+
| 70059e836d19883d | started | vms101.liruilongs.github.io | https://192.168.26.101:2380 | https://192.168.26.101:2379 |
| ac5f6045dbe477b3 | started | vms100.liruilongs.github.io | https://192.168.26.100:2380 | https://192.168.26.100:2379 |
| b8cb9f66c2e63b91 | started | vms102.liruilongs.github.io | https://192.168.26.102:2380 | https://192.168.26.102:2379 |
+------------------+---------+-----------------------------+-----------------------------+-----------------------------+
```



```
┌──[root@vms100.liruilongs.github.io]-[~]
└─$ETCDCTL_API=3 etcdctl  --endpoints https://127.0.0.1:2379  --cert=&quot;/etc/kubernetes/pki/etcd/server.crt&quot;  --key=&quot;/etc/kubernetes/pki/etcd/server.key&quot;  --cacert=&quot;/etc/kubernetes/pki/etcd/ca.crt&quot; endpoint status --cluster  -w table
+-----------------------------+------------------+---------+---------+-----------+-----------+------------+
|          ENDPOINT           |        ID        | VERSION | DB SIZE | IS LEADER | RAFT TERM | RAFT INDEX |
+-----------------------------+------------------+---------+---------+-----------+-----------+------------+
| https://192.168.26.101:2379 | 70059e836d19883d |   3.5.4 |   88 MB |     false |       603 |   22227327 |
| https://192.168.26.100:2379 | ac5f6045dbe477b3 |   3.5.4 |   88 MB |     false |       603 |   22227327 |
| https://192.168.26.102:2379 | b8cb9f66c2e63b91 |   3.5.4 |   88 MB |      true |       603 |   22227327 |
+-----------------------------+------------------+---------+---------+-----------+-----------+------------+
┌──[root@vms100.liruilongs.github.io]-[~]
└─$
```

故障节点恢复，在实际的操作中，添加完节点，我们需要确认故障节点的配置文件是否是正确的配置文件

---

© 2018-2024 liruilonger@gmail.com , All rights reserved. 保持署名-非商用-相同方式共享(CC BY-NC-SA 4.0)
