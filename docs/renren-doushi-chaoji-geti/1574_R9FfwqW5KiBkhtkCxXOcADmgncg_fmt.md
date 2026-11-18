
# 2023-08-09贾佳亚团队提出`LISA`大模型：理解人话「分割一切」，在线可玩-今日头条

> 来源：[https://xuqiwei1986.feishu.cn/docx/WAbndt2CYotAuyxhxtUcqUhznIf](https://xuqiwei1986.feishu.cn/docx/WAbndt2CYotAuyxhxtUcqUhznIf)
>
> 🔗 原文链接：https://www.toutiao.com/article/7265163768886837796/?log_from=547a1f0a24546_1691562026472

## 贾佳亚团队提出`LISA`大模型：理解人话「分割一切」，在线可玩

2023-08-09 11:47 · 量子位

> 金磊 整理自 凹非寺
>
> 量子位 | 👸号 QbitAI

分割一切 这事，又有一项重磅研究入局。

香港中文大学终身教授 贾佳亚 团队，最新提出 `LISA` 大模型——理解人话，精准分割。

![](img/3285c49dab42e9aaee4778a21d3e1120.png)

例如让AI看一张早餐图，要识别“哪个是橙子”是比较容易的，但若是问一句 “哪个食物维他命C最高” 呢？

毕竟这不是一个简单分割的任务了，而是需要先认清图中的每个食物，还要对它们的成分有所了解。

但现在，对于这种人类复杂的自然语言指令，AI已经是没有在怕的了，来看下`LISA`的表现：

![](img/b2e7c170f9d26f1f3613d104f12e9bb5.png)

不难看出，`LISA`精准无误的将橘子分割了出来。

再“投喂”`LISA`一张图并提问：

> 是什么让这位女士站的更高？请把它分割出来并解释原因。

![](img/7046faf8566d3131b256b0e42e3d686f.png)

从结果上来看，`LISA`不仅识别出来了“🪜”，而且也对问题做出了解释。

还有一个更有意思的例子。

许多朋友在看到这个大模型的名字，或许会联想到女子组合BLACK PINK里的Lisa。

贾佳亚团队还真拿她们的照片做了个测试—— 让`LISA`找Lisa ：

![](img/ef9712d910ae5773618a5f66ea81bf5c.png)

不得不说，会玩！

## 基于`LISA`，复杂分割任务拿下SOTA

根据发布的论文来看，`LISA`是一个多模态大模型，它在这次研究中主攻的任务便是 推理分割 （Reasoning Segmentation）。

这个任务要求模型能够处理复杂的自然语言指令，并给出精细的分割结果。

![](img/15e887a46876831b78d5f2249ccd9d79.png)

如上图所示，推理分割任务具有很大的挑战性，可能需要借鉴世界知识 （例如，左图需要了解“短镜头更适合拍摄近物体”） ，或进行复杂图文推理 （如右图需要分析图像和文本语义，才能理解图中“栅栏保护婴儿”的含义） ，才能获得最终理想的分割结果。

尽管当前多模态大模型 （例如Flamingo[1], BLIP-2[2], LLaVA[3], miniGPT-4[4], Otter[5]） 使得AI能够根据图像内容推理用户的复杂问题，并给出相应的文本分析和回答，但仍无法像视觉感知系统那样在图像上精确定位指令对应的目标区域。

因此，`LISA`通过引入一个 `<SEG>` 标记来扩展初始大型模型的词汇表，并采用Embedding-as-Mask的方式赋予现有多模态大型模型分割功能，最终展现出强大的零样本泛化能力。

同时，这项工作还创建了ReasonSeg数据集，其中包含上千张高质量图像及相应的推理指令和分割标注。

那么`LISA`这种精准理解人话的分割能力，具体是如何实现的呢？

![](img/a5c3869792efac2a7204bf069f6fd0c2.png)

首先将图像`ximg`和文本`xtxt`送到多模态-大语言模型`F` （在实验中即`LLaVA`） ，得到输出的文本结果，如果此时文本结果包含 `<SEG>` 标记，则表示需要通过输出分割预测来解决当前问题。反之，若不包含 `<SEG>` 标记，则无分割结果输出。

如果存在标记，则将 `<SEG>` 标记在多模态大模型`F`最后一层对应的embedding经过一个MLP层得到`hseg`，并将其与分割视觉特征`f`一起传递给解码器`Fdec` （其中分割视觉特征`f`由输入编码器`Fenc`对图像`ximg`进行编码得到） 。

最终，`Fdec`根据生成最终的分割结果`M`。

`LISA`在训练过程中使用了自回归交叉熵损失函数，以及对分割结果监督的`BCE`和`DICE`损失函数。

实验证明，在训练过程中仅使用不包含复杂推理的分割数据 （通过将现有的语义分割数据如ADE20K[6]，COCO-Stuff[7]以及现有指代分割数据refCOCO系列[8]中的每条数据转换成“图像-指令-分割Mask”三元组） ，`LISA`能在推理分割任务上展现出优异的零样本泛化能力。

此外，进一步使用239个推理分割数据进行微调训练还能显著提升`LISA`在推理分割任务上的性能。而且`LISA`还表现出高效的训练特性，只需在8张具有24GB显存的3090显卡上进行10,000次训练迭代，即可完成7B模型的训练。

最终，`LISA`不仅在传统的语言-图像分割指标 （refCOCO、refCOCO+和refCOCOg） 上展现出优异性能，还能处理以下分割任务情景：⑴复杂推理；⑵联系世界知识；⑶解释分割结果以及⑷多轮对话。

![](img/7939b6a9cc66af87f426122d983818fb.png)

在有复杂情景的ReasonSeg数据集上，`LISA`显著领先于其他相关工作，进一步证明其出色的推理分割能力。

![](img/a91f319b598c86d66d0303d230916a63.png)

## 在线可玩

值得一提的是，`LISA`的推理分割能力已经出了demo，可以在线体验的那种。

![](img/066d6b434c4e6d7e92833af9ec74e918.png)

操作也极其简单，只需填写“指令”，然后上传要处理的图像即可。

若是不会描述指令，Demo下方也给出了一些示例，小伙伴们也可以参照一下。

![](img/12f026d5945f806f2401751a6e02d5df.png)

GitHub地址：

https://github.com/dvlab-research/LISA

论文地址：

https://arxiv.org/pdf/2308.00692.pdf

Demo地址：

http://103.170.5.190:7860/

参考链接：

[1] Alayrac, Jean-Baptiste, et al. “Flamingo: a visual language model for few-shot learning.” In NeurIPS, 2022.

[2] Li, Junnan, et al. “Blip-2: Bootstrapping language-image pre-training with frozen image encoders and large language models.” In arXiv preprint, 2023.

[3] Liu, Haotian, et al. “Visual instruction tuning.” In arXiv preprint, 2023.

[4] Zhu, Deyao, et al. “Minigpt-4: Enhancing vision-language understanding with advanced large language models.” In arXiv preprint, 2023.

[5] Li, Bo, et al. “Otter: A multi-modal model with in-context instruction tuning.” In arXiv preprint, 2023.

[6] Zhou, Bolei, et al. “Scene parsing through ade20k dataset.” In CVPR, 2017.

[7] Caesar, Holger, Jasper Uijlings, and Vittorio Ferrari. “Coco-stuff: Thing and stuff classes in context.” In CVPR, 2018.

[8] Kazemzadeh, Sahar, et al. “Referitgame: Referring to objects in photographs of natural scenes.” In EMNLP, 2014.

— 完 —

量子位 QbitAI · 头条号签约

关注我们，第一时间获知前沿科技动态
