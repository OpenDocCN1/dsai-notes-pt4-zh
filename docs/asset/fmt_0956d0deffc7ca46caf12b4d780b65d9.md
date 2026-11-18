
## LLM帮助理解整个Github Repository提高Coding效率的进展和挑战（2024.6）

贡献人：https://zhuanlan.zhihu.com/p/701989753?utm_psn=1782097799741050880

大语言模型（LLMs）在理解和处理整个 GitHub 仓库代码方面取得了进展，但仍存在挑战。进步包括上下文长文本的实现、推理成本的降低、软件工程框架的成熟和读取 - 理解 - 生成（RAG）技术的逐渐成熟。技术细节涉及如何通过 LLMs 处理代码层级关系、注意力分配和多模态数据对齐问题，以及如何解决缺乏人类反馈数据和微调阶段数据复杂性不足的问题。相关论文和工具提出了基于知识图谱、Topological 数据构建和迭代检索生成等解决方案，目标是让 LLMs 更像人类一样进行精确的代码设计和优化。作者正在从事自动反思工程，旨在提升 LLMs 在代码生成和优化中的自主能力。

![](img/05ce0895c23c722bfdebed5b0dc93071.png)

## Torchao 稀疏性

贡献人：https://github.com/pytorch/ao/tree/main/torchao/sparsity

torchao 稀疏性部分提供了一系列工具和 API，用于实现模型的加速和内存优化。网页详细说明了稀疏性技术的目标、设计理念以及实现方法。稀疏性的关键在于选择合适的参数移除策略，以及如何通过加速核心和优化算法来实现性能提升。网页还展示了使用 torchao 稀疏性功能对 segment-anything 和 BERT 模型的加速成果，包括具体的性能指标和内存使用情况。此外，网页还提供了稀疏性相关术语的解释，以及不同稀疏性模式的比较和优化策略。

![](img/883e4821b7ff71dd181d11fb5fb81a03.png)

## 未来HBM的核心键合技术

贡献人：https://mp.weixin.qq.com/s/uHITm74neCv1sHRP5ff3LQ

未来 HBM 技术的核心在于实现更高密度和带宽的 DRAM 堆叠，关键在于采用 Hybrid Bonding 技术。该技术通过铜间直接键合，去除了对 bump 的依赖，实现了 Copper Pad 到 Copper Pad 的直接结合，显著减少了连接层厚度，提升了散热性能，并支持更小的 bump pitch。Hybrid Bonding 在较低温度下完成，有助于提高晶圆的稳定性和可靠性。与传统的 Micro Bump 技术相比，Hybrid Bonding 减少了寄生参数，有利于提升传输速度和降低互联功耗。目前，HBM3/HBM3E 已实现 12 层 DRAM 堆叠，而业界正面向 HBM4，期待通过 Hybrid Bonding 技术实现更多层数的堆叠，以满足未来更大容量和更高带宽的需求。

![](img/bab79e17269015291fdaa3f641e6578d.png)

## AI + Universities

https://www.bondcap.com/reports/aiu

报告探讨了人工智能（AI）对高等教育的深远影响，特别是在美国大学中的应用。核心观点包括：AI 发展迅速，美国在全球处于领先地位；AI 正在改变学习和教学方式，提供个性化学习和实时反馈；未来大学将更数字化，传统教学模式将被颠覆；AI 可以显著提高教育效率，但也要求大学重新思考和调整教育策略，以应对新的挑战和机遇。

![](img/305b815fe69bcbf0bc2d87bc6247d722.png)

## 声明

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

![](img/d74fe8cc645e4950fd9c2223daf6837e.png)
