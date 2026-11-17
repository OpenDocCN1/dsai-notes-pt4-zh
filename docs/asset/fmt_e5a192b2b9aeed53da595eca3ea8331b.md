
1. 需要使用`Headless Service`来负责`Pod`的网络标识，因此需要创建`Headless Service`。

1. 删除`StatefulSet`时，不能保证删除它管理的`Pod`。可以通过调整副本数量为0来实现。

常见的哪些应用程序是有状态的，例如：MySQL集群、MongoDB集群、kafka集群等。

## 3.10、DaemonSet

`DaemonSet` 确保全部（或者某些）节点上运行一个 `Pod` 的副本。当有节点加入集群时， 也会为他们新增一个 `Pod` 。当有节点从集群移除时，这些 `Pod` 也会被回收。删除 `DaemonSet` 将会删除它创建的所有 `Pod`。

`DaemonSet` 的一些典型用法：

+ 在每个节点上运行集群守护进程

+ 在每个节点上运行日志收集守护进程

+ 在每个节点上运行监控守护进程

一种简单的用法是为每种类型的守护进程在所有的节点上都启动一个 `DaemonSet`。一个稍微复杂的用法是为同一种守护进程部署多个 `DaemonSet`；每个具有不同的标志， 并且对不同硬件类型具有不同的内存、CPU 要求。

## 3.11、Service（重要概念）

`Service`服务也是Kubernetes里的核心资源对象之一，其主要作用是将运行在一组`Pods`上的应用程序公开为网络服务，这其实就是我们经常提起的微服务。通过`service`资源对象定义一个`service`的访问入口地址。前端应用通过访问这个入口，从而访问其背后的一组有`Pod`副本组成的集群实例。`service`所针对的目标`Pods`集合通常通过`Label Selector`来确定，如下图：

![](img/1907b2bd94db3678e678d9cfde59eeb0.png)

`Service` 一旦被定义，就被分配了一个不可变更的`Cluster IP`，在整个`Service`的生命周期内，该IP地址都不会发生改变。

## 3.12、Job

`Job`即工作任务，`Job` 会创建一个或者多个 `Pods`，来执行工作任务。`Job`会跟踪记录成功完成的`Pod`数量，当成功完成的数量达到了指定的成功个数时，`Job`结束。当执行过程中`Pod`出现失败的情况，`Job`会创建新的`Pod`来替代该`Pod`。删除 `Job` 的操作会清除所创建的全部 `Pods`。挂起 `Job` 的操作会删除 `Job`的所有活跃 `Pod`，直到 `Job` 被再次恢复执行。`Job`通常为单次任务，如果需要运行定时`Job`则应该使用`CronJob`。

## 3.13、Volume

`Volume` 是k8s抽象出来的对象，用于解决`Pod`中的容器运行时，文件存放的问题以及多容器数据共享的问题。`Pod` 可以同时使用任意数量的`Volume`类型。临时卷类型的生命周期与 `Pod` 相同，但持久卷的生命周期与`Pod`无关。当 `Pod` 不再存在时，Kubernetes 会销毁临时卷；不过 Kubernetes 不会销毁持久卷。对于给定 `Pod` 中任何类型的卷，在容器重启期间数据都不会丢失，也就是说数据卷的生命周期与容器无关。`Volume`的核心是一个目录，其中可能存有数据，`Pod` 中的容器可以访问该目录中的数据。所采用的特定的`Volume`类型将决定该目录如何形成的、使用何种介质保存数据以及目录中存放的内容。

`Volume` 支持多种`Volume`类型，例如：`cephfs`、`configMap`、`csi`、`downwardAPI`、`emptyDir`、`fc(fibre channel)`、`gcePersistentDisk`、`glusterfs`、`hostPath`、`iscsi`、`local`、`nfs`、`persistentVolumeClaim`、`projected`、`secret`、`rbd`等。

`Volume`的使用比较简单，通常情况下，我们在`Pod`上声明一个`Volume`,然后在容器里引用该`Volume`并挂载到容器的某个目录上即可，例如：

```
template：
  metadata：
        labels:
          app: myapp
  spec:
        volumes:
          - name: datavol
                emptyDir: {}
        containers:
        - name: nginx
          image: nginx
          volumeMounts:
                - mountPath: /mydata
                  name: datavol
```

## 3.14、Persistent Volume

持久卷（`PersistentVolume`，`PV`）是集群中的一块存储，可以由管理员事先供应，或者 使用存储类（`Storage Class`）来动态供应。持久卷是集群资源，就像节点也是集群资源一样。`PV` 持久卷和普通的`Volume` 一样，也是使用`Volume`插件来实现的，只是它们拥有独立的生命周期。

`Pod`通过`PersistentVolumeClaim`（`PVC`）来申领`PV`作为存储卷使用。集群会通过`PVC`找到其绑定的`PV`，并将该`PV`挂载到`Pod`。

下面是一个`NFS`类型的`PV`的一个YAML定义文件，声明了8Gi的存储空间：

```
apiVersion: v1
kind: PersistentVolume
metadata:
  name:pv0001
spec:
  capacity:
        storage: 8Gi
  accessModes:
        - ReadWriteOnce
  nfs:
        path: /somepath
        server: 192.168.0.106
```

`PV`的`accessModes`属性：

1. `ReadWriteOnce`：读写权限，只允许被单个`Node`挂载。

1. `ReadOnlyMany`：只读权限，允许被多个`Node`挂载。

1. `ReadWriteMany`：读写权限，允许被多个`Node`挂载。

如果某个`Pod`需要申请某种类型的`PV`，则需要先定义一个`PersistentVolumeClaim`对象：

```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: myclaim
spec:
  accessModes:
        - ReadWriteOnce
  resources:
        requests:
          storage: 8Gi
```

然后，在`Pod`的`Volume`定义中引用`PVC`即可：

```
volumes:
  - name: mypd
        persistentVolumeClaim:
          claimName: myclaim
```

最后说说`PV`的状态；`PV`的状态包括以下几种：

1. `Available`：空闲状态。

1. `Bound`：已经绑定到某个`PVC`上。

1. `Released`：对应的`PVC`已经被删除，但资源还没有被集群回收。

1. `Failed`：`PV`自动回收失败。

## 3.15、Namespace（重要的概念）

`Namespace`（命名空间）是Kubernetes系统中的另一个非常重要的概念，主要提供资源隔离。通过命名空间可以将同一个集群中的资源划分为相互隔离的组。统一命名空间下的资源名称必须唯一，但是不同命名空间下的资源名称可以一样。命名空间的作用仅针对那些带有命名空间的资源对象，例如：

`Deployment`，`service`，`pod`，`rc`等。对集群对象不适用，例如：`Node`，`namespace`，`PV`等。

## 3.16、Annotation

`Annotation`（注解）与`Label`类似，也是使用`key/value`键值对的形式进行定义。不同的是`Label` 义的是Kubernetes对象的元数据（`metadata`），并且用于`Label Selector`。`Annotation` 则是用户任意定义的附加信息，以便于外部工具查找。

## 3.17、ConfigMap

`ConfigMap`用于将其他资源对象所需要使用的非机密配置项的数据保存在键值对中。

例如：应用程序的配置文件。如此一来，就可以集中管理集群所使用的所有配置项。

## 3.18、Secret

`Secret`与`ConfigMap`类似，但是`Secret`是专门用来存储机密数据的。

# 总结

1. `Pod`是Kubernetes非常重要的概念，就像容器对于`docker`的重要性；`Pod`和容器的区别：一个`Pod`可以包含多个容器，一个`Pod`一定有一个根容器，网络和数据卷都是挂在在根容器下。

1. `Label`类似🛰️朋友里的标签，用于资源管理的，在`kubernetes`中做资源管理。比如`service`里的`Label`可作为负载均衡的一个依据。

1. `RC`（`replication controller`）已不推荐使用，可以使用`deployment`进行部署，比`RC`更具优势。

1. `replica set`（简称`RS`）是一个升级版的`RC`，但是通常不会显式的使用它，由`kubernetes`系统自动使用它。

1. `deployment`用于部署无状态应用；有状态应用使用`StatefulSet`部署。

欢迎关注我的👸号

每日推送文章

![](img/a44d61b544a5a48e449204ba1a0551a0.png)

长按上图，识别图中二维码即可关注
