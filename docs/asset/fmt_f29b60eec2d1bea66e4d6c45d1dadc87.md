
# Tokenization 标记化

The idea [`tokenization`](https://www.assemblyai.com/blog/recent-developments-in-generative-ai-for-audio/) isn’t only relevant to text; audio, images, and video can also be “tokenized” for use in `Transformer`-style architectures, and there a range of tradeoffs to consider between tokenization and other methods like convolution. The next two sections will look more into visual inputs; this blog post from AssemblyAI touches on a number of relevant topics for audio tokenization and representation in sequence models, for applications like audio generation, text-to-speech, and speech-to-text.

标记化的想法不仅与文本相关，而且与文本相关。音频、图像和视频也可以被“标记化”以用于 `Transformer` 风格的架构，并且在标记化和卷积等其他方法之间需要考虑一系列权衡。接下来的两节将更多地研究视觉输入； AssemblyAI 的这篇博文涉及音频标记化和序列模型表示的许多相关主题，适用于音频生成、文本转语音和语音转文本等应用。

## Chapter 52: VQ-VAE 第52章：VQ-VAE

The `VQ-VAE` architecture has become quite popular for image generation in recent years, and underlies at least the earlier versions of `DALL-E`.

近年来，`VQ-VAE` 架构在图像生成方面变得非常流行，并且至少是 `DALL-E` 早期版本的基础。

Resources: 资源：

+   [`“Understanding VQ-VAE (DALL-E Explained Pt. 1)”`](https://mlberkeley.substack.com/p/vq-vae) from the Machine Learning @ Berkeley blog

“理解 `VQ-VAE`（`DALL-E` 解释第 1 部分）”来自 Machine Learning @ Berkeley 博客

+   [`“How is it so good ? (DALL-E Explained Pt. 2)”`](https://mlberkeley.substack.com/p/dalle2)

“怎么这么好？ （`DALL-E` 解释第 2 部分）”

+   [`“Understanding Vector Quantized Variational Autoencoders (VQ-VAE)”`](https://shashank7-iitd.medium.com/understanding-vector-quantized-variational-autoencoders-vq-vae-323d710a888a) by Shashank Yadav

“理解矢量量化变分自编码器 (`VQ-VAE)”作者：Shashank Yadav

## Chapter 53: Vision Transformers 第53章：视觉变形金刚

[`Vision Transformers`](https://d2l.ai/chapter_attention-mechanisms-and-transformers/vision-transformer.html) extend the `Transformer` architecture to domains like image and video, and have become popular for applications like self-driving cars as well as for multimodal LLMs. There’s a nice section in the `d2l.ai` book about how they work.

`Vision Transformers` 将 `Transformer` 架构扩展到图像和视频等领域，并在自动驾驶汽车以及多模式 LLMs 等应用中变得流行。 `d2l.ai` 书中有一个很好的章节介绍了它们的工作原理。

[`“Generalized Visual Language Models”`](https://lilianweng.github.io/posts/2022-06-09-vlm/) by Lilian Weng discusses a range of different approaches and for training multimodal `Transformer`-style models.

Lilian Weng 的“广义视觉语言模型”讨论了一系列不同的方法以及用于训练多模式 `Transformer` 风格模型的方法。

The post “Guide to Vision Language Models” from Encord’s blog overviews several architectures for mixing text and vision.

Encord 博客中的文章“视觉语言模型指南”概述了混合文本和视觉的几种架构。

If [you're](https://arxiv.org/abs/2403.09611) interested in practical large-scale training advice for `Vision Transformers`, the `MM1` paper from Apple examines several architecture and data tradeoffs with experimental evidence.

如果您对 `Vision Transformers` 的实用大规模训练建议感兴趣，Apple 的 `MM1` 论文通过实验证据检查了几种架构和数据权衡。

[`“Multimodal Neurons in Artificial Neural Networks”`](https://distill.pub/2021/multimodal-neurons/) from `Distill.pub` has some very fun visualizations of concept representations in multimodal networks.

`Distill.pub` 的“人工神经网络中的多模态神经元”对多模态网络中的概念表示进行了一些非常有趣的可视化。

<https://aigcreative.feishu.cn/sync/K4Gvd9vUQsy9Oubwqaeclu0Fnbg>
