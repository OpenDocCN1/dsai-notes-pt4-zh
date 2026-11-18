
# 奇绩大模型日报（10月 8日）

> 来源：[https://miracleplus.feishu.cn/docx/V4N3dQmwdoo2n9xbxmZcb8kNn2g](https://miracleplus.feishu.cn/docx/V4N3dQmwdoo2n9xbxmZcb8kNn2g)

## 🔉潜空间活动报名

### 潜空间系列活动重磅回归！

> 本期活动将在**10月13日** **10:00**开始，我们邀请到的嘉宾是朱亦博，他将在潜空间上海场进行分享。阶跃星辰（StepFun）系统负责人，曾任字节跳动 AI 基础设施负责人。朱亦博在大规模数据处理和人工智能算法优化方面有着深厚的背景和丰富的经验。阶跃星辰专注于通用大模型的研发，发布了 Step 系列通用大模型，包括千亿参数的语言大模型和多模态大模型，以及万亿参数的 MoE 语言大模型预览版。他将带来《从模型到集群硬件，关于全栈式 AI Infra 设计与搭建的思考的分享。

> 除嘉宾分享外，每期设置了【匹配合伙人 Cofounder Matching】环节。你可以和 GenAI 时代最有活力的创业者和研究者线下面对面交流，将有机会找到志同道合、有共同创业梦想的小伙伴。

> 报名通道已开启，欢迎扫描下方二维码报名。

![](img/546386706c30550863862b4a3c111dfb.png)

## 资讯

### 诺贝尔物理学奖破天荒颁给「AI教父」！Hinton成首位图灵奖诺贝尔物理学奖双料得主

2024年的诺贝尔物理学奖，花落John Hopfield和Geoffrey Hinton，理由是运用物理学原理训练人工神经网络。委员会宣布获奖名单时，所有人倒吸一口冷气。现在，网友已经炸锅了。

两人获得诺贝尔物理学奖的理由是，他们使用物理学工具训练了人工神经网络，这些方法是当今强大机器学习的基础。他们的工作，让如今的深度神经网络和深度学习得以诞生。

![](img/6f780e3e3b6350727cd30cb393f4ec84.png)

### COLM奖项公布：被ICLR拒稿的`Mamba`入选杰出论文

2023年，为了更好地探索语言模型领域，一批知名青年学者发起了COLM（Conference on Language Modeling）会议，旨在创建一个专注语言模型研究、改进和交流的学术平台。今年，COLM公布了2024年杰出论文奖，共有4篇论文获奖，其中引起广泛关注的包括`Mamba模型`。

`Mamba模型`的研究解决了`Transformer`在长序列处理中的效率问题。`Mamba`通过将状态空间模型（`SSM`）参数化为输入函数，根据当前token选择性传播或遗忘信息，实现了线性时间序列建模。`Mamba`在语言、音频、基因组学等多种模态中达到SOTA水平，并在语言建模中表现优于同规模`Transformer`模型。该模型的硬件感知算法将`SSM`与`Transformer`的`MLP`块融合，形成同质架构，能够支持百万token长度的推理，提升5倍推理吞吐量，并能与两倍规模的`Transformer`模型媲美。

另一篇获奖论文“Dated Data: Tracing Knowledge Cutoffs in Large Language Models“研究了`LLM`的“有效截止日期”问题，提出了一种无需访问预训练数据的方法来估计不同数据集的实际有效时间点，并揭示了数据版本错位与重复数据处理导致的知识偏移问题。

第三篇论文“AI-generated text boundary detection with RoFT”提出了在文本中检测人类与AI生成部分的边界。通过测试不同边界检测算法，作者发现基于困惑度的检测方法在处理跨领域数据时更为鲁棒，指出了现有检测方法在处理特定文本特征时的局限性。

最后一篇获奖论文“Auxiliary task demands mask the capabilities of smaller language models”探讨了任务需求对小型语言模型能力评估的影响。实验表明，任务复杂度越高，小型模型表现越差，这种“需求差距”使得模型性能不能直接代表智能水平。

COLM致力于促进语言模型技术的发展，并推动学术界与产业界的深度交流。`Mamba模型`及其他获奖论文展示了语言模型领域的前沿探索，为未来的研究指明了方向。

![](img/cfbe13431d39e4eca39c15cccb7b8fc2.png)

### 在 Google Cloud TPU 上微调 `LLaMa3.1`，成本降低 30%，并实现无缝扩展！

https://mp.weixin.qq.com/s/eJwg4GwH--9IVFedum_BnA

随着AI模型参数量的增长，对算力需求也急剧上升。例如，`Llama-3.1`的405B版本需要900GB以上的内存，对算力构成巨大挑战。为了解决这一问题，`Felafax`公司致力于简化AI训练集群的搭建，采用了性价比更高的`AMD GPU`，并通过`JAX`对`LLaMA 3.1` 405B模型进行微调。

`JAX`是一个强大的机器学习库，它结合了类似`NumPy`的API、自动微分功能和Google的`XLA`编译器，在非英伟达硬件上表现优异。使用`JAX`可以在多种硬件设备上高效运行，而无需修改代码。这种硬件无关的设计使`JAX`成为非英伟达硬件上的最佳选择。相比之下，`PyTorch`在迁移至`AMD GPU`或`TPU`时，需要更多适配工作。`Felafax`利用`JAX`在8张`AMD MI300X GPU`上成功微调了`LLaMA 3.1` 405B模型。每张`MI300X`拥有192GB的`HBM3`内存，使得`LLaMA 405B`在`AMD`节点上能够高效运行。

模型微调采用了`LoRA`（Low-Rank Adaptation）方法，将所有权重和`LoRA`参数设为`bfloat16`格式。`LoRA`通过将权重更新分解为低秩矩阵，减少了可训练参数的数量，有效降低了内存开销。`LoRA`的`rank`值设为8，`alpha`值设为16，最终模型占用总显存的77%，即约1200GB。在此设置下，使用`JAX`急切模式时模型训练速度为35 tokens/秒，显存利用率达到70%。虽然由于硬件和显存限制无法使用`JIT`编译，但整体扩展性在8张`GPU`上表现接近线性。

模型分片策略是`Felafax`优化的关键。使用`JAX`的设备网格（`device mesh`）功能，可将模型的参数和计算任务分配到不同`GPU`上。在此应用中，设备网格形状为（1, 8, 1），表示数据并行（`dp`）、全分片数据并行（`fsdp`）和模型并行（`mp`）。模型的LM head（`lm_head/kernel`）张量在第一个轴上被分片到8个`GPU`，而没有设置分片规范的参数（如层归一化）则会被复制到所有设备上。

训练过程中，`Felafax`还对`LoRA`参数进行了分片策略优化。`LoRA`的A矩阵参数沿着`fsdp`轴分片到8个设备，而B矩阵则沿着`mp`轴分片，减少了通信开销，增强了训练并行性。最终，该策略在训练`LLaMA 405B`模型时，仅计算`LoRA`参数的梯度，保持主模型权重不变，从而降低了内存使用并加快了训练速度。

https://github.com/felafax/felafax

![](img/e37d9d5082235ad4d12956a76d5bd265.png)

### 商汤 `SensePPL` 深度调优实践

https://mp.weixin.qq.com/s/KhWLmWugFmTswUOj8JkIIg

端侧大模型在2024年迎来了广泛关注，手机、汽车、PC等领域的AI应用逐渐成熟。各大厂商如苹果、vivo、OPPO、小米纷纷推出端侧AI系统，理想汽车和小米汽车也推出了自研语音大模型，AIPC逐步成为热点。端侧推理的快速发展得益于大模型的小型化及硬件算力的迭代，如`LLAMA3`、`Phi-3`、`Mini CPM`模型与高通、联发科的AI芯片，为端侧推理提供了坚实的技术基础。

端侧推理流程可分为`prefill`和`decoding`阶段。`prefill`阶段以计算密集型操作为主，`decoding`阶段则是访存密集型操作。评估推理系统性能的常用指标包括吞吐量、每秒请求数、首次延迟和解码延迟。云侧推理侧重提升吞吐量和并发处理能力，而端侧推理则更关注首次延迟和整体响应时间的优化。端侧推理面临内存占用、延迟、模型效果和跨平台兼容性四大挑战。

优化端侧推理的显存占用是关键，采用`W4A16`量化方案，并结合`AWQ`量化技术可有效降低权重尺寸，提升量化精度。`KV Cache`采用`INT8`量化方案，并对算子进行优化融合，通过减少量化和反量化过程中的开销实现更高效的`MHA`计算。`prefill`阶段主要优化矩阵乘法，`decoding`阶段优化矩阵向量乘法。在`GPU`资源受限情况下，结合`GPU`内存特性使用`n`连续、`k`连续和`K8N4`等算法策略，选择适合不同数据规模的算法方案。

端侧推理在跨平台兼容性上也面临挑战。芯片种类繁多，包括手机、车机和PC的`CPU`、`GPU`、`NPU`等异构架构。为此，端侧推理框架需针对不同硬件架构进行适配，并根据硬件特性和模型参数选择合适算法及分块策略。实际应用中，通过离线处理和`Kernel`编译进一步提升系统兼容性。

`Serving Pipeline`优化在端侧推理中扮演重要角色，从接收用户输入到分词、推理、采样，再到输出的整个流程，通过`Serving`层进行统一管理，并实现`KV Cache`管理与多轮对话优化。多轮对话优化可以在`KV Cache`中拼接历史prompt与回答，减少整体推理过程中的计算开销。

在应用场景中，`SensePPL`已在高通和MTK的旗舰机型中得到验证。`prefill`和`decoding`性能分别提升30%，内存占用降低50%。在WAIC大会上，展示的多模态模型部署方案在`LLM`性能上比开源方案提升了三倍。未来，`SensePPL`将继续拓展至`AIPC`和机器人领域，推动AI技术更广泛地普及和落地。

![](img/007812d838c3f839cac1995537415876.png)

### Ceph：20 年前沿边缘存储的发展历程

https://mp.weixin.qq.com/s/58WsOP9hZGxE6jw2CKK7yg

在2023年OpenInfra Summit Asia大会上，`Ceph`的使用率已达到82%，成为全球范围内广泛应用的数据存储解决方案。`Ceph`最初是加州大学圣克鲁斯分校一个学生项目，由Sage Weil作为博士研究的一部分启动，逐渐发展成为业界领先的分布式存储系统。`Ceph`具备独特的技术特性，如分布式对象存储、数据与元数据解耦、动态分布式元数据管理和`CRUSH`算法，使其能够在多个节点上横向扩展，提供一致可靠的存储服务。

`Ceph`采用`RADOS`作为基础架构，引入`CRUSH`算法来确定性地放置数据，消除集中式分配表的需求。它通过动态子树分区（`DSP`）自适应地分配元数据管理，实现元数据和数据的分离，提升了系统可扩展性。`Ceph`的设计理念将更多智能推向边缘设备，与当时的主流存储解决方案如`Lustre`和`GFS`区别开来，强调自主管理和单点故障避免。

在2007至2011年，`DreamHost`为`Ceph`的发展提供了关键支持，核心组件逐步稳定并引入了大量新特性。随着`Inktank`的成立，`Ceph`的企业级支持进一步增强，并最终被红帽收购，推动`Ceph`成为企业级生产系统。2011年，`Ceph`被纳入`Linux`内核`2.6.34`主线版本，性能大幅提升。`Ceph`的重大突破之一是引入`BlueStore`，它通过直接管理SSD和HDD设备，避免传统文件系统的性能瓶颈。`BlueStore`采用嵌入式`RocksDB`键值数据库存储元数据映射，提升了数据存取效率和可靠性。

`Ceph`在存储数据的可靠性和恢复能力上表现出色。不同于传统的“磁盘复制”或“对象复制”方法，`Ceph`采用对象分组机制，即使发生故障，也能在数分钟内快速重建数据，显著缩短了恢复时间。`Ceph`的创新理念和技术特性使其在企业和云基础设施中得到广泛应用，并在人工智能和机器学习领域也具备潜力。2018年，`Ceph`基金会成立，进一步推动其生态发展。未来，`Ceph`将在数据存储领域继续发挥重要作用，为各类应用场景提供高效可靠的存储解决方案。

![](img/2dfa4d4ab0a9f2f18d43fa639a815ca3.png)

## 推特

### Sully分享使用OpenAI `o1`的工作流：如何正确使用`o1`来看到真正的魔力

贡献人：

https://x.com/SullyOmarr/status/1843425434501091718

很多人发现 `O1` 有点难用。

所以我制作了一段视频，展示了我经常使用的工作流程。

主要要点：

+   `O1` 非常智能，但它需要上下文才能发挥作用。

+   不要像用 `GPT-4` 那样（来回对话）使用它，那样是浪费时间。

+   先构建上下文（语音模式），创建一个大文档，然后用另一个模型进行优化。

+   最后把所有内容交给 `O1`，这样才能看到真正的魔力。

完整视频在下方。

![](img/9c077a608ab821b9d06e44b74f2c959f.png)

[ssstwitter.com_1728379408109.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Gj7SbhXoLoT9APxubv2cqih4nld)

### `Supervision-0.24.0`：按类别统计线的穿越次数

贡献人：

https://x.com/skalskip92/status/1843321679210279043

`Supervision-0.24.0` 正式发布！

你终于可以按类别统计线的穿越次数了。很多人一直在要求这个功能，现在我们终于实现了！

我用 `Supervision` 花了不到 30 分钟就做了这个演示！

链接: [roboflow/supervision](https://github.com/roboflow/supervision)

![](img/3dc1a7c8fde4fbb320eda1d8f458ef1d.png)

[ssstwitter.com_1728317637663.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Gjn1blP5DohXNlxt7nEcek7nnqf)

### `LeLaN`：从野外视频中学习语言条件导航策略

贡献人：

https://x.com/svlevine/status/1843181197163483495

我们的新论文关于使用 `YouTube` 视频进行语言条件导航学习已经发布！通过利用预训练模型和从网络中获取的视频数据，我们可以让机器人更好地理解语言指令。

---

很高兴分享我们最近的研究 `LeLaN`，它用于从野外视频中学习语言条件导航策略，由加州大学伯克利分校和丰田北美共同研究。我们将在 `CoRL 2024` 上展示 `LeLaN`。

![](img/21ff3b2af9e659faf736e05a3ae58031.png)

[ssstwitter.com_1728379557092.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/CApnbJSjVowBulx8IRlcbtmEnQd)

### Imrat整理Lex Fridman 播客与 `Cursor` 团队中10个片段

贡献人：

我刚看了 Lex Fridman 播客与 `Cursor` 团队的第一小时内容。

我整理了其中我最喜欢的 10 个片段，并剪辑了播客的部分内容，放在下面。

如果你也想看第二小时的内容，告诉我哦。

总结一下：这是关于 AI 编程最深入的讨论之一。不仅仅是 `Cursor` 粉丝，任何对 AI 编程感兴趣的人都应该看。

无论你是使用 `Claude`、`O1`、`Aider`、`GPT Engineer`、`Copypasta`、`Amazon CodeWhisperer` 还是 `Replit Agent`，这里都有你能学到的东西。

我有遗漏什么吗？你最喜欢的部分是什么？

![](img/0e1c99777b53577e73662014a0277c37.png)

[ssstwitter.com_1728379651192.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/E5euboLaVoFxDKxUnxdcjItSnng)

更多在原帖中～

### RunwayML分享：为生成模型制定的安全性、公平性和诚信度的保障措施

贡献人：

https://x.com/runwayml/status/1843285078002061611

随着我们不断构建能够提升人类创造力、支持艺术家、增强媒体和娱乐行业的通用世界模型，我们也更加深刻地感受到构建对世界产生正面影响的工具的责任感。

今天，我们分享了我们为生成模型制定的安全性、公平性和诚信度的保障措施，旨在防止我们工具的滥用。

链接: [runwayml.com/research/foundations-for-safe-generative-media](https://runwayml.com/research/foundations-for-safe-generative-media)

![](img/0a94b0ae7be77882fd65196ccd5e7b36.png)

## 信号

### `VideoGuide`: Improving Video Diffusion Models without Training Through a Teacher's Guide

贡献人：

原文链接：https://arxiv.org/abs/2410.04364
