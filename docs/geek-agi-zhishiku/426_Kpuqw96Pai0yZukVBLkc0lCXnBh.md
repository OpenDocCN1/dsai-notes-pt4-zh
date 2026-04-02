# K8s专题(二)：Kubernetes架构与组件

> 来源：[https://geekbang.feishu.cn/docx/D9NIdeYSdogOocxPix4cqoAMnYf](https://geekbang.feishu.cn/docx/D9NIdeYSdogOocxPix4cqoAMnYf)

> 👨🏻 作者：尼尔

> 📃 作者介绍：热爱技术的技术人

> 🛖 公众号：螺竹编程

> ⏰ 收录时间：2024-05-27 11:15:56 (UTC+8)

> 🔗 原文链接：h[ttps://mp.weixin.q](https://mp.weixin.qq.com/s/wefXQffbW8mN5tYFFOOcfA)q.com/s/wefXQffb...

![](img/631871261f88569dc60e5b98777db95a.png)

关注作者，获取更多相关知识

**Kubernetes集群架构**

1

![](img/4b0017efe269a9fdeceed36663447b9c.png)

图片来自Kubernetes官方文档

Kubernetes集群主要由Master节点和Worker节点组成，Master 节点是 Kubernetes 集群的控制平面,负责管理和控制整个集群的状态，Worker 节点是运行用户应用程序的节点。这种架构使 Kubernetes 能够提供高可用、可扩展、自我修复的容器编排能力。

Kubernetes集群中的组件

2

在Master节点上，通常包含以下组件：

1.  API服务器（API Server） ：API服务器是Kubernetes集群的中央控制平面。它提供了与集群进行通信的接口，处理来自用户和其他组件的API请求，并管理集群的状态。

1.  **调度器（Scheduler） **：调度器负责将新创建的Pod分配到工作节点上。它根据资源需求、亲和性规则和其他策略，选择最佳的节点来运行Pod。

1.  控制器管理器（Controller Manager） ：控制器管理器是一组控制器的集合，用于维护集群中各种资源的状态。每个控制器负责确保所控制的资源与期望状态保持一致。例如，副本控制器（Replication Controller）用于管理Pod副本的数量。

在Worker节点上，通常包含以下组件：

1.  **kubelet **：kubelet是每个工作节点上的代理组件，负责与Master节点通信，并管理该节点上的Pod。它接收来自API服务器的指令，确保Pod按照规范运行，并报告节点上的状态。

1.  **Pod **：Pod是Kubernetes中最小的可部署单元，代表一个或多个容器的集合。Pod由kubelet负责创建、启动、监控和销毁。

1.  容器运行时（Container Runtime） ：容器运行时是负责运行容器的软件。常见的容器运行时包括Docker、containerd、CRI-O等。它负责加载和执行容器镜像，并提供容器的隔离环境。

1.  **kube-proxy **：kube-proxy是在每个工作节点上运行的网络代理组件。它负责为Pod提供网络代理和负载均衡功能，以实现集群内部和外部的网络连接。