# 【文档智能】多模态预训练模型及相关数据集汇总

> 来源：[https://geekbang.feishu.cn/docx/IkXxdoSo1ohT7ixNPp3cvPfZnBf](https://geekbang.feishu.cn/docx/IkXxdoSo1ohT7ixNPp3cvPfZnBf)

> 👨🏻 作者：余俊晖

> 📃 作者介绍：一名NLP炼丹师，NLP算法竞赛TOP选手

> 🛖 公众号：大模型自然语言处理

> ⏰ 收录时间：2024-05-28 17:09:42

> 🔗 原文链接：h[ttps://mp.weixin.q](https://mp.weixin.qq.com/s?__biz=Mzg4NjI0NDg0Ng==&mid=2247484335&idx=1&sn=f5f9236ef8ef361d7d9cf594c472f543&chksm=cf9dd72ef8ea5e38d6f567d867c19228a2770674d14e13ad12e569abbbaf4f74329a19a95059#rd)q.com/s?__biz=Mz...

![](img/45a5e8c043ecb616b156266e53e71c74.png)

关注作者公众号，获取更多AI知识

## ** ****前言 **

大模型时代，在现实场景中或者企业私域数据中，大多数数据都以文档的形式存在，如何更好的解析获取文档数据显得尤为重要。文档智能也从以前的目标检测（版面分析）阶段转向多模态预训练阶段，本文将介绍目前一些前沿的多模态预训练模型及相关数据集。

## ** ****多模要预训练模型导图 **

![](img/cda15af9aae2103fa76d62e8e1ab4d73.png)

## ** ****LayoutLM **

概述：该模型主要针对文档信息的处理，传统的类Bert模型仅使用文本信息，该模型引入了布局信息（即文本的位置坐标）和视觉信息，并使用MVLM和MDC训练策略进行预训练。

paper：LayoutLM: Pre-training of Text and Layout for Document Image Understanding

link：https://arxiv.org/abs/1912.13318

code：https://github.com/microsoft/unilm/tree/master/layoutlm

### ** 模型结构 **

![](img/04549488e1fb4d0f43537e448b6efac0.png)

+   文本嵌入：Bert backbone

+   布局嵌入：（1）Bert的backbone增加边界框嵌入（  ，  ，  ，  ），其中（  ,  ）对应于边界框中左上角的位置，（  ,  ）表示右下角的位置;（2）[CLS]位置则用整个文本块的边界框坐标；

+   视觉嵌入(仅在finetune阶段使用)：（1）用FasterRCNN提取扫描件的视觉特征，将其与bert的输出融合；（2）利用OCR工具或pdf parser工具获取文本和文本对应的对应布局信息（bbox）；（3）[CLS]使用Faster R-CNN视觉骨架来生成视觉特征嵌入，使用整个扫描的文档图像作为 ROI 区域;

### ** 预训练任务 **

+   Masked Visual-Language Model (MVLM)：使用bert的MLM任务MASK掉token，但保留对应的布局信息，借助布局信息和上下文信息预测对应token。

+   Multi-label Document Classification (MDC)：一个可选策略，旨在对文档进行分类，模型可以对来自不同领域的知识进行聚类，并生成更好的文档级表示。

## ** ****LayoutLMv2 **

概述：相比于LayoutLM更好的融入了视觉信息，以及视觉、文档和布局信息的对齐，引入空间自注意力编码，提出两种新的损失函数TIA和TIM进行预训练。LayoutLMv1仅在fine-tuning阶段用到视觉特征，pretraining阶段没有视觉特征与文本特征的跨模态进行交互。Pretraining阶段，提出multi-modal transformer，融合文本、布局和视觉信息；在LayoutLMv1中MVLM基础上，新增text-image alignment（文图对齐）与text-image matching（文图匹配）两个预训练任务。

paper：LayoutLMv2: Multi-modal Pre-training for Visually-rich Document Understanding

link：https://arxiv.org/abs/2012.14740

code：https://github.com/microsoft/unilm/tree/master/layoutlmv2

### ** 模型结构 **

![](img/fd200092403bff7439e4ef7ccaa5636a.png)

+   文本嵌入：Bert backbone

+   视觉嵌入 ：

+   视觉特征提取backbone：ResNeXt-FPN

+   将图片统一缩放到224x224大小，经过backbone后平均池化成固定大小WxH（7x7），再展平得到长度为49的序列，再经过一个线性层映射到与文本嵌入相同的维度；

+   额外增加一维位置嵌入，与文本一维位置嵌入共享参数

+   布局嵌入：

+   除边界框嵌入（  ，  ，  ，  ）外，还增加宽w和高h嵌入;

+   布局嵌入与LayoutLM中四个坐标相加不同，这里拼接六个位置嵌入为一个输入，每个位置嵌入维度为隐层维度/6；

+   图像经backbone得到长度为49的序列，将其视为把原图切分成49个块，视觉部分的布局嵌入使用每个块的布局信息；

+   对于[CLS]， [SEP]和[PAD]，布局信息使用(0,0,0,0,0,0)表示；

#### ** 预训练任务 **

+   Masked Visual-Language Model (MVLM)：与LayoutLM相同，为了避免视觉信息泄露，在送入视觉backbone之前，将对应掩码token的视觉区域掩盖掉。

+   Text-Image Alignment (TIA)：作用：帮助模型学习图像和边界框坐标之间的空间位置对应关系。（1）随机选择一些文本行，覆盖对应图像区域；（2）在编码器最后一层设置一个分类层，预测是否被覆盖，分类标签为[Convered]、[Not Convered]，计算二元交叉熵损失；当同时被mask和cover时，不计算TIA损失，为了防止学习到mask到cover的映射。

+   Text-Image Matching (TIM)：将[CLS]处的输出表示输入分类器，以预测图像和文本是否来自同一文档页面。帮助模型学习文档图像和文本内容之间的对应关系。

## ** ****LayoutLMv3 **

概述：相较于前两个版本，LayoutLMv3文本的布局信息使用了片段级别，一段文本共用一组坐标。视觉骨架借鉴了ViT的方法替换CNN，减少了参数以及省去了很多的预处理步骤。使用了两种新的损失MIM和WPA进行预训练。

paper：LayoutLMv3: Pre-training for Document AI with Unified Text and Image Masking

link：https://arxiv.org/abs/2204.08387

code：https://github.com/microsoft/unilm/tree/master/layoutlmv3

### ** 模型结构 **

![](img/3d33ef3091d78dc27d7994831295bdfe.png)

+   文本嵌入：RoBerta backbone

+   视觉嵌入：与layoutLMv2相同，与之前的单词级别的边界框不同，此处使用了片段级别的嵌入，即：块边界框。

+   布局嵌入：不再使用CNN网络，采用类似ViT思想的backbone，将图片切分成一个个的patch，再经过展平变成序列，在此过程中，每个patch经过线性投影成与文本嵌入相同的维度。

### ** 预训练任务 **

+   Masked Language Modeling (MLM)：使用span掩码策略，mask掉30%的文本token，maks的span长度服从泊松分布(λ=3)

+   Masked Image Modeling (MIM)：

+   用分块掩码策略随机掩盖掉40%的图像token，用交叉熵损失驱动其重建被掩盖的图像区域；

+   图像token的标签来自一个图像tokenizer，通过图像vocab将密集图像的像素转化成离散token，相比于低级高噪声的细节部分，更促进学习高级特征；

+   Word-Patch Alignment (WPA)：学习文本单词和图像patches之间的细粒度对齐。具体的， MLM和MIM分别进行掩码，彼此之间没有对齐，然而， 每个文本token都对应着一个图像patch，那么 WPA的目的就是预测文本token的相应图像patch是否被屏蔽（当对应的图像标记也被取消屏蔽时，为未屏蔽的文本标记分配一个对齐的标签[aligned]。否则，将指定一个未对齐的标签[unaligned]；当文本token被掩码时，不参与WPA损失计算，防止学习mask与图像块的对应关系。）

## ** ****LayoutXLM **

概述：该模型是LayoutLMv2的多语言版本，区别就是在布局特征嵌入时，LayoutLMv2是单词级别的布局信息，由于各种语言的切分方式差异，LayoutLM使用的是字符级别的布局信息，然后通过合并一个token中每个字符的边界框来得到最终的边界框坐标。

其他结构与 LayoutLM v2， 不再详细赘述。

paper：LayoutXLM: Multimodal Pre-training for Multilingual Visually-rich Document Understanding

link：https://arxiv.org/abs/2104.08836

code：https://github.com/microsoft/unilm/tree/master/layoutxlm

![](img/2ffe6b2edc03a1ad587f5f1e00e3e340.png)

## ** ****ERNIE-Layout **

概述：通过增强布局知识的预训练模型，以学习更好的表示，结合文本、布局和图像的特征。旨在利用文档布局相关信息，进行文档理解，进一步提高文档信息抽取的性能。

paper：ERNIE-Layout: Layout Knowledge Enhanced Pre-training for Visually-rich Document Understanding

link：https://arxiv.org/abs/2210.06155

code：https://github.com/PaddlePaddle/PaddleNLP/tree/develop/model_zoo/ernie-layout

该模型之前介绍过，详细请移步《 文档智能：ERNIE-Layout 》

![](img/78fd1ae955b784ad4e899c71b0c7da90.png)

## ** ****GeoLayoutLM **

概述：GeoLayoutLM通过显式建模几何关系和特殊的预训练任务来提高文本和布局的特征表示。该模型能够提高文档信息抽取的性能。

paper：GeoLayoutLM: Geometric Pre-training for Visual Information Extraction

link：https://arxiv.org/abs/2304.10759

code：https://github.com/AlibabaResearch/AdvancedLiterateMachinery/tree/main/DocumentUnderstanding/GeoLayoutLM

[**该模型之前介绍过，详细请移步《 【文档智能】：G**](https://mp.weixin.qq.com/s?__biz=Mzg4NjI0NDg0Ng==&mid=2247484315&idx=1&sn=ded8d3214fd1232f31be7ecb84f2aa7e&scene=21#wechat_redirect)eoLayoutLM：一种用于视觉信息提取（VIE）的多模态预训练模型 》

![](img/cab01bee8075604bb2d10f2cff0c97e8.png)

## ** ****数据集 **

![](img/73bf311fe0d01b0e5f77ce2fef781b97.png)

## ** ****总结 **

本文简单介绍了文档智能领域关于多模态预训练语言模型相关内容及相关数据集，相对于基于目标检测（版面分析）的pipline形式，多模态预训练模型能够一定程度的实现端到端的提取文本内容。但实际应用还需要根据特定的场景进行进一步的研究。

## ** ****参考文献 **

【1】https://mp.weixin.qq.com/s/cw5wCpJCYo7Wdi1PtsRgGw

【2】https://mp.weixin.qq.com/s/3l4NvGfy8LaKuj_3HGH5pg

【3】LayoutLM: Pre-training of Text and Layout for Document Image Understanding

【4】LayoutLMv2: Multi-modal Pre-training for Visually-rich Document Understanding

【5】LayoutLMv3: Pre-training for Document AI with Unified Text and Image Masking

【6】LayoutXLM: Multimodal Pre-training for Multilingual Visually-rich Document Understanding

【7】ERNIE-Layout: Layout Knowledge Enhanced Pre-training for Visually-rich Document Understanding

【8】GeoLayoutLM: Geometric Pre-training for Visual Information Extraction