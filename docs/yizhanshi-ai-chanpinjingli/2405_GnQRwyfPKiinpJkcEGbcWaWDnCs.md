# 02🈷️05日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/KwYFd7LyUobnERx4Z25cUo1qnKZ](https://v11enp9ok1h.feishu.cn/docx/KwYFd7LyUobnERx4Z25cUo1qnKZ)

> 🔔AI情报助手晚报「02月05日」

> ✨✨✨✨✨✨✨✨

> 1️⃣

> 🌏

> 自有歪果仁为DeepSeek「辩经」：揭穿围绕DeepSeek的谣言

>

+   DeepSeek R1模型以其开源性和较低训练成本引起广泛关注。

+   针对DeepSeek的误解包括其公司背景、训练成本和创新能力，文章逐一反驳这些观点。

+   DeepSeek展示了高效的训练效率和在模型架构上的创新，如Multi-latent注意力和GRPO算法。

+   文章强调DeepSeek的成功并非依赖于ChatGPT的知识蒸馏，而是其自身的技术创新。

+   讨论了中国在AI领域的竞争力，认为中美在AI领域的竞争态势并未因DeepSeek而有太大变化。

> 关键词：DeepSeek，开源模型，训练效率，技术创新，AI竞争

>

> 🔗 文中引用的链接

+   [原文链接](https://www.tanishq.ai/blog/posts/deepseek-delusions.html)

> 2️⃣

> 🧬

> 训练1000样本就能超越o1，李飞飞等人画出AI扩展新曲线

>

+   斯坦福大学博士Niklas Muennighoff提出新方法s1，仅用1K样本和简单的测试时间干预就能重现o1的预览扩展和性能。

+   s1通过构建小型数据集s1K并实施「预算强制」策略，控制测试时间计算，提升模型推理性能。

+   在竞赛数学问题上，s1-32B模型表现比o1-preview高出27%，展现了测试时间扩展的有效性。

+   s1方法专注于序列扩展，提出了新的序列扩展方法和基准测试方式，强调了序列扩展在深度推理和迭代优化中的优势。

+   s1-32B是完全开源的，包括权重、推理数据和代码，是样本效率最高的开放数据推理模型之一。

> 关键词：AI扩展，测试时间扩展，序列扩展，预算强制，开源模型

>

> 🔗 文中引用的链接

+   [论文链接](https://arxiv.org/abs/2501.19393)

+   [项目链接](https://github.com/simplescaling/s1)

> 3️⃣

> 🤖

> [英伟达机器人跳APT舞惊艳全网，科比](https://mp.weixin.qq.com/s/Y0dHqWBKoRlPzlOWLOj5oA)C罗完美复刻！CMU 00后华人共同一作

>

+   英伟达和CMU联合研发的人形机器人展示了科比、C罗等球星的经典动作，以及高难度的全身控制动作和APT舞蹈。

+   通过ASAP模型，实现了“真实→仿真→真实”的全身控制策略，有效缩小了仿真与现实动力学差异。

+   ASAP模型包含预训练和后训练两大阶段，通过delta动作模型补偿动力学误差，实现更敏捷的人形机器人动作。

+   研究团队在多个仿真器和真实世界的Unitree G1人形机器人上验证了ASAP的有效性，显著降低了运动跟踪误差。

+   该研究为机器人奥运会等未来应用场景提供了技术基础，并开源了多仿真器训练与评估代码库。

> 关键词：人形机器人，ASAP模型，动力学补偿，全身控制，开源代码库

>

> 🔗 文中引用的链接

+   [项目主页](https://agile.human2humanoid.com/)

+   [论文地址](https://arxiv.org/abs/2502.01143)

+   [开源项目](https://github.com/LeCAR-Lab/ASAP)

> 4️⃣

> 🧚🏻‍♀️

> [超越DeepSeek V3！Ai2再祭开](https://mp.weixin.qq.com/s/hX2pNUupJ5yJ-eqe3qyP-Q)源杀器Tülu 3，强化学习打破性能瓶颈

>

+   美国艾伦人工智能研究所（Ai2）推出基于强化学习的开源模型Tülu 3 405B，性能超越DeepSeek v3和GPT-4o。

+   Tulu 3 405B在多项关键基准测试中表现优异，超越了先前发布的后训练开源模型。

+   Tulu 3的训练方法包括指令微调、偏好微调和基于强化学习的新方法，称为可验证奖励强化学习（RLVR）。

+   Tulu 3项目完全开源，包括数据、评估、训练代码及开发配方，推动开放后训练研究。

+   Tulu 3的成功标志着AI领域后训练研究的新里程碑，为多目标和多阶段训练框架内的研究铺平了道路。

> 关键词：AI，强化学习，开源模型，性能超越，后训练研究

>

> 🔗 文中引用的链接

+   [论文链接](https://arxiv.org/pdf/2411.15124)

+   [demo传送门](https://playground.allenai.org/)

+   [仓库地址](https://github.com/allenai/open-instruct)

> 5️⃣

> 🤖

> [AI编程L1-L5超全分级来了！GitHub](https://mp.weixin.qq.com/s/j5wFTPUxh-8pOZAHq8EMeg) Copilot仅L1，Devin是L4

>

+   AI编程工具分为L1至L5五个等级，每个等级代表不同的自动化程度和能力。

+   L1级工具如GitHub Copilot专注于代码补全，而L4级工具如Devin能实现从需求到部署的完全自动化。

+   L2级工具如ChatGPT进行任务自动化，需要精心设计的提示和代码上下文。

+   L3级工具如Codegen实现项目级自动化，但目前还处于初级阶段，需要人工干预。

+   L5级工具预示着AI系统能复制整个软件开发团队，实现高度自动化。

> 关键词：AI编程，自动化，代码补全，项目自动化，软件开发

>

> 🔗 文中引用的链接

+   [参考资料](https://arxiv.org/html/2403.08299v1)

+   [16x Prompt博客](https://prompt.16x.engineer/blog/ai-coding-l1-l5)