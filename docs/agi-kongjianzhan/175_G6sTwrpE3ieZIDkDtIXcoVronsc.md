# （图片）Embed-Photos：图片、照片搜索，就是这么容易。开源、免费。

> 来源：[https://agispace.feishu.cn/docx/YykRdu2f7owVHnxSAL0cSH9PnOd](https://agispace.feishu.cn/docx/YykRdu2f7owVHnxSAL0cSH9PnOd)

> 原创：黔岭苗七哥

> 原文：https://mp.weixin.qq.com/s/UCuGArO3zfSyzHMu2WT0YA

在**这个视觉信息爆炸的时代，**我们如何快速找到与我们心中所想相匹配的图片？今天，我要介绍一个创新的开源项目 —— Embed-Photos，它是一个基于MLX（苹果芯片）和CLIP模型的照片相似性搜索网络应用程序。

**什么是Embed-Photos**

Embed-Photos是一个强大的照片相似性搜索引擎，它利用了CLIP（Contrastive Language-Image Pre-training）模型的先进功能，通过文本描述来发现视觉上相似的图片。这不仅为摄影师和设计师提供了一个强大的工具，也为广大图片用户带来了便利。

**项目特色**

1.快速高效：基于CLIP模型，实现快速的图像搜索。

2.苹果芯片优化：专为Apple Silicon（MLX）设计，确保了卓越的性能。

3.持久化存储：通过SQLite和Chroma，实现图片嵌入的持久化存储。

4.Web界面：提供简洁直观的Web界面，便于用户交互和探索。

5.安全与隐私：注重图片服务的安全和隐私保护。

6.性能监控：内建日志记录和性能监控，帮助用户分析和优化搜索体验。

Embed-Photos项目仍在不断进化中，开发者计划引入更多特性，包括使用siglip替代clip，添加更健壮的配置选项，以及使mlx成为可选配置。

Embed-Photos的成功离不开开源社区的支持。项目基于Apple (mlx!)、CLIP模型的工作，并使用了多个开源库。我们向所有这些项目的贡献者表示最深切的感谢。

项目链接：https://github.com/harperreed/photo-similarity-search