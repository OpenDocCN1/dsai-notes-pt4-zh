# 7.手机AI新时代：轻松运行小型模型

> 来源：[https://langgptai.feishu.cn/docx/ARJRdJXueo54WwxMGRMc4NUunse](https://langgptai.feishu.cn/docx/ARJRdJXueo54WwxMGRMc4NUunse)

最近微软发布Phi-3 Mini：性能媲美GPT-3.5，都宣传说可以塞进手机。那到底如何塞进手机里面呢？

近期作者研究一系列的各种方案，踩各种坑，最终研究出小白都能将phi-3、llama3-8B等模型轻松运行在手机中。

人的注意力只有十分钟，十分钟内实操完成，废话不多说，我们就开始实践操作整个过程。

温馨提示：整个操作过程尽量使用**魔法，**下载速度会快很多。

### AnLinux 安装

什么是AnLinux？

AnLinux就像是一个魔法盒子，它能让你的安卓手机或平板电脑变身为一台小型的Linux电脑。AnLinux就是一个让安卓设备变得更加多才多艺的工具，可以随身携带的多功能电脑（模型）。

有条件自行下载termux，软件地址https://github.com/EXALAB/AnLinux-App，也可以选择以下安装包自行安装，以下安装包是经过验证可行。

![](img/c3e66eefd2eb56a98591d9f3606faee6.png)

[app-release.apk](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/WI6TbuD5ooYQe7xLDrUcAwuJnke)

### termux安装

什么是termux？

termux 是一款基于 Android 平台的开源 终端模拟器，使用进行软件包的管理。可以使用手机操作相关命令，使得能正常跑通大模型。

有条件自行下载termux，软件地址 https://termux.dev/en/ ，也可以选择以下安装包自行安装，以下安装包是经过验证可行。

![](img/1a5841c170dbaac5dc52dd468fca44f8.png)

[termux-app_v0.118.0+github-debug_arm64-v8a.apk](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/ZcsMbCy8Lo1VQvxFWHDco9NLnGh)

## 安装系统过程

1.  自行termux与Anlinux安装完成，同时给必要的权限。

1.  打开termux软件，执行  pkg update && pkg upgrade 命令，安装过程中需要enter键确认。

![](img/6dad2d4a4a4692512da9c50085d7736b.png)

1.  安装wget 命令 pkg install wget

1.  打开Anlinux软件，准备安装操作系统环境。同时复制下载操作系统命令。

![](img/75481c6076331e96f515036f917a3892.png)

![](img/530e9292037faa20454c872d2e608804.png)

![](img/8b3f2e7e37c66993eb237282155470f2.png)

![](img/b9e4fffff3533f648f33247c7484bb49.png)

1.  AnLinux 复制下来的安装命令，termux软件中执行。（温馨提示：安装过程中需要enter键确认）

1.  termux软件中执行命令 ./start-ubuntu.sh

1.  启动完成后如下图所示，有 root@localhost:~#    说明启动成功。

![](img/cf9055f6c98c53ce63c9ed15f9db7565.png)

### 命令集合

```
pkg update &amp;&amp; pkg upgrade
pkg install wget
AnLinux软件 复制命令到termux软件执行下载
./start-ubuntu.sh
```

## ollama

### Ollama是什么？

Ollama 是一个开源框架，专为在本地机器上便捷部署和运行大型语言模型（LLM）而设计。 以下是其主要特点和功能概述： 简化部署大型语言模型的过程，使得非专业用户也能方便地管理和运行复杂的模型。

官方网站：https://ollama.com/

### 避坑操作

以下操作都在termux软件进行操作

1.  termux软件中执行命令 ** ****apt-get update**

1.  安装curl命令，apt install curl -y

1.  安装ollama命令，**curl -fsSL ****https**://ollama.com/install.sh | sh

![](img/612f04961717d2b403806cbdcbb53112.png)

1.  图一 ollama安装过程； 图二 ollama安装完成

![](img/575685a5637e2c228d4555bf2bc1c266.png)

![](img/69bdc547de2a27aadddbd83c930ad0d1.png)

1.  termux软件中执行命令   **ollama serve**

![](img/2904a2397c26bd6becb0b88f2ef4aa6d.png)

1.  后台运行termux软件（正常返回到手机桌面即可）

1.  **返回桌面长按termux**软件图标，选择 New session，重新启动一个新termux连接

![](img/2763764771f472e6e590e7d388f76e29.png)

1.  新termux连接中执行命令 ./start-ubuntu.sh

![](img/0ce4210e0b5220d44667e19cc4f2ff06.png)

#### 命令集合

```
 apt-get update
 apt install curl -y
 curl -fsSL https://ollama.com/install.sh | sh
 ollama serve
 ./start-ubuntu.sh
```

### 启动模型

我使用phi3模型来做演示

1.  termux软件中执行命令 **ollama run phi3**

1.  等待下载完成phi3模型，请耐心等待

1.  启动正常，可以正常使用**phi3模型**

![](img/da87c82a301625c4ac5800da172bf862.png)

#### 命令集合

```
ollama run phi3
```

当然ollama 还有很多其他模型，如有需要请ollama 官网自行查看命令与下载。

## 简单结果输出

问题一：你相信AGI的到来吗？

![](img/0755fde459efd3fefd08162c7d83bc6a.png)

问题二：使用python写出冒泡代码。使用最简洁写出来。

![](img/c2dfe837de6a02ad8fd8a64392813500.png)

作者个人简介，如有问题可以直接连接作者。或者社区里面直接联系盘盘，也是可以。

[Welcome to P. | 盘盘个人杂志](https://g10npwhy4nv.feishu.cn/docx/U1Iqduc3mox5F8xmbUzcCC6qnof)