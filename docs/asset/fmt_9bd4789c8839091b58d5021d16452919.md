
# LCM 开启AI实时绘画时代

> 来源：[https://xuqiwei1986.feishu.cn/docx/LXE6dOMFIoVRuBxXqxocqkm2nyb](https://xuqiwei1986.feishu.cn/docx/LXE6dOMFIoVRuBxXqxocqkm2nyb)

没想到这么快 AI 实时绘画就来了，上一期我们讲了 LCM 模型，可以让我们的图片生成速度最高提升 10 倍，没想到这么快 LCM 模型就有了新的应用场景了，没错，就是实时绘画。

想要实现实时绘画，需要 Krita+ComfyUI+LCM 结合使用。ComfyUI 不用听雨介绍了吧，LCM 模型也刚介绍过。Krita 简单介绍一下，Krita 是一款开源的绘画工具，我们可以把它当成 Photoshop 的免费平替。

我们就来看看具体如何操作。

## 下载与安装 Krita

首先需要下载 Krita，免安装包听雨已经放在网盘中了，后续要用到的插件听雨也会放在文末的网盘中。

解压安装包以后，进入解压的目录下，点击 `krita.exe` 就可以打开 Krita 了。

> `\krita-x64-5.2.1\bin`

![](img/3f49b1f809459d59647c7f4bbeed25c0.png)

正常打开 Krita 以后，就可以在看到以下目录了，把网盘中的插件解压拷贝到以下文件夹下。然后我们重启 Krita。

> `C:\Users\<user>\AppData\Roaming\krita\pykrita`

![](img/6695eea55701d10f7468478cb1b2be59.png)

## 启用 AI 插件

在 Krita 中启用插件（设置 ‣ 配置 Krita ‣ Python 插件管理器），勾选「AI Image Diffusion」，点击确定并重启 Krita。

![](img/09657332e35f1a29c98bd352fc374923.png)

然后新建一个自定义图像。

![](img/1e260743a9cad73ba0313cdb680072f6.png)

继续点击设置 ‣ 面板列表 ‣ AI Image Generation，勾选。可以看到右下角多出了一个「Configure」按钮。点击该按钮。

![](img/f87eadce9117ae4c26eab21082869ebd.png)

## 连接 ComfyUI

先不管弹出来的配置界面，我们先把本地的 ComfyUI 跑起来。然后继续回到 Krita 中，点击「Connect」，连接 ComfyUI 会显示我们缺少的东西。

![](img/47a8caf020a8491cc1229a0a824f75eb.png)

官网提供了需要安装的插件和模型，小伙伴们按照官方的需求安装。

> [https://github.com/Acly/krita-ai-diffusion/blob/main/doc/comfy-requirements.md](https://github.com/Acly/krita-ai-diffusion/blob/main/doc/comfy-requirements.md)

相关的插件和模型，听雨也都会放在网盘中，小伙伴们可以自取。

![](img/31ef5fed4c15a10959fc97588e34321e.png)

以上的插件和模型都安装好以后，然后我们继续回来点击「Connect」。出现以下的绿色字体就说明是连接成功了。

![](img/4c4bcd769046ef828292ea3cf0f23e60.png)

## 实时绘画操作

然后，我们进入「Styles」中选择大模型和 Lora 模型，Lora 模型需要选择 LCM 专属的 lora 模型。

![](img/bdf9e57e88e55ec73cd3c104bd30ea8d.png)

新建图像时，小伙伴们修改下宽和高哦，当然，如果小伙伴们的电脑配置够高那也可以使用默认的。

![](img/00ec94c84d6f756a4c3fa8b8428066d7.png)

创建图像以后，可以看到右边框中 A B C D 四个功能区域。

A 功能区域，可以选择三个功能，默认是最基本的生成图像功能「Generate」，第二个是放大图像功能「Upscale」，第三个功能是就是实时绘画功能啦「Live」。今天我们就来尝试第三个功能「Live」。

B 功能区域，主要我们在「Styles」中设置的模型配置信息，我们可以设置多个，这边可以选择。

C 功能区域和 B 对应，可以设置 「Styles」。

D 就是我们写提示词的地方了。

![](img/a29d4c6afef6efd9ed2f765f86bda266.png)

如果想要反向提示词框的小伙伴可以将以下的配置勾选。不过实时绘画是没有反向提示词框的，可以统一写作「Styles」配置中。

![](img/883151b195a13c38ac37330baee36888.png)

接下来操作，图文可能不太看得出效果，我们直接上视频哈。

以下就是是实时绘画功能了，听雨的电脑配置不太行哈，没能做到真正的实时渲染，小伙伴们要是配置高的，那就相当丝滑啦！

![](img/a71a54743187ee441ba0b116c7de61b4.png)

[🐧录屏20231123220927.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/HTsqbbP43oZMTHxBct1cv5M1nCg)

## 结合 ControlNet

实时绘画功能还可以结合 ControlNet 来绘图，不过这里只支持 SD1.5 的模型，听雨这边就选择 pose 来给小伙伴们演示了，其他的 ControlNet 功能，小伙伴们可以自行尝试哦！

![](img/82c74d1aeb74fd1d25a4800e008a6726.png)

[🐧录屏20231123223701.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/DhcUb5EF1oNjQzxMySicMmQanTg)

## 参考资源

想要了解 Krita 更多用法的小伙伴可以看官方教程哦：

> https://docs.krita.org/zh_CN/reference_manual.html

网盘链接：[夸克网盘分享](https://pan.quark.cn/s/491fc1c602a8)

好了，今天的分享就到这里，感兴趣的小伙伴快去试试吧！
