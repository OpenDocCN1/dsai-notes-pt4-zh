
# 大型语言模型中提示工程的系统调研：技术与应用

> 来源：[https://feicaiclub.feishu.cn/docx/KusHdCGtho9yCRx8aCSck9bwnMb](https://feicaiclub.feishu.cn/docx/KusHdCGtho9yCRx8aCSck9bwnMb)

![](img/7b3b979c02206767be7a4951036b6402.png)

[A Systematic Survey of Prompt Engineering in Large Language Models- Techniques and Applications.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/D20VbQ7wPoP8i5xdZ8mcrNoXnCg)

---

## 论文总结: 《A Systematic Survey of Prompt Engineering in Large Language Models: Techniques and Applications》

### 摘要

Prompt engineering已经成为扩展大规模语言模型（LLMs）和视觉语言模型（VLMs）功能的不可或缺的技术。通过任务特定的指令（称为prompts）增强模型效果，无需修改核心模型参数。本文系统概述了最近在prompt engineering领域的进展，并根据应用领域对其进行分类。每种提示方法的总结包括其方法论、应用、涉及的模型和使用的数据集，同时讨论了每种方法的优缺点。

### 引言

#### 背景

- 重要性：Prompt engineering通过提供上下文指令，指导模型输出而不改变模型参数，使LLMs和VLMs在各种任务和领域中表现出色。

- 变革性：这种方法与传统的模型再训练或大规模微调不同，通过精心设计的指令，prompt engineering推动了AI的边界，开辟了未来的可能性。

#### 研究动机

- 文献缺口：尽管prompt engineering在LLMs和VLMs中有广泛的应用，但缺乏系统的概述和分类。

- 目标：提供对prompt engineering的结构化概述，分析其应用、模型和数据集，探讨其优缺点，帮助未来的研究和实践。

### 主要贡献

1. 系统概述：详细分类和总结了29种不同的prompt engineering技术，涵盖了从零样本提示到最新的链式提示技术。

2. 应用分析：探讨了每种方法的应用、所用模型和数据集，并评估了其效果和局限性。

3. 未来研究方向：提出了当前研究中的挑战和未来机会，鼓励更多创新和改进。

## Prompt Engineering 技术分类

### 新任务无需大量训练

- 零样本提示（Zero-Shot Prompting）：通过精心设计的提示，引导模型完成新任务，无需特定的输入-输出映射训练数据。

- 少样本提示（Few-Shot Prompting）：提供少量高质量的输入-输出示例，提高模型在复杂任务上的性能。

### 推理与逻辑

- 链式推理提示（Chain-of-Thought, CoT）：通过分步提示，促进模型进行连贯的推理过程，提高复杂推理任务的表现。

- 自动链式推理提示（Automatic CoT）：通过自动生成推理链，增强模型在少样本学习中的鲁棒性。

- 自一致性（Self-Consistency）：通过生成多样的推理链，选择最一致的最终答案，提高推理任务的准确性。

### 减少幻觉

- 检索增强生成（RAG）：通过检索相关信息，丰富提示的背景，减少模型生成的错误。

- 反应提示（ReAct）：结合推理和任务特定操作，提高模型在回答问题和验证事实上的性能。

- 验证链（CoVe）：通过生成和验证问题，减少模型的逻辑错误和幻觉。

### 用户交互

- 主动提示（Active Prompting）：通过选择最具影响力的问题进行注释，优化模型在复杂任务上的性能。

### 微调与优化

- 自动提示工程（Automatic Prompt Engineer, APE）：自动生成和选择最优提示，提高模型在特定任务上的表现。

### 知识推理与生成

- 自动推理与工具使用（ART）：通过多步骤推理和外部工具的无缝集成，增强模型在复杂任务中的能力。

### 改善一致性和连贯性

- 对比链式推理提示（Contrastive CoT）：提供正确和错误的推理示例，帮助模型学习错误的原因，提高推理任务的准确性。

### 管理情绪和语气

- 情绪提示（Emotion Prompting）：通过加入情绪刺激句子，增强模型的情绪智能和任务表现。

### 代码生成与执行

- 草稿本提示（Scratchpad Prompting）：通过生成中间步骤，提高模型在复杂编程任务中的准确性。

- 程序化推理提示（Program of Thoughts, PoT）：使用外部语言解释器进行计算，提高模型在数学表达式求解中的表现。

### 优化与效率

- 通过提示优化（Optimization by Prompting, OPRO）：利用自然语言提示迭代生成解决方案，提高优化任务的性能。

### 理解用户意图

- 重述与响应提示（Rephrase and Respond, RaR）：通过重述和扩展问题，提高模型的理解和响应准确性。

### 元认知与自我反思

- 后退一步提示（Take a Step Back Prompting）：通过抽象和推理，增强模型在复杂多步骤任务中的推理能力。

## 结论

Prompt engineering已经成为解锁LLMs潜力的变革性力量。本文系统分类了29种prompt engineering技术，分析了其应用、模型和数据集，为研究人员和从业者提供了宝贵的资源。尽管取得了显著成功，仍需解决偏见、事实不准确和可解释性等挑战，未来的研究有望在元学习和混合提示架构等新兴趋势下进一步提升。

## 参考文献

（本文参考了大量关于prompt engineering的文献，详细列出了每种方法的出处和作者）
