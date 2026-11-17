
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
