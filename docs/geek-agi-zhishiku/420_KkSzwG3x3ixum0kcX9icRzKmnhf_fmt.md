
# Kubernetes中资源限制的一些笔记整理

> 来源：[https://geekbang.feishu.cn/docx/NwdRdEOl2opAOnxwpzEch4b6ntd](https://geekbang.feishu.cn/docx/NwdRdEOl2opAOnxwpzEch4b6ntd)

> 👨🏻 作者：山河已无恙

> 📃 作者介绍：SRE，云原生相关技术干货分享，RHCA/RHCE/RHCAS、CKA 认证，CSDN/51CTO博客专家，InfoQ写作平台/Alibaba Cloud社区签约作者、Huawei Cloud社区云享专家、Tencent Cloud社区内容共创官

> ⏰ 收录时间：2024-05-27 11:48:42 (UTC+8)

> 🔗 原文链接：https://mp.weixin.qq.com/s?__biz=MzkyNjIxNTYwMw==&mid=2247493685&idx=1&sn=94a7fbda0a303fa519d1fef93eaae591&chksm=c23814e9f54f9dff8134b52b8c1b6d557d2b9616c90eb684e7bb71a267b340c804f244e0ce29#rd

![](img/62a730808aae7e9a0b35acbc8c29bec4.png)

关注作者，获取更多AI知识

## 写在前面

+ 今天和小伙伴们分享`K8s`中`pod`资源限制的一些笔记

+ 博文内存涉及`pod`中通过`request`和`limits`实现资源的申请和限制

+ 理解不足小伙伴帮忙指正，生活加油 ^_^

我们的痛苦来源于“夸父追日”一般的对“更好”的追求，也来自于自己的自卑与狂妄。--------duoduokk

---

学习之前，我们要准备下实验环境，新建一个新的命名空间，并且切换过去,提前拉一下实验镜像

```
┌──[root@vms81.liruilongs.github.io]-[~/ansible]
└─$mkdir resources
┌──[root@vms81.liruilongs.github.io]-[~/ansible]
└─$cd resources/
┌──[root@vms81.liruilongs.github.io]-[~/ansible/resources]
└─$kubectl  create ns  resources
namespace/resources created
┌──[root@vms81.liruilongs.github.io]-[~/ansible/resources]
└─$kubectl config set-context  $(kubectl config current-context) --namespace=resources
Context "kubernetes-admin@kubernetes" modified.
┌──[root@vms81.liruilongs.github.io]-[~/ansible/resources]
└─$cd ..
┌──[root@vms81.liruilongs.github.io]-[~/ansible]
└─$ansible node -m shell -a "docker pull hub.c.163.com/library/centos"
```

### 为什么要资源限制

一般在本地做实验，或者`Demo`的时候，不会对`pod`的资源进行限制，只有在生产环境会严格配置`pod`的资源限制。

当不对`pod`的资源做限制时，`K8s`的调度器会认为当前`pod`所需要的资源很少，并且可以调度到任意节点上，但是这样有一个很严重的弊端，如果不做资源限制，比如拿内存来讲，如果`pod`中的容器存在内存不回收的情况，那么会无休止的使用宿主节点的内存资源，从而会引发宿主机的`OOM`，甚至会触发宿主机内核 `OOM Killer` (内存杀手)，来保证宿主机不挂掉。

看一个`Demo`，创建一个没有限制资源的`pod`

```
┌──[root@vms81.liruilongs.github.io]-[~/ansible/resources]
└─$cat pod-demo.yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: pod-demo
  name: pod-demo
spec:
  containers:
  - image: hub.c.163.com/library/centos
    imagePullPolicy: IfNotPresent
    name: pod-demo
    command: ['sh','-c','sleep 500000']
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```

`pod`创建成功后,我们可以看到调度到了 `vms83.liruilongs.github.io`

```
┌──[root@vms81.liruilongs.github.io]-[~/ansible/resources]
└─$kubectl get pods -o wide
No resources found in resources namespace.
┌──[root@vms81.liruilongs.github.io]-[~/ansible/resources]
└─$vim  pod-demo.yaml
┌──[root@vms81.liruilongs.github.io]-[~/ansible/resources]
└─$kubectl  apply  -f pod-demo.yaml
pod/pod-demo created
┌──[root@vms81.liruilongs.github.io]-[~/ansible/resources]
└─$kubectl get pods -o wide
NAME       READY   STATUS    RESTARTS   AGE   IP             NODE                         NOMINATED NODE   READINESS GATES
pod-demo   1/1     Running   0          42s   10.244.70.50   vms83.liruilongs.github.io   <none>           <none>
```

这里我们在`pod`里安装一个内存分配工具 `bigmem`，用于模拟`pod`中容器进程内存不回收的情况。

```
┌──[root@vms81.liruilongs.github.io]-[~/ansible]
└─$kubectl  cp ./bigmem-7.0-1.r29766.x86_64.rpm pod-demo:/root/
┌──[root@vms81.liruilongs.github.io]-[~/ansible]
└─$kubectl  exec -it pod-demo  -- bin/bash
[root@pod-demo /]# cd root/
[root@pod-demo ~]# ls
anaconda-ks.cfg  bigmem-7.0-1.r29766.x86_64.rpm  original-ks.cfg
[root@pod-demo ~]# rpm -ivh bigmem-7.0-1.r29766.x86_64.rpm
Preparing...                          ################################# [100%]
Updating / installing...
   1:bigmem-7.0-1.r29766              ################################# [100%]
[root@pod-demo ~]# bigmem 1000M
Attempting to allocate 1000 Mebibytes of resident memory...
Press <Enter> to exit^C
[root@pod-demo ~]# bigmem 2000M
Attempting to allocate 2000 Mebibytes of resident memory...
Killed
```

通过上下内存信息可以发现，当分配`1000M`内存时，宿主机用户使用内存增加了`1000M`，可用内存为`117M`，当申请内存为`2000M`时，超出宿主机可用内存，`bigmem 2000M`命令所在进程直接被`kill`了。

```
┌──[root@vms81.liruilongs.github.io]-[~/ansible]
└─$ansible vms83.liruilongs.github.io -m shell  -a "free -h"
vms83.liruilongs.github.io | CHANGED | rc=0 >>
              total        used        free      shared  buff/cache   available
Mem:           4.4G        2.5G        583M        216M        1.4G        1.4G
Swap:            0B          0B          0B
┌──[root@vms81.liruilongs.github.io]-[~/ansible]
└─$ansible vms83.liruilongs.github.io -m shell  -a "free -h"
vms83.liruilongs.github.io | CHANGED | rc=0 >>
              total        used        free      shared  buff/cache   available
Mem:           4.4G        3.5G        117M        216M        857M        456M
Swap:            0B          0B          0B
```

查看宿主机日志 `/var/log/messages`，可以发现 `bigmem` 所在进程造成`OOM`。被`OOM Killer`杀掉了。

```
┌──[root@vms83.liruilongs.github.io]-[~]
└─$cat /var/log/messages | grep -i memory
Aug 10 20:37:27 vms83 kernel: [<ffffffff81186bd6>] out_of_memory+0x4b6/0x4f0
Aug 10 20:37:27 vms83 kernel: Out of memory: Kill process 25143 (bigmem) score 1347 or sacrifice child
```

如果不对`pod`做资源限制，他会认为宿主机的资源全是自己的，会无休止的使用，直到宿主机内存不足被`OOM Killer`直接杀了，为什么会被 宿主机 杀掉，容器的本质即运行在宿主机上的一个进程组。

通过 `top` 命令监控 `node`资源，可以发现由于 `OOM`的问题， 可能会造成的节点短暂性死机，无法采集同步节点信息。

```
┌──[root@vms81.liruilongs.github.io]-[~/ansible]
└─$kubectl top nodes
NAME                         CPU(cores)   CPU%        MEMORY(bytes)   MEMORY%
vms81.liruilongs.github.io   189m         9%          1816Mi          58%
vms82.liruilongs.github.io   112m         3%          819Mi           17%
vms83.liruilongs.github.io   <unknown>    <unknown>   <unknown>       <unknown>
```

必须对资源进行限制，虽然上面的`OOM`情况下，杀掉了`bigmem`进程，但是实际上 `OOM Killer` 杀进程是不确定的，确定`OOM`杀手应该杀死哪个进程，内核会为每个进程保持一个运行不良评分，分数越高，进程越有可能被`OOM`杀手杀死。许多因素被用来计算这个分数。

所以当前节点上的任何一个`Pod` 进程都有可以能被杀掉。但有些`Pod`可能担负着很重要的职责，比其他`Pod`更重要，比如与数据存储相关的、与登录相关的、与查询余额相关的，即使系统资源严重不足，也需要保障这些`Pod`的存活。

所以`Kubernetes`中提供了一些保障机制：

1.  通过资源限额来确保不同的`Pod`只能占用指定的资源

2.  允许集群的资源被超额分配，以提高集群的资源利用率。

3.  为`Pod`划分等级，确保不同等级的`Pod`有不同的服务质量(`QoS`)，资源不足时，低等级的`Pod`会被清理，以确保高等级的`Pod`稳定运行。

今天和小伙伴分享的主要第一种方式

`Kubernetes`集群里的 `节点` 提供的资源主要是 `计算资源` ，`计算资源`是可计量的能被申请、分配和使用的基础资源，这使之区别于 `API资源` (`API Resources`，例如`Pod`和`Services`等)。当前`Kubernetes`集群中的计算资源主要包括 `CPU`、`GPU`及`Memory` ，绝大多数常规应用是用不到`GPU`的，因此这里重点介绍`CPU`与`Memory`的资源管理问题。

我们知道，一个程序所使用的`CPU`与`Memory`是一个动态的量，确切地说，是一个范围，跟它的负载密切相关：负载增加时，`CPU`和`Memory`的使用量也会增加。因此最准确的说法是，某个进程的`CPU`使用量为 `0.1个CPU~1个CPU` ，内存占用则为 `500MB~1GB` 。对应到`Kubernetes`的`Pod`容器上，就是下面这4个参数：

+ `spec.container[].resources.requests.cpu`

+ `spec.container[].resources.limits.cpu`

+ `spec.container[].resources.requests.memory`

+ `spec.container[].resources.limits.memory`

### Request 和 limits

在配置`Pod`时可以通过参数 `Request` 为其中的每个容器指定所需使用的 `CPU`与`Memory` 量,类似于一种申请，`Kubernetes`会根据`Request`的值去查找有足够资源的`Node`来调度此`Pod`，如果没有，则调度失败。`pod`会一直处于`pending`

```
┌──[root@vms81.liruilongs.github.io]-[~/ansible/resources]
└─$cat pod-demo-momory.yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: pod-demo
  name: pod-demo
spec:
  containers:
  - image: hub.c.163.com/library/centos
    imagePullPolicy: IfNotPresent
    name: pod-demo
    command: ['sh','-c','sleep 500000']
    resources:
      requests:
        memory: "5000Mi"
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```

当前`pod`所调度的所有`node`最大只有`4.4G`内存，所以`pod`会一直处于`pending`

```
┌──[root@vms81.liruilongs.github.io]-[~/ansible/resources]
└─$kubectl  apply  -f pod-demo-momory.yaml
pod/pod-demo created
┌──[root@vms81.liruilongs.github.io]-[~/ansible/resources]
└─$kubectl  get pods
NAME       READY   STATUS    RESTARTS   AGE
pod-demo   0/1     Pending   0          6s
┌──[root@vms81.liruilongs.github.io]-[~/ansible/resources]
└─$
```

`limits`对应资源量的上限，即最多允许使用这个上限的资源量。

由于`CPU`资源是可压缩的，进程无论如何也不可能突破上限，因此设置起来比较容易。对于`Memory`这种不可压缩资源来说，它的`Limit`设置就是一个问题了，如果设置得小了，当进程在业务繁忙期试图请求超过`Limit`限制的`Memory`时，此进程就会被`Kubernetes`杀掉。因此，`Memory`的`Request`与`Limit`的值需要结合进程的实际需求谨慎设置。



# Kubernetes 内存资源限制与 Cgroup 隔离示例

## 查看 Pod 配置文件

```
┌──[root@vms81.liruilongs.github.io]-[~/ansible/resources]
└─$cat pod-demo-momory.yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: pod-demo
  name: pod-demo
spec:
  containers:
  - image: hub.c.163.com/library/centos
    imagePullPolicy: IfNotPresent
    name: pod-demo
    command: ['sh','-c','sleep 500000']
    resources:
      requests:
        memory: "2000Mi"
      limits:
        memory: "3000Mi"
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```

## 应用配置并查看 Pod 状态

```
┌──[root@vms81.liruilongs.github.io]-[~/ansible/resources]
└─$vim pod-demo-momory.yaml
┌──[root@vms81.liruilongs.github.io]-[~/ansible/resources]
└─$kubectl  apply  -f pod-demo-momory.yaml
pod/pod-demo created
┌──[root@vms81.liruilongs.github.io]-[~/ansible/resources]
└─$kubectl  get pods -o wide
NAME       READY   STATUS    RESTARTS   AGE   IP             NODE                         NOMINATED NODE   READINESS GATES
pod-demo   1/1     Running   0          12s   10.244.70.55   vms83.liruilongs.github.io   <none>           <none>
┌──[root@vms81.liruilongs.github.io]-[~/ansible/resources]
└─$
```

## 在容器中测试内存限制

```
┌──[root@vms81.liruilongs.github.io]-[~/ansible]
└─$kubectl  cp ./bigmem-7.0-1.r29766.x86_64.rpm pod-demo:/root/
┌──[root@vms81.liruilongs.github.io]-[~/ansible]
└─$kubectl  exec -it pod-demo -- bin/bash
[root@pod-demo /]# cd /root/
[root@pod-demo ~]# rpm -ivh bigmem-7.0-1.r29766.x86_64.rpm
Preparing...                          ################################# [100%]
Updating / installing...
   1:bigmem-7.0-1.r29766              ################################# [100%]
[root@pod-demo ~]# bi
bigmem  bind
[root@pod-demo ~]# free -h
              total        used        free      shared  buff/cache   available
Mem:           4.4G        548M        3.1G        216M        818M        3.4G
Swap:            0B          0B          0B
[root@pod-demo ~]# bigmem  3000M
Attempting to allocate 3000 Mebibytes of resident memory...
Killed
[root@pod-demo ~]# bigmem  2000M
Attempting to allocate 2000 Mebibytes of resident memory...
Press <Enter> to exit^C
[root@pod-demo ~]# bigmem  20000M
Attempting to allocate 20000 Mebibytes of resident memory...
Killed
[root@pod-demo ~]# exit
exit
command terminated with exit code 137
```

可以发现，我们设置了`limits`的值，所以由`cgroup`实现资源隔离来处理内存不够，可以看到顶层的控制组为 `kubepods.slice`，内存不够的时候通过 `Cgroup` 来`kill`掉进程，而不是通过宿主机内核的 OOM Killer 来杀

## 查看内存溢出日志

```
┌──[root@vms83.liruilongs.github.io]-[~]
└─$cat /var/log/messages | grep -i memory
Aug 10 21:08:06 vms83 kernel: [<ffffffff81186c24>] pagefault_out_of_memory+0x14/0x90
Aug 10 21:08:06 vms83 kernel: memory: usage 3072000kB, limit 3072000kB, failcnt 2760
Aug 10 21:08:06 vms83 kernel: memory+swap: usage 3072000kB, limit 9007199254740988kB, failcnt 0
Aug 10 21:08:06 vms83 kernel: Memory cgroup stats for /kubepods.slice/kubepods-burstable.slice/kubepods-burstable-pod804e1f6c_d5c0_4f8a_aa27_bcd66393087c.slice: cache:0KB rss:0KB rss_huge:0KB mapped_file:0KB swap:0KB inactive_anon:0KB active_anon:0KB inactive_file:0KB active_file:0KB unevictable:0KB
Aug 10 21:08:06 vms83 kernel: Memory cgroup stats for /kubepods.slice/kubepods-burstable.slice/kubepods-burstable-pod804e1f6c_d5c0_4f8a_aa27_bcd66393087c.slice/docker-bf3af784f917c54a50d0cb422d8f2624be3ba65a904e126e89081817d457c4d4.scope: cache:0KB rss:40KB rss_huge:0KB mapped_file:0KB swap:0KB inactive_anon:0KB active_anon:40KB inactive_file:0KB active_file:0KB unevictable:0KB
Aug 10 21:08:06 vms83 kernel: Memory cgroup stats for /kubepods.slice/kubepods-burstable.slice/kubepods-burstable-pod804e1f6c_d5c0_4f8a_aa27_bcd66393087c.slice/docker-80f5aff910f2d6e0203fb6fc871cec9bf1d358d7e06ab9d4a381e46bac311465.scope: cache:0KB rss:3071960KB rss_huge:0KB mapped_file:0KB swap:0KB inactive_anon:0KB active_anon:3071912KB inactive_file:0KB active_file:0KB unevictable:0KB
Aug 10 21:08:06 vms83 kernel: Memory cgroup out of memory: Kill process 47482 (bigmem) score 1561 or sacrifice child
Aug 10 21:09:11 vms83 kernel: [<ffffffff81186c24>] pagefault_out_of_memory+0x14/0x90
Aug 10 21:09:11 vms83 kernel: memory: usage 3072000kB, limit 3072000kB, failcnt 2770
Aug 10 21:09:11 vms83 kernel: memory+swap: usage 3072000kB, limit 9007199254740988kB, failcnt 0
Aug 10 21:09:11 vms83 kernel: Memory cgroup stats for /kubepods.slice/kubepods-burstable.slice/kubepods-burstable-pod804e1f6c_d5c0_4f8a_aa27_bcd66393087c.slice: cache:0KB rss:0KB rss_huge:0KB mapped_file:0KB swap:0KB inactive_anon:0KB active_anon:0KB inactive_file:0KB active_file:0KB unevictable:0KB
Aug 10 21:09:11 vms83 kernel: Memory cgroup stats for /kubepods.slice/kubepods-burstable.slice/kubepods-burstable-pod804e1f6c_d5c0_4f8a_aa27_bcd66393087c.slice/docker-bf3af784f917c54a50d0cb422d8f2624be3ba65a904e126e89081817d457c4d4.scope: cache:0KB rss:40KB rss_huge:0KB mapped_file:0KB swap:0KB inactive_anon:0KB active_anon:40KB inactive_file:0KB active_file:0KB unevictable:0KB
Aug 10 21:09:11 vms83 kernel: Memory cgroup stats for /kubepods.slice/kubepods-burstable.slice/kubepods-burstable-pod804e1f6c_d5c0_4f8a_aa27_bcd66393087c.slice/docker-80f5aff910f2d6e0203fb6fc871cec9bf1d358d7e06ab9d4a381e46bac311465.scope: cache:0KB rss:3071960KB rss_huge:0KB mapped_file:0KB swap:0KB inactive_anon:0KB active_anon:3071948KB inactive_file:0KB active_file:0KB unevictable:0KB
Aug 10 21:09:11 vms83 kernel: Memory cgroup out of memory: Kill process 48483 (bigmem) score 1561 or sacrifice child
```

## 如果不设置 CPU 或 Memory 的 Limit 值会怎样

（不考虑上面的问题）

在这种情况下，该`Pod`的资源使用量有一个弹性范围，我们不用绞尽脑汁去思考这两个`Limit`的合理值，但问题也来了，考虑下面的例子：

> `PodA`的`Memory` `Request`被设置为1GB，`NodeA`当时空闲的`Memory`为1.2GB，符合`PodA`的需求，因此`PodA`被调度到`NodeA`上。运行3天后，`PodA`的访问请求大增，内存需要增加到1.5GB，此时`NodeA`的剩余内存只有200MB，由于`PodA`新增的内存已经超出系统资源，所以在这种情况下，`PodA`就会被Kubernetes杀掉。

没有设置`Limit`的`Pod`，或者只设置了`CPULimit`或者`Memory` `Limit`两者之一的`Pod`，表面看都是很有弹性的，但实际上，相对于4个参数都被设置的`Pod`，是处于一种相对不稳定的状态的。

CPU和内存这两种计算资源的特点进行说明。

1. `CPU` `CPU`的`Requests`和`Limits`是通过`CPU`数（`cpus`）来度量的。`CPU`的资源值是绝对值，而不是相对值，比如`0.1CPU`在单核或多核机器上是一样的，都严格等于`0.1CPU` core。

2. `Memory` 内存的`Requests`和`Limits`计量单位是字节数。使用整数或者定点整数加上国际单位制（International System of Units）来表示内存值。国际单位制包括十进制的E、P、T、G、M、K、m，或二进制的Ei、Pi、Ti、Gi、Mi、Ki。KiB与MiB是以二进制表示的字节单位，常见的KB与MB则是以十进制表示的字节单位，Kubernetes的计算资源单位是大小写敏感的

### 官方文档的一些描述

#### 如果你没有指定内存限制

如果你没有为一个容器指定内存限制，则自动遵循以下情况之一：

+ 容器可无限制地使用内存。容器可以使用其所在节点所有的可用内存，进而可能导致该节点调用 OOM Killer。此外，如果发生 OOM Kill，没有资源限制的容器将被杀掉的可行性更大。

+ 运行的容器所在命名空间有默认的内存限制，那么该容器会被自动分配默认限制。集群管理员可用使用 `LimitRange` 来指定默认的内存限制。

#### 内存请求和限制的目的

通过为集群中运行的容器配置内存请求和限制，你可以有效利用集群节点上可用的内存资源。通过将 `Pod` 的内存请求保持在较低水平，你可以更好地安排 `Pod` 调度。通过让内存限制大于内存请求，你可以完成两件事：

+ `Pod` 可以进行一些突发活动，从而更好的利用可用内存。

+ `Pod` 在突发活动期间，可使用的内存被限制为合理的数量。

## 博文参考

+ 《Kubernetes权威指南 第4版》

+ `https://kubernetes.io/zh-cn/docs/tasks/configure-pod-container/assign-memory-resource/`
