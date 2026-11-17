
# K8s：Pod 中 command、args 与 Dockerfile 中 CMD、ENTRYPOINT 的对应关系

> 来源：https://geekbang.feishu.cn/docx/OKnbdwMaBo1LRTxRppicLaTAnKe
>
> 👨🏻 作者：山河已无恙
>
> 📃 作者介绍：SRE，云原生相关技术干货分享，RHCA/RHCE/RHCAS、CKA 认证，CSDN/51CTO博客专家，InfoQ写作平台/Alibaba Cloud社区签约作者、Huawei Cloud社区云享专家、Tencent Cloud社区内容共创官
>
> ⏰ 收录时间：2024-05-27 11:44:58 (UTC+8)
>
> 🔗 原文链接：https://liruilong.blog.csdn.net/article/details/133966519

![](img/8ffc2fcf0edc115dadd6dbc96871fa8a.png)

关注作者，获取更多AI知识

## 写在前面

- 前几天被问到，这里整理笔记
- 之前也没怎么注意这个问题
- 理解不足小伙伴帮忙指正

**曾以为老去是很遥远的事，突然发现年轻是很久以前的事了。时光好不经用，抬眼已是半生，所谓的中年危机，真正让人焦虑的不是孤单、不是贫穷、更不是衰老，而是人到中年你才发现，你从来没有按照自己喜欢的方式活过，这烟火人间，事事值得、事事与遗憾，该用多懂事的理智，去压抑住心中的不甘与难过。——余华《活着》**

---

[K8s](https://so.csdn.net/so/search?q=K8s&spm=1001.2101.3001.7020) Pod 中的 `command`、`args` 配置会覆盖 `Dockerfile` 中 `ENTRYPOINT` 和 `CMD` 指令，具体 `command` 命令代替 `ENTRYPOINT` 的 [命令行](https://so.csdn.net/so/search?q=%E5%91%BD%E4%BB%A4%E8%A1%8C&spm=1001.2101.3001.7020)，`args` 代替 `CMD` 的参数。但是并不是说他们是一个等价的覆盖关系。

具体的问题具体分析，这里我们准备一个镜像做简单测试

```
┌──[root@vms100.liruilongs.github.io]-[~/docker]
└─$docker build -t liruilong/my-busybox:latest <<EOF
> FROM busybox:latest
> ENTRYPOINT ["printenv"]
> CMD ["HOSTNAME", "KUBERNETES_PORT"]
> EOF
Sending build context to Docker daemon  2.048kB
Step 1/3 : FROM busybox:latest
 ---> beae173ccac6
Step 2/3 : ENTRYPOINT ["printenv"]
 ---> Using cache
 ---> 490cf6028e36
Step 3/3 : CMD ["HOSTNAME", "KUBERNETES_PORT"]
 ---> Using cache
 ---> 497b83a63aad
Successfully built 497b83a63aad
Successfully tagged liruilong/my-busybox:latest
```

上面的 `Dockerfile` 基础镜像使用 `busybox:latest`，添加了 `CMD` 和 `ENTRYPOINT` 指令，构建好的镜像上传 hub.docker 仓库。

```
┌──[root@vms100.liruilongs.github.io]-[~]
└─$docker login
Authenticating with existing credentials...
WARNING! Your password will be stored unencrypted in /root/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
┌──[root@vms100.liruilongs.github.io]-[~/docker]
└─$docker push liruilong/my-busybox:latest
The push refers to repository [docker.io/liruilong/my-busybox]
...
```

然后我们使用 `kubectl` 做下具体的测试，对应四种情况。

> `command` 和 `args` 均没有指定，这种情况会使用 `Dockerfile` 的配置的 `ENTRYPOINT` 和 `CMD`

```
┌──[root@vms100.liruilongs.github.io]-[~/ansible/commm_args]
└─$kubectl run demo  --image liruilong/my-busybox -n demo
pod/demo created
```

```
┌──[root@vms100.liruilongs.github.io]-[~/ansible/commm_args]
└─$kubectl logs demo -n demo
demo
tcp://10.96.0.1:443
```

直接使用的 `Dockerfile` 中的配置

```
ENTRYPOINT ["printenv"]
CMD ["HOSTNAME", "KUBERNETES_PORT"]
```

> `command` 和 `args` 都指定了，那么 `Dockerfile` 的配置 `ENTRYPOINT` 和 `CMD` 被忽略，执行 `command` 并追加上 `args` 参数。

```
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: demo
  name: demo
  namespace: demo
spec:
  containers:
  - command:
    - echo
    args:
    - "山河已无恙"
    image: liruilong/my-busybox
    imagePullPolicy: Always
    name: demo
```

通过日志可以看到覆盖了 `Dockerfile` 中的配置

```
┌──[root@vms100.liruilongs.github.io]-[~/docker]
└─$kubectl apply  -f demo.yaml
pod/demo created
┌──[root@vms100.liruilongs.github.io]-[~/docker]
└─$kubectl logs demo -n demo
山河已无恙
```

> `command` 没有指定，指定了 `args`，那么 `Dockerfile` 中配置的 `ENTRYPOINT` 的命令行会被执行，`CMD` 会被 `args` 中填写的参数覆盖，追加到 `ENTRYPOINT` 中。

```
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: demo
  name: demo
  namespace: demo
spec:
  containers:
  - args:
    - PATH
    image: liruilong/my-busybox
    imagePullPolicy: Always
    name: demo
```

`CMD` 指令被覆盖

```
┌──[root@vms100.liruilongs.github.io]-[~/docker]
└─$kubectl logs demo -n demo
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

> `command` 指定，`args` 没有指定，那么 `Dockerfile` 默认的 `ENTRYPOINT` 配置会被忽略，执行输入的 `command`，同时 `Dockerfile` 中的 `CMD` 也会被忽略。

```
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: demo
  name: demo
  namespace: demo
spec:
  containers:
  - command:
    - echo
    - 山河已无恙
    - 活着
    image: liruilong/my-busybox
    imagePullPolicy: Always
    name: demo
```

这个有些特殊，需要注意下，只指定了 `command`，但是 `entrypoint` 被覆盖，`cmd` 被忽略

```
┌──[root@vms100.liruilongs.github.io]-[~/docker]
└─$kubectl logs demo -n demo
山河已无恙 活着
```

指定的 `CMD` 并没有拼到后面

```
CMD ["HOSTNAME", "KUBERNETES_PORT"]
```

### 博文部分内容参考

© 文中涉及参考链接内容版权归原作者所有，如有侵权请告知。

---

https://kubernetes.io/zh-cn/docs/tasks/inject-data-application/define-command-argument-container/#notes

---

© 2018-2023 liruilonger@gmail.com, All rights reserved. 保持署名-非商用-相同方式共享 (CC BY-NC-SA 4.0)
