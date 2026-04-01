# 工具教程-Flux AI

> 来源：[https://agispace.feishu.cn/docx/Vrsbdc8NhoaV2jx5wFdcgCHOnWf](https://agispace.feishu.cn/docx/Vrsbdc8NhoaV2jx5wFdcgCHOnWf)

前几天提到过的[**Stable**](https://agispace.feishu.cn/wiki/Xs6HwsNyDi92O8kayXJclpcZnsd#part-Hbkcd5NtwoFiK3xXxzBcL0oan5c) Diffusion 原班人马，发布FLUX.1，又整活了。最新在社交媒体上发布了一组由Flux生成的合照。

**AI**生图工具Flux在全网掀起了一场风暴。Flux生成的情侣合照和其他图像细节逼真，几乎无法挑出破绽。无论是光线、纹理还是毛发，Flux都能处理得非常细腻。视频、声音和口型的同步也达到了令人惊叹的程度。

以下是部分样图：（点击可放大）

![](img/9e3959a4395e9d083f084e31ecc6b5f2.png)

![](img/bc3bbf228a2dc07d9dd2a3ba3b1d58ef.png)

![](img/ffd4325d8b0cd2c8af9ca4265484b2b9.png)

![](img/9d7a54edadde234c09ae1c0bf1613fba.png)

![](img/cb84d535730e3e22d08451e75573bbe6.png)

![](img/16064f9db58fb0fd02e87cd44f3281e4.png)

![](img/5b316fd5ae714c5526958b1ed370cfb8.png)

开源文生图模型霸主**Stable** Diffusion原班人马，宣布推出全新的图像生成模型FLUX.1。

![](img/ed1ed77631f6f3800c8e050c29514ab1.png)

FLUX.1在视觉质量、图像细节和输**出多样性等方面性能优越，其具**有三大特点：文字生成、复杂构图、人手描绘。

第一个变体是 **FLUX.1 [pro]**，它是全新的 SOTA 文生图模型，具有极其丰富的图像细节、极强的 prompt 遵循能力和多样化风格。目前可以通过 API 使用。

+   API 地址：https://docs.bfl.ml/

第二个是 **FLUX.1 [dev]**，它是 FLUX.1 [pro] 的开放权重、非商用变体，并直接基于后者蒸馏而成。该模型的表现优于 Midjourney 和 Stable Diffusion 3 等其他图像模型。推理代码和权重已经放在了 GitHub 上。下图是与竞品图像模型的比较。

+   GitHub 地址：https://github.com/black-forest-labs/flux

第三个是开源的 FLUX.1 [schnell]，它是超高效的 4-step 模型，遵循了 Apache 2.0 协议。该模型在性能上与 [dev]、[pro] 非常接近，可以在 Hugging Face() 上使用。

+   Hugging Face 地址：https://huggingface.co/black-forest-labs/FLUX.1-schnell

![](img/cedfabb5a4b8a01b1bffa3250d17e342.png)

FLUX.1包含**专业版、开发者版、快速版**三种模型，其中前两款模型击败SD3-Ultra等主流模型，较小规模的FLUX.1[schnell]也超越了Midjourney v6.0、DALL·E 3等更大的模型。

三款模型实用地址：

https://replicate.com/black-forest-labs/flux-pro

https://replicate.com/black-forest-labs/flux-dev

https://replicate.com/black-forest-labs/flux-schnell

**在下一步的工作上，黑**森林预告将发布一款SOTA文生视频模型，“让所有人都能将文本转为视频”。该模型将建立在FLUX.1的基础上，“以高清和前所未有的速度实现精确创作和编辑”。

**推荐阅读**

（Jack Cui）又一个爆火的AI项目，逼真到恐怖的Flux！【保姆级硬核教程】

https://mp.weixin.qq.com/s/T8Wew9zvUtIhKXSEfgkP4Q

（新智元）情侣合照逼真到恐怖，竟被代码识出破绽？沃顿教授预言AI 18个月封神！

https://mp.weixin.qq.com/s/XUb8dzNB19Guqm3Y4oxroA