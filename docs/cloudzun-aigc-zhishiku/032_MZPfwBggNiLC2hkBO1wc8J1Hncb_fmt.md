
# ChatGLM-2大语言模型的部署和使用

> 来源：[https://zsyhjtnsa5.feishu.cn/docx/X0bodh0v2ouBAfx15r8cZkM2ncg](https://zsyhjtnsa5.feishu.cn/docx/X0bodh0v2ouBAfx15r8cZkM2ncg)

## 注意事项

1. 我们采用`Docker`来构建模型运行所需的环境。`Docker`的优势在于其跨平台性和隔离性。无论在`Windows`还是`Linux`的操作系统下，`Docker`都能提供一致的运行环境，保证了测试结果的一致性。其隔离性则确保了每个测试环境的干净和独立，这在进行多模型测试时尤其重要。

1. 请注意，我们的测试环境是基于消费级硬件构建的，因此，其性能具有一定的局限性。因此，本手册的主要目标是帮助用户理解和掌握模型的运行、调用、调优和推理的基本理念及其相关的操作步骤，而不是追求最高的模型运行质量。由于硬件性能的限制，对于大型模型的运行性能，我们恳请您理解并适当调整预期。

1. ……

## 测试环境硬件配置

为了顺利执行本实验手册中的各项步骤，我们建议您使用显存不低于24GB的显卡，例如RTX 4090。此外，对于其他硬件配置，也有一定的要求。以下是我们测试环境的详细硬件配置，供您参考：

| 序号 | 类型 | 硬件 | 数量 | 备注 |
| --- | --- | --- | --- | --- |
| 1 | 水冷系统 | 利民 AQUA ELITE AE360 | 1 |  |
| 2 | 处理器 | I7-13700K 16核/24线 | 1 | 散片 |
| 3 | 主板 | 华硕TUF GAMING B760M-Pro D5 | 1 |  |
| 4 | 内存 | 芝奇幻锋戟 64/6000 32GB | 2 | 择机再买两根 |
| 5 | 固态硬盘 | 宏基掠夺者(PREDATOR)2TB SSD固态硬盘 | 1 | 酌情再加一块 |
| 6 | 显卡 | 微星 RTX4090 GAMING X TRIO | 1 |  |
| 7 | 机箱 | 影驰 沐光海景房机箱 | 1 |  |
| 8 | 电源 | 鑫谷 GM1250W金牌全模组 额定1250W ATX3.0 + 棱镜2代风扇6个 | 1 |  |

## 环境选项1：`WSL` on Windows

如果希望所有的操作在`Windows`上就能实现，可以考虑使用`WSL`里的`docker`环境。

+ 首先是确定`docker`是否能支持 `nvidia` `gpu`，查到了`docker`上的资料： https://www.docker.com/blog/wsl-2-gpu-support-for-docker-desktop-on-nvidia-gpus/

得到了官方确定："Nvidia used the term near-native to describe the performance to be expected."

+ 然后到`nvidia`官网 https://developer.nvidia.com/cuda/wsl 下载`cuda`： https://us.download.nvidia.com/Windows/536.67/536.67-desktop-win10-win11-64bit-international-dch-whql.exe

### 准备环境

1. 安装`hyper-v`：https://learn.microsoft.com/zh-cn/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v

1. 安装`WSL`：https://learn.microsoft.com/zh-cn/windows/wsl/install

+ 这里有个细节需要注意，要启用“Windows 更新 > 高级设置 > 接受其他Microsoft产品的更新” 这个选项，否则如果仅从Microsoft Store下载 `Ubuntu 22.04` 是无法启用`WSL`的。

1. 安装`Windows Docker Desktop`：https://docs.docker.com/desktop/install/windows-install/

+ 也可以在`WSL`用命令行安装`docker`，但是并不是推荐的做法，为了避免后续可能的坑，还是服从最佳实践。

### 环境确认

1. 使用`nvidia-smi`查询宿主机上的信息。

`nvidia-smi` 是`NVIDIA System Management Interface`的缩写，它是一个用于监控和管理`NVIDIA GPU`设备的命令行实用程序。这个工具提供了对`GPU`的实时监控，以及对一些参数的配置控制。

以下是`nvidia-smi`的一些主要功能：

1. `GPU状态监控`：`nvidia-smi`可以显示关于`GPU`的各种信息，包括但不限于其型号、内存使用情况、温度、风扇速度、功耗以及当前正在运行的进程。

1. `GPU性能管理`：`nvidia-smi`允许用户对`GPU`进行一些配置，如设置功耗限制，调整内存和`GPU`时钟频率等。

1. `GPU计算模式管理`：`nvidia-smi`可以改变`GPU`的计算模式，这对于优化特定应用的性能可能是必要的。

1. `GPU进程管理`：`nvidia-smi`可以列出正在使用`GPU`的进程，并显示它们的`GPU`和内存使用情况。它也可以用于终止这些进程。

在大多数情况下，你只需要运行`nvidia-smi`命令，它就会显示出你的`GPU`的当前状态。例如，它可能会显示出你的`GPU`型号、总内存、已使用的内存、`GPU`利用率、内存利用率、温度等信息。

`nvidia-smi`是一个非常强大的工具，尤其是对于需要密集计算的应用，如深度学习、科学计算等。它允许你监控并管理你的`GPU`，以确保它们能够以最佳性能运行。

```
nvidia-smi
Fri Jul 28 10:10:27 2023
+---------------------------------------------------------------------------------------+
| NVIDIA-SMI 535.86.01              Driver Version: 536.67       CUDA Version: 12.2     |
|-----------------------------------------+----------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |         Memory-Usage | GPU-Util  Compute M. |
|                                         |                      |               MIG M. |
|=========================================+======================+======================|
|   0  NVIDIA GeForce RTX 4090        On  | 00000000:01:00.0  On |                  Off |
|  0%   38C    P8              27W / 450W |    313MiB / 24564MiB |      7%      Default |
|                                         |                      |                  N/A |
+-----------------------------------------+----------------------+----------------------+

+---------------------------------------------------------------------------------------+
| Processes:                                                                            |
|  GPU   GI   CI        PID   Type   Process name                            GPU Memory |
|        ID   ID                                                             Usage      |
|=======================================================================================|
|  No running processes found                                                           |
+---------------------------------------------------------------------------------------+
```

> 这是`nvidia-smi`命令的输出，提供了关于系统中`NVIDIA GPU`的信息。以下是对这个输出的解读：

+ `NVIDIA-SMI 535.86.01`：这是你正在运行的`nvidia-smi`的版本。

+ `Driver Version: 536.67`：这是你的`NVIDIA`驱动的版本。

+ `CUDA Version: 12.2`：这是你的`CUDA`的版本。`CUDA`是`NVIDIA`开发的一个平台，它允许开发者直接访问`GPU`的计算能力。

> 下面的表格提供了关于你的`GPU`的详细信息：

+ `GPU`：这是你的`GPU`的编号。在这个例子中，你只有一个`GPU`，所以它的编号是0。

+ `Name`：这是你的`GPU`的名称，在这个例子中，它是`NVIDIA GeForce RTX 4090`。

+ `Persistence-M`：这是`GPU`的持久模式状态。如果启用了持久模式，`GPU`将在系统启动时立即初始化，而不是在运行`CUDA`程序时进行初始化。

+ `Bus-Id`：这是`GPU`在系统中的位置。

+ `Disp.A`：这是显示活动的状态。如果为“On”，则`GPU`正在驱动显示器。

+ `Volatile Uncorr. ECC`：这是`GPU`的错误校验和纠正(`ECC`)的状态。在这个例子中，`ECC`是关闭的。

+ `Fan`：这是风扇的速度，以百分比表示。

+ `Temp`：这是`GPU`的当前温度。

+ `Perf`：这是`GPU`的性能状态，它影响`GPU`的时钟频率。

+ `Pwr:Usage/Cap`：这是`GPU`的当前功耗和最大功耗。

+ `Memory-Usage`：这是`GPU`内存的当前使用情况和总容量。

+ `GPU-Util`：这是`GPU`的利用率。

+ `Compute M.`：这是`GPU`的计算模式。

> 最后的表格列出了正在使用`GPU`的进程。在这个例子中，没有进程正在使用`GPU`。

`nvidia-smi dmon`命令用于监控`NVIDIA GPU`设备的性能。它提供了一种实时监控`GPU`状态的方式，包括功耗、温度、使用率等各种参数。这个命令会持续运行，定期更新并打印出新的状态信息，直到用户停止它。

```
chengzh@AIStudio:~$ nvidia-smi dmon
# gpu    pwr  gtemp  mtemp     sm    mem    enc    dec    jpg    ofa   mclk   pclk
# Idx      W      C      C      %      %      %      %      %      %    MHz    MHz
    0     27     38      -      0      0      0      0      0      0    405    210
    0     27     38      -      0      0      0      0      0      0    405    210
    0     27     38      -      0      0      0      0      0      0    405    210
    0     26     38      -      3      0      0      0      0      0    405    210
    0     27     38      -      0      0      0      0      0      0    405    210
    0     27     38      -      3      1      0      0      0      0    405    210
    0     27     38      -      0      0      0      0      0      0    405    210
    0     27     38      -      0      0      0      0      0      0    405    210
    0     27     38      -      0      0      0      0      0      0    405    210
```

> 以下是对这个输出的解读：

+ `gpu`：这是`GPU`的编号。在这个例子中，你只有一个`GPU`，所以它的编号是0。

+ `pwr`：这是`GPU`的当前功耗，单位为瓦特。

+ `gtemp`：这是`GPU`的当前温度，单位为摄氏度。

+ `mtemp`：这是`GPU`内存的当前温度。在这个例子中，这个值没有提供。

+ `sm`：这是流多处理器的利用率，以百分比表示。流多处理器是`GPU`内部的处理单元。

+ `mem`：这是`GPU`内存的利用率，以百分比表示。

+ `enc`：这是`GPU`的视频编码器的利用率，以百分比表示。

+ `dec`：这是`GPU`的视频解码器的利用率，以百分比表示。

+ `jpg`：这是`GPU`的`JPEG`解码器的利用率，以百分比表示。

+ `ofa`：这是其他的`GPU`功能的利用率，以百分比表示。

+ `mclk`：这是`GPU`内存的时钟频率，单位为兆赫兹。

+ `pclk`：这是`GPU`处理器的时钟频率，单位为兆赫兹。

> 在这个例子中，你可以看到你的`GPU`的功耗稳定在27瓦，温度稳定在38摄氏度。`GPU`的利用率和内存利用率都非常低，这意味着`GPU`在这段时间内没有进行大量的计算任务。另外，`GPU`的时钟频率也相对较低，这也说明`GPU`没有进行大量的计算任务。

1. 验证容器可以调用到`GPU`

1. 使用`nvidia-smi`命令监控容器中的`GPU`参数

```
docker run -it --gpus=all --rm nvidia/cuda:11.4.3-base-ubuntu20.04 nvidia-smi
```

> 这是一个`Docker`命令，用于运行一个特定的`Docker`容器。下面是对这个命令的详细解释：

+ `docker run`: 这是`Docker`的主要命令，用于运行一个`Docker`容器。

+ `-it`: 这是两个参数，`-i`和`-t`。`-i`代表交互模式，这意味着你可以与容器交互。`-t`为运行的容器分配一个伪终端或终端。

+ `--gpus=all`: 这个参数告诉`Docker`你想要使用主机上的所有`GPU`。这是在运行需要大量计算能力的程序（如深度学习模型）时非常有用的。

+ `--rm`: 这个参数表示一旦容器停止，就自动删除它。这可以防止你的硬盘被停止的容器占用空间。

+ `nvidia/cuda:11.4.2-base-ubuntu20.04`: 这是你想要运行的`Docker`镜像的名称。在这个例子中，你正在运行一个包含`NVIDIA CUDA 11.4.2`和基于`Ubuntu 20.04`的镜像。`CUDA`是`NVIDIA`开发的一个平台，它允许开发者直接访问`GPU`的计算能力。

+ `nvidia-smi`: 这是在容器内部运行的命令。`nvidia-smi`是一个用于监控和管理你的`NVIDIA GPU`的实用程序。在这个例子中，它会显示你的`GPU`的状态信息，如温度、内存使用情况、利用率等。

> 总的来说，这个命令的作用是运行一个包含`NVIDIA CUDA`的`Docker`容器，并在其中运行`nvidia-smi`命令来查看`GPU`的状态信息。

运行效果



## 使用 nvidia-smi dmon 监控容器中的 GPU 状态

```
docker run -it --gpus=all --rm nvidia/cuda:11.4.3-base-ubuntu20.04 nvidia-smi
Unable to find image &#x27;nvidia/cuda:11.4.3-base-ubuntu20.04&#x27; locally
11.4.3-base-ubuntu20.04: Pulling from nvidia/cuda
ca1778b69356: Pull complete
3b595136a210: Pull complete
b0cbedff9be8: Pull complete
569ff44c1698: Pull complete
1c34507dbc74: Pull complete
Digest: sha256:bee557a9785dc70cc3c5084e9b358ad1d64f4b1fc5d87e0fe82c540355a5eba1
Status: Downloaded newer image for nvidia/cuda:11.4.3-base-ubuntu20.04
Fri Jul 28 02:19:10 2023
+---------------------------------------------------------------------------------------+
| NVIDIA-SMI 535.86.01              Driver Version: 536.67       CUDA Version: 12.2     |
|-----------------------------------------+----------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |         Memory-Usage | GPU-Util  Compute M. |
|                                         |                      |               MIG M. |
|=========================================+======================+======================|
|   0  NVIDIA GeForce RTX 4090        On  | 00000000:01:00.0  On |                  Off |
|  0%   38C    P8              28W / 450W |    336MiB / 24564MiB |      0%      Default |
|                                         |                      |                  N/A |
+-----------------------------------------+----------------------+----------------------+

+---------------------------------------------------------------------------------------+
| Processes:                                                                            |
|  GPU   GI   CI        PID   Type   Process name                            GPU Memory |
|        ID   ID                                                             Usage      |
|=======================================================================================|
|  No running processes found                                                           |
+---------------------------------------------------------------------------------------+
```

使用 `nvidia-smi dmon` 命令监控容器中的 GPU 状态

```
docker run -it --gpus=all --rm nvidia/cuda:11.4.3-base-ubuntu20.04 nvidia-smi dmon
```

> 这个 Docker 命令用于运行一个特定的 Docker 容器，并在其中运行 `nvidia-smi dmon` 命令来连续监控 GPU 的状态。下面是对这个命令的详细解释：

+ `docker run`: 这是 Docker 的主要命令，用于运行一个 Docker 容器。
+ `-it`: 这是两个参数，`-i` 和 `-t`。`-i` 代表交互模式，这意味着你可以与容器交互。`-t` 为运行的容器分配一个伪终端或终端。
+ `--gpus=all`: 这个参数告诉 Docker 你想要使用主机上的所有 GPU。这是在运行需要大量计算能力的程序（如深度学习模型）时非常有用的。
+ `--rm`: 这个参数表示一旦容器停止，就自动删除它。这可以防止你的硬盘被停止的容器占用空间。
+ `nvidia/cuda:11.4.3-base-ubuntu20.04`: 这是你想要运行的 Docker 镜像的名称。在这个例子中，你正在运行一个包含 NVIDIA CUDA 11.4.3 和基于 Ubuntu 20.04 的镜像。CUDA 是 NVIDIA 开发的一个平台，它允许开发者直接访问 GPU 的计算能力。
+ `nvidia-smi dmon`: 这是在容器内部运行的命令。`nvidia-smi` 是一个用于监控和管理你的 NVIDIA GPU 的实用程序，`dmon` 是它的一个选项，用于连续监控 GPU 的状态。在这个例子中，它会连续显示你的 GPU 的状态信息，如温度、内存使用情况、利用率等。

> 总的来说，这个命令的作用是运行一个包含 NVIDIA CUDA 的 Docker 容器，并在其中运行 `nvidia-smi dmon` 命令来连续监控 GPU 的状态信息。

## 运行效果

```
docker run -it --gpus=all --rm nvidia/cuda:11.4.3-base-ubuntu20.04 nvidia-smi dmon
# gpu    pwr  gtemp  mtemp     sm    mem    enc    dec    jpg    ofa   mclk   pclk
# Idx      W      C      C      %      %      %      %      %      %    MHz    MHz
    0     27     38      -      7      1      0      0      0      0    405    210
    0     27     38      -      3      0      0      0      0      0    405    210
    0     27     38      -      4      0      0      0      0      0    405    210
    0     27     38      -      0      0      0      0      0      0    405    210
    0     27     38      -      0      0      0      0      0      0    405    210
    0     27     38      -      0      0      0      0      0      0    405    210
    0     27     38      -      3      0      0      0      0      0    405    210
    0     27     38      -      1      0      0      0      0      0    405    210
    0     27     38      -      3      0      0      0      0      0    405    210
    0     27     38      -      0      0      0      0      0      0    405    210
    0     27     38      -      0      0      0      0      0      0    405    210
    0     27     38      -      0      0      0      0      0      0    405    210
    0     30     38      -      3      0      0      0      0      0    405    210
```

## 容器 GPU 性能压力测试

### 执行压力测试

```
docker run -it --gpus=all --rm nvcr.io/nvidia/k8s/cuda-sample:nbody nbody -benchmark
```

> 这个命令也是用来运行一个 Docker 容器，但是这次你正在运行一个包含 NVIDIA CUDA 样本的容器，并在其中运行一个体模拟（n-body simulation）基准测试。下面是对这个命令的详细解释：

+ `docker run`: 这是 Docker 的主要命令，用于运行一个 Docker 容器。
+ `-it`: 这是两个参数，`-i` 和 `-t`。`-i` 代表交互模式，这意味着你可以与容器交互。`-t` 为运行的容器分配一个伪终端或终端。
+ `--gpus=all`: 这个参数告诉 Docker 你想要使用主机上的所有 GPU。这是在运行需要大量计算能力的程序（如深度学习模型）时非常有用的。
+ `--rm`: 这个参数表示一旦容器停止，就自动删除它。这可以防止你的硬盘被停止的容器占用空间。
+ `nvcr.io/nvidia/k8s/cuda-sample:nbody`: 这是你想要运行的 Docker 镜像的名称。在这个例子中，你正在运行一个包含 NVIDIA CUDA 的 n-body 模拟样本的镜像。
+ `nbody -benchmark`: 这是在容器内部运行的命令。`nbody` 是一个模拟星系的重力相互作用的程序，`-benchmark` 选项使其运行一个基准测试。这个基准测试将测量运行模拟所需的时间，这可以用来评估你的 GPU 的性能。

> 总的来说，这个命令的作用是运行一个包含 NVIDIA CUDA 的 n-body 模拟样本的 Docker 容器，并在其中运行一个基准测试来评估你的 GPU 的性能。

亦可使用以下命令运行一个持续运行的 GPU 测试容器

```
docker run -d  --gpus=all --rm nvidia/cuda:11.4.3-base-ubuntu20.04 nvidia-smi dmon
```

然后使用交互式方式和他进行操作

```
docker exec -ti $(docker ps -ql) bash
```

## 执行效果

```
docker run -it --gpus=all --rm nvcr.io/nvidia/k8s/cuda-sample:nbody nbody -benchmark
Unable to find image &#x27;nvcr.io/nvidia/k8s/cuda-sample:nbody&#x27; locally
nbody: Pulling from nvidia/k8s/cuda-sample
22c5ef60a68e: Pull complete
1939e4248814: Pull complete
548afb82c856: Pull complete
a424d45fd86f: Pull complete
207b64ab7ce6: Pull complete
f65423f1b49b: Pull complete
2b60900a3ea5: Pull complete
e9bff09d04df: Pull complete
edc14edf1b04: Pull complete
1f37f461c076: Pull complete
9026fb14bf88: Pull complete
Digest: sha256:59261e419d6d48a772aad5bb213f9f1588fcdb042b115ceb7166c89a51f03363
Status: Downloaded newer image for nvcr.io/nvidia/k8s/cuda-sample:nbody
Run &quot;nbody -benchmark [-numbodies=&lt;numBodies&gt;]&quot; to measure performance.
        -fullscreen       (run n-body simulation in fullscreen mode)
        -fp64             (use double precision floating point values for simulation)
        -hostmem          (stores simulation data in host memory)
        -benchmark        (run benchmark to measure performance)
        -numbodies=&lt;N&gt;    (number of bodies (&gt;= 1) to run in simulation)
        -device=&lt;d&gt;       (where d=0,1,2.... for the CUDA device to use)
        -numdevices=&lt;i&gt;   (where i=(number of CUDA devices &gt; 0) to use for simulation)
        -compare          (compares simulation results running once on the default GPU and once on the CPU)
        -cpu              (run n-body simulation on the CPU)
        -tipsy=&lt;file.bin&gt; (load a tipsy model file for simulation)

NOTE: The CUDA Samples are not meant for performance measurements. Results may vary when GPU Boost is enabled.

&gt; Windowed mode
&gt; Simulation data stored in video memory
&gt; Single precision floating point simulation
&gt; 1 Devices used for simulation
MapSMtoCores for SM 8.9 is undefined.  Default to use 128 Cores/SM
MapSMtoArchName for SM 8.9 is undefined.  Default to use Ampere
GPU Device 0: &quot;Ampere&quot; with compute capability 8.9

&gt; Compute 8.9 CUDA device: [NVIDIA GeForce RTX 4090]
131072 bodies, total time for 10 iterations: 75.760 ms
= 2267.682 billion interactions per second
= 45353.635 single-precision GFLOP/s at 20 flops per interaction
```

> 这段信息来自于 NVIDIA CUDA 样本程序中的一个，名为 `nbody` 的程序。这个程序模拟了一个 n-body 问题，也就是在重力作用下的多体运动问题。这个问题在天体物理中非常常见，用于模拟恒星、行星等天体的运动。

> 这个程序提供了一些命令行选项，用于控制程序的运行方式和性能测量。以下是对这些选项的解释：

+ `-benchmark`: 运行性能基准测试。
+ `-numbodies=<N>`: 在模拟中使用的天体数量。N 应该大于等于 1。
+ `-device=<d>`: 选择要使用的 CUDA 设备。d 应该是一个有效的设备编号。
+ `-numdevices=<i>`: 选择要用于模拟的 CUDA 设备数量。i 应该是一个大于 0 的数。
+ `-compare`: 比较在默认 GPU 和 CPU 上运行的模拟结果。
+ `-cpu`: 在 CPU 上运行 n-body 模拟。
+ `-tipsy=<file.bin>`: 加载一个 tipsy 模型文件用于模拟。

> 然后，程序输出了一些关于运行模拟的信息：

+ `Windowed mode`: 程序在窗口模式下运行。



# GPU 模拟性能测试

+   Simulation data stored in video memory：模拟数据存储在视频内存中。

+   Single precision floating point simulation：使用单精度浮点数进行模拟。

+   1 Devices used for simulation：用于模拟的设备数量为1。

+   `GPU Device 0`: `"Ampere"` with compute capability 8.9：正在使用的GPU设备是具有8.9计算能力的`Ampere`。

> 然后，程序输出了模拟的结果：

+   131072 bodies, total time for 10 iterations: 75.760 ms：模拟了131072个天体，10次迭代的总时间是75.760毫秒。

+   = 2267.682 billion interactions per second：这相当于每秒2267.682亿次交互。

+   = 45353.635 single-precision GFLOP/s at 20 flops per interaction：这相当于每次交互20 flops，总共45353.635 GFLOP/s的单精度浮点运算性能。

更多测试用例请见： https://www.docker.com/blog/wsl-2-gpu-support-for-docker-desktop-on-nvidia-gpus/

# 环境选项2：Linux

如果您希望在一个较为纯净的Linux环境中进行操作，可以参考以下步骤。这些步骤是在一家国内云服务平台的虚拟机上执行并优化的，因此所有的细节都以该环境为主。只要您的本地机器具备足够的硬件条件，也能够实现相同的效果。

## 虚机规格

+   GPU加速型 | 8vCPUs | 64GiB | `p2s.2xlarge.8`

+   GPU显卡: 1 * `NVIDIA V100-PCIe-32G` / 1 * 32G

+   系统映像：`Ubuntu 22.04` server 64bit for GPU

+   系统磁盘：高IO | 200 GiB

+   带宽：选择到最高档300 Mbit/s

+   费用： 配置费用¥14.871/小时+ 弹性公网IP流量费用¥0.64/GB

## 安装nvidia显卡驱动

检查是否存在`nvidia`显卡以及显卡适配的型号

```
nvidia-detector
```

得到以下输出

```
root@aistudio:~# nvidia-detector
nvidia-driver-535
```

> `nvidia-detector` 是一个用于检测系统中 NVIDIA 显卡的工具，它是 Ubuntu 的一部分。如果你已经安装了这个工具，你可以通过运行 `nvidia-detector` 命令来检测你的系统中是否有 NVIDIA 显卡。

> 如果 `nvidia-detector` 返回的是 `none`，那么你的系统中可能没有 NVIDIA 显卡。如果它返回的是一个 NVIDIA 显卡的型号，那么你的系统中有一个 NVIDIA 显卡。

> 请注意，`nvidia-detector` 只能检测到物理上存在的 NVIDIA 显卡，它不能检测到显卡驱动程序是否已经正确安装和运行。如果你的 `nvidia-smi` 命令仍然失败，那么可能是因为你的 NVIDIA 驱动程序没有正确安装或运行。

> 如果 `nvidia-detector` 返回 `nvidia-driver-535`，这意味着它检测到你的系统中有一个 NVIDIA 显卡，并且这个显卡应该使用 `nvidia-driver-535` 驱动程序。

安装`nvidia`驱动程序和`nvidia-dkms`

```
sudo apt-get install -y nvidia-driver-535 nvidia-dkms-535
```

驱动执行完成之后，执行`nvidia-smi`

```
nvidia-smi
```

```
root@aistudio:~# nvidia-smi
Sun Jul 30 10:05:42 2023
+---------------------------------------------------------------------------------------+
| NVIDIA-SMI 535.54.03              Driver Version: 535.54.03    CUDA Version: 12.2     |
|-----------------------------------------+----------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |         Memory-Usage | GPU-Util  Compute M. |
|                                         |                      |               MIG M. |
|=========================================+======================+======================|
|   0  Tesla V100S-PCIE-32GB          Off | 00000000:00:0D.0 Off |                    0 |
| N/A   32C    P0              24W / 250W |      0MiB / 32768MiB |      0%      Default |
|                                         |                      |                  N/A |
+-----------------------------------------+----------------------+----------------------+

+---------------------------------------------------------------------------------------+
| Processes:                                                                            |
|  GPU   GI   CI        PID   Type   Process name                            GPU Memory |
|        ID   ID                                                             Usage      |
|=======================================================================================|
|  No running processes found                                                           |
+---------------------------------------------------------------------------------------+
```

> 以下是 `nvidia-smi` 命令的输出的详细解释：

```
Sun Jul 30 10:05:42 2023
```

> 这是命令运行的时间和日期。

```
NVIDIA-SMI 535.54.03              Driver Version: 535.54.03    CUDA Version: 12.2
```

> 这是 NVIDIA System Management Interface (SMI) 的版本，驱动程序版本和`CUDA`版本。在这里，驱动程序版本是 `535.54.03`，`CUDA`版本是 `12.2`。

> 接下来是一个表格，包含了关于你的 GPU 的详细信息：

```
| GPU  Name                 Persistence-M | Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |         Memory-Usage | GPU-Util  Compute M. |
|                                         |                      |               MIG M. |
|=========================================+======================+======================|
|   0  Tesla V100S-PCIE-32GB          Off | 00000000:00:0D.0 Off |                    0 |
| N/A   32C    P0              24W / 250W |      0MiB / 32768MiB |      0%      Default |
|                                         |                      |                  N/A |
```

> 这个表格包含了以下信息：

+   `0 Tesla V100S-PCIE-32GB`: 这是你的 GPU 的编号和名称。在这里，你的 GPU 编号是 `0`，名称是 `Tesla V100S-PCIE-32GB`。

+   `Persistence-M`: 这是 GPU 的持久模式状态。如果持久模式被打开，GPU 将不会在没有运行的计算工作负载时关闭。

+   `Bus-Id`: 这是 GPU 在主机系统的总线 ID。

+   `Disp.A`: 这是显示活动状态。如果为 `"Off"`，则表示没有连接显示器。

+   `Volatile Uncorr. ECC`: 这是 GPU ECC 错误计数。在这里，错误计数是 `0`。

+   `Fan`: 这是风扇的速度，但对于某些数据中心 GPU（如 `Tesla`），这个值可能不适用（`N/A`）。

+   `Temp`: 这是 GPU 的当前温度，这里是 `32` 摄氏度。

+   `Perf`: 这是性能状态，`P0` 表示最大性能状态。

+   `Pwr:Usage/Cap`: 这是 GPU 的当前功率使用和最大功率限制。在这里，当前功率使用是 `24` 瓦特，最大功率限制是 `250` 瓦特。

+   `Memory-Usage`: 这是 GPU 的当前内存使用和总内存。在这里，没有任何内存被使用，总内存是 `32768 MiB`（`32 GB`）。

+   `GPU-Util`: 这是 GPU 的当前利用率。在这里，利用率是 `0%`。

+   `Compute M.`: 这是 GPU 的计算模式。`"Default"` 模式允许多个计算应用共享 GPU。

+   `MIG M.`: 这是 Multi-Instance GPU (MIG) 模式。`N/A` 表示你的 GPU 不支持 MIG，或者 MIG 没有被启用。

> 最后，`nvidia-smi` 会列出当前正在 GPU 上运行的进程：

```
+---------------------------------------------------------------------------------------+
| Processes:                                                                            |
|  GPU   GI   CI        PID   Type   Process name                            GPU Memory |
|        ID   ID                                                             Usage      |
|=======================================================================================|
|  No running processes found                                                           |
+---------------------------------------------------------------------------------------+
```

> 在这里，没有任何进程正在使用 GPU。

## GPU Docker 环境的安装和配置

安装`docker`

```
apt -y install apt-transport-https ca-certificates curl software-properties-common

curl -fsSL https://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable"

apt update -y 
apt install docker-ce -y 
```

添加`nvidia`容器工具包APT源

```
distribution=ubuntu22.04 && \
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg && \
curl -s -L https://nvidia.github.io/libnvidia-container/$distribution/libnvidia-container.list | sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
```

安装`nvidia`容器工具包

```
sudo apt-get update && sudo apt-get install -y nvidia-container-toolkit
```

为 `docker` 添加 `nvidia`运行时

```
sudo nvidia-ctk runtime configure --runtime=docker
```

以下输出说明命令执行成功，需要重启`docker`以使配置生效

```
root@aistudio:~# sudo nvidia-ctk runtime configure --runtime=docker
INFO[0000] Loading docker config from /etc/docker/daemon.json
INFO[0000] Config file does not exist, creating new one
INFO[0000] Wrote updated config to /etc/docker/daemon.json
INFO[0000] It is recommended that the docker daemon be restarted.
```

重启`docker`

```
sudo systemctl restart docker
```

查看运行时

```
 docker info | grep Runtimes
```

以下输出，说明`nvidia`容器运行时已经生效了

```
root@aistudio:~# docker info | grep Runtimes
 Runtimes: io.containerd.runc.v2 nvidia runc
```

运行一个测试容器，尝试在容器里使用`nvidia-smi`查看GPU信息

```
docker run --gpus all -it --rm nvcr.io/nvidia/cuda:11.4.3-base-ubuntu20.04 nvidia-smi
```

以下输出说明GPU在容器中工作正常



```
root@aistudio:~/docker-llama2-chat# docker run --gpus all -it --rm nvcr.io/nvidia/cuda:11.4.3-base-ubuntu20.04 nvidia-smi
Unable to find image 'nvcr.io/nvidia/cuda:11.4.3-base-ubuntu20.04' locally
11.4.3-base-ubuntu20.04: Pulling from nvidia/cuda
ca1778b69356: Pull complete
3b595136a210: Pull complete
b0cbedff9be8: Pull complete
569ff44c1698: Pull complete
1c34507dbc74: Pull complete
Digest: sha256:bee557a9785dc70cc3c5084e9b358ad1d64f4b1fc5d87e0fe82c540355a5eba1
Status: Downloaded newer image for nvcr.io/nvidia/cuda:11.4.3-base-ubuntu20.04
Sun Jul 30 03:31:02 2023
+---------------------------------------------------------------------------------------+
| NVIDIA-SMI 535.54.03              Driver Version: 535.54.03    CUDA Version: 12.2     |
|-----------------------------------------+----------------------+----------------------|
| GPU  Name                 Persistence-M | Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |         Memory-Usage | GPU-Util  Compute M. |
|                                         |                      |               MIG M. |
|=========================================+======================+======================|
|   0  Tesla V100S-PCIE-32GB          Off | 00000000:00:0D.0 Off |                    0 |
| N/A   34C    P0              25W / 250W |      0MiB / 32768MiB |      0%      Default |
|                                         |                      |                  N/A |
+-----------------------------------------+----------------------+----------------------+

+---------------------------------------------------------------------------------------+
| Processes:                                                                            |
|  GPU   GI   CI        PID   Type   Process name                            GPU Memory |
|        ID   ID                                                             Usage      |
|=======================================================================================|
|  No running processes found                                                           |
+---------------------------------------------------------------------------------------+
```

其他测试过程可以参考上一节的相关内容。

# 模型的基本运行

## 构造基本镜像

### 创建Dockerfile

创建Dockerfile

```
nano Dockerfile
```

```
FROM nvcr.io/nvidia/pytorch:23.06-py3
RUN pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple && \
    pip install \
    accelerate==0.21.0 \
    bitsandbytes==0.40.2 \
    gradio==3.37.0 \
    protobuf==3.20.3 \
    scipy==1.11.1 \
    sentencepiece==0.1.99 \
    transformers==4.31.0 \
    cpm_kernels \
    torch>=2.0 \
    mdtex2html \
    sse-starlette \
    streamlit>=1.24.0
WORKDIR /app
```

> 这个`Dockerfile`文件定义了一个Docker镜像，它基于NVIDIA提供的PyTorch镜像，并在此基础上做了一些额外的设置和安装。

> 下面是每一行的解释：

1.  `FROM nvcr.io/nvidia/pytorch:23.06-py3`: 这一行表示这个Docker镜像是基于NVIDIA的PyTorch镜像构建的。`23.06-py3`是镜像的标签，指定了PyTorch的版本（23.06）和Python的版本（3）。

1.  `RUN pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple`: 这一行设置了`pip`的全局index-url为清华大学的镜像站，这可以加速在中国大陆的`pip`下载速度。

1.  `pip install \`: 这一行开始了一个`pip`安装命令，用于安装一些Python库。这个命令在后面的几行中继续。

1.  `accelerate==0.21.0 \`, `bitsandbytes==0.40.2 \`, `gradio==3.37.0 \`, `protobuf==3.20.3 \`, `scipy==1.11.1 \`, `sentencepiece==0.1.99 \`, `transformers==4.31.0 \`, `cpm_kernels \`, `torch>=2.0 \`, `mdtex2html \`, `sse-starlette \`, `streamlit>=1.24.0`: 这些行安装了指定版本的各种Python库。

1.  `WORKDIR /app`: 这一行设置了工作目录为`/app`。这意味着当你运行这个Docker镜像时，你将默认在`/app`目录下。

> 总的来说，这个`Dockerfile`创建了一个包含PyTorch和一些其他Python库的Docker镜像，这可以为运行需要这些库的Python代码提供环境。

> 这些库都是Python的第三方库，用于各种不同的目的。下面是每个库的简单介绍：

1.  `accelerate==0.21.0`: Accelerate是由Hugging Face开发的，用于简化在多GPU或TPU上训练PyTorch模型的库。

1.  `bitsandbytes==0.40.2`: bitsandbytes是一个用于深度学习的快速定制优化库，主要用于优化深度学习训练中的内存和计算效率。

1.  `gradio==3.37.0`: Gradio是一个用于快速创建用户界面以展示和测试机器学习模型的库。

1.  `protobuf==3.20.3`: Protocol Buffers（protobuf）是Google的一种数据交换格式，它可以用于序列化结构化数据。

1.  `scipy==1.11.1`: SciPy是一个开源的Python算法库和数学工具包。SciPy包含的模块有最优化、线性代数、积分、插值、特殊函数、快速傅里叶变换、信号处理和图像处理、常微分方程求解和其他科学与工程中常用的计算。

1.  `sentencepiece==0.1.99`: SentencePiece是一个用于神经网络文本生成任务的无监督文本分词器和文本编码器。

1.  `transformers==4.31.0`: Transformers是由Hugging Face开发的，提供了大量预训练模型（如BERT、GPT-2等）和用于处理文本数据的工具。

1.  `cpm_kernels`: 这个库的具体信息我没有找到，可能是一个特定项目或者私有库。

1.  `torch>=2.0`: PyTorch是一个开源的深度学习框架，用于构建和训练神经网络。

1.  `mdtex2html`: 这个库用于将Markdown文本转换为HTML文本。

1.  `sse-starlette`: 这个库是用于Starlette框架的Server-Sent Events的一个实现。

1.  `streamlit>=1.24.0`: Streamlit是一个用于创建机器学习和数据科学web应用的开源库。

> 这些库的组合看起来像是为了运行一些涉及深度学习模型的web应用。

### 创建基本镜像

```
docker build -t chatglm2:base .
```

### 查看镜像

```
docker images
```

运行效果如下

```
chengzh@AIStudio:~$ docker build -t chatglm2:base .
[+] Building 61.7s (7/7) FINISHED                                                                                                      docker:default
 => [internal] load .dockerignore                                                                                                                0.0s
 => => transferring context: 2B                                                                                                                  0.0s
 => [internal] load build definition from Dockerfile                                                                                             0.0s
 => => transferring dockerfile: 455B                                                                                                             0.0s
 => [internal] load metadata for nvcr.io/nvidia/pytorch:23.06-py3                                                                                0.0s
 => CACHED [1/3] FROM nvcr.io/nvidia/pytorch:23.06-py3                                                                                           0.0s
 => [2/3] RUN pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple &&     pip install     accelerate==0.21.0     bitsandby  59.8s
 => [3/3] WORKDIR /app                                                                                                                           0.0s
 => exporting to image                                                                                                                           1.8s
 => => exporting layers                                                                                                                          1.8s
 => => writing image sha256:e2c54e186f0b4d32a5d0bc9c10324095fdd903cde8d772b0368bc62f22248fac                                                     0.0s
 => => naming to docker.io/library/chatglm2:base                                                                                                 0.0s

What's Next?
  View summary of image vulnerabilities and recommendations → docker scout quickview
chengzh@AIStudio:~$ docker images
REPOSITORY         TAG      IMAGE ID       CREATED         SIZE
chatglm2           base     e2c54e186f0b   7 seconds ago   20.4GB                                           
```

## 下载模型

为了能够复用模型文件，提升效率，模型文件现在到宿主机某个特定的目录，在本例中是`/mnt/c/BKF/`，后续再挂接到容器中执行，所以请记住这个路径。

### 安装Git LFS

```
sudo apt-get install git-lfs
git lfs install
```

> `git lfs install`是Git Large File Storage（LFS）的安装命令。Git LFS 是一种 Git 扩展，它用于处理大文件，如音频样本、视频、数据集等。默认情况下，Git 不适合处理大文件，因为它会将所有版本的所有文件都存储在本地仓库中，这可能会导致仓库变得非常大。Git LFS 解决了这个问题，它将大文件存储在远程服务器上，只在本地保留一个轻量级的引用。

> 当你运行 `git lfs install` 命令时，它将执行以下操作：

1.  安装一些必要的钩子（hooks），这些钩子将在执行 `git` 命令时运行，以确保大文件被正确地处理。例如，pre-push 钩子将在推送到远程仓库之前运行，post-checkout 钩子将在检出分支或提交后运行。

1.  设置一些必要的 Git 配置。例如，它将设置 `filter.lfs.clean` 和 `filter.lfs.smudge` 配置，这些配置定义了如何在将大文件添加到仓库和从仓库检出大文件时处理这些文件。

> 注意，在运行 `git lfs install` 命令之前，你需要先安装 Git LFS。你可以从 Git LFS 的 GitHub 仓库下载适合你的操作系统的安装程序。

### 克隆模型项目库

```
git clone https://huggingface.co/THUDM/chatglm2-6b
```

执行效果如下



```
chengzh@AIStudio:/mnt/c/BKF$ git clone https://huggingface.co/THUDM/chatglm2-6b
Cloning into &#x27;chatglm2-6b&#x27;...
remote: Enumerating objects: 172, done.
remote: Counting objects: 100% (73/73), done.
remote: Compressing objects: 100% (58/58), done.
remote: Total 172 (delta 46), reused 15 (delta 15), pack-reused 99
Receiving objects: 100% (172/172), 1.94 MiB | 718.00 KiB/s, done.
Resolving deltas: 100% (84/84), done.
Filtering content: 100% (8/8), 11.63 GiB | 9.45 MiB/s, done.
```

总大小24G

```
chengzh@AIStudio:/mnt/c/BKF/chatglm2-6b$ du -sh .
24G     .
```

# 运行容器

确认已经进入到模型下载目录`/mnt/c/BKF/`

```
chengzh@AIStudio:/mnt/c/BKF$ pwd
/mnt/c/BKF
chengzh@AIStudio:/mnt/c/BKF$ dir
chatglm2-6b  docker-llama2-chat  langchain-ChatGLM
```

使用`chatglm2:base`映像运行容器

```
docker run --gpus all --ipc=host --ulimit memlock=-1 --ulimit stack=67108864 --rm -it -v `pwd`/chatglm2-6b:/app/chatglm2-6b -p 7860:7860 -p 8000:8000 -p 8501:8501 chatglm2:base
```

> 这个命令是用来运行一个Docker容器的。下面是每个部分的解释：

1.  `docker run`: 这是Docker的运行命令，用于启动一个新的容器。

1.  `--gpus all`: 这个选项告诉Docker你想使用主机上的所有GPU。

1.  `--ipc=host`: 这个选项设置了容器的IPC（InterProcess Communication，进程间通信）模式为`host`，这意味着容器将使用主机的IPC命名空间，这在某些需要大量共享内存的应用（如某些深度学习框架）中是很有用的。

1.  `--ulimit memlock=-1 --ulimit stack=67108864`: 这些选项设置了容器的资源限制。`memlock=-1`移除了内存锁定的限制，`stack=67108864`设置了栈大小的限制为64MB。

1.  `--rm`: 这个选项告诉Docker在容器退出时自动删除容器。

1.  `-it`: 这些选项让Docker在新容器中打开一个交互式终端。

1.  `-v` `pwd`/chatglm2-6b:/app/chatglm2-6b: 这个选项挂载了主机上的`chatglm2-6b`目录（位于你当前的工作目录）到容器的`/app/chatglm2-6b`目录。

1.  `-p 7860:7860 -p 8000:8000 -p 8501:8501`: 这些选项将容器的端口映射到主机的端口。例如，`-p 7860:7860`将容器的`7860`端口映射到主机的`7860`端口。

1.  `chatglm2:base`: 这是你想要运行的Docker镜像的名字。

> 总的来说，这个命令启动了一个新的`chatglm2:base`容器，使用了主机的所有GPU和IPC命名空间，移除了内存锁定的限制，设置了栈大小的限制，挂载了一个目录，并映射了一些端口。在容器退出时，它将被自动删除。

命令执行效果如下，而且当前上下文已经切换为容器里面

```
chengzh@AIStudio:/mnt/c/BKF$ docker run --gpus all --ipc=host --ulimit memlock=-1 --ulimit stack=67108864 --rm -it -v `pwd`/chatglm2-6b:/app/chatglm2-6b -p 7860:7860 -p 8000:8000 chatglm2:base

=============
== PyTorch ==
=============

NVIDIA Release 23.06 (build 63009835)
PyTorch Version 2.1.0a0+4136153

Container image Copyright (c) 2023, NVIDIA CORPORATION &amp; AFFILIATES. All rights reserved.

Copyright (c) 2014-2023 Facebook Inc.
Copyright (c) 2011-2014 Idiap Research Institute (Ronan Collobert)
Copyright (c) 2012-2014 Deepmind Technologies    (Koray Kavukcuoglu)
Copyright (c) 2011-2012 NEC Laboratories America (Koray Kavukcuoglu)
Copyright (c) 2011-2013 NYU                      (Clement Farabet)
Copyright (c) 2006-2010 NEC Laboratories America (Ronan Collobert, Leon Bottou, Iain Melvin, Jason Weston)
Copyright (c) 2006      Idiap Research Institute (Samy Bengio)
Copyright (c) 2001-2004 Idiap Research Institute (Ronan Collobert, Samy Bengio, Johnny Mariethoz)
Copyright (c) 2015      Google Inc.
Copyright (c) 2015      Yangqing Jia
Copyright (c) 2013-2016 The Caffe contributors
All rights reserved.

Various files include modifications (c) NVIDIA CORPORATION &amp; AFFILIATES.  All rights reserved.

This container image and its contents are governed by the NVIDIA Deep Learning Container License.
By pulling and using the container, you accept the terms and conditions of this license:
https://developer.nvidia.com/ngc/nvidia-deep-learning-container-license

root@54da0227583b:/app#
```

查看当前目录，并进入到`/app/chatglm2-6b`目录中

```
root@54da0227583b:/app# pwd
/app
root@54da0227583b:/app# dir
chatglm2-6b
root@54da0227583b:/app# cd chatglm2-6b/
root@54da0227583b:/app/chatglm2-6b#
```

在容器上下文里clone `chatglm2-6b`项目

```
 git clone https://github.com/THUDM/ChatGLM2-6B
 CD ChatGLM2-6B
```

> 官方GitHub仓库 `https://github.com/THUDM/ChatGLM2-6B` 提供了运行模型所需的全部配置文件。根据当前的执行方式，这些项目文件将被持久化并存储在主机的本地目录中，具体路径为：`/mnt/c/BKF/chatglm2-6b/ChatGLM2-6b`。这种配置确保了即使在Docker容器周期性地创建和销毁的过程中，相关的项目文件也能够被安全地保留和管理。

安装依赖环境（可选）

```
 pip install -r requirements.txt
```

备注：在下一节实验做完之后可以直接运行以下命令

```
docker run --gpus all --ipc=host --ulimit memlock=-1 --ulimit stack=67108864 -v $(pwd)/chatglm2-6b:/app/chatglm2-6b --restart=always -d -p 7860:7860 -p 8000:8000 -p 8501:8501 --name=chatglm2 chatglm2:base python /app/chatglm2-6b/ChatGLM2-6B/web_demo.py
```

> 这个命令是用来运行一个Docker容器的，和上一个命令相似，但有一些不同的地方。下面是每个部分的解释：

1.  `--gpus all`: 这个选项告诉Docker你想使用主机上的所有GPU。

1.  `--ipc=host`: 这个选项设置了容器的IPC（InterProcess Communication，进程间通信）模式为`host`，这意味着容器将使用主机的IPC命名空间，这在某些需要大量共享内存的应用（如某些深度学习框架）中是很有用的。

1.  `--ulimit memlock=-1 --ulimit stack=67108864`: 这些选项设置了容器的资源限制。`memlock=-1`移除了内存锁定的限制，`stack=67108864`设置了栈大小的限制为64MB。

1.  `-v` `$(pwd)`/chatglm2-6b:/app/chatglm2-6b: 这个选项挂载了主机上的`chatglm2-6b`目录（位于你当前的工作目录）到容器的`/app/chatglm2-6b`目录。

1.  `--restart=always`: 这个选项告诉Docker在容器退出时或者Docker重启时总是重新启动这个容器。

1.  `-d`: 这个选项让Docker在后台运行新的容器，并返回容器ID。

1.  `-p 7860:7860 -p 8000:8000 -p 8501:8501`: 这些选项将容器的端口映射到主机的端口。例如，`-p 7860:7860`将容器的`7860`端口映射到主机的`7860`端口。

1.  `--name=chatglm2`: 这个选项给新的容器命名为`chatglm2`。

1.  `chatglm2:base`: 这是你想要运行的Docker镜像的名字。

1.  `python /app/chatglm2-6b/ChatGLM2-6B/web_demo.py`: 这是你想要在新容器中运行的命令。这个命令启动了一个Python脚本。

> 总的来说，这个命令启动了一个新的`chatglm2:base`容器，使用了主机的所有GPU和IPC命名空间，移除了内存锁定的限制，设置了栈大小的限制，挂载了一个目录，并映射了一些端口。这个容器在退出或者Docker重启时将会自动重启，并在后台运行。在容器中，它运行了一个Python脚本。

使用以下命令和容器进行交互

```
docker exec -it chatglm2 bash
```

删除容器

```
docker rm -f chatglm2
```

又及，仅启用流式API的容器快速运行命令

```
docker run --gpus all --ipc=host --ulimit memlock=-1 --ulimit stack=67108864 -v $(pwd)/chatglm2-6b:/app/chatglm2-6b --restart=always -d -p 8000:8000 --name=chatglm2 chatglm2:base python /app/chatglm2-6b/ChatGLM2-6B/openai_api.py
```

# 使用网页调用模型

## 启动基于 Gradio 的网页版 Demo

修改`web_demo.py`

```
nano web_demo.py
```

修改 6 7行的模型路径为`/app/chatglm2-6b`

108行修改为 `launch(server_name="0.0.0.0", share=False, inbrowser=True)`

```
from transformers import AutoModel, AutoTokenizer
import gradio as gr
import mdtex2html
from utils import load_model_on_gpus

tokenizer = AutoTokenizer.from_pretrained(&quot;/app/chatglm2-6b&quot;, trust_remote_code=True) # 修改模型路径
model = AutoModel.from_pretrained(&quot;/app/chatglm2-6b&quot;, trust_remote_code=True).cuda() # 修改模型路径
# 多显卡支持，使用下面两行代替上面一行，将num_gpus改为你实际的显卡数量
# from utils import load_model_on_gpus
# model = load_model_on_gpus(&quot;THUDM/chatglm2-6b&quot;, num_gpus=2)
model = model.eval()

&quot;&quot;&quot;Override Chatbot.postprocess&quot;&quot;&quot;


def postprocess(self, y):
    if y is None:
        return []
    for i, (message, response) in enumerate(y):
        y[i] = (
            None if message is None else mdtex2html.convert((message)),
            None if response is None else mdtex2html.convert(response),
        )
    return y


gr.Chatbot.postprocess = postprocess


def parse_text(text):
    &quot;&quot;&quot;copy from https://github.com/GaiZhenbiao/ChuanhuChatGPT/&quot;&quot;&quot;
    lines = text.split(&quot;\n&quot;)
    lines = [line for line in lines if line != &quot;&quot;]
    count = 0
    for i, line in enumerate(lines):
        if &quot;```&quot; in line:

            count += 1

            items = line.split(&#x27;`&#x27;)

            if count % 2 == 1:

                lines[i] = f&#x27;&lt;pre&gt;&lt;code class=&quot;language-{items[-1]}&quot;&gt;&#x27;

            else:

                lines[i] = f&#x27;&lt;br&gt;&lt;/code&gt;&lt;/pre&gt;&#x27;

        else:

            if i &gt; 0:

                if count % 2 == 1:

                    line = line.replace(&quot;`&quot;, &quot;\`&quot;)

                    line = line.replace(&quot;&lt;&quot;, &quot;&amp;lt;&quot;)

                    line = line.replace(&quot;&gt;&quot;, &quot;&amp;gt;&quot;)

                    line = line.replace(&quot; &quot;, &quot;&amp;nbsp;&quot;)

                    line = line.replace(&quot;*&quot;, &quot;&amp;ast;&quot;)

                    line = line.replace(&quot;_&quot;, &quot;&amp;lowbar;&quot;)

                    line = line.replace(&quot;-&quot;, &quot;&amp;#45;&quot;)

                    line = line.replace(&quot;.&quot;, &quot;&amp;#46;&quot;)

                    line = line.replace(&quot;!&quot;, &quot;&amp;#33;&quot;)



## 基于 Gradio 的 Web Demo

```python
line = line.replace("(", "&#40;")
line = line.replace(")", "&#41;")
line = line.replace("$", "&#36;")
lines[i] = "<br>" + line

text = "".join(lines)

return text

def predict(input, chatbot, max_length, top_p, temperature, history, past_key_values):
    chatbot.append((parse_text(input), ""))

    for response, history, past_key_values in model.stream_chat(tokenizer, input, history, past_key_values=past_key_values,
                                                                return_past_key_values=True,
                                                                max_length=max_length, top_p=top_p,
                                                                temperature=temperature):
        chatbot[-1] = (parse_text(input), parse_text(response))
        yield chatbot, history, past_key_values

def reset_user_input():
    return gr.update(value="")

def reset_state():
    return [], [], None

with gr.Blocks() as demo:
    gr.HTML("""<h1 align="center">ChatGLM2-6B</h1>""")

    chatbot = gr.Chatbot()

    with gr.Row():
        with gr.Column(scale=4):
            with gr.Column(scale=12):
                user_input = gr.Textbox(show_label=False, placeholder="Input...", lines=10).style(
                    container=False)
            with gr.Column(min_width=32, scale=1):
                submitBtn = gr.Button("Submit", variant="primary")
        with gr.Column(scale=1):
            emptyBtn = gr.Button("Clear History")
            max_length = gr.Slider(0, 32768, value=8192, step=1.0, label="Maximum length", interactive=True)
            top_p = gr.Slider(0, 1, value=0.8, step=0.01, label="Top P", interactive=True)
            temperature = gr.Slider(0, 1, value=0.95, step=0.01, label="Temperature", interactive=True)

    history = gr.State([])
    past_key_values = gr.State(None)

    submitBtn.click(predict, [user_input, chatbot, max_length, top_p, temperature, history, past_key_values],
                    [chatbot, history, past_key_values], show_progress=True)

    submitBtn.click(reset_user_input, [], [user_input])

    emptyBtn.click(reset_state, outputs=[chatbot, history, past_key_values], show_progress=True)

demo.queue().launch(server_name="0.0.0.0", share=False, inbrowser=True) # 可以考虑将 share=False 改成 True，这样可以从公网上调用模型
```

### 启用 Gradio 页面

`python web_demo.py`

```text
root@750840fb9d50:/app/chatglm2-6b/ChatGLM2-6B# python web_demo.py
Loading checkpoint shards: 100%|██████████████████| 7/7 [00:16<00:00,  2.41s/it]
/app/chatglm2-6b/ChatGLM2-6B/web_demo.py:89: GradioDeprecationWarning: The `style` method is deprecated. Please set these arguments in the constructor instead.
  user_input = gr.Textbox(show_label=False, placeholder="Input...", lines=10).style(
Running on local URL:  http://0.0.0.0:7860

To create a public link, set `share=True` in `launch()`.
```

访问容器宿主机 7860 端口

![image-20230907112738278](ChatGLM-2 实验手册.assets/image-20230907112738278.png)

这项功能测试完成之后，在命令行界面中，使用 `ctrl+c` 终止脚本运行

## 启动基于 Streamlit 的网页版 Demo

> 经测试，基于 Streamlit 的网页版 Demo 会更流畅。

### 修改配置文件

`nano web_demo2.py`

修改 14 15 行的模型路径为 `/app/chatglm2-6b`

```python
from transformers import AutoModel, AutoTokenizer

import streamlit as st

st.set_page_config(
    page_title="ChatGLM2-6b 演示",
    page_icon=":robot:",
    layout='wide'
)

@st.cache_resource
def get_model():
    tokenizer = AutoTokenizer.from_pretrained("/app/chatglm2-6b", trust_remote_code=True)
    model = AutoModel.from_pretrained("/app/chatglm2-6b", trust_remote_code=True).cuda()

    # 多显卡支持，使用下面两行代替上面一行，将num_gpus改为你实际的显卡数量
    # from utils import load_model_on_gpus
    # model = load_model_on_gpus("THUDM/chatglm2-6b", num_gpus=2)

    model = model.eval()

    return tokenizer, model

tokenizer, model = get_model()

st.title("ChatGLM2-6B")

max_length = st.sidebar.slider(
    'max_length', 0, 32768, 8192, step=1
)

top_p = st.sidebar.slider(
    'top_p', 0.0, 1.0, 0.8, step=0.01
)

temperature = st.sidebar.slider(
    'temperature', 0.0, 1.0, 0.8, step=0.01
)

if 'history' not in st.session_state:
    st.session_state.history = []

if 'past_key_values' not in st.session_state:
    st.session_state.past_key_values = None

for i, (query, response) in enumerate(st.session_state.history):
    with st.chat_message(name="user", avatar="user"):
        st.markdown(query)

    with st.chat_message(name="assistant", avatar="assistant"):
        st.markdown(response)

with st.chat_message(name="user", avatar="user"):
    input_placeholder = st.empty()

with st.chat_message(name="assistant", avatar="assistant"):
    message_placeholder = st.empty()

prompt_text = st.text_area(label="用户命令输入",
                           height=100,
                           placeholder="请在这儿输入您的命令")

button = st.button("发送", key="predict")

if button:
    input_placeholder.markdown(prompt_text)

    history, past_key_values = st.session_state.history, st.session_state.past_key_values

    for response, history, past_key_values in model.stream_chat(tokenizer, prompt_text, history,
                                                                past_key_values=past_key_values,
                                                                max_length=max_length, top_p=top_p,
                                                                temperature=temperature,
                                                                return_past_key_values=True):
        message_placeholder.markdown(response)

    st.session_state.history = history
    st.session_state.past_key_values = past_key_values
```

运行以下命令启用 Streamlit 输出：

`streamlit run web_demo2.py`

```text
root@750840fb9d50:/app/chatglm2-6b/ChatGLM2-6B# streamlit run web_demo2.py
Collecting usage statistics. To deactivate, set browser.gatherUsageStats to False.

  You can now view your Streamlit app in your browser.

  Network URL: http://172.17.0.3:8501

  External URL: http://52.184.87.231:8501
```

访问宿主机 8501 端口

![image-20230907133749129](ChatGLM-2 实验手册.assets/image-20230907133749129.png)

这项功能测试完成之后，在命令行界面中，使用 `ctrl+c` 终止脚本运行

## 启用 OpenAI 格式的流式 API

修改 `openai_api.py`

`nano openai_api.py`

依然是把模型的路径修改为 `/app/chatglm2-6b`，具体位置在 169 行开始的那一段代码

```python
# coding=utf-8

# Implements API for ChatGLM2-6B in OpenAI's format. (https://platform.openai.com/docs/api-reference/chat)
# Usage: python openai_api.py
# Visit http://localhost:8000/docs for documents.

import time

import torch

import uvicorn

from pydantic import BaseModel, Field

from fastapi import FastAPI, HTTPException

from fastapi.middleware.cors import CORSMiddleware

from contextlib import asynccontextmanager

from typing import Any, Dict, List, Literal, Optional, Union

from transformers import AutoTokenizer, AutoModel

from sse_starlette.sse import ServerSentEvent, EventSourceResponse

@asynccontextmanager
async def lifespan(app: FastAPI): # collects GPU memory
    yield

    if torch.cuda.is_available():
        torch.cuda.empty_cache()
        torch.cuda.ipc_collect()

app = FastAPI(lifespan=lifespan)

app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)

class ModelCard(BaseModel):
    id: str

    object: str = "model"

    created: int = Field(default_factory=lambda: int(time.time()))

    owned_by: str = "owner"
```



# ChatGLM-2 OpenAI API 部署指南

## 模型定义与 API 接口

```python
root: Optional[str] = None

parent: Optional[str] = None

permission: Optional[list] = None

class ModelList(BaseModel):

    object: str = "list"

    data: List[ModelCard] = []

class ChatMessage(BaseModel):

    role: Literal["user", "assistant", "system"]

    content: str

class DeltaMessage(BaseModel):

    role: Optional[Literal["user", "assistant", "system"]] = None

    content: Optional[str] = None

class ChatCompletionRequest(BaseModel):

    model: str

    messages: List[ChatMessage]

    temperature: Optional[float] = None

    top_p: Optional[float] = None

    max_length: Optional[int] = None

    stream: Optional[bool] = False

class ChatCompletionResponseChoice(BaseModel):

    index: int

    message: ChatMessage

    finish_reason: Literal["stop", "length"]

class ChatCompletionResponseStreamChoice(BaseModel):

    index: int

    delta: DeltaMessage

    finish_reason: Optional[Literal["stop", "length"]]

class ChatCompletionResponse(BaseModel):

    model: str

    object: Literal["chat.completion", "chat.completion.chunk"]

    choices: List[Union[ChatCompletionResponseChoice, ChatCompletionResponseStreamChoice]]

    created: Optional[int] = Field(default_factory=lambda: int(time.time()))

@app.get("/v1/models", response_model=ModelList)

async def list_models():

    global model_args

    model_card = ModelCard(id="gpt-3.5-turbo")

    return ModelList(data=[model_card])

@app.post("/v1/chat/completions", response_model=ChatCompletionResponse)

async def create_chat_completion(request: ChatCompletionRequest):

    global model, tokenizer

    if request.messages[-1].role != "user":

        raise HTTPException(status_code=400, detail="Invalid request")

    query = request.messages[-1].content

    prev_messages = request.messages[:-1]

    if len(prev_messages) > 0 and prev_messages[0].role == "system":

        query = prev_messages.pop(0).content + query

    history = []

    if len(prev_messages) % 2 == 0:

        for i in range(0, len(prev_messages), 2):

            if prev_messages[i].role == "user" and prev_messages[i+1].role == "assistant":

                history.append([prev_messages[i].content, prev_messages[i+1].content])

    if request.stream:

        generate = predict(query, history, request.model)

        return EventSourceResponse(generate, media_type="text/event-stream")

    response, _ = model.chat(tokenizer, query, history=history)

    choice_data = ChatCompletionResponseChoice(

        index=0,

        message=ChatMessage(role="assistant", content=response),

        finish_reason="stop"

    )

    return ChatCompletionResponse(model=request.model, choices=[choice_data], object="chat.completion")

async def predict(query: str, history: List[List[str]], model_id: str):

    global model, tokenizer

    choice_data = ChatCompletionResponseStreamChoice(

        index=0,

        delta=DeltaMessage(role="assistant"),

        finish_reason=None

    )

    chunk = ChatCompletionResponse(model=model_id, choices=[choice_data], object="chat.completion.chunk")

    yield "{}".format(chunk.json(exclude_unset=True, ensure_ascii=False))

    current_length = 0

    for new_response, _ in model.stream_chat(tokenizer, query, history):

        if len(new_response) == current_length:

            continue

        new_text = new_response[current_length:]

        current_length = len(new_response)

        choice_data = ChatCompletionResponseStreamChoice(

            index=0,

            delta=DeltaMessage(content=new_text),

            finish_reason=None

        )

        chunk = ChatCompletionResponse(model=model_id, choices=[choice_data], object="chat.completion.chunk")

        yield "{}".format(chunk.json(exclude_unset=True, ensure_ascii=False))

    choice_data = ChatCompletionResponseStreamChoice(

        index=0,

        delta=DeltaMessage(),

        finish_reason="stop"

    )

    chunk = ChatCompletionResponse(model=model_id, choices=[choice_data], object="chat.completion.chunk")

    yield "{}".format(chunk.json(exclude_unset=True, ensure_ascii=False))

    yield '[DONE]'

if __name__ == "__main__":

    tokenizer = AutoTokenizer.from_pretrained("/app/chatglm2-6b", trust_remote_code=True)

    model = AutoModel.from_pretrained("/app/chatglm2-6b", trust_remote_code=True).cuda()

    # 多显卡支持，使用下面两行代替上面一行，将num_gpus改为你实际的显卡数量

    # from utils import load_model_on_gpus

    # model = load_model_on_gpus("/app/chatglm2-6b", num_gpus=2)

    model.eval()

    uvicorn.run(app, host='0.0.0.0', port=8000, workers=1)
```

### 启用流式传输

执行 `python openai_api.py` 启动服务：

```bash
python openai_api.py 
```

### 运行输出效果

启动成功后输出如下：

```text
root@750840fb9d50:/app/chatglm2-6b/ChatGLM2-6B# python openai_api.py 

Loading checkpoint shards: 100%|███████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 7/7 [00:17<00:00,  2.43s/it]

INFO:     Started server process [1195]

INFO:     Waiting for application startup.

INFO:     Application startup complete.

INFO:     Uvicorn running on http://0.0.0.0:8000 (Press CTRL+C to quit)
```

访问 `http://localhost:8000/docs` 查看接口文档。

![image-20230907120015961](ChatGLM-2 实验手册.assets/image-20230907120015961.png)

由此已经实现了 OpenAI 格式的流式 API 部署，可以作为任意基于 ChatGPT 的应用的后端，比如 ChatGPT-Next-Web。

## 对接 ChatGPT-Next 页面

使用 `ChatGPT-Next`：`https://github.com/Yidadaa/ChatGPT-Next-Web`

在模型容器宿主机之外的机器上运行：

```bash
docker run -e BASE_URL=http://aistudio:8000 -p 3000:3000 yidadaa/chatgpt-next-web
```

> 这个命令是在 Docker 中运行一个新的容器。我将为您详细解释一下这个命令的各个部分：
> 
> +   `docker run`：这是 Docker 的一个命令，用于运行一个新的容器。
> 
> +   `-e BASE_URL=http://aistudio:8000`：这部分设置了一个环境变量 `BASE_URL`，并将其值设置为 `http://aistudio:8000`。环境变量是在容器内部可用的，并可以被运行在容器内部的应用程序读取。
> 
> +   `-p 3000:3000`：这部分将容器的 `3000` 端口映射到宿主机的 `3000` 端口。这意味着如果容器内的应用程序在监听 `3000` 端口，那么您可以通过访问宿主机的 `3000` 端口来访问这个应用程序。
> 
> +   `yidadaa/chatgpt-next-web`：这是 Docker 镜像的名称，Docker 将从这个镜像创建一个新的容器。在这种情况下，镜像名称是 `yidadaa/chatgpt-next-web`。
> 
> 这个命令将会启动一个新的 Docker 容器，这个容器将基于 `yidadaa/chatgpt-next-web` 镜像，映射 `3000` 端口，并设置环境变量 `BASE_URL` 的值为 `http://aistudio:8000`。

打开页面进行提问，建议预先修改一下模型的名称。

![image-20230907122658054](ChatGLM-2 实验手册.assets/image-20230907122658054.png)

测试

![image-20230907120057333](ChatGLM-2 实验手册.assets/image-20230907120057333.png)

### 在模型容器本机运行

注意：如果运行在模型容器本机，请使用以下命令：

```bash
docker run -e BASE_URL=http://host.docker.internal:8000 -p 3000:3000 yidadaa/chatgpt-next-web
```

或者（针对Linux上的Docker环境）：

```bash
docker run --add-host=aistudio:$(ip -4 addr show scope global dev eth0 | grep inet | awk '{print $2}' | cut -d / -f 1) -e BASE_URL=http://aistudio:8000 -p 3000:3000 yidadaa/chatgpt-next-web
```

### 查看 UI 容器的后台日志

![image-20230907120130940](ChatGLM-2 实验手册.assets/image-20230907120130940.png)

```text
[User IP]  ::ffff:172.17.0.1

[Time]  8/18/2023, 3:06:27 AM

[Auth] admin did not provide an api key

[Proxy]  v1/chat/completions?path=v1&path=chat&path=completions

[Base Url] http://aistudio:8000

[OpenAI Route] params  { path: [ 'dashboard', 'billing', 'usage' ] }

[Auth] allowed hashed codes:  []

[Auth] got access code:

[Auth] hashed access code: d41d8cd98f00b204e9800998ecf8427e
```



# 日志解读、部署与微调指南

## API 请求日志解读

> 从您提供的日志信息来看，这是一个API请求的日志，其中包含了用户IP、时间、鉴权信息、请求的URL路径等信息。下面是对这些信息的简单解读：

1. `User IP` `::ffff:172.17.0.1`: 这是用户的IP地址，`::ffff:172.17.0.1`是一个IPv6的地址，实际上表示的是IPv4地址`172.17.0.1`。

2. `Time` `8/18/2023, 3:06:27 AM`: 这是请求发生的时间。

3. `Auth` `admin did not provide an api key`: 这表明用户（在这种情况下是管理员）没有提供API密钥进行鉴权。

4. `Proxy` `v1/chat/completions?path=v1&path=chat&path=completions`: 这是被代理的请求路径。

5. `Base Url` `http://aistudio:8000`: 这是服务器的基础URL。

6. `OpenAI Route` `params { path: [ 'dashboard', 'billing', 'usage' ] }`: 这是请求的OpenAI路由参数。

7. `Auth` `allowed hashed codes: []`: 这是允许的哈希码列表，看起来这个列表是空的。

8. `Auth` `got access code::`: 这表明获取到了访问码，但是看起来访问码是空的。

9. `Auth` `hashed access code: d41d8cd98f00b204e9800998ecf8427e`: 这是获取到的访问码的哈希值，但由于原始的访问码是空的，所以这个哈希值实际上是空字符串的MD5哈希值。

> 总的来说，这个日志显示了一个API请求的详细信息，但是看起来这个请求没有提供API密钥，也没有提供访问码，可能无法通过鉴权。

## 模型容器 HTTP 日志解读

在模型容器这边可以看到日志

```
2023-08-18T03:05:07.580954421Z INFO:     172.17.0.1:32930 - "POST /v1/chat/completions?path=v1&path=chat&path=completions HTTP/1.1" 200 OK
2023-08-18T03:06:08.489583122Z INFO:     172.17.0.1:32932 - "POST /v1/chat/completions?path=v1&path=chat&path=completions HTTP/1.1" 200 OK
2023-08-18T03:06:31.109598989Z INFO:     172.17.0.1:32934 - "POST /v1/chat/completions?path=v1&path=chat&path=completions HTTP/1.1" 200 OK
```

> 这条日志信息是HTTP服务器日志的一部分，它提供了关于特定HTTP请求的信息。下面是对这条日志的解读：

+ `2023-08-18T03:06:31.109598989Z`: 这是请求发生的时间，使用的是ISO 8601的日期和时间表示法，包括了日期、时间和时区（Z表示UTC时区）。

+ `INFO`: 这是日志的级别，表示这是一条信息级别的日志，通常用于描述正常的运行情况。

+ `172.17.0.1:32934`: 这是发起请求的客户端的IP地址和端口号。

+ `"POST /v1/chat/completions?path=v1&path=chat&path=completions HTTP/1.1"`: 这是请求的详细信息，包括了HTTP的方法（POST）、请求的路径（`/v1/chat/completions`）以及路径参数（`path=v1&path=chat&path=completions`）和HTTP的版本（HTTP/1.1）。

+ `200 OK`: 这是服务器对请求的响应状态码和状态消息，200表示请求已成功处理，OK是对应的状态消息。

> 总的来说，这条日志告诉我们在特定的时间，来自特定IP地址和端口的客户端向服务器发起了一个POST请求，请求的路径是`/v1/chat/completions`，路径参数是`path=v1&path=chat&path=completions`，服务器已经成功处理了这个请求并返回了200 OK的响应。

这项功能测试完成之后，在命令行界面中，使用`ctrl+c`终止运行脚本运行

## 部署备注

### 备注 1：使用 docker-compose 正式部署

正式部署可以使用以下`docker-compose`文件

```
version: '3.1'

services:

  chatgpt-next-web:

    image: yidadaa/chatgpt-next-web

    restart: always

    environment:

      BASE_URL: http://aistudio:8000

    ports:

      - 3000:3000
```

### 备注 2：使用 chatbot-ui 部署

另一个可用项目`chatbot-ui`：`https://github.com/mckaywrigley/chatbot-ui`

```
docker run -e OPENAI_API_HOST=http://192.168.14.111:8000 -e OPENAI_API_KEY=sk-XXXXXXXXXXXXXXXXXXXX -p 3000:3000 ghcr.io/mckaywrigley/chatbot-ui:main
```

## 微调

> 参照`https://github.com/THUDM/ChatGLM2-6B/tree/main`和`https://github.com/THUDM/P-tuning-v2/ptuning`对`ChatGLM2-6B`模型使用基于`P-Tuning v2`的微调。`P-Tuning v2`将需要微调的参数量减少到原来的 0.1%，再通过模型量化、`Gradient Checkpoint`等方法，最低只需要 7GB 显存即可运行。

### 环境准备

使用`chatglm2-6b`作为测试环境

```
docker run --gpus all --ipc=host --ulimit memlock=-1 --ulimit stack=67108864 -v $(pwd)/chatglm2-6b:/app/chatglm2-6b --restart=always -d -p 7860:7860 -p 8000:8000 -p 8501:8501 --name=chatglm2 chatglm2:base python /app/chatglm2-6b/ChatGLM2-6B/openai_api.py
```

进入容器上下文

```
docker exec -it chatglm2 bash
```

确定现在位于正确的路径下 `/app/chatglm2-6b/ChatGLM2-6B`

```
root@1e141c20dc2e:/app/chatglm2-6b/ChatGLM2-6B# pwd

/app/chatglm2-6b/ChatGLM2-6B

root@1e141c20dc2e:/app/chatglm2-6b/ChatGLM2-6B# dir

FAQ.md         README.md     __pycache__  chatglm2-6b  evaluation     ptuning           resources  web_demo.py

MODEL_LICENSE  README_EN.md  api.py       cli_demo.py  openai_api.py  requirements.txt  utils.py   web_demo2.py
```

下面以 [ADGEN](https://aclanthology.org/D19-1321.pdf) (广告生成) 数据集为例介绍代码的使用方法。

ADGEN 数据集任务为根据输入（`content`）生成一段广告词（`summary`）。

```json
{
    "content": "类型#上衣*版型#宽松*版型#显瘦*图案#线条*衣样式#衬衫*衣袖型#泡泡袖*衣款式#抽绳",
    "summary": "这件衬衫的款式非常的宽松，利落的线条可以很好的隐藏身材上的小缺点，穿在身上有着很好的显瘦效果。领口装饰了一个可爱的抽绳，漂亮的绳结展现出了十足的个性，配合时尚的泡泡袖型，尽显女性甜美可爱的气息。"
}
```

从 [Google Drive](https://drive.google.com/file/d/13_vf0xRTQsyneRKdD1bZIr93vBGOczrk/view?usp=sharing) 或者 [Tsinghua Cloud](https://cloud.tsinghua.edu.cn/f/b3f119a008264b1cabd1/?dl=1) 下载处理好的 ADGEN 数据集，将解压后的 `AdvertiseGen` 目录放到本目录下。

### 训练

进入`ptuning`目录并查看目录内容

```
cd ptuning
```

```
root@1e141c20dc2e:/app/chatglm2-6b/ChatGLM2-6B# cd ptuning/

root@1e141c20dc2e:/app/chatglm2-6b/ChatGLM2-6B/ptuning# dir

AdvertiseGen  __pycache__   deepspeed.json        evaluate.sh           main.py  train.sh       trainer.py          web_demo.py

README.md     arguments.py  ds_train_finetune.sh  evaluate_finetune.sh  output   train_chat.sh  trainer_seq2seq.py  web_demo.sh
```

安装依赖环境

```
pip install rouge_chinese nltk jieba datasets
```

> 这个命令是用来安装四个Python库的。以下是这些库的简单介绍：

1. `rouge_chinese`: 这是一个用于评估自然语言生成模型（如机器翻译、文本摘要等）的库，特别是针对中文。它提供了ROUGE（Recall-Oriented Understudy for Gisting Evaluation）的评价指标，这是一种常用的评价自然语言生成模型的方法。

2. `nltk`: NLTK（Natural Language Toolkit）是一个非常强大的自然语言处理库，它提供了大量的功能，包括词性标注、命名实体识别、文本分类、文本挖掘、词干提取、语义理解等。

3. `jieba`: Jieba是一个非常流行的中文分词库。分词是自然语言处理中的一个重要步骤，特别是对于中文。Jieba提供了基于词典的分词方法，也支持新词识别和自定义词典。

4. `datasets`: 这是Hugging Face公司开发的一个库，用于方便地加载和预处理各种自然语言处理的数据集。Hugging Face是一个专注于自然语言处理的开源公司，他们开发了很多有用的工具和模型，如`Transformers`库。

> 通过`pip install`命令，你可以将这些库安装到你的Python环境中，然后在你的代码中使用这些库的功能。

`main.py`解析

> 这个Python脚本主要用于微调（`fine-tuning`）基于Hugging Face库的序列到序列（`sequence-to-sequence`）模型。这是一个常用的训练和评估自然语言处理模型的脚本，适用于诸如机器翻译、文本生成、文本摘要等任务。

> 以下是一些主要的功能点：

1. 数据加载和预处理：脚本加载了训练、验证和测试数据集，并对数据进行了预处理，包括对输入和目标进行分词，以及对数据进行必要的截断和填充。

2. 模型加载：脚本加载了预训练的模型和分词器。如果指定了模型的检查点，脚本还会加载检查点中的模型状态。

3. 训练：如果指定了训练参数，脚本会对模型进行训练，并在训练结束后保存模型。

4. 评估和预测：如果指定了评估或预测参数，脚本会对模型进行评估或预测，并计算一些评价指标，如ROUGE和BLEU。

5. 日志和指标记录：脚本还包含了一些日志和指标记录的代码，用于记录训练和评估过程中的信息。

> 总的来说，这个脚本提供了一个完整的流程，用于加载数据、加载和微调模型、进行模型评估和预测，并记录相关的信息。

`train.sh`解析

```
PRE_SEQ_LEN=128

LR=2e-2

NUM_GPUS=1

torchrun --standalone --nnodes=1 --nproc-per-node=$NUM_GPUS main.py \

    --do_train \

    --train_file AdvertiseGen/train.json \

    --validation_file AdvertiseGen/dev.json \

    --preprocessing_num_workers 10 \

    --prompt_column content \

    --response_column summary \

    --overwrite_cache \

    --model_name_or_path /app/chatglm2-6b \ #此处需要进行模型路径的替换

    --output_dir output/adgen-chatglm2-6b-pt-$PRE_SEQ_LEN-$LR \

    --overwrite_output_dir \

    --max_source_length 64 \

    --max_target_length 128 \

    --per_device_train_batch_size 1 \

    --per_device_eval_batch_size 1 \

    --gradient_accumulation_steps 16 \

    --predict_with_generate \

    --max_steps 3000 \

    --logging_steps 10 \

    --save_steps 1000 \

    --learning_rate $LR \

    --pre_seq_len $PRE_SEQ_LEN \

    --quantization_bit 4
```

> 这个脚本是用来启动一个PyTorch训练任务的。它使用了`torchrun`命令，这是PyTorch的一个命令行工具，用于在多GPU或多节点环境中运行分布式训练。

> 以下是脚本中每个参数的解释：

1. `PRE_SEQ_LEN=128`: 设置预处理序列长度为128。

2. `LR=2e-2`: 设置学习率为0.02。

3. `NUM_GPUS=1`: 设置使用的GPU数量为1。

> `torchrun`命令的参数：

1. `--standalone`: 运行一个独立的训练任务，不需要与其他节点通信。

2. `--nnodes=1`: 设置使用的节点数量为1。

3. `--nproc-per-node=$NUM_GPUS`: 每个节点上的进程数量，这里设置为1，即每个节点上运行一个进程。

> 接下来是`main.py`脚本的参数：

1. `--do_train`: 进行训练。

2. `--train_file AdvertiseGen/train.json`: 训练数据的文件路径。

3. `--validation_file AdvertiseGen/dev.json`: 验证数据的文件路径。

4. `--preprocessing_num_workers 10`: 用于数据预处理的工作进程数量。

5. `--prompt_column content`: 输入列的名称。

6. `--response_column summary`: 输出列的名称。

7. `--overwrite_cache`: 如果设置，将在运行前清除缓存。

8. `--model_name_or_path /app/chatglm2-6b`: 预训练模型的名称或路径。

9. `--output_dir output/adgen-chatglm2-6b-pt-$PRE_SEQ_LEN-$LR`: 输出目录的路径，其中包括训练过程中保存的模型和其他文件。

10. `--overwrite_output_dir`: 如果设置，将在运行前清除输出目录。

11. `--max_source_length 64`: 输入序列的最大长度。

12. `--max_target_length 128`: 输出序列的最大长度。

13. `--per_device_train_batch_size 1`: 每个设备的训练批次大小。

14. `--per_device_eval_batch_size 1`: 每个设备的评估批次大小。

15. `--gradient_accumulation_steps 16`: 梯度累积步数，这可以有效地增加总的批次大小。

16. `--predict_with_generate`: 使用生成模式进行预测。

17. `--max_steps 3000`: 训练的最大步数。

18. `--logging_steps 10`: 每隔多少步记录一次日志。

19. `--save_steps 1000`: 每隔多少步保存一次模型。

20. `--learning_rate $LR`: 学习率。

21. `--pre_seq_len $PRE_SEQ_LEN`: 预处理序列长度。

22. `--quantization_bit 4`: 量化位数，这是用于模型压缩的技术。

> 这个脚本的目的是使用指定的预训练模型在给定的训练数据上进行训练，并将训练过程中的模型和其他文件保存到指定的输出目录。

启动`P-Tuning v2`模型训练

```
bash train.sh
```



# 模型训练配置与日志分析

## 训练超参数配置

`train.sh` 中的 `PRE_SEQ_LEN` 和 `LR` 分别是 soft prompt 长度和训练的学习率，可以进行调节以取得最佳的效果。`P-Tuning-v2` 方法会冻结全部的模型参数，可通过调整 `quantization_bit` 来被原始模型的量化等级，不加此选项则为 `FP16` 精度加载。

在默认配置 `quantization_bit=4`、`per_device_train_batch_size=1`、`gradient_accumulation_steps=16` 下，`INT4` 的模型参数被冻结，一次训练迭代会以 `1` 的批处理大小进行 `16` 次累加的前后向传播，等效为 `16` 的总批处理大小，此时最低只需 `6.7G` 显存。若想在同等批处理大小下提升训练效率，可在二者乘积不变的情况下，加大 `per_device_train_batch_size` 的值，但也会带来更多的显存消耗，请根据实际情况酌情调整。

## 训练过程中的日志片段

```
{'loss': 3.4834, 'learning_rate': 0.0166, 'epoch': 0.07}
{'loss': 3.4369, 'learning_rate': 0.016533333333333334, 'epoch': 0.07}
{'loss': 3.5347, 'learning_rate': 0.016466666666666668, 'epoch': 0.07}
{'loss': 3.4491, 'learning_rate': 0.016399999999999998, 'epoch': 0.08}
{'loss': 3.4395, 'learning_rate': 0.01633333333333333, 'epoch': 0.08}
{'loss': 3.396, 'learning_rate': 0.01626666666666667, 'epoch': 0.08}
{'loss': 3.4089, 'learning_rate': 0.016200000000000003, 'epoch': 0.08}
{'loss': 3.4193, 'learning_rate': 0.016133333333333333, 'epoch': 0.08}
{'loss': 3.5055, 'learning_rate': 0.016066666666666667, 'epoch': 0.08}
{'loss': 3.4632, 'learning_rate': 0.016, 'epoch': 0.08}
{'loss': 3.4498, 'learning_rate': 0.015933333333333334, 'epoch': 0.09}
{'loss': 3.385, 'learning_rate': 0.015866666666666668, 'epoch': 0.09}
{'loss': 3.4645, 'learning_rate': 0.0158, 'epoch': 0.09}
{'loss': 3.5168, 'learning_rate': 0.015733333333333332, 'epoch': 0.09}
{'loss': 3.4531, 'learning_rate': 0.015666666666666666, 'epoch': 0.09}
{'loss': 3.4663, 'learning_rate': 0.015600000000000001, 'epoch': 0.09}
{'loss': 3.4403, 'learning_rate': 0.015533333333333333, 'epoch': 0.09}
{'loss': 3.4088, 'learning_rate': 0.015466666666666667, 'epoch': 0.09}
```

这是模型训练过程中的日志输出。每一行都代表一个训练阶段的统计信息，包括以下几个字段：

+   `loss`：这是模型在当前阶段的平均损失。损失越低，模型的预测结果就越接近实际结果。在这个例子中，损失值开始是 3.4834，然后有所下降，但也有一些波动。这是正常的，因为模型在学习过程中会不断调整参数以最小化损失。

+   `learning_rate`：这是当前阶段的学习率。学习率是一种超参数，用于控制模型参数更新的速度。较高的学习率可能使学习过程更快，但也可能导致模型在最优解附近震荡而无法收敛。较低的学习率可能使学习过程更稳定，但也可能导致学习过程过慢。在这个例子中，学习率从 0.0166 开始，然后逐渐减小。这可能是因为使用了学习率衰减策略，随着训练的进行，降低学习率，以便更精细地优化模型参数。

+   `epoch`：这是当前阶段的训练轮数。一轮（`epoch`）意味着模型已经看过了所有的训练数据一次。在这个例子中，每个阶段的训练轮数都在增加，这意味着模型正在不断地学习训练数据。

总的来说，这个日志显示了模型训练过程中的一些关键指标，可以帮助你了解模型的学习情况，以及是否需要调整学习率或其他超参数。

## 训练过程中的 GPU 负载情况

```
chengzh@AIStudio:~$ nvidia-smi

Fri Aug  4 16:58:24 2023

+---------------------------------------------------------------------------------------+

| NVIDIA-SMI 535.86.01              Driver Version: 536.67       CUDA Version: 12.2     |

|-----------------------------------------+----------------------+----------------------+

| GPU  Name                 Persistence-M | Bus-Id        Disp.A | Volatile Uncorr. ECC |

| Fan  Temp   Perf          Pwr:Usage/Cap |         Memory-Usage | GPU-Util  Compute M. |

|                                         |                      |               MIG M. |

|=========================================+======================+======================|

|   0  NVIDIA GeForce RTX 4090        On  | 00000000:01:00.0 Off |                  Off |

| 34%   67C    P2             346W / 450W |   8505MiB / 24564MiB |     97%      Default |

|                                         |                      |                  N/A |

+-----------------------------------------+----------------------+----------------------+

+---------------------------------------------------------------------------------------+

| Processes:                                                                            |

|  GPU   GI   CI        PID   Type   Process name                            GPU Memory |

|        ID   ID                                                             Usage      |

|=======================================================================================|

|    0   N/A  N/A      1325      C   /python3.10                               N/A      |

+---------------------------------------------------------------------------------------+
```

从 `nvidia-smi` 输出来看，这块 NVIDIA GeForce RTX 4090 显卡的使用情况如下：

+   GPU 利用率：97%，表示你的显卡正在进行大量的计算任务。

+   显卡温度：67°C，这是一个正常的工作温度，但如果持续高负载运行，可能需要确保有足够的冷却。

+   显卡电源使用：346W，接近显卡的最大电源容量（450W）。

+   显卡内存使用：8505MiB，总共有 24564MiB，表示你的显卡内存使用率约为 34.6%。

这表明你的显卡正在进行一项计算密集型任务，但还有一些剩余的显卡内存可供使用。如果你的应用程序需要更多的 GPU 内存，你可能需要优化你的代码或者考虑使用更大内存的显卡。
请注意，持续的高 GPU 利用率可能会导致显卡过热，特别是如果冷却系统不足的情况下。你应该密切关注你的显卡温度，并确保你的系统有足够的冷却。

## 使用 `nvidia-smi dmon` 查看 GPU 负载

```
chengzh@AIStudio:~$ nvidia-smi dmon

# gpu    pwr  gtemp  mtemp     sm    mem    enc    dec    jpg    ofa   mclk   pclk

# Idx      W      C      C      %      %      %      %      %      %    MHz    MHz

    0    348     68      -     97     73      0      0      0      0  10251   2745

    0    348     68      -     96     74      0      0      0      0  10251   2745

    0    349     68      -     95     73      0      0      0      0  10251   2745

    0    349     68      -     94     71      0      0      0      0  10251   2745

    0    347     68      -     94     71      0      0      0      0  10251   2745

    0    351     68      -     97     74      0      0      0      0  10251   2745

    0    349     68      -     97     74      0      0      0      0  10251   2745

    0    348     68      -     97     73      0      0      0      0  10251   2745

    0    348     68      -     96     73      0      0      0      0  10251   2745

    0    349     68      -     93     71      0      0      0      0  10251   2745

    0    348     68      -     97     73      0      0      0      0  10251   2745

    0    350     68      -     97     74      0      0      0      0  10251   2745

    0    349     68      -     97     74      0      0      0      0  10251   2745

    0    349     67      -     96     73      0      0      0      0  10251   2745

    0    348     68      -     93     71      0      0      0      0  10251   2745

    0    351     68      -     97     74      0      0      0      0  10251   2745

    0    350     68      -     97     74      0      0      0      0  10251   2745

    0    349     68      -     97     74      0      0      0      0  10251   2745

    0    349     69      -     96     73      0      0      0      0  10251   2745

    0    348     68      -     93     71      0      0      0      0  10251   2745

    0    349     68      -     95     72      0      0      0      0  10251   2745
```



# GPU监控与模型训练推理报告

## GPU监控数据分析

### 原始数据

```
0    349     68      -     97     74      0      0      0      0  10251   2745
0    348     69      -     97     74      0      0      0      0  10251   2745
0    349     69      -     96     73      0      0      0      0  10251   2745
0    350     67      -     96     74      0      0      0      0  10251   2745
0    349     68      -     94     72      0      0      0      0  10251   2745
0    348     68      -     96     73      0      0      0      0  10251   2745
```

### 关键指标统计

从给出的数据中，我们可以计算出`gtemp`（GPU温度）、`sm`（流多处理器利用率）和`mem`（显存利用率）的平均数值。

+   `gtemp`（GPU温度）的平均值：68.3°C。
+   `sm`（流多处理器利用率）的平均值：95.9%。
+   `mem`（显存利用率）的平均值：73.1%。

### 数据分析

这些数值表明：

1.  GPU的温度在68.3°C，这个温度在大多数情况下都是安全的。通常，只有当GPU的温度超过85°C时，才需要考虑可能的过热问题。

1.  流多处理器利用率平均为95.9%，这意味着GPU的计算能力被高度利用。这通常表明GPU正在进行大量的并行计算，如深度学习训练。

1.  显存利用率平均为73.1%，这意味着大约三分之二的显存正在被使用。这可能表明你的模型或数据集占用了大量的显存，或者你正在同时运行多个需要显存的任务。

总的来说，这些数据表明你的GPU正在进行大量的计算，并且在正常的工作温度下运行。然而，由于显存利用率较高，你可能需要注意是否有足够的显存来处理更大的模型或数据集。如果显存不足，你可能需要优化你的模型或数据处理流程，或者考虑使用具有更大显存的GPU。

## 模型训练完成报告

完成之后的输出，用时2:09:21

```
Training completed. Do not forget to share your model on huggingface.co/models =)

{'train_runtime': 7761.6199, 'train_samples_per_second': 6.184, 'train_steps_per_second': 0.387, 'train_loss': 3.402895772298177, 'epoch': 0.42}

100%|████████████████████| 3000/3000 [2:09:21<00:00,  2.59s/it]

***** train metrics *****

  epoch                    =       0.42

  train_loss               =     3.4029

  train_runtime            = 2:09:21.61

  train_samples            =     114599

  train_samples_per_second =      6.184

  train_steps_per_second   =      0.387
```

这是一个深度学习模型训练完成后的输出报告，主要包含了以下信息：

1.  `train_runtime`: 这是模型训练的总时间，以秒为单位。在这个例子中，模型训练了7761.6199秒，也就是大约2小时9分钟。

1.  `train_samples_per_second`: 这是模型训练的速度，以每秒处理的样本数来衡量。在这个例子中，模型每秒处理了大约6.184个样本。

1.  `train_steps_per_second`: 这是模型训练的速度，以每秒执行的训练步骤数来衡量。在这个例子中，模型每秒执行了大约0.387个训练步骤。

1.  `train_loss`: 这是模型训练的损失值，它反映了模型在训练数据上的预测错误程度。在这个例子中，训练损失为3.4029。一般来说，训练损失越低，模型在训练数据上的表现就越好。

1.  `epoch`: 这是模型训练的轮数，每轮训练表示模型已经完整地遍历了一次训练数据。在这个例子中，模型训练了0.42轮。这可能意味着训练在未完全遍历所有训练数据的情况下就已经停止。

1.  `train_samples`: 这是模型训练过程中处理的样本总数。在这个例子中，模型处理了114599个样本。

这些信息可以帮助你理解模型的训练过程，以及模型在训练数据上的表现。

## 全参数微调（可选）

```
bash ds_train_finetune.sh
```

[注](https://github.com/microsoft/DeepSpeed)：需要安装 `Deepspeed`

## 模型推理

### 分析 evaluate.sh

```
PRE_SEQ_LEN=128
CHECKPOINT=adgen-chatglm2-6b-pt-128-2e-2
STEP=3000
NUM_GPUS=1
torchrun --standalone --nnodes=1 --nproc-per-node=$NUM_GPUS main.py \
    --do_predict \
    --validation_file AdvertiseGen/dev.json \
    --test_file AdvertiseGen/dev.json \
    --overwrite_cache \
    --prompt_column content \
    --response_column summary \
    --model_name_or_path /app/chatglm2-6b \ #修改为模型当前路径
    --ptuning_checkpoint ./output/$CHECKPOINT/checkpoint-$STEP \
    --output_dir ./output/$CHECKPOINT \
    --overwrite_output_dir \
    --max_source_length 64 \
    --max_target_length 64 \
    --per_device_eval_batch_size 1 \
    --predict_with_generate \
    --pre_seq_len $PRE_SEQ_LEN \
    --quantization_bit 4
```

这个命令是使用PyTorch的`torchrun`工具来运行一个名为`main.py`的Python脚本。让我们一行一行地解析这个命令：

1.  `PRE_SEQ_LEN=128`、`CHECKPOINT=adgen-chatglm2-6b-pt-128-2e-2`、`STEP=3000` 和 `NUM_GPUS=1`：这四行设置了一些环境变量，这些变量在下面的命令中会被使用。

1.  `torchrun --standalone --nnodes=1 --nproc-per-node=$NUM_GPUS main.py`：`torchrun`是PyTorch的一个命令行工具，用于运行分布式任务。`--standalone`表示这是一个独立的任务，不依赖于任何其他任务。`--nnodes=1`表示只有一个节点参与计算。`--nproc-per-node=$NUM_GPUS`表示每个节点上有多少个进程，这里设置为环境变量`NUM_GPUS`的值，即1。`main.py`是要运行的Python脚本。

1.  `--do_predict`：这是一个标志，表示这个任务是进行预测，而不是训练。

1.  `--validation_file AdvertiseGen/dev.json` 和 `--test_file AdvertiseGen/dev.json`：这两个选项指定了验证集和测试集的文件路径。

1.  `--overwrite_cache`：如果设置了这个选项，那么在处理数据时，如果缓存文件已经存在，就会被覆盖。

1.  `--prompt_column content` 和 `--response_column summary`：这两个选项指定了输入和输出的列名。

1.  `--model_name_or_path /app/chatglm2-6b`：这个选项指定了模型的名称或路径。

1.  `--ptuning_checkpoint ./output/$CHECKPOINT/checkpoint-$STEP`：这个选项指定了预训练模型的检查点文件的路径。

1.  `--output_dir ./output/$CHECKPOINT`：这个选项指定了输出文件的目录。

1.  `--overwrite_output_dir`：如果设置了这个选项，那么在输出文件时，如果目录已经存在，就会被覆盖。

1.  `--max_source_length 64` 和 `--max_target_length 64`：这两个选项指定了输入和输出的最大长度。

1.  `--per_device_eval_batch_size 1`：这个选项指定了每个设备的评估批次大小。

1.  `--predict_with_generate`：这是一个标志，表示在预测时使用生成模式。

1.  `--pre_seq_len $PRE_SEQ_LEN`：这个选项指定了预处理序列的长度，这里设置为环境变量`PRE_SEQ_LEN`的值，即128。

1.  `--quantization_bit 4`：这个选项指定了量化位数，这是一个用于模型压缩的技术，可以减小模型的大小和内存占用，但可能会牺牲一些精度。

### 执行推理

```
bash evaluate.sh
```

执行过程的输出

```
  5%|███████▊                                                                                                                                                  | 54/1070 [01:37<27:54,  1.65s/it][INFO|configuration_utils.py:577] 2023-08-04 11:11:02,568 >> Generate config GenerationConfig {
  "_from_model_config": true,
  "eos_token_id": 2,
  "pad_token_id": 0,
  "transformers_version": "4.30.2"
}
```

这是在执行你的shell脚本过程中，模型生成配置（`GenerationConfig`）的相关信息。让我们来看看这个日志的具体内容：

+   `5%|███████▊ | 54/1070 [01:37<27:54, 1.65s/it]`：这是一个进度条，表示当前处理进度。在这个例子中，已经处理了54个任务，总共有1070个任务，已经完成了大约5%。已经过去的时间是1分37秒，预计还需要27分54秒。

+   `[INFO|configuration_utils.py:577] 2023-08-04 11:11:02,568 >> Generate config GenerationConfig { "_from_model_config": true, "eos_token_id": 2, "pad_token_id": 0, "transformers_version": "4.30.2" }`：这是模型生成配置的日志信息。`INFO`表示这是一条信息级别的日志，`configuration_utils.py:577`表示这条日志来自`configuration_utils.py`文件的第577行。然后是日志的时间戳，`2023-08-04 11:11:02,568`。

+   `"_from_model_config": true`：表示这个生成配置是从模型配置中获取的。

+   `"eos_token_id": 2`：表示结束符（End of Sentence）的标记ID是2，这个标记用于指示模型生成的句子的结束。

+   `"pad_token_id": 0`：表示填充符（Padding）的标记ID是0，这个标记用于在批处理中对较短的句子进行填充，以使所有句子的长度相同。

+   `"transformers_version": "4.30.2"`：表示使用的`transformers`库的版本是`4.30.2`。

这个日志输出主要是为了帮助你了解模型的生成配置，以及当前处理进度。

经过0:33:17，推理完成

```
Prefix dict has been built successfully.
08/04/2023 11:42:36 - DEBUG - jieba - Prefix dict has been built successfully.
100%|████████████████████████| 1070/1070 [33:13<00:00,  1.86s/it]

***** predict metrics *****

  predict_bleu-4             =     7.7979

  predict_rouge-1            =    29.8432

  predict_rouge-2            =     6.5415

  predict_rouge-l            =    24.1092

  predict_runtime            = 0:33:17.10

  predict_samples            =       1070

  predict_samples_per_second =      0.536

  predict_steps_per_second   =      0.536
```



# 模型预测报告与部署

这是一个深度学习模型预测（或称为评估或测试）完成后的输出报告，主要包含了以下信息：

1. `predict_bleu-4`: 这是BLEU-4分数，它是一种用于评估机器翻译模型的指标，特别是在句子级别的评估。BLEU分数的范围是0-100，分数越高，表示模型的预测结果与参考结果越接近。在这个例子中，BLEU-4分数是7.7979。

1. `predict_rouge-1`, `predict_rouge-2` 和 `predict_rouge-l`: 这些是ROUGE分数，是一种用于评估自动文摘和机器翻译的指标。ROUGE-1、ROUGE-2和ROUGE-L分别对应于单词级、二元词组级和最长公共子序列的匹配。这些分数的范围也是0-100，分数越高，表示模型的预测结果与参考结果越接近。在这个例子中，ROUGE-1分数是29.8432，ROUGE-2分数是6.5415，ROUGE-L分数是24.1092。

1. `predict_runtime`: 这是模型预测的总时间，以秒为单位。在这个例子中，模型预测了33分钟17秒。

1. `predict_samples`: 这是模型预测过程中处理的样本总数。在这个例子中，模型处理了1070个样本。

1. `predict_samples_per_second` 和 `predict_steps_per_second`: 这是模型预测的速度，以每秒处理的样本数和每秒执行的预测步骤数来衡量。在这个例子中，这两个数值都是0.536。

这些信息可以帮助你理解模型的预测过程，以及模型在预测数据上的表现。

模型的质量通常是通过比较其在某个特定任务上的表现与其他已知模型或基准的表现来评估的。在这个情况下，我们有`BLEU-4`和`ROUGE`（包括`ROUGE-1`、`ROUGE-2`和`ROUGE-L`）分数。这些都是评估机器翻译或自动文摘任务的常用指标。

1. `BLEU-4`: 在这个例子中，BLEU-4分数是7.7979。BLEU分数的范围是0-100，分数越高，表示模型的预测结果与参考结果越接近。因此，7.7979的BLEU-4分数相对较低，这可能表明模型的预测质量有待提高。

1. `ROUGE-1`, `ROUGE-2` 和 `ROUGE-L`: 在这个例子中，ROUGE-1分数是29.8432，ROUGE-2分数是6.5415，ROUGE-L分数是24.1092。这些分数的范围也是0-100，分数越高，表示模型的预测结果与参考结果越接近。这些ROUGE分数也相对较低，特别是ROUGE-2分数，这可能表明模型在捕捉更长的词组匹配方面的表现较差。

根据这些评估指标，我们可以初步判断，这个模型在这个特定任务上的表现可能不是非常理想。然而，这并不意味着模型是无用的。可能需要进一步的优化和调整，例如使用更多或更高质量的训练数据，或者调整模型的参数和结构，以提高其表现。

评测指标为中文 `Rouge score` 和 `BLEU-4`。生成的结果保存在 `./output/adgen-chatglm2-6b-pt-128-2e-2/generated_predictions.txt`。

## 模型部署

### 分析 web_demo.py

这个Python脚本主要执行了以下几个功能：

1. 导入必要的库和模块：这包括一些基本的Python库（如`os`和`sys`），以及一些用于机器学习和深度学习的库（如`torch`和`transformers`）。此外，还导入了用于构建和运行交互式用户界面的`gradio`库，以及用于将Markdown文本转换为HTML的`mdtex2html`库。

1. 定义了一些辅助函数：这些函数用于处理和格式化聊天机器人的输入和输出，以及进行模型预测。

1. 构建了一个交互式用户界面：这个界面包括一个聊天机器人窗口，用户可以在其中输入文本，并看到机器人的回复。界面还包括一些滑动条和按钮，用于控制模型的参数和操作。

1. 加载和配置模型：脚本加载了一个预训练的`transformers`模型，并进行了一些配置。如果指定了量化位数，模型还会进行量化操作以减少内存占用和提高运行速度。

1. 启动用户界面：最后，脚本启动了用户界面，用户可以在浏览器中与聊天机器人进行交互。

总的来说，这个脚本的主要功能是加载一个预训练的聊天机器人模型，并提供一个交互式用户界面，用户可以在其中与机器人进行对话，并实时看到机器人的回复。

### 分析 web_demo.sh

```bash
PRE_SEQ_LEN=128

CUDA_VISIBLE_DEVICES=0 python3 web_demo.py \

    --model_name_or_path /app/chatglm2-6b \

    --ptuning_checkpoint output/adgen-chatglm2-6b-pt-128-2e-2/checkpoint-3000 \

    --pre_seq_len $PRE_SEQ_LEN
```

这个命令是用于启动一个Web演示的Python脚本，具体解释如下：

- `PRE_SEQ_LEN=128`：这是一个环境变量，表示预先设定的序列长度为128。
- `CUDA_VISIBLE_DEVICES=0`：这是一个环境变量，用于指定哪些GPU设备可见，即被CUDA使用。在这里，它被设置为0，表示只有第一个GPU（索引为0）可见。
- `python3 web_demo.py`：这是用Python 3运行名为`web_demo.py`的脚本。
- `--model_name_or_path /app/chatglm2-6b`：这是一个命令行参数，指定模型的名称或路径为`/app/chatglm2-6b`。
- `--ptuning_checkpoint output/adgen-chatglm2-6b-pt-128-2e-2/checkpoint-3000`：这是一个命令行参数，指定了微调模型的检查点路径。
- `--pre_seq_len $PRE_SEQ_LEN`：这是一个命令行参数，指定了预设序列长度。这里使用了前面定义的环境变量`$PRE_SEQ_LEN`。

这个命令主要是为了运行一个Web演示，该演示使用了一个微调过的模型，并设定了一些模型的参数。

### 执行模型部署

```bash
bash web_demo.sh
```

```text
root@750840fb9d50:/app/chatglm2-6b/ChatGLM2-6B/ptuning# bash web_demo.sh

/app/chatglm2-6b/ChatGLM2-6B/ptuning/web_demo.py:101: GradioDeprecationWarning: The `style` method is deprecated. Please set these arguments in the constructor instead.

  user_input = gr.Textbox(show_label=False, placeholder="Input...", lines=10).style(

Loading prefix_encoder weight from output/adgen-chatglm2-6b-pt-128-2e-2/checkpoint-3000

Loading checkpoint shards: 100%|█████████████████| 7/7 [00:22<00:00,  3.19s/it]

Some weights of ChatGLMForConditionalGeneration were not initialized from the model checkpoint at /app/chatglm2-6b and are newly initialized: ['transformer.prefix_encoder.embedding.weight']

You should probably TRAIN this model on a down-stream task to be able to use it for predictions and inference.

Running on local URL:  http://0.0.0.0:7860

Running on public URL: https://697fcf76e5fc419f99.gradio.live

This share link expires in 72 hours. For free permanent hosting and GPU upgrades, run `gradio deploy` from Terminal to deploy to Spaces (https://huggingface.co/spaces)
```

之所以有一个public URL: `https://697fcf76e5fc419f99.gradio.live` 是因为我们修改了`web_demo.py`中的`launch()`

```python
demo.queue().launch(server_name="0.0.0.0", share=True, inbrowser=True)
```

微调数据集是服装广告语，所以我们可以问一些穿搭问题

![image-20230908105150954](ChatGLM-2 实验手册.assets/image-20230908105150954.png)

# 图形化微调

https://github.com/hiyouga/LLaMA-Efficient-Tuning/tree/main

https://github.com/hiyouga/ChatGLM-Efficient-Tuning

针对以下高效微调方法的支持：

- `LoRA`: 仅微调低秩适应器。
- `P-Tuning V2`: 仅微调前缀编码器。
- `Freeze Tuning`: 仅微调后几层的全连接层。
- 全量微调: 微调模型所有参数。

注意：目前网页 UI 仅支持单卡训练

参数信息请参照：https://github.com/hiyouga/ChatGLM-Efficient-Tuning/wiki/%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95

## 环境准备

使用`chatglm2-6b` 作为测试环境

```bash
docker run --gpus all --ipc=host --ulimit memlock=-1 --ulimit stack=67108864 -v $(pwd)/chatglm2-6b:/app/chatglm2-6b --restart=always -d -p 7860:7860 -p 8000:8000 -p 8501:8501 --name=chatglm2 chatglm2:base python /app/chatglm2-6b/ChatGLM2-6B/openai_api.py
```

进入容器上下文

```bash
docker exec -it chatglm2 bash
```

在容器命令行上下文安装`git-lfs`

```bash
cd chatglm2-6b/

apt update

apt-get install git-lfs
```

下载项目

```bash
git lfs install

git clone https://github.com/hiyouga/ChatGLM-Efficient-Tuning.git
```

初始化环境

```bash
conda create -n chatglm_etuning python=3.10

conda activate chatglm_etuning
```

这两行命令是用于创建和激活一个新的`conda`环境的。`conda`是一个开源的包管理系统和环境管理系统，用于安装多个版本的软件包及其依赖关系，并能在他们之间轻松切换。

1. `conda create -n chatglm_etuning python=3.10`：这是创建一个新的`conda`环境的命令。`-n`参数后面的`chatglm_etuning`是你为新环境选择的名称。`python=3.10`指定了你想在这个环境中安装的Python版本，即3.10。

1. `conda activate chatglm_etuning`：这是激活你刚刚创建的`conda`环境的命令。执行这个命令后，你的终端或命令行界面将切换到`chatglm_etuning`环境，你在这个环境中运行的所有命令和程序都将使用这个环境中的软件包版本。

这两条命令通常用于确保你的项目在一个独立的环境中运行，避免不同项目之间由于依赖不同版本的同一软件包而导致的冲突。

安装依赖

```bash
cd ChatGLM-Efficient-Tuning

pip install -r requirements.txt
```

这个`requirements.txt`文件包含了一系列的Python包及其版本号，这些包都是为了运行某个特定的Python项目而需要的。下面是对每个包的简单解释：

1. `torch>=1.13.1`：PyTorch是一个开源的深度学习框架，用于构建和训练神经网络。这个要求指定了项目需要PyTorch版本1.13.1或更高。

1. `transformers>=4.29.1`：Transformers库由Hugging Face开发，提供了大量预训练的NLP模型，以及相应的训练和使用工具。项目需要版本4.29.1或更高。

1. `datasets>=2.12.0`：Datasets库也是由Hugging Face开发，提供了用于NLP任务的大量数据集和数据处理工具。项目需要版本2.12.0或更高。

1. `accelerate>=0.21.0`：Accelerate库由Hugging Face开发，用于简化在多种硬件配置上进行深度学习训练的过程。项目需要版本0.21.0或更高。

1. `peft>=0.4.0`：这个库可能是一个错误，因为在Python的公开库中并没有找到名为"peft"的包。可能是某个特定的私有库，或者是一个拼写错误。

1. `trl>=0.4.7`：在Python的公开库中并没有找到名为"trl"的包。可能是某个特定的私有库，或者是一个拼写错误。

1. `sentencepiece`：SentencePiece是一个开源的文本分词工具，用于支持包括subword和character-level的分词。

1. `jieba`：Jieba是一个用于中文分词的Python库。

1. `rouge-chinese`：Rouge-chinese是一个用于计算中文文本的ROUGE评分的库。

1. `nltk`：NLTK（Natural Language Toolkit）是一个广泛使用的自然语言处理（NLP）库。

1. `gradio>=3.36.0`：Gradio是一个用于快速创建机器学习模型的可视化界面的库。项目需要版本3.36.0或更高。

1. `uvicorn`：Uvicorn是一个基于ASGI规范的Python服务器，通常用于运行FastAPI或Starlette这样的应用。

1. `pydantic==1.10.11`：Pydantic是一个数据验证库，用于处理和验证Python数据类型，通常与FastAPI一起使用。

1. `fastapi==0.95.1`：FastAPI是一个用于构建API的现代、快速（高性能）的web框架。

1. `sse-starlette`：SSE-Starlette是一个用于在Starlette或FastAPI应用中实现Server-Sent Events（SSE）的库。

1. `matplotlib`：Matplotlib是一个用于创建静态、动态或交互式的2D图表的Python库。

1. `protobuf`：Protocol Buffers（protobuf）是Google开发的一种数据序列化协议。

1. `cpm-kernels`：在Python的公开库中并没有找到名为"cpm-kernels"的包。可能是某个特定的私有库，或者是一个拼写错误。

这个`requirements.txt`文件包含了一系列的Python包及其版本号，这些包都是为了运行某个特定的Python项目而需要的。通常，可以通过运行`pip install -r requirements.txt`命令来安装这些包。

### 分析 src/train_web.py

```python
from glmtuner.webui.interface import create_ui

def main():

    demo = create_ui()

    demo.queue()

    demo.launch(server_name="0.0.0.0", server_port=7860, share=True, inbrowser=True)

if __name__ == "__main__":

    main()
```



# ChatGLM-Efficient-Tuning

## 启动 Web UI

> 这是一个Python脚本，它的主要目的是创建一个web UI（用户界面），并在本地服务器上运行这个UI。

1. `from glmtuner.webui.interface import create_ui`：这行代码从`glmtuner.webui.interface`模块导入了一个名为`create_ui`的函数。`glmtuner`可能是一个特定于你的项目的模块，用于创建和配置web UI。

1. `def main():`：定义了一个名为`main`的函数，这个函数的主要工作是创建和运行web UI。

1. `demo = create_ui()`：调用了前面导入的`create_ui`函数，并将返回的UI对象赋值给变量`demo`。

1. `demo.queue()`：调用`demo`对象的`queue`方法。这个方法的具体作用可能会根据`create_ui`函数的实现而变化，但通常这种方法会用于初始化或配置UI对象。

1. `demo.launch(server_name="0.0.0.0", server_port=7860, share=True, inbrowser=True)`：调用`demo`对象的`launch`方法，启动一个web服务器来运行UI。服务器的地址被设置为`"0.0.0.0"`，这意味着服务器将接受来自任何IP地址的连接。服务器的端口被设置为`7860`。`share=True`可能意味着这个UI可以被多个用户共享，`inbrowser=True`可能意味着这个UI将在用户的浏览器中打开。

1. `if __name__ == "__main__":`：这是一个常见的Python模式，用于检查当前脚本是否作为主程序运行。如果是，那么`__name__`变量的值将为`"main"`，并且`main`函数将被调用。如果当前脚本是被其他脚本导入的，那么`__name__`变量的值将为该脚本的名字，`main`函数将不会被调用。这样可以防止在导入脚本时运行不必要的代码。

> 这个脚本的具体工作将取决于`glmtuner.webui.interface.create_ui`函数的实现，但从这个脚本的结构来看，它的主要目的应该是创建和运行一个web UI。

### 启动图形页面

```
CUDA_VISIBLE_DEVICES=0 python src/train_web.py
```

输出效果

```
root@ebe1e0a7a821:/app/chatglm2-6b/ChatGLM-Efficient-Tuning# python src/train_web.py

Running on local URL:  http://0.0.0.0:7860

Running on public URL: https://d8cf48ab22d48c13c5.gradio.live

This share link expires in 72 hours. For free permanent hosting and GPU upgrades, run `gradio deploy` from Terminal to deploy to Spaces (https://huggingface.co/spaces)
```

## 训练

调整模型名称：`/app/chatglm2-6b`

设置微调方法：`p_tuning`

设置数据路径：默认路径为`data`，自定义数据路径可以参照`/app/chatglm2-6b/ChatGLM-Efficient-Tuning/data`

选择数据集：`alpaca_zh`

![image-20230908105751803](ChatGLM-2 实验手册.assets/image-20230908105751803.png)

设置参数

![image-20230908105820808](ChatGLM-2 实验手册.assets/image-20230908105820808.png)

运行了一个半小时，成功结束

![image-20230908105854799](ChatGLM-2 实验手册.assets/image-20230908105854799.png)

checkpoint路径：`/app/chatglm2-6b/ChatGLM-Efficient-Tuning/saves/ChatGLM2-6B/p_tuning/2023-08-05-04-14-29`

## 推理

Checkpoints 从下来菜单中进行选择

![image-20230908105924525](ChatGLM-2 实验手册.assets/image-20230908105924525.png)

注意：为了减少实验时间，此处`Max samples`设置为`500`

大概7分钟运行结束

![image-20230908105959791](ChatGLM-2 实验手册.assets/image-20230908105959791.png)

> 这个结果显示了你的模型在一些关键性能指标上的表现：

1. **Accuracy（精度）**：这个模型的精度是0.020，这意味着只有约2%的预测是完全正确的。这个值相当低，可能意味着模型在预测任务上的表现不佳。

1. **BLEU-4**：这是一个常用于机器翻译任务的指标，值为14.62。BLEU分数的范围是0-100，越高越好。这个分数表示模型的表现中等，但还有改进的空间。

1. ROUGE-1, ROUGE-2, ROUGE-L：这些是用于评估自动文本摘要任务的指标，它们分别考察了生成的摘要和参考摘要之间的单词级别（ROUGE-1）、二元词组级别（ROUGE-2）和最长公共子序列（ROUGE-L）的一致性。这些分数分别为34.27、13.93和26.74，这表明模型在不同的一致性级别上的表现都有待提高。

1. **Runtime（运行时间）**：模型的预测运行时间为387.72秒。

1. Samples per second（每秒处理的样本数）：模型每秒处理约1.287个样本，这是一个关于模型速度的指标。

1. Steps per second（每秒处理的步骤数）：模型每秒执行约0.162个步骤，这也是一个关于模型速度的指标。

> 总的来说，这个模型的性能可能需要改进。精度较低，可能需要通过调整模型的参数、使用更复杂的模型或者使用更大的训练数据集来提升。同时，BLEU和ROUGE分数也指出了模型在一致性方面的问题，可能需要进一步优化模型以提高这些分数。在速度方面，你可能需要考虑是否可以接受当前的运行时间和处理速度，或者是否需要进一步优化以提高速度。

测试结果保存在`/app/chatglm2-6b/ChatGLM-Efficient-Tuning/saves/ChatGLM2-6B/p_tuning/eval_2023-08-05-04-14-29`

## 测试

![image-20230908110101785](ChatGLM-2 实验手册.assets/image-20230908110101785.png)

![image-20230908110127782](ChatGLM-2 实验手册.assets/image-20230908110127782.png)

## 导出模型

设置Export dir 为 `/app/chatglm2-6b/ChatGLM-Efficient-Tuning/saves/ChatGLM2-6B/p_tuning/chatglm2-6b-2`

![image-20230908110218631](ChatGLM-2 实验手册.assets/image-20230908110218631.png)

```
root@5e376f019017:/app/chatglm2-6b/ChatGLM-Efficient-Tuning/saves/ChatGLM2-6B/p_tuning/chatglm2-6b-2# ls -l

total 12197472

-rwxrwxrwx 1 1000 1000       1341 Aug  5 08:35 config.json

-rwxrwxrwx 1 1000 1000       2246 Aug  5 08:35 configuration_chatglm.py

-rwxrwxrwx 1 1000 1000        111 Aug  5 08:35 generation_config.json

-rwxrwxrwx 1 1000 1000      50717 Aug  5 08:35 modeling_chatglm.py

-rwxrwxrwx 1 1000 1000 9986264855 Aug  5 08:36 pytorch_model-00001-of-00002.bin

-rwxrwxrwx 1 1000 1000 2502811253 Aug  5 08:36 pytorch_model-00002-of-00002.bin

-rwxrwxrwx 1 1000 1000      20583 Aug  5 08:35 pytorch_model.bin.index.json

-rwxrwxrwx 1 1000 1000      14692 Aug  5 08:35 quantization.py

-rwxrwxrwx 1 1000 1000          3 Aug  5 08:36 special_tokens_map.json

-rwxrwxrwx 1 1000 1000      10061 Aug  5 08:36 tokenization_chatglm.py

-rwxrwxrwx 1 1000 1000    1018370 Aug  5 08:36 tokenizer.model

-rwxrwxrwx 1 1000 1000        325 Aug  5 08:36 tokenizer_config.json
```

# Langchain-Chatchat

https://github.com/chatchat-space/Langchain-Chatchat/

> 一种利用 [langchain](https://github.com/hwchase17/langchain) 思想实现的基于本地知识库的问答应用，目标期望建立一套对中文场景与开源模型支持友好、可离线运行的知识库问答解决方案。

## 下载项目和模型

下载项目库：

```
git clone https://github.com/chatchat-space/Langchain-Chatchat/
```

下载 `Embedding` 模型 `moka-ai/m3e-base`

```
git lfs install

git clone https://huggingface.co/moka-ai/m3e-base
```

下载LLM模型`ChatGLM2-6B`

```
git lfs install

git clone https://huggingface.co/THUDM/chatglm2-6b
```

## 运行容器进行环境初始化

运行容器

```
docker run --gpus all --ipc=host --ulimit memlock=-1 --ulimit stack=67108864 -it -v `pwd`/chatglm2-6b:/app/chatglm2-6b -v `pwd`/m3e-base:/app/m3e-base -v `pwd`/Langchain-Chatchat:/app/Langchain-Chatchat -p 7861:7861 -p 8888:8888 -p 8501:8501 --name=chatchat chatglm2:base
```

输出效果如下

```
chengzh@AIStudio:/mnt/c/BKF$ docker run --gpus all --ipc=host --ulimit memlock=-1 --ulimit stack=67108864 --rm -it -v `pwd`/chatglm2-6b:/app/chatglm2-6b -v `pwd`/m3e-base:/app/m3e-base -v `pwd`/Langchain-Chatchat:/app/Langchain-Chatchat -p 7860:7860 -p 8000:8000 -p 8501:8501 chatglm2:base

=============

== PyTorch ==

=============

NVIDIA Release 23.06 (build 63009835)

PyTorch Version 2.1.0a0+4136153

Container image Copyright (c) 2023, NVIDIA CORPORATION &amp; AFFILIATES. All rights reserved.

Copyright (c) 2014-2023 Facebook Inc.

Copyright (c) 2011-2014 Idiap Research Institute (Ronan Collobert)

Copyright (c) 2012-2014 Deepmind Technologies    (Koray Kavukcuoglu)

Copyright (c) 2011-2012 NEC Laboratories America (Koray Kavukcuoglu)

Copyright (c) 2011-2013 NYU                      (Clement Farabet)

Copyright (c) 2006-2010 NEC Laboratories America (Ronan Collobert, Leon Bottou, Iain Melvin, Jason Weston)

Copyright (c) 2006      Idiap Research Institute (Samy Bengio)

Copyright (c) 2001-2004 Idiap Research Institute (Ronan Collobert, Samy Bengio, Johnny Mariethoz)

Copyright (c) 2015      Google Inc.

Copyright (c) 2015      Yangqing Jia

Copyright (c) 2013-2016 The Caffe contributors

All rights reserved.

Various files include modifications (c) NVIDIA CORPORATION &amp; AFFILIATES.  All rights reserved.

This container image and its contents are governed by the NVIDIA Deep Learning Container License.

By pulling and using the container, you accept the terms and conditions of this license:

https://developer.nvidia.com/ngc/nvidia-deep-learning-container-license

root@04f8f5afb7c2:/app# dir

Langchain-Chatchat  chatglm2-6b  m3e-base

root@04f8f5afb7c2:/app#
```

> 注意已经到了容器上下文里

创建和使用Python虚拟环境

```
cd Langchain-Chatchat

pip install virtualenv

virtualenv env

source env/bin/activate
```

安装依赖

```
pip install -r requirements.txt
```



# 环境准备与配置

此处可能会耗时20分钟，请保持耐心。

## 依赖库说明

> 这是一个`requirements.txt`文件，它列出了项目需要的所有Python库及其版本。当你运行`pip install -r requirements.txt`命令时，pip会安装文件中列出的所有库。

> 以下是这个文件中列出的库的一些可能的用途：

- `langchain==0.0.266`: 这可能是一个特定的Python库，用于处理与语言链相关的任务。不过我没有找到这个库的公开信息，可能是一个私有或者自定义的库。

- `openai`: 这个库是OpenAI的官方Python客户端，用于与OpenAI的API进行交互。

- `zhipuai`: 我没有找到这个库的公开信息，可能是一个私有或者自定义的库。

- `sentence_transformers`: 这个库用于生成句子的向量表示，通常用于NLP任务。

- `fschat==0.2.24`: 我没有找到这个库的公开信息，可能是一个私有或者自定义的库。

- `transformers>=4.31.0`: 这个库是Hugging Face的Transformers库，提供了大量预训练的NLP模型。

- `torch~=2.0.0`: 这是PyTorch库，一个用于机器学习和深度学习的开源库。

- `fastapi~=0.99.1`, `uvicorn~=0.23.1`, `starlette~=0.27.0`, `pydantic~=1.10.11`: 这些库用于创建和运行FastAPI web应用。

- `nltk~=3.8.1`: 这是Natural Language Toolkit，一个用于符号和统计自然语言处理的库。

- `unstructured[all-docs]>=0.10.4`: 这可能是用于处理非结构化数据的库，但我没有找到这个库的公开信息。

- `python-magic-bin; sys_platform == 'win32'`: 这个库用于在Windows平台上处理文件类型识别。

- `SQLAlchemy==2.0.19`: 这是一个Python的SQL工具包和ORM（Object Relational Mapper）。

- `faiss-cpu`: 这是Facebook的FAISS库，用于高效的相似度搜索和聚类大量向量。

- `accelerate`: 这是Hugging Face的库，用于简化在多个设备上运行PyTorch代码。

- `spacy`: 这是一个用于NLP任务的库。

- `PyMuPDF==1.22.5`: 这个库用于处理PDF和其他类型的文档。

- `rapidocr_onnxruntime>=1.3.2`: 这可能是一个用于OCR（Optical Character Recognition）的库，但我没有找到这个库的公开信息。

- `numpy~=1.24.4`, `pandas~=2.0.3`: 这些是用于数据处理的库。

- `streamlit>=1.26.0`, `streamlit-option-menu>=0.3.6`, `streamlit-antd-components>=0.1.11`, `streamlit-chatbox>=1.1.6`, `streamlit-aggrid>=0.3.4.post3`: 这些库用于创建和运行Streamlit web应用。

- `httpx~=0.24.1`: 这是一个用于发送HTTP请求的库。

- `watchdog`: 这个库用于监控文件系统的变化。

> 这个`requirements.txt`文件中的其他库可能是用于特定的数据处理或机器学习任务，或者是用于与特定服务进行交互。

## 修改配置文件

### 新建配置文件

```
cp configs/model_config.py.example ./configs/model_config.py

cp configs/server_config.py.example ./configs/server_config.py
```

### 修改 `model_config.py`

```
nano /configs/model_config.py
```

把23行中的模型路径改为`/app/m3e-base`

把48行中的模型路径修改为`/app/chatglm2-6b`

```
import os

import logging

# 日志格式

LOG_FORMAT = "%(asctime)s - %(filename)s[line:%(lineno)d] - %(levelname)s: %(message)s"

logger = logging.getLogger()

logger.setLevel(logging.INFO)

logging.basicConfig(format=LOG_FORMAT)

# 在以下字典中修改属性值，以指定本地embedding模型存储位置

# 如将 "text2vec": "GanymedeNil/text2vec-large-chinese" 修改为 "text2vec": "User/Downloads/text2vec-large-chinese"

# 此处请写绝对路径

embedding_model_dict = {

    "ernie-tiny": "nghuyong/ernie-3.0-nano-zh",

    "ernie-base": "nghuyong/ernie-3.0-base-zh",

    "text2vec-base": "shibing624/text2vec-base-chinese",

    "text2vec": "GanymedeNil/text2vec-large-chinese",

    "text2vec-paraphrase": "shibing624/text2vec-base-chinese-paraphrase",

    "text2vec-sentence": "shibing624/text2vec-base-chinese-sentence",

    "text2vec-multilingual": "shibing624/text2vec-base-multilingual",

    "text2vec-bge-large-chinese": "shibing624/text2vec-bge-large-chinese",

    "m3e-small": "moka-ai/m3e-small",

    "m3e-base": "/app/m3e-base",

    "m3e-large": "moka-ai/m3e-large",

    "bge-small-zh": "BAAI/bge-small-zh",

    "bge-base-zh": "BAAI/bge-base-zh",

    "bge-large-zh": "BAAI/bge-large-zh",

    "bge-large-zh-noinstruct": "BAAI/bge-large-zh-noinstruct",

    "piccolo-base-zh": "sensenova/piccolo-base-zh",

    "piccolo-large-zh": "sensenova/piccolo-large-zh",

    "text-embedding-ada-002": os.environ.get("OPENAI_API_KEY")

}

# 选用的 Embedding 名称

EMBEDDING_MODEL = "m3e-base"

# Embedding 模型运行设备。设为"auto"会自动检测，也可手动设定为"cuda","mps","cpu"其中之一。

EMBEDDING_DEVICE = "auto"

llm_model_dict = {

    "chatglm-6b": {

        "local_model_path": "THUDM/chatglm-6b",

        "api_base_url": "http://localhost:8888/v1",  # "name"修改为fastchat服务中的"api_base_url"

        "api_key": "EMPTY"

    },

    "chatglm2-6b": {

        "local_model_path": "/app/chatglm2-6b",

        "api_base_url": "http://localhost:8888/v1",  # URL需要与运行fastchat服务端的server_config.FSCHAT_OPENAI_API一致

        "api_key": "EMPTY"

    },

    "chatglm2-6b-32k": {

        "local_model_path": "THUDM/chatglm2-6b-32k",  # "THUDM/chatglm2-6b-32k",

        "api_base_url": "http://localhost:8888/v1",  # "URL需要与运行fastchat服务端的server_config.FSCHAT_OPENAI_API一致

        "api_key": "EMPTY"

    },

    # 调用chatgpt时如果报出： urllib3.exceptions.MaxRetryError: HTTPSConnectionPool(host='api.openai.com', port=443):

    #  Max retries exceeded with url: /v1/chat/completions

    # 则需要将urllib3版本修改为1.25.11

    # 如果依然报urllib3.exceptions.MaxRetryError: HTTPSConnectionPool，则将https改为http

    # 参考https://zhuanlan.zhihu.com/p/350015032

    # 如果报出：raise NewConnectionError(

    # urllib3.exceptions.NewConnectionError: <urllib3.connection.HTTPSConnection object at 0x000001FE4BDB85E0>:

    # Failed to establish a new connection: [WinError 10060]

    # 则是因为内地和香港的IP都被OPENAI封了，需要切换为日本、新加坡等地

    # 如果出现WARNING: Retrying langchain.chat_models.openai.acompletion_with_retry.<locals>._completion_with_retry in

    # 4.0 seconds as it raised APIConnectionError: Error communicating with OpenAI.

    # 需要添加代理访问(正常开的代理软件可能会拦截不上)需要设置配置openai_proxy 或者 使用环境遍历OPENAI_PROXY 进行设置

    # 比如: "openai_proxy": 'http://127.0.0.1:4780'

    "gpt-3.5-turbo": {

        "api_base_url": "https://api.openai.com/v1",

        "api_key": os.environ.get("OPENAI_API_KEY"),

        "openai_proxy": os.environ.get("OPENAI_PROXY")

    },

    # 线上模型。当前支持智谱AI。

    # 如果没有设置有效的local_model_path，则认为是在线模型API。

    # 请在server_config中为每个在线API设置不同的端口

    # 具体注册及api key获取请前往 http://open.bigmodel.cn

    "chatglm-api": {

        "api_base_url": "http://127.0.0.1:8888/v1",

        "api_key": os.environ.get("ZHIPUAI_API_KEY"),

        "provider": "ChatGLMWorker",

        "version": "chatglm_pro",  # 可选包括 "chatglm_lite", "chatglm_std", "chatglm_pro"

    },

}

# LLM 名称

LLM_MODEL = "chatglm2-6b"

# 历史对话轮数

HISTORY_LEN = 3

# LLM 运行设备。设为"auto"会自动检测，也可手动设定为"cuda","mps","cpu"其中之一。

LLM_DEVICE = "auto"

# 日志存储路径

LOG_PATH = os.path.join(os.path.dirname(os.path.dirname(__file__)), "logs")

if not os.path.exists(LOG_PATH):

    os.mkdir(LOG_PATH)

# 知识库默认存储路径

KB_ROOT_PATH = os.path.join(os.path.dirname(os.path.dirname(__file__)), "knowledge_base")

# 数据库默认存储路径。

# 如果使用sqlite，可以直接修改DB_ROOT_PATH；如果使用其它数据库，请直接修改SQLALCHEMY_DATABASE_URI。

DB_ROOT_PATH = os.path.join(KB_ROOT_PATH, "info.db")

SQLALCHEMY_DATABASE_URI = f"sqlite:///{DB_ROOT_PATH}"

# 可选向量库类型及对应配置

kbs_config = {

    "faiss": {

    },

    "milvus": {

        "host": "127.0.0.1",

        "port": "19530",

        "user": "",

        "password": "",

        "secure": False,

    },

    "pg": {

        "connection_uri": "postgresql://postgres:postgres@127.0.0.1:5432/langchain_chatchat",

    }

}

# 默认向量库类型。可选：faiss, milvus, pg.

DEFAULT_VS_TYPE = "faiss"

# 缓存向量库数量

CACHED_VS_NUM = 1

# 知识库中单段文本长度

CHUNK_SIZE = 250

# 知识库中相邻文本重合长度

OVERLAP_SIZE = 50

# 知识库匹配向量数量

VECTOR_SEARCH_TOP_K = 5

# 知识库匹配相关度阈值，取值范围在0-1之间，SCORE越小，相关度越高，取到1相当于不筛选，建议设置在0.5左右
```



# 项目配置与知识库初始化

`SCORE_THRESHOLD = 1`

## 搜索引擎匹配结题数量

`SEARCH_ENGINE_TOP_K = 5`

## nltk 模型存储路径

`NLTK_DATA_PATH = os.path.join(os.path.dirname(os.path.dirname(__file__)), "nltk_data")`

## 基于本地知识问答的提示词模版

使用 Jinja2 语法，简单点就是用双大括号代替 `f-string` 的单大括号。

```
PROMPT_TEMPLATE = """<指令>根据已知信息，简洁和专业的来回答问题。如果无法从中得到答案，请说 “根据已知信息无法回答该问题”，不允许在答案中添加编造成分，答案请使用中文。 </指令>

<已知信息>{{ context }}</已知信息>

<问题>{{ question }}</问题>"""
```

## API 是否开启跨域

默认为 `False`，如果需要开启，请设置为 `True`。

`OPEN_CROSS_DOMAIN = False`

## Bing 搜索必备变量

使用 Bing 搜索需要使用 Bing Subscription Key，需要在 Azure portal 中申请试用 Bing Search。

具体申请方式请见：`https://learn.microsoft.com/en-us/bing/search-apis/bing-web-search/create-bing-search-service-resource`

使用 Python 创建 Bing API 搜索实例详见：`https://learn.microsoft.com/en-us/bing/search-apis/bing-web-search/quickstarts/rest/python`

`BING_SEARCH_URL = "https://api.bing.microsoft.com/v7.0/search"`

注意不是 Bing Webmaster Tools 的 API Key。

此外，如果是在服务器上，报 `Failed to establish a new connection: [Errno 110] Connection timed out` 是因为服务器加了防火墙，需要联系管理员加白名单，如果公司的服务器的话，就别想了 GG。

`BING_SUBSCRIPTION_KEY = ""`

## 是否开启中文标题加强

通过增加标题判断，判断哪些文本为标题，并在 `metadata` 中进行标记；然后将文本与往上一级的标题进行拼合，实现文本信息的增强。

`ZH_TITLE_ENHANCE = False`

## 修改 server_config.py

把第 12 行的 `DEFAULT_BIND_HOST` 改成 `0.0.0.0`。

```
from .model_config import LLM_MODEL, llm_model_dict, LLM_DEVICE

import httpx

# httpx 请求默认超时时间（秒）。如果加载模型或对话较慢，出现超时错误，可以适当加大该值。
HTTPX_DEFAULT_TIMEOUT = 300.0

# API 是否开启跨域，默认为 False，如果需要开启，请设置为 True

# is open cross domain
OPEN_CROSS_DOMAIN = False

# 各服务器默认绑定 host
DEFAULT_BIND_HOST = "0.0.0.0" # 此处改为 0.0.0.0

# webui.py server
WEBUI_SERVER = {
    "host": DEFAULT_BIND_HOST,
    "port": 8501,
}

# api.py server
API_SERVER = {
    "host": DEFAULT_BIND_HOST,
    "port": 7861,
}

# fastchat openai_api server
FSCHAT_OPENAI_API = {
    "host": DEFAULT_BIND_HOST,
    "port": 8888,  # model_config.llm_model_dict 中模型配置的 api_base_url 需要与这里一致。
}

# fastchat model_worker server

# 这些模型必须是在 model_config.llm_model_dict 中正确配置的。

# 在启动 startup.py 时，可用通过 `--model-worker --model-name xxxx` 指定模型，不指定则为 `LLM_MODEL`
FSCHAT_MODEL_WORKERS = {
    # 所有模型共用的默认配置，可在模型专项配置或 llm_model_dict 中进行覆盖。
    "default": {
        "host": DEFAULT_BIND_HOST,
        "port": 20002,
        "device": LLM_DEVICE,
        # 多卡加载需要配置的参数
        # "gpus": None, # 使用的 GPU，以 str 的格式指定，如 "0,1"
        # "num_gpus": 1, # 使用 GPU 的数量
        # "max_gpu_memory": "20GiB", # 每个 GPU 占用的最大显存
        # 以下为非常用参数，可根据需要配置
        # "load_8bit": False, # 开启 8bit 量化
        # "cpu_offloading": None,
        # "gptq_ckpt": None,
        # "gptq_wbits": 16,
        # "gptq_groupsize": -1,
        # "gptq_act_order": False,
        # "awq_ckpt": None,
        # "awq_wbits": 16,
        # "awq_groupsize": -1,
        # "model_names": [LLM_MODEL],
        # "conv_template": None,
        # "limit_worker_concurrency": 5,
        # "stream_interval": 2,
        # "no_register": False,
    },
    "baichuan-7b": { # 使用 default 中的 IP 和端口
        "device": "cpu",
    },
    "chatglm-api": { # 请为每个在线 API 设置不同的端口
        "port": 20003,
    },
}

# fastchat multi model worker server
FSCHAT_MULTI_MODEL_WORKERS = {
    # TODO:
}

# fastchat controller server
FSCHAT_CONTROLLER = {
    "host": DEFAULT_BIND_HOST,
    "port": 20001,
    "dispatch_method": "shortest_queue",
}
```

# 知识库初始化与迁移

> 第一次运行本项目，知识库尚未建立，或者配置文件中的知识库类型、嵌入模型发生变化，或者之前的向量库没有开启 `normalize_L2`，需要初始化或重建知识库

## 分析 init_database.py

```
from server.knowledge_base.migrate import create_tables, reset_tables, folder2db, recreate_all_vs, list_kbs_from_folder

from configs.model_config import NLTK_DATA_PATH

import nltk

nltk.data.path = [NLTK_DATA_PATH] + nltk.data.path

from startup import dump_server_info

from datetime import datetime

if __name__ == "__main__":

    import argparse

    parser = argparse.ArgumentParser()

    parser.formatter_class = argparse.RawTextHelpFormatter

    parser.add_argument(

        "--recreate-vs",

        action="store_true",

        help=(

            '''

            recreate all vector store.

            use this option if you have copied document files to the content folder, but vector store has not been populated or DEFAUL_VS_TYPE/EMBEDDING_MODEL changed.

            if your vector store is ready with the configs, just skip this option to fill info to database only.

            '''

        )

    )

    args = parser.parse_args()

    dump_server_info()

    start_time = datetime.now()

    if args.recreate_vs:

        reset_tables()

        print("database talbes reseted")

        print("recreating all vector stores")

        recreate_all_vs()

    else:

        create_tables()

        print("database talbes created")

        print("filling kb infos to database")

        for kb in list_kbs_from_folder():

            folder2db(kb, "fill_info_only")

    end_time = datetime.now()

    print(f"总计用时： {end_time-start_time}")
```

> 这个 Python 脚本似乎是用于处理和管理一个知识库（knowledge base，简称 KB）的。具体来说，这个脚本可能是用于创建、重置和填充一个用于存储和检索知识的数据库。这个知识库可能包含一些文档，这些文档可能已经被转换成了向量形式以便于搜索和检索。

> 让我们看一下这个脚本的主要部分：

1.  导入模块和函数：脚本开始时，它导入了一些需要使用的模块和函数。这包括一些用于处理知识库的函数，如 `create_tables`、`reset_tables`、`folder2db`、`recreate_all_vs` 和 `list_kbs_from_folder`。它还导入了 `NLTK_DATA_PATH`，这可能是用于处理自然语言的库 `NLTK` 的数据路径。

2.  设置 `NLTK` 数据路径：脚本设置了 `NLTK` 的数据路径为 `NLTK_DATA_PATH`，这可能是因为脚本需要使用 `NLTK` 来处理文本数据。

3.  解析命令行参数：脚本接下来定义了一个命令行参数解析器，这个解析器可以接受一个参数 `--recreate-vs`。如果这个参数被设置，那么脚本将会重置数据库表并重新创建所有的向量存储。否则，它将创建数据库表并将知识库信息填充到数据库中。

4.  执行主逻辑：在脚本的主体部分，它首先打印服务器信息，然后记录开始时间。接着，如果 `--recreate-vs` 参数被设置，它将重置数据库表并重新创建所有的向量存储。否则，它将创建数据库表并将知识库信息填充到数据库中。最后，它记录结束时间并打印总计用时。

> 总的来说，这个脚本的主要目的是管理一个知识库，包括创建和重置数据库表，以及填充和更新知识库信息。

## 初始化知识库

```
python init_database.py --recreate-vs
```

## 输出效果如下

```
(env) root@04f8f5afb7c2:/app/Langchain-Chatchat# python init_database.py --recreate-vs

2023-09-08 07:10:55,845 - utils.py[line:148] - INFO: Note: NumExpr detected 24 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 8.

2023-09-08 07:10:55,845 - utils.py[line:160] - INFO: NumExpr defaulting to 8 threads.

2023-09-08 07:11:09,977 - instantiator.py[line:21] - INFO: Created a temporary directory at /tmp/tmp58j0si5t

2023-09-08 07:11:09,977 - instantiator.py[line:76] - INFO: Writing /tmp/tmp58j0si5t/_remote_module_non_scriptable.py

==============================Langchain-Chatchat Configuration==============================

操作系统：Linux-5.10.102.1-microsoft-standard-WSL2-x86_64-with-glibc2.35.

python版本：3.10.6 (main, May 29 2023, 11:10:38) [GCC 11.3.0]

项目版本：v0.2.3

langchain版本：0.0.266. fastchat版本：0.2.24

当前 LLM 模型：chatglm2-6b @ cuda

{'api_base_url': 'http://localhost:8888/v1',
 'api_key': 'EMPTY',
 'local_model_path': '/app/chatglm2-6b'}

当前 Embbedings 模型：m3e-base @ cuda

==============================Langchain-Chatchat Configuration==============================

database talbes reseted

recreating all vector stores

loading vector store in 'samples'.

2023-09-08 07:11:12 | INFO | sentence_transformers.SentenceTransformer | Load pretrained SentenceTransformer: /app/m3e-base

2023-09-08 07:11:15 | INFO | faiss.loader | Loading faiss with AVX2 support.

2023-09-08 07:11:16 | INFO | faiss.loader | Successfully loaded faiss with AVX2 support.

知识库 samples 中共有 0 个文档。

即将清除向量库。

知识库 samples 缓存刷新：1

清理后，知识库 samples 中共有 0 个文档。

UnstructuredFileLoader used for /app/Langchain-Chatchat/knowledge_base/samples/content/test.txt
```



`[E050]` Can't find model `zh_core_web_sm`. It doesn't seem to be a Python package or a valid path to a data directory.

```
page_content='ChatGPT是OpenAI开发的一个大型语言模型，可以提供各种主题的信息，

# 如何向 ChatGPT 提问以获得高质量答案：提示技巧工程完全指南

## 介绍

我很高兴欢迎您阅读我的最新书籍《The Art of Asking ChatGPT for High-Quality Answers: A complete Guide to Prompt Engineering Techniques》。本书是 一本全面指南，介绍了各种提示技术，用于从ChatGPT中生成高质量的答案。' metadata={'source': '/app/Langchain-Chatchat/knowledge_base/samples/content/test.txt'}
```

正在将 `samples/test.txt` 添加到向量库，共包含83条文档

`loading vector store in 'samples'.`

```
Batches: 100%|████████████████████████████████████████████████████████████████| 1/1 [00:03<00:00,  3.01s/it]
Batches: 100%|█████████████████████████████████████████████████████████████████| 1/1 [00:00<00:00, 221.78it/s]
Batches: 100%|█████████████████████████████████████████████████████████████████| 1/1 [00:00<00:00, 211.66it/s]
Batches: 100%|█████████████████████████████████████████████████████████████████| 1/1 [00:00<00:00, 217.68it/s]
Batches: 100%|█████████████████████████████████████████████████████████████████| 1/1 [00:00<00:00, 218.89it/s]
```

知识库 `samples` 缓存刷新：2

总计用时： 0:00:19.982187

# 启动服务

一键加载所有服务

```
python startup.py -a
```

命令行输出效果如下：

```
==============================Langchain-Chatchat Configuration==============================

操作系统：Linux-5.10.102.1-microsoft-standard-WSL2-x86_64-with-glibc2.35.

python版本：3.10.6 (main, May 29 2023, 11:10:38) [GCC 11.3.0]

项目版本：v0.2.3

langchain版本：0.0.266. fastchat版本：0.2.24

当前LLM模型：chatglm2-6b @ cuda

{'api_base_url': 'http://localhost:8888/v1',

 'api_key': 'EMPTY',

 'local_model_path': '/app/chatglm2-6b'}

当前Embbedings模型： m3e-base @ cuda

服务端运行信息：

    OpenAI API Server: http://0.0.0.0:8888/v1

     (请确认llm_model_dict中配置的api_base_url与上面地址一致。)

    Chatchat  API  Server: http://0.0.0.0:7861

    Chatchat WEBUI Server: http://0.0.0.0:8501

==============================Langchain-Chatchat Configuration==============================

Collecting usage statistics. To deactivate, set browser.gatherUsageStats to False.

2023-09-08 07:29:08 | INFO | model_worker | Send heart beat. Models: ['chatglm-api']. Semaphore: None. call_ct: 0. worker_id: 9d84d241.

2023-09-08 07:29:08 | INFO | controller | Receive heart beat. http://0.0.0.0:20003

2023-09-08 07:29:08 | INFO | stdout | INFO:     127.0.0.1:40818 - "POST /receive_heart_beat HTTP/1.1" 200 OK

  You can now view your Streamlit app in your browser.

  URL: http://0.0.0.0:8501
```

查看Chatchat API Server

![image-20230908165134303](ChatGLM-2 实验手册.assets/image-20230908165134303.png)

访问webui界面，进行基本对话

+   对话模式：LLM对话

+   LLM模型：`chatglm2-6b`

![image-20230908162009315](ChatGLM-2 实验手册.assets/image-20230908162009315.png)

对话模式切换为知识库问答

![image-20230908162515600](ChatGLM-2 实验手册.assets/image-20230908162515600.png)

# 持久运行容器环境

在功能测试都通过之后，在项目库根目录下创建一个 `init.sh` 文件

```
nano init.sh
```

```
#!/bin/bash

#!/bin/bash

cd Langchain-Chatchat

pip install virtualenv

virtualenv env

source env/bin/activate

pip install -r requirements.txt

python init_database.py --recreate-vs

python startup.py -a
```

后续即可使用以下命令运行持久运行的环境，

```
docker run -d --restart=always --gpus all --ipc=host --ulimit memlock=-1 --ulimit stack=67108864 -v `pwd`/chatglm2-6b:/app/chatglm2-6b -v `pwd`/m3e-base:/app/m3e-base -v `pwd`/Langchain-Chatchat:/app/Langchain-Chatchat -p 7861:7861 -p 8888:8888 -p 8501:8501 --name=chatchat chatglm2:base /bin/bash -c "Langchain-Chatchat/init.sh"
```

注意：从开始运行容器到webui界面就绪大概需要3~5分钟，请保持耐心。

# 附录

## ChatGLM2-6B 模型官方介绍和声明

ChatGLM2-6B 是开源中英双语对话模型 ChatGLM-6B 的第二代版本，在保留了初代模型对话流畅、部署门槛较低等众多优秀特性的基础之上，ChatGLM2-6B 引入了如下新特性：

1. **更强大的性能**：基于 [ChatGLM](https://github.com/THUDM/GLM) 初代模型的开发经验，我们全面升级了 ChatGLM2-6B 的基座模型。ChatGLM2-6B 使用了 GLM 的混合目标函数，经过了 1.4T 中英标识符的预训练与人类偏好对齐训练，评测结果显示，相比于初代模型，ChatGLM2-6B 在 MMLU（+23%）、CEval（+33%）、GSM8K（+571%）、BBH（+60%）等数据集上的性能取得了大幅度的提升，在同尺寸开源模型中具有较强的竞争力。

1. **更长的上下文**：基于 [FlashAttention](https://github.com/HazyResearch/flash-attention) 技术，我们将基座模型的上下文长度由 ChatGLM-6B 的 2K 扩展到了 32K，并在对话阶段使用 8K 的上下文长度训练。对于更长的上下文，我们发布了 ChatGLM2-6B-32K 模型。LongBench 的测评结果表明，在等量级的开源模型中，ChatGLM2-6B-32K 有着较为明显的竞争优势。

1. **更高效的推理**：基于 Multi-Query Attention 技术，ChatGLM2-6B 有更高效的推理速度和更低的显存占用：在官方的模型实现下，推理速度相比初代提升了 42%，INT4 量化下，6G 显存支持的对话长度由 1K 提升到了 8K。

1. **更开放的协议**：ChatGLM2-6B 权重对学术研究完全开放，在填写问卷进行登记后亦允许免费商业使用。

---

ChatGLM2-6B 开源模型旨在与开源社区一起推动大模型技术发展，恳请开发者和大家遵守开源协议，勿将开源模型和代码及基于开源项目产生的衍生物用于任何可能给国家和社会带来危害的用途以及用于任何未经过安全评估和备案的服务。目前，本项目团队未基于 ChatGLM2-6B 开发任何应用，包括网页端、安卓、苹果 iOS 及 Windows App 等应用。

尽管模型在训练的各个阶段都尽力确保数据的合规性和准确性，但由于 ChatGLM2-6B 模型规模较小，且模型受概率随机性因素影响，无法保证输出内容的准确性，且模型易被误导。本项目不承担开源模型和代码导致的数据安全、舆情风险或发生任何模型被误导、滥用、传播、不当利用而产生的风险和责任。

## LangChain-Chatchat 项目官方介绍

🤖️ 一种利用 [langchain](https://github.com/hwchase17/langchain) 思想实现的基于本地知识库的问答应用，目标期望建立一套对中文场景与开源模型支持友好、可离线运行的知识库问答解决方案。

💡 受 [GanymedeNil](https://github.com/GanymedeNil) 的项目 [document.ai](https://github.com/GanymedeNil/document.ai) 和 [AlexZhangji](https://github.com/AlexZhangji) 创建的 ChatGLM-6B Pull Request 启发，建立了全流程可使用开源模型实现的本地知识库问答应用。本项目的最新版本中通过使用 [FastChat](https://github.com/lm-sys/FastChat) 接入 [Vicuna](https://github.com/tiangolo/fastapi)、[Alpaca](https://github.com/streamlit/streamlit)、LLaMA、Koala、RWKV 等模型，依托于 [langchain](https://github.com/langchain-ai/langchain) 框架支持通过基于 FastAPI 提供的 API 调用服务，或使用基于 Streamlit 的 WebUI 进行操作。

✅ 依托于本项目支持的开源 LLM 与 Embedding 模型，本项目可实现全部使用开源模型离线私有部署。与此同时，本项目也支持 OpenAI GPT API 的调用，并将在后续持续扩充对各类模型及模型 API 的接入。

⛓️ 本项目实现原理如下图所示，过程包括加载文件 -> 读取文本 -> 文本分割 -> 文本向量化 -> 问句向量化 -> 在文本向量中匹配出与问句向量最相似的 top k 个 -> 匹配出的文本作为上下文和问题一起添加到 prompt 中 -> 提交给 LLM 生成回答。

📺 [原理介绍视频](https://www.bilibili.com/video/BV13M4y1e7cN/?share_source=copy_web&vd_source=e6c5aafe684f30fbe41925d61ca6d514)

[!](https://github.com/chatchat-space/Langchain-Chatchat/blob/master/img/langchain+chatglm.png) [实现原理图](ChatGLM-2 实验手册.assets/langchain+chatglm.png)

从文档处理角度来看，实现流程如下：

[!](https://github.com/chatchat-space/Langchain-Chatchat/blob/master/img/langchain+chatglm2.png) [实现原理图2](ChatGLM-2 实验手册.assets/langchain+chatglm2.png)

🚩 本项目未涉及微调、训练过程，但可利用微调或训练对本项目效果进行优化。
