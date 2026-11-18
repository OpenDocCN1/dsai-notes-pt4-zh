
# Datawhale - AI视频生成学习

> 来源：[https://datawhaler.feishu.cn/docx/G4LkdaffWopVbwxT1oHceiv9n0c](https://datawhaler.feishu.cn/docx/G4LkdaffWopVbwxT1oHceiv9n0c)

> 写在前面：Sora很火但还**没正式开放**，风很大，心很燥。希望大家独立思考，减少噪声，找信源和一手信息，了解原理，动手去做，在实践中学习。

## Sora是什么？

Sora是 `text-to-video` 模型（可能是世界模型），OpenAI的研究人员选择这个名字，因为它“唤起了无限创造力潜能”，特点是：创建最多 60 秒的视频，高度详细的场景，复杂的多相机视角以及富有情感的多角色。

Sora官网链接：https://openai.com/sora

![](img/b6000c2680e0fdac5ffcf7f34a3c0013.png)

![](img/5ee9dce220cd8a168ba2c2699cc42485.png)

![](img/3c39f0d7299f213dc0ddf3838a9ca90d.png)

[视频案例](https://datawhaler.feishu.cn/wiki/XTZ0wCYtti8fPAkvIzecpecznfh?from=from_copylink) 是 OpenAI 的官方 Demo 视频效果，更多详见：

## [Sora原理与技术实战](https://datawhaler.feishu.cn/wiki/LxSCw0EyRidru1kFkttc1jNQnnh?fromScene=spaceOverview)

![](img/0908d8f62a5a6dab4b1724b491d28e3c.png)

[**Datawhale x**](https://datawhaler.feishu.cn/wiki/LxSCw0EyRidru1kFkttc1jNQnnh?fromScene=spaceOverview)[Sora原理与技术实战](https://datawhaler.feishu.cn/wiki/LxSCw0EyRidru1kFkttc1jNQnnh) 魔搭社区（阿里巴巴达摩院模型开源社区 `modelscope`）联合组织 Sora 原理与技术实战课程，以下为课程信息简介，完整课程信息见：，课程完全开源，学习活动免费。

<https://datawhaler.feishu.cn/sync/ZrZEdw1X2s0GIUbS4Pnceuvbntb>

预计学习周期 14 天，适合以下人群：

+ 有一定 `Python` 基础
+ 能熟练阅读和理解项目源代码
+ 有足够的精力投入，积极参与开源，热爱分享

<https://datawhaler.feishu.cn/sync/GJcYdh5IBsxypxbpMlfcBt7knNd>

多维表格（`J0lobxBRKaLtHJslUylcco4InPg_tbl1jdFunqozlHoV`）

## 动手学[AI视频生成](https://datawhaler.feishu.cn/wiki/CLJZwjyRXiIevRkf7AjcQHR4nce)

**虽然 Sora 还没开放，但 AI 视频生成已有很多 AI 工具可以使用，先建立工作流，再不断优化工作流，重点是动手去做。**

以下教程，通过 GPT 等生成剧本、分镜脚本，MJ/SD 等生成图片，然后 PIKA/SDV 等生成视频，剪映等做视频后期，搭建了基础 AI 视频生成的工作流，并以新年为场景案例，希望可以启发大家动手去做，Sora 开放后可再优化。

**听了那么多 AI 新闻，不如动手去做一遍！自己动手，才知道 AI 的能力与边界，AI 如何提升生活的效率与效果！**

[AI + 视频](https://datawhaler.feishu.cn/wiki/CLJZwjyRXiIevRkf7AjcQHR4nce) 课程内容来自奇想星球平台，完整内容点击：

![](img/bcbefe9e70dbb0f73dc076f7af6c86dd.png)

![](img/145d0a486e2d950613f809a86873a7cc.png)

[AI新年好.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Z92rbUX23odCoUxxFT9c8F3rnsh)

**AI新年好**

![](img/784185aa04d6a1b502c5bf76481c1d4f.png)

[AI生成新年主题视频.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/KggnbYUuPoij8kxt1ptcqvXznKd)

**雪夜奇缘 - 新年喵喵好**

## 类Sora模型

基于 `DiT` 架构，腾讯混元文生图大模型 `Hunyuan-DiT` 开源了，包含模型权重、推理代码、模型算法等完整信息。

主页：https://dit.hunyuan.tencent.com/

代码：https://github.com/Tencent/HunyuanDiT

## 聊聊Sora

Datawhale 会持续邀请学术界、产业界等各界 [朋友](https://datawhaler.feishu.cn/docx/G4LkdaffWopVbwxT1oHceiv9n0c#TY2Ydav83oPJx8xwa71cV4Kcnxo)，一起来聊聊 Sora，希望参与请联系玉鑫。

| 聊聊Sora | 特邀嘉宾 | 核心看点 | 视频链接 | 相关文档 |
| --- | --- | --- | --- | --- |
| 第一期：[Sora](https://www.bilibili.com/video/BV1KZ42127GP) [硬核技术解读](https://www.bilibili.com/video/BV1KZ42127GP)，欢迎来到 AI 生成的世界 | 袁超逸：Datawhale 成员，某大厂 NLP 算法工程师 | 司玉鑫 - AI 视频生成 Sora 学习文档解读 | <https://player.bilibili.com/player.html?bvid=1KZ42127GP> | 1. HuggingSD 负责人李孝杰的解读：https://zhuanlan.zhihu.com/p/683004185 |

## Sora技术报告

Sora技术报告原文：https://openai.com/research/video-generation-models-as-world-simulators

核心摘要：我们探索了利用视频数据对生成模型进行大规模训练。具体来说，我们在不同持续时间、分辨率和纵横比的视频和图像上联合训练了以文本为输入条件的扩散模型。我们引入了一种 `transformer` 架构，该架构对视频的时空序列包和图像潜在编码进行操作。我们最顶尖的模型 `Sora` 已经能够生成最长一分钟的高保真视频，这标志着我们在视频生成领域取得了重大突破。我们的研究结果表明，通过扩大视频生成模型的规模，我们有望构建出能够模拟物理世界的通用模拟器，这无疑是一条极具前景的发展道路。

相关技术解读：

1. 推荐来自魔搭社区成晨老师的技术解读：https://mp.weixin.qq.com/s/xP46EocNg1x7IlTN_iF8kw

![](img/b4b27bed63ea02f3e9183c85a056c3a6.png)

1. 推荐来自阿里 CV 高级算法专家周文猛的解读：一文看 Sora 技术推演：https://mp.weixin.qq.com/s/3RnrO7fSMizEl3mN3SXG5w

![](img/647f016bcc4e7534a1399f2bc1496554.png)

1. 推荐魔搭社区基于 `ViT` 的扩散模型技术的开源教程：https://mp.weixin.qq.com/s/LQGwoU6xZJftmMtsQKde_w

![](img/6a3df7c6c5a72b4e7ae2cea618d98a77.png)

1. 推荐腾讯科技对 Sora 技术报告的解读：https://mp.weixin.qq.com/s/gSvxvOVqYtGcKw0ueDGbFA

1. 来自 NLP 算法工程师袁超逸的解读：https://mp.weixin.qq.com/s/O4kw6WRGMpxbMf0yYLU51Q

1. 厦大平潭研究院研究员 杨知铮老师的解读：https://zhuanlan.zhihu.com/p/682579303?utm_campaign=shareopn&utm_medium=social&utm_oi=55619453190144&utm_psn=1742473712694239232&utm_source=wechat_session&s_r=0

1. HuggingSD 负责人李孝杰的解读：https://zhuanlan.zhihu.com/p/683004185

## Sora相关论文

Sora 技术报告的 `References` 中共有 32 篇 `paper`，我们汇总了所有的 32 篇论文（还有一些非 32 篇的有趣文章），并且列出了文章核心关键词、摘要，希望深度学习的小伙伴可以研究，知其然，知其所以然，希望大家研究地开心（希望参与深度研讨，请联系玉鑫）。

更多内容请关注：[论文研究](https://datawhaler.feishu.cn/wiki/EEnnwRdlxiYUDsklpWkcw5zGnW2)

![](img/e64e464e252a0cdf4d53893ec7fc6447.png)

多维表格（`J0lobxBRKaLtHJslUylcco4InPg_tblKuVcENO1T0cna`）

## 应该关注谁？

目前 Sora 还没有正式开放，一切以 OpenAI 团队为准，以下为 [Sora 团队](https://openai.com/research/video-generation-models-as-world-simulators) 的名单（来源于技术报告里的 [Authors](https://mp.weixin.qq.com/s/ecfhRMJCvwG4HQTHkOZdag)）与相关社媒（看作者的一手信息），Sora 团队成员的背景分析（果然大佬都是六边形战士）：https://mp.weixin.qq.com/s/ecfhRMJCvwG4HQTHkOZdag

- OpenAI：官推发正式通告，https://twitter.com/OpenAI
- Sam Altman：真一手消息，在线接单，@他有机会体验 Sora，https://twitter.com/sama
- Tim Brooks：研发 Leader，https://twitter.com/_tim_brooks
- Bill Pebbles：研发 Leader，https://twitter.com/billpeeb
- Connor Holmes：Systems Lead for Sora，https://twitter.com/cmikeh2
- Will DePue：https://twitter.com/willdepue
- Yufei Guo
- LiJing：https://twitter.com/jingli9111
- David Schnurr：https://twitter.com/_dschnurr
- Joe Taylor：https://twitter.com/joetaylor341
- Troy Luhman：https://twitter.com/LuhmanTroy
- Eric Luhman
- Clarence Wing Yin Ng
- Ricky Wang：https://twitter.com/ryw999
- Aditya Ramesh：https://twitter.com/model_mechanic

## 如何体验到Sora？

目前 Sora **尚未开放**，仅有极少量的 `red-teaming` 内测使用者，一切非 OpenAI 官方提供服务皆可视为诈骗。

![](img/40aa58c2800abe0ad5ac93d0db0014b5.png)

![](img/6c6eb4e57e1009632706f35c8d8d6b5f.png)

那么如何才能有机会体验到 Sora？

1. 申请成为 `red-teaming`：https://openai.com/form/red-teaming-network

![](img/69e981fd32625ec0d95111048cb35fae.png)

1. X 上在 Sam Altman 的推特下评论（奥特曼在线接单）

![](img/c9fec4e3fdf298a0df96994ed1e7745a.png)

1. 申请加入 OpenAI 的官方论坛，https://forum.openai.com/，加入后可能有机会体验到

![](img/19ab68a64d3834ce460e8a02744f628c.png)

![](img/f55dbf50dec82aafb88bc955af5fd17c.png)

## 如何加入Sora？

Systems Lead for Sora，Connor Holmes：

如果这个 Sora 示例让你想起了 `reduce-scatter` 和 `all-gather`，那么我们正在招聘！我们正在寻找对系统效率充满热情、相信在扩展方面有经验并且擅长解决这些问题的人才。如果这听起来符合你的背景，请联系我们！

David Schnurr：

我们正在招聘具有大规模视频基础设施经验的人才，涵盖 `serving`、`processing`、`ingest` 等方面的经验。如果你认为自己是一个合适的人选，请联系我们！

![](img/57202ba4dd3843db86a1b2b30cd89bb8.png)

![](img/be4bc6af87c3e3ceef0dc847aa1a4860.png)

目前 OpenAI 的 `Careers` 上没有更新 Sora 的岗位（及时关注 http://openai.com/jobs），可以在发布岗位的 Connor Holmes 与 David Schnurr 的推特下留言。

![](img/e9496467251b8cb8c48cf5258265770e.png)

## 多方观点

[各方观点](https://datawhaler.feishu.cn/wiki/Eku4wRPUwidOd7k4l9Qc7DO7nMb)听则明，偏听则暗，独立思考，更多内容关注：

> **360 创始人 周鸿祎**

1. `AGI` 实现时间缩短：由于科技的发展，`AGI` 的实现时间可能会从 10 年缩短到 1 年。
2. 科技竞争与人才密度：科技竞争最终取决于人才密度和深度积累。`OpenAI` 等拥有核心技术的公司在 `AI` 领域的实力强劲，这表明创业公司不能仅依赖 `AI` 技术，还需要有创新和创意。
3. `AI` 对行业的颠覆：`AI` 可能不会立即颠覆所有行业，但它能激发人们的创造力。例如，`Sora` 可能对广告业、电影预告片和短视频行业产生重大影响，但不太可能迅速击败 `TikTok`，更可能成为其创作工具。
4. 国内 `AI` 发展水平：尽管国内大模型的发展水平接近 `GPT-3.5`，但与 `GPT-4.0` 相比仍有一年半的差距。`OpenAI` 可能还持有一些未公开的技术优势。
5. 大语言模型的能力：大语言模型不仅仅是填空机，它们能够完整地理解世界知识。`Sora` 通过结合大语言模型（`LLM`）和 `Diffusion` 技术，实现了对现实世界的理解和模拟，这代表了未来 `AI` 的发展方向。
6. `AI` 对物理世界的理解：`Sora` 展示了 `AI` 对物理世界的模拟能力，这将对机器人具身智能和自动驾驶等领域产生重大影响，因为这些领域需要对世界有深入的理解。
7. 视频学习的重要性：`OpenAI` 训练 `Sora` 时可能会阅读大量视频，这将帮助 `AI` 更好地理解世界。通过视频学习，`AI` 对世界的理解将远超文字学习，这可能会加速 `AGI` 的实现。

> **陶芳波-心识宇宙 CEO**

> 1. `Dall-E` 会被弃用，所有的视觉生成统一到 `Sora`，`Sora` 未来成为世界模型的基础，负责图片、视频、3D。
> 2. 下一步，`Sora` 被统一到 `LLM`，构造下一代 `Foundation Model`（真正的 `GPT-5`？），`LLM` 负责抽象世界，`Sora` 负责现实世界，但架构统一，参数融合（配上各种特异 `decoders`，如 `diffusion`），智力的借用也更融合。
> 3. 再往后，不再有各种 `AIG-X` 模型，世界范围内统一一套 `Foundation Model` 和 `Foundation Agent` 的标准和参数，每几年迭代一次，成为 `World AI Kernel` / 世界智能内核。所有人基于该标准，来开发 `AI-Native` 应用。
>
> 结论:
> 1. `OpenAI` 可能是世界的福音，但是是当下 `AI` 创业的噩梦。
> 2. 比特的大一统，比能量（力）的大一统来的更快。

> **谢赛宁-纽约大学任助理教授**



# Sora学习交流群

## 简单性和可扩展性

在开发Sora模型时，Bill和我优先考虑了简单性和可扩展性。简单性带来了灵活性，使得模型在处理输入数据时更加灵活，例如在屏蔽自动编码器（`MAE`）中的应用。可扩展性则是`DiT`（Vision Transformer）论文的核心主题，它不仅提高了运行速度，而且证明了`DiT`缩放定律适用于视频，如同在图像处理中一样。

## 灵活性

`ViT`（视觉变换器）的一个优点是它在处理输入数据时的灵活性。例如，Sora能够通过在适当大小的网格中排列随机初始化的补丁来控制生成视频的大小，而`UNet`（一种卷积神经网络架构）并不直接提供这种灵活性。

## 尺寸推测

我推测Sora可能使用了Google的`Patch n'Pack`（`NaViT`），这使得`DiT`能够适应可变的分辨率、持续时间和长宽比。他还提到，Sora可能使用了较大的模型尺寸，大约有3B（30亿）个参数，这可能意味着训练Sora模型并不需要像预期的那样多的GPU资源。

## 未来迭代速度

由于Sora的可扩展性和简单性，未来的迭代将会非常快。

本文由Datawhale成员编写，引用需要注明作者和出处

关注Datawhale，学习AI不迷路，群里老哥说话超好听，我超爱在这里的，加群一起学习交流AI视频生成，show出你的AI视频demo

![](img/5bb247205adae01794e08c4b717778fd.png)

![](img/6dd8b6d9057622f31ae397d0096608a3.png)

> 赠人玫瑰，手留余香，看到这里了，点个赞吧
