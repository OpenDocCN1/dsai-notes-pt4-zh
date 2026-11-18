
在 PyTorch 2.3 版本中，引入了`torch.distributed.tensor.parallel`模块，专门用于张量并行。核心函数`parallize_module`负责将模型并行化。并行化方法通过设置不同的`ParallelStyle`来定义，包括`ColwiseParallel`和`RowwiseParallel`两种主要的张量并行方式，以及`SequenceParallel`、`PrepareModuleInput`和`PrepareModuleOutput`等。这些并行策略需要指定参数、输入和输出的处理方式，即`Placement`，包括`Shard`、`Replicate`和`_Partial`三种方法。通过一个 MLP 的例子，文章展示了如何将`nn.Linear`层的参数沿列或行分割，以及如何在多个 GPU 上复制模型或进行维度对齐。此外，文章还强调了并行训练过程中的关键点，如梯度累积、通信开销和内存使 AGE，以及如何使用`torch.cuda.memory_allocated()`来监控内存使用情况。

![](img/58dc961b216d2e1fc76b8da3f3006239.png)

### 使用 vLLM 为多个 LoRA 适配器提供服务

贡献人：

https://zhuanlan.zhihu.com/p/712668798?utm_psn=1804465132673298432

文章探讨了使用 vLLM 技术为多个 LoRA 适配器提供服务的方法。LoRA 适配器通过在预训练模型中引入低秩矩阵进行微调，能够快速适应新任务。vLLM 通过虚拟化技术，使得单一模型能够支持多个 LoRA 适配器，从而在保持模型轻量级的同时提升了其在多任务处理上的能力。这种方法减少了内存和计算资源的消耗，尤其对于大规模模型的部署具有重要意义。

### [2024智源大会速览] 视频生成篇

贡献人：

https://zhuanlan.zhihu.com/p/713143946?utm_psn=1804435630656192512

2024 智源大会视频生成篇聚焦了 AI 在视频制作领域的应用前沿。Aditya Ramesh 从 OpenAI 介绍了从图生文到文生图的发展脉络，强调了语言智能在视觉智能中的作用。万鹏飞从快手提出的可灵视频生成大模型中，展示了 3D 时空注意力机制和 Scaling Law 的应用，实现了分辨率高、运动合理的视频生成。王长虎从爱诗科技角度探讨了 Character2Video 和 MagicBrush 技术，提出了基于 adapter 方法和 RL 的 PPO 模块来提升视频生成的保真度和美学质量。卢志武教授从人民大学视角分析了视频生成的两种范式，即基于图片 SD 模型的时序关系建模和直接全局生成的方法。鲍凡从生数科技介绍了 Vidu 及其在 4D 视频生成中的应用，以及如何通过 DGS 方法进行 3D 场景的重建和优化。SVD 方法介绍了其在 Large Video Dataset (VLD) 建设、Temporal Video Fine-Tuning 和 Generate High Frame & High Resolution Video 方面的技术创新。Sora 方法通过 NaViT 技术支持原分辨率训练，实现了长视频的连贯性和高效率生成。

![](img/37e59ed23efc42a2ac2f46cb6ea47cf3.png)

### 大模型分不清 9.9 与 9.11 谁大，那 Embedding 模型呢？

贡献人:

https://mp.weixin.qq.com/s/wIm3loi5KcznFYTpgIl-Dg

本文由肖涵从 Jina AI 撰写，探讨了大型语言模型在数字比较任务中的挑战。通过对`jina-embeddings-v2-base-en`和`jina-reranker-v2-multilingual`模型进行的实验，文章评估了它们在不同数字比较场景下的性能，包括小数、货币、日期和时间等。实验采用余弦相似度和相关性得分作为评价指标，结果显示 Embedding 模型在简单数字比较中表现较好，但在复杂数值区间或浮点数比较时效果下降。Reranker 模型在这些任务中的表现不稳定，尤其是在处理大数字和随机数字范围时。文章指出，分词策略和训练数据对模型的数值推理能力有重要影响，并强调了这种能力对于提升搜索质量的重要性。特别是在处理结构化数据如 JSON 时，模型的算术能力至关重要。

![](img/c1f2f375b8851c2655072501362dad98.png)

### 赖耶 AI 工厂-基于 NVIDIA AI Enterprise 的最佳落地实践

贡献人：

https://zhuanlan.zhihu.com/p/713030782?utm_psn=1804173697554190336

赖耶 AI 工厂由北京赖耶信息科技有限公司推出，致力于提供全栈式大语言模型技术服务，推动各行各业的智能化转型。公司核心团队成员具备丰富的专业知识和实践经验，在高性能计算集群建设、基础设施及大模型应用方面积累了丰富的实践经验。赖耶 AI 工厂的技术优势和服务内容包括：万卡集群管理系统 KAA、企业大模型服务平台 MANAS、企业级专家技术服务以及一体化集群解决方案。KAA 系统提供了高效的集群管理和自动化工具，支持多租户架构，并具备高可靠性和故障恢复能力。MANAS 平台则提供了从数据管理到模型部署的全套解决方案，支持模型定制和优化，以及信息检索和数据防护。赖耶科技还提供了针对企业的专家技术支持服务，包括业务标准支持和增值技术支持，以及客户生命周期服务。最后，赖耶 AI 工厂提供了一体化集群部署方案，帮助企业快速搭建高效计算中心。

![](img/dc0f30014baac1a85ba75fbb395487bc.png)

# 声明

本文档仅供学习交流使用，版权归原作者所有，若涉侵权，请联系Jack Jin 15101136166

（欢迎在下方反馈您宝贵的意见！整理不易，觉得有用给AI日报小组点个赞👍吧！）

ChatGPT；Angela；Zijia Liu；Alex；Zeqi Tan；Tiezhen（HuggingFace）；Naixu；Lizhi；Peter；Suoman；Jack

封面生成——右脑科技

如果你有大模型相关的创业想法，欢迎来找我们聊聊～🛰️：15101136166

> 快邀请你的小伙伴们一起来读大模型日报吧！

> 【日报知识库】这里是近期日报内容的汇总,欢迎大家查阅

> [大模型日报知识库(分享版)](https://miracleplus.feishu.cn/wiki/KapVwXfkriOBiskB0B2cUHxjnTh)

>

> 欢迎大家多多帮我们分享,转发

> 如果大家有任何的建议或者吐槽,欢迎随时找我聊天~

![](img/09f3d013969b8cc5113babe132206341.png)
