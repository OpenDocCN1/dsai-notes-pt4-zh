
MLPerf 训练 4.0 测试新增了能耗指标和大语言模型（LLM）微调基准，NVIDIA 再次证明了其在 AI 领域的领导地位，赢得了所有工作流的冠军。新的基准测试包括了图神经网络（GNN）和 LoRA 技术的 LLM 微调，后者是一种参数高效的微调方法，能显著降低训练时间和计算资源需求。在这次测试中，NVIDIA 的 GPU 展现了卓越的性能，特别是 H200 GPU 在性能上 relatively to H100 GPU 的提升。此外，Intel 的 Habana Gaudi2、Google 的 TPU v-5P 和 AMD 的 Radeon RX 7900 XTX GPU 也显示了强劲的竞争力。MLCommons 通过 MLPerf 基准测试推动了 AI 性能的提升，并为行业提供了评估和比较不同系统性能的工具。多家公司，如 Intel、Juniper Networks、Google、HPE、Fujitsu、Giga Computing、Dell Technologies、联想、NVIDIA、Oracle、Quanta Cloud Technology、Supermicro、Sustainable Metal Cloud (SMC) 和 Tiny Corp，都提交了他们的基准测试结果，这些结果不仅展示了技术进步，也强调了在 AI 训练中能效的重要性。特别是，Sustainable Metal Cloud (SMC) 通过其自主研发的 “Sustainable AI Factories” 平台，实现了在保持世界级性能的同时显著减少能源消耗。这些进展和优化措施为 AI 技术的未来发展奠定了基础，并在推动 AI 技术向更高效、可持续的方向发展中发挥了关键作用。

![](img/c0ac21b467ad404736acc1f805e50ac5.png)

### 重温SSM（二）：HiPPO的一些遗留问题

`https://kexue.fm/archives/10137`

文章深入探讨了HiPPO矩阵在离散化和实际应用中的技术细节。文章详细介绍了两种线性ODE系统HiPPO-LegT和HiPPO-LegS的离散化方法，强调了前向欧拉和后向欧拉方法在数值稳定性和精度上的差异。此外，文章分析了LegS型ODE的优良性质，如步长无关性和多项式衰减特性，确保了长尾记忆能力和计算高效性。最后，通过傅立叶基推导，对比了不同基下的记忆效果，进一步深化对HiPPO的理解。

# 声明

本文档仅供学习交流使用，版权归原作者所有，若涉侵权，请联系Jack Jin 15101136166

（欢迎在下方反馈您宝贵的意见！整理不易，觉得有用给AI日报小组点个赞👍吧！）

ChatGPT；Angela；Zijia Liu；Alex；Zeqi Tan；Tiezhen（HuggingFace）；Naixu；Lizhi；Peter；Suoman；Jack

封面生成——右脑科技

如果你有大模型相关的创业想法，欢迎来找我们聊聊～🛰️：15101136166

> 快邀请你的小伙伴们一起来读大模型日报吧！

> **【日报知识库】**这里是近期日报内容的汇总,欢迎大家查阅

> [大模型日报知识库(分享版)](https://miracleplus.feishu.cn/wiki/KapVwXfkriOBiskB0B2cUHxjnTh)

> 欢迎大家多多帮我们分享,转发~~

> 如果大家有任何的建议或者吐槽,欢迎随时找我聊天~

![](img/5edf07725c3844f1d7c8c1eb9fbab573.png)
