
# K8s `CrashLoopBackOff` 如何排障？

> 来源：[https://geekbang.feishu.cn/docx/VLh6dDT5Yoe1ezxV4TXczYMOnYb](https://geekbang.feishu.cn/docx/VLh6dDT5Yoe1ezxV4TXczYMOnYb)

> 👨🏻 作者：山河已无恙

> 📃 作者介绍：SRE，云原生相关技术干货分享，RHCA/RHCE/RHCAS、CKA 认证，CSDN/51CTO博客专家，InfoQ写作平台/Alibaba Cloud社区签约作者、Huawei Cloud社区云享专家、Tencent Cloud社区内容共创官

> ⏰ 收录时间：2024-05-27 11:48:42 (UTC+8)

> 🔗 原文链接：h[ttps://mp.weixin.q](https://mp.weixin.qq.com/s?__biz=MzkyNjIxNTYwMw==&mid=2247493685&idx=1&sn=94a7fbda0a303fa519d1fef93eaae591&chksm=c23814e9f54f9dff8134b52b8c1b6d557d2b9616c90eb684e7bb71a267b340c804f244e0ce29#rd)q.com/s?__biz=Mz...

![](img/812ca7f4ff77dfa24dc1d26588177dc2.png)

关注作者，获取更多AI知识

## 写在前面

- 整理 `CrashLoopBackOff` 排故相关笔记分享给小伙伴。

- 博文内容涉及：

- 什么是 `CrashLoopBackOff` ？

- 如何对 `CrashLoopBackOff` 排故？

- 理解不足小伙伴帮忙指正

「 中秋明月，豪门有，贫家也有。极慰人心。——烽火戏诸侯《剑来》 」

---

## 什么是 `CrashLoopBackOff`

`CrashLoopBackOff` 是在 k8s 中较常见的一种 `Pod` 异常状态，最直接的表述，集群中的 `Pod` 在不断的重启挂掉，一直循环，往往 `Pod` 运行几秒钟 因为程序异常会直接死掉，没有常驻进程，但是 容器运行时 会根据 `Pod` 的重启策略(默认为：`always`)一直的重启它，所以会 `CrashLoopBackOff`

`pod`的重启策略 `restartpolicy` ：`pod`在遇到故障之后重启的动作：

- `always`：当容器退出时，总是重启容器，默认策略

- `onfailure`：当容器异常退出(退出状态码非0)时，重启容器

- `nerver`：当容器退出时，从不重启容器

复现很容易，可以简单的启动一个 `busybox` 容器，`sleep` 一会，`exit` 指定异常退出状态码

```
apiVersion: v1
kind: Pod
metadata:
  name: crashlookbackoff-pod
spec:
  containers:
  - name: busybox
    image: busybox
    args:
    - /bin/sh
    - -c
    - sleep 5;exit 3
```

安装了 `Istio` ，所以自动注入了初始化容器 `istio-init` 和容器代理 `istio-proxy`

```
┌──[root@vms100.liruilongs.github.io]-[~/ansible/crashlookbackoff_demo]
└─$kubectl apply -f crashlookbackoff_pod.yaml
pod/crashlookbackoff-pod created
┌──[root@vms100.liruilongs.github.io]-[~/ansible/crashlookbackoff_demo]
└─$kubectl get pods crashlookbackoff-pod -w
NAME                   READY   STATUS     RESTARTS   AGE
crashlookbackoff-pod   0/2     Init:0/1           0          10s
crashlookbackoff-pod   0/2     PodInitializing    0          12s
crashlookbackoff-pod   1/2     Running            0          30s
crashlookbackoff-pod   0/2     Error              0          38s
crashlookbackoff-pod   1/2     Error              0          39s
crashlookbackoff-pod   2/2     Running            1 (21s ago)   55s
crashlookbackoff-pod   1/2     Error              1 (27s ago)   61s
crashlookbackoff-pod   1/2     CrashLoopBackOff   1 (11s ago)   71s
crashlookbackoff-pod   2/2     Running            2 (13s ago)   73s
crashlookbackoff-pod   1/2     Error              2 (18s ago)   78s
crashlookbackoff-pod   1/2     CrashLoopBackOff   2 (17s ago)   94s
┌──[root@vms100.liruilongs.github.io]-[~/ansible/crashlookbackoff_demo]
└─$
```

当然，这是 会触发 `CrashLoopBackOff` 的其中一种原因，实际生产中还有很多其他场景

### `Pod` 内容器化应用程序异常退出

在最上面的问题复现即是这种情况，容器中的进程因为内部原因挂掉，对于这种情况， 需要根据 `Pod` 日志信息来排查问题 ，对代码中的问题进行处理。也可以通过 `kubectl describe pods` 来定位问题的原因

- 程序本身有 Bug，导致常驻进程挂掉

- 需要加载的配置信息，需要获取的其他信息没有加载到，或者是设置错误 ，程序报错挂掉，获取无法正常启动

- 容器需要的 `pod` 内部资源被占用 比如端口，文件系统等 ，文件系统只读，一个 `pod` 的两个容器同时监听一个端口

- 网络方面，无法连接或者访问某个服务，程序超时挂掉，或者在微服务中依赖的相关服务没有起来或者挂掉。

### 健康检查配置异常

`Kubernetes` 对 `Pod` 的健康状态可以通过两类探针来检查: `LivenessProbe` 和 `ReadinessProbe` ,检测方式都有三种(`exec`/`TCP`/`HTTP`) 这里我们只考虑第一种 `LivenessProbe` ，`LivenessProbe` 用于检查服务是否存活 在返回失败后 `kubelet` 会杀掉容器，根据重启策略重启启动一个， `ReadinessProbe` 用于检测 `pod` 对应的服务是否会提供能力，当返回失败的会把对应的 `endpoint` 从 `Service` 负载列表中移除。

在 `LivenessProbe` 中如果配置错误，那么探针返回的永远都是失败，那么 `kubelet` 会认为 `pod` 已经死掉，会杀掉当前进程，重启一个新的 `pod`，以此循环，触发 `CrashLoopBackOff` 。

### 资源不足

这种情况在我的本地实验环境中遇到很多几次，实验环境为物理机上的虚机环境，资源不足导致容器无法加载，往往是在使用 `HELM` 或者其他工具安装部署大量 API 对象,涉及大量工作负载时会发生，比如 ELK 或者 普罗米修斯，其他的一些很重的客户端或者观测工具相关部署时，部分 `Pod` 状态会变成这样.

大部分原因是因为默认的 `LivenessProbe` 配置的时间内， `Pod` 因为资源的问题无法正常运行。健康检查返回 失败，`kubelet` 会杀掉 `pod` 重新启动。以此循环。还有一种情况是内存不够触发 OOM killer ，`pod` 容器进程被干掉导致。

对于这种情况，可以调整工作负载相关控制器对应的副本数，会尝试调小一点，如果没办法调整，一般不去理会，等几个小时自己就好了 :) ，建议尝试调整相关资源限制，`pod` 的 `resources` (考虑Qos) 或者 考虑 `LimitRange` ， `Resource Quotas` (没有实际调整过，感觉没什么用，应该只涉及到准入检查)

## 如何对 `CrashLoopBackOff` 排故？

对于 `CrashLoopBackOff` 的问题定位，建议通过下面的方式进行。

### 检查事件

运行 `kubectl describe pod [name]` 。查看 事件 信息，通过事件可以查看到 健康检查失败 等相关信息，比如下面的一些事件

- `Back-off restarting failed container.`

- `Liveness probe failed: XXXX`

可能是资源不够用，导致 `pod` 没有启动成功，健康检查判断失败，任务 `pod` 已经挂掉，所以 `kubelet` 会反复杀掉当前 `pod` 重启一个。也可能是 健康检查配置问题，配置的检测条件是一个持续失败的条件。看一个 demo .

```
apiVersion: v1
kind: Pod
metadata:
  name: crashlookbackoff-pod
spec:
  containers:
  - name: busybox
    image: busybox
    args:
    - /bin/sh
    - -c
    - sleep infinity
    livenessProbe:
      exec:
        command:
        - cat
        - /tmp/liruilong
      initialDelaySeconds: 5 #容器启动的5s内不监测
      periodSeconds: 5 #每5s钟检测一次
```

上面的是一个配置健康检查的 `pod`，检查方式为 `exec` ,这是一个永远返回失败的条件，所以正常会触发 `CrashLoopBackOff` ,下面为通过 `kubectl describe pods crashlookbackoff-pod | grep -A 20 -i event` 查看事件信息。

```
┌──[root@vms100.liruilongs.github.io]-[~/ansible/crashlookbackoff_demo]
└─$kubectl describe  pods crashlookbackoff-pod | grep -A 20  -i event
Events:
  Type     Reason     Age                   From               Message
  ----     ------     ----                  ----               -------
  Normal   Scheduled  17m                   default-scheduler  Successfully assigned default/crashlookbackoff-pod to vms106.liruilongs.github.io
  Normal   Pulled     17m                   kubelet            Container image &quot;docker.io/istio/proxyv2:1.16.2&quot; already present on machine
  Normal   Created    17m                   kubelet            Created container istio-init
  Normal   Started    17m                   kubelet            Started container istio-init
  Normal   Pulled     17m                   kubelet            Successfully pulled image &quot;busybox&quot; in 15.691344116s
  Normal   Pulled     17m                   kubelet            Container image &quot;docker.io/istio/proxyv2:1.16.2&quot; already present on machine
  Normal   Created    17m                   kubelet            Created container istio-proxy
  Normal   Started    17m                   kubelet            Started container istio-proxy
  Warning  Unhealthy  17m (x2 over 17m)     kubelet            Readiness probe failed: Get &quot;http://10.244.31.80:15021/healthz/ready&quot;: dial tcp 10.244.31.80:15021: connect: connection refused
  Normal   Pulled     16m                   kubelet            Successfully pulled image &quot;busybox&quot; in 15.599021058s
  Normal   Created    16m (x2 over 17m)     kubelet            Created container busybox
  Normal   Started    16m (x2 over 17m)     kubelet            Started container busybox
  Warning  Unhealthy  16m (x6 over 17m)     kubelet            Liveness probe failed: cat: can&#x27;t open &#x27;/tmp/liruilong&#x27;: No such file or directory
  Normal   Killing    16m (x2 over 17m)     kubelet            Container busybox failed liveness probe, will be restarted
  Normal   Pulling    12m (x6 over 17m)     kubelet            Pulling image &quot;busybox&quot;
  Warning  BackOff    2m39s (x36 over 11m)  kubelet            Back-off restarting failed container
┌──[root@vms100.liruilongs.github.io]-[~/ansible/crashlookbackoff_demo]
└─$kubectl get pods crashlookbackoff-pod -w
NAME                   READY   STATUS             RESTARTS       AGE
crashlookbackoff-pod   1/2     CrashLoopBackOff   7 (3m4s ago)   15m  
```

可以看到 `Liveness probe failed: `, `Back-off restarting failed container` 相关的事件，在生产环境可以通过 服务可用性检查相关定位问题，检查是配置错误，还是资源不够用，如果是资源问题，是否需要调整 `periodSeconds` 或 `initialDelaySeconds` 给应用程序更多的启动时间。

### 检查日志

如果上一步没有提供任何细节或无法识别，下一步我们可以通过日志信息获取相关信息。

类似在最上面的 Demo，模拟程序 bug 的 `sleep 5;exit 3` ，我们查看事件信息，基本上得不到有用的信息(见下面的代码), `Back-off restarting failed container` 只是告诉我们容器重启失败。



```
┌──[root@vms100.liruilongs.github.io]-[~/ansible/crashlookbackoff_demo]
└─$kubectl describe pods crashlookbackoff-pod | grep -A 20  Events:
Events:
  Type     Reason     Age                     From               Message
  ----     ------     ----                    ----               -------
  Normal   Scheduled  9m52s                   default-scheduler  Successfully assigned default/crashlookbackoff-pod to vms106.liruilongs.github.io
  Normal   Pulled     9m50s                   kubelet            Container image &quot;docker.io/istio/proxyv2:1.16.2&quot; already present on machine
  Normal   Created    9m50s                   kubelet            Created container istio-init
  Normal   Started    9m50s                   kubelet            Started container istio-init
  Normal   Pulled     9m49s                   kubelet            Successfully pulled image &quot;busybox&quot; in 1.007350697s
  Normal   Created    9m48s                   kubelet            Created container istio-proxy
  Normal   Pulled     9m48s                   kubelet            Container image &quot;docker.io/istio/proxyv2:1.16.2&quot; already present on machine
  Normal   Started    9m47s                   kubelet            Started container istio-proxy
  Normal   Pulled     9m26s                   kubelet            Successfully pulled image &quot;busybox&quot; in 15.685743099s
  Normal   Pulled     8m53s                   kubelet            Successfully pulled image &quot;busybox&quot; in 16.040759951s
  Normal   Pulling    8m22s (x4 over 9m50s)   kubelet            Pulling image &quot;busybox&quot;
  Normal   Created    8m6s (x4 over 9m48s)    kubelet            Created container busybox
  Normal   Started    8m6s (x4 over 9m48s)    kubelet            Started container busybox
  Normal   Pulled     8m6s                    kubelet            Successfully pulled image &quot;busybox&quot; in 15.878975739s
  Warning  BackOff    4m50s (x15 over 9m20s)  kubelet            Back-off restarting failed container
┌──[root@vms100.liruilongs.github.io]-[~/ansible/crashlookbackoff_demo]
└─$kubectl get pods crashlookbackoff-pod
NAME                   READY   STATUS             RESTARTS        AGE
crashlookbackoff-pod   1/2     CrashLoopBackOff   6 (2m27s ago)   10m
┌──[root@vms100.liruilongs.github.io]-[~/ansible/crashlookbackoff_demo]
└─$
```

这里可以通过日志信息定位问题

## 检查 pod 日志

通过 `kubectl logs [podname] -c [containername]` 获取 `pod` 日志信息，如果只有一个容器，则不需要 `-c` 参数。这里看一个我前段时间遇到 `CrashLoopBackOff` 的问题，在安装阿里开源的一个 k8s 工作负载增强工具 `Kruise` 的时候，`DS` 资源一直在重启

下面为通过 `Helm` 部署后的资源查看

```
┌──[root@vms100.liruilongs.github.io]-[~/ansible/openkruise]
└─$kubectl get all -n kruise-system
NAME                                             READY   STATUS             RESTARTS      AGE
pod/kruise-controller-manager-7dc584559b-j8j78   1/1     Running            0             2m31s
pod/kruise-controller-manager-7dc584559b-r954q   1/1     Running            0             2m32s
pod/kruise-daemon-24fgt                          0/1     CrashLoopBackOff   4 (9s ago)    2m33s
pod/kruise-daemon-7t5q6                          0/1     CrashLoopBackOff   4 (11s ago)   2m32s
pod/kruise-daemon-fbt8m                          0/1     CrashLoopBackOff   4 (16s ago)   2m33s
pod/kruise-daemon-fc8xr                          0/1     CrashLoopBackOff   4 (11s ago)   2m32s
pod/kruise-daemon-kjjfd                          0/1     CrashLoopBackOff   4 (15s ago)   2m32s
pod/kruise-daemon-krs9s                          0/1     CrashLoopBackOff   4 (17s ago)   2m33s
pod/kruise-daemon-lb5nq                          0/1     CrashLoopBackOff   4 (15s ago)   2m32s
pod/kruise-daemon-zpfzg                          0/1     CrashLoopBackOff   3 (32s ago)   2m32s
```

通过对 `DS` 的日志查看发现，它通过默认的 `CRI` 接口实现找不到对应的 runtime 容器运行时，

```
W0228 07:29:31.671667       1 mutation_detector.go:53] Mutation detector is enabled, this will result in memory leakage.
E0228 07:29:31.671746       1 factory.go:224] /hostvarrun/docker.sock exists, but not found /hostvarrun/dockershim.sock
W0228 07:29:31.767342       1 factory.go:113] Failed to new image service for containerd (, unix:///hostvarrun/containerd/containerd.sock): failed to fetch cri-containerd status: rpc error: code = Unimplemented desc = unknown service runtime.v1alpha2.RuntimeService
W0228 07:29:31.767721       1 mutation_detector.go:53] Mutation detector is enabled, this will result in memory leakage.
panic: runtime error: invalid memory address or nil pointer dereference
```

重点在这一句：`/hostvarrun/docker.sock` 存在，但是没有找到 `/hostvarrun/dockershim.sock`，创建新的镜像服务失败。

无法识别 `CRI` 接口实现，`k8s` 在 `1.24` 版本彻底移除了 `dockershim` 的 `CRI` 接口实现，当前版本为 `1.25`，所以找不到对应的 `CRI` 实现，高版本容器运行时还是用 `docker` 的话，需要安装一个 `cri-docker` 的 `CRI` 接口实现，所以这里需要告诉这些 `DS`，`CRI` 的接口实现是 `cri-docker` 不是默认的 `dockershim`。

最后提了 `issue`，有大佬指出可以在部署时指定 `daemon.socketLocation=/var/run/`，`daemon.socketFile=cri-dockerd.sock`。指定之后工具可以顺利安装。

## 检查 deploy 日志

运行以下命令以检索 `kubectl` 部署日志

```
┌──[root@vms100.liruilongs.github.io]-[~/ansible/crashlookbackoff_demo]
└─$kubectl logs  -f deployments/release-name-grafana
Found 2 pods, using pod/release-name-grafana-76f4b7b77d-bbvws
[2023-03-26 03:14:28] Starting collector
................
```

这也可能提供有关应用程序级别问题的线索。例如，您可以在下面看到一个显示 `./data` can’t be mounted，可能是因为它已被使用并被其他容器锁定。

## 资源限制

由于内存资源不足，触发 `OOM killer`，可能会遇到 `CrashLoopBackOff` 错误。可以通过更改容器资源清单中的 `resources:limits` 来增加内存限制。或者考虑重 `Qos` 设置，提高 `Qos` 等级等。

这里为什么只有内存，计算资源有很多，`CUP`、`GPU` 之类，这因为 `CPU` 是可压缩资源。内存是不可压缩资源，比如对于一个 `Pod` 来讲。

+ 当 `CUP` 不够时，`cgroups` 会对 `Pod` 中的容器的 `CPU` 使用进行限流（`Throttled`）

+ 当内存不够时，如果 `Pod` 使用的内存量超过了它的 `Requests` 的配置，那么这个 `Pod` 有可能被 `Kubernetes` 杀掉，如果 `Pod` 使用的内存量超过了它的 `Limits` 设置，那么操作系统内核会杀掉 `Pod` 所有容器的所有进程中使用内存最多的一个，直到内存不超过 `Limits` 为止。

## 镜像问题

如果仍然存在问题，另一个原因可能是您使用的 `docker` 镜像可能无法正常工作，您需要确保单独运行时它可以正常工作。如果这对 `Kubernetes` 有效但失败了，您可能需要提前找到正在发生的事情，尝试以下操作。

### 确定 entrypoint 和 cmd

需要确定 `entrypoint` 和 `cmd` 以获得对容器的访问权限以进行调试。请执行下列操作：

+ 运行 `docker pull [image-id]` 拉取镜像。

+ 运行 `docker history 6600fae04efd --no-trunc` 并找到容器的启动命令，或者可以使用 `docker inspect [image-id]` 。

```
┌──[root@vms100.liruilongs.github.io]-[~/ansible/crashlookbackoff_demo]
└─$docker history 6600fae04efd --no-trunc
................
...   /bin/sh -c #(nop)  CMD [&quot;haproxy&quot; &quot;-f&quot; &quot;/usr/local/etc/haproxy/haproxy.cfg&quot;]
...   /bin/sh -c #(nop)  ENTRYPOINT [&quot;/docker-entrypoint&quot;]
 .................
```

### 覆盖 entrypoint

由于容器崩溃无法启动，需要暂时将 `Dockerfile` 的 `entrypoint` 更改为 `tail -f /dev/null`

```
Spec:
     containers:
      - command: [&#x27;tail&#x27;,&#x27;-f&#x27;,&#x27;/dev/null&#x27;]
```

### 检查原因

使用命令行 `kubectl exec` 执行进入问题容器。

```
┌──[root@vms100.liruilongs.github.io]-[/etc/kubernetes/manifests]
└─$kubectl exec -it release-name-grafana-76f4b7b77d-ddr7k -- sh
/app $ ls
helpers.py    resources.py  sidecar.py
/app $
```

单独启动进程，查看日志，确认配置信息。存在问题修复。

## 社区提问，请教大佬

如果问题还是没有解决，可以在相对活跃的社区提问，请教大佬，比如 `StackOverflow` 或者 `Slack`。

## 博文部分内容参考

© 文中涉及参考链接内容版权归原作者所有，如有侵权请告知

---

`https://foxutech.medium.com/kubernetes-crashloopbackoff-how-to-troubleshoot-940dbb16bc84`

---

© 2018-2023 liruilonger@gmail.com, All rights reserved. 保持署名-非商用-相同方式共享(CC BY-NC-SA 4.0)
