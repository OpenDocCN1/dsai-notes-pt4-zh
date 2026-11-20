
# Stable Diffusion 3发布，还有什么人工智能应用方面，值得注意的信息

> 来源：[https://agijuejin.feishu.cn/docx/UPBHdXtyIo9o3Uxi3otcpy6rnMg](https://agijuejin.feishu.cn/docx/UPBHdXtyIo9o3Uxi3otcpy6rnMg)
>
> 作者：德里克文
>
> 原文：[https://zhuanlan.zhihu.com/p/683606248](https://zhuanlan.zhihu.com/p/683606248)

## Sora的发布引发的连锁效应。

![](img/8cd41c1ba3246e4204fbb6d868fb5203.png)

不仅仅是Stable Diffusion 3发布，据说Mid Journey马上也要发布视频以及V7。

![](img/5d0cbb383a0aae5bd44f5b84f710e455.png)

同时OPENAI很可能会在近期发布ChatGPT4.5, Anthropic也会在近期发布Claude 3。

![](img/afbb1190c0555aa8350ed94b6758a36a.png)

这也从一个侧面看出人工智能应用圈是有多卷。Stable diffusion 3 的突然发布，显然是不希望被其他竞争对手抢了头条。

![](img/c846a68e01616b4fb4f6ce57ba414e27.png)

其实，就在前几天Stability AI才放出Stable Video，正式开放公测了。而Stable Video背后的技术还是基于Stable Video Diffusion 1.1。

正是之前因为Sora发布，导致Stability发布的Stable Video Diffusion 1.1信息，没有引发什么讨论热潮。

![](img/4ea82d0e38f73246c5b1da00c502e058.png)

对比OpenAI并不OPEN的闭源做法，我认为Stability AI发布的内容更加值得关注，无论是Stable Diffusion 3还是SVD，未来都是有可能开源继而影响整个图像视频的生态环境的。

![](img/3722665bd6cf83feac2a379a0466e263.png)

而Stability AI公司首席执行官 Emad Mostaque 在 推特的帖子中说在未来，他们会把这个模型模型开源，这才是真正的互联网精神。

## 一、Stable Diffusion 3相关信息

Stability AI 最近宣布了其最新的模型系列——Stable Diffusion 3。

### 1.情况介绍

Stable Diffusion 3系列的模型具有不同的参数量，范围从800万到80亿，这样的设计可以让这些模型能够在多种便携设备上顺畅运行，可以显著降低AI大模型的部署难度。

而相较于前代的SDXL版本，Stable Diffusion 3 在图像生成质量方面取得了显著提升。

![](img/c0ccbb107b2ccd0dd0b2613e259136fc.png)

这一进步不仅体现在图像的清晰度和细节丰富度上，还包括了对多主题提示的优化处理，使得用户可以更精确地指导模型创造出符合特定主题的图像。

### 2.文字生成

模型在文字生成效果上也得到了增强，能够更准确地模拟和再现各种字体和书写风格，这对于需要高质量文本元素的图像创作尤为重要。

在下图中可以看到，主要英文均拼写正确，而且还有不同字体。

![](img/dc29bb9b0ebda45c1e56555792ce648f.png)

但在氛围感方面也有提高，例如以下提示词，美学效果上较为平淡：

```
电影照片，教室的桌子上放着一个红苹果，黑板上用粉笔写着 &quot;go big or go home&quot; 的字样（cinematic photo of a red apple on a table in a classroom, on the blackboard are the words &quot;go big or go home&quot; written in chalk） 
```

![](img/1052062fd020522fc5b622dcb5e11469.png)

```
厨房的桌子上放着一块绣花布，上面写着“晚安”和一只绣着小老虎。在布的旁边有一根点燃的蜡烛。灯光昏暗而引人注目。
```

![](img/a56418669b5495ad97b64cba235c8acf.png)

### 3.技术架构

而在技术架构方面，Stability AI 与 Sora 团队一样，选择了 Diffusion transformer（DiT）架构作为新模型的基础。

他们在官方博客中提供了对 William (Bill) Peebles 和谢赛宁共同撰写的 DiT 相关论文的链接。该论文目前被引用次数已达到201次，预计在本年度内这个论文的引用量肯定会显著提升。

![](img/22b3f3466c55da8ee7f09b41f8b9c09a.png)

不过Stable Diffusion 3 模型目前尚未完全对外开放，Stability AI 团队表示，他们正在实施一系列安全措施，以防止潜在的恶意使用者滥用这一技术。

### 4.效果对比及试用链接

那么Stable Diffusion 3 和其他的软件相比，例如和Firefly、DALLE 3、Midjourney 比效果如何？

网上有外国网友做了测试，看起来似乎没有拉开差距，DALLE 3和Midjourney 语义和效果呈现都还不错。

![](img/b8de3b180f3740b47110cde790f3d478.png)

但是，Stable Diffusion 3 是开源领域的希望，只有Stability AI秉承着开源的理念，在营造更加丰富的应用生态。

因此，我们还是更应该关注Stability AI的动态。

想要尝鲜的用户可以点击以下链接提交申请：

[ ](https://link.zhihu.com/?target=https%3A//stability.ai/stablediffusion3)

## 二、Stable Video相关信息

目前我们无法体验Stable Diffusion 3,但是我们可以尝试体验SVD，SVD1.1模型是在SVD-XT模型上微调而来，可以生成25帧1024x576的短视频。

### 1.进步

对比去年11月份SVD（Stable Vision Diffusion）模型的1.0版本，最新的SVD 1.1版本在模型体积上实现了显著的优化，减小了近一半，不仅极大地提高了生成速度，同时也降低了对运行设备的性能要求。

在视频处理方面，SVD 1.1版本在镜头运动和动作幅度的表现上有了显著的进步。人物主体的动态表现更加流畅自然，稳定性也得到了增强。特别是在人物面部表情的渲染上，如眨眼和口型等细节，表现得尤为逼真。

![](img/ada12ac2d12c04c839509dcc22cef84b.png)

### 2.缺憾

虽然取得了明显进步，但是SVD1.1对比Sora还是有很多缺憾。主要有以下几点：

1. SVD 1.1模型生成的视频片段时长较短，通常不超过4秒。
1. 模型在生成过程中可能倾向于产生静态或缓慢移动的画面，难以捕捉快速或复杂的动作序列。
1. 在交互性和控制方面，SVD1.1目前还不支持通过文本指令直接控制视频内容的生成，其功能主要集中在将静态图像转换为视频。
1. 而在文字内容的生成上，尤其是在需要生成清晰、准确文字信息的场景中无法生成清晰文字。
1. 当视频中的人物占比较小时，SVD1.1可能无法准确生成人物的面部特征。

尽管如此，随着技术的不断进步，希望这些问题能尽快改进。下面让我们看看SVD的效果吧。

### 3.效果展示

SVD虽然开放，但是登陆生成较慢，我找了好友帮我生成部分视频，有效果好的，例如下图

```
提示词：Vineyard in the sun, fruitfulgreen grapes hanging from thebranches
```

[🎦 点击观看视频](https://vdn3.vzuu.com/HD/e05474c2-d221-11ee-b5df-d246a47ad65d-v8_f2_t1_lSNRfKv7.mp4?auth_key=1724153610-0-0-588e8feac771519be65d75d196faa109&bu=1513c7c2&c=avc.8.0&disable_local_cache=1&expiration=1724153610&f=mp4&pu=e59e796c&v=tx&pp=ChMxNDAxNjIzODY1NzM5NTc5MzkyGGMiC2ZlZWRfY2hvaWNlMhMxMzY5MDA1NjA4NTk5OTA0MjU3PXu830Q%3D&pf=Web&pt=zhihu)

![](img/be7483285bd3cfd610a94c01ba90d511.png)

00:04

我把SVD和Sora的效果做了一个对比。

有效果平平的，几乎没有太强动感的

```
a giant cathedral is completely filled with cats. there are cats everywhere you look, a man enters thecathedral and bows before the giant cat king sitting on a throne.
```

[🎦 点击观看视频](https://vdn6.vzuu.com/HD/a9fe9eb4-d223-11ee-bff9-624e731c821e-v8_f2_t1_kyTjxk0P.mp4?pkey=AAXiPaaqpPXgk4oiKfdLbUAUo_kAMFtyCKXILAFCYgT2gXZpbr0XxYSN_UsVPAFxfiv3vLGbVgFzSskVgzZgor-X&bu=1513c7c2&c=avc.8.0&expiration=1724157210&f=mp4&pu=e59e796c&v=ks6&pp=ChMxNDAxNjIzODY1NzM5NTc5MzkyGGMiC2ZlZWRfY2hvaWNlMhMxMzY5MDA1NjA4NTk5OTA0MjU3PXu830Q%3D&pf=Web&pt=zhihu)

![](img/e551d3a112e937ffd1b4826a2c6d0d27.png)

00:04

对比Sora差的不是一点。

[🎦 点击观看视频](https://vdn6.vzuu.com/HD/e481fb44-d223-11ee-b593-622eacbb3a3c-v8_f2_t1_UOsuLefw.mp4?pkey=AAXhw39naOt90gj0f5RM0ZL_ZxbU1dcjn8nUV6DHiEuH25TPYPAgQs9QJBFY-gpbIHVzd2vvmZWjSZ04UZk72t3w&bu=1513c7c2&c=avc.8.0&expiration=1724157210&f=mp4&pu=e59e796c&v=ks6&pp=ChMxNDAxNjIzODY1NzM5NTc5MzkyGGMiC2ZlZWRfY2hvaWNlMhMxMzY5MDA1NjA4NTk5OTA0MjU3PXu830Q%3D&pf=Web&pt=zhihu)

![](img/e324ea57e1bb95cfbdce2d5b3b1fb25c.png)

00:20

还有效果惊悚的，这个后面直接就崩了。

```
Animated scene features a close-up of a short fluffy monster kneeling beside a melting red candleThe art style is 3D and realistic, with a focus on lighting and texture. The mood of the painting is one ofwonder and curiosity, as the monster gazes at the flame with wide eyes and open mouth. Its pose andexpression convey a sense of innocence and playfulness, as if it is exploring the world around it for the firsttime. The use of warm colors and dramatic lighting further enhances the cozy atmosphere of the image.
```

[🎦 点击观看视频](https://vdn6.vzuu.com/HD/1760d5aa-d222-11ee-a496-dab8c54acdb4-v8_f2_t1_uI4b116T.mp4?pkey=AAUZVWwKFqUg8elWe8rEpNaA-24yS0k1lU2tLDAUa24efrfur5d2AOCWC565Kq4oOM0DI4xIBQ7mIzaQ2SBEf19w&bu=1513c7c2&c=avc.8.0&expiration=1724157210&f=mp4&pu=e59e796c&v=ks6&pp=ChMxNDAxNjIzODY1NzM5NTc5MzkyGGMiC2ZlZWRfY2hvaWNlMhMxMzY5MDA1NjA4NTk5OTA0MjU3PXu830Q%3D&pf=Web&pt=zhihu)

![](img/7d87a4c0fb122dc0c299dd4c6951e917.png)

00:04

从这几个测试视频来看，目前的技术还是存在差距。

另外，特别提醒想要体验的小伙伴，目前SVD不支持中文，例如这个视频：

[🎦 点击观看视频](https://vdn.vzuu.com/HD/2a7dc0ec-d224-11ee-ba0b-ae1166a9cdc1-v8_f2_t1_4AXPtSNH.mp4?auth_key=1724153610-0-0-ea8778f58ddd5b0e1368e37e2ba623fc&bu=1513c7c2&c=avc.8.0&disable_local_cache=1&expiration=1724153610&f=mp4&pu=e59e796c&v=ali&pp=ChMxNDAxNjIzODY1NzM5NTc5MzkyGGMiC2ZlZWRfY2hvaWNlMhMxMzY5MDA1NjA4NTk5OTA0MjU3PXu830Q%3D&pf=Web&pt=zhihu)

![](img/1fc3fdd0e5d03688f0dfe30890a79d2d.png)

00:04

你很难想象这个提示词是这个：

![](img/8dfba9bbaa096fae3bb7ea164139c242.png)

### 4.体验地址

说了这么多，也奉上体验地址，大家可以自己去试试：

[ ](https://link.zhihu.com/?target=https%3A//www.stablevideo.com/)

从前，这个模型需要用户自己上手部署，现在Stability AI已经帮忙部署好了，甚至不需要排队就可以使用。

有两种模式，第一个是上传图片生成视频。

![](img/61713384395b13b4b95033ce0b6f6993.png)

另外一个是和 Sora 相同，可以通过文本输入生成视频：

![](img/246d4eaddf7b3c9ab5476c16683aabee.png)

快去尝试体验下吧！

## 结语

今年一开年，就这么多巨大的变化，看来，今年也会是人工智能方面的大战时刻。

![](img/2a624afc10b08f759b820f996d89aee5.png)

作为普通人的我们，一定要努力跟上时代，学习新的知识！

---

我是德里克文，一个对AI绘画，人工智能有强烈兴趣，从业多年的设计师！如果对我的文章内容感兴趣，请帮忙关注点赞收藏，谢谢！
