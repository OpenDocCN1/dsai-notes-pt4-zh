# 神经网络算法 - 一文搞懂DiT（Diffusion Transformer）

> 来源：[https://geekbang.feishu.cn/docx/P1tUdhAt4obZcZxgSogcuexxnCf](https://geekbang.feishu.cn/docx/P1tUdhAt4obZcZxgSogcuexxnCf)

> 👨🏻 作者：AllenTang

> 📃 作者介绍：分享人工智能，让所有人玩转AI

> 🛖 公众号：架构师带你玩转AI

> ⏰ 收录时间：2024-05-23 11:28:42

> 🔗 原文链接： h[ttps://mp.weixin.q](https://mp.weixin.qq.com/s?__biz=MzkzMTEzMzI5Ng==&mid=2247488615&idx=1&sn=f8a80a8da207fd7a17267bad3f270f40&chksm=c26ef678f5197f6e77adc76e2dca93cc35384586c95435de1bcb67398dd2eb4279f849d74ecb&scene=21#wechat_redirect)q.com/s?__biz=Mz...

![](img/e6e3c67e1b0073ac4e98aeeb683b529c.png)

关注作者公众号，获取更多AI知识

本文将从 DiT的本质 、DiT 的原理 、DiT 的应用 三个方面，带 您一文搞懂 Diffusion Transformer｜D iT 。

![](img/6542a4eaccdd7a3b3ab1b55caed36e42.png)

Diffusion Transformer（DiT）

**一、D iT 的本质**

**Di**T的定义： Diffusion Transformer是一种结合了Transformer架构的扩散模型，用于图像和视频生成任务，能够高效地捕获数据中的依赖关系并生成高质量的结果。

![](img/69e0424bdd7b4a51d3680a43eff153b6.png)

Diffusion Transformer（DiT）

**扩散模型的定义 ： Diffusion Models是 一种新型**的、先进的生成模型 ，用于生成与训练数据相似的数据，可 以生成各种高分辨率图像。

![](img/5da9114cc875a84d94b862a330fb7117.png)

**扩散模型 的定义**

**扩散模型的核心思想： Diffusion Models**是 一种受到非平衡热力学启发的生成模型，其核心思想是 通过模拟扩散过程来逐步添加噪声到数据中，并随后学习反转这个过程以从噪声中构建出所需的数据样本。

![](img/c27a56bed1def427c9490fb7bcab947b.png)

**扩散过程**

详细了解扩散模型Diffusion Models： 神经网络算法 - 一文搞懂扩散模型Diffusion Models

**DiT的本质： Diffusion Transformer是一种**新型的扩散模型，结合了去噪扩散概率模型(DDPM)和Transformer架构。

![](img/632164c1442594fb5f1c868fdccaee06.png)

**去噪扩散概率模型(DDPM)**

**Di**T的核心思想： Diffusion Transformer的核心思想是使用Transformer作为扩散 模型的骨干网络，而不是传统的卷积神经网络(如U-Net)，以处理图像的潜在表示。

** **

![](img/ec0131507ee9fe3ed6fcac20061280d2.png)

**DiT的核心思想**

**二、D iT 的原理**

**DiT的架构 ： ****Di**T架构基于Latent Diffusion Model（LDM）框架，采用Vision Transformer（ViT）作为主干网络，并通过调整ViT的归一化来构建可扩展的扩散模型。 如下图所示：

![](img/07709cdc4bdeab46092c54704694068d.png)

**DiT 的架构 **

**DiT的核心组件 ： ****D**iT有三种变种形式，分别与In-Context Conditioning、Cross-Attention、adaLN-Zero 相组合。

![](img/8f600954e266c77b6f9206f15f93ac98.png)

**ViT的核心组件**

**对应Diffusion Transf**ormer模型架构图中由右到左的顺序：

1.  上下文条件（In-context conditioning）： 这是模型处理输入数据的一种方式，允许模型根据给定的上下文信息生成输出。

1.  **交叉注意力块（Cross-Atte**ntion）： 该模块允许模型在生成过程中关注输入数据中的特定部分，从而提高生成的准确性。

1.  **自适应层归一化块（Adaptive Layer Nor**malization, AdaLN）： 这是一个归一化技术，有助于加速模型的训练并提高性能。

![](img/a6f7db755051d037717415e8e904e851.png)

**DiT的三种变形形式**

**Di**T的工作流程： 通过引入噪声并训练神经网络来逆转噪声增加的过程，结合Transformer模型，实现图像或视频的生成与变换。这个过程涉及数据预处理、噪声引入、模型训练以及最终的图像或视频生成。

1.  **数据预处理： **将输入的图像或视频数据转换为模型可以处理的格式，如将图像切分成固定大小的patches（小块），然后将这些patches转换为特征向量。

1.  **噪声引入： **在数据预处理后的特征向量上逐步引入噪声，形成一个噪声增加的扩散过程。这个过程可以视为从原始数据到噪声数据的转换。

1.  **模型训练： **使用引入了噪声的特征向量作为输入，训练Diffusion Transformer模型。模型的目标是学习如何逆转噪声增加的过程，即从噪声数据恢复出原始数据。

1.  **图像或视频生成： **在模型训练完成后，可以通过输入噪声数据（或随机生成的噪声）到模型中，经过模型的处理后生成新的图像或视频。这个生成过程利用了模型学习到的从噪声到原始数据的映射关系。

![](img/eaf1f27ca487952511668cb83b5b27f2.png)

**DiT的工作流程 **

**三、D iT 的应用  **

**Sora的定义： Sora ****模**型是一种先进的视觉技术模型， 以其独特的方式生成视频，通过逐步去除噪声来形成最终画面，使得生成的场景更加细致，并具备学习复杂动态的能力。

**视**频生成模型作为世界模拟器（Video generation models as world simulators）。

**Sora**

**So**ra的核心组件： Sora模型的核心组成包括Diffusion Transformer（DiT）、Variational Autoencoder（VAE）和Vision Transformer（ViT）。

**D**iT负责从噪声数据中恢复出原始的视频数据，VAE用于将视频数据压缩为潜在表示，而ViT则用于将视频帧转换为特征向量以供DiT处理。

1.  Diffusion Transformer（DiT）： DiT结合了扩散模型和Transformer架构的优势，通过模拟从噪声到数据的扩散过程，DiT能够生成高质量、逼真的视频内容。 在Sora模型中，DiT负责从噪声数据中恢复出原始的视频数据。

1.  Variational Autoencoder（VAE）： VAE是一个生成模型，它**能**够将输入的图像或视频数据压缩为低维度的潜在表示（latent representation），并通过解码器将这些潜在表示还原为原始数据。 在Sora模型中，VAE被用作编码器，将输入的视频数据压缩为DiT的输入，从而指导DiT生成与输入视频相似的视频内容。

1.  Vision Transformer（ViT）： ViT是一种基于Transformer的图像处理模型，它将图像视为一系列的patches（小块），并将这些patches转换为特征向量作为Transformer的输入。 在Sora模型中，ViT可能被用作预处理步骤或作为模型的一个组件。

![](img/9df4fda1ad24b396ea28529a077fe359.png)

**Sora的核心组件**

**详细了解Sora底层技术： **大模型剖析 - 一文搞懂Sora底层技术

**相关论文**

+   《 Scalable Diffusion Models with Transformers 》