
# 打开云原生大门：了解Linux命名空间的奥秘和Docker容器隔离技术

> 来源：[https://geekbang.feishu.cn/docx/Gxz5dhlLsohv4mx6Lxzca1yEned](https://geekbang.feishu.cn/docx/Gxz5dhlLsohv4mx6Lxzca1yEned)

> 👨🏻 作者：Lion莱恩呀

> 📃 作者介绍：专注分享高性能服务器后台开发知识，包括C/C++、Linux、网络协议、设计模式、中间件、云原生、数据库、分布式架构等领域知识。

> 🛖 👸号：Lion莱恩呀

> ⏰ 收录时间：2024-05-27 11:15:56 (UTC+8)

> 🔗 原文链接：h[ttps://mp.weixin.q](https://mp.weixin.qq.com/s?__biz=Mzk0ODUzNDc2Mw==&mid=2247485230&idx=1&sn=058025df5a21f27eab43fabff0d9cd68&scene=19#wechat_redirect)q.com/s?__biz=Mz...

![](img/17d5f98834ee000de6156a1d397c3838.png)

关注作者，获取更多相关知识

## 一、根目录RootFs概述

`rootfs` 是Docker 容器在启动时内部进程可见的文件系统，即Docker容器的根目录。`rootfs`通常包含一个操作系统运行所需的文件系统，例如可能包含经典的类Unix操作系统中的目录系统，如`/dev`、`/proc`、`/bin`、`/etc`、`/lib`、`/usr`、`/tmp`及运行Docker容器所需的配置文件、工具等。

就像每个进程都有自己的根目录：

```
fly@fly:~$ ls /
bin   cdrom  etc   lib    lib64   lost+found  mnt  proc  run   snap  swap.img  tmp  var
boot  dev    home  lib32  libx32  media       opt  root  sbin  srv   sys       usr
fly@fly:~$ cd /proc/
fly@fly:/proc$ ls
1     1172  133   201  221  241  261  293  36   741  871  98           irq           sched_debug
10    118   134   202  222  242  262  294  380  742  872  99           kallsyms      schedstat
100   119   135   203  223  243  263  295  4    743  874  acpi         kcore         scsi
101   12    136   204  224  244  264  296  424  757  877  asound       keys          self
102   120   1369  205  225  245  266  297  425  767  879  buddyinfo    key-users     slabinfo
103   121   1385  206  226  246  268  298  498  768  88   bus          kmsg          softirqs
104   122   1388  207  227  247  27   299  5    79   883  cgroups      kpagecgroup   stat
105   123   14    208  228  248  270  3    517  8    890  cmdline      kpagecount    swaps
106   124   145   209  229  249  272  30   522  80   9    consoles     kpageflags    sys
107   125   148   21   23   25   274  300  524  81   90   cpuinfo      loadavg       sysrq-trigger
108   126   15    210  230  250  276  301  529  82   906  crypto       locks         sysvipc
109   127   1510  211  231  251  278  302  559  83   907  devices      mdstat        thread-self
11    128   1511  212  232  252  28   303  561  84   908  diskstats    meminfo       timer_list
110   129   1568  213  233  253  280  304  6    840  91   dma          misc          tty
111   13    16    214  234  254  282  31   7    842  93   driver       modules       uptime
112   130   161   215  235  255  284  310  720  85   938  execdomains  mounts        version
113   131   17    216  236  256  286  32   721  855  94   fb           mpt           version_signature
114   132   18    217  237  257  288  324  722  86   95   filesystems  mtrr          vmallocinfo
115   1325  19    218  238  258  29   325  723  860  955  fs           net           vmstat
1150  1326  2     219  239  259  290  326  724  861  96   interrupts   pagetypeinfo  zoneinfo
116   1327  20    22   24   26   291  327  733  87   960  iomem        partitions
117   1328  200   220  240  260  292  354  739  870  97   ioports      pressure
fly@fly:/proc$ cd 110
fly@fly:/proc/110$ ls
ls: cannot read symbolic link 'cwd': Permission denied
ls: cannot read symbolic link 'root': Permission denied
ls: cannot read symbolic link 'exe': Permission denied
arch_status  comm             fdinfo     mem         oom_adj        root          stack    timerslack_ns
attr         coredump_filter  gid_map    mountinfo   oom_score      sched         stat     uid_map
autogroup    cpuset           io         mounts      oom_score_adj  schedstat     statm    wchan
auxv         cwd              limits     mountstats  pagemap        sessionid     status
cgroup       environ          loginuid   net         patch_state    setgroups     syscall
clear_refs   exe              map_files  ns          personality    smaps         task
cmdline      fd               maps       numa_maps   projid_map     smaps_rollup  timers
fly@fly:/proc/110$ sudo ls root
[sudo] password for fly:
bin   cdrom  etc   lib          lib64   lost+found  mnt  proc  run   snap  swap.img  tmp  var
boot  dev    home  lib32  libx32  media       opt  root  sbin  srv   sys       usr
```

进程的运行依赖于根文件系统。

## 二、Linux Namespace

Namespace是 Linux 内核用来隔离内核资源的方式。Linux实现了七种不同类型的命名空间。每个命名空间的用途是将特定的全局系统资源包装在抽象中，使命名空间中的进程看起来它们具有自己的全局资源独立实例。命名空间的总体目标之一是支持容器的实现。

| Namespace | 隔离内容 |
| --- | --- |
| `Mount` | 文件系统挂载点 |
| `IPC` | 进程间通信资源，即系统VIPC对象和POSIX消息队列 |
| `PID` | 进程ID |
| `Network` | 网络设备、IP 地址、IP 路由表、`/proc/net`目录、端口号 |
| `UTS` | 主机名与网络信息服务域名 |
| `User` | 用户和用户组 |
| `Cgroup` | `Cgroup`根目录 |

### 2.1、进程命名空间

#### 2.1.1、`lsns` 命令

列出系统命名空间。

```
-p --task＜pid＞
#打印进程命名空间
```

符号说明：

+   `NS`：命名空间标识符（索引节点号）。
+   `TYPE`：命名空间类型。
+   `PATH`：命名空间的`PATH`路径。
+   `NPROCS`：命名空间中的进程数。
+   `PID`：命名空间中的最小`PID`。
+   `PPID`：`PID`的父级`PID`。
+   `COMMAND`：`PID`的命令行。
+   `UID`：`PID`的`UID`。
+   `USER`：`PID`的`User`。
+   `NETNSID`：网络子系统使用的命名空间`ID`。
+   `NSFS`：`nsfs` 文件系统挂载点（通常用于网络子系统）。

进程用到的命名空间（比如110进程的命名空间）：

```
fly@fly:/proc/110$ cd ns/
fly@fly:/proc/110/ns$ sudo ls -al
total 0
dr-x--x--x 2 root root 0 Dec  6 12:54 .
dr-xr-xr-x 9 root root 0 Dec  6 12:52 ..
lrwxrwxrwx 1 root root 0 Dec  6 13:16 cgroup -> 'cgroup:[4026531835]'
lrwxrwxrwx 1 root root 0 Dec  6 13:16 ipc -> 'ipc:[4026531839]'
lrwxrwxrwx 1 root root 0 Dec  6 13:16 mnt -> 'mnt:[4026531840]'
lrwxrwxrwx 1 root root 0 Dec  6 13:16 net -> 'net:[4026531992]'
lrwxrwxrwx 1 root root 0 Dec  6 13:16 pid -> 'pid:[4026531836]'
lrwxrwxrwx 1 root root 0 Dec  6 13:16 pid_for_children -> 'pid:[4026531836]'
lrwxrwxrwx 1 root root 0 Dec  6 13:16 user -> 'user:[4026531837]'
lrwxrwxrwx 1 root root 0 Dec  6 13:16 uts -> 'uts:[4026531838]'
```

使用`lsns`命令就可以查询进程的命名空间，可以看到和上面的命名空间标识符是一致的。

```
fly@fly:/proc/110/ns$ sudo lsns
        NS TYPE   NPROCS   PID USER             COMMAND
4026531835 cgroup    206     1 root             /sbin/init auto automatic-ubiquity noprompt
4026531836 pid       206     1 root             /sbin/init auto automatic-ubiquity noprompt
4026531837 user      206     1 root             /sbin/init auto automatic-ubiquity noprompt
4026531838 uts       203     1 root             /sbin/init auto automatic-ubiquity noprompt
4026531839 ipc       206     1 root             /sbin/init auto automatic-ubiquity noprompt
4026531840 mnt       198     1 root             /sbin/init auto automatic-ubiquity noprompt
4026531860 mnt         1    22 root             kdevtmpfs
4026531992 net       206     1 root             /sbin/init auto automatic-ubiquity noprompt
4026532548 mnt         1   529 root             /lib/systemd/systemd-udevd
4026532549 uts         1   529 root             /lib/systemd/systemd-udevd
4026532625 mnt         1   757 systemd-timesync /lib/systemd/systemd-timesyncd
4026532626 uts         1   757 systemd-timesync /lib/systemd/systemd-timesyncd
4026532627 mnt         1   840 systemd-network  /lib/systemd/systemd-networkd
4026532637 mnt         1   842 systemd-resolve  /lib/systemd/systemd-resolved
4026532693 uts         1   879 root             /lib/systemd/systemd-logind
4026532749 mnt         1   960 root             /usr/sbin/ModemManager
4026532750 mnt         1   870 root             /usr/sbin/irqbalance --foreground
4026532751 mnt         1   879 root             /lib/systemd/systemd-logind
```

`fork`进程时，如果没有指定进程的命名空间，子进程将继承父进程的相关命名空间。

#### 2.1.2、查看元祖进程命名空间

（1）列出系统所有命名空间。

```
sudo lsns --output-all
```
