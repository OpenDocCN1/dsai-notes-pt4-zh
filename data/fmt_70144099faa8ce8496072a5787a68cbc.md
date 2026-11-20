
# 2.4.1 LayoutLLM：一种多模态文档布局模型和大模型结合的框架

> 来源：[https://geekbang.feishu.cn/docx/QqX8dlBM7ocrnnxsdROcwz6TnRe](https://geekbang.feishu.cn/docx/QqX8dlBM7ocrnnxsdROcwz6TnRe)

> 👨🏻 作者：余俊晖

> 📃 作者介绍：一名`NLP`炼丹师，`NLP`算法竞赛TOP选手

> 🛖 👸号：大模型自然语言处理

> ⏰ 收录时间：2024-05-28 17:09:42

> 🔗 原文链接：`https://mp.weixin.qq.com/s?__biz=Mzg4NjI0NDg0Ng==&mid=2247484743&idx=1&sn=c90fb57241cb1e8ca6ab20510b3be690&chksm=cf9dd1c6f8ea58d036168af0647c766944571ec119cfe45f699f66a91ec7b88cd149b76b81e2#rd`

关注作者👸号，获取更多AI知识

![](img/7818aecd17382fc50a6db38eb3fee623.png)

## 前言

传统的文档理解任务，通常的做法是先经过预训练，然后微调相应的下游任务及数据集，如文档图像分类和信息提取等，通过结合图像、文本和布局结构的预训练知识来增强文档理解。`LayoutLLM`是一种结合了大模型和视觉文档理解技术的单模型方法，通过多模态指令数据集的微调来提高对图像文档的理解和分析能力。

## LayoutLMv3

在此之前，先简单介绍下`LayoutLLM`的编码器`LayoutLMv3`。

概述：文本的布局信息使用了片段级别，一段文本共用一组坐标。视觉借鉴了`ViT`的方法替换`CNN`，减少了参数以及省去了很多的预处理步骤。使用了两种新的损失`MIM`和`WPA`进行预训练。

paper：`LayoutLMv3: Pre-training for Document AI with Unified Text and Image Masking`

link：`https://arxiv.org/abs/2204.08387`

code：`https://github.com/microsoft/unilm/tree/master/layoutlmv3`

### 模型结构

![](img/99e28c2e25cbd90cf4f98b4e0fcac7d8.png)

+   文本嵌入：`RoBerta` backbone

+   视觉嵌入：与`layoutLMv2`相同，与之前的单词级别的边界框不同，此处使用了片段级别的嵌入，即：块边界框。

+   布局嵌入：不再使用`CNN`网络，采用类似`ViT`思想的backbone，将图片切分成一个个的`patches`。

### 预训练任务

+   `Masked Language Modeling (MLM)`：使用`span`掩码策略，`mask`掉30%的文本`token`，`maks`的`span`长度服从泊松分布（`λ=3`）

+   `Masked Image Modeling (MIM)`：

+   用分块掩码策略随机掩盖掉40%的图像`token`，用交叉熵损失驱动其重建被掩盖的图像区域；

+   图像`token`的标签来自一个图像`tokenizer`，通过图像`vocab`将密集图像的像素转化成离散`token`，相比于低级高噪声的细节部分，更促进学习高级特征；

+   `Word-Patch Alignment (WPA)`：学习文本单词和图像`patches`之间的细粒度对齐。`WPA`的目的是预测文本单词的相应图像补丁是否被屏蔽。具体地说，当对应的图像标记也被取消屏蔽时，为未屏蔽的文本标记分配一个对齐的标签`aligned`。否则，将指定一个未对齐的标签`unaligned`。

## LayoutLLM

### 模型架构

![](img/62a27143da94f3cb95e0e9dc82edc415.png)

`LayoutLLM`主要由两部分组成：编码器（`Encoder`）和解码器（`Decoder`）。

1. **编码器**：负责对文档图像进行编码，处理视觉和布局信息。这里使用的是预训练的`VrDU`模型，特别是`LayoutLMv3`，它能够捕捉文档的布局结构和文本信息，并生成相应的特征。简单来说就是将`OCR`文本和视觉信息从文档图像中编码，生成一个最大序列长度为512的一维序列，以便输入到`Llama`模型中。

1. **解码器**：基于大型语言模型（`LLMs`），如`Llama`，它负责解释任务指令，并使用其语言理解能力来分析文档的文本内容，最终输出结果。

### VrDU Prompts

结合大模型，通过对不同的下游任务设定提示词。`LayoutLLM`能够理解不同类型的`VrDU`任务，并结合文档的特征来生成适当的响应。这种方法使得单一模型能够灵活地处理多种任务，而不需要为每个任务单独训练模型。

#### prompt格式

`prompt`格式和`Alpaca`模型的格式保持一致：

```
The previous information is about document images.
Below is an instruction that describes a task. Write a
response that appropriately completes the request.
### Instruction: {instruction}
### Response
```

#### 不同下游任务的prompt示例

+   文档分类：

```
“执行文档分类。分类标签是...”。
```

+   文档信息提取：

```
“执行文档信息提取。分类标签是... 输出格式是一组提取词及其标签，用逗号分隔。如果存在多个提取目标，使用\n作为分隔符并分割输出。”。
```

+   这个提示指导模型识别文档中的语义实体，并按照指定的格式输出提取的信息和标签。

+   文档问答：

```
“执行文档问答。问题是...”。
```

## 评价

![](img/c38ee2f36cd4e5194adc7fd3b16f0050.png)

![](img/5477c6ea18d25d8359163eaa7f028579.png)

## 总结

本文介绍了一种传统布局模型结合大模型做文档理解的方法：`LayoutLLM`。这个框架通过结合`VrDU`编码器来捕捉文档图像的特征，以及使用`LLM`作为解码器来处理任务指令，有效地提高了对文档图像的理解和分析能力。

## 参考文献

【1】`LayoutLMv3: Pre-training for Document AI with Unified Text and Image Masking`，`https://arxiv.org/abs/2204.08387`

【2】`LayoutLLM: Large Language Model Instruction Tuning for Visually Rich Document Understanding`，`https://arxiv.org/abs/2403.14252`

## 往期相关

[【文档智能】再谈基于`Transformer`架构的文档智能理解方法论和相关数据集](http://mp.weixin.qq.com/s?__biz=Mzg4NjI0NDg0Ng==&mid=2247484719&idx=1&sn=6ac0c35b90b65827d016ce7e769ea0c1&chksm=cf9dd1aef8ea58b82b9ff1d8a8ecf3cb23b176bf759a799d5dae3c44b482ca9990cf612f53ee&scene=21#wechat_redirect)

【预处理】大模型下开源文档解析工具总结及技术思考

【文档智能】多模态预训练模型及相关数据集汇总

[【文档智能】：`GeoLayoutLM`：一种用于视觉信息提取（`VIE`）的多模态预训练模型](http://mp.weixin.qq.com/s?__biz=Mzg4NjI0NDg0Ng==&mid=2247484315&idx=1&sn=ded8d3214fd1232f31be7ecb84f2aa7e&chksm=cf9dd71af8ea5e0c4f70501d8126de74d9bc97e7744038607e6912cb02e440500992104d3c6c&scene=21#wechat_redirect)

文档智能：`ERNIE-Layout`
