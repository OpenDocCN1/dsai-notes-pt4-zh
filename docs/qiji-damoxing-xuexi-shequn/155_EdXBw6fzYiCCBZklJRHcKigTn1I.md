
# 奇绩大模型日报（7月 13-14日）

> 来源：[https://miracleplus.feishu.cn/docx/XMY7d57ijohxloxYwk4cgNaBn4e](https://miracleplus.feishu.cn/docx/XMY7d57ijohxloxYwk4cgNaBn4e)

## 资讯

### OpenAI新模型「草莓」曝光：强推理/长任务规划/超大规模训练！还给出AGI分级

https://mp.weixin.qq.com/s/t8qQCirCG5_JggvBOkGd🐧

OpenAI最新绝密项目曝光！知情人士透露，OpenAI正在开发一个名为“草莓（Strawberry）”的新模型，它的前身是`Q*`。其工作机制在内部被严格保密，且细节从未被外界报道过。消息称，“草莓”模型不仅在超大规模数据集上完成预训练，并且采用了一种特殊的方法。

![](img/458258954719b384cc41c8a3d93ccffd.png)

### OpenAI Lilian Weng万字长文解读LLM幻觉：从理解到克服

https://mp.weixin.qq.com/s/UGcui0rLW2Vz7y2Mt4atqA

人会有幻觉，大型语言模型也会有幻觉。近日，OpenAI 安全系统团队负责人 Lilian Weng 更新了博客，介绍了近年来在理解、检测和克服 LLM 幻觉方面的诸多研究成果。

![](img/3fc9795b4aefcdeabd80750f76745ec7.png)

### 英伟达又赚到了！FlashAttention3来了：H100利用率飙升至75%

https://mp.weixin.qq.com/s/74-p3l8uK_kkfwVCV8o32w

随着大型语言模型（LLM）加速落地，扩展模型上下文窗口变得越来越重要。然而，`Transformer` 架构的核心 —— 注意力层的时间复杂度和空间复杂度与输入序列长度的平方成正比。这使得扩展模型上下文窗口存在挑战。2022 年，一种快速、内存高效的注意力算法 —— `FlashAttention` 问世，该算法无需任何近似即可加速注意力并减少内存占用。`FlashAttention` 对注意力计算进行重新排序的算法，并利用 tiling 和重计算来显著加快计算速度，将内存使用量从序列长度的二次减少到线性。2023 年，研究团队宣布推出 `FlashAttention-2`，在算法、并行化和工作分区等方面有了显著改进。现在，来自 Meta、英伟达、Together AI 等机构的研究者宣布推出 `FlashAttention-3`，它采用了加速 Hopper GPU 注意力的三种主要技术：

+ 通过 `warp-specialization` 重叠整体计算和数据移动；

+ 交错分块 `matmul` 和 `softmax` 运算；

+ 利用硬件支持 `FP8` 低精度的不连贯处理。

`FlashAttention-3` 的速度是 `FlashAttention-2` 的 1.5-2.0 倍，高达 740 TFLOPS，即 H100 理论最大 FLOPS 利用率为 75%。使用 `FP8`，`FlashAttention-3` 的速度更是接近 1.2 PFLOPS。

![](img/33a84ac920f1a38e02eeaa9b32b15254.png)

### 豆包大模型团队发布全新Detail Image Caption评估基准，提升VLM Caption评测可靠性

https://mp.weixin.qq.com/s/rPBqdlPDSooPm_z4ChFV6w

当前的视觉语言模型（VLM）主要通过 QA 问答形式进行性能评测，而缺乏对模型基础理解能力的评测，例如 detail image caption 性能的可靠评测手段。针对这一问题，中科院、北大和字节豆包大模型团队发布了 `DetailCaps-4870` 数据集，并提出了一种有效的评估指标 `CAPTURE`，取得了开源评估指标中最高的专家评价一致性，并低成本实现了与 `GPT-Eval` 可比的效果。

![](img/f705c34a97812c20de8bdbc0be74f8bb.png)

### OpenAI 首位投资人 Vinod Khosla 最新福布斯对谈：中美技术经济战，赢得 AI 将会拥有巨大经济影响力

https://mp.weixin.qq.com/s/jn8tfp0n6dzDBk41EnffAg

近日，硅谷著名投资人和企业家 Vinod Khosla 与《福布斯》高级编辑 Alex Konrad 探讨了多个热点话题，特别是 AI 在未来社会中的变革性影响。

![](img/a532445c51363b539d5862b92d0e6d4e.png)

### 布局 IPU！继 Arm 翻盘之后，软银孙正义又收购一家英国 AI 芯片公司 Graphcore

https://mp.weixin.qq.com/s/w7dr62S78Cl8mqygXSxLgg

7 月 11 日，英国半导体公司 Graphcore 宣布，公司已被软银集团收购。根据交易，Graphcore 将成为软银全资子公司，并将继续以 Graphcore的名义运营。 Graphcore 于 2016年在英国布里斯托成立，设计了一种名为“智能处理单元”（IPU）的新型处理器，不同于英伟达 GPU，虽然两者都能加速计算，但 IPU 的架构是专为 AI 工作负载设计的。

![](img/d370cb77a68ad9f0285c7b4a645b69f6.png)

## 推特

### CAMELAI分享引入了一个知识图谱代理：从处理过的内容中提取实体之间的关系，更高效地结构化数据

https://x.com/guohao_li/status/1812183771179069573

📢 我们刚刚在 🐫 `CAMEL` 框架中引入了一个知识图谱代理。

它首先使用 `@UnstructuredIO` 处理给定的文本。然后，这个代理从处理过的内容中提取实体之间的关系，更高效地结构化数据，以便可以轻松地存储在知识图谱数据库中，例如 `@neo4j`。

🔗 尝试一下：https://colab.research.google.com/drive/1IyttIWlsJKGWtWpc-QXDGbVok8fbud_j

---

🕸️ `GraphRAG` 非常强大，可以减少 LLM 的幻觉。然而，首先你需要有知识图谱才能执行 `GraphRAG`。你知道吗，你可以通过代理轻松地从非结构化文档创建知识图谱。我们在 🐫 `CAMEL` 中引入了一个知识图谱代理，用于无缝地从非结构化数据中创建知识图谱！

![](img/4aae87eaa22b26eba69d7d02821aab7a.png)

### supervision再更新：用于面部和身体姿势关键点的默认可视化工具

https://x.com/skalskip92/status/1811771443480277101

`supervision`-0.22.0 今天发布

我们发布的一项内容是 `Mediapipe` 集成，以及用于面部和身体姿势关键点的默认可视化工具

链接：https://github.com/roboflow/supervision

![](img/3d24f91fa6c626857b6bd1f03a925842.png)

[ssstwitter.com_1720945389827.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/LW7Vb9SOQothFqxPs5tcTvQrn3d)

### Margaritelli分享：Llama3 8B在异构家庭集群上运行

https://x.com/evilsocket/status/1812110504531259900

`llama3` 8B（未量化）在一个由以下设备组成的异构家庭集群上运行：

+ iPhone 15 Pro Max

+ iPad Pro（不确定哪个版本 XD）

+ MacBook Pro（M1 Max）

+ NVIDIA GeForce 3080（视频中不可见）

+ 2x NVIDIA Titan X Pascal

很快还会支持安卓设备（我必须也添加我的 NVIDIA Shield GPU!!!!!）。

单一代码库，单一模型格式（缩减并最佳分配到每个节点以节省空间）。

所有内容（包括 iOS 代码）都在这里开源：https://github.com/evilsocket/llama3-cake ...在社区的帮助下，将这个项目在优化和支持方面提升到一个新的水平将是非常好的。我的愿景是一个分布式推理服务器，可以在任何集群拓扑中运行任何后端的任何模型——让我们一起对抗计划性报废，民主化推理！

![](img/b14488bf963bb60136baf1eec4d78ea7.png)

[ssstwitter.com_1720945683204.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/LU2Ub3rV5oNfFhxdl2xcOnYHnpg)

### Werbei分享：对少数 AI 研究人员显而易见但大多数人不愿相信的事情

https://x.com/tedx_ai/status/1811945091696853431?s=46&t=GRStLXDcUNuun8J5Noyw4Q

一些对少数 AI 研究人员显而易见但大多数人不愿相信的事情：

1. 90% 最具影响力的 AI 研究已经发布在 arxiv、x 或公司博客文章中。

1. `q*` aka strawberry = `STaR`（自学推理者）与动态自我发现 + 类似 `DSPy` 的 RAG 优化

1. 最先进的搜索 = `GoT`（思想图） + `MCTS`（蒙特卡洛树搜索） + `DSPy` / `CLIN` 启发的工具，用于构建和运行时的“自我游戏”优化 + 基于图的知识库

1. 连续学习代理是受 `GraphRAG`、`CLIN`、`DSPy` 和动态推理模块启发的巧妙图检索

1. 当这些设计模式被融入模型中，并在构建时和运行时通过人类参与进行微调和提示优化时，将实现多个数量级的改进。

虽然围绕这些模式的新基础模型可能不是实现现有开源模型显著改进所必需的……

![](img/cf7bb1632dad6b10086e3b842493556a.png)

### RunwayML分享Gen-3 Alpha多个Demo：可以模拟水、油漆、油、蜂蜜和熔融玻璃等液体

https://x.com/runwayml/status/1811751431453450449

`Gen-3 Alpha` 可以模拟水、油漆、油、蜂蜜和熔融玻璃等液体。所有这些都具有逼真的粘度、基于物理的交互性和光学效应。

![](img/2e4f418b1fa21eeecbe381c44ac09869.png)

[ssstwitter.com_1720946770182.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/VyXWbJXZeouiKFxmNZPc4b9inTf)

## 产品

### `Inrō`

贡献人：

https://www.inro.social/

`Inrō`是一款针对 Instagram 的 AI 驱动营销自动化平台，旨在帮助品牌与用户建立个性化、规模化的互动关系。它提供了高级自动化、定向私信营销、内置 CRM、无代码集成以及可定制的 AI 客户助理等功能，帮助品牌在 Instagram 上提高营销效果，增强与客户的联系。

![](img/f01393930712269837aaa65cc584ce87.png)

### `Vocabuo`

贡献人：

https://vocabuo.com/

`Vocabuo` 是一款专注于高效词汇学习的语言学习应用程序。它使用 AI 和间隔重复的闪卡方式，可以从各种内容中自动抓取单词并生成包含图像、例句和音频的闪卡。应用程序没有过多的游戏化元素，而是专注于让用户尽可能高效地复习和学习大量单词。此外，`Vocabuo` 还支持自定义复习算法和卡片类型，目前支持多种语言。

![](img/be8b028b0d76c065b38c988d8b422939.png)

## HuggingFace&Github

### `Motion-I2V`

贡献人：

https://github.com/G-U-N/Motion-I2V

`Motion-I2V` 是一个新颖的图像到视频生成框架，它通过将复杂的映射分解为两个阶段并进行运动建模来实现更一致和可控的视频生成。第一阶段使用基于扩散的运动场预测器推断参考图像像素的轨迹，第二阶段则利用运动增强的时间注意力机制有效地将参考图像特征传播到合成帧中。与现有方法相比，`Motion-I2V` 在大运动和视角变化下生成更一致的视频，并通过训练稀疏轨迹控制网络支持用户精确控制运动轨迹和区域，提供更好的可控性。此外，`Motion-I2V` 的第二阶段还支持零样本视频到视频转换，在一致性和可控性方面都优于之前的图像到视频生成方法。

![](img/84d808dd90825e2e988fd608ae6614c3.png)

论文链接：https://xiaoyushi97.github.io/Motion-I2V/

### `Crawlee-python`

贡献人：

https://github.com/apify/crawlee-python

`Crawlee` 是一个用于 `Python` 的网络爬取和浏览器自动化库。它提供了一个统一的接口来进行 `HTTP` 和无头浏览器爬取，支持自动并行爬取、错误重试、代理轮换、会话管理等功能，可以帮助开发者快速构建可靠的爬虫。`Crawlee` 还支持使用 `BeautifulSoup` 和 `Playwright` 等工具进行数据提取，适用于各种网页爬取场景。

![](img/98efa65e4a31e616b2f8d049bbc18f70.png)

## 投融资

### 2024年美国AI初创公司融资动态

贡献人：

https://techcrunch.com/2024/07/13/heres-the-full-list-of-28-us-ai-startups-that-have-raised-100m-or-more-in-2024/

2024年上半年，全球AI初创公司共获得超过355亿美元的投资。美国公司在这些融资中占据了主导地位，其中有28家初创公司单轮融资金额超过1亿美元。

1. Hebbia：7月8日完成由Andreessen Horowitz领投的1.3亿美元融资，估值达7亿美元。

1. Skild AI：7月9日宣布完成3亿美元A轮融资，估值15亿美元，投资者包括Lightspeed Venture Partners和Bezos Expeditions。

1. Bright Machines：6月25日获得BlackRock领投的1.06亿美元C轮融资，总融资额超过4.37亿美元。

![](img/016130ca7d4477ac0b22f16836658c90.png)

### Aidatify完成830万美元融资

贡献人：

https://www.coincarp.com/fundraising/aidatify-undisclosed/

2024年7月11日，`DeFi`策略聚合器`Aidatify`在首轮融资中获得830万美元，估值达到1.5亿美元。此次融资由`STIX`、`Oilinvest`、`Alter Global`和天使投资人Spencer支持。新资金将用于扩展`Aidatify`生态系统和产品。`Aidatify`利用专有的AI模型训练技术，聚焦区块链领域的数据捕获和分析，每日处理500TB数据和300万个智能合约地址。

公司官网：https://www.aidatify.com/

![](img/5049b436d5b304137af6b1ad04ca8a88.png)

## 学习

### 大视觉模型（`LVM`）解读（六）：`VIM`：使用 `ViT` 改进的 `VQGAN` 进行矢量量化图像生成

贡献人：

https://zhuanlan.zhihu.com/p/705224905

文章首先介绍了 `VIM` 的背景，即受到自然语言处理领域的启发，视觉领域也探索了使用 `GPT` 风格的模型对图片进行自回归预训练，但传统的方法如 `iGPT` 对高分辨率图像处理效率低下。`VQGAN` 通过将图片量化为离散的 latent tokens 并自回归建模，提高了图像生成的效果。`VIM` 进一步改进了 `VQGAN`，通过使用 `Vision Transformer`（`ViT`）替代原有的 `ConvNet` 作为编码器和解码器，提高了图像重建的质量和效率。`VIM` 的框架包括两个阶段：图像量化和矢量量化图像建模。在第一阶段，`ViT-VQGAN` 将图像编码为离散的 image tokens；在第二阶段，使用 `Transformer` 模型自回归预测这些 tokens，实现图像生成和理解任务。文章详细描述了 `ViT-VQGAN` 的编码器结构、训练损失函数、以及如何通过低维度的线性投影和 `ℓ2` 归一化提高码本使用率和重建质量。实验结果表明，`ViT-VQGAN` 在 `ImageNet` 数据集上的无条件和 class-conditioned 图像生成能力显著优于 `VQGAN`，并且在无监督学习任务中的 `Linear Probing` 精度也超过了 `iGPT`。最后，文章还讨论了不同模型大小和码本大小对实验结果的影响。

![](img/d874e5f7f3ab2ab3c02dd2dbb567d23b.png)

### 蚂蚁集团在大模型推荐上的算法和应用

贡献人：

https://zhuanlan.zhihu.com/p/708502339

蚂蚁集团在大模型推荐系统中应用了两阶段融合策略：首先利用大模型进行知识提取，构建结构化的知识图谱；其次，通过蒸馏技术将大模型的推理能力转移到轻量级模型，以优化推荐效果和计算效率。在知识提取过程中，通过设计高质量的 `prompt` 和采用 `KG-BERT` 排序等策略，确保生成的目标实体具有高准确性、创新性和多样性。大模型作为教师模型，通过生成式 `Loss` 微调小模型，使其能够生成推荐理由，并通过排名蒸馏确保知识迁移的有效性。



![](img/f5d6a3583c315c00ffc4a6c4024a9361.png)

## 一周AI入门到设计一款NPU芯片系列一 给芯片设计爱好者的礼物

贡献人：

https://zhuanlan.zhihu.com/p/708454487

本文旨在为 AI 和 FPGA 芯片设计初学者提供一份深入的技术指南，重点介绍了 NPU 芯片的设计原理和实现过程。作者首先指出了芯片行业的就业挑战，强调了知识分享的重要性，并对 NPU 在市场上的现状和重要性进行了阐述。文章详细介绍了 NPU 的基本概念、主要功能以及在推理任务中的应用。

文章接着深入探讨了 NPU 的设计细节，包括如何通过 `API` 接口加速卷积运算，以及如何设计一个通用的 `CNN` 加速器。作者以一个开源项目为例，详细剖析了 NPU 的关键模块，如处理元素（`PE`）、`Relu` 和最大池化模块，并提供了 `Verilog HDL` 的代码实现。这些模块的设计涉及到了多种技术细节，例如乘法器、加法器树、D 触发器的使用，以及如何处理不同大小的输入和权重。

![](img/531f8d307bd94915542116a375a60711.png)

## A Primer on Memory Consistency and Cache Coherence 翻译计划（一）：引言

https://zhuanlan.zhihu.com/p/552305284

文章作者 kaitoukito 为了系统学习内存模型相关知识，开始了对《A Primer on Memory Consistency and Cache Coherence (2nd Edition)》的翻译工作。该书的翻译计划已经移步到 GitHub，以便更多人参与和学习。文章中，作者解释了为什么需要 memory consistency model 和 cache coherence，并阐述了这两个概念的区别和联系。Memory consistency model 定义了共享内存系统正确性的精确规范，而 cache coherence 是实现该模型的一种手段。文章还概述了书中各章节的内容，包括 consistency 和 coherence 的基础知识、不同的内存一致性模型（如顺序连贯性、总存储顺序、宽松连贯性模型）、cache coherence 协议（如监听缓存一致性和目录缓存一致性协议），以及异构系统中的 consistency 和 coherence 问题。最后，作者提到了本书未涉及的内容，如同步技术、商用的宽松连贯性模型、并行编程和分布式系统的 consistency，并列出了参考文献。

## `Ascend C` 多核并行/流水计算/`double buffer`技术

贡献人：

https://zhuanlan.zhihu.com/p/708461025

文章介绍了 `Ascend C` 算子在 Ascend AI 处理器（NPU）上实现多核并行、流水计算和 `double buffer` 技术的原理和实践。首先，介绍了 `AI Core` 的硬件基础，包括标量计算单元、向量计算单元、矩阵计算单元、`Local Memory` 和 `DMA` 单元，这些是实现大规模神经网络计算加速的基础。其中，向量计算单元每个指令周期可处理 128 个 `FP16` 的加法，矩阵计算单元每个指令周期可完成两个 `16x16` `FP16` 的矩阵乘法。

接着，文章通过一个典型算子的实现，展示了如何基于 `Ascend C` 进行数据切分和流水线处理。开发者通过编写核函数，利用 `block_idx` 计算输入输出数据在 `Global Memory` 上的偏移地址，实现多核并行。通过 `TPIPE` 流水线范式，`CopyIn`、`Compute`、`CopyOut` 三个阶段可以实现流水线工作，并通过 `double buffer` 技术进一步提高流水线的执行效率。

文章还详细解释了流水计算中的 `set_flag`/`wait_flag` 指令对，它们通过实现锁机制确保数据拷贝和计算的正确执行顺序。最后，通过引入 `BUFFER_NUM` 变量和修改少量代码，可以启用 `double buffer`，使得数据搬运和计算可以重叠进行，提高了整体的计算效率。

![](img/e4e1f4177a52afd8cdaf6f87e30e1a02.png)

## 重温SSM（四）：有理生成函数的新视角

贡献人：

https://zhuanlan.zhihu.com/p/708444856

本文首先回顾了 `S4` 模型的基础，包括线性 `RNN` 的表达形式、卷积运算与生成函数的关系，以及如何通过生成函数高效计算卷积核 `K_{<L}`。接着，详细介绍了 `RFT` 方法，即将 `SSM` 模型的训练、推理和参数化转移到生成函数空间中，这种方法不再依赖于 `state size d`，能够降低计算复杂度至 `O(LlogL)`。作者进一步解释了有理函数与 `SSM` 模型的联系，并通过行列式恒等式确立了系数 `a` 和 `b` 与 `A`、`B`、`C` 的对应关系。文章还探讨了如何求解 `A`、`B`、`C`，提出了使用 “`Companion matrix`” 来构造实矩阵的方法。最后，作者讨论了初始化方式，指出在初始化时将 `a_1`、`a_2`、`...`、`a_d` 设为零能够提高数值稳定性，并且通过实验效果展示了 `RFT` 模型的优势。文章最终总结了 RTF 的计算流程简化和表达能力提升，以及如何通过增大 state size 来改善模型效果，而不增加计算复杂度。

## 浅谈寒武纪开源的`Triton-Linalg`编译器前端

贡献人：

https://zhuanlan.zhihu.com/p/706401510

寒武纪最近开源了 `Triton-Linalg`，这是一款跨平台的 AI 编译器前端，专为支持 `Triton` 语言设计，它能够帮助开发者以较低的成本快速集成特定领域架构（`DSA`）的指令集，并与上层 AI 应用对接。`Triton-Linalg` 的技术实现包括 `Analysis`、`Conversion`、`Dialect` 和 `Transforms` 四个部分，其中 `Analysis` 包括指针连续性分析算法等，`Conversion` 负责将 `Triton/Arith/Math` 转换为 `Linalg`，`Dialect` 定义了 `Linalg` 扩展算子，`Transforms` 则扩充了社区方言的标准化和优化 pass。与微软的 `Triton-Shared` 相比，寒武纪的 `Triton-Linalg` 在指针处理和算子处理上采用了不同的原则和技术，特别是在指针和 `Tensor/Memref` 的衔接以及连续性分析算法方面展现了原创性。这些技术的目的是为了优化 `DSA` 场景中的访存性能，并降低 AI 硬件集成 `Triton` 的难度。

![](img/7bb1a65a29192fee53ea9eacaa561438.png)

# 声明

本文档仅供学习交流使用，版权归原作者所有，若涉侵权，请联系Jack Jin 15101136166

（欢迎在下方反馈您宝贵的意见！整理不易，觉得有用给AI日报小组点个赞👍吧！）

ChatGPT；Angela；Zijia Liu；Alex；Zeqi Tan；Tiezhen（HuggingFace）；Naixu；Lizhi；Peter；Suoman；Jack

封面生成——右脑科技

如果你有大模型相关的创业想法，欢迎来找我们聊聊～🛰️：15101136166

> 快邀请你的小伙伴们一起来读大模型日报吧！
> `【日报知识库】`这里是近期日报内容的汇总,欢迎大家查阅
> [大模型日报知识库(分享版)](https://miracleplus.feishu.cn/wiki/KapVwXfkriOBiskB0B2cUHxjnTh)
>
> 欢迎大家多多帮我们分享,转发~~
> 如果大家有任何的建议或者吐槽,欢迎随时找我聊天~

![](img/6a7b84a3ebdd7d5d694a4c678504f40b.png)
