# MacOS系统(M1/M2)安装StableDiffusion保姆级教程

> 来源：[https://xmgtoagi.feishu.cn/docx/ZQm1deCj0ofi1uxpMKpcdFPxnMb](https://xmgtoagi.feishu.cn/docx/ZQm1deCj0ofi1uxpMKpcdFPxnMb)

**实操环境：**

macOS 13 Arm64（建议12以上的系统使用）

Apple M1

先来看几个样例：

![](img/41971c73d58e73742d7475bda4e29067.png)

废话不多说，直接上教程

# HomeBrew安装检测

如果没有安装HomeBrew，在终端运行如下命令安装：

在终端命令行窗口输入安装命令。

这里需要特别说明几点内容。

下面是Homebrew官方给出的安装命令：（如果没有VPN，不要使用此命令安装！）

```
/bin/bash -c &quot;$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)&quot;
```

通常情况下，官网给出的指令会报错：

```
curl: (7) Failed to connect to raw.githubusercontent.com port 443: Connection refused
```

因为这是国外网站，由于GFW（中国长城防火墙）的存在，如果没有vpn，是无法访问的，所以连接被拒绝！

**别着急，有办法解决！**

只要我们从国内镜像网站安装，就一切OK了。

以下为国内安装Homebrew的正确姿势：(基于gitee上某大神的自动安装脚本)

```
/bin/zsh -c &quot;$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)&quot;
```

回车执行指令后，根据提示操作。具体包括以下提示操作：

#### 1.选择下载镜像

根据需要选择下载源，例如，我这里选择中科大下载源，就输入‘1’，回车。

![](img/058131fe18632799d0d67659a5d3e77c.png)

#### 2.确认删除旧版本

如果存在旧版本，会弹出删除旧版本提示，输入"Y"，回车。

![](img/c47d10fd0bd31cd7aff9441c27b6c6d3.png)

#### 3.输入开机密码（用于mac确认第三方应用安装）

#### 4.回车跳过即可

![](img/0a0947fe6a3bb807fb7872d3dcb92767.png)

#### 5.安装成功配置以后的镜像源

![](img/b800f1dd2e8cc3fffdae0079e9460007.png)

#### 6.重启终端或者运行source /Users/xixi/.zprofile  让brew生效

![](img/60fa928b0902d3eafcc3bdda4f694bb2.png)

# Git安装检测

如果没有安装Git需要安装

输入如下命令：

brew install git

# 软件安装：

网盘下载：我用夸克网盘分享了「AITools(爱兔)」，点击链接即可保存。打开「夸克APP」

链接：https://pan.quark.cn/s/7d93e96740e5

![](img/e157ef6155eb52ebf97c51c6e3e3c92c.png)

安装后打开：

![](img/d0f7b68a30b63822c5e4f2a5f1213250.png)

![](img/18cc4d38523e81df4bc7df1d6d29b40e.png)

首次启动会安装各种所需要的依赖库，时间比较久，请耐心等待。

![](img/d3607348a81a85e141236e8496193360.png)

最后启动成功：

![](img/378a15353be99ba238415f73aca6dc2f.png)