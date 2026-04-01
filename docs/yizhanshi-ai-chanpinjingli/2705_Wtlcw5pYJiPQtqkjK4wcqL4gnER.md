# 12🈷️12日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/N7DUdYBpUoIpknxNYWjcJpecnid](https://v11enp9ok1h.feishu.cn/docx/N7DUdYBpUoIpknxNYWjcJpecnid)

> 🔔AI情报助手午报「12月12日」

> ✨✨✨✨✨✨✨✨

> 1️⃣

> 🔮

> [你的Agent可能设计错了：UIUC & 斯](https://mp.weixin.qq.com/s/LM5WVxbgw_S3IHxZb3fSTw)坦福等联合发文，重构Agent适配2X2

>

+   UIUC领衔全球顶尖高校发布《Adaptation of Agentic AI》综述，将提示工程与微调统一为“适配”，提出2X2框架划分四大Agent适配范式，实现基础模型与工具的神经同步。

+   核心2X2框架按适配目标（改Agent/改Tool）和信号来源（过程/结果）分A1（工具执行信号改Agent）、A2（结果改Agent）、T1（通用工具）、T2（代理监督改工具）四范式。

+   T2范式颠覆性强：固定黑盒大模型（如GPT-4），训练轻量工具对齐它，s3案例用2400条数据达Search-R1（17万数据）效果，数据效率提升70倍。

+   最佳实践为混合架构：如A1→T1“毕业”路径（专家模型转工具复用），A2+T2联邦路径（核心模型+专用子智能体）。

+   未来方向是协同适配（Agent与Tool双向训练），但需警惕Reward Hacking等安全风险，需添加安全护栏。

> 关键词：Agent适配，2X2框架，T2范式，混合架构，协同适配

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/LM5WVxbgw_S3IHxZb3fSTw)

> 2️⃣

> 🔮

> [AAAI 2026｜华中科大×湖大×清华联合提出Anomag](https://mp.weixin.qq.com/s/jFvTDNlnJp5tSDmsbtJQVA)ic：实现跨模态提示驱动的零样本异常生成

>

+   🤖 华中科大等机构联合提出Anomagic，可通过文本/图像提示零样本生成真实异常与像素级掩码，无需领域微调适配工业、医疗等多场景。

+   🔧 Anomagic含跨模态提示编码（融合文本视觉语义）、对比掩码细化（精准对齐）、大规模数据集AnomVerse三大核心创新模块。

+   🎯 基于潜扩散模型（LDM），训练用AnomVerse三元组学习生成逻辑，推理阶段接收用户提示完成异常生成与掩码细化。

+   📊 实验显示Anomagic在生成质量（IS/IL指标）及下游检测性能上远超现有方法，泛化能力覆盖多领域。

+   🧬 AnomVerse是当前最大多领域"异常-掩码-描述"三元组数据集，含12987样本支撑模型训练。

> 关键词：Anomagic，零样本异常生成，跨模态提示，潜扩散模型，AnomVerse

>

> 🔗 文中引用的链接

+   [原文章链接](https://mp.weixin.qq.com/s/jFvTDNlnJp5tSDmsbtJQVA)

+   [论文地址](https://arxiv.org/abs/2511.10020v1)

+   [代码仓库](https://github.com/yuxin-jiang/Anomagic)

+   [数据集仓库](https://huggingface.co/datasets/yuxinjiang11/AnomVerse)

+   [HuggingFace在线演示](https://huggingface.co/spaces/yuxinjiang11/Anomagic)

> 3️⃣

> 🤖

> [NeurlPS'25开源 | 中科院新作AutoSeg3](https://mp.weixin.qq.com/s/_5QuylqaBze_rIbcmhDZYQ)D：在线分割一切3D物体，超越ESAM！

>

+   🧠 中科院团队提出AutoSeg3D模型，将在线3D分割重构为实例跟踪任务，通过长短期记忆查询流维护对象ID与特征，结合空间一致性学习抑制过分割。

+   🚀 在ScanNet200基准上比ESAM提升2.8 AP且保持实时；使用SAM时AP提升3.3，FastSAM也有一致性能提升，泛化能力强。

+   🛠️ 核心模块含长期记忆（LTM）维护跟踪库、短期记忆（STM）优化实例嵌入、空间一致性学习（SCL）合并掩码片段，解决VFM碎片化问题。

+   🌟 该框架适合具身智能体动态环境感知，能处理遮挡和部分视图，为机器人技术提供精准3D分割能力。

+   📚 论文已开源，arxiv链接和GitHub代码链接均已发布，方便研究者探索细节。

> 关键词：AutoSeg3D，在线3D分割，实例跟踪，空间一致性学习，具身智能

>

> 🔗 文中引用的链接

+   [原文章链接](https://mp.weixin.qq.com/s/_5QuylqaBze_rIbcmhDZYQ)

+   [论文链接](https://arxiv.org/abs/2512.07599)

+   [代码链接](https://github.com/AutoLab-SAI-SJTU/AutoSeg3D)