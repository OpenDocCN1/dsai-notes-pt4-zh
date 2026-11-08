# 在Windows系统下部署运行ChatGLM3-6B模型

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/Qvl2dBU7DoPj2ixtwMfcnASmnJd](https://kq4b3vgg5b.feishu.cn/docx/Qvl2dBU7DoPj2ixtwMfcnASmnJd)

本文涉及到的所有ChatGLM3-6B模型的权重文件[及](https://pan.baidu.com/s/15sRruaOuzpyY-k_O8i3aLA?pwd=q88n)应用程序安装包，已全部上传至百度网盘：https://pan.baidu.com/s/15sRruaOuzpyY-k_O8i3aLA?pwd=q88n

在前几期视频中，我们已经详细的介绍了如何在Ubuntu 22.04 下部署运行ChatGLM3-6B模型服务，对   于大模型服务来说， Ubuntu通常在资源使用和性能方面更加高效，特别是在服务器和云环境中，更加适合于 多卡运行、服务优化等企业级的部署环境。但很多小伙伴还是比较倾向于在Windows系统上进行实践和学

习，虽然Windows操作系统在处理大规模计算任务时，性能和资源管理方面与Ubuntu相比略显不足，但更 贴合大多数人的使用习惯。

从部署过程来看，在Windows操作系统上部署ChatGLM3-6B模型与Ubuntu操作系统上看并无太明显的 差别。在本期课程中，我们将一步步详细介绍如何在Windows 11操作系统上部署大模型服务。通过这些实   用的指导，希望帮助大家有效地完成部署流程，确保ChatGLM3-6B模型在Windows 11环境下运行顺畅。

ChatGLM相关的信息获取方途径：

.  官方网站：https://www.zhipuai.cn/

.  智谱清言： https://chatglm.cn

.  API开放平台： https://bigmodel.cn/

.  Github仓库：https://github.com/THUDM

大模型本身运算的复杂性，目前任何开源大模型的运行都有一定的硬件要求。当然，相比之下，

ChatGLM系列模型的硬件门槛相对更低，而且支持多种不同的运行模式。根据官方介绍，  ChatGLM目前支   持GPU运行（需要英伟达显卡）、 CPU运行以及Apple M系列芯片运行。其中GPU运行需要至少6GB以上显  存（4Bit精度运行模式下），而CPU运行则需要至少32G的内存。而由于Apple M系列芯片是统一内存架构， 因此最少需要13G内存即可运行。

![](img/1c5767e23af79567dfddc1fb4c9c052a.png)

考虑到CPU运行模式下内存占用过大且运行效率较低，因此这里我们将重点介绍更为通用的GPU模式部  署方法。当然，如果不想通过本[地部署而](https://www.chatglm.cn/)直接体验ChatGLM系列模型的对话功能，也推荐大家直接使用智谱 AI推出的大模型对话应用——智谱清言。 👉 智谱清言

![](img/f32015ce747f709e72463d0db4d28349.png)

# **1. 查询计算机硬件配置                 **

在正式安装之前，需要先确认当前电脑的硬件环境。正如此前所说，部署和运行ChatGLM3-6B最低需要 6GB显存。这里我们可以在Windows操作系统下的设备管理器中查看当前电脑的显卡型号，并根据显卡型号  搜索得到显卡显存规格。

如果显存低于6.7GB，无法在GPU上运行ChatGLM3-6B模型。查看方式如下：

![](img/db338420db55ecd8300e6b511fd45e16.png)

查看显卡等级。

![](img/3075b9b21661e0a07d7535b9c01d5f44.png)

不同的显卡等级有不同的显存大小，可以根据显卡型号去查询显存的大小。同时也有一种更直接的查询 方法，如下：

.  **Step 1. 同时按下Windows**键和R键，在运行里面输入“dxdiag”

![](img/40ff8ac20a2171eb576a50f38e9dde18.png)

**.  ****Step 2. 若弹出一个DirectX诊断工具**对话框，提示我们是否希望检查，点击是即可

![](img/2feffdb5ffc019d4394871e80a19b086.png)

**.  ****Step 3. 通过弹出的Direc**tX诊断工具界面---显示标签中查看显存

![](img/976075f3af6a14c48a5ee71570cf0d41.png)

# **2**. 安装NVIDIA显卡驱动

一般来说，台式机显卡需要2060（ 6G显存显卡）及以上显卡，笔记本则需要3060（ 8G显存显卡）及以 上显卡。这里需要注意，同型号的显卡，移动端显卡（笔记本显卡）要比主机显卡（台式机显卡）性能要

弱、显存也会更少。

如果你的电脑中出现这两种情况：

![](img/0e0d8701562325f454d889e0d5d3a7f6.png)

说明你的电脑上并没有安装NVIDIA的驱动，需要下载安装。如果有，可以跳过此章节。

.  S[tep 1. 进入NVIDIA驱动的官网 ：https:/](https://www.nvidia.cn/Download/index.aspx?lang=cn)/www.nvidia.cn/Download/index.aspx?lang=cn 根据自己电脑上的显卡型号、系统版本搜索最合适的显卡驱动。

![](img/7c5952b098587af424d9313725b5f376.png)

系统会自动匹配最合适的显卡驱动，这里执行下载。

![](img/2aa072fedf5618d5089d4c69a6d36e4a.png)

这里就会提示该驱动还会额外安装NVIDIA控制面板，选择 同意并开始下载 。

![](img/995e4da62679e2bac7f265e435f3bda9.png)

如果网络情况正常的话，这里会弹出下载的弹窗，选择需要保存下载文件的位置，等待下载完成。

![](img/73a694fbbfaf82287f7acbf29e49e683.png)

**  ****.  **Step 2. 安装NVIDIA驱动

下载完成后，找到驱动的.exe文件，执行傻瓜式安装。

![](img/9dcc4395422551375d3a5b37227333aa.png)

这里建议选择默认的路径，因为有些程序如果不在指定路径可能会发生意外错误，所以为了确保一次性 安装成功，不进行更改是一个比较好的选择。

![](img/b87331188222c9fd8c872eb556cb84f4.png)

选择安装显卡驱动和GeForce Experience。

![](img/3289be788106731a2779d3b0253d6e12.png)

选择精简安装。

![](img/1e8b015ecc49b31f605bfbc92d7a4f1f.png)

这个安装过程会有几次黑屏，属于正常现象。

![](img/058774c5dae684a2f07e1227de1facdd.png)

**.  **Step 3. 验证NVIDIA驱动安装是否成功

安装完成后，需要重启电脑，才会生效。验证方法是：重启电脑后，在桌面点击鼠标右键，出现如下图 标即可表明安装成功。

![](img/401a9a5e0d405f2e488b6c1a03142b3b.png)

NVIDIA控制面板会显示显卡驱动和显卡的基础参数等，同时支持进行个性化设置。

![](img/a5348af74ee2d714c64e89d09f48f789.png)

# **3. 本地部署ChatGLM3-6B模型      **

要部署和运行ChatGLM3-6b，我们需要下载两部分文件。第一部分是ChatGLM3-6B的项目文件，这包 含ChatGLM3-6B模型的一些代码逻辑文件，官方提供了包括运行、微调等Demo，可以让我们快速启动

ChatGLM3-6B模型服务。第二部分是ChatGLM3-6B模型的权重文件，直白点说就是ChatGLM3-6B这个模型 本身。

首先我们来下载第一步部分， ChatGLM3-6B模型的项目文件。这一部分代码库和相关文档存储在

GitHub 这个在线平台上。 GitHub 是一个代码托管平台，提供版本控制和协作功能。要下载其项目文件，需 进入ChatGLM3-6B的GitHub官网：https://github.com/THUDM/ChatGLM3

![](img/db949d2dace64a4d32ebfa50f7fd58fb.png)

提供了两种下载方式：

+   方式一：克隆 (Clone)是使用 Git 命令行的方式。可以克隆仓库到本地计算机，创建仓库的一个完整副 本。好处是可以跟踪远程仓库的所有更改，并且可以提交自己的更改。

+   方式二：不需要使用 Git，直接下载压缩包解压即可使用。适合对 Git 不熟悉的用户。

接下来我们对上述两种方式一一进行介绍。但强烈建议使用Git命令行的方式（方式一）进行项目管理。

![](img/98149086354aa30e72ef33aa03647027.png)

## **3**.1 下载项目文件（二选一）

### **3**.1.1 方式一：使用Git工具下载（推荐）

ChatGLM3-6B模型的项目文件托管在GitHub上，而GitHub是基于Git的服务， GitHub作为一个代码托 管服务，它托管使用Git来对项目做版本控制。如果要复制（克隆） 一个GitHub仓库到本地计算机，需要使 用 git clone命令可以。这个命令是Git的一部分，而Windows操作系统默认不自带Git。因此，为了使用  git clone或其他Git功能，需要先从Git的官方网站下载并安装Git。

.  Step 1. 进入Git官网： https://git-scm.com/

![](img/b8f356f7865ddf79bf308aa572177f3c.png)

**.  ** Step 2. 下载Windwos版本的Git安装程序

![](img/e00688946bcd8d546aa033c9bab403e5.png)

这里选择Windwos版本进行下载。

![](img/28036cbabfb397d3f225d2e92c01fc3f.png)

根据自己的Windwos系统情况，选择32-bit或者64bit。

![](img/08509b4d07d73df9e61d1f743870bdd6.png)

选择Git安装程序的存储路径。

![](img/7a0eda738646fa3eadbb431153853bca.png)

**.  ****Step 3. 安装Git**

![](img/15f8b7bbb04969753902bb9288d4d0f0.png)

选择Next。

![](img/d2403881c769d24733ef819587799449.png)

自行选择Git的安装位置。

![](img/0822d5e1e1fbab74072b70d25f9b8ac2.png)

接下来的选项，全部默认，点击Next即可。不影响正常的使用。

![](img/92c098e93c6aa5f8cc482dc48da80e1c.png)

![](img/9dbac721707142d284e1bb995d53722a.png)

![](img/377807fd8a05e4ec28c42a69b2e7b9b4.png)

![](img/3a6c4e8f45f7cb81b1d3de876a7fb672.png)

![](img/581105b802d27fff2ff342422dc4fa76.png)

默认选择到这里，执行安装。

![](img/0d65d225adf42733f6e587cbc4e9092f.png)

执行到此进度，说明安装已经完成。

![](img/070425fb0d7d394ceb3a61156ce6398b.png)

**.  **Step 4. 验证Git安装是否成功

在桌面上鼠标右键，如能看到如下两个快捷方式，说明已经正常安装Git。

![](img/9ffa6533b4da82a97ec6a983a918efa9.png)

点击Git Bash 后，进入命令行终端，可以输入 git --version 查看当前的Git版本。

![](img/a89ef159ec5b1988fe37c55801a5e959.png)

**.  **Step 5. 使用Git安装ChatGLM3-6B模型的项目文件

安装好Git后，就可以使用 git clone命令从GitHub上复制（克隆） ChatGLM3-6B 的仓库到本地。这 里我们需要先找到ChatGLM3-6B远程仓库的URL地址，查找方式如下：

ChatGLM3-6B的GitHub地址：https://github.com/THUDM/ChatGLM3

![](img/c43a698b8c1b6ef10eac68f402bfc2f4.png)

然后回到Git Bash的命令行终端，首先，通过 cd命令进入到想存放ChatGLM3-6B模型项目文件的路 径。我这里想存放的路径是 E:\03_LLM 。

![](img/b48cd4d806d6d99b203d91e375bf6067.png)

然后执行 git clone + 刚才复制的ChatGLM3-6B的远程仓库URL 。

![](img/6ad6cadc6d0b4f184f0d9c3b088bd658.png)

**.  **Step 6. 验证ChatGLM3-6B项目文件是否拉取成功

执行完 git clone + 刚才复制的ChatGLM3-6B的远程仓库URL操作后，会在指定的路径中生成 ChatGLM3 这个文件夹。

![](img/fabd760487c36cc2d202f05f98d3b271.png)

一些主要的项目文件说明如下。

![](img/45493d0d46a3b26096f5d9c3b1a70890.png)

### 3.1.2 方式二：直接打包下载

如果不想使用Git工具下载的方式，可以使用更加简单的直接打包下载ChatGLM3-6B模型项目文件的方 式，具体执行方法如下：

ChatGLM3-6B的GitHub地址：https://github.com/THUDM/ChatGLM3 还是先进入ChatGLM3-6B的GitHub官网，选择直接下载ZIP压缩包。

![](img/84812e2061f9e148f01354785211cd7d.png)

自行选择压缩包的本地存储路径。

![](img/cad5b92e80eabb16a7e82df0706c909b.png)

下载完成后，使用解压工具解压压缩包。

![](img/a0d6e1c32340b96b2f77dac70d90e654.png)

解压完成后，该文件夹中存放的就是ChatGLM3-6B模型的项目文件。

![](img/c53a593ddc89dcb1b31f7d2a9982946e.png)

存放的文件和使用Git方式下载的文件是完全一致的。

![](img/2cc6873b6c14dc003ed996c82f568037.png)

## **3**.2 配置项目运行环境（二选一）

大家无论是通过 git clone下载ChatGLM3-6B的项目文件方式，还是直接下载压缩包的方式，都需要   使用安装该项目的依赖包，原因主要是：大多数Python项目依赖于特定的库或模块来正常运行。例如，  一个 网页爬虫可能依赖于requests库来发送网络请求，而数据分析项目可能依赖于pandas或numpy。这些依赖   不会随着项目代码一起被包含在GitHub仓库中。所以我们下载的自然也不会包含这部分的内容。所以需要安装这个依赖环境。

### 3.2.1 方式一：使用Anaconda创建项目依赖环境（推荐）

Anaconda 是一个流行的开源Python发行版，用于科学计算。它包含了数据科学和机器学习领域中常用 的一系列工具和库。其安装的方式也非常简单。具体安装过程如下：

.  Step 1. 进入Anaconda3的官网： https://www.anaconda.com/

![](img/96b9f1e73767fcf625ead2ea84d431f0.png)

**.  **Step 2. 下载Windows版本的Anaconda3

![](img/101ad45d3f27129e82a16a9a60077711.png)

如果网络正常，会自动弹出下载弹窗，自行选择下载路径。

![](img/8ce8465d98939939a20ed35fd13b5c52.png)

**.  **Step 3. 安装Windows版本的Anaconda3

直接鼠标双击运行安装程序，执行傻瓜式安装。

![](img/2048dc1bcc9e9091de58027474703b63.png)

![](img/01f8e89e16e4731591e6f59fd52f77e6.png)

这里根据情况自行选择，选择哪一个都不会影响Windows当前用户的使用。

![](img/484410b1757e35749a0304a8b83611ec.png)

这个强烈不建议修改，如修改后可能会导致一下环境变量出现问题。

![](img/0544fc4a645ab3ff64865ae64a46ba2d.png)

这里强烈建议勾选 Add Anaconda3 to my_path environment variable ，这会自动设置环境变量， 否则还需要后续再手动设置。

![](img/970d807e4a4b457ab25e6610be17abd7.png)

![](img/3d061040eaba1489c083e4eb48f894e5.png)

**.  **Step 4. 启动Anaconda3进行验证

![](img/d2456cb161a69548d7ea45fc829967aa.png)

根据个人情况，自行选择是否需要升级到最新版本。

![](img/840b10791f57fa9b7860021ab45294a9.png)

如果选择了升级的话，等待完成即可。

![](img/4dc7cfff7762cc695e756b68a2913018.png)

**.  **Step 5. Anaconda3简要说明

Anaconda Navigator 是 Anaconda 发行版中的一个图形用户界面（GUI），提供了一个直观的界面，

通过这个界面，用户可以管理不同的环境、安装和更新包，以及启动各种数据科学和机器学习工具，例如

Jupyter Notebook、Spyder、 RStudio 等。此外，它还提供了对 Anaconda Cloud 和其他资源的访问，使得 分享工作和发现其他人的工作变得更加容易。  Navigator 适合那些不习惯使用命令行界面的用户。其中比较   关键的应用程序已经做了标注：

1. Anaconda CMD.exe Prompt ：一个基于 Windows 命令行界面的工具，它预配置了 Anaconda 发行 版的环境变量。使用 Anaconda Prompt，可以执行各种与 Anaconda 相关的命令，例如创建和管理   Conda 环境、安装、更新和卸载包等。这是一个非常强大的工具，特别是对于那些熟悉命令行操作的  用户。

2. Powershell Prompt：类似于 Anaconda CMD.exe Prompt ， Powershell Prompt 是另一种命令行界 面，但它基于 Windows 的 PowerShell。 PowerShell 是一种跨平台的任务自动化解决方案，包含命令 行壳、脚本语言和配置管理框架。 Anaconda 的 Powershell Prompt 也被配置为可以直接执行与

Anaconda 和 Conda 相关的操作。

3. **PyCharm **：一个流行的 Python 集成开发环境（IDE），由 JetBrains 开发。它提供了代码完成、调 试、测试、版本控制等多种功能，非常适合 Python 项目的开发。

4.**Jupyter Lab**：它是 Jupyter Notebook 的下一代界面，提供了一个灵活和强大的工具集，用于交互式 数据科学和科学计算。 Jupyter Lab 不仅包括了 Jupyter Notebook 的所有功能，还增加了许多新特

性，如更灵活的窗口布局、更好的编辑器、实时预览、终端、文件浏览器等。它支持多种类型的文档和 活动，包括文本编辑器、 Jupyter 笔记本、数据视图等，是一个更为综合的数据科学工作环境。

5.Jupyter Notebook：它是一个开源的 Web 应用程序，允许创建和共享包含实时代码、方程、可视化 和叙述文本的文档。它广泛用于数据清洗和转换、数值模拟、统计建模、数据可视化、机器学习等领

域。Jupyter Notebook 支持超过 40 种编程语言，包括 Python、 R、Julia 和 Scala。它是一个非常流行 的工具，特别是在数据科学和学术研究中。

![](img/78aa2b90073f5b346e4bcc6f9d640c32.png)

.  **Step 6. 验证conda**

在Anaconda环境中， conda 是一个开源的包管理器和环境管理器，用于安装、运行和升级包和环境。 验证方式如下：

![](img/66a7dcd10f810070b37fdc6fd4bfc7c9.png)

点击后，会进入如下命令行终端，输入 conda --version  查看conda版本。

![](img/8926cba254d82d32cf99aba1d777d010.png)

该命令会生效的原因，是因为我们在安装Anaconda3的过程中，自动添加了conda的环境变量。

![](img/5c2d17f263f11a88e315b908c3c69b8c.png)

![](img/c6d874bde5730ba20a3e5061bd5628db.png)

.  Step 7. 升级conda到最新版本

强烈建议将conda 升级到最新版本，主要原因是最新版本的 conda 更有可能与其他最新的软件和库兼    容。比如对最新版本的Python的支持，以及与最新发布的包的兼容性，会在解析和冲突处理方面变得更加精 确和高效。升级非常简单，执行如下命令：

| # 首先更新conda工具 |
| --- |

在Anaconda Navigator 中进入命令行终端。

![](img/5259d41660099fbca29a414ef482bf83.png)

在命令行终端先更新conda工具

![](img/8668151515780438672e373a00aec58c.png)

在更新各依赖包。

![](img/23afd95fd613008e5a61eb171cfda542.png)

.  Step 8. 使用conda创建虚拟环境

使用 conda 安装项目依赖时，最好先创建一个虚拟环境，可以将项目的依赖与系统中的其他Python项 目隔离开来。不同的Python项目可能需要同一库的不同版本。在虚拟环境中工作可以防止版本冲突。

在 Conda 中创建虚拟环境的步骤相对简单。以下是创建虚拟环境的基本步骤：

首先，使用以下命令创建一个新的虚拟环境，其中 myenv 是给环境起的名字， python=3.11 指定了 Python的版本（大家可以根据需要选择不同的版本），如果你不指定Python版本， Conda将使用默认的 Python版本创建环境。

| # 大模型运行环境，要求python 3.10 版本以上 |
| --- |

![](img/04bd6a7b225ff857f32fd62d0ed3a043.png)

创建环境后，需要激活它才能开始使用。在 Windows 上，使用：

| conda activate windwos_chatglm3-6b |
| --- |

![](img/12899d9ff8a6bd3c63ac0751b6e53e4a.png)

一旦虚拟环境被激活，就可以开始安装所需的包了。

但需要注意：如果关闭终端，会退出该虚拟环境，此时可以通过这种方式重新进入：

![](img/086cfb9ee5012e1839326bf870402287.png)

.  Step 9. 使用pip安装ChatGLM3-6B项目的所有依赖包

一个项目一般都会提供一个 requirements.txt 文件，用于列出项目所需的所有Python包及其确切版 本。我们看一下：

![](img/9a7126e14abb5181b8b5cb5e48818580.png)

其文件内容如下：

![](img/32eb929998fb942f52c7498c0215f676.png)

Conda是一个强大的包管理器，尤其适合于处理复杂的科学计算包和环境，但它并不总是包含Python生 态系统中的所有包。在某些情况下，特定的Python包可能只在pip的PyPI仓库中可用，而不在Conda的渠道   中。进入虚拟环境后使用 pip 安装 requirements.txt 中的依赖包是常见的做法。所以我们这里使用如下命令 安装所有依赖项，而不是逐个手动安装，大大简化设置依赖包安装过程。

| # 使用 -i + 清华镜像源 ，加速安装 |
| --- |

![](img/fce09f4ff51a72873e1972a54f8b50c6.png)

**.  **Step 10. 查看当前NVIDIA显卡驱动最高支持的CUDA版本

在安装好Python的依赖环境后，接下来就需要安装GPU版本的PyTorch。 PyTorch是通用的深度学习框 架，ChatGLM3-6B运行过程需要借助PyTorch来完成相关计算。需要根据CUDA版本选择Pytorch框架，在  Windows操作系统上查看当前的CUDA版本方式如下：

![](img/6b11a13c6124a3bbe929dc97208221f7.png)

如果 nvidia-smi命令无法执行，请回到 2.安装显卡驱动步骤仔细检查安装过程。

.  [**Step 11. 根据CUDA版本要求安装Pytorch：h**](https://pytorch.org/get-started/previous-versions/)ttps://pytorch.org/get-started/previous-versions/

这里根据自己电脑显卡驱动的CUDA版本要求，在Pytorch官网中找到适合自己的Pytorch安装命令。

![](img/4664bf1315298f44175fcfddb2ff17d0.png)

复制安装命令，进入虚拟环境的终端中执行安装。注意：这个终端并不是查看 nvidia-smi 的windows 终端，而是我们创建虚拟环境的conda终端。

![](img/223d698cddbf685e497f1f379dff2736.png)

**.  **Step 12. 验证安装GPU 版Pytorch是否成功

待安装完成后，如果想要检查是否成功安装了GPU版本的PyTorch，可以通过几个简单的步骤在Python 环境中进行验证：

| import torch |
| --- |

![](img/dc8a535a6657ac558d01f68e2edb589b.png)

如果输出是 True，则表示GPU版本的PyTorch已经安装成功并且可以使用CUDA，如果输出是False，则 表明没有安装GPU版本的PyTorch，或者CUDA环境没有正确配置，此时根据教程，重新检查自己的执行过   程。

### 3.2.2 方式二：直接使用Python创建项目依赖环境

如果不想要使用Anaconda3来管理虚拟环境，可以直接在Windows系统上安装指定版本的Python包， 然后再使用该版本的Python来创建虚拟环境。操作过程如下：

.  Step 1. 进入Python官网下载指定版本的Python包：https://www.python.org/

![](img/bff76ae34884d77b667b17542ffe3f40.png)

自行选择Python版本，运行大模型环境，需要Python 3.10版本及以上。我这里选择Python 3.10.9 的 Windows 64位安装。

![](img/eee745834665a05556919c3364e6a9ac.png)

如果网络正常，会自动弹出下载弹窗，自行选择安装程序的存储路径。

![](img/101e180716f4d0cd8218c7c1655cd507.png)

**.  **Step 2. 下载完成后，执行傻瓜式安装

![](img/cf63e86b91e16d74b35b057eb0b633a3.png)

注意：这里勾选 Add python.exe to PATH ,这会在安装过程中将Python环境设置好环境变量，否则还 需要手动设置。

![](img/a2434eefe24e21dca4bc1f00ac3bfb03.png)

![](img/3ec67da6f86e64514c01ddf91459c8e5.png)

**.  **Step 3. 安装完成后，验证是否安装成功

能正确输出安装的Python版本，说明安装正常。

![](img/e60db23b872bbbc2071a06fba93bcaae.png)

.  Step 4. 升级pip到最新版本

pip 是 Python 的官方包管理工具，用于安装和管理 Python 包。它允许用户从 Python 包索引（PyPI） 中安装、升级和删除包。在创建新的虚拟环境前建议升级 pip ，新版本的 pip 会提供更好的安装性能，更准 确的依赖项解析。升级命令如下：

| pip install --upgrade pip |
| --- |

![](img/01405d2d59f97b5f0345aee5f3fc83ef.png)

.  Step 5. 使用pip创建虚拟环境

使用 pip 安装项目依赖时，最好先创建一个虚拟环境，可以将项目的依赖与系统中的其他Python项目隔 离开来。不同的Python项目可能需要同一库的不同版本。在虚拟环境中工作可以防止版本冲突。创建虚拟环 境的步骤相对简单。以下是创建虚拟环境的基本步骤：

打开命令行或终端，然后输入以下命令。这里假设你想要创建的虚拟环境名为 myenv。

| # 可以指定路径 |
| --- |

![](img/715624b283c7350373e1c9e09e1e9084.png)

创建环境后，需要激活它才能开始使用。在 Windows 上，使用：

| # 这里要修改成大家实际的路径 |
| --- |

![](img/898d9f9911dfbbd0a9c4178f5a17cf0b.png)

一旦虚拟环境被激活，就可以开始安装所需的包了。

.  Step 6. 使用pip安装ChatGLM3-6B项目的所有依赖包

一个项目一般都会提供一个 requirements.txt 文件，用于列出项目所需的所有Python包及其确切版 本。我们看一下：

![](img/1943c5610f83cbb14cdfa9d4915c0621.png)

其文件内容如下：

![](img/564e18c137df85f92dbc48b427564087.png)

进入虚拟环境后使用 pip 安装 requirements.txt 中的依赖包是常见的做法。所以我们这里使用如下命令 安装所有依赖项，而不是逐个手动安装，大大简化设置依赖包安装过程。

| # 使用 -i + 清华镜像源 ，加速安装 |
| --- |

![](img/cb4dfd6a3f3f2e2b33b95bf54ff20eda.png)

**.  **Step 7. 查看当前NVIDIA显卡驱动最高支持的CUDA版本

在安装好Python的依赖环境后，接下来就需要安装GPU版本的PyTorch。 PyTorch是通用的深度学习框 架，ChatGLM3-6B运行过程需要借助PyTorch来完成相关计算。需要根据CUDA版本选择Pytorch框架，在  Windows操作系统上查看当前的CUDA版本方式如下：

![](img/26792e73fed226bb30a95c791a9fad14.png)

如果 nvidia-smi命令无法执行，请回到 2.安装显卡驱动步骤仔细检查安装过程。

.  [**Step 8. 根据CUDA版本要求安装Pytorch：ht**](https://pytorch.org/get-started/previous-versions/)tps://pytorch.org/get-started/previous-versions/

这里根据自己电脑显卡驱动的CUDA版本要求，在Pytorch官网中找到适合自己的Pytorch安装命令。

![](img/7ada6191e16200bb80bd74fdaf68220d.png)

复制安装命令，进入虚拟环境的终端中执行安装。注意：这个终端并不是查看 nvidia-smi 的windows 终端，而是我们创建虚拟环境的conda终端。

![](img/93e2c7ce958947ab791614aa57bb4327.png)

**.  **Step 9. 验证安装GPU 版Pytorch是否成功

待安装完成后，如果想要检查是否成功安装了GPU版本的PyTorch，可以通过几个简单的步骤在Python 环境中进行验证：

| import torch |
| --- |

![](img/60729944898da6e245d6045154038833.png)

如果输出是 True，则表示GPU版本的PyTorch已经安装成功并且可以使用CUDA，如果输出是False，则 表明没有安装GPU版本的PyTorch，或者CUDA环境没有正确配置，此时根据教程，重新检查自己的执行过   程。

### 3.3.3 如何管理多个不同版本的Python环境？

对于[直](https://www.python.org/)[接使用Pyt](https://www.python.org/)hon直接创建虚拟环境的这种方式（3.2.2 方式二：直接使用Python创建项目依赖环境），如果需要切换python版本，则只能重新下载不同版本的Python的包，重复执行上述操作。比如我们   上述安装的是Python 3.10.9，如果另外一个项目需要使用Python3.12版本，则需要重新进入https://www.python.org/官网中下载。

![](img/a2163172d67e7f1a5243de196545da2c.png)

找到Python程序的安装位置。

![](img/ff491bb5af39ad4d0f4537bc5336e419.png)

所有Python版本的环境变量都在这里定义。

![](img/a55c3c69f44c956e48b474f9845e7e1e.png)

如果想通过不同的指令，启动不同版本的Python，可以在系统的环境变量中找到指定版本的Python存 放位置。

![](img/4f4ac14efc1f378abb2b353cbececae9.png)

默认所有版本的python执行文件都是python.exe，可以进入指定文件夹中看到。

![](img/441f92478ef899e246975b7e3a7cf029.png)

如果想区分不同版本Python，并在终端可以使用，可以通过如下方式添加版本编号。

![](img/3b78d555ced725fd8b868190f63dc0eb.png)

启动验证。

![](img/c04eaae119c5f1b1309cf1a51ae60b18.png)

通过这种方式来灵活的管理一台机器上的多个Python环境。

## **3**.3 下载模型权重文件（三选一）

经过 3.1 下载项目权重文件和 3.2 配置项目运行环境两步操作后，的操作过程，我们下载到的只是

ChatGLM3-6B的一些运行文件和项目代码，并不包含ChatGLM3-6B这个模型。这里我们需要进入到

Hugging Face 下载。最后一步需要把ChatGLM3-6B这个模型本身下载到本地。这里提供三种下载的方式， 大家根据自身的实际情况灵活选择。

+     方式一：借助Git工具在Hugging Face官网拉取到本地。这种方式需要🪜。

![](img/9f1f6fb4b77f6a9d3901587dd34a51f8.png)

接下来依次介绍三种下载方式。

### 3.1 方式一：从Hugging Face官网下载（需要🪜环境）

Hugging Face 是一个丰富的模型库，开发者可以上传和共享他们训练好的机器学习模型。这些模型通常 是经过大量数据训练的，并且很大，因此需要特殊的存储和托管服务。不同于GitHub ，GitHub 仅仅是一个   代码托管和版本控制平台，托管的是项目的源代码、文档和其他相关文件。同时对于托管文件的大小有限

制，不适合存储大型文件，如训练好的机器学习模型。相

反， Hugging Face 专门为此类大型文件设计，提供了更适合大型模型的存储和传输解决方案。

.  [**Step 1. 进入Hugging Fa**](https://huggingface.co/THUDM/chatglm3-6b)ce的官网：https://huggingface.co/THUDM/chatglm3-6b

![](img/cd922e50f5d7369dcd77fa285f7a238c.png)

需要执行两个操作，第一步是初始化gitlfs工具，第二步是下载模型权重文件。

![](img/5da0781cf9fd0c8604d6b29974b92ac1.png)

.  Step 2. 初始化 gitlfs工具

Git Large File Storage（ Git LFS）是一种用于处理大文件的工具，在 Hugging Face 下载大模型时，通 常需要安装 Git LFS，主要的原因是： Git 本身并不擅长处理大型文件，因为在 Git 中，每次我们提交一个文 件，它的完整内容都会被保存在 Git 仓库的历史记录中。但对于非常大的文件，这种方式会导致仓库变得庞 大而且低效。而 Git LFS ， 就不会直接将它们的内容存储在仓库中。相反，它存储了一个轻量级的“指针”文   件，它本身非常小，它包含了关于大型文件的信息（如其在服务器上的位置），但不包含文件的实际内容。

当我们需要访问或下载这个大型文件时， Git LFS 会根据这个指针去下载真正的文件内容。所以如果不安装 Git LFS 而直接从 Hugging Face 或其他支持 LFS 的仓库下载大型文件，通常只会下载到一个包含指向实际 文件的指针的小文件，而不是文件本身。

![](img/682edbd1d96806f488896a8344626b85.png)

**.  **Step 3. 开始下载ChatGLM3-6B的模型权重文件

![](img/8d37ca34e991e4882cb2fc2c34ae82f6.png)

**.  ****Step 4. 安装完成后，会在本地生成如**下目录，这个过程会比较慢，因为文件非常大

![](img/fe43cd40f9e06b44e98996ba87ccd77a.png)

**.  ****Step 5. 验证下载文件**

![](img/212a4e46d2a801ff4e553f0db49961be.png)

### 3.2 方式二：从魔搭社区官网下载（国内镜像）

魔搭社区（ModelScope）是一个汇集了各类模态上工业级领先的模型和丰富的数据集的开源平台。它  为泛AI开发者提供灵活、易用、低成本的一站式模型服务产品。在魔搭社区中，开发者可以发现、学习、定 制和分享心仪的模型。社区还发布了各种模型的最新进展、实践、微调、评测等内容，使模型应用变得更加 简单。特别适合于需要大量不同类型模型的AI开发者，它提供了一个共建模型开源社区的环境，让开发者能 够在一个平台上探索体验、推理、训练、部署和应用多种模型。此外，社区上已经承载了来自各个机构贡献 的不同系列的模型，并且社区的开发者也在这些模型的基础上，贡献了许多创新应用。

.  [**S**](https://modelscope.cn/models/ZhipuAI/chatglm3-6b/summary)tep 1. 进入魔搭社区的官网： https://modelscope.cn/models/ZhipuAI/chatglm3-6b/summary

![](img/c03b71543f906b7c3daeb9514aeaba6c.png)

提供了两种下载方式，且提供了明确的代码。建议还是使用git的方式下载。

![](img/e2c066e69ba818678dfdf622c8a423fd.png)

.  Step 2. 初始化 gitlfs工具

Git Large File Storage（ Git LFS）是一种用于处理大文件的工具，在 Hugging Face 下载大模型时，通 常需要安装 Git LFS，主要的原因是： Git 本身并不擅长处理大型文件，因为在 Git 中，每次我们提交一个文 件，它的完整内容都会被保存在 Git 仓库的历史记录中。但对于非常大的文件，这种方式会导致仓库变得庞 大而且低效。而 Git LFS ， 就不会直接将它们的内容存储在仓库中。相反，它存储了一个轻量级的“指针”文   件，它本身非常小，它包含了关于大型文件的信息（如其在服务器上的位置），但不包含文件的实际内容。 当我们需要访问或下载这个大型文件时， Git LFS 会根据这个指针去下载真正的文件内容。所以如果不安装  Git LFS 而直接从 Hugging Face 或其他支持 LFS 的仓库下载大型文件，通常只会下载到一个包含指向实际   文件的指针的小文件，而不是文件本身。

![](img/80525281222e10a0f8e5ed1755111d0e.png)

**.  **Step 3. 开始下载ChatGLM3-6B的模型权重文件

![](img/49ab888b67bf7a77033f82fba7c77cbc.png)

**.  ****Step 4. 安装完成后，会在本地生成如**下目录，这个过程会比较慢，因为文件非常大

![](img/7647f6766abdf74007e0ff75956194e0.png)

**.  ****Step 5. 验证下载文件**

![](img/d0f354559a6ae3a160f06ec15439ef4f.png)

### 3.3 方式三：三种直接下载模型的方式

如果上述两种方式都无法下载ChatGLM3-6B模型的权重文件，可以使用如下三种最简单粗暴的方式，下

载全部文件后，新建 chatglm3-6b文件夹统一存放。

+   [**方**](https://huggingface.co/THUDM/chatglm3-6b)[**式一：从Hu**](https://huggingface.co/THUDM/chatglm3-6b)gging Face直接下载（仍然需要🪜）：https://huggingface.co/THUDM/chatglm3-6b

![](img/bd83026a0afd2c2c6accaa3c5db6e2d5.png)

手动一个一个下载，直至下载完全部文件。

![](img/ddaf6e388746699ffaff0f4ed5c28631.png)

+   [**方式二：从modelscope直接**](https://modelscope.cn/models/ZhipuAI/)下载（不需要🪜）：https://modelscope.cn/models/ZhipuAI/chatglm3-6b/summary

![](img/1eb6096ebb389d8f2eeaccad095dbde1.png)

需要点击具体的文件，进去后才会出现下载链接。

![](img/c21f5c023e17a6750136ffb2e204a83f.png)

点击下载按钮即可下载。

![](img/23ad2f7d7aa923f34298ccbcb3ba1aa7.png)

+   [**方式三：从我们提供的百度网盘中下载：  https://p**](https://pan.baidu.com/s/15sRruaOuzpyY-k_O8i3aLA?p)[**an.baid**](https://pan.baidu.com/s/15sRruaOuzpyY-k_O8i3aLA?pwd=q88n)u.com/s/15sRruaOuzpyY-k_O8i3aLA?pwd=q88n

![](img/34f3e0d274e086ab1a0ab7d1b78e549e.png)

# 4. 启动ChatGLM3-6B模型服务

ChatGLM3还提供了一些简单应用demo，供开发者尝试运行。这里我们由简到难依次对其进行介绍。

## **4.1 基于命令行的交互式对话**

首先是基于命令行的模型调用与对话方法。该应用旨在为非技术用户提供一个脱离代码环境的对话方     式。我们可以通过运行cli_demo.py脚本，来启动该应用。脚本位置同样也在ChatGLM3安装文件主目录下：

![](img/de26055c458758da920b1b27392737f1.png)

使用文件编辑器，编辑 cli_demo.py文件，我这里使用的是Notepad ++。

![](img/e591f1986c60181f053c365be9d6578a.png)

修改完成后，在终端启动该服务。

![](img/5c73eaae7d9486a40fe2ac5d1f225022.png)

可以正常对话，说明服务运行成功。输入stop则可退出交互对话。 如果初次启动出现如下报错：

| ImportError: cannot import name &#x27;COMMON_SAFE_ASCII_CHARACTERS&#x27; from |
| --- |

该报错说明Python 无法从 charset_normalizer 包的 constant 模块中导入名为 COMMON_SAFE_ASCII_CHARACTERS 的项。我们需要执行如下操作：

| pip uninstall charset_normalizer pip install charset_normalize |
| --- |

卸载后重新安装，即可解决该报错问题。

## 4.2 基于 Gradio 的Web端对话应用

接下来则是两个基于网页端的对话应用demo，基于网页端的对话也是目前非常通用的大语言交互方     式，ChatGLM3官方项目组提供了两种Web端对话demo，两个示例应用功能一致，只是采用了不同的Web 框架进行开发。首先先介绍基于 Gradio 的Web端对话应用demo。

Gradio是一个Python库，用于快速创建用于演示机器学习模型的Web界面。开发者可以用几行代码为   模型创建输入和输出接口，用户可以通过这些接口与模型进行交互。这使得非技术用户可以轻松地测试和使 用机器学习模型，比如通过上传图片来测试图像识别模型，或者输入文本来测试自然语言处理模型。  Gradio 非常适合于快速原型设计和模型展示。

基于 Gradio 的Web端对话应用demo运行脚本为web_demo.py，同样也是位于ChatGLM3文件主目录 内：

![](img/c1f00fe23567d5c8b667830b91581c26.png)

使用文件编辑器，编辑 web_demo_gradio.py文件，我这里使用的是Notepad ++。

![](img/128aa52b7157a104bb40d791b6dc487c.png)

然后回到终端，执行启动命令。

![](img/9f6ed31251742a75ccdfcb46702bcd1b.png)

顺利运行之后，即可在http://127.0.0.1:8501/进行对话，当然，也可以通过修改web_demo.py文件， 来修改端口或设置是否允许公开链接进行访问。

需要注意的是，在运行web demo时最好关闭代理工具，否则可能会出现无法正常响应的问题。

## **4**.3 基于 Streamlit 的Web端对话应用

ChatGLM3官方项目组提供的第二个Web对话应用demo，则是一个基于Streamlit的Web应用。

Streamlit是另一个用于创建数据科学和机器学习Web应用的Python库。它强调简单性和快速的开发流程，

让开发者能够通过编写普通的Python脚本来创建互动式Web应用。 Streamlit自动管理UI布局和状态，这样   开发者就可以专注于数据和模型的逻辑。 Streamlit应用通常用于数据分析、可视化、构建探索性数据分析工 具等场景。

我们可以通过运行web_demo_streamlit.py来启动该对话应用。 web_demo_streamlit.py也同样位于 ChatGLM3文件主目录内：

![](img/8c8ade73c439ff28417928670c6edb29.png)

使用文件编辑器，编辑 web_demo_streamlit.py文件，我这里使用的是Notepad ++。

![](img/4c150bc8883eeef80c43f1c70cd13dcc.png)

然后回到终端，执行启动命令。

![](img/44490a7fefb22fb2970ba713d9fe0865.png)

顺利运行之后，即可在http://127.0.0.1:8501/进行对话。在命令行中输入 CTRL+c之后即可停止运行。 需要注意的是，在运行web demo时最好关闭代理工具，否则可能会出现无法正常响应的问题。

## **4.4 使用OpenAI风格API调用ChatGLM**3-6B（重点）

除了上述调用流程外， ChatGLM3-6B模型还提供了OpenAI风格的API调用方法。正如此前所说，在

OpenAI几乎定义了整个前沿AI应用开发标准的当下，提供一个OpenAI风格的API调用方法，毫无疑问可以让 ChatGLM3模型无缝接入OpenAI开发生态。

![](img/796021bebe72a64b96a5e382d5b4b082.png)

所谓的OpenAI风格的API调用，指的是借助OpenAI库中的ChatCompletion函数进行ChatGLM3模型调  用。需要知道的是，该函数原本是OpenAI为调用gpt系列模型而准备的函数。例如一次gpt-3.5的调用过程入 下：

| import os |
| --- |

而现在，我们只需要在model参数上输入chatglm3-6b，即可调用ChatGLM3模型。调用API风格的统 一，无疑也将大幅提高开发效率。具体的执行过程如下：

· Step 1. 首先启动ChatGLM3-6B的API服务

启动API服务需要运行 openai_api.py这个脚本。该脚本位于ChatGLM3安装文件内。

![](img/6875ae26858998754053ec8cb940cb07.png)

使用文件编辑器，编辑 openai_api.py文件，我这里使用的是Notepad ++。

![](img/cca90567817e0d5a5c173688896121ee.png)

然后回到终端，执行启动命令。

![](img/fc6cd377cd715c87145c4680ab0ac5de.png)

顺利运行之后，即可通过代码调用其API服务。

**· **Step 2. 打开可以执行Python代码的编辑器

大家根据个人使用习惯，自行选择Python IDE （集成环境）。我这里使用Jupyter Lab进行演示。

![](img/998e4448505a97feba59f483b16f26d2.png)

Jupyter Lab启动后，会进入默认的工作目录。

![](img/6ad525aca2f721f174ae3feb48f89e8f.png)

**· ****如需修改Anaconda中Jupyterlab**的默认工作目录，请按照如下操作步骤执行：

![](img/8841525bc33b63d664ecd1ff98e5c724.png)

在终端输入 jupyter lab --generate-config 。

![](img/3be7207d73bd4e31aa29aedece9b0b36.png)

找到上述命令生成的这个配置文件。

![](img/68280bdf9b2183708a415a2e0369bbe1.png)

修改工作目录的路径。

![](img/f786cf85e55a4436f819a184aceadec9.png)

这里注意要使用双斜杠（\ \）。

![](img/4e744b1c33ccf5cd0e374450c5f1004b.png)

保存好修改后，再次启动Jupyter Lab即可加载新的工作路径。

![](img/96399f0a9531ebd9a0871327e6d5e461.png)

**· **Step 3. 新建一个NoteBook

![](img/0f33784b46fe984af57998d63d77a408.png)

.  Step 3. 安装openai库

要执行OpenAI风格的API调用，则首先需要安装openai库。 openai库是一个公开的库，安装过程并不需 要使用魔法，国内网络也可进行安装，这里直接使用pip方法进行安装即可：具体执行流程如下：

OpenAI目前已将openai库更新至1.x，但目前Chatglm3-6B仍需要使用旧版本 0.28。所以需要确保当前环境的openai版本。

![](img/49368631c5e6d57f7b48aaa86a72a990.png)

安装完成后，重启一下kernel使其生效。

![](img/f80f6313de7ff6fd3cac53770d56eda1.png)

验证一下openai的包版本。

![](img/63d315d4c2ca7b4b54b0acf5c5bff5b4.png)

**· **Step 4. 调用ChatGLM3-6B模型

输入如下代码：

| import openai |
| --- |

![](img/73074df4a8e4bd876dde2fd9059844bd.png)

**· **Step 5. 关闭API调用服务

若要终止openai_api.py脚本，则需要回到anaconda prompt中，按住 CTRL+c ，则可以终止进程。需要 注意的是，当脚本停止运行之后，就无法在Jupyter Lab中调用OpenAI风格的API了。

至此，本期内容已经详尽地介绍了在Windows操作系统上启动和部署ChatGLM3-6B模型的所有技术细   节。现在，大家可以进一步体验ChatGLM3-6B模型的强大功能，并探索其在大模型应用领域的众多热门应用 开发，带来更丰富的创新可能性和实践经验。

---

🍻现开设了大**模型学习交流群，**扫描下👇码，来遇见更多志同道合的小伙伴~

![](img/7307a4a6a0d06af471d99b13c9b22983.png)

海量硬核独家技术**干货内容**+无门槛**技术交流****+不定期开设****硬核干货&前沿技术公开课****，****扫码**👆即刻入群！