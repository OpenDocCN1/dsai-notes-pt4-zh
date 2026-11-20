
# 小白一文秒懂的 Stable Diffusion 安装教程

> 来源：[https://agijuejin.feishu.cn/docx/P28fdhdq0oeOQqxhf6RcXJEcnae](https://agijuejin.feishu.cn/docx/P28fdhdq0oeOQqxhf6RcXJEcnae)

> 作者：德里克文

> 原文：[https://zhuanlan.zhihu.com/p/643656433](https://zhuanlan.zhihu.com/p/643656433)

关于现在非常红火的AI绘画，很多感兴趣的人不知道如何入手，如果你的电脑配置足够好，那么不要犹豫，让我来教你如何在本地电脑全免费运行当下最强大的AI绘画工具——Stable Diffusion 吧！

![](img/aacd4d2a906a2e4c9f4af3fd0f9b4ab2.png)

## 一、Stable Diffusion 是什么

### 1.官方网站

传送门：`https://link.zhihu.com/?target=https%3A//stablediffusionweb.com/`

[  ](https://link.zhihu.com/?target=https%3A//stablediffusionweb.com/)

### 2.知识科普

![](img/8a40a3823f99085bbcd20152191e3eef.png)

Stable Diffusion 简介

Stable Diffusion是一种基于深度学习的文本到图像生成模型，于2022年发布。它能够根据文本描述生成详细的图像，同时也可以应用于其他任务，例如图生图，生成简短视频等。

Stable Diffusion是一种潜在扩散模型，由慕尼黑大学的CompVis研究团体开发。该模型由初创公司Stability Al，CompVis与Runway合作开发，同时得到了EleutherAl和LAION的支持。

Stable Diffusion的代码和模型权重已经公开发布，并且可以在大多数配备有适度GPU的电脑硬件上运行。相比之下，之前的专有文生图模型（如DALL-E和Midjourney）只能通过云计算服务访问。

![](img/bd141db5cf3e09eef4152a9bcfffe6b1.png)

Stable Diffusion Web UI 简介

Stable Diffusion Web UI是一个基于Web的用户界面，用于使用Stable Diffusion进行图像生成和其他任务。该Web UI由AUTOMATIC1111开发，并开源上传至Github，也是目前使用最多的WebUI版本。

Stable Diffusion Web UI提供了易于使用的图形界面，可以帮助用户更直观地了解和使用Stable Diffusion的功能，并在基本不需要编写代码的情况下启动和监视训练过程。

对于对这些不感兴趣的朋友们可以直接跳过这些专业术语，对编程和算法感兴趣的朋友们可以自行搜索了解更多关于Stable Diffusion的专业信息。

![](img/c92272fd4f8cb9bb6abe3a61ce62e91e.png)

### 3.Stable Diffusion 的主要优点：

(1) 免费开源

与另外一个主流的AI绘画软件Midjourney相比，它是免费开源的。Midjourney需要登陆Discard上进行使用（国内无法直接链接注册），而且需要付费才能继续使用，最低价格为10美元/月，而SD在B站上有人整理好的免费安装包，无需付费即可下载一键安装。将SD安装到本地后，用户可以随时使用，生成的图片只有自己可以看到，保密性更高。

![](img/1310a8c3b4c41057d2fb0af9c5354378.png)

(2) SD拥有强大的开源模型和插件

由于其开源属性，SD拥有许多免费的高质量外接预训练模型和插件。例如，可以提取物体轮廓、人体姿势骨架、图像深度信息的插件`Controlnet`可以让用户在绘画过程中精确控制人物的动作姿势、手势和画面构图等细节；插件`Mov2Mov`可以将真实视频进行风格化转换。

此外，SD还具备`Inpainting`和`Outpainting`功能，可以智能地对图像进行局部修改和扩展，而这些功能目前Midjourney无法实现。

![](img/6feb950bd16d92091c71e5c7a589732e.png)

## 二、本地安装教程

### 1. 本地部署特点

本地部署 Stable Difusion 具有更强的可扩展性，用户可以自定义安装所需的模型和插件，从而提高系统的适应性。此外，本地部署方案还能够提供更高的隐私性和安全性，并且用户可以享受更大的自由度。最重要的是，这一解决方案完全免费。

它也存在一些缺点，那就是对本机硬件的要求较高，Windows 用户需要配备 NVIDIA 显卡，并且至少需要8G的显存和16G的内存才能正常运行。而对于 Mac 用户，系统的运行需要 M1/M2 芯片的支持。

![](img/ad130faa1c851f348242b77f40d9c8f9.png)

### 2.电脑配置要求

电脑配置最核心的关键点: 看显卡、看内存、看硬盘、看 CPU。其中最重要的是看显卡。N 卡(英伟达 Nvida 独立显卡)首选，效率远超集显/AMD/Intel 显卡和 CPU 渲染，最低 10 系起步，体验感佳用 40 系，显存最低 4G，6G及格，上不封顶;内存最低 8G，16G 及格，上不封顶:

硬盘可用空间最好有个 500G 朝上，固态最佳系统要求: 支持 Win10/Win11/macOS (仅限 Apple Silicon, Intel 版本的 Mac 无法调用 Radeon 显卡) 和 Linux系统，苹果版 SD 兼容的插件数量较少，功能性不及 Windows 与 Linux 电脑。

![](img/01e6ac068ad6c2aeef04f689ebb865b4.png)

从图中可看出，与 AMD 或英特尔的任何产品相比，Nvidia 的 GPU 提供了卓越的性能--有时是以巨大的优势。随着 `Torch` 的 `DLL` 修复到位，RTX 4090 的性能比带有 `xformers` 的 RTX 3090 T 高出 50%，而没有 `xformers` 的性能则高出 43%。生成每张图片只需要三秒多。

如果身边没有合适的电脑可以考虑购买云主机，比如阿里云、腾讯云、青椒云等等云服务器。若无法使用独立显卡和云服务，亦可修改启动配置，使用 CPU 渲染 (兼容性强，出图速度慢，需要 16G 以上内存)

[视频讲解（可以参考B站的内容）：【入门01】5分钟搞定Stable Diffusion环境配置，消灭奇怪的报错](https://www.bilibili.com/video/BV1k54y1T7Lf/?spm_id_from=333.788&vd_source=05604b6063f398633af513a2e9a7c804)

[  ](https://link.zhihu.com/?target=https%3A//www.bilibili.com/video/BV1k54y1T7Lf/%3Fspm_id_from%3D333.788%26vd_source%3D05604b6063f398633af513a2e9a7c804)

### 3.具体安装教程

### Win 版本

1.我分享的云盘链接：`https://pan.baidu.com/share/init?surl=jPqx2k5L072cda8f4zLQDQ` 提取码：xoz1

Nvidia 显卡用户须知:

在使用 SD 前，请登录 Nvidia 官网下载安装对应显卡最新版驱动程序，链接：`https://www.nvidia.cn/geforce/drivers`

对应本地化安装讲解视频：`https://www.bilibili.com/video/BV1E24y1K75K/`

特别注意: 下载的文件一定要放在空间比较大的盘里。

### Mac 版本

DifusionBee 提供了一个基于 stable diffusion 的应用程序，无需复杂部署，正常安装就可以使用。完全免费

官网地址：`https://diffusionbee.com/`

安装及使用流程参考:

硬件要求参考:

1、建议: 8GB M1 MacBook Air 以上

2、`DiffusionBee` 适用于基于 Intel 的机器。如果你没有专用的图形芯片，它与 M1 机器相比非常

3、`DiffusionBee` 需要更高版本的 macOS 12.5.1，推荐使用 Apple 芯片的机器

![](img/74a7d4c934ce27a7b4cb70267eeb705a.png)

## 三、云端部署

### 1.飞浆(PaddlePaddle)

飞浆是以百度多年的深度学习技术研究和业务应用为基础，集深度学习核心训练和推理框架、基础模型库、端到端开发套件、主富的工具组件于一体，是中国首个自主研发、功能丰富、开源开放的产业级深度学习平台。

飞浆上租用运行环境需要算力支持，GPU最好的V100四卡需要8点算力/小时。正常运行建议可以考虑1点、4点算力一小时即可。`https://aistudio.baidu.com/aistudio/projectdetail/6537485`

![](img/b9249c6e6594eafc469d0fc4b811eb59.png)

### 2. 阿里云

建议参考B站教程：`https://www.bilibili.com/video/BV19u411x7Bk/?spm_id_from=333.999.0.0&vd_source=05604b6063f398633af513a2e9a7c804`

### 3. 腾讯云

可以点击以下链接进行预约`https://cloud.tencent.com/act/redirect?from=10680&page=gpu-study`

### 4.Colab云

`https://www.bilibili.com/video/BV15j411c7fE/?spm_id_from=333.337.search-card.all.click&vd_source=05604b6063f398633af513a2e9a7c804`

### 5.青椒云

这个是青椒云的官网：`https://www.qingjiaocloud.com/pricing/`

然后这个是他们的使用手册：`https://qingjiaocloud.oss-cn-hangzhou.aliyuncs.com/doc/%E9%9D%92%E6%A4%92%E4%BA%91%E4%BD%BF%E7%94%A8%E6%89%8B%E5%86%8Cv2.0.1.pdf`

---

我是德里克文，一个对AI绘画，人工智能有强烈兴趣，从业多年的室内设计师！如果对我的文章内容感兴趣，请帮忙关注点赞收藏，谢谢！
