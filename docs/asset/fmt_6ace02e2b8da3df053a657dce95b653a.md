
# 暴打SD MJ 的Flux（附安装教程及国行免费使用网址）

> 来源：[https://bluefocus.feishu.cn/docx/T3H4dzkUBodF0Dxknk3cC63Kn2b](https://bluefocus.feishu.cn/docx/T3H4dzkUBodF0Dxknk3cC63Kn2b)

今天带大家了解**文生图赛道黑马——FLUX**

![](img/78b76e7166f1ff7ab7c3dd8df85428d3.png)

## FLUX简介

最近 Flux 爆火，人物写真更加逼真、细腻，这些图片你能看出来是 AI 生成的吗？

![](img/5edae25d5bec914550ed00b973d22591.png)

甚至还能精准控制生成的文字，比如在手上和纸上写：I am not real.

![](img/84d25cee7f592eba384ef6254a6edda2.png)

![](img/a3b517a7e3ca83aae014c241b4ccc8ef.png)

无论是单人还是多人，脸部打光、肌肤纹理、毛发，都很难看出破绽。

![](img/f12f8a73fc3b325c8da23b5b17fd6894.png)

而这样的逼真图片，使用 Flux 就可以轻松生成：

![](img/110609016ee6a4532e9632247172cb0d.png)

Black Forest Labs由Stable Diffusion 原班人马于2024年8月1日创立，专注于生成式图像AI和视频AI。该公司推出了其首个产品FLUX图像模型，还计划近期推出视频生成模型。

FLUX.1是一个开源的AI图像生成模型，拥有12B参数，是迄今为止参数最多的文本到图像（文生图）模型之一。该模型基于多模态和Transformer架构，能够生成高质量、细节丰富、风格多样的图像。

FLUX.1系列包括三种不同的版本：专业版（pro）、开发者版（dev）和快速版（schnell），分别适用于不同的使用场景和需求。

+   Flux.1 [pro]：这是Flux.1的顶级版本，提供最先进的图像生成性能，但是模型不开源；
+   Flux.1 [dev]：开源的模型，但是不能商用；
+   Flux.1 [schnell]：这是Flux.1模型家族中最快的模型，专为本地开发和个人使用优化。

## FLUX玩法

FLUX已经被玩疯了。自8月初发布以来，FLUX可以说是已经被网友们玩出花了。

### 1. FLUX+图生视频

其中最流行的玩法当然还是用FLUX生图，然后用Runway Gen-3、Kling AI、Luma Dream Machine等AI视频工具转成视频。

举牌求关注视频、自拍视角的Vlog、对镜拍视频、品牌高级感广告大片统统手到擒来。

这光影、这构图，还有模特的妆容和表现力，高级感拉满：

![](img/037cd61d507bfebef247723f32ed0703.png)

还有网友给视频进行了口型同步，这下AI YouTuber的春天怕不是要来了：

### 2. FLUX+局部重绘

还有人把FLUX和局部重绘相结合，只见他用笔刷选中怀里的金毛，然后输入提示词“小狮子”，金毛立马摇身一变成了小狮子，而且融合度非常高，手部细节也没有出错。

这小狮子一抱，连带着这个哥们的气质都变了，瞬间有一种迪拜富豪即视感。

![](img/0f681833acddd763932c72ce55438337.png)

感兴趣的玩家可以在HuggingFace()上在线体验：

https://huggingface.co/spaces/SkalskiP/FLUX.1-inpaint

![](img/497c3aa7144e03ca8f7d508e933e891c.png)

### 3. FLUX+Magnific AI

X网友“Javi Lopez”用AI图像增强软件Magnific对FLUX生成的图片进行了高清和放大处理，照片的真实感瞬间next level。

![](img/add2f29cfce7e84e473715a391eae623.png)

比如这张王室合照，在经过Magnific优化后，无论是皮肤纹理，还是发丝和光影效果都可谓与真人无异，人物神情也更为细腻。

![](img/860a6b99839adb60c76be7cdc7fda8e1.png)

当然，不止人像，插画、风景、食物、建筑等都可以用Magnific增强。

![](img/d98444b24faa62b6868e83728253276a.png)

不过，Magnific AI现在开始收费了，而且还不便宜，每月39美元至299美元不等，不支持退款。

![](img/855b94ee9684cd625975fe1b381facc7.png)

## 国内免费体验教程

**目前**国内硅基流动SiliconCloud 平台已经可以免费使用开发者版（dev）和快速版（schnell）！

清风试了一下，效果非常好！视觉质量、图像细节和输出多样性等方面性能优越，输出极佳的图片生成效果。

光线不如 MJ 梦幻，偏写实一些

无论是人物、场景、产品特写，都非常接近真实摄像。感觉以后广告公司都不用真实摄影了！

**1.登录**

国内直联，一键体验传送门

（点击本文左下角“阅读原文”直达）：

https://cloud.siliconflow.cn?referrer=clwwz14f1000xjg07ik1kl0pm

![](img/fcca7564309ea957b57c894fb5b45afb.png)

**使用上述网址的新用户完成账号注册**，立刻免费获得2000万Tokens！

使用国内邮箱，如🐧邮箱，即可完成注册

![](img/0323c40bd94509fb02ef5b76b1c8c2cb.png)

**2.界面**

登录后界面如下，左侧选择文生图，右侧下拉框中选择Flux两个模型，第一个效果好，第二个速度快

其实第一个效果好的也是10秒成图

![](img/604114bba3f0f90b2add8023e99241fe.png)

**3.参数**

图片尺寸、推理步骤、种子；提示词最好用英文，中文支持不是太好

![](img/842868249cf5a8ad048b5d43a1ee8eb1.png)

**4.案例**

提示词：A Chinese little boy is leaning over the desk doing his homework. photography. Professional color grading with soft shadows, professional studio lighting

![](img/9d03c8cdc3cb5595467c055480b8e60d.png)

提示词：Gothic Neon' style of a witch with long purple hair and tattoos, merging the Gothic era's dark, romantic vibes with bright, neon modernity, in deep purple and vibrant hot pink

![](img/0539c9309d939f94d5c776b7b8d89ab5.png)

**试试场景：**

提示词：photography。A vibrant Parisian scene at the 2024 Olympic Games, capturing the essence of the French capital's rich history, culture, and modernity. Show the athletes in their respective sports, with a focus on the iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Seine River. Highlight the diverse crowd, including locals, tourists, and international athletes, all united in their support for the games

![](img/99f9383f2998bc0da28dec9c53284ed2.png)

提示词：A fairyland-like classical Chinese palace scene in the clouds, giving a person a dreamy feeling of being detached from the world. A character dressed in traditional light blue attire stands in the foreground, gazing at the magnificent buildings. These buildings have a traditional style, with multi-layered eaves that are upturned and decorated with intricate golden details that sparkle in the light. The palace seems to rise among the clouds, connected by a series of steps and paths, seemingly pointing towards an extraordinary place, possibly the heavenly realm. The use of color is soft and vivid, with sunlight filtering through the mist, creating a tranquil and mysterious atmosphere.

![](img/ceaa5d8c84a1bd44dac5dc91e0a2cb8e.png)

**看看艺术摄影**

提示词：Create an image of a young woman in a serene natural setting, reminiscent of a classical Chinese landscape. She is seated by a calm river, wearing a flowing white dress with a sheer shawl draped over her shoulders. Her hair is elegantly styled, and she is adorned with delicate jewelry, holding an ornate book, suggesting a moment of peaceful reading. Alongside her, there is a modern laptop displaying a dynamic stock market chart, juxtaposing the tranquility of nature with the bustling world of finance. In the background, traditional Chinese boats navigate the gentle river, while majestic karst mountains rise in the distance, enveloped in a soft mist, illustrating a blend of tradition and modernity

![](img/f71616de3d0062b302ede5fe160bc290.png)

提示词：Create an image of a young woman in a serene natural setting, reminiscent of a classical Chinese landscape. She is seated by a calm river, wearing a flowing white dress with a sheer shawl draped over her shoulders. Her hair is elegantly styled, and she is adorned with delicate jewelry, holding an ornate book, suggesting a moment of peaceful reading. Alongside her, there is a modern laptop displaying a dynamic stock market chart, juxtaposing the tranquility of nature with the bustling world of finance. In the background, traditional Chinese boats navigate the gentle river, while majestic karst mountains rise in the distance, enveloped in a soft mist, illustrating a blend of tradition and modernity

![](img/f924f574f89d9a2bf27dcd0db786a074.png)

**再试试产品摄影**

提示词：Product Photography, Razer headset, Close-Up angle, with a festive wrapping paper background, emphasizing the surprise element and colorful designs, Glowing Light from a nearby Christmas lantern

![](img/99a8d55361d57f1a8eac267104c90ebe.png)

提示词：Product Photography, Rolex watch, Close-Up angle, with a festive wrapping paper background, emphasizing the surprise element and colorful designs, Glowing Light from a nearby Christmas

![](img/e0ca6511cafccb0c00a57c32ea56e36e.png)

提示词：cosmetics_mockup,cosmetic tube mockup,the mockup has the text ‘SiliconFlow’,nohumans,redbackground, minimalistic space aesthetic,bubble,flower,gradient background,still life,gradient,photorealistic

![](img/aec2e1e9afdb830dd4d1c62f2aaeda5e.png)

## 正式 Flux 初体验

目前 ComfyUI() 已经集成了 Flux，可以在 ComfyUI 上部署体验。

所以今天教学分为两个部分，ComfyUI 安装和 Flux 安装。

### 1、ComfyUI 的安装

ComfyUI 的安装可以说是非常简单了，打开 releases 地址：

https://github.com/comfyanonymous/ComfyUI/releases

就能看到最新的启动包，下载好这个。

![](img/485f1865c4505c6c9f5c13f31f53568a.png)
