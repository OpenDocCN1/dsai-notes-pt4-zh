# 打开云原生大门：了解Linux命名空间的奥秘和Docker容器隔离技术

> 来源：[https://geekbang.feishu.cn/docx/Gxz5dhlLsohv4mx6Lxzca1yEned](https://geekbang.feishu.cn/docx/Gxz5dhlLsohv4mx6Lxzca1yEned)

> 👨🏻 作者：Lion莱恩呀

> 📃 作者介绍：专注分享高性能服务器后台开发知识，包括C/C++、Linux、网络协议、设计模式、中间件、云原生、数据库、分布式架构等领域知识。

> 🛖 公众号：Lion莱恩呀

> ⏰ 收录时间：2024-05-27 11:15:56 (UTC+8)

> 🔗 原文链接：h[ttps://mp.weixin.q](https://mp.weixin.qq.com/s?__biz=Mzk0ODUzNDc2Mw==&mid=2247485230&idx=1&sn=058025df5a21f27eab43fabff0d9cd68&scene=19#wechat_redirect)q.com/s?__biz=Mz...

![](img/17d5f98834ee000de6156a1d397c3838.png)

关注作者，获取更多相关知识

# 一、根目录RootFs概述

rootfs 是Docker 容器在启动时内部进程可见的文件系统，即Docker容器的根目录。rootfs通常包含一个操作系统运行所需的文件系统，例如可能包含经典的类Unix操作系统中的目录系统，如/dev、/proc、/bin、/etc、/lib、/usr、/tmp及运行Docker容器所需的配置文件、工具等。

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
ls: cannot read symbolic link &#x27;cwd&#x27;: Permission denied
ls: cannot read symbolic link &#x27;root&#x27;: Permission denied
ls: cannot read symbolic link &#x27;exe&#x27;: Permission denied
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

# 二、Linux Namespace

Namespace是 Linux 内核用来隔离内核资源的方式。Linux实现了七种不同类型的命名空间。每个命名空间的用途是将特定的全局系统资源包装在抽象中，使命名空间中的进程看起来它们具有自己的全局资源独立实例。命名空间的总体目标之一是支持容器的实现。

| Namespace | 隔离内容 |
| --- | --- |
| Mount | 文件系统挂载点 |
| IPC | 进程间通信资源，即系统VIPC对象和POSIX消息队列 |
| PID | 进程ID |
| Network | 网络设备、IP 地址、IP 路由表、/proc/net目录、端口号 |
| UTS | 主机名与网络信息服务域名 |
| User | 用户和用户组 |
| Cgroup | Cgroup根目录 |

## 2.1、进程命名空间

### 2.1.1、lsns 命令

列出系统命名空间。

```
-p --task＜pid＞
#打印进程命名空间
```

符号说明：

+   NS：命名空间标识符（索引节点号）。

+   TYPE：命名空间类型。

+   PATH：命名空间的PATH路径。

+   NPROCS：命名空间中的进程数。

+   PID：命名空间中的最小PID。

+   PPID：PID的父级PID。

+   COMMAND：PID的命令行。

+   UID：PID的UID。

+   USER：PID的User。

+   NETNSID：网络子系统使用的命名空间ID。

+   NSFS：nsfs 文件系统挂载点（通常用于网络子系统）。

进程用到的命名空间（比如110进程的命名空间）：

```
fly@fly:/proc/110$ cd ns/
fly@fly:/proc/110/ns$ sudo ls -al
total 0
dr-x--x--x 2 root root 0 Dec  6 12:54 .
dr-xr-xr-x 9 root root 0 Dec  6 12:52 ..
lrwxrwxrwx 1 root root 0 Dec  6 13:16 cgroup -&gt; &#x27;cgroup:[4026531835]&#x27;
lrwxrwxrwx 1 root root 0 Dec  6 13:16 ipc -&gt; &#x27;ipc:[4026531839]&#x27;
lrwxrwxrwx 1 root root 0 Dec  6 13:16 mnt -&gt; &#x27;mnt:[4026531840]&#x27;
lrwxrwxrwx 1 root root 0 Dec  6 13:16 net -&gt; &#x27;net:[4026531992]&#x27;
lrwxrwxrwx 1 root root 0 Dec  6 13:16 pid -&gt; &#x27;pid:[4026531836]&#x27;
lrwxrwxrwx 1 root root 0 Dec  6 13:16 pid_for_children -&gt; &#x27;pid:[4026531836]&#x27;
lrwxrwxrwx 1 root root 0 Dec  6 13:16 user -&gt; &#x27;user:[4026531837]&#x27;
lrwxrwxrwx 1 root root 0 Dec  6 13:16 uts -&gt; &#x27;uts:[4026531838]&#x27;

```

使用lsns命令就可以查询进程的命名空间，可以看到和上面的命名空间标识符是一致的。

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

fork进程时，如果没有指定进程的命名空间，子进程将继承父进程的相关命名空间。

### 2.1.2、查看元祖进程命名空间

（1）列出系统所有命名空间。

```
sudo lsns --output-all
```

```
$ sudo lsns --output-all 
        NS TYPE   PATH              NPROCS   PID PPID COMMAND        UID USER                NETNSID NSFS
4026531835 cgroup /proc/1/ns/cgroup    206     1    0 /sbin/init aut   0 root                        
4026531836 pid    /proc/1/ns/pid       206     1    0 /sbin/init aut   0 root                        
4026531837 user   /proc/1/ns/user      206     1    0 /sbin/init aut   0 root                        
4026531838 uts    /proc/1/ns/uts       203     1    0 /sbin/init aut   0 root                        
4026531839 ipc    /proc/1/ns/ipc       206     1    0 /sbin/init aut   0 root                        
4026531840 mnt    /proc/1/ns/mnt       198     1    0 /sbin/init aut   0 root                        
4026531860 mnt    /proc/22/ns/mnt        1    22    2 kdevtmpfs        0 root                        
4026531992 net    /proc/1/ns/net       206     1    0 /sbin/init aut   0 root             unassigned 
4026532548 mnt    /proc/529/ns/mnt       1   529    1 /lib/systemd/s   0 root                        
4026532549 uts    /proc/529/ns/uts       1   529    1 /lib/systemd/s   0 root                        
4026532625 mnt    /proc/757/ns/mnt       1   757    1 /lib/systemd/s 102 systemd-timesync            
4026532626 uts    /proc/757/ns/uts       1   757    1 /lib/systemd/s 102 systemd-timesync            
4026532627 mnt    /proc/840/ns/mnt       1   840    1 /lib/systemd/s 100 systemd-network             
4026532637 mnt    /proc/842/ns/mnt       1   842    1 /lib/systemd/s 101 systemd-resolve             
4026532693 uts    /proc/879/ns/uts       1   879    1 /lib/systemd/s   0 root                        
4026532749 mnt    /proc/960/ns/mnt       1   960    1 /usr/sbin/Mode   0 root                        
4026532750 mnt    /proc/870/ns/mnt       1   870    1 /usr/sbin/irqb   0 root                        
4026532751 mnt    /proc/879/ns/mnt       1   879    1 /lib/systemd/s   0 root   
```

上面的结果中=命名空间所属进程ID（PID）为1，表示元祖进程的命名空间，即系统默认命名空间。进程没有特殊指定需要创建新的命名空间的情况下，命名空间将与父进程保持一致。

（2）通过文件查看元祖进程命名空间。

```
sudo ls -al /proc/1/ns/ --color
```

```
total 0
dr-x--x--x 2 root root 0 Dec  6 12:53 .
dr-xr-xr-x 9 root root 0 Dec  6 12:52 ..
lrwxrwxrwx 1 root root 0 Dec  6 13:17 cgroup -&gt; &#x27;cgroup:[4026531835]&#x27;
lrwxrwxrwx 1 root root 0 Dec  6 13:17 ipc -&gt; &#x27;ipc:[4026531839]&#x27;
lrwxrwxrwx 1 root root 0 Dec  6 12:53 mnt -&gt; &#x27;mnt:[4026531840]&#x27;
lrwxrwxrwx 1 root root 0 Dec  6 13:17 net -&gt; &#x27;net:[4026531992]&#x27;
lrwxrwxrwx 1 root root 0 Dec  6 13:17 pid -&gt; &#x27;pid:[4026531836]&#x27;
lrwxrwxrwx 1 root root 0 Dec  6 13:32 pid_for_children -&gt; &#x27;pid:[4026531836]&#x27;
lrwxrwxrwx 1 root root 0 Dec  6 13:17 user -&gt; &#x27;user:[4026531837]&#x27;
lrwxrwxrwx 1 root root 0 Dec  6 13:17 uts -&gt; &#x27;uts:[4026531838]&#x27;

```

### 2.1.3、查看当前用户进程命名空间。

（1）查看当前用户进程命名空间列表。

```
lsns --output-all
```

```

        NS TYPE   PATH                 NPROCS   PID PPID COMMAND                 UID USER    NETNSID NSFS
4026531835 cgroup /proc/1385/ns/cgroup      3  1385    1 /lib/systemd/systemd - 1000 fly             
4026531836 pid    /proc/1385/ns/pid         3  1385    1 /lib/systemd/systemd - 1000 fly             
4026531837 user   /proc/1385/ns/user        3  1385    1 /lib/systemd/systemd - 1000 fly             
4026531838 uts    /proc/1385/ns/uts         3  1385    1 /lib/systemd/systemd - 1000 fly             
4026531839 ipc    /proc/1385/ns/ipc         3  1385    1 /lib/systemd/systemd - 1000 fly             
4026531840 mnt    /proc/1385/ns/mnt         3  1385    1 /lib/systemd/systemd - 1000 fly             
4026531992 net    /proc/1385/ns/net         3  1385    1 /lib/systemd/systemd - 1000 fly  unassigned 

```

**注意，使用sudo是查看系统所有命名空间，不使用**sudo是查看当前用户进程命名空间列表。

（2）fork一个新的进程，并且不共享父进程命名空间。

创建新的进程，使用-u指定新的命名空间，若没有指定-U则需要超级权限：

```
unshare --fork -m -u -i -n -p -U -C sleep 100
```

然后查看所有命名空间。

```
lsns --output-all
```

```
        NS TYPE   PATH                 NPROCS   PID  PPID COMMAND                UID USER    NETNSID NSFS
4026531835 cgroup /proc/1385/ns/cgroup      4  1385     1 /lib/systemd/systemd  1000 fly             
4026531836 pid    /proc/1385/ns/pid         5  1385     1 /lib/systemd/systemd  1000 fly             
4026531837 user   /proc/1385/ns/user        4  1385     1 /lib/systemd/systemd  1000 fly             
4026531838 uts    /proc/1385/ns/uts         4  1385     1 /lib/systemd/systemd  1000 fly             
4026531839 ipc    /proc/1385/ns/ipc         4  1385     1 /lib/systemd/systemd  1000 fly             
4026531840 mnt    /proc/1385/ns/mnt         4  1385     1 /lib/systemd/systemd  1000 fly             
4026531992 net    /proc/1385/ns/net         4  1385     1 /lib/systemd/systemd  1000 fly  unassigned 
4026532639 user   /proc/7027/ns/user        2  7027  1511 unshare --fork -m -u  1000 fly             
4026532640 mnt    /proc/7027/ns/mnt         2  7027  1511 unshare --fork -m -u  1000 fly             
4026532641 uts    /proc/7027/ns/uts         2  7027  1511 unshare --fork -m -u  1000 fly             
4026532642 ipc    /proc/7027/ns/ipc         2  7027  1511 unshare --fork -m -u  1000 fly             
4026532643 pid    /proc/7028/ns/pid         1  7028  7027 sleep 100             1000 fly             
4026532644 cgroup /proc/7027/ns/cgroup      2  7027  1511 unshare --fork -m -u  1000 fly             
4026532646 net    /proc/7027/ns/net         2  7027  1511 unshare --fork -m -u  1000 fly  unassigned 

```

通过NS列可以看出，新进程和元祖进程的命名空间是不一样的。

新fork出来的进程，在指定新命名空间后，其命名空间字段的值与系统默认命名空间不一致，说明进程创建了新的命名空间。

## 2.2、容器进程命名空间

docker容器本身就是一个进程，所以docker容器隔离机制使用的就是进程的隔离机制。

### 2.2.1、查看容器进程命名空间列表

（1） 运行容器。

```
# 启动nginx 容器
# -d 指示后台运行，--name指示容器名称，nginx是镜像
docker run -d --name mynginx nginx
```

```
Unable to find image &#x27;nginx:latest&#x27; locally
latest: Pulling from library/nginx
025c56f98b67: Pull complete 
ca9c7f45d396: Pull complete 
ed6bd111fc08: Pull complete 
e25b13a5f70d: Pull complete 
9bbabac55ab6: Waiting 
9bbabac55ab6: Pull complete 
e5c9ba265ded: Pull complete 
Digest: sha256:ab589a3c466e347b1c0573be23356676df90cd7ce2dbf6ec332a5f0a8b5e59db
Status: Downloaded newer image for nginx:latest
f022bdc00b5adca5cf97866497bc853d764e9f973d306ed73df9e577f4d6eee6

```

（2）获取进程ID，即获取nginx主进程ID。

```
docker top mynginx
```

```
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
root                7583                7553                0                   13:56               ?                   00:00:00            nginx: master process nginx -g daemon off;
systemd+            7638                7583                0                   13:56               ?                   00:00:00            nginx: worker process
systemd+            7639                7583                0                   13:56               ?                   00:00:00            nginx: worker process

```

使用docker ps查看其他信息：

```
docker ps
```

```
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS     NAMES
f022bdc00b5a   nginx     &quot;/docker-entrypoint.…&quot;   2 minutes ago   Up 2 minutes   80/tcp    mynginx
```

（3）查看进程命名空间。

```
sudo lsns -p &lt;pid&gt; --output-all
```

示例：

```
$ sudo lsns -p 7583 --output-all 
        NS TYPE   PATH              NPROCS   PID  PPID COMMAND                                     UID USER NETNSID NSFS
4026531835 cgroup /proc/1/ns/cgroup    213     1     0 /sbin/init auto automatic-ubiquity noprompt   0 root         
4026531837 user   /proc/1/ns/user      213     1     0 /sbin/init auto automatic-ubiquity noprompt   0 root         
4026532641 mnt    /proc/7583/ns/mnt      3  7583  7553 nginx: master process nginx -g daemon off;    0 root         
4026532642 uts    /proc/7583/ns/uts      3  7583  7553 nginx: master process nginx -g daemon off;    0 root         
4026532643 ipc    /proc/7583/ns/ipc      3  7583  7553 nginx: master process nginx -g daemon off;    0 root         
4026532644 pid    /proc/7583/ns/pid      3  7583  7553 nginx: master process nginx -g daemon off;    0 root         
4026532646 net    /proc/7583/ns/net      3  7583  7553 nginx: master process nginx -g daemon off;    0 root       0 /run/docker/netns/e06fb2b7b9df

```

nginx容器默认使用了mnt、uts、ipc、pid、net 命名空间隔离，而user与cgroup则继承系统默认命名空间。网络命名空间指定了文件系统挂载点。

查看系统所有的命名空间：

```
$ sudo lsns
        NS TYPE   NPROCS   PID USER             COMMAND
4026531835 cgroup    213     1 root             /sbin/init auto automatic-ubiquity noprompt
4026531836 pid       210     1 root             /sbin/init auto automatic-ubiquity noprompt
4026531837 user      213     1 root             /sbin/init auto automatic-ubiquity noprompt
4026531838 uts       207     1 root             /sbin/init auto automatic-ubiquity noprompt
4026531839 ipc       210     1 root             /sbin/init auto automatic-ubiquity noprompt
4026531840 mnt       202     1 root             /sbin/init auto automatic-ubiquity noprompt
4026531860 mnt         1    22 root             kdevtmpfs
4026531992 net       210     1 root             /sbin/init auto automatic-ubiquity noprompt
4026532548 mnt         1   529 root             /lib/systemd/systemd-udevd
4026532549 uts         1   529 root             /lib/systemd/systemd-udevd
4026532625 mnt         1   757 systemd-timesync /lib/systemd/systemd-timesyncd
4026532626 uts         1   757 systemd-timesync /lib/systemd/systemd-timesyncd
4026532627 mnt         1   840 systemd-network  /lib/systemd/systemd-networkd
4026532637 mnt         1   842 systemd-resolve  /lib/systemd/systemd-resolved
4026532641 mnt         3  7583 root             nginx: master process nginx -g daemon off;
4026532642 uts         3  7583 root             nginx: master process nginx -g daemon off;
4026532643 ipc         3  7583 root             nginx: master process nginx -g daemon off;
4026532644 pid         3  7583 root             nginx: master process nginx -g daemon off;
4026532646 net         3  7583 root             nginx: master process nginx -g daemon off;
4026532693 uts         1   879 root             /lib/systemd/systemd-logind
4026532749 mnt         1   960 root             /usr/sbin/ModemManager
4026532750 mnt         1   870 root             /usr/sbin/irqbalance --foreground
4026532751 mnt         1   879 root             /lib/systemd/systemd-logind

```

发现会多出nginx容器的命名空间。

### 2.2.2、修改容器命名空间

（1）-uts参数指定修改容器的uts用户命名空间。

```
# 修改uts的命名空间使用主机的命名空间
docker run -d --uts host --name mynginx1 nginx
```

（2）查看进程ID：

```
$ docker top mynginx1
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
root                8454                8427                0                   14:24               ?                   00:00:00            nginx: master process nginx -g daemon off;
systemd+            8511                8454                0                   14:24               ?                   00:00:00            nginx: worker process
systemd+            8512                8454                0                   14:24               ?                   00:00:00            nginx: worker process

```

（3）查看进程命名空间。

```
sudo lsns -p &lt;pid&gt; --output-all
```

示例：

```
$ sudo lsns -p 8454 --output-all 
        NS TYPE   PATH              NPROCS   PID  PPID COMMAND                                     UID USER NETNSID NSFS
4026531835 cgroup /proc/1/ns/cgroup    219     1     0 /sbin/init auto automatic-ubiquity noprompt   0 root         
4026531837 user   /proc/1/ns/user      219     1     0 /sbin/init auto automatic-ubiquity noprompt   0 root         
4026531838 uts    /proc/1/ns/uts       213     1     0 /sbin/init auto automatic-ubiquity noprompt   0 root         
4026532703 mnt    /proc/8454/ns/mnt      3  8454  8427 nginx: master process nginx -g daemon off;    0 root         
4026532704 ipc    /proc/8454/ns/ipc      3  8454  8427 nginx: master process nginx -g daemon off;    0 root         
4026532705 pid    /proc/8454/ns/pid      3  8454  8427 nginx: master process nginx -g daemon off;    0 root         
4026532707 net    /proc/8454/ns/net      3  8454  8427 nginx: master process nginx -g daemon off;    0 root       1 /run/docker/netns/0722fcb79ac8

```

（4）查看系统的所有命名空间，可以明显的对比开启的两个nginx容器的命名空间差异。

```
sudo lsns
```

```
$ sudo lsns
        NS TYPE   NPROCS   PID USER             COMMAND
4026531835 cgroup    217     1 root             /sbin/init auto automatic-ubiquity noprompt
4026531836 pid       211     1 root             /sbin/init auto automatic-ubiquity noprompt
4026531837 user      217     1 root             /sbin/init auto automatic-ubiquity noprompt
4026531838 uts       211     1 root             /sbin/init auto automatic-ubiquity noprompt
4026531839 ipc       211     1 root             /sbin/init auto automatic-ubiquity noprompt
4026531840 mnt       203     1 root             /sbin/init auto automatic-ubiquity noprompt
4026531860 mnt         1    22 root             kdevtmpfs
4026531992 net       211     1 root             /sbin/init auto automatic-ubiquity noprompt
4026532548 mnt         1   529 root             /lib/systemd/systemd-udevd
4026532549 uts         1   529 root             /lib/systemd/systemd-udevd
4026532625 mnt         1   757 systemd-timesync /lib/systemd/systemd-timesyncd
4026532626 uts         1   757 systemd-timesync /lib/systemd/systemd-timesyncd
4026532627 mnt         1   840 systemd-network  /lib/systemd/systemd-networkd
4026532637 mnt         1   842 systemd-resolve  /lib/systemd/systemd-resolved
4026532641 mnt         3  7583 root             nginx: master process nginx -g daemon off;
4026532642 uts         3  7583 root             nginx: master process nginx -g daemon off;
4026532643 ipc         3  7583 root             nginx: master process nginx -g daemon off;
4026532644 pid         3  7583 root             nginx: master process nginx -g daemon off;
4026532646 net         3  7583 root             nginx: master process nginx -g daemon off;
4026532693 uts         1   879 root             /lib/systemd/systemd-logind
4026532703 mnt         3  8454 root             nginx: master process nginx -g daemon off;
4026532704 ipc         3  8454 root             nginx: master process nginx -g daemon off;
4026532705 pid         3  8454 root             nginx: master process nginx -g daemon off;
4026532707 net         3  8454 root             nginx: master process nginx -g daemon off;
4026532749 mnt         1   960 root             /usr/sbin/ModemManager
4026532750 mnt         1   870 root             /usr/sbin/irqbalance --foreground
4026532751 mnt         1   879 root             /lib/systemd/systemd-logind

```

### 2.2.3、容器进程命名空间的具体体现

（1）开启docker user命名空间配置，/etc/docker/daemon.json 文件添加以下选项：

```
# 默认生成
&quot;userns-remap&quot;:&quot;default&quot;
# 或
# 指定已存在用户和组
&quot;userns-remap&quot;:&quot;user:group&quot;
```

示例：

```
{
        &quot;userns-remap&quot;:&quot;default&quot;,
        &quot;registry-mirrors&quot;:[
                &quot;https://hub-mirror.c.163.com&quot;,
                &quot;https://docker.mirrors.ustc.edu.cn&quot;,
                &quot;https://registry.docker-cn.com&quot;
        ]
}

```

daemon.json是docker的配置文件，一般是没有的，如果没有就自己创建并添加内容，如果有就直接修改相关内容。镜像源就是在这个文件下配置。

（2）重启docker服务。

```
sudo systemctl restart docker.service

```

重启前的docker info：

```
$ docker info 
Client:
 Context:    default
 Debug Mode: false

Server:
 Containers: 3
  Running: 2
  Paused: 0
  Stopped: 1
 Images: 2
 Server Version: 20.10.12
 Storage Driver: overlay2
  Backing Filesystem: extfs
  Supports d_type: true
  Native Overlay Diff: true
  userxattr: false
 Logging Driver: json-file
 Cgroup Driver: cgroupfs
 Cgroup Version: 1
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local logentries splunk syslog
 Swarm: inactive
 Runtimes: io.containerd.runc.v2 io.containerd.runtime.v1.linux runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: 
 runc version: 
 init version: 
 Security Options:
  apparmor
  seccomp
   Profile: default
 Kernel Version: 5.4.0-135-generic
 Operating System: Ubuntu 20.04.5 LTS
 OSType: linux
 Architecture: x86_64
 CPUs: 2
 Total Memory: 1.907GiB
 Name: fly
 ID: PALW:CS67:UTR6:Q3TR:QUH7:U2LI:KGEJ:U4KL:OG3L:R2WT:2I5X:R33I
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 Registry: https://index.docker.io/v1/
 Labels:
 Experimental: false
 Insecure Registries:
  127.0.0.0/8
 Live Restore Enabled: false

WARNING: No swap limit support

```

重启后的docker onfo：

```
$ docker info 
Client:
 Context:    default
 Debug Mode: false

Server:
 Containers: 0
  Running: 0
  Paused: 0
  Stopped: 0
 Images: 0
 Server Version: 20.10.12
 Storage Driver: overlay2
  Backing Filesystem: extfs
  Supports d_type: true
  Native Overlay Diff: true
  userxattr: false
 Logging Driver: json-file
 Cgroup Driver: cgroupfs
 Cgroup Version: 1
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local logentries splunk syslog
 Swarm: inactive
 Runtimes: io.containerd.runc.v2 io.containerd.runtime.v1.linux runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: 
 runc version: 
 init version: 
 Security Options:
  apparmor
  seccomp
   Profile: default
  userns
 Kernel Version: 5.4.0-135-generic
 Operating System: Ubuntu 20.04.5 LTS
 OSType: linux
 Architecture: x86_64
 CPUs: 2
 Total Memory: 1.907GiB
 Name: fly
 ID: PALW:CS67:UTR6:Q3TR:QUH7:U2LI:KGEJ:U4KL:OG3L:R2WT:2I5X:R33I
 Docker Root Dir: /var/lib/docker/165536.165536
 Debug Mode: false
 Registry: https://index.docker.io/v1/
 Labels:
 Experimental: false
 Insecure Registries:
  127.0.0.0/8
 Registry Mirrors:
  https://hub-mirror.c.163.com/
  https://docker.mirrors.ustc.edu.cn/
  https://registry.docker-cn.com/
 Live Restore Enabled: false

WARNING: No swap limit support

```

可以明显的看到Docker Root Dir发生了改变；165536.165536就是某个用户的从属ID。

（3）宿主机上查看docker容器默认生成的用户配置。

```
# 用户ID
cat /etc/subuid
# 用户组
cat /etc/subgid
```

cat /etc/subuid显示：

```
fly:100000:65536
dockremap:165536:65536
```

fly的用户ID从100000开始，有65536个；dockremap的用户ID从165536开始，有65536个。

cat /etc/subuid显示：

```
fly:100000:65536
dockremap:165536:65536
```

/etc/subuid文件：dockremap:165536:65536 表示宿主机使用dockremap用户，容器使用其从属ID，范围从0 ~ 65536，与之对应的宿主机ID范围：165536 ~ 165536+65536

/etc/subgid文件：针对用户组与/etc/subuid 类似。

（4）User命名空间：启动新的nginx容器，查看user命名空间。

```
# 启动一个容器
docker run -d --name mynginx nginx
# 查询进程ID
docker top mynginx
# 查看进程命名空间，进程拥有独立的命名空间
sudo lsns -p &lt;pid&gt; --output-all
```

```
fly@fly:~$ docker top mynginx 

UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
165536              10660               10630               0                   15:04               ?                   00:00:00            nginx: master process nginx -g daemon off;
165637              10718               10660               0                   15:04               ?                   00:00:00            nginx: worker process
165637              10719               10660               0                   15:04               ?                   00:00:00            nginx: worker process

fly@fly:~$ sudo lsns -p 10660 --output-all 
        NS TYPE   PATH                NPROCS   PID  PPID COMMAND                                        UID USER   NETNSID NSFS
4026531835 cgroup /proc/1/ns/cgroup      215     1     0 /sbin/init auto automatic-ubiquity noprompt      0 root           
4026532641 user   /proc/10660/ns/user      3 10660 10630 nginx: master process nginx -g daemon off;  165536 165536         
4026532642 mnt    /proc/10660/ns/mnt       3 10660 10630 nginx: master process nginx -g daemon off;  165536 165536         
4026532643 uts    /proc/10660/ns/uts       3 10660 10630 nginx: master process nginx -g daemon off;  165536 165536         
4026532644 ipc    /proc/10660/ns/ipc       3 10660 10630 nginx: master process nginx -g daemon off;  165536 165536         
4026532645 pid    /proc/10660/ns/pid       3 10660 10630 nginx: master process nginx -g daemon off;  165536 165536         
4026532647 net    /proc/10660/ns/net       3 10660 10630 nginx: master process nginx -g daemon off;  165536 165536       0 /run/docker/netns/a7fc1e4e622c

```

可以看到，用户不再是root了，而是165536。

也可以与容器交互,查看当前用户信息，显示为root：

```
docker exec -it mynginx bash
```

（5）运行容器，指定私有cgroupns,指定user。

```
docker run -d --cgroupns private --user root --name mynginx1 nginx
```

查看进程用户空间信息：

```
fly@fly:~$ docker top mynginx1
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
165536              11373               11344               0                   15:21               ?                   00:00:00            nginx: master process nginx -g daemon off;
165637              11428               11373               0                   15:21               ?                   00:00:00            nginx: worker process
165637              11429               11373               0                   15:21               ?                   00:00:00            nginx: worker process

fly@fly:~$ sudo lsns -p 11373 --output-all 
        NS TYPE   PATH                  NPROCS   PID  PPID COMMAND                                       UID USER   NETNSID NSFS
4026532704 user   /proc/11373/ns/user        3 11373 11344 nginx: master process nginx -g daemon off; 165536 165536         
4026532705 mnt    /proc/11373/ns/mnt         3 11373 11344 nginx: master process nginx -g daemon off; 165536 165536         
4026532706 uts    /proc/11373/ns/uts         3 11373 11344 nginx: master process nginx -g daemon off; 165536 165536         
4026532707 ipc    /proc/11373/ns/ipc         3 11373 11344 nginx: master process nginx -g daemon off; 165536 165536         
4026532708 pid    /proc/11373/ns/pid         3 11373 11344 nginx: master process nginx -g daemon off; 165536 165536         
4026532710 net    /proc/11373/ns/net         3 11373 11344 nginx: master process nginx -g daemon off; 165536 165536       1 /run/docker/netns/dc8a107bfb4f
4026532770 cgroup /proc/11373/ns/cgroup      3 11373 11344 nginx: master process nginx -g daemon off; 165536 165536     
```

（6）UTS命名空间：启动新容器，设置hostname与domain。

```
# 运行容器，指定hostname与域名
docker run -d --domainname abc.nick.com --hostname abcdefg --userns host --name mynginx2 nginx
# 与容器交互，进入交互模式
docker exec -it mynginx2 bash
# 访问hostname 与 domainname
hostname
domainname
# 通过hostname与domainname访问应用
curl http://abcdefg
curl http://abcdefg.abc.nick.com
# 通过文件查看hostname与domainname
cat /proc/sys/kernel/hostname
cat /proc/sys/kernel/domainname

```

```
fly@fly:~$ docker run -d --domainname www.fly.com --hostname abc --userns host --name mynginx2 nginx
1f2d1a8d657fcaacda4853b1ca93ca69a4600f5d595b1ab8cf6c5d01eb930916
fly@fly:~$ docker exec -it mynginx2 bash
root@abc:/# hostname 
abc
root@abc:/# domainname 
www.fly.com
root@abc:/# curl http://abc
&lt;!DOCTYPE html&gt;
&lt;html&gt;
&lt;head&gt;
&lt;title&gt;Welcome to nginx!&lt;/title&gt;
&lt;style&gt;
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
&lt;/style&gt;
&lt;/head&gt;
&lt;body&gt;
&lt;h1&gt;Welcome to nginx!&lt;/h1&gt;
&lt;p&gt;If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.&lt;/p&gt;

&lt;p&gt;For online documentation and support please refer to
&lt;a href=&quot;http://nginx.org/&quot;&gt;nginx.org&lt;/a&gt;.&lt;br/&gt;
Commercial support is available at
&lt;a href=&quot;http://nginx.com/&quot;&gt;nginx.com&lt;/a&gt;.&lt;/p&gt;

&lt;p&gt;&lt;em&gt;Thank you for using nginx.&lt;/em&gt;&lt;/p&gt;
&lt;/body&gt;
&lt;/html&gt;
root@abc:/# curl http://abc.www.fly.com
&lt;!DOCTYPE html&gt;
&lt;html&gt;
&lt;head&gt;
&lt;title&gt;Welcome to nginx!&lt;/title&gt;
&lt;style&gt;
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
&lt;/style&gt;
&lt;/head&gt;
&lt;body&gt;
&lt;h1&gt;Welcome to nginx!&lt;/h1&gt;
&lt;p&gt;If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.&lt;/p&gt;

&lt;p&gt;For online documentation and support please refer to
&lt;a href=&quot;http://nginx.org/&quot;&gt;nginx.org&lt;/a&gt;.&lt;br/&gt;
Commercial support is available at
&lt;a href=&quot;http://nginx.com/&quot;&gt;nginx.com&lt;/a&gt;.&lt;/p&gt;

&lt;p&gt;&lt;em&gt;Thank you for using nginx.&lt;/em&gt;&lt;/p&gt;
&lt;/body&gt;
&lt;/html&gt;
root@abc:/# cat /proc/sys/kernel/hostname 
abc
root@abc:/# cat /proc/sys/kernel/domainname 
www.fly.com
root@abc:/# 

```

（7）mount、PID、Network 命名空间：启动一个工具容器。

```
# 运行工具容器
docker run -dit --name mycurl radial/busyboxplus:curl
# 进入交互模式
docker exec -it mycurl sh

```

mount命名空间：容器内部执行mount 与宿主机内执行mount命令对比，即可看出各自拥有不同的mounts。mounts文件位于：/proc/mounts 和 /proc/{PID}/mounts。

mounts文件列说明：

| 标识 | 描述 |
| --- | --- |
| Device | mount的设备 |
| Mount Point | 挂载点，也就是挂载的路径 |
| File System Type | 文件系统类型，如ext4、xfs等 |
| Options | 挂载选项，包括读写权限等参数 |

PID命名空间：容器内部进程ID为1，宿主机内进程ID不为1。

```
[ root@cf6976e1333f:/ ]$ ps
PID   USER     COMMAND
    1 root     /bin/sh
    9 root     sh
   18 root     ps

```

```
$ docker top mycurl 
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
165536              12361               12328               0                   15:38               pts/0               00:00:00            /bin/sh

```

NetWork命名空间：通过ifconfig工具，查看网络信息。容器与宿主机网络完全是两个独立的网络栈。

```
[ root@cf6976e1333f:/ ]$ ifconfig 
eth0      Link encap:Ethernet  HWaddr 02:42:AC:11:00:05  
          inet addr:172.17.0.5  Bcast:172.17.255.255  Mask:255.255.0.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:10 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:796 (796.0 B)  TX bytes:0 (0.0 B)

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)

```

```
~$ ifconfig 
docker0: flags=4163&lt;UP,BROADCAST,RUNNING,MULTICAST&gt;  mtu 1500
        inet 172.17.0.1  netmask 255.255.0.0  broadcast 172.17.255.255
        inet6 fe80::42:f4ff:fe99:44a7  prefixlen 64  scopeid 0x20&lt;link&gt;
        ether 02:42:f4:99:44:a7  txqueuelen 0  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 7  bytes 746 (746.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

ens33: flags=4163&lt;UP,BROADCAST,RUNNING,MULTICAST&gt;  mtu 1500
        inet 192.168.0.103  netmask 255.255.255.0  broadcast 192.168.0.255
        inet6 fe80::20c:29ff:fe74:ce67  prefixlen 64  scopeid 0x20&lt;link&gt;
        ether 00:0c:29:74:ce:67  txqueuelen 1000  (Ethernet)
        RX packets 206318  bytes 290602412 (290.6 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 58446  bytes 5176631 (5.1 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73&lt;UP,LOOPBACK,RUNNING&gt;  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10&lt;host&gt;
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 400  bytes 38020 (38.0 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 400  bytes 38020 (38.0 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

```

# 总结

docker使用的隔离机制就是进程的隔离机制。

docker不是虚拟机，他就是一个进程，容器隔离使用的就是进程命名隔离机制。