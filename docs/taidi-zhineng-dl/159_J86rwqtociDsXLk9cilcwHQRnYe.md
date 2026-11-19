
# Ubuntu 安装 & 配置

> 来源：[https://learning.feishu.cn/docx/NsZFdRCUFojBrOxQETvcfvA8nNb](https://learning.feishu.cn/docx/NsZFdRCUFojBrOxQETvcfvA8nNb)

tested on `20.04` / `22.04`

## U 盘安装 Ubuntu

### 制作 U 盘启动盘

从 `ubuntu 16.04` 开始，用自带的 `Make Startup Disk` 就很稳了。

![](img/872ca9558c844440e54f3d289b9dc1f5.png)

### 磁盘分区

近 10 年来，我一直习惯于 4 分区，直到我开始搞深度学习，与 `CUDA` 亲密接触。

遵循 2020 年代，Linux 电脑磁盘分区的最佳实践

我的分区配置如下：

![](img/27e2cc51994ea957a6c4103d4e26450d.png)

## Driver & SSH config

### apt source

我一直用阿里云的镜像，可以手动改配置文件，也可以直接在 GUI 上点几下解决。

![](img/196f9f81736014f76e87e3610b295925.png)

![](img/e15b1c8ea5b9c98ec16a13b93850f943.png)

### ssh 配置

在 `ubuntu` 上安装 `ssh` server，默认没有安装

```
# tested on 22.04 desktop
sudo apt update
sudo apt install openssh-server

# Ubuntu20.04 or earlier
sudo apt-get install openssh-server openssh-client
sudo /etc/init.d/ssh start
# [ ok ] Starting ssh (via systemctl): ssh.service.
```

打通免密码登陆

```
ssh-copy-id -i ~/.ssh/m1_key gpu
```

### Nvidia driver

```
# ubuntu 20.04 tested
sudo apt install nvidia-driver-465

# ubuntu 22.04 at 2024.03
sudo apt install nvidia-driver-535

sudo reboot  # reboot the computer to enable it
```

验证安装成功的方法：

```
nvidia-smi
```

显示我的 Driver Version: `535.161.07`

![](img/a75aba23ad571a6fb8316ca649d135f1.png)

`cuda` 可以选 `docker` 的方案，3 点原因:

第一，`docker` 已经优化的非常成熟了，包括 `IO` 在内，引入性能 `overhead` 都可以忽略不计。

第二，`tensorflow` 的官方文档，也开始推荐 `docker` 而非本地装 `cuda` 了。

[https://www.tensorflow.org/install/gpu](https://www.tensorflow.org/install/gpu)

> 为了简化安装并避免库冲突，建议您使用支持 GPU 的 TensorFlow Docker 映像（仅限 Linux）。此设置仅需要 NVIDIA® GPU 驱动程序

第三，日常工作需要大量使用预训练模型，尝试别人的开源代码，版本很乱，依赖的 `cuda` 版本、`cudnn` 版本等都排列组合起来非常多。用 `docker` 更好管理。

## Pip 源

`pip` 源，我一直使用阿里云的镜像源

创建文件 `~/.pip/pip.conf`，添加如下内容

```
[global]
index-url = http://mirrors.aliyun.com/pypi/simple/

[install]
trusted-host = mirrors.aliyun.com
```

### docker

[https://docs.docker.com/engine/install/ubuntu/](https://docs.docker.com/engine/install/ubuntu/)

[配置 docker image 源：使用阿里云的源。官方文档](https://help.aliyun.com/document_detail/60750.html)：https://help.aliyun.com/document_detail/60750.html

修改 `/etc/docker/daemon.json` 内容如下：(阿里镜像，域名里的 `a10q86at` 要换成自己的)

```
{
  "registry-mirrors": ["https://a10q86at.mirror.aliyuncs.com"]
}
```

重启 `docker` 使配置生效

```
sudo systemctl daemon-reload
sudo systemctl restart docker
```

#### nvidia-docker

安装了之后，才能在 `docker` 内看到并使用 `GPU`。

```
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg && curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | \
    sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
    sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list

# Add the package repositories
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list \
    | sudo tee /etc/apt/sources.list.d/nvidia-docker.list

sudo apt-get update
sudo apt-get install -y nvidia-container-toolkit
sudo systemctl restart docker
```

验证安装，执行如下命令，预期能看到 `nvidia-smi` 的输出。

```
docker run --gpus all nvidia/cuda:10.0-base nvidia-smi

docker run --gpus all nvidia/cuda:12.0-base nvidia-smi
```

## 其他工具一把梭

```
sudo apt-get install tmux ranger git vim htop ncdu nload silversearcher-ag flake8 ctags
pip install nvidia-ml-py3
pip install glances
```

1.  看大量自己不熟悉的代码。工具：`ag`。更人性化的 `grep`。

1.  在命令行里工作。工具：`tmux`，`ranger`，`git`，`vim`。

1.  故障排查。电脑为什么慢了，磁盘空间为什么没了。对应的工具是：`htop`、`ncdu`，`nload`，`nvidia-smi`，`glances`

注：

+   `flake8` 是 `python` 的 `pep8` 工具，我的 `vim` 里配置依赖他的 `lint` 命令。

+   `ctags` 是 `c` 语言的源码自动跳转工具，`vim` 配置依赖。

工具配置

```
# git
git config --global user.name "Jackon Yang"
git config --global user.email "i@jackon.me"
# vim
git clone git@github.com:JackonYang/vimrc.git ~/.vim
cd ~/.vim
./install.sh
```

其他一些注意事项：

`ubuntu 16.04` 开始，默认的 `vim` 只支持 `python3`，而不支持 `python 2`。

从 `ubuntu 18.04` 开始，我的个人开发环境也全部切换至 `python3`。不再需要 `python2` 相关的支持。

### glances 介绍

增强版的 `htop`。

`cpu`，`gpu`，温度，内存 等各种监控在一张图里

!

### ranger 介绍

命令行里的文件浏览器，可以预览文件内容，支持 `vim` 快捷键

!

### ncdu 介绍

更好用的 `du` 命令，看磁盘空间占用

!

### nload 介绍

看各个网卡实时的流入/流出带宽

!

### htop 介绍

更好用的 `top` 命令

!
