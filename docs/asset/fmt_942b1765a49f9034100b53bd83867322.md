
# 03🈷️26日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/IxaPdv8yzomIFqx4C12cChavnAh](https://v11enp9ok1h.feishu.cn/docx/IxaPdv8yzomIFqx4C12cChavnAh)

> 🔔AI情报助手早报「03月26日」

## 1️⃣ 埃默里大学、UT Austin联合提出Spatial-RAG：开启空间智能问答的新时代

- 埃默里大学和UT Austin联合提出Spatial-RAG框架，旨在增强LLMs空间推理能力，弥合结构化空间数据库与自然语言问答的差距。
- 该框架创新点包括通用框架、稀疏-密集空间混合检索器、多目标引导的空间文本生成器及真实世界评估。
- 实验在纽约和迈阿密旅游数据集上进行，Spatial-RAG表现优异，能有效处理复杂空间推理问题。
- 消融实验表明各模块对模型性能有不同影响。
- Spatial-RAG为旅游推荐、路径规划等应用提供强大支持。

**关键词**：Spatial-RAG，LLMs，空间推理，空间智能问答

### 参考链接

- [论文链接](https://www.researchgate.net/publication/388656403_Spatial-RAG_Spatial_Retrieval_Augmented_Generation_for_Real-World_Spatial_Reasoning_Questions)

## 2️⃣ Uni-3DAR用自回归统一微观与宏观的3D世界，性能超扩散模型256%，推理快21.8倍

- Uni-3DAR是深势科技等机构研发的科学大模型，通过自回归将3D结构生成与理解统一。
- 其核心技术包括层次化token化方法，实现微观到宏观3D结构统一表示。
- Masked Next-Token Prediction策略，构建统一自回归框架，提升预测效果。
- 实验显示在微观3D结构任务中，生成性能超扩散模型，理解任务表现与相关模型持平。
- 未来将在宏观结构任务验证通用性，融合数据类型与任务，扩展多模态。

**关键词**：Uni-3DAR，3D结构，自回归，微观宏观统一，多模态

### 参考链接

- [论文地址](https://arxiv.org/abs/2503.16278)
- [项目主页](https://uni-3dar.github.io)
- [代码仓库](https://github.com/dptech-corp/Uni-3DAR)

## 3️⃣ 推理延展到真实物理世界，英伟达Cosmos-Reason1：8B具身推理表现超过OpenAI o1

- 英伟达发布针对物理常识推理优化的模型Cosmos-Reason1，含模型、本体和基准，有8B和56B两个版本。
- 模型经视觉预训练等四个阶段训练，采用仅解码器多模态架构和混合Mamba-MLP-Transformer主干。
- 实验显示，经物理AI监督式微调，Cosmos-Reason1在物理常识和具身推理表现上优于基线模型。
- 经物理AI强化学习后训练，多数基准性能提升，但在RoboFail基准表现未提升。
- Cosmos-Reason1能拒绝不明选项，在直觉物理推理任务有进步。

**关键词**：英伟达，Cosmos-Reason1，物理常识推理，具身推理，物理AI

### 参考链接

- [论文地址](https://arxiv.org/abs/2503.15558)
- [代码地址](https://github.com/nvidia-cosmos/cosmos-reason1)
- [NVIDIA 物理AI术语表](https://www.nvidia.cn/glossary/physical-ai/)
- [NVIDIA Cosmos-Reason1 实验室页面](https://research.nvidia.com/labs/dir/cosmos-reason1/)

## 4️⃣ 挖掘DiT的位置解耦特性，Personalize Anything免训练实现个性化图像生成

- 清华、北航团队推出免训练架构Personalize Anything，实现个性化图像生成新范式。
- 该架构特点为高效免训练、高保真与可控、高扩展性，能完成多任务。
- 发现DiT架构位置编码影响注意力机制，提出标记替换实现主体重建。
- 创新时间步适应替换策略与特征扰动，提升生成多样性与质量。
- 实验显示其性能优于现有方法，未来有望拓展到视频、3D生成领域。

**关键词**：个性化图像生成，DiT架构，免训练，特征替换，时间步适应

### 参考链接

- [论文链接](https://arxiv.org/abs/2503.12590)
- [项目主页](https://fenghora.github.io/Personalize-Anything-Page/)
- [代码仓库](https://github.com/fenghora/personalize-anything)

## 5️⃣ 分子对接速度提升10,000倍，深度图学习加速RNA虚拟筛选，助力药物靶标发现

- 麦吉尔大学等机构提出RNAmigos2，基于结构的RNA虚拟筛选方法，用深度图学习加速RNA虚拟筛选。
- 该模型实现对接速度10000倍提升，在测试集上表现出色，首次通过实验验证基于结构的RNA VS深度学习成功。
- RNAmigos2采用编码器-解码器框架，经训练可预测结合亲和力及兼容性分数。
- 研究人员公开数据集、源代码和模型权重，该方法有需预定义结合位点等局限。
- 该方法有望与RNA分子设计技术等协同，助力下一代RNA药物发现。

**关键词**：RNA虚拟筛选，深度图学习，RNAmigos2，药物靶标发现

### 参考链接

- [相关代码1](https://doi.org/10.5281/zenodo.14803961)
- [相关代码2](https://github.com/cgoliver/rnamigos2/)
- [论文链接](https://www.nature.com/articles/s41467-025-57852-0)

## 6️⃣ 为矛盾的理论提供统一解释，牛津大学等揭秘非晶硅结构

- 牛津大学等机构研究非晶硅结构，其两种主要理论存在矛盾。
- 论文利用机器学习驱动模拟方法，采样淬火硅构型空间，揭示非晶化与结晶化界限。
- 采用teacher-student方法，将精确但慢的机器学习势蒸馏成更快模型。
- 对不同规模和淬火速率进行模拟，创建结构模型库并分析。
- 提出修正的类准晶硅模型，与实验数据一致。

**关键词**：非晶硅结构，机器学习，模拟方法，类准晶硅模型

### 参考链接

- [论文地址](https://www.nature.com/articles/s41467-025-57406-4)
