
# 奇绩大模型日报（7月 15日）

> 来源：[https://miracleplus.feishu.cn/docx/D7PCdg4XbojoxWxSQHkcDlaenJ5](https://miracleplus.feishu.cn/docx/D7PCdg4XbojoxWxSQHkcDlaenJ5)

## 资讯

### 谷歌DeepMind被曝抄袭开源成果，论文还中了顶流会议

https://mp.weixin.qq.com/s/TVAx3Z4zrah3ql4pHa8V_w

浅看一眼两篇论文的摘要对比。

谷歌DeepMind的论文说的是，tokenization给约束语言模型输出带来了麻烦，他们引入自动机理论来解决这些问题，核心是避免在每个解码步骤遍历所有逻辑值（logits）。

而“原告”的说法大致是：

提出了一个高效框架，通过在语言模型的词汇表上构建索引，来大幅提升约束文本生成的效率。简单来说，就是通过索引避免对全部逻辑值的遍历。

同样“不依赖于具体模型”。

![](img/9b9279bad780589ef79e31b64c763de2.png)

### PyTorch团队首发技术路线图，近百页文档披露2024下半年发展方向

https://mp.weixin.qq.com/s/HVI2UfawF9DDmcugHyytDg

最近，PyTorch团队首次公布了开发路线图，由内部技术文档直接修改而来，披露了这个经典开源库下一步的发展方向。

![](img/baa698b050ed03738b0e9bd247b887ad.png)

### 230 亿美元收购 Wiz！谷歌或迎来有史以来最大规模收购

https://mp.weixin.qq.com/s/H4fOO3VQMor37rYs__68rw

根据 WSJ，Google 正在与网络安全初创公司 Wiz 就 230 亿美元收购进行深入谈判。若谈判成功，将成 Google 有史以来最大规模收购。

据悉，此次收购有助于 Google 加强其在云计算领域的业务，云计算是一项重要且不断增长的业务，但旗下 Google Cloud 在这一领域落后于同行。

![](img/8dd1bdf9aad0817b246a5c2becb77d3d.png)

### 美国启动「曼哈顿计划2.0」，AI进入奥本海默时刻？60亿砸向无人机，已有800个AI项目

https://mp.weixin.qq.com/s/FwWM3VwLlbKc9jSUPiYUug

曼哈顿计划2.0来了？截止目前，美国军方已经拥有800多个活跃的AI项目，仅在24年就为AI申请了18亿美元的资金。在未来五年内，美国还将拨款60亿美元，用于无人协作战斗机的研发。现在，AI似乎已经进入了奥本海默时刻。

![](img/12b0d7b076401d39ebef01da5eb56fec.png)

## 推特

### Reddit超火Claude Sonnet 3.5的系统提示

https://x.com/rohanpaul_ai/status/1812295914566176866

非常好的系统提示 Claude Sonnet 3.5

![](img/02bfd3f1175c9c121410d1d6ca19abaf.png)

### Virat分享股票市场API，现在涵盖所有S&P 500股票代码，可连接AI金融代理

https://x.com/virattt/status/1812549169447616953

它即将上线。

我的股票市场API现在涵盖所有S&P 500股票代码。

• 收入报表

• 资产负债表

• 现金流量表

超过30年的数据。没有API限制。

你可以将你的AI金融代理连接到这些数据。

这个公开测试版将运行大约1周。

主要目标是在全面上线前进行系统负载测试。

全面上线将覆盖超过15,000只股票。

三件事：

• 创建一个账户 🙏

• 发送API请求 💻

• 来Discord闲聊 ❤️

![](img/4a576c0a33217fd1279759ad89ddffbb.png)

[ssstwitter.com_1721038687767.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/PPS5bCAADoh361x8AZfcQ4U9ngc)

### `ai-renamer`：使用LLMs 根据文件内容重命名文件的CLI工具

https://x.com/JackBlair87/status/1812619229994295544

这太棒了。有人开发了一个使用LLMs (Ollama) 根据文件内容重命名文件的CLI工具。

https://github.com/ozgrozer/ai-renamer

---

`ai-renamer`

一个 Node.js 命令行工具，使用 Ollama 和 LM Studio 模型（Llava, Gemma, Llama 等）根据文件内容智能地重命名文件。

![](img/480b8354b8ebb3484510142e4275f6c5.png)

[ssstwitter.com_1721038785676.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/TeYxbyWxooa7Q9xyHpXcECWNnJd)

### Krishan Rana 分享：如何扩展模仿学习，以应对长时间、多对象任务并从仅10次演示中进行泛化

https://x.com/krshnrana/status/1812517940790689852

📌 我们如何扩展模仿学习，以应对长时间、多对象任务并从仅10次演示中进行泛化（空间和类别内）？

✨ 介绍以可行性为中心的策略分解

💡 相对任务框架扩散策略，自我链接实现无缝操作

---

数据收集对于长时间任务来说是繁琐的，多对象设置导致的组合空间变化需要大量的演示。我们提出将策略学习分解为更简单的子任务，这些子任务可以独立收集并无缝组合，以解决长时间、多对象任务，而无需完整的长时间任务演示。我们的方法的关键是根据相对于正在操纵的对象上的相对任务框架来训练每个子策略。这提供了对象位置的空间不变性，使我们能够泛化到未见过的多对象空间变化。将相对任务框架放置在对象的可行性中心区域，使我们能够从图像状态表示中抽象出来，并学习表现出类别内泛化的策略。为了确保独立训练的子策略之间的无缝过渡，我们将每个子策略的可行性中心框架重新定向到机器人的当前工具框架，以确保机器人处于后续子策略的数据支持范围内，并训练每个子策略预测其当前任务进度，利用这一进度在子策略之间进行过渡，而无需额外的仲裁策略。这项工作受到之前在关键点提取（kPAM, DINO ViT）、姿态跟踪（Foundation Pose）和策略学习（Diffusion Policy）等通用视觉系统开发方面的显著进展的启发和支持。

![](img/dce574625078be7440431fe7432e4714.png)

[ssstwitter.com_1721038939877.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/SnxZbiMopoLfI7x19FTc0Qldnlh)

## 产品

### AI Web Designer

贡献人：

https://aiwebdesigner.io/

AI Web Designer 是一款新推出的 AI 辅助网页设计工具，简化网页设计流程，让网页设计更加高效便捷。它允许用户只需复制粘贴网站 URL，就可以使用 AI 生成网站的落地页或整个登录页面。用户可以对生成的设计进行编辑和导出，并将其应用到现有的网站上。即将推出的新功能还允许用户添加文本描述，以便 AI 根据用户的期望和目标生成更加符合需求的最终设计。

![](img/33a5d3b79c9df2180aa7bc9163968dd7.png)

### AI Image Extender

贡献人：

https://www.aiimageextender.com/

AI image extender 是一款图像编辑工具，可以帮助用户轻松地扩展图像背景。它通过 AI 技术自动分析图像内容，并根据原有背景自然地扩展图像，无需繁琐的手动裁剪。该工具提供了三种不同的纵横比选项，用户可以拖拽和调整图像大小，快速创造出适合社交媒体发布的图片。

![](img/9b834b20f5a015134598a5bd52ee11ad.png)

## HuggingFace&Github

### Strom

贡献人：

https://github.com/stanford-oval/storm

STORM 是一个基于大语言模型的知识整理系统，可以从互联网上收集信息，生成维基百科风格的文章。该系统分为两个阶段：

1. 预写作阶段：系统通过互联网搜索收集相关信息，并生成大纲。

2. 写作阶段：系统使用收集的信息和生成的大纲来生成完整的文章，并添加引用。

为了提高问题的深度和广度，STORM 采用了两种策略：

1. 基于视角的问题生成：系统根据输入的主题，调查相似主题的现有文章，并使用这些视角来控制问题生成过程。

2. 模拟对话：系统模拟了一个维基百科作者和主题专家之间的对话，以更新对主题的理解并提出后续问题。

![](img/299bfcae8c4c8f91f809ecbb26136279.png)

![](img/9d28d3fbb18c53bb31b4771af3151044.png)

### Cradle

贡献人：

https://github.com/BAAI-Agents/Cradle

Cradle 是一个赋能基础模型在通用计算机控制方面的框架。它通过统一的人机交互界面（截图输入、键鼠操作输出）使基础模型能够执行复杂的计算机任务。该框架支持多种游戏和软件环境，并提供了灵活的扩展机制以适应新的环境。

最新的视频：https://www.youtube.com/watch?v=fkkSJw1iJJ8

![](img/87282ec8ee33528505c7fe7d1454c9cd.png)

![](img/ddcff6e52f472cd9c1b403bd1e3f421a.png)

### Aider

贡献人：

https://github.com/paul-gauthier/aider

Aider 是一个在终端中进行 AI 对编程的工具。它可以让用户与大型语言模型（如 GPT-4o 和 Claude 3.5 Sonnet）配对编程，编辑本地 Git 仓库中的代码。

![](img/472e1ab8a9255e208a4f48c33ec34e0d.png)

## 投融资

### 逐际动力完成数亿元A轮融资

贡献人：

https://36kr.com/p/2860538964462467

人形机器人公司逐际动力近日宣布完成数亿元A轮融资。本轮融资由多家巨头领投，展示了市场对人形机器人领域的高度关注和认可。融资将用于加强技术研发和市场拓展，进一步推动人形机器人在各行业的应用。此次融资为逐际动力的发展注入了强大动力，有望加速其在行业内的布局和创新。

公司官网：https://www.limxdynamics.com/

![](img/22f6f3c0fca705d7c85fc0df1bfc1ada.png)

## 学习

### Blaze：SparkSQL Native算子优化在快手的深度优化及大规模应用实践

贡献人：

https://mp.weixin.qq.com/s/ne5FCgFDK29BWbLHjm0ZqA

快手开发的 Blaze 是一款基于 Rust 和 `datafusion` 框架的 SparkSQL Native 执行引擎，旨在通过向量化技术和 SIMD 指令提升执行效率。Blaze 通过 Native Session Extension 将 Spark 物理执行计划转换为 Native Plan，实现与 Spark 分布式计算框架的兼容，并通过 Native Engine 执行计算，提高性能。在深度优化中，Blaze 采用了细粒度 `FailBack` 机制、基于 CBO 的转换策略、自定义的向量化数据传输格式、多级内存管理策略，以及对聚合算法的优化，减少了重复计算，提升了执行效率。目前，Blaze 支持 Parquet 向量化读写、常用算子和表达式，以及自研的 Remote Shuffle Service，并在 TPC-H 测试中显示出性能的显著提升

![](img/8d9590c28103e6ce3192f3fe216c7fce.png)

### AI计算时代的数据中心转型

贡献人：

https://mp.weixin.qq.com/s/yqMsg0_ZBPp3XriKihceLA

在 AI 计算时代，数据中心正面临前所未有的转型挑战。CPU、GPU、FPGA 和 DSA 等多样化计算技术各自在推理、训练和特定任务中展现出色，但也带来了更高的功耗和冷却需求。随着大型语言模型（LLM）的兴起，对算力的需求呈指数级增长，推动了不同类型加速器的应用。基准测试如 MLCommons 在评估加速器性能中扮演了关键角色，帮助业界选择最佳 AI 计算解决方案。数据中心的发展从单节点向整机架和集群级别迁移，需要优化网络设计，并考虑更高的功率密度和冷却设施的升级。此外，AI 基础设施的规模化和成本控制也成为了重要议题，需要平衡推理和训练的资源分配，以实现 AI 技术的广泛应用和商业化。

![](img/d042efffaa41a2300e1e2a9985c5b5b9.png)

### 语言模型的训练时间：从估算到 FLOPs 推导

贡献人：

https://zhuanlan.zhihu.com/p/646905171?utm_psn=1788246568375296000

本文旨在解释如何估算大型语言模型（LLM）训练所需的时间，并通过计算浮点运算次数（`FLOPs`）来精确推导。作者提出了一个简化的公式 `训练时间 ≈ 6TP/nX`，其中 `T` 是总 token 数量，`P` 是模型参数量，`n` 是显卡数量，`X` 是每张卡每秒实际做的浮点运算数。文章详细阐述了 `FLOPs` 的估算和精算过程，指出了在计算中只考虑权重矩阵的矩阵乘法，并且展示了如何将 `FLOPs` 转换为实际的训练时间。此外，作者还讨论了模型训练中的激活值重计算、显存带宽瓶颈以及不同硬件对 `FLOPS` 的影响。通过这篇文章，读者可以学习到如何基于模型参数和硬件性能来估算训练大型语言模型所需的时间，并理解在实际训练中可能遇到的技术细节和挑战。

![](img/a5f3bdad7a6da23fdd90a3c13e2ccc6b.png)

### CUDA实现矩阵乘法的性能优化

贡献人：

https://zhuanlan.zhihu.com/p/708583794?utm_psn=1795762463217680384

本文深入探讨了在 CUDA 环境下实现矩阵乘法的性能优化。首先，介绍了矩阵乘法的并行算法基础，包括如何在 GPU 上分配内存、传输数据以及启动核函数。然后，详细阐述了性能优化的关键技术细节，如通过合理组织线程和共享内存来减少全局内存访问延迟，以及如何通过 tile size 的调整和 loop unrolling 优化来提高计算效率。文章还强调了使用合适的矩阵分块策略能够显著提升数据重用和内存带宽利用率。通过实验对比，展示了这些优化措施能够显著提高矩阵乘法运算的性能。

![](img/197ab689cc925293503963559f06e011.png)

### 序列并行云台第29将

贡献人：

https://zhuanlan.zhihu.com/p/708670154?utm_psn=1795760188160094209

KV-Runahead 提出了一种高效的并行方案，通过协调多个进程生成 KV 缓存，以加速大型语言模型（LLM）的推理过程，特别是第一个标记的生成时间（TTFT）。与传统的张量或顺序并行化方案不同，KV-Runahead 利用因果注意力图的特性，自动最小化计算和通信开销，显著提升了推理速度。实验结果表明，KV-Runahead 在 Llama 7B 和 Falcon 7B 上分别实现了超过 1.4 倍和 1.6 倍的加速，展示了其在不同上下文长度和 GPU 数量配置下的优越性能。

![](img/236f25b79f957dd583bd55a172c9af03.png)

### CUDA-MODE 课程笔记 第四课: PMPP 书的第4-5章笔记

贡献人：

https://zhuanlan.zhihu.com/p/708682239?utm_psn=1795759927307952128

CUDA-MODE 课程笔记第四课，深入探讨了 PMPP 书籍中的并行程序模型和性能分析技术。课程强调了并行度、工作量、延迟和带宽等概念的重要性，这些是构建并行性能模型的基础。在性能分析方面，课程指出了如何通过识别瓶颈和使用性能模型来优化并行程序，特别是在 GPU 加速环境下。技术细节上，课程提供了具体的分析方法和工具，如使用性能计数器和分析工具，如 NVIDIA Nsight Systems 和 NVIDIA Nsight Compute，来进行性能测试和优化。

![](img/8842f2c289d2d25f4e0284f36aa92d27.png)

### LLM前沿技术跟踪：experts是否越多越好？MOE的Scaling law研究

贡献人:

https://zhuanlan.zhihu.com/p/700582171?utm_psn=1779899329677246464



文章首先介绍了 MOE 模型中关键的两个参数：专家总数量和每个 token 激活的专家数量。通过实验，研究发现在激活专家数量不变的情况下，增加专家总数量可以提高模型效果，而模型推理成本基本不变。然而，专家数量的增加也带来了训练困难。研究者训练了一系列模型，每个模型配置了不同数量的 experts（2、4、8、16、32），并发现模型参数、expert 数量和训练 token 数目越多，loss 越低，但 loss 曲线随着 expert 数量增长会趋向于平稳。尽管模型只训练一次且离线，推理可能有无数次，因此推理成本更为重要。研究表明，4-8 个 expert 数量最佳，因为过多的 expert 会导致推理时大量参数不被使用，浪费内存。在相同预算下，专家人数越多，推理质量越好，但推理成本越高。最终，研究总结了在 attention 参数量不变的情况下，expert 数量越多，模型总参数越多，效果越好，但推理时内存浪费率越高；在总参数量不变的情况下，增加 expert 数量可以减少所需训练数据量，以更少的训练成本达到相同性能。

![](img/ad934b9d2b287d00df7aba2102bc122d.png)

# 声明

本文档仅供学习交流使用，版权归原作者所有，若涉侵权，请联系Jack Jin 15101136166

## 反馈与联系

（欢迎在下方反馈您宝贵的意见！整理不易，觉得有用给AI日报小组点个赞👍吧！）

ChatGPT；Angela；Zijia Liu；Alex；Zeqi Tan；Tiezhen（HuggingFace）；Naixu；Lizhi；Peter；Yinan；Suoman；Jack

封面生成——右脑科技

如果你有大模型相关的创业想法，欢迎来找我们聊聊～🛰️：15101136166

> 快邀请你的小伙伴们一起来读大模型日报吧！

> **【日报知识库】**这里是近期日报内容的汇总,欢迎大家查阅

> [大模型日报知识库(分享版)](https://miracleplus.feishu.cn/wiki/KapVwXfkriOBiskB0B2cUHxjnTh)

> 欢迎大家多多帮我们分享,转发~~

> 如果大家有任何的建议或者吐槽,欢迎随时找我聊天~

![](img/dc4dff658f61a6157b416311bf0bfd53.png)
