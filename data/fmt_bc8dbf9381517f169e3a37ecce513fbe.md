
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

`nginx`容器默认使用了`mnt`、`uts`、`ipc`、`pid`、`net` 命名空间隔离，而`user`与`cgroup`则继承系统默认命名空间。网络命名空间指定了文件系统挂载点。

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

发现会多出`nginx`容器的命名空间。

### 2.2.2、修改容器命名空间

（1）`-uts`参数指定修改容器的`uts`用户命名空间。

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
sudo lsns -p <pid> --output-all
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

（4）查看系统的所有命名空间，可以明显的对比开启的两个`nginx`容器的命名空间差异。

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

### 2.2.3、容器进程命名空间的具体体現

（1）开启`docker` `user`命名空间配置，`/etc/docker/daemon.json` 文件添加以下选项：

```
# 默认生成
"userns-remap": "default"
# 或
# 指定已存在用户和组
"userns-remap": "user:group"
```

示例：

```
{
        "userns-remap": "default",
        "registry-mirrors": [
                "https://hub-mirror.c.163.com",
                "https://docker.mirrors.ustc.edu.cn",
                "https://registry.docker-cn.com"
        ]
}
```
