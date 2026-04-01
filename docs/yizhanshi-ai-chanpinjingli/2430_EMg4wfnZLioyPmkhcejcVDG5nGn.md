# 03🈷️01日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/XhAPdH8oto6WlcxDVVgc2t9En6f](https://v11enp9ok1h.feishu.cn/docx/XhAPdH8oto6WlcxDVVgc2t9En6f)

> 🔔AI情报助手早报「03月01日」

> ✨✨✨✨✨✨✨✨

> 1️⃣

> 🧚🏻‍♀️

> [谷歌发布BIG-Bench超难基准：DeepSeek-R1](https://mp.weixin.qq.com/s/mhWiMNUvpzDaqYyTEryBqw)得分6.8，只有o3-mini超过10分

>

+   谷歌发布了一个名为BIG-Bench Extra Hard（BBEH）的高难度AI基准测试，以评估AI模型的高阶推理能力。

+   BBEH基于BIG-Bench Hard构建，每个任务都比原任务更困难，为AI模型提供了更多的进步空间。

+   在BBEH测试中，即使是最先进的模型也表现不佳，例如DeepSeek-R1得分仅为6.8分。

+   该基准测试的目的是评估AI模型在更广泛的推理技能上的表现，包括数学、科学、编程等。

+   研究结果表明，推理专用模型在BBEH上的表现优于通用模型，但即使是专用模型的最佳性能也仅有44.8%。

> 关键词：AI基准测试，BIG-Bench，高阶推理，DeepSeek-R1，模型性能

>

> 🔗 文中引用的链接

+   [论文地址](https://arxiv.org/pdf/2502.19187)

+   [数据地址](https://github.com/google-deepmind/bbeh)

+   Mehran Kazemi的推特

+   [Yi Tay的推特](https://x.com/YiTayML/status/1894939679943991661)

> 2️⃣

> 🌏

> DeepSeek开源周，第五弹，3fs来袭~

>

+   DeepSeek开源周最后一天，发布了3FS，一个为AI训练和推理设计的高性能分布式文件系统。

+   3FS具备分离式架构、强一致性（CRAQ技术）和无状态元服务文件接口，优化资源利用和简化应用开发。

+   在数据准备、数据加载器、检查点和KVCache等场景中，3FS提供了有效的解决方案。

+   3FS在性能测试中表现优异，特别是在KVCache读取吞吐量和减少VRAM使用方面。

+   GPT 4.5发布，API价格显著上涨，对DeepSeek的影响和市场反应成为关注焦点。

> 关键词：AI训练、分布式文件系统、3FS、性能优化、GPT 4.5

>

> 🔗 文中引用的链接

+   [DeepSeek开源周文章](https://mp.weixin.qq.com/s/_q4yAmkXO3Tju62XJ5VBDQ)

> 3️⃣

> 🧠

> [如何准确观察小胶质细胞在三维脑组织](https://mp.weixin.qq.com/s/QZtFgfzL6YgS6VAxRVt7Kw)中的动态？DeepCellMap给出答案

>

+   巴黎高等师范学院生物研究所的研究团队开发了DeepCellMap，一个结合深度学习与空间统计的开源分析平台，用于自动化细胞分类和多尺度空间关联分析。🌐

+   DeepCellMap通过自适应阈值分割提取细胞轮廓，基于深度网络分类小胶质细胞，并量化细胞间的空间关联，分类F1分数达81%。🔍

+   该平台揭示了人类发育脑组织中小胶质细胞形态多样性及其空间网络的系统解析，为神经发育与疾病研究提供了新工具。🧬

+   在病理条件下，DeepCellMap发现了小胶质细胞与血管的异常互作，为理解病毒垂直传播的神经病理机制提供了新视角。🏥

+   DeepCellMap的未来展望包括整合光片显微与空间转录组技术，以揭示小胶质细胞的迁移轨迹与分子调控网络。🚀

> 关键词：DeepCellMap，小胶质细胞，三维脑组织，神经发育，病理机制

>

> 🔗 文中引用的链接

+   [论文链接](https://www.nature.com/articles/s41467-025-56560-z)

+   [代码地址](https://github.com/holcman-lab/DeepCellMap)

> 4️⃣

> 🧬

> 无需侵入，一键重构细胞「电活动」，AI革新药物心脏毒性评估

>

+   斯坦福大学和加州大学联合团队开发了一种AI支持技术，利用纳米电极阵列（NEA）记录的电生理信号，非侵入性评估药物心脏毒性。

+   该技术采用基于物理学的深度学习模型，通过数千个同步细胞内外动作电位（eAP和iAP）对，展示了长期、高通量评估的潜力。

+   研究提出了物理知情注意力Unet（PIA-UNET），一种从eAP波形重建iAP波形的深度学习方法，绕过了复杂的参数估计步骤。

+   通过实验证明，使用物理信息深度学习模型可以从eAP记录中完全准确地重建iAP，为心脏毒性评估提供新方法。

+   该技术有望改进体外模型，使用干细胞来源的心肌细胞更准确地评估心脏毒性，推动电生理学研究。

> 关键词：AI，电生理学，心脏毒性评估，纳米电极阵列，深度学习

>

> 🔗 文中引用的链接

+   [论文链接](https://www.nature.com/articles/s41467-024-55571-6#data-availability)

> 5️⃣

> 🧬

> R1爆火之后，思考到底什么任务适合用RL做？

>

+   讨论了RL（强化学习）适用的场景，包括有Ground Truth信号和无Ground Truth信号的任务。

+   强调了RL在难以标注或定义好答案的任务上的优势，如数学题和代码题。

+   提出对于标准答案容易产生的任务，RL并非必要，例如挂接知识库的Agent。

+   分析了在不存在标准答案的任务中，RL如何帮助处理宏观标准容易定义但细节难以精确的任务，如Chat Bot。

+   强调了RL在处理标准变更和数据不一致性方面的优势，以及其在探索未知答案和迭代优化中的作用。

> 关键词：强化学习，Ground Truth，标注数据，标准答案，RL适用性

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/H5RMUPWsHRK9voEu4-SOwQ)

> 6️⃣

> 🌏

> DeepSeek今日连开3源！针对优化的并行策略，梁文锋本人参与开发

>

+   DeepSeek开源周第四天，发布三项针对并行策略的优化，包括DualPipe、EPLB和Profiling Data，旨在提升计算效率和资源利用。

+   DualPipe通过双向流水线并行算法，实现前后向计算-通信阶段的完全重叠，减少流水线气泡，提高并行计算效率。

+   EPLB是用于MoE的负载均衡算法，通过复制高负载专家并智能分配到不同GPU上，确保计算资源均衡利用。

+   Profiling Data提供了训练和推理框架的性能分析数据，可视化通信-计算重叠策略和底层实现细节。

+   发布后不久，三项开源项目在GitHub上的星标迅速破300，显示社区对这些技术的热烈反响。

> 关键词：AI，NLP，大模型，开源应用，性能优化

>

> 🔗 文中引用的链接

+   [DualPipe GitHub](https://github.com/deepseek-ai/DualPipe)

+   [EPLB GitHub](https://github.com/deepseek-ai/eplb)

+   Profiling Data GitHub