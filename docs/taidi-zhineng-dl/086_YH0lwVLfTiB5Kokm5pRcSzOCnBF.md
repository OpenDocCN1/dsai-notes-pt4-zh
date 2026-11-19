
# Stable Diffusion

> 来源：[https://learning.feishu.cn/docx/KifDdrJ3ToYjejxRTTYc4xXDnLe](https://learning.feishu.cn/docx/KifDdrJ3ToYjejxRTTYc4xXDnLe)

https://huggingface.co/blog/annotated-diffusion

[十分钟读懂Diffusion：图解Diffusion扩散模型](https://zhuanlan.zhihu.com/p/599887666?utm_id=0)

## 系统概述

Stable Diffusion 并不是一个单一模型，而是由多个部分和模型一起构成的系统。

1. Text feature -> image feature。一般是选了某个多模态的预训练模型，与 diffusion model 关系不大。典型的是 CLIP。
2. Image 生成。
3. 都是 diffusion model。特别是 OpenAI 的 Dall-E，一年内，从 VQ-VAE 换成了 diffusion model。
4. Backbone 都是 `UNet`，但 building block 部分的换成了 transformer block。
5. 高精大图离不开超分辨率。更具体的说：1024 * 1024 分辨率，基本都不是直接画的。

Stable Diffusion 的三个主要组成部分，每个部分都有自己的神经网络：

+ `ClipText`：用于文本编码。输入: 文本。输出: 77个token embeddings向量，每个向量有768维。
+ `UNet`+调度程序：在信息（潜在）空间中逐步处理信息。输入: 文本embeddings和一个初始化的多维数组（结构化的数字列表，也称为张量）组成的噪声。输出：经过处理的信息数组。
+ `Autoencoder Decoder`：使用经过处理的信息数组绘制最终图像。输入：经过处理的信息数组（维数：(4,64,64)）输出: 生成的图像（维数：（3,512,512），即（红/绿/蓝；宽，高）)。

在 latent space 上进行 diffusion，用 cross-attention 实现多模态训练

![](img/14dd596e285155ec6e6854bc44836b9e.png)

![](img/8b8f16049ff8ad2692fc835c831cb436.png)
