
# 01月18日

> 来源：https://v11enp9ok1h.feishu.cn/docx/IztDdaahYoXskWxtoMRcqOzfnXd

> 🔔 AI情报助手午报「01月18日」

## 1️⃣ 多模态入门--CogVLM, VILA, MM1, MM1.5 和 Pixtral-12B

CogVLM 通过创新的结构和训练方法优化图文空间对齐，提升多模态效果。
VILA 强调 LLM 训练的重要性，并发现图文交错数据对模型效果有显著提升。
MM1 通过细致的消融实验和多维度优化，实现了预训练指标 SOTA 的模型。
MM1.5 对 MM1 进行升级，增加了视频和 UI 处理能力，优化了 SFT 和继续预训练。
Pixtral 12B 引入 PixtralViT 视觉编码器，针对图像二维特性进行了改进。

关键词：多模态学习，CogVLM，VILA，MM1，PixtralViT

> 🔗 文中引用的链接

+ [MM1: Methods, Analysis & Insights from Multimodal LLM Pre-training](https://arxiv.org/abs/2403.09611)
+ [MM1.5: Methods, Analysis & Insights from Multimodal LLM Fine-tuning](https://arxiv.org/abs/2409.20566)
+ [VILA: On Pre-training for Visual Language Models](https://arxiv.org/abs/2312.07533)
+ [Pixtral 12B](https://arxiv.org/abs/2410.07073)
+ [CogVLM: Visual Expert for Pretrained Language Models](https://arxiv.org/abs/2311.03079)

## 2️⃣ 多模态&LLM Reyes：一个从 0 到 1 开始训练的多模态大模型

Reyes（睿视）是一个 8B 参数的多模态大模型，由视觉编码器 `InternViT-300M-448px-V2_5` 和语言模型 `Qwen2.5-7B-Instruct` 组成。
Reyes 通过两层 MLP 投影层连接视觉编码器与语言模型，实现了更小参数量下的性能超越，得分 0.447，高于 `llava1.5-13B` 的 0.367 分。
Reyes 模型的架构和代码已开源，提供了详细的模型实现和数据处理流程。
在 MMMU 评测中，Reyes 展现了在多学科任务中的表现，包括艺术与设计、商业等多个领域，得分高于 `llava1.5-13B`。
文章还探讨了未来 Reyes 模型的改进方向，包括加入更多多模态数据和私有数据进行训练。

关键词：多模态，大模型，Reyes，MMMU 评测，模型优化

> 🔗 文中引用的链接

+ [模型权重开源地址](https://modelscope.cn/models/yujunhuinlp/Reyes-8B)
+ [github](https://github.com/yujunhuics/Reyes)
+ [预训练数据](https://huggingface.co/datasets/liuhaotian/LLaVA-Pretrain)
+ [SFT 数据](https://huggingface.co/datasets/liuhaotian/LLaVA-Instruct-150K/blob/main/llava_v1_5_mix665k.json)

## 3️⃣ 从“说”到“做”：大语言模型为何会进化成智能体

文章探讨了大语言模型（LLM）如何通过技术进步成为智能体，强调了智能体的自主性、感知能力、决策能力、行动能力和适应性。
分析了传统软件系统的局限性，包括依赖用户输入、缺乏自主性和学习能力，以及大模型如何克服这些限制。
讨论了 ReAct 方法，它是首次将推理与行动结合在大模型中，使模型能够动态调整行动计划并利用外部知识。
ReAct 展示了多任务学习和适应性，能够在不同任务中快速适应和学习，提升了智能体的决策和行动效率。
文章强调了语言模型的进化对智能体实现的重要性，以及它们如何借助语言的力量在复杂环境中灵活应对和进化。

关键词：大语言模型，智能体，自主性，ReAct，多任务学习

> 🔗 文中引用的链接

+ [原文链接](https://mp.weixin.qq.com/s/w_fB33rE9zuAiB7P7jY0Ig)

## 4️⃣ 2024 诺贝尔化学奖得主：「模型幻觉」给我无限创造力

2024 年诺贝尔化学奖得主 David Baker 利用 AI「模型幻觉」技术加速蛋白质设计，获得重大科学突破。
AI「幻觉」技术通过生成新概念帮助科学家追踪癌症、设计药物，开辟研究新领域。
Baker 博士的实验室利用 AI 设计了一千万种全新蛋白质，许多用于医疗用途，获得约 100 项专利。
AI「幻觉」技术被视为科学方法的加速版，能够快速验证科学假设，推动科学发现。
尽管存在争议，AI「幻觉」技术在科学界的应用展示了其在促进科学发现方面的潜力。

关键词：AI「幻觉」，蛋白质设计，科学发现，诺贝尔化学奖，创新加速

> 🔗 文中引用的链接

+ [论文地址](https://www.nature.com/articles/s41586-021-04184-w)
+ [论文地址](https://pmc.ncbi.nlm.nih.gov/articles/PMC7225573/pdf/nihms-1586979.pdf)
+ [论文地址](https://www.cs.utexas.edu/~xiao/papers/hallucination.pdf)

## 5️⃣ 速报：智谱与阶跃星辰齐发力 | 马斯克 xAI 推出网页版 Grok 聊天机器人，无需 X 账户即可体验

智谱清言 APP 推出“2025 克穷暴富”春节活动，用户可参与 AI 互动活动赢取现金红包。
马斯克 xAI 公司发布网页版 Grok 聊天机器人，无需 X 账户即可体验智能服务。
智谱发布 GLM 系列新模型，专注于高效、低延迟的语言和视觉理解解决方案。
阶跃星辰推出自研推理模型 `Step R-mini`，强化复杂问题解决能力。
LinkFox AI 为跨境电商提供 AI 文案生成和图片处理等智能工具。

关键词：AI 活动，Grok 聊天机器人，GLM 模型，`Step R-mini`，跨境电商

> 🔗 文中引用的链接

+ [智谱清言春节活动](https://mp.weixin.qq.com/s/KsVT58QJrNdwCTaxY-1mGg)
+ [Grok 聊天机器人](https://x.com/i/grok)
+ [智谱 GLM 系列新模型](https://mp.weixin.qq.com/s/jpwClxbl2514AzSfgYVEeA)
+ [阶跃星辰 Step R-mini](https://mp.weixin.qq.com/s/jpwClxbl2514AzSfgYVEeA)
+ [LinkFox AI](https://ai.linkfox.com/home)

## 6️⃣ 文末赠书！“西瓜书”《机器学习》官方配套习题集重磅出版

南京大学周志华教授所著的《机器学习》官方配套习题集《机器学习习题参考》出版，由叶翰嘉和詹德川两位老师合作编写。
习题集包含 142 道习题，分为简答题、计算题和编程题，覆盖《机器学习》第 1～10 章内容，旨在帮助读者更好地掌握和巩固知识点。
本书习题均具有详细解答，提供了出题思路、知识要点、可能的解法和相关知识点的关联拓展。
习题集适用于《机器学习》教材的教学或自学辅助，适合作为高等院校人工智能、计算机等专业的教学辅助书籍。
书中编程题基于 `Python 3` 脚本语言，方便读者在实际数据上探究知识点和算法性质。

关键词：机器学习，习题集，教学辅助，`Python`，算法应用

> 🔗 文中引用的链接

+ [原文链接](https://mp.weixin.qq.com/s/ZGsaYTcIUt0qq6XM92uNyQ)
