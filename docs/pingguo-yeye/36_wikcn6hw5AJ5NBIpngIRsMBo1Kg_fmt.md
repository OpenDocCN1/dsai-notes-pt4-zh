
# 用Colab部署自己的AI绘画云平台

## 项目介绍

> 来源：[https://e0lrm2v2v1u.feishu.cn/docx/OUZhdGL7Ao7wRKx07jucSlZGnJe](https://e0lrm2v2v1u.feishu.cn/docx/OUZhdGL7Ao7wRKx07jucSlZGnJe)

AI绘画是最近的热门话题，它可以让你用文字或图片生成各种惊艳的作品。但是，如果你没有强大的本地显卡，你可能会遇到运行速度慢、内存不足、模型不兼容等问题。怎么办呢？别担心，今天我就来给你介绍一个云端的一键集成方案。

网址：`https://github.com/camenduru/stable-diffusion-webui-colab`。

这个网址提供了一些常见模型的一键方案，如果没有你喜欢的模型，可以随便挑选一个进入`colab`后根据需要下载。

![](img/68dd2e52d58be086f95fd9f2001439df.png)

其中：

`lite` 具有稳定的 `WebUI` 和稳定的已安装扩展。

`stable` 具有 `ControlNet v1.1`、稳定的 `WebUI` 和稳定的已安装扩展。

`nightly` 具有 `ControlNet v1.1`、最新的 `WebUI` 和 `PyTorch 2.0`，以及每日安装的扩展更新。

选择一个合适的大模型，比如：`Anything5`，我们选择`nightly` 最新版本进入。

![](img/6ae8091660eaa29aac4d16c551396b09.png)

## 部署步骤

接下来，只需要按照以下简单的步骤就可以完成部署了：

1.  进入`colab`界面后，点击`连接`，系统会自动分配资源，可以看到自动分配了RAM12.7，GPU大概15GB，对于跑大部分图已经足够了。注意：Google官方已经发布声明免费版的`colab`已经不适用于`Stable Diffusion WebUI`，所以建议大家购买一个`colab pro`（某宝70元左右/月）。

![](img/3251e7905629f9c2f8004af5595e23bd.png)

1.  分配完资源后，点击左侧的运行按钮，就开始自动部署环境，启动`stable diffusion webui`。

![](img/6c378806ed19d325b8115cc79234f569.png)

1.  部署完成后（一般在10分钟左右即可完成），我们可以在日志输出栏里找到访问地址，注意这里的地址和本地启动地址不一样（本地启动地址一般是`127.0.0.1`）。

![](img/a306a4c8cb75a36a8716754c8732dbe6.png)

然后，你就可以开始你的云端AI绘画之旅了。

![](img/41623381e1ec1c06748f5cc182030f6c.png)

## 功能体验

1.  我们来简单体验一下`txt2img`(文生图)，选择基础模型：`Anything5`，`VAE`模型随便选一个，可以不选。

正面提示词：1girl, blue bow, animal ears, red eyes, long hair, very long hair, long sleeves, solo, bow, hair between eyes, tail, fox tail, bangs, dress, white legwear, white hair

负面提示词：((cowboy)),(((pubic))), ((((`pubic_hair`))))sketch, duplicate, ugly, huge eyes, text, logo, monochrome, worst face, (bad and mutated hands:1.3), (worst quality:2.0), (low quality:2.0), (blurry:2.0), horror, geometry, `bad_prompt`, (bad hands), (missing fingers), multiple limbs, bad anatomy, (interlocked fingers:1.2), Ugly Fingers, (extra digit and hands and fingers and legs and arms:1.4), crown braid, ((2girl)), (deformed fingers:1.2), (long fingers:1.2),succubus wings,horn,succubus horn,succubus hairstyle, (`bad-artist-anime`), `bad-artist`, bad hand

尺寸：512*768

步数：25

其它参数就不选了，看看效果吧。

![](img/0af6c84cdaf1ad6be093360bd9996d56.png)

1.  我们再来体验一下图生图。从刚才文生图的四张中选择一张最满意的，发送到`img2img`，所有参数都不改，直接运行。可以看出来，图生图会根据底图的样式进行重绘，图生图的功能非常强大，搭配`controlNet`可以给线稿上色、控制人物造型等等功能，让你的Ai绘画不再随机出图。

![](img/79ce652847949c2aeb2d669f35500dcf.png)

## 结语

今天的分享就到这里了，[苹果爷爷【AI知识库】首页](https://e0lrm2v2v1u.feishu.cn/wiki/wikcn8SwQrC91pMBXERgUvytBug)后给大家推荐一个AI知识库，飞书地址：  ，你可以在里面找到AI相关的最新资讯、教程、工具、案例、研报和社群，让你快速掌握AI的技能和知识，实现AI的应用和变现。
