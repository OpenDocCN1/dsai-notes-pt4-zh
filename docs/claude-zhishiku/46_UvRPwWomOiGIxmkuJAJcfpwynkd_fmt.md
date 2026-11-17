
# Live2D 扩展教程

> 来源：https://sqivg8d05rm.feishu.cn/docx/Z9wednwzGoZbeMxihfxceUyAnYb

## 前言

此扩展允许您为角色使用 Live2D 动画模型，为您的虚拟角色提供动态和互动元素。

Sxma 的部署前配置：国外 VPS 部署的 Clewd 和 SillyTavern

> **注意！！！**
> 此步骤需要酒馆是最新版且魔法畅通。VPS 部署的在酒馆目录下输入 `git pull` 更新，`bash start.sh` 重启。

[要从此](https://git-scm.com/downloads)页面下载扩展，您需要安装 Git。VPS 部署的人已经安装了 Git。

## 一、DownloadExtensions&Assets 下载扩展和资源

![](img/389f622e4ff5773798f79f1ca559352d.png)

注意：需要再次刷新网页重新加载酒馆使得 Live2D 扩展出现。

## 二、下载 Live2D

欢迎后来者更新更好的下载办法和下载地址。

### 1、快捷下载

[为了方便](https://www.bilibili.com/video/BV1u14y1f7ik/?t=4&spm_id_from=333.1350.jump_directly&vd_source=b321576eba39aedaf1743088883e6c6b)[测试也可以下载这](https://www.bilibili.com/video/BV1u14y1f7ik/?t=4&spm_id_from=333.1350.jump_directly&vd_source=b321576eba39aedaf1743088883e6c6b)个，此文件在网上搜 Live2D 找到的。【免费Live2D模型】小魔女一键出道+自带直播UI+舰长礼物稿哔哩哔哩bilibili

![](img/ccff61ee7ff60b3184a1adb876fe54ac.png)

![](img/142d35a55afea9995b225b43fb379251.png)

（前面目录写错，现已修改）

将此目录放入酒馆下的：

`SillyTavern\data\default-user\assets\live2d`

旧版：`/public/assets/live2d`

看到这儿就可以直接跳往 三、使用 了。此处往后是添加 Live2D 下载地址。

### 2、GitHub 上有很多 Live2D 模型

链接：https://github.com/Eikanya/Live2d-model，网上肯定很多，这儿只是一处。

部分下载

![](img/d63783268b5ea41ee880cdf553c96a1c.png)

在登录了 GitHub 账号的 GitHub 网页里，单机顶部 `gitzip`，Nomal,跳转后授权：

![](img/2cfd776ffd0a33599898d15e35c8d4d9.png)

![](img/341b70910a4aa2203962ef8ebc3f9c45.png)

右键，选择 `gitzip` 即可下载，但我这儿未知原因失败了，有谁知道是什么原因吗，还请私信群里 Sxma，感谢🥰

全部下载（体积大）

![](img/034494e2812da66420164617d99784d8.png)

## 三、使用

### 1、将模型放入目录

`/public/assets/live2d`

### 2、老婆模式

打开某个角色卡的聊天界面，打开老婆模式以显示模型，后面可以自行拖动到任意位置。

![](img/332aa66c0e9b99d8ebbd8d4a63903651.png)

### 3、选择使用

![](img/cad0d91df76d765ef1f9620497fd552f.png)

进入 Gallery 后可以快速选择并切换不同的 Live2D 模型。

![](img/deb64cd69e2c474fd4d7fa9027d3b6c6.png)

选定模型之后往下滑还可以指定模型在不同状态下的动作。

![](img/5c8904d28d8786d12c675c1c5a016698.png)

目前缺接入声音的教程，欢迎私信群里 Sxma 指教。
