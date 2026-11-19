
# Ubuntu 22.04下本地化部署ChatGLM3-6B模型

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/QdRfdf7gjoewfhxcUnKcwb7onrh](https://kq4b3vgg5b.feishu.cn/docx/QdRfdf7gjoewfhxcUnKcwb7onrh)

## Ch.4 在Ubuntu 22.04系统下部署运行ChatGLM3-6B模型

大模型部署整体来看并不复杂，且官方一般都会提供标准的模型部署流程，但很多人在部署过程中会遇到各种各样的问题，很难成功部署，主要是因为这个过程会涉及非常多依赖库的安装和更新及根据本地的安装情况，需要适时的调整代码逻辑。除此之外也有一定的硬件要求，总的来说还是具有一定的部署和使用门槛。因此本期课程，我们特地详细整理了一份ChatGLM3-6B模型的部署流程教程，供大家参考和学习。

### 操作系统要求

首先看系统要求。目前开源的大模型都支持在Windows、Linux和Mac上部署运行。但在应用开发领域中，Linux系统通常被优先选择而不是Windows，主要原因是Linux系统具有强大的包管理系统（如`apt`、`yum`、`pacman`），允许开发者轻松安装、更新和管理软件包，这比Windows操作系统上的软件安装和管理更加方便快捷。同时Linux系统与多种编程语言和开发工具的兼容性较好，尤其是一些开源工具，仅支持在Linux系统上使用。整体来看，在应用运行方面对硬件的要求较低，且在处理多任务时表现出色，所以被广泛认为是一个非常稳定和可靠的系统，特别是对于服务器和长时间运行的应用。

Linux操作系统有许多不同的发行版，每种发行版都有其特定的特点和用途，如CentOS、Ubuntu和Debian等。CentOS是一种企业级的Linux发行版，以稳定性和安全性著称。它是RHEL（Red Hat Enterprise Linux）的免费替代品，与RHEL完全兼容，适用于服务器和企业环境。而Ubuntu，是最受欢迎的Linux发行版之一，其优势就是对用户友好和很强的易用性，其图形化界面都适合大部分人的习惯。

所以，在实践大模型时，强烈建议大家使用Ubuntu系统。同时，本教程也是针对Ubuntu 22.04桌面版系统来进行ChatGLM3-6B模型的部署和运行的。

### 硬件配置要求

其次，关于硬件的需求，ChatGLM3-6B支持GPU运行（需要英伟达显卡）、CPU运行以及Apple M系列芯片运行。其中GPU运行需要至少6GB以上显存（4Bit精度运行模式下），而CPU运行则需要至少32G的内存。而由于Apple M系列芯片是统一内存架构，因此最少需要13G内存即可运行。其中CPU运行模式下内存占用过大且运行效率较低，因此我们也强调过，GPU模式部署才能有效的进行大模型的学习实践。

在本教程中，我们将重点讲解如何配置GPU环境来部署运行ChatGLM3-6B模型。

基于上述两方面的原因，我们在前两期内容也分别详细地介绍了如何根据大模型的官方配置需求来选择最合适的硬件环境，及如何部署一个纯净的Ubuntu 22.04双系统。本期内容就在这样的环境基础上，安装必要的大模型运行依赖环境，并实际部署、运行及使用ChatGLM3-6B模型。

在开始之前，请大家确定当前使用的硬件环境满足ChatGLM3-6B模型本地化运行的官方最低配置需求：

![](img/bebabb5bcd427371001783220c8cd880.png)

如果配置满足需求，接下来我们就一步一步执行本地化部署ChatGLM3-6B模型。本期内容将首先介绍ChatGLM3-6B模型在Ubuntu 22.04系统下单显卡部署流程，更加专业的Linux多卡部署模式，我们将在下一期课程中进行详细介绍。

本期教程进行演示环境的GPU资源是：NVIDIA RTX 4080，单卡共计16G显存。

## 一、 Ubuntu系统初始化配置

如果跟随上一期视频安装完Ubuntu双系统后，当前的环境是一个比较纯净的系统，首先建议大家做的操作是进行系统的软件更新。这种更新涉及安全补丁、软件更新、之前版本中的错误和问题修复和依赖包的更新，一方面是可以提升系统的安全性，另一方面更重要的也是，更新可以确保所有依赖项都是最新和相互兼容的。虽然不做更新系统仍然可以运行，但我们强烈建议先执行这一操作。

### 1.1 更换国内软件源

Ubuntu的软件源服务器在境外，所以会导致下载速度很慢，甚至有时无法使用，所以建议在进行软件更新前，将软件源更改为国内的镜像网站。

#### Step 1. 备份软件源配置文件

进入`/ect/apt`路径，找到软件源配置文件“`sources.list`”，将其源文件做个备份，以免修改后出现问题可以及时回退。命令如下：

```
cd /ect/apt
```

![](img/88a99f0a2c00f60cc7be817ff8512736.png)

#### Step 2. 安装vim编辑器

Ubuntu默认自带的`vi`是一个非常基础的文本编辑器，而`vim`（Vi IMproved）是`vi`的扩展版本，提供了语法高亮、代码折叠、多级撤销/重做、自动命令、宏记录和播放等高级编辑功能。先执行如下命令进行安装：

```
sudo apt install vim
```

![](img/63a411fef696852b7403e0e5315b31cd.png)

#### Step 3. 使用vim编辑器修改软件源配置文件

Ubuntu的国内镜像源非常多，比较有代表性的有清华源、中科大源、阿里源、网易源，以下是它们的网址：

```
清华源：https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/
```

我们这里使用中科大源。使用Vim编辑器进入后，按"`i`"键插入内容，将如下内容复制进去：

```
deb https://mirrors.ustc.edu.cn/ubuntu/ focal main restricted universe multiverse
```

![](img/a54b2c30fcdd34f17525c0a9f624e118.png)

写入内容后，先按`ESC`，然后输入`:wq!`后保存写入并退出。

### 1.2 系统软件更新

更新完软件源后，我们执行系统的软件更新。

#### Step 1. 打开“终端” -> 输入`sudo apt update`命令，先更新软件包列表

在这里，如果大家看到的URL前缀已经变成了刚才设置的软件源（中科大），就说明上一步更改国内镜像源成功了，否则请返回上一步检查执行的操作哪里出现了纰漏。

![](img/4b015f76e03941af09762d159a2a8521.png)

#### Step 2. 输入`sudo apt upgrade`命令，执行更新所有可更新的软件包

![](img/1d2c74d69c11feb01a987aecde0402ce.png)

### 1.3 设置英文目录路径

上一期视频中在Ubuntu的双系统安装过程中，我们建议大家选择的语言是“English”，主要还是因为英文的路径在使用命令行进行路径切换时不会产生字符编码的问题。而如果有小伙伴选择了中文安装，强烈建议大家要将路径名称更改成英文，如果直接使用的是英文安装的，可以跳过这一步骤。

#### Step 1. 如果大家当前的路径是这样的，说明就是中文的

![](img/c1a192bc547116a9c0a1869ff3d266ed.png)

#### Step 2. 打开终端，快捷键`Ctrl + Alt + T`依次输入如下命令：

```
export LANG=en_US  # 设置当前会话的语言环境变量为英文
```

![](img/a4e3b89209ce9521eba56cccbc46358e.png)

#### Step 3. 跳出对话框询问是否将目录转化为英文路径

![](img/7bae4aed0fb567d1672e08a0ad0539e6.png)

#### Step 4. 如果没有弹出，需要重新生成`user-dirs.locale`文件

`user-dirs.locale`主要作用是存储关于用户目录（如“文档”、“下载”、“音乐”、“图片”等）的本地化（语言和地区）设置，如果这个文件中的语言设置为英语，那么用户目录将使用英文名称（如`Documents`、`Downloads`），如果设置为中文，则这些目录可能会显示为中文名称（如文档、下载）。依次输入如下命令：

```
# 先生成user-dirs.locale文件，
```

![](img/305d2a48c0da0a61dc82a2847523e7ad.png)

#### Step 5. 更改成功后，如下所示

![](img/032e8f6dedbfaae8f25f42fb8c302b93.png)

此时相关目录名称已经变更。（实际上是删除原中文名目录再新建英文名目录，如果中文名称的目录中有文件，则会被保留下来，如“图片”和“Pictures”）

### 1.4 安装Chrome浏览器

安装Chrome浏览器很有必要，对于开发来说，其优势还是在于与Google的其他服务（如Gmail、Google Drive和Google搜索）紧密集成，且扩展程序生态系统丰富，提供了大量的扩展程序。

除此之外，后面我们需要配置VPN、启用ChatGLM3-6B时采用基于Gradio的Web端等操作，都需要用到浏览器。其安装过程相较于Windows操作系统稍有复杂。具体安装过程如下：

#### Step 1. 先找到Ubuntu的默认安装的浏览器

![](img/85472387721b104b3bc4d43d64f06bf4.png)

#### Step 2. 进入谷歌浏览器官网：`https://www.google.com/intl/zh-CN/chrome/`

![](img/9262185d1a1e6ba2e04057aa2b4dcb6b.png)

#### Step 3. 下载Chrome浏览器的“.deb”后缀文件

Ubuntu使用`.deb`包格式的原因与其底层架构和历史有关。Ubuntu是基于Debian操作系统的，而Debian使用`.deb`包格式来管理和分发软件。`.deb`文件中包含了软件程序的文件、脚本以及安装该软件所需的其他信息。这种格式支持复杂的安装场景，包括依赖关系处理、预先和事后脚本执行等。

![](img/0b733ccf9000da87fbdcf0218c2c3812.png)

下载的文件，默认是存放在`/home/Downloads`中的。

#### Step 4. 进入终端，执行安装

Ubuntu使用DPKG包管理系统来安装、删除和管理`.deb`包，提供了一种稳定和灵活的方式来管理系统中的软件。

![](img/34bb654ae4a6ee1312e9a6f42634fb3c.png)

#### Step 5. 验证安装

当安装完成后，可以在左下角的程序管理页面，找到对应的应用图标。

![](img/157caa5871515d8c6a483c6635a41500.png)

### 1.5 配置VPN

在Linux系统上🪜方式有很多，一般使用的软件，都支持在各平台上使用。大家根据个人的使用情况，按照其软件说明进行配置即可，一般都会有比较详细的说明。这里需要配置VPN的原因主要是后面下载Chatglm3-6B的模型权重时需要用到。我个人使用的Pigcha加速器，大家可以参考一下配置过程。

#### Step 1. 进入官网：`https://www.pigcha.com.hk/`

![](img/6c3d36cddb32d5dd633d5106363ac666.png)

#### Step 2. 选择Linux版本的软件进行下载

![](img/7d4b5f986ed63e16b09ddf1d0606503a.png)

#### Step 3. 使用`dpkg -i`的方式安装`.deb`的包

![](img/6d7574c7a3aec926150079e130659130.png)

安装完成，即可找到该加速器的快捷方式，可以直接打开使用。

![](img/91cac9a451d81a824886214f26c23cdd.png)

#### Step 4. 按需输入购买的账户和密码

![](img/50313b434ed94654995533c69d99337f.png)

#### Step 5. 验证网络的连通性

进如果开启加速器后可以访问到Google的资源，说明代理可以正常使用。

![](img/8b6381f755f20aa5ec3966151a524960.png)

## 二、配置大模型运行环境

关于大模型的运行环境，安装显卡驱动显然是首先要做的事情。我们需要确保可以正常的将大模型部署在GPU上，这也是大家比较容易出现问题的环节，比如安装过程中因各种环境问题导致安装不成功，缺依赖包的问题等，总会遇到莫名奇妙的报错导致这第一步就把人的心态搞崩。

### 2.1 安装显卡驱动

显卡驱动是软件，它可以允许操作系统和其他软件与显卡硬件进行交互。对于NVIDIA的GPU，这些驱动是由NVIDIA提供的，安装以后，在该系统上就可以来使用GPU的功能，比如图形渲染，显卡驱动会激活GPU，使其能够处理图形和视频任务。在Ubuntu系统下安装显卡驱动，主要有两种方式：

- 方法一：使用官方的NVIDIA驱动进行手动安装，这种方式比较稳定、靠谱，但可能会遇到很多问题；
- 方法二：使用系统自带的“软件和更新”程序-附加驱动更新，这种方法需要联网，但是非常简单，很难出现问题；（我们推荐大家先使用这种方法）

无论使用哪种方法，前置的操作都是一样的，包括安装依赖包和禁用默认的显卡驱动，具体执行过程如下：

#### Step 1. 安装依赖包

在终端依次执行完如下命令：

```
sudo apt install gcc
sudo apt install g++
sudo apt install make
```

#### Step 2. 禁用Ubuntu默认的显卡驱动

Ubuntu默认安装了开源显卡驱动`Nouveau`，用于NVIDIA显卡。这些驱动通常用来支持基本的桌面图形需求，如2D和一些轻度的3D渲染。但对于我们的高性能显卡，需要安装专有的驱动来获得更高性能或特定功能的支持。所以，在安装前，需要将默认安装的`Nouveau`驱动禁用。

用vim编辑器打开黑名单配置文件：

![](img/a7b8a5c258b5041c47802735382b9aba.png)

在文件末尾添加如下代码，输入“`:wq`”后保存退出。

![](img/677661d3dedfe158606e3961a571791a.png)

#### Step 3. 让配置立即生效

![](img/10cb70d2b06e41f43059be6bf9455ad6.png)

#### Step 4. 使用Ubuntu自带的更新软件安装NVIDIA（强烈建议使用这种方式）

![](img/30e380fff922618185477e6075b64a3b.png)

#### Step 5. 选择驱动

直接选择对应的显卡驱动就好。如果没有，检查一下网络连接情况，如果联网了还没有，可能是显卡不支持、版本较低等情况，只能手动安装。

![](img/f7b36629fded175203ba56a34d86f127.png)

#### Step 6. 进行用户认证

![](img/0fed570b85e9a44afddbe0d175e61b41.png)

#### Step 7. 安装完成后执行重启操作

![](img/c1f26016c5d886aaeef29a68f7f35c6e.png)

#### Step 8. 验证驱动是否安装成功

![](img/02e45ae229fd05a8321ea9cbc39935cd.png)

输入`nvidia-smi`命令，如果能正确的输出当前环境下的GPU信息，则说明驱动安装成功。

#### 如果采用手动安装

如果有的小伙伴的电脑无法直接使用Ubuntu自带的更新软件安装NVIDIA的显卡驱动，则需要按照如下过程来执行安装步骤：

[先进入NVIDIA的官网，选择最适合自己显卡型号的驱动](https://www.nvidia.cn/Download/index.aspx?lang=cn)

![](img/0e12224ca1f4595eec6b6877fe4d5a3e.png)

选择好显卡驱动和适用平台后，点击下载。

![](img/398ba102bbd50d337b64d6570fbdbdd4.png)

下载完成后，对该驱动添加执行权限，否则无法进入安装页面。

![](img/de6204d57ec9f4871ffd5922f7d178be.png)

在安装之前，需要关闭图形化界面，需要判断你目前的ubuntu系统的图像化界面管理器是`gdm3`（默认）或是其它。`gdm3`或`lightdm`负责登录界面和用户会话的初始化，是系统启动进程的一部分，用于用户登录和启动图形用户界面（GUI）会话。其中`gdm3`是安装Ubuntu系统时默认安装的，而`lightdm`可以选择性安装，它是一个更轻量级的显示管理器。



关闭的原因是因为显示管理器（如 `gdm3`、`lightdm`）控制着图形界面，包括使用显卡驱动来显示内容。在这些图形界面运行时尝试安装或更新显卡驱动可能会导致冲突，因为驱动程序文件可能正在被系统使用。所以我们需要进入命令行模式来安装显卡驱动。

如果之前执行过 `sudo apt install lightdm`，就说明当前环境下已经使用 `lightdm` 代替了 `gdm3`，此时需要如下命令关闭：

```bash
sudo service lightdm stop
```

否则就是默认的 `gdm3`，这样关闭：

```bash
sudo /etc/init.d/gdm3 stop
```

关闭后，进入命令行模式。最简单的方法是使用 `telinit` 命令更改为运行级别 3。执行以下 Linux 命令后，显示服务器将停止。

```bash
sudo telinit 3
```

通过 `Ctrl+Alt+F3`（F1-F6）快捷键打开终端，先登录然后输入下面命令：

```bash
# 删除已安装的显卡驱动
```

随后进入安装界面，依次选择“Continue” --> 不安装 32 位兼容库(选择 `no`) --> 不运行 `X` 配置(选择 `no`) 即可。最后输入 `reboot` 命令重启主机。重新进入图形化界面，在终端输入 `nvidia-smi` 命令即可。

### 2.2 如何理解 CUDA

有一个误区，就是安装完驱动后，通过 `nvidia-smi` 命令可以看到 CUDA 版本，本机显示版本为 `CUDA Version: 12.2`，很多人以为已经安装了 CUDA 12.2 版本，但实质上，这指的是显卡驱动兼容的 CUDA 版本。意味着我们当前的系统驱动支持的 CUDA 最高版本是 12.2。安装更高版本的 CUDA 可能会导致不兼容的问题。

需要明确的概念：显卡驱动可以使计算机系统能够识别和使用显卡，但这与安装 CUDA 是两个不同的过程。 `CUDA`（Compute Unified Device Architecture）是 NVIDIA 开发的一个平台，允许开发者使用特定的 NVIDIA GPU 进行通用计算。它主要用于那些需要大量并行处理的计算密集型任务，如深度学习、科学计算、图形处理等。如果我们的应用程序或开发工作需要利用 GPU 的并行计算能力，那么 `CUDA` 是非常关键的。但如果只是进行常规使用，比如网页浏览、办公软件使用或轻度的图形处理，那么安装标准的显卡驱动就足够了，无需单独安装 `CUDA`。对我们要做大模型实践的需求来看， `CUDA` 一定是要安装的。

`CUDA` 提供了两种主要的编程接口： `CUDA Runtime API` 和 `CUDA Driver API`。

+ `CUDA Runtime API` 是一种更高级别的抽象，旨在简化编程过程，它自动处理很多底层细节。大多数 `CUDA` 程序员使用 `Runtime API` ，因为它更易于使用。

+ `CUDA Driver API` 提供了更细粒度的控制，允许直接与 `CUDA` 驱动交互。它通常用于需要精细控制的高级应用。

而要安装 `CUDA`，其实就是在安装 `CUDA Toolkit` ，其版本决定了我们可以使用的 `CUDA Runtime API` 和 `CUDA Driver API` 的版本，当安装 `CUDA Toolkit` 时会安装一系列工具和库，用于开发和运行 `CUDA` 加速的应用程序。这包括了 `CUDA` 编译器（ `nvcc` ）、 `CUDA` 库和 API，以及其他用于支持 `CUDA` 编程的工具。如果安装好 `CUDA Toolkit`，就可以开发和运行使用 `CUDA` 的程序了。

当我们运行 `CUDA` 应用程序时，通常是在使用与安装的 `CUDA Toolkit` 版本相对应的 `Runtime API`。这可以通过 `nvcc -V` 命令查询：

![](img/b19e6a9f1dff742723fc0ba12700d65d.png)

可以看到，默认是并没有安装的。可以直接通过提示的命令进行安装。

![](img/2afc0cc96835f7e8cb23ebfbe4ed022b.png)

通过 `apt install nvidia-cuda-toolkit` 安装的是 Ubuntu 仓库中可用的 `CUDA Toolkit` 版本，这可能不是最新的，也可能不是特定需要的版本。主要用于本地 `CUDA` 开发（如果想直接编写 `CUDA` 程序或编译 `CUDA` 代码）。

如果想安装指定版本的 `CUDA-Toolkit`，如何操作呢？

需要进入 NVIDIA 官网：[CUDA Toolkit Archive](https://developer.nvidia.com/cuda-toolkit-archive) ，找到需要下载的 `Cuda` 版本。

![](img/08a2f2b356c3e0b78294ae1efb85ac18.png)

根据当前情况依次选择操作系统、版本等。

![](img/41c10b9b0a0461eeb487051c0725bd30.png)

最后根据当前官方给出的代码，在终端执行即可安装。

![](img/42731f86e2b3f1e1ba3104e59c470627.png)

但其实，通常不需要预先手动安装 `CUDA` ，因为我们目前使用的 `Pytorch` 等框架在安装过程会处理这些依赖。当我们通过 `Conda/pip` 等方式安装 `Pytorch` 时会指定的 `CUDA` 版本，该 `CUDA` 版本就会与当前的 `Pytorch` 版本相兼容，预编译并打包了与 `CUDA` 版本相对应的二进制文件和库。所以除非有特定的需求或要进行 `CUDA` 级别的开发，才可能需要手动安装 `CUDA Toolkit`。

### 2.3 安装 Anaconda 环境

`Anaconda` 是一个为科学计算设计的发行版，适用于数据科学、机器学习、科学计算和工程领域。

它会提供大量预安装的科学计算和数据科学相关的库，且提供了 `Conda` 这样一个包管理器，用来安装、管理和升级包，同时也可以创建隔离的环境以避免版本和依赖冲突。相较于单独安装 `Python`，对初学者更友好，尤其是对于不熟悉 `Python` 和包管理的用户。

运行大模型需要 `Python` 环境。所以我们这里选择使用 `Anaconda` 来构造和管理 `Python` 环境。

Step 1. 进入 `Anaconda` 官网：https://www.anaconda.com/download

![](img/820b621f440b51c21ee9f3cd913d39fd.png)

**. Step 2. 下载安装程序**

`Anaconda` 官网会根据系统版本自动下载对应的安装程序。

![](img/f22457225a742904e2bcc72c69b66c5a.png)

**. Step 3. 进入终端，执行安装**

找到安装包的下载位置，执行如下命令：

```bash
bash Anaconda3-2023.09.0-Linux-x86_64.sh
```

![](img/7b1195df14a0a2885ed780cc702f16b0.png)

在此处输入 `yes`，然后按 `Enter` 键使用 `Anaconda` 的默认安装位置 （`/home/${account}/anaconda3`）。

![](img/f3724197c67237fdecb235e789062151.png)

**. Step 4. 等待安装完成**

![](img/5915c64ff840ed5fdb0b42ce843bbb0c.png)

**. Step 5. 验证安装情况**

安装完成后，会在对应的安装目录中出现 `anaconda3` 文件夹。

![](img/bb9cac2853c20a2a52e1ebbf34265dc3.png)

**. Step 6. 配置环境变量**

在终端的命令行修改配置文件：

```bash
vim ~/.bashrc
```

![](img/8e6f1d42baf21324dafd6b5e1a8d375d.png)

在打开的配置文件末尾添加 `export PATH= {Anaconda3 的实际安装路径}`，配置完成后，按 `:wq!` 保存并退出。

```bash
# 我的 anaconda3 的安装路径是 /home/muyu/anaconda3
```

![](img/4cd87367d5bc26ea4bf624dcc5dfabf8.png)

使用如下命令使环境变量的修改立即生效。

![](img/0c848e2974f519ea969c58d3f6ec338b.png)

**. Step 7. 启动 Anaconda**

配置好环境变量后，在终端输入 `anaconda-navigator` 即可打开 `Anaconda`，和 `Windows` 操作系统下的操作就基本一致了。

![](img/bb719778c330910f3f63623d2799a482.png)

## 三、 ChatGLM3-6B 介绍与快速入门

`ChatGLM3` 是智谱 AI 和清华大学 `KEG` 实验室在 2023 年 10 月 27 日联合发布的新一代对话预训练模型。 `ChatGLM3-6B` 是 `ChatGLM3` 系列中的开源模型，免费下载，免费的商业化使用。

该模型在保留了前两代模型对话流畅、部署门槛低等众多优秀特性的基础上， `ChatGLM3-6B` 引入了如下特性： `ChatGLM 3 GitHub`

1. **更强大的基础模型：** `ChatGLM3-6B` 的基础模型 `ChatGLM3-6B-Base` 采用了更多样的训练数据、更充分的训练步数和更合理的训练策略。在语义、数学、推理、代码、知识等不同角度的数据集上测评显示，在 44 个中英文公开数据集测试中处于国内模型的第一位。 `ChatGLM3-6B-Base` 具有在 `10B` 以下的基础模型中最强的性能。

2. **更完整的功能支持：** [ChatGLM3](https://github.com/THUDM/ChatGLM3/blob/main/PROMPT.md)[-6B](https://github.com/THUDM/ChatGLM3/blob/main/tool_using/README.md) 采用了全新设计的 Prompt 格式，除正常的多轮对话外。同时 原生支持工具调用（ `Function Call` ）、代码执行（ `Code Interpreter` ）和 `Agent` 任务等复杂场景。

3. **更全面的开源系列：** 除了对话模型 [ChatGLM3-6B](https://huggingface.co/THUDM/chatglm3-6b) 外，还开源了基础模型 `ChatGLM3-6B-Base`、长文本对话模型 [ChatGLM3-6B-32K](https://huggingface.co/THUDM/chatglm3-6b-32k)。以上所有权重对学术研究完全开放，在填写问卷进行登记后亦允许免费商业使用。

![](img/bdf510d505c46e4b30e1511dc4d0a8e9.png)

性能层面， `ChatGLM3-6B` 在 `10B` 范围内性能最强，推理能力直逼 `GPT-3.5`；功能层面， `ChatGLM3-6B` 重磅更新多模态功能、代码解释器功能、联网功能以及 `Agent` 优化功能四项核心功能，全线逼近 `GPT-4`！

![](img/a8fa97163c08b333b1a37e2f74e2ed02.png)

`AI Agent` （人工智能代理）是一个能够自主执行任务或达成目标的系统或程序，能够围绕复杂问题进行任务拆解，规划多步执行步骤；能够实时围绕自动编写的代码进行 `debug`；能够根据人类意见反馈修改答案，实时积累修改对话，并进行阶段性微调等等，具有很强的决策和执行能力。那 `ChatGLM3-6B` 模型开放的 `Function calling` 能力，是大语言模型推理能力和复杂问题处理能力的核心体现，是本次 `ChatGLM 3` 模型最为核心的功能迭代，也是 `ChatGLM 3` 模型性能提升的有力证明。

![](img/afcf97d94054e3e9230261a8d841da4b.png)

相关的信息获取方途径

. 官方网站：https://www.zhipuai.cn/

. 智谱清言： https://chatglm.cn

. API 开放平台： https://bigmodel.cn/

. Github 仓库：https://github.com/THUDM

开源模型列表：

| 模型 | 介绍 | 上下文 | 代码链接 | 模型权重下载链接 |
| --- | --- | --- | --- | --- |
| `ChatGLM3-6B` | 第三代 ChatGLM 对话模型。 | 8K | [Huggingface](https://huggingface.co/THUDM/chatglm3-6b) | [魔搭社区](https://modelscope.cn/models/ZhipuAI/chatglm3-6b)、[始智社区](https://www.wisemodel.cn/models/ZhipuAI/chatglm3-6b)、[Swanhub](https://swanhub.co/ZhipuAI/chatglm3-6b)、[启智社区](https://openi.pcl.ac.cn/Zhipu.AI/ChatGLM3/modelmanage/model_readme_tmpl?name=chatglm3-6b) |
| `ChatGLM3-6B-base` | 第三代 ChatGLM 基座模型。 | 8K | [Huggingface](https://huggingface.co/THUDM/chatglm3-6b-base) | [魔搭社区](https://modelscope.cn/models/ZhipuAI/chatglm3-6b-base)、[始智社区](https://www.wisemodel.cn/models/ZhipuAI/chatglm3-6b-base)、[Swanhub](https://swanhub.co/ZhipuAI/chatglm3-6b-base)、[启智社区](https://openi.pcl.ac.cn/Zhipu.AI/ChatGLM3/modelmanage/model_readme_tmpl?name=chatglm3-6b-base) |
| `ChatGLM3-6B-32k` | 第三代 ChatGLM 长上下文对话模型。在 `ChatGLM3-6B` 的基础上进一步强化了对于长文本的理解能力，能够更好的处理最多 32K 长度的上下文。 | 32K | [Huggingface](https://huggingface.co/THUDM/chatglm3-6b-32k) | [魔搭社区](https://modelscope.cn/models/ZhipuAI/chatglm3-6b-32k)、[始智社区](https://www.wisemodel.cn/models/ZhipuAI/chatglm3-6b-32k)、[Swanhub](https://swanhub.co/ZhipuAI/chatglm3-6b-32k)、[启智社区](https://openi.pcl.ac.cn/Zhipu.AI/ChatGLM3/modelmanage/model_readme_tmpl?name=chatglm3-6b-32k) |

![](img/4d50bfb30da3abfea2635e84e73cc206.png)

[ChatGLM3](https://github.com/THUDM/ChatGLM3)

## 四、 ChatGLM3-6B 私有化部署

对于部署 `ChatGLM3-6B` 来说，从官方说明上看，其规定了 `Transformers` 库版本应该 `4.30.2` 以及以上的版本，`torch` 库版本应为 `2.0` 及以上的版本，`gradio` 库版本应该为 `3.x` 的版本，以获得最佳的推理性能。所以为了保证 `torch` 的版本正确，建议大家严格按照官方文档的说明安装相应版本的依赖包。

Step 1. 创建 `conda` 虚拟环境

`Conda` 创建虚拟环境的意义在于提供了一个隔离的、独立的环境，用于 `Python` 项目和其依赖包的管理。每个虚拟环境都有自己的 `Python` 运行时和一组库。这意味着我们可以在不同的环境中安装不同版本的库而互不影响。例如，可以在一个环境中使用 `Python 3.8`，而在另一个环境中使用 `Python 3.9`。对于大模型来说，建议 `Python` 版本 3.10 以上。创建的方式也比较简单，使用以下命令创建一个新的虚拟环境：

```bash
# myenv 是你想要给环境的名称，python=3.8 指定了要安装的 Python 版本。你可以根据需要选择不同的名称和/或 Python 版本。
```

![](img/7f45e245365b2b75a38ecbec1f4f6ec9.png)

创建虚拟环境后，需要激活它。使用以下命令来激活刚刚创建的环境。

![](img/5354d3b6038d10355deeb08d16e111d8.png)

如果成功激活，可以看到在命令行的最前方的括号中，就标识了当前的虚拟环境 （`chatglm3_test`），然后，按照官方的要求安装 `torch`。

**. Step 2. 查看当前驱动最高支持的 CUDA 版本**

我们需要根据 `CUDA` 版本选择 `Pytorch` 框架，先看下当前的 `CUDA` 版本：

![](img/9847573aeb7084a776edc5bd808f6228.png)

**. Step 3. 在虚拟环境中安装 Pytorch**

进入 [PyTorch 官网](https://pytorch.org/get-started/previous-versions/)。

![](img/a99c0d897bbdd3a0ae8db9822e0784b9.png)



当前的电脑 `CUDA` 的最高版本要求是 `12.2`，所以需要找到 `>=12.2` 版本的 `PyTorch`。

![](img/404621ff1fbb14e5c4f77a97aa901a81.png)

直接复制对应的命令，进入终端执行即可。这实际上安装的是为 `CUDA` 12.1 优化的 `PyTorch` 版本。这个 `PyTorch` 版本预编译并打包了与 `CUDA` 12.1 版本相对应的二进制文件和库。

![](img/2dbf90dc1b9137f54869f4d065e55a7c.png)

## Step 4. 安装 PyTorch 验证

待安装完成后，如果想要检查是否成功安装了 GPU 版本的 `PyTorch`，可以通过几个简单的步骤在 `Python` 环境中进行验证：

```
import torch
```

![](img/88474ef368810b5080a9f9d967bb5ac8.png)

如果输出是 `True`，则表示 GPU 版本的 `PyTorch` 已经安装成功并且可以使用 `CUDA`，如果输出是 `False`，则表明没有安装 GPU 版本的 `PyTorch`，或者 `CUDA` 环境没有正确配置，此时根据教程，重新检查自己的执行过程。

## Step 5. 下载 ChatGLM3 的项目文件

`ChatGLM3` 的代码库和相关文档存储在 `GitHub` 这个在线平台上。`GitHub` 是一个广泛使用的代码托管平台，它提供了版本控制和协作功能。

[要下载 ChatGLM3-6B 的项目文件，需要进入 ChatGLM3 的 Github：https://github.com/THUDM/ChatGLM3](https://github.com/THUDM/ChatGLM3)

![](img/7eb90b30f81868df6fea95ed27281a82.png)

在 `GitHub` 上将项目下载到本地通常有两种主要方式：克隆 (`Clone`) 和 下载 ZIP 压缩包。

克隆 (`Clone`) 是使用 `Git` 命令行的方式。我们可以克隆仓库到本地计算机，从而创建仓库的一个完整副本。这样做的好处是我们可以跟踪远程仓库的所有更改，并且可以提交自己的更改。如果要克隆某一个仓库，可以使用如下命令：

```
git clone <repository-url>   # 其中 <repository-url> 是 GitHub 仓库的 URL。
```

推荐使用克隆 (`Clone`) 的方式。对于 `ChatGLM3` 这个项目来说，我们首先在 `GitHub` 上找到其仓库的 URL。

![](img/94bf6c03ab7cf4ff2139a809fd95d021.png)

在执行命令之前，先安装 `git` 软件包。

![](img/a985c87abee886c2f2247c97090f9022.png)

然后创建一个存放 `ChatGLM3-6B` 项目文件的文件夹。

![](img/7b89673fb341f8e42949c21c5090429f.png)

执行克隆命令，将 `Github` 上的项目文件下载至本地。

![](img/f0a20ed7b9900f5c75662a3715db4eec.png)

如果克隆成功，本地应该会出现如下文件内容：

![](img/4e39a3e0344a2d24dbeed131751b3a8d.png)

除了直接通过 `git clone` 的方式拉取代码至本地，也可以直接下载压缩包。这是更简单的下载方式，不需要使用 `Git`，适合那些不打算使用 `Git` 版本控制的用户。在 `GitHub` 仓库页面上，通常会有一个 “Download ZIP” 按钮，我们可以点击这个按钮下载仓库的当前状态的压缩包

![](img/cbf701be42869fed210173125c0be47f.png)

选择压缩包的下载路径。

![](img/fed0b0153fc25f827de054d2639b62dd.png)

下载后，只需解压缩该文件即可访问项目文件。压缩包中存放的是 `ChatGLM3` 运行的一些项目文件。

![](img/e689a949cd285a0edaeca2e088047753.png)

通过这种方式下载的项目文件，需要 `xftp` 这样的工具在上传到服务器使用。

## Step 6. 升级 pip 版本

`pip` 是 `Python` 的一个包管理器，用于安装和管理 `Python` 软件包。允许从 `Python Package Index`（`PyPI`）和其他索引中安装和管理第三方库和依赖。一般使用 `pip` 来安装、升级和删除 `Python` 软件包。除此之外，`pip` 自动处理 `Python` 软件包的依赖关系，确保所有必需的库都被安装。在 `Python` 环境中，尽管我们是使用 `conda` 来管理虚拟环境，但 `conda` 是兼容 `pip` 环境的，所以使用 `pip` 下载必要的包是完全可以的。

我们建议在执行项目的依赖安装之前升级 `pip` 的版本，如果使用的是旧版本的 `pip`，可能无法安装一些最新的包，或者可能无法正确解析依赖关系。升级 `pip` 很简单，只需要运行如下命令：

```
python -m pip install --upgrade pip
```

![](img/2435401b75a2c73bb7a29e3c1e6cdd9f.png)

## Step 7. 使用 pip 安装 ChatGLM 运行的项目依赖

一般项目中都会提供 `requirements.txt` 这样一个文件，该文件包含了项目运行所必需的所有 `Python` 包及其精确版本号。使用这个文件，可以确保在不同环境中安装相同版本的依赖，从而避免了因版本不一致导致的问题。我们可以借助这个文件，使用 `pip` 一次性安装所有必需的依赖，而不必逐个手动安装，大大提高效率。命令如下：

```
pip install -r requirements.txt
```

![](img/aee64caac0469ef853f4c3b9036f705f.png)

## Step 8. 从 Hugging Face 下载 ChatGLM3 模型权重

经过 Step 8 在 `Hugging Face` 下载模型权重的操作后，当前的 `ChatGLM3-6B` 模型的项目文件中会出现 `chatglm3-6b` 这样一个新的文件。

![](img/8c6fe2fe6c814e3fe55b59277376af79.png)

`chatglm3-6b` 中的文件内容如下，请确保不缺少文件。

![](img/7fa7f361ac3f9e1cc309b633758c9b88.png)

至此，我们就已经把 `ChatGLM3-6B` 模型部署运行前所需要的文件全部准备完毕。

## 五、运行 ChatGLM3-6B 模型的方式

`ChatGLM3-6B` 提供了一些简单应用 `Demo`，存放在供开发者尝试运行。这里我们由简到难依次对其进行介绍。

![](img/58bd2d502b46f76155215f1d337ace69.png)

### 5.1 基于命令行的交互式对话

这种方式可以为非技术用户提供一个脱离代码环境的对话方式。对于这种启动方式，官方提供的脚本名称是：`cli_demo.py`。

![](img/cb68e9aa90bcfbe30056770146ea6aa5.png)

基于网页端的对话是目前非常通用的大语言交互方式，`ChatGLM3` 官方项目组提供了两种 Web 端对话 demo，两个示例应用功能一致，只是采用了不同的 Web 框架进行开发。首先是基于 `Gradio` 的 Web 端对话应用 demo。`Gradio` 是一个 `Python` 库，用于快速创建用于演示机器学习模型的 Web 界面。开发者可以用几行代码为模型创建输入和输出接口，用户可以通过这些接口与模型进行交互。用户可以轻松地测试和使用机器学习模型，比如通过上传图片来测试图像识别模型，或者输入文本来测试自然语言处理模型。`Gradio` 非常适合于快速原型设计和模型展示。

对于这种启动方式，官方提供的脚本名称是：`web_demo_gradio.py`。同样，我们只需要使用 `vim` 编辑器进入修改模型的加载路径，直接使用 `python` 启动即可。

![](img/bbdd0db090b72f7e8855f4b7ae1e42f8.png)

如果启动正常，会自动弹出 Web 页面，可以直接在 Web 页面上进行交互。

![](img/07c1ebfebaadd9672ad34be9ebbf0cfe.png)

### 5.3 基于 Streamlit 的 Web 端对话应用

`ChatGLM3` 官方提供的第二个 Web 对话应用 demo，是一个基于 `Streamlit` 的 Web 应用。`Streamlit` 是另一个用于创建数据科学和机器学习 Web 应用的 `Python` 库。它强调简单性和快速的开发流程，让开发者能够通过编写普通的 `Python` 脚本来创建互动式 Web 应用。`Streamlit` 自动管理 UI 布局和状态，这样开发者就可以专注于数据和模型的逻辑。`Streamlit` 应用通常用于数据分析、可视化、构建探索性数据分析工具等场景。

对于这种启动方式，官方提供的脚本名称是：`web_demo_streamlit.py`。同样，先使用 `vim` 编辑器修改模型的加载路径。

![](img/a53a99c7733023a8566488adb848f0a2.png)

启动命令略有不同，不再使用 `python` ，而是需要使用 `streamkit run` 的方式来启动。

![](img/91e3287c5d85934cc4e883b3f51e7705.png)

### 5.4 在指定虚拟环境的 Jupyter Lab 中运行

我们在部署 `ChatGLM3-6B` 模型之前，创建了一个 `chatglme3_test` 虚拟环境来支撑该模型的运行。除了在终端中使用命令行启动，同样可以在 `Jupyter Lab` 环境中启动这个模型。具体的执行过程如下：

首先，在终端中找到需要加载的虚拟环境，使用如下命令可以查看当前系统中一共存在哪些虚拟环境：

```
conda env list
```

![](img/8ea041dc3ca86dd4dd3671502fa050bf.png)

这里可以看到我们之前创建的 `chatglm3_test` 虚拟环境，需要使用如下命令进入该虚拟环境：

```
# 这里的 `env_name` 就是需要进入的虚拟环境名称
```

![](img/d3076a1bc7ecee0a011bef17e6d82e1c.png)

在该环境中安装 `ipykernel` 软件包。这个软件包将允许 `Jupyter Notebook` 使用特定环境的 `Python` 版本。运行以下命令：

```
conda install ipykernel
```

![](img/187d89f7d50c21c5ce96dc854fe1e2ae.png)

将该环境添加到 `Jupyter Notebook` 中。运行以下命令：

```
# 这里的 `env_name` 替换成需要使用的虚拟环境名称
```

![](img/ca9b904d1a26439578f7fd2d429338da.png)

执行完上述过程后，在终端输入 `jupyter lab` 启动。

![](img/d381515b2cd213c24eec045fd94e19fd.png)

打开后就可以看到，当前环境下我们已经可以使用新的虚拟环境创建 `Notebook`。

![](img/d24788e336ffce7848cbe8dd1b1ecc4c.png)

基本调用流程也比较简单，官方也给出了一个实例：

![](img/74e529f9f086ee4daad014d69e30bb91.png)

只需要从 `transformers` 中加载 `AutoTokenizer` 和 `AutoModel`，指定好模型的路径即可。`tokenizer` 这个词大家应该不会很陌生，可以简单理解我们在之前使用 `gpt` 系列模型的时候，使用 `tiktoken` 库帮我们把输入的自然语言，也就是 `prompt` 按照一种特定的编码方式来切分成 `token`，从而生成 API 调用的成本。但在 `Transform` 中 `tokenizer` 要干的事会更多一些，它会把输入到大语言模型的文本，包在



`tokenizer`中去做一些前置的预处理，会将自然语言文本转换为模型能够理解的格式，然后拆分为 `tokens`（如单词、字符或子词单位）等操作。

而对于模型的加载来说，官方的代码中指向的路径是 `THUDM/chatglm3-6b`，表示可以直接在云端加载模型，所以如果我们没有下载`chatglm3-6b`模型的话，直接运行此代码也是可以的，只不过第一次加载会很慢，耐心等待即可，同时需要确保当前的网络是联通的（必要的情况下需要开🪜）。

因为我们已经将`ChatGLM3-6B`的模型权重下载到本地了，所以此处可以直接指向我们下载的 `Chatglm3-6b`模型的存储路径来进行推理测试。

![](img/456081e5493a654e75271cec30f842fc.png)

对于其他参数来说，`model` 有一个`eval`模式，就是评估的方法，模型基本就是两个阶段的事，一个是训练，一个是推理，计算的量更大，它需要把输入的值做一个推理，如果是一个有监督的模型，那必然存在一个标签值，也叫真实值，这个值会跟模型推理的值做一个比较，这个过程是正向传播。差异如果很大，就说明这个模型的能力还远远不够，既然效果不好，就要调整参数来不断地修正，通过不断地求导，链式法则等方式进行反向传播。当模型训练好了，模型的参数就不会变了，形成一个静态的文件，可以下载下来，当我们使用的时候，就不需要这个反向传播的过程，只需要做正向的推理就好了，此处设置 `model.eval()`就是说明这个过程。而`trust_remote_code=True` 表示信任远程代码（如果有），`device='cuda'` 表示将模型加载到CUDA设备上以便使用GPU加速，这两个就很好理解了。

### 5.5（重点） OpenAI风格API调用方法

`ChatGLM3-6B`模型提供了OpenAI风格的API调用方法。正如前文所说，在OpenAI几乎定义了整个前沿AI应用开发标准的当下，提供一个OpenAI风格的API调用方法，无疑也可以让`ChatGLM3`模型无缝接入OpenAI开发生态。所谓的OpenAI风格的API调用，指的是借助OpenAI库中的`ChatCompletion` 函数进行`ChatGLM3`模型调用。而现在，我们只需要在`model`参数上输入`chatglm3-6b`，即可调用`ChatGLM3`模型。调用API风格的统一，无疑也将大幅提高开发效率。

![](img/64d916231d11745166d51e32e528d007.png)

而要执行OpenAI风格的API调用，则首先需要安装`openai`库，并提前运行`openai_api.py`脚本。具体执行流程如下:

首先需要注意：OpenAI目前已将`openai`库更新至1.x，但目前`Chatglm3-6B`仍需要使用旧版本 0.28。所以需要确保当前环境的`openai`版本。

![](img/9a1eafeba7609e0be6705e0aedc34c44.png)

如果想要使用API持续调用`Chatglm3-6b`模型，需要启动一个脚本，该脚本位于 `open_api_demo` 中。

![](img/0e54f0e498216cb2ed89a8e225403dc7.png)

启动之前，需要安装`tiktoken`包，用于将文本分割成`tokens`。

![](img/571b92dbc8184c3cb5c790ac357cecd8.png)

同时，需要降级 `typing_extensions`依赖包，否则会报错。

![](img/692f73f51a89bf01f3445aaaf35a1d83.png)

最后，还需要安装 `sentence_transformers`依赖包，安装最新的即可。

![](img/1f21426aacea04fb30cd40210b26c1b1.png)

安装完成后，使用命令 `python openai_api.py`启动，第一次启动会有点慢，耐心等待。

![](img/c7f26917b2c23472da12763eabbb5ab1.png)

启动成功后，在`Jupyter lab`上执行如下代码，进行API调用测试。

![](img/77c23e9826510d5ac52899387173f68d.png)

如果上述代码出现如下报错的话，是因为开代理导致的，需要关闭，如果关闭后仍无法解决，重启电脑后才可重新运行。

![](img/7f1c972a17242a734634650fda1d2c3b.png)

如果服务正常是可以得到模型的回复的。

![](img/4aeec186938fe5265b8da87d95c5f7ea.png)

同时，在终端应用运行处，也可以看到API的实时调用情况。

![](img/0a461f6194ba0093e5d0dc6b2f0d0cf8.png)

除此之外，大家还可以去测试`ChatGLM3-6B`的`Function Calling`等更高级的用法时的性能情况。我们推荐大家使用OpenAI风格的API调用方法是进行学习和尝试构造高级的AI Agent，同时积极参与国产大型模型的开源社区，共同增强国内在这一领域的实力和影响力。

---

🍻现开设了大**模型学习交流群，**扫描下👇码，来遇见更多志同道合的小伙伴~

![](img/46524e06f65ff48f5009b7b54e4f7b8e.png)

海量硬核独家技术**干货内容**+无门槛**技术交流****+不定期开设****硬核干货&前沿技术公开课****，****扫码**👆即刻入群！
