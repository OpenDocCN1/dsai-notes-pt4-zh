# K8s：通过 ResourceQuotas 限制命名空间对象创建数量和计算资源使用

> 来源：[https://geekbang.feishu.cn/docx/JgLvdEKrooBZXVxmIa1cmon3nBl](https://geekbang.feishu.cn/docx/JgLvdEKrooBZXVxmIa1cmon3nBl)

> 👨🏻 作者：山河已无恙

> 📃 作者介绍：SRE，云原生相关技术干货分享，RHCA/RHCE/RHCAS、CKA 认证，CSDN/51CTO博客专家，InfoQ写作平台/Alibaba Cloud社区签约作者、Huawei Cloud社区云享专家、Tencent Cloud社区内容共创官

> ⏰ 收录时间：2024-05-27 11:48:42 (UTC+8)

> 🔗 原文链接： h[ttps://mp.weixin.q](https://mp.weixin.qq.com/s?__biz=MzkyNjIxNTYwMw==&mid=2247493115&idx=1&sn=f7159b03293a041e29d615cf84679f2b&chksm=c2381927f54f903123de9c1ec27af2bdc70d1e5eca159fdbece5424bacb168a8b65530310072#rd)q.com/s?__biz=Mz...

![](img/ac67227d3e337792c30e1e2b617d898e.png)

关注作者，获取更多AI知识

## ** 写在前面**

---

+   分享一些 K8s 中资源配额管理 Resource Quotas 的笔记

+   博文内容涉及：

+   Resource Quotas(资源配额) 简单介绍

+   如何开启 资源配额

+   计算，存储，对象资源配额 Demo

+   配额作用域的简单介绍

+   理解不足小伙伴帮忙指正

投我以桃，报之以李。——《大雅·抑》

---

###  Resource Quotas 简单介绍

在 k8s 中，容器、Pod 级别的的计算资源约束设置，可以通过定义 yaml 文件时的 limits 和 requests 字段来做限制， 通过配置不同的 Limits 和 requests 不但可以约束资源 , 还可以实现不同等级的 Qos ，同时可以通过 LimitRange 来对命名空间内的所有 pod 计算资源配置做统一的默认值设置，约束管理。那么对于集群级别命名空间的计算资源约束, k8s 提供了什么解决方案？对于对象数量又有什么方式来进行约束，难道可以无限制的创建 pod、cm、svc 么？

当然不是，对于集群级别， k8s 可以通过 Resource Quotas 来实现集群级别的资源配额，实现对每个命名空间的资源消耗总量提供限制。这里的限制包括：

+   限制命名空间中某种类型的 对象的总数目上限

+   限制命名空间中的 Pod 可以使用的 计算资源的总上限

集群管理员可以为每个命名空间创建一个或多个 Resource Quota 对象。

当用户在命名空间下创建资源（如 Pod、Service 等）时，Kubernetes 的 配额系统 会跟踪集群的资源使用情况， 以确保使用的资源用量不超过 Resource Quota 中定义的 硬性资源限额 。

如果资源创建或者更新请求 违反了配额约束 ，那么该请求会报错 （HTTP 403 FORBIDDEN） ， 并在消息中给出有可能违反的约束。

不管是资源竞争还是配额的修改，都不会影响已经创建的资源使用对象。

如果集群中总的可用资源小于各命名空间中资源配额的总和，那么可能会导致资源竞争。资源竞争时，Kubernetes 系统会遵循先到先得的原则。

对于计算资源，这里的 Resource Quota 和 LimitRange 职责并不重合， Resource Quota 限制命名空间 pod 总的用量(不考虑配额域)，而 LimitRange 限制当前命名空间 中 每个 Pod 或者容器的计算资源。

### ** 启用资源配额 **

当 API 服务器 的命令行标志 --enable-admission-plugins= 中包含 ResourceQuota 时， 资源配额会被启用。当命名空间中存在一个 ResourceQuota 对象时，对于该命名空间而言，资源配额就是开启的。

如果需要开启资源配额，需要在 修改 apiservice 组件静态 pod 的 yaml 文件 kube-apiserver.yaml

```
┌──[root@vms81.liruilongs.github.io]-[/etc/kubernetes/manifests]
└─$cat kube-apiserver.yaml  | grep -i  quota
    - --enable-admission-plugins=NodeRestriction,ResourceQuota
```

查看当前命名空间的 资源配额

```
┌──[root@vms81.liruilongs.github.io]-[/etc/kubernetes/manifests]
└─$kubectl get resourcequotas  -A
No resources found
```

默认情况下，不指定 配额域 的情况，配额配置对当前命名空间有效，指定了配额域的情况，只对配额域匹配的资源有效。

### ** 计算资源配额 **

用户可以对给定命名空间下的可被请求的 计算资源 总量进行限制。

如果命名空间下的计算资源 （如 cpu 和 memory）的 配额被启用 ， 则用户必须为这些 资源设定请求值（request）和约束值（limit） ，否则 配额系统将拒绝 Pod 的创建 。可使用 LimitRanger 准入控制器来为没有设置计算资源需求的 Pod 设置默认值。

```
┌──[root@vms81.liruilongs.github.io]-[~/ansible/quota]
└─$kubectl apply -f my-quota.yaml
resourcequota/object-quota-demo created
┌──[root@vms81.liruilongs.github.io]-[~/ansible/quota]
└─$kubectl describe  resourcequota object-quota-demo
Name:            object-quota-demo
Namespace:       liruilong-topo-namespace
Resource         Used   Hard
--------         ----   ----
limits.cpu       50m    6
limits.memory    100Mi  20Gi
requests.cpu     20m    4
requests.memory  50Mi   15Gi
```

上面的列表中 ， Used 为当前的计算资源情况， Hard 为配额情况。配额机制所支持的资源类型

```
┌──[root@vms81.liruilongs.github.io]-[~/ansible/quota]
└─$cat my-quota.yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: object-quota-demo
spec:
  hard:
    limits.cpu: &#x27;6&#x27;        #所有非终止状态的 Pod，其 CPU 限额总量不能超过该值。
    limits.memory: &#x27;20Gi&#x27;  #所有非终止状态的 Pod，其内存限额总量不能超过该值。
    requests.cpu: &#x27;4&#x27;        #所有非终止状态的 Pod，其 CPU 需求总量不能超过该值。
    requests.memory: &#x27;15Gi&#x27;  #所有非终止状态的 Pod，其内存需求总量不能超过该值。

    #hugepages-&lt;size&gt;        对于所有非终止状态的 Pod，针对指定尺寸的巨页请求总数不能超过此值。
    #cpu     与 requests.cpu 相同。
    #memory  与 requests.memory 相同。

```

### ** 存储资源配额 **

用户可以对给定命名空间下的存储资源 总量进行限制。此外，还可以根据相关的存储类（Storage Class）来限制存储资源的消耗。

```
┌──[root@vms81.liruilongs.github.io]-[~/ansible/quota]
└─$kubectl get sc
NAME                   PROVISIONER             RECLAIMPOLICY   VOLUMEBINDINGMODE      ALLOWVOLUMEEXPANSION   AGE
local-path (default)   rancher.io/local-path   Delete          WaitForFirstConsumer   false                  84d
┌──[root@vms81.liruilongs.github.io]-[~/ansible/quota]
└─$
```

当前只有一个默认的以本地存储做的一个 SC，我们用来 Demo。

```
┌──[root@vms81.liruilongs.github.io]-[~/ansible/quota]
└─$kubectl apply  -f my-quota-sc.yaml
resourcequota/object-quota-sc-demo created
┌──[root@vms81.liruilongs.github.io]-[~/ansible/quota]
└─$kubectl describe  resourcequotas object-quota-sc-demo
Name:                                                          object-quota-sc-demo
Namespace:                                                     liruilong-topo-namespace
Resource                                                       Used  Hard
--------                                                       ----  ----
local-path.storageclass.storage.k8s.io/persistentvolumeclaims  0     3
local-path.storageclass.storage.k8s.io/requests.storage        0     10Gi
persistentvolumeclaims                                         0     50Gi
requests.storage                                               0     20Gi
┌──[root@vms81.liruilongs.github.io]-[~/ansible/quota]
└─$
```

具体可以限制的 存储资源配额

```
┌──[root@vms81.liruilongs.github.io]-[~/ansible/quota]
└─$cat  my-quota-sc.yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: object-quota-sc-demo
spec:
  hard:
    requests.storage: &quot;20Gi&quot;     #所有 PVC，存储资源的需求总量不能超过该值。
    persistentvolumeclaims: &quot;50Gi&quot;  #在该命名空间中所允许的 PVC 总量。
    local-path.storageclass.storage.k8s.io/requests.storage: &quot;10Gi&quot;  #在所有与 &lt;storage-class-name&gt; 相关的持久卷申领中，存储请求的总和不能超过该值 。
    local-path.storageclass.storage.k8s.io/persistentvolumeclaims: 3  #在与 storage-class-name 相关的所有持久卷申领中，命名空间中可以存在的持久卷申领总数
┌──[root@vms81.liruilongs.github.io]-[~/ansible/quota]
└─$
```

### ** 对象数量配额 **

可以使用以下语法对所有标准的、命名空间域的资源类型进行配额设置：

+   count/.：用于非核心（core）组的资源

+   count/：用于核心组的资源

```
kubectl create quota test --hard=count/deployments.apps=2,count/replicasets.apps=4,count/pods=3,count/secrets=4 --namespace=myspace
```

也可以直接通过 yaml 资源文件的方式配置

```
┌──[root@vms81.liruilongs.github.io]-[~/ansible/quota]
└─$kubectl apply  -f my-quota-objects.yaml
resourcequota/object-quota-count-demo created
┌──[root@vms81.liruilongs.github.io]-[~/ansible/quota]
└─$kubectl describe   resourcequotas object-quota-count-demo
Name:                   object-quota-count-demo
Namespace:              liruilong-topo-namespace
Resource                Used  Hard
--------                ----  ----
configmaps              1     10
persistentvolumeclaims  0     5
pods                    4     15
replicationcontrollers  0     10
resourcequotas          3     5
secrets                 4     7
services                0     5
services.loadbalancers  0     5
services.nodeports      0     5
```

对象数量配额对应的 yaml 文件

```
┌──[root@vms81.liruilongs.github.io]-[~/ansible/quota]
└─$cat my-quota-objects.yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: object-quota-count-demo
spec:
  hard:
    configmaps: 10  #在该命名空间中允许存在的 ConfigMap 总数上限。
    persistentvolumeclaims: 5  #在该命名空间中允许存在的 PVC 的总数上限。
    pods: 15    #在该命名空间中允许存在的非终止状态的 Pod 总数上限。Pod 终止状态等价于 Pod 的 .status.phase in (Failed, Succeeded) 为真。
    replicationcontrollers: 10  #在该命名空间中允许存在的 ReplicationController 总数上限。
    resourcequotas: 5  #在该命名空间中允许存在的 ResourceQuota 总数上限。
    services: 5        #在该命名空间中允许存在的 Service 总数上限。
    services.loadbalancers: 5  #在该命名空间中允许存在的 LoadBalancer 类型的 Service 总数上限。
    services.nodeports: 5      #在该命名空间中允许存在的 NodePort 类型的 Service 总数上限。
    secrets: 7  #在该命名空间中允许存在的 Secret 总数上限。
```

资源配额将整个集群中的资源总量做了一个静态划分，但它并没有对集群中的节点做任何限制：不同命名空间中的 Pod 仍然可以运行在同一个节点上，如果期望 pod 均匀的分布到不同的 工作节点，需要考虑使用 pod 的拓扑分布约束来实现。

### ** 配额作用域 **

上面的配额配置中，默认情况下对整个命名空间有效，实际上每个配额都有一组相关的 scope（作用域），配额只会对作用域内的资源生效。配额机制仅统计所列举的作用域的交集中的资源用量。

当一个作用域被添加到配额中后，它会对作用域相关的资源数量作限制。如配额中指定了允许（作用域）集合之外的资源，会导致验证错误。

+   Terminating ：匹配所有 spec.activeDeadlineSeconds 不小于 0 的 Pod。

+   NotTerminating :匹配所有 spec.activeDeadlineSeconds 是 nil 的 Pod。

+   BestEffort : 匹配所有 Qos 是 BestEffort 的 Pod。

+   NotBestEffort : 匹配所有 Qos 不是 BestEffort 的 Pod。

+   PriorityClass :匹配所有引用了所指定的优先级类的 Pods。

activeDeadlineSeconds 表示 Pod 可以运行的最长时间，达到设置的该值后，Pod 会自动停止， BestEffort 表示 Qos 的三个等级中等级最低的一个，尽力而为的，不太可靠的，

```
┌──[root@vms81.liruilongs.github.io]-[~/ansible/quota]
└─$kubectl apply  -f my-quota-objects-scop.yaml
resourcequota/object-quota-count-scop-demo created
┌──[root@vms81.liruilongs.github.io]-[~/ansible/quota]
└─$kubectl describe  resourcequotas  object-quota-count-scop-demo
Name:       object-quota-count-scop-demo
Namespace:  liruilong-topo-namespace
Resource    Used  Hard
--------    ----  ----
pods        2     15
┌──[root@vms81.liruilongs.github.io]-[~/ansible/quota]
└─$cat  my-quota-objects-scop.yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: object-quota-count-scop-demo
spec:
  hard:
    pods: 15    #在该命名空间中允许存在的非终止状态的 Pod 总数上限。Pod 终止状态等价于 Pod 的 .status.phase in (Failed, Succeeded) 为真。
  scopeSelector:
    matchExpressions:
    - operator: Exists
      scopeName: BestEffort

┌──[root@vms81.liruilongs.github.io]-[~/ansible/quota]
└─$
```

## ** 博文参考**

---

https://kubernetes.io/zh-cn/docs/concepts/policy/resource-quotas/

© 2018-2023 liruilonger@gmail.com, All rights reserved. 保持署名-非商用-自由转载-相同方式共享(创意共享3.0许可证)