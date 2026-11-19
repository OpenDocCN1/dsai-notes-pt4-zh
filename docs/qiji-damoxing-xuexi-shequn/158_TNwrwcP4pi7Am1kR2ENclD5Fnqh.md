
# 奇绩大模型日报（7月 10日）

> 来源：[https://miracleplus.feishu.cn/docx/DeqddStVcoRferxWrq8cdsfwnfb](https://miracleplus.feishu.cn/docx/DeqddStVcoRferxWrq8cdsfwnfb)

## 资讯

### ControlNet作者新项目爆火：仅一张图生成25秒绘画全过程！不到一天GitHub揽星600+

https://mp.weixin.qq.com/s/ssboe4kZL1RHMu_1hduf0Q

据GitHub主页上的介绍，`Paints-Undo`的命名有“其绘制过程就像是在绘画软件中多次按下撤消（`Undo`）按钮”之意。

该项目包含一系列模型，展示了各种人类绘画行为，比如素描、勾线、上色、阴影、变形、左右翻转、调整色彩曲线、改变图层透明度，甚至在绘画过程中改变整体想法等等。

目前发布了两个型号：`paints_undo_single_frame`和`paints_undo_multi_frame`。

`paints_undo_single_frame`为单帧模型，基于`SD1.5`架构进行了改进，输入为一张图和一个`operation step`，并输出一张图。

`operation step`相当于`Ctrl+Z`（撤销）多少次，比如`operation step`为100，就相当于会得到一张`Ctrl+Z` 100次的效果图。

`paints_undo_multi_frame`是多帧模型，基于`VideoCrafter`打造，但没有使用原始Crafter的`lvdm`，所有的训练/推理代码都是从头开始完全实现。

![](img/8647e5c8550d4dde8869897d3819fc27.png)

[`v1_x264.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/OG9wb7OS7oC9rax39SEcKap1nWf)

### Claude编程支持一键共享，第一批网友已经开始晒作品了

https://mp.weixin.qq.com/s/CbRsZ74sgWS8Vn6pIhVx9w

Claude 3.5上新的“工坊模式”（`Artifacts`）再次更新，写完的网页应用支持一键分享了！不用自建服务器，不用部署后台程序，点一下就能自动获得链接。而且被分享的应用还能一键导入自己的对话，分分钟魔改个新版本出来。这也是继3.5这个大版本更新和“工坊模式”上线以来，Claude给广大网友带来的又一次惊喜。

新功能的接连发布，也让网友开始纷纷cue起OpenAI，表示OpenAI你在干啥，现在两家的对比实在是太鲜明了。

![](img/327a5606d53a7fad352cbc2167d5d0c8.png)

[ssstwitter.com_1720576097831.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/E3ALbjGA3oHyiwxSDAVcKHmonNg)

### 人人可做提示工程师！Claude上新：一键生成、测试和评估prompt

https://mp.weixin.qq.com/s/KL40M1bR3EPlAGWYYEghCg

在构建 AI 应用时，`prompt` 质量对结果有着重大影响。但制作高质量的 `prompt` 具有挑战性，需要研究者深入了解应用需求，并具备大型语言模型方面的专业知识。为了加快开发速度并改善结果，AI 初创公司 Anthropic 简化了此流程，可以让用户更轻松地制作高质量的 `prompt`。

具体来说，研究者为 Anthropic Console 添加了新功能，可以生成、测试和评估 `prompt`。

Anthropic 提示工程师 Alex Albert 表示：这是他们过去几周投入了大量工作才得到的成果，现在 Claude 在提示工程（`Prompt Engineering`）方面表现得非常好。

![](img/c1117c224fd0a279f58e2e01b47c0d61.png)

### WAIC观察：隐私计算加速落地产业，全新的技术标准体系呼之欲出

https://mp.weixin.qq.com/s/uFATm6uEP4Wg6PYJFMdVbQ

为此，基于《网络安全法》、《个人信息保护法》中设置的「个人信息匿名化条款」指导，对外经济贸易大学、大数据技术标准推进委员会和蚂蚁集团共同撰写了《个人信息匿名化制度：技术与法律（2024）》白皮书。

这是学术与产业界首次联合从技术与法律双重维度对个人信息匿名化问题做系统性梳理与阐释、探寻可落地技术方案与数据流通解决路径。

对外经济贸易大学数字经济与法律创新研究中心主任许可表示，匿名化制度的存而不用已经成为当前推动数据流通复用、发挥数据要素乘数效应的重大障碍之一。

![](img/502db7b2f28121891f6005cb6a675ba6.png)

### DeepMind新方法：训练时间减少13倍，算力降低90%

https://mp.weixin.qq.com/s/8rkE6Rp2yw31gw0XhFcZXg

`JEST`是一种用于联合选择最佳数据批次进行训练的方法。它就像一个智能的图书管理员，在一大堆书（数据）中挑选出最适合当前读者（模型）阅读的几本书（数据批次）。

这样做可以让读者更快地学到知识（训练模型），还能节省时间（减少迭代次数）和精力（减少计算量）。研究显示，`JEST`大幅加速了大规模多模态预训练，与之前的最先进水平（`SigLIP`）相比，迭代次数和浮点运算次数减少了10倍。

![](img/4bb467bb9c99825a95f9ca1c1df42b1f.png)

## 推特

### Lilian Weng新文章：大语言模型的外在幻觉

https://x.com/lilianweng/status/1810916954133385382

在7月4日假期期间，写了关于外在幻觉的文章。https://lilianweng.github.io/posts/2024-07-07-hallucination/

以下是`ChatGPT`建议的有趣推文内容：

🚀 探索AI幻觉的奇妙世界！

🤖 了解`LLM`如何创造出一些极具创意（有时也很荒谬）的内容。

🌍 学习其成因，发现其特点，并

✨ 探索将AI保持在现实中的最新方法。

![](img/6a12d061f64091131272e354a7ebd6cc.png)

### Rachitsky：人工智能距离取代产品经理比我预期的更近

https://x.com/lennysan/status/1810686158201901488

人工智能距离取代产品经理还有多远？

比我预期的更近。

与 `@hammer_mt`（专业提示工程师和O'Reilly《生成式AI提示工程》一书的合著者）合作，我们开始制定一个基准，来跟踪AI在最常见和最困难的产品经理任务中的表现。在我们的首次尝试中，AI在三分之二的任务中击败了人类，包括制定产品战略 😮

查看我们的方法和令人惊讶的结果：https://lennysnewsletter.com/p/how-close-is-ai-to-replacing-product

![](img/ec5569490865f0dc9b9c802f877cd7e2.png)

### Balaji分享Claude Demo：“以100倍的速度从文档到设计再到原型”

https://x.com/balajis/status/1810338989544816679

是的。现在我们可以以100倍的速度从文档到设计再到原型。

![](img/2117688dc374c1660f9ea7a672a9b808.png)

[ssstwitter.com_1720605087323.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/PY2ybFNBzo0PETxTOw9cETejnZg)

### Aman Arora分享关于Gemma 2新博客：分组查询注意力、滑动窗口注意力、旋转位置嵌入(RoPE)、对数软上限和模型合并

https://x.com/amaarora/status/1810447884531466256

我很兴奋地分享一篇关于Gemma 2的新博客文章，其中详细介绍了：

分组查询注意力、滑动窗口注意力、旋转位置嵌入(`RoPE`)、对数软上限和模型合并。

所有这些都附有易于理解的`PyTorch`实现！

这篇博客文章详细介绍了Gemma 2的架构组件，包括分组查询注意力、滑动窗口注意力、旋转位置嵌入(`RoPE`)、对数软上限和模型合并，并提供了易于理解的`PyTorch`实现。分组查询注意力通过减少键值头的数量来降低参数量；滑动窗口注意力将矩阵分块以减少计算量；`RoPE`在自注意力中引入相对位置依赖；对数软上限提高了训练稳定性；文章还讨论了模型合并技术和指数移动平均(`EMA`)。这些创新有助于提高模型性能和效率。

![](img/1a99ee8b781b2871ac44a9a9bf163174.png)

### Anole：第一个开源的、自回归的原生LMM，用于多模态生成

https://x.com/stefan_fee/status/1810695036432232576

大型多模态模型的羊驼时刻!我们能否像`Llama`那样构建用于简单多模态生成的原生`LMM`?

介绍`Anole`:第一个开源的、自回归的原生`LMM`,用于多模态生成。

基于`@AIatMeta`的`Chameleon`构建:

https://github.com/GAIR-NLP/anole

![](img/be916f7629dd1707f8f41feb54e6ae16.png)

## 论文

### 数据，到处都是数据：预训练数据集构建指南

贡献人：

链接：http://arxiv.org/abs/2407.06380v1

近期语言模型的令人印象深刻的能力主要归功于庞大的训练数据集。然而，模型开发者未披露构建方法，导致对有效训练集开发缺乏开放信息。为解决此问题，我们首次进行了系统研究，涵盖整个训练集构建流程。我们对现有的训练集开发技术进行消融实验，确定哪些方法对下游评估的模型准确性产生最大增益。然后，我们对最广泛使用的数据来源——网络爬虫快照进行了分类，包括毒性属性、质量、语言类型和领域。最后，我们展示了如何利用这些属性信息进一步完善和提升训练集的质量。这些发现提供了从业者可以使用的一系列可操作步骤，用于开发高质量的训练集。

![](img/9311789c12243e6d4a0eba2cc39dd993.png)

### 熵定律：数据压缩和LLM性能背后的故事

贡献人：

链接：http://arxiv.org/abs/2407.06645v1

数据是`LLM`的基石，但并非所有数据都适用于模型学习。精心选择的数据可以更好地唤起`LLM`的能力，而计算开销则少得多。大多数方法集中于评估数据选择中单个样本的质量，而忽略了样本之间的组合效果。即使每个样本的质量都很好，它们的组合可能由于固有的同质性或矛盾而无法最优地教导`LLM`。本文旨在揭示`LLM`性能与数据选择之间的潜在关系。受`LLM`信息压缩特性的启发，我们揭示了一种将`LLM`性能与数据压缩比和第一轮训练损失联系起来的“熵定律”，这两个指标分别反映了数据集中信息冗余和内在知识的掌握程度。通过理论推导和实证评估，我们发现模型性能与训练数据的压缩比呈负相关，通常会产生更低的训练损失。基于熵定律的发现，我们提出了一种非常高效且通用的数据选择方法，命名为`ZIP`，旨在优先考虑呈现低压缩比的数据子集。通过以贪婪方式选择多样化数据的多阶段算法，我们可以获得具有令人满意多样性的良好数据子集。进行了广泛实验验证熵定律和`ZIP`在不同`LLM`主干和对齐阶段的优越性。我们还提出了熵定律的一个有趣应用，可在模型训练初期检测潜在性能风险。

![](img/599f02341ce92cc2682b7fde86a2b2dc.png)

### `B'MOJO`：混合状态空间模型具有 eidetic 和 fading 记忆

贡献人：

链接：http://arxiv.org/abs/2407.06324v1

我们描述了一系列支持转导推理的架构，允许内存增长到有限但先验未知的界限，同时有效利用有限资源进行推理。当前的架构使用这些资源来在有限跨度内（在`Transformer`中称为“上下文”）或在无限跨度内逐渐消失（在`State Space Models`或`SSM`中）表示数据。最近的混合架构已经结合了eidetic和fading memory，但存在限制，不允许设计者或学习过程无缝调节两者，也不能扩展eidetic memory跨度。我们利用随机实现理论的思想开发了一类模型，称为`B'MOJO`，可以在一个基本的可组合模块内无缝组合eidetic和fading memory。整体架构可以用来实现可以访问短期eidetic memory“上下文中”的模型，永久结构性memory“权重中”的模型，逐渐消失的memory“状态中”的模型，以及长期eidetic memory“存储中”的模型，通过本地化地从异步更新的内存中检索。我们展示了`Transformers`，现有`SSM`s（如`Mamba`）和混合架构（如`Jamba`）都是`B'MOJO`的特例，并描述了一种基本的开源实现，在硬件中可以高效堆叠和扩展。我们在转导推理任务上测试了`B'MOJO`，例如联想召回，在此任务中它优于现有的`SSM`和混合模型；作为基准，我们测试了普通语言建模，在此任务中，`B'MOJO`在参数相似的`Transformer`和`SSM`上达到了相当的困惑度，而且训练速度提高了最多10％。最后，我们展示了`B'MOJO`调控eidetic和fading memory的能力在更长序列上的推理效果更好，测试的最长序列长度为32K token，是训练时最长序列长度的四倍。

![](img/d15a442fa81f029c6ecab12fde04a18c.png)

### 上下文学习中的模式匹配的关键机制：Induction Heads

贡献人：

链接：http://arxiv.org/abs/2407.07011v1

`LLM`（大语言模型）通过上下文学习展示了学习和执行复杂任务的显著能力。但是，对其内部机制的全面理解仍然缺乏。本文探讨了感应头在少样本上下文学习设置中的作用。我们在抽象模式识别和NLP任务上分析了两种最先进的模型，`Llama-3-8B`和`InternLM2-20B`。结果显示，即使对感应头进行最小程度的消融，也会导致抽象模式识别任务的ICL性能下降高达~32％，接近随机水平。对于NLP任务，这种消融显着减少了模型从例子中受益的能力，使少样本下的ICL性能接近于零样本提示。我们进一步使用注意力击败来禁用特定的感应模式，并提供了对感应机制在ICL中发挥作用的细致证据。

![](img/be00e10dfa11a2a3697a778b13df4bb8.png)

## 产品

### Threado AI

贡献人：

https://www.threado.com/

Threado 是一家提供 AI 驱动的客户支持解决方案的公司。他们的产品包括可训练的聊天机器人、集成到工具中的代理助手，以及连接多个数据源的智能知识中心。

![](img/a7990e9329fdcfaa1d8d90382e30af61.png)

### Shram

贡献人：

https://www.shram.io/

Shram 是一款游戏化的工作管理工具，让工作管理变得更有趣、有意义和富有成就感。它不仅可以管理项目和任务，还能提供每个团队成员绩效的实时报告，将工作效率与工作成果联系起来，而不仅仅依赖工作时长。Shram 通过任务审批、经验值奖励等游戏化机制，帮助管理者更好地管理团队，也帮助员工更好地管理自己的工作和绩效。

![](img/283f99a8e05416c45474dbfaf911f414.png)

## HuggingFace&Github

### `aints-Undo`

贡献人：

https://github.com/lllyasviel/Paints-UNDO

`PaintsUndo` 是一个提供人类绘画行为基础模型的项目，包含单帧模型和多帧模型两种，可以结合使用生成模拟人类绘画过程的视频。单帧模型输入图像和操作步骤，输出模拟"撤销"操作的中间状态图像；多帧模型输入两张图像，输出 16 个中间帧以生成更连贯的视频。这两个模型都基于 `Stable Diffusion` 和 `VideoCrafter` 进行了大量修改和训练。

![](img/60515db3a4e7fdc14a9ee89879aaafef.png)

[`1a3e29ee-9595-405f-ad70-d5ca31e4f966_x264.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Br81b4BDMoo30uxcLkxcEXNhnKf)

### KVQuant

贡献人：

https://github.com/SqueezeAILab/KVQuant

`KVQuant` 是一种用于高效 KV 缓存量化的方法，希望解决长上下文长度推理时的内存瓶颈问题。它通过考虑不同 `LLM` 中缓存 `KV` 值的一些一致模式，并开发了利用这些模式的方法，实现了低精度 `KV` 缓存量化的高精度。这些方法包括：针对键的每通道、预 `RoPE` 量化、非均匀量化（`NUQ`）以及稠密和稀疏量化。使用 `KVQuant`，可以在单个 `A100-80GB` GPU 上高效地推理 `LLaMA-7B` 模型，甚至可以在 8 GPU 系统上推理 10M 上下文长度的 `LLaMA-7B` 模型。

![](img/f82b846a5ec0583b8f78d8d7fdbf3361.png)

## 投融资

### Accend获320万美元种子融资

贡献人：

https://www.finsmes.com/2024/07/accend-raises-3-2m-in-seed-funding.html



# 公司融资动态

Accend是一家总部位于纽约的公司，专注于帮助金融科技公司和银行加速业务客户的入驻流程。该公司获得了320万美元的种子轮融资，由Adverb Ventures领投，参与者包括Y Combinator、General Catalyst、645 Ventures以及来自Brex、Stripe和Carta的天使投资人。融资将用于扩展运营和开发工作。Accend利用高度可定制的AI模型，为客户提供深入的行业风险洞察，并有效提取财务文件数据，服务对象包括Slope、Pleo和Pliant等欧美客户。

公司官网：https://www.withaccend.com/

![](img/4c33c640249cb22ebc3ccf3a3a8e1596.png)

## Enso获600万美元种子融资，计划为中小企业引入AI代理

贡献人：

https://techcrunch.com/2024/07/09/with-6m-in-seed-funding-enso-plans-to-bring-ai-agents-to-smbs/

Enso是一家由RapidAPI联合创始人Mickey Haslavsky创办的公司，旨在为中小企业（SMB）提供预编程AI代理以处理重复性任务。该公司宣布获得600万美元种子轮融资，由NFX领投，天使投资人包括Google Research负责人Yossi Matias和前红杉资本GP Shmil Levy。Enso提供70多个行业的AI代理，帮助客户进行SEO管理、Instagram互动、竞争对手追踪等，并以每个代理每月29至79美元的价格提供服务，旨在将企业级AI普及到中小企业。

公司官网：https://enso.bot/

![](img/b95650bbc8cd995332b5f6090b24ebca.png)

## DreamFlare AI视频生成平台获得投资

贡献人：

https://techcrunch.com/2024/07/09/dreamflare-ai-generated-video-platform/

DreamFlare AI是一家由前Google员工Josh Liss和纪录片制片人Rob Bralver共同创立的初创公司，旨在帮助内容创作者制作和盈利短视频内容。该公司刚刚从隐形模式中脱颖而出，目前已筹集到160万美元的资金。投资方包括FoundersX Ventures。此外，DreamFlare还声称与包括迪士尼、Netflix和环球影业在内的娱乐业高管，以及“奥斯卡和艾美奖得主的电影制片人和节目制作人”达成了创意合作。

公司官网：https://www.dreamflare.ai/

![](img/abd3097cdf291b56a09da7f3eaff27cc.png)

# 学习

## [线性RNN系列] TTT (Test-Time Training) layer

贡献人：

https://zhuanlan.zhihu.com/p/707826685?utm_psn=1794147707427897344

TTT（Test-Time Training）层是一种在线性 RNN 中应用的技术，它通过在测试阶段进行权重更新，提高了模型处理序列数据的能力。TTT-linear 版本利用 Delta Rule 进行权重更新，可以实现高效并行训练。该模型使用 2D 隐藏状态表示权重，并通过梯度下降方法进行动态更新。在线性 RNN 中，TTT 层的每个 token 都被视为单独的训练样本，通过 GD 或其变体（如在线 GD、批 GD 和小批 GD）进行权重优化。文章指出，TTT-linear 模型能够更好地利用长序列上下文，因为它在更新权重时考虑了与隐藏状态的交互。然而，该模型的并行化能力受到限制，需要选择特定的 L2 loss 函数以保持线性性和并行性。尽管 TTT-linear 在理论上有其优势，但实验结果显示，它在利用长上下文方面的性能并不总是优于传统的 Transformer++ 模型。作者强调，线性架构的研究应该超越单一的 PPL 评估，关注更广泛的应用，如检索和上下文学习。

![](img/b5edb7cd536910bb99c01ae1cd0e1f50.png)

## 多模态模型(VLM)部署方法抛砖引玉

贡献人：

https://mp.weixin.qq.com/s/U1yg3qsJYfpFTmSZtM9lpA

文章详细讨论了多模态模型（VLM）的部署方法。首先，阐述了 VLM 相对于大语言模型（LLM）的发展情况，指出 VLM 的发展虽然没有 LLM 成熟，但两者结构相似，可以利用 LLM 的经验。

在部署方案部分，文章讨论了三种主流框架：`TensorRT-LLM`、`vLLM` 和 `lmdeploy`，并详细介绍了 `TensorRT-LLM` 在多模态模型部署上的实践，包括视觉端和语言端的模型转换、服务整合，以及如何处理包含 `cross-attention` 的多模态模型。

![](img/464ad667e34edb4e34138c2e16c827d7.png)

## SFT Packing详解

贡献人：

https://zhuanlan.zhihu.com/p/707329908

文章首先介绍了非 Packing 的 SFT 训练方法，区分了单轮数据和多轮数据的构建方式，并指出了多轮数据在 LLaMA Factory 中的高效处理方法。接着详细解释了 SFT Packing 的概念，即将不同的文本序列数据打包到同一个样本序列中以减少计算上的浪费。文章进一步分析了 LLaMA Factory 中 SFT Packing 的三个版本的实现细节，从最初的简单拼接方法，到最新的版本中对不同文档之间的 attention 进行隔离和 `4D mask` 的自定义应用。文章指出，尽管通过 `4D mask` 可以防止不同文档之间的交叉污染，但仍有一些问题未解决，如 `position ids` 的重置和模型输出的不一致现象。最后，作者提出了一些疑问，并提供了参考资料以供进一步讨论。

![](img/c18b283bd4b359599511504ffa4d22a5.png)

## 预训练LLM副本攻略：结构优化与超参数调整

贡献人：

https://zhuanlan.zhihu.com/p/707993464

文章首先介绍了当前深度学习领域中出现的一系列卓越的大规模语言模型，如 DeepSeek、MiniCPM、Index 等，它们不仅推动了技术进步，也增强了开源社区的活力。接着，文章详细探讨了模型结构和超参数调整的关键点。在模型结构方面，提到了 Index 的技术报告中提出的稳定的 `LM-Head` 层 ——`Norm-Head`，它能够提高模型训练的稳定性。此外，文章还讨论了模型形状的影响，指出在同样大小的前提下，适当增加深度指标更有利于模型性能的提升，但同时也会带来显存增加的问题。在超参数调整方面，文章重点讨论了 `Batch size` & 学习率的关系，包括 Scaling Law 的应用、`Batch size` 增加时如何调整学习率，以及 `Adam` 优化器下最优学习率的 “浪涌现象”。文章还介绍了学习率调度器的重要性，特别是 `Warmup-Stable-Decay`（`WSD`）调度器，它能够在任何训练阶段获得最优模型，并且在持续训练中表现出超越 `Cosine` 学习率调度器的优势。最后，文章提到了 `Batch size` 调度的实验，指出用更大的 `Batch size` 可能可以达到更低的 loss，但需进一步研究以确定其最优实践。

![](img/4cc69c337e76990d8445f4a618cf1b7b.png)

## RAGFlow 正式进入 Agentic 时代

贡献人：

https://mp.weixin.qq.com/s/ovmyZWUu2slYzApe-bb9qw

RAGFlow 从 0.8 版本开始，推出了基于图的任务编排框架，并支持前端无代码编辑，正式进入 Agentic 时代。该框架旨在克服 RAG 1.0 版本的局限，如召回精度不足、缺乏精确检索和用户意图识别等问题。RAGFlow 的 RAG 2.0 版本将重点实现端到端的搜索系统，包括数据的信息抽取、文档预处理、索引构建和检索等环节。RAGFlow 目前已经在深度文档理解和混合搜索方面做出了努力，为未来的高级功能如查询意图、查询改写和数据预处理等做好了铺垫。此外，RAGFlow 的无代码工作流编辑功能使其在企业级场景中更具优势，能够快速适应企业的特定需求，如多轮对话和复杂问答场景。RAGFlow 的技术细节体现在其对 RAG 和 Agent 相互依存关系的深化理解、对 RAG 1.0 局限性的克服、对 RAG 2.0 的探索，以及在无代码工作流编辑和多种业务系统模板提供上的创新。这些技术细节展现了 RAGFlow 在 AI Infra 领域的独特定位和未来发展的潜力。

![](img/d3d5e373e287a342d1fd6b1ea885cd1b.png)

## DataFunCon北京站精彩回顾

贡献人：

https://mp.weixin.qq.com/s/L-zPUD8oJ0WEyjJoZioGMg

DataFunCon 北京站于 2024 年 7 月 5 日至 6 日成功举办，会议聚焦大数据与大模型的融合发展，探讨智能时代的进步。来自阿里云、云器科技、飞轮科技等多家公司的超过 1000 名业界精英和行业专家分享了各自领域的最新研究成果和实践经验。会议包括主论坛、分论坛、圆桌论坛等多个环节，涵盖了数据智能落地的现在与未来、新一代数据架构、量化决策的艺术、大模型微调、大模型增强用户体验与用户服务、智能高效：大模型重塑个人助理与办公协作、数智驱动 AI 时代业务创新、数据治理、大模型驱动运营与营销策略升级、AIOps 落地实践、大模型驱动搜索与推荐、大模型塑造数据产品研发新范式、全球化视野下的多云数据架构、指标体系设计与应用、信息挖掘新篇章：RAG 检索实践、AI Agent 赋能场景革命、实时洞察，湖仓之力等多个热点话题。

## 华泰 |新马篇 - 半导体及数据中心有望迎来快速发展机遇

贡献人：

https://mp.weixin.qq.com/s/n01SO_J3o5j6sbh5yo4v4w

报告指出，东盟近 20 年一直是全球半导体出口的重要地区，其中新加坡、马来西亚和菲律宾等国家的净出口额较大。新加坡和马来西亚分别为全球前道晶圆制造和后道封测的重镇，东盟区域的半导体产业主要集中于这两个国家。新加坡半导体产业在 FDI 和本土企业的共同驱动下逐渐具备较大的前道晶圆制造产能和较强的芯片研发能力。马来西亚则以外资企业建厂驱动为主，封测领域布局较为完备。

新加坡半导体产业已形成上游设备 / 材料到后道封装测试的全环节布局，成为全球产业链不可或缺的重要一环。新加坡目前拥有从上游设计制造到下游封装测试的代表性企业，且在 2020 年以来，多家国际半导体厂商持续加大对新加坡的投资。新加坡贡献了东南亚区域主要的前道晶圆制造产能，但全球份额较小。预计到 2027 年，东南亚在格罗方德、联电等海外企业的加注投资和积极扩产的推动下，全球产能份额有望提升。

马来西亚半导体产业集群于槟城州地区，该州是全球半导体封测重镇。马来西亚对于外商投资的激励政策起源早且涉及项目丰富，半导体在内的电子电器行业为重点领域。马来西亚可以零关税出口半导体到美国等主要市场，且在地理位置优越、人口红利、良好的营收环境及政策激励等优势下，2019-2023 年马来西亚半导体产业迎来加速发展期。全球封测重镇马来西亚，半导体企业集群于槟城州地区，目前有多家国际领先的半导体公司在马来西亚有增资扩产计划，主要集中在后道封测环节。

马来西亚数据中心产业主要位于大吉隆坡地区与柔佛，柔佛州虽然运营中数据中心容量较小，但马来西亚规划的多数新建超大规模数据中心位于柔佛，因此未来容量或将远超大吉隆坡地区。受益于临近新加坡的地理位置，柔佛数据中心建设或将再上台阶。

![](img/4638b86dc47cd09be04fe803d13784f9.png)

# 声明

本文档仅供学习交流使用，版权归原作者所有，若涉侵权，请联系Jack Jin 15101136166

（欢迎在下方反馈您宝贵的意见！整理不易，觉得有用给AI日报小组点个赞👍吧！）

ChatGPT；Angela；Zijia Liu；Alex；Zeqi Tan；Tiezhen（HuggingFace）；Naixu；Lizhi；Peter；Suoman；Jack

封面生成——右脑科技

如果你有大模型相关的创业想法，欢迎来找我们聊聊～🛰️：15101136166

> 快邀请你的小伙伴们一起来读大模型日报吧！

> **【日报知识库】**这里是近期日报内容的汇总,欢迎大家查阅

> [大模型日报知识库(分享版)](https://miracleplus.feishu.cn/wiki/KapVwXfkriOBiskB0B2cUHxjnTh)

>

> 欢迎大家多多帮我们分享,转发~~

> 如果大家有任何的建议或者吐槽,欢迎随时找我聊天~

![](img/62626c583d02f5ad9c13dc5986ed050f.png)
