
```
$ sudo systemctl status cri-docker
● cri-docker.service - CRI Interface for Docker Application Container Engine
     Loaded: loaded (/etc/systemd/system/cri-docker.service; enabled; vendor preset: enabled)
     Active: active (running) since Fri 2023-03-17 06:57:35 UTC; 6s ago
TriggeredBy: ● cri-docker.socket
       Docs: https://docs.mirantis.com
   Main PID: 6200 (cri-dockerd)
      Tasks: 8
     Memory: 10.9M
     CGroup: /system.slice/cri-docker.service
             └─6200 /usr/bin/cri-dockerd --container-runtime-endpoint fd:// --network-plugin=cni --pod-infra-container-image==registry.al>

Mar 17 06:57:35 fly cri-dockerd[6200]: time="2023-03-17T06:57:35Z" level=info msg="The binary conntrack is not installed, this can cause >
Mar 17 06:57:35 fly cri-dockerd[6200]: time="2023-03-17T06:57:35Z" level=info msg="The binary conntrack is not installed, this can cause >
Mar 17 06:57:35 fly cri-dockerd[6200]: time="2023-03-17T06:57:35Z" level=info msg="Loaded network plugin cni"
Mar 17 06:57:35 fly cri-dockerd[6200]: time="2023-03-17T06:57:35Z" level=info msg="Docker cri networking managed by network plugin cni"
Mar 17 06:57:35 fly cri-dockerd[6200]: time="2023-03-17T06:57:35Z" level=info msg="Docker Info: &{ID:VGW7:H3EQ:B56E:TDMW:2RJV:4PUH:LVB3:U>
Mar 17 06:57:35 fly cri-dockerd[6200]: time="2023-03-17T06:57:35Z" level=info msg="Setting cgroupDriver cgroupfs"
Mar 17 06:57:35 fly cri-dockerd[6200]: time="2023-03-17T06:57:35Z" level=info msg="Docker cri received runtime config &RuntimeConfig{Netw>
Mar 17 06:57:35 fly cri-dockerd[6200]: time="2023-03-17T06:57:35Z" level=info msg="Starting the GRPC backend for the Docker CRI interface>
Mar 17 06:57:35 fly cri-dockerd[6200]: time="2023-03-17T06:57:35Z" level=info msg="Start cri-dockerd grpc backend"
Mar 17 06:57:35 fly systemd[1]: Started CRI Interface for Docker Application Container Engine.
lines 1-21/21 (END)...skipping...
● cri-docker.service - CRI Interface for Docker Application Container Engine
     Loaded: loaded (/etc/systemd/system/cri-docker.service; enabled; vendor preset: enabled)
     Active: active (running) since Fri 2023-03-17 06:57:35 UTC; 6s ago
TriggeredBy: ● cri-docker.socket
       Docs: https://docs.mirantis.com
   Main PID: 6200 (cri-dockerd)
      Tasks: 8
     Memory: 10.9M
     CGroup: /system.slice/cri-docker.service
             └─6200 /usr/bin/cri-dockerd --container-runtime-endpoint fd:// --network-plugin=cni --pod-infra-container-image==registry.aliyuncs.com/google_containers/pause:3.7

Mar 17 06:57:35 fly cri-dockerd[6200]: time="2023-03-17T06:57:35Z" level=info msg="The binary conntrack is not installed, this can cause failures in network connection cleanup."
Mar 17 06:57:35 fly cri-dockerd[6200]: time="2023-03-17T06:57:35Z" level=info msg="The binary conntrack is not installed, this can cause failures in network connection cleanup."
Mar 17 06:57:35 fly cri-dockerd[6200]: time="2023-03-17T06:57:35Z" level=info msg="Loaded network plugin cni"
Mar 17 06:57:35 fly cri-dockerd[6200]: time="2023-03-17T06:57:35Z" level=info msg="Docker cri networking managed by network plugin cni"
Mar 17 06:57:35 fly cri-dockerd[6200]: time="2023-03-17T06:57:35Z" level=info msg="Docker Info: &{ID:VGW7:H3EQ:B56E:TDMW:2RJV:4PUH:LVB3:U2ER:5OVK:YTTO:A6ZQ:EDGS Containers:0 ContainersRunning:0 ContainersPaused:0 ContainersStopped:0 Images:>
Mar 17 06:57:35 fly cri-dockerd[6200]: time="2023-03-17T06:57:35Z" level=info msg="Setting cgroupDriver cgroupfs"
Mar 17 06:57:35 fly cri-dockerd[6200]: time="2023-03-17T06:57:35Z" level=info msg="Docker cri received runtime config &RuntimeConfig{NetworkConfig:&NetworkConfig{PodCidr:,},}"
Mar 17 06:57:35 fly cri-dockerd[6200]: time="2023-03-17T06:57:35Z" level=info msg="Starting the GRPC backend for the Docker CRI interface."
Mar 17 06:57:35 fly cri-dockerd[6200]: time="2023-03-17T06:57:35Z" level=info msg="Start cri-dockerd grpc backend"
Mar 17 06:57:35 fly systemd[1]: Started CRI Interface for Docker Application Container Engine.
```

# 三、`kubeadm`和相关工具

![](img/0a25cf785a2ade0ba0d062e2fecbab17.png)

`kubeadm`是`Kubernetes`从1.4版本开始引入的命令行工具，致力于简化集群的安装过程，并解决`Kubernetes`集群高可用问题。

1. 源码地址。
1. 安装说明文档地址。

## 3.1、安装`kubeadm`、`kubelet`、`kubectl`：

（1）更新包管理器索引，并安装相关软件支持`HTTPS`访问。

```
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl
```

（2）下载`gpg`秘钥。

```
sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://mirrors.aliyun.com/kubernetes/apt/doc/apt-key.gpg
```

（3）设置`Kubernetes`镜像源。

```
echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://mirrors.aliyun.com/kubernetes/apt/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
```

（4）更新`apt`软件索引，并查看相关软件的可用版本。

```
sudo apt-get update
apt-cache madison kubelet kubeadm kubectl
```

![](img/1188cd84796428f5fdce6a6bd1e6dab7.png)

（5）安装特定版本。

```
sudo apt-get install -y kubelet=<VERSION_STRING> kubeadm=<VERSION_STRING> kubectl=<VERSION_STRING>
```

例如：

```
sudo apt-get install -y kubelet=1.24.1-00 kubeadm=1.24.1-00 kubectl=1.24.1-00
```

（6）安装最新版本：

```
sudo apt-get install -y kubelet kubeadm kubectl
```

（7）锁定软件版本：

```
# 防止自动升级更新
sudo apt-mark hold kubelet kubeadm kubectl
```

（8）检查`kubelet`状态：

```
systemctl status kubelet
```

注意：`kubelet`在刚安装完成时，会处于一个自动启动状态，每10s启动一次，在没有完成初始化之前它一致处于这种状态，所以不要纠结于`kubelet`安装之后没有启动。

## 3.2、卸载

```
sudo apt-get remove kubelet kubectl kubeadm
```

# 四、主机环境调整

## 4.1、修改节点上`Docker` 的`cgroup driver`为`systemd`

由于`Kubernetes`中`kubelet`组件默认使用的`cgroupdriver`为 `systemd`，所以我们将`docker`的`cgroupdriver`也修改为 `systemd`。

参考文档：

+   `configure-cgroup-driver`

+   `container-runtimes`

（1）打开`docker`配置文件进行修改，该文件默认情况下不存在，可以新建一个，不同操作系统的文件位置不一样。

```
sudo vim /etc/docker/daemon.json
```

（2）增加配置内容：

```
"exec-opts": [
        "native.cgroupdriver=systemd"
]
```

（3）重新加载配置并重启服务：

```
sudo systemctl daemon-reload
sudo systemctl restart docker
```

（4）最终配置文件内容如下：

```
{
        "exec-opts": [
                        "native.cgroupdriver=systemd"
        ]
}
```

## 4.2、关闭防火墙

```
sudo systemctl stop firewalld
sudo systemctl disable firewalld
```

## 4.3、禁用`selinux`

`selinux`，这个是用来加强安全性的一个组件，但非常容易出错且难以定位，一般上来装完系统就先给禁用了。

```
# 查看 selinux 状态
sudo apt install selinux-utils
getenforce
# 禁用
sudo setenforce 0
```

## 4.4、禁用`swap`

```
# 查看交换区
free
# 禁用交换区
sudo swapoff -a
# 打开文件注释交换区定义
sudo vim /etc/fstab
```

注释`/swap.img`一行：

```
# /etc/fstab: static file system information.
# 
# Use 'blkid' to print the universally unique identifier for a
# device; this may be used with UUID= as a more robust way to name devices
# that works even if disks are added and removed. See fstab(5).
#
# <file system> <mount point>   <type>  <options>       <dump>  <pass>
# / was on /dev/ubuntu-vg/ubuntu-lv during curtin installation
/dev/disk/by-id/dm-uuid-LVM-P6zL7vPKbBTRUnirliIyM4zbR4bNr6fERoQHPjV6FsHDBNZpidJ6kb98rN02CCYB / ext4 defaults 0 1
# /boot was on /dev/sda2 during curtin installation
/dev/disk/by-uuid/78bed4d5-bb1b-4723-a5f1-f908860f7603 /boot ext4 defaults 0 1
# /swap.img     none    swap    sw      0       0
```

查看交换区是否关闭：

```
fly@fly:~$ free
              total        used        free      shared  buff/cache   available
Mem:        3994704      373356     2904464        1604      716884     3379752
Swap:             0           0           0
```

## 4.5、修改主机名

（1）`/etc/hosts`文件增加主机名与本机ip映射：`127.0.1.1` `k8s-master1`。
