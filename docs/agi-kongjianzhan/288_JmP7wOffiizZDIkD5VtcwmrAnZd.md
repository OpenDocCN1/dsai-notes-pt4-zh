# 一文搞懂OpenAI推出的AI文本视频生成模型Sora

> 来源：[https://agispace.feishu.cn/docx/WHv0ddcD3orWlbx1g4JcB1ICnSc](https://agispace.feishu.cn/docx/WHv0ddcD3orWlbx1g4JcB1ICnSc)

> 作者：毅鸣

> 👸号： AI洞察笔记

> 原文链接：https://mp.weixin.qq.com/s/YvxDwbPEjc_FAPDS5GbtVQ

![](img/986761488908f959f746aae3109b6172.png)

# 1 Sora是什么？

2月15日，OpenAI官网上线了一个新的人工智能模型，可以通过文本提示词指令从0到1创建长达60秒的高清视频，完美继承了DALL.E 3的画质和遵循、理解指令的能力。正如周鸿祎所说的：Sora意味着AGI实现将从10年缩短到1年。

官网视频案例及提示词大全，复制下方官网链接查看详情。

官网地址：https://openai.com/sora

![](img/4b521ee46d89bbd7374c21b7af818fa8.png)

[1-一文了解Sora视频生成AI模型流畅-0bc3feacoaaafuabjlotarsvakode4uqajya.f10004.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/CIEzbw5FvoUxI8x6DZMcpw8GnCb)

# 2 Sora能做什么？

Sora的主要功能是将文本描述转化为视频内容，能够准确理解并准确执行提示词，生成具有连贯性的视频，这些视频能够包含多个角色、动作和场景，也能够从静态图像生成动画，或扩展现有视频，这一技术展示了AI在理解和模拟物理世界运动方面的进步，为创意产业发展提供了新的可能性。

+   Sora 能够生成提供图像和提示作为输入的视频。

+   Sora 还能够在时间上向前或向后扩展视频。比如多个视频都是从生成的视频片段开始向后延伸的。因此，几个视频的开头都不同，但所有视频的结局都是相同的。使用此方法向前和向后扩展视频以产生无缝的无限循环。

+   扩散模型启用了多种根据文本提示编辑图像和视频的方法。将其中一种方法 SDEdit, 32 应用于 Sora。这项技术使 Sora 能够零镜头地改变输入视频的风格和环境。

+   还可以使用 Sora 在两个输入视频之间逐渐进行插值，从而在具有完全不同主题和场景构成的视频之间创建无缝过渡。

+   Sora 还能够生成图像。通过在时间范围为一帧的空间网格中排列高斯噪声块来实现这一点。该模型可以生成各种尺寸的图像，分辨率高达 2048x2048。

![](img/b10a625d66c84cb35bfe55d69bf86919.png)

[2-一文了解Sora视频生成AI模型流畅-0b2eceab6aaae4acre6tc5svaeodd4iqahya.f10004.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Nh7jbQMSaoSqpLxZvM5cklqTnag)

# 3 Sora适用场景有哪些？

**创意内容制作: **为电影、动画、游戏和广告行业提供快速原型制作和概念验证教育和培训:创建教学视频，模拟复杂场景，帮助学生和专业人士学习新技能。

**娱乐和艺术：**为艺术家和设计师提供工具，以探索新的创意表达方式。

**模拟和预测：**在科学研究和工程领域，模拟实验和预测结果。

**虚拟现实和增强现实：**为 VR 和 AR 应用生成逼真的虚拟环境和交互体验。

**社交媒体和内容创作：**帮助内容创作者快速生成吸引人的视频内容，增加社交媒体互动。

**新闻和媒体：**制作新闻报道的模拟场景，增强报道的视觉效果。

Sora 的这些应用场景展示了其在多个领域的潜力，尤其是在需要视觉内容创新和快速生成的场合。

![](img/497e529876e3ddb149608b5a2499076a.png)

[3-一文了解Sora视频生成AI模型流畅-0bc36uacuaaaweabauwtefsvb5odfl2qakqa.f10104.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/KPdAbImFQoV6JUxu3RLcWzKVnDg)

Sora的问世，不仅给短视频行业，更是给AI行业都带来了无限可能，目前我们还不能体验，只能通过官网的例子来了解这些视频，来了解Sora的强大，希望能够早日开放给我们普通人使用。

![](img/c13b3be7fa491462d22e81b1f2c0146a.png)

![](img/21f904a6d302198f565e7a9b5e4a10c6.png)