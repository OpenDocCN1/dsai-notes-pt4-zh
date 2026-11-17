
# 百度也来卷RAG了，Self-Reasoning比Self-RAG最高提升11.8%

> 来源：[https://agijuejin.feishu.cn/docx/HvVxd0aEcobZ1ixlY0KcRdNAngN](https://agijuejin.feishu.cn/docx/HvVxd0aEcobZ1ixlY0KcRdNAngN)

> 作者：PaperAgent

> 原文：[https://mp.weixin.qq.com/s/njeJk-tKs7_7zrSEYWoutw](https://mp.weixin.qq.com/s/njeJk-tKs7_7zrSEYWoutw)

检索增强型语言模型（Retrieval-Augmented Language Model, RALM）通过在推理过程中整合外部知识，减轻了LLM固有的事实幻觉问题。然而，RALMs 仍面临挑战：检索到的不相关文档可能导致无效的回答生成，生成的输出中缺乏适当的引用，使得验证模型的可信度变得复杂。

一个示例，展示了百度自我推理（Self-Reasoning）框架如何通过相关意识过程、证据意识选择过程和轨迹分析过程生成推理轨迹。

![](img/684bbb29a01cd5b4f5dd6cd8b830f936.png)

## SELF-REASONING 框架概述

为了解决上述问题，百度（Baidu Inc.）提出了一种新颖的自我推理框架（SELF-REASONING framework），旨在通过LLM自身生成的推理轨迹来提高RALMs的可靠性和可追溯性。它的创新之处在于通过端到端的框架直接增强LLMs的性能，而无需依赖外部的推理模型，从而提供了一种更有效和可扩展的解决方案。

对提高 RALMs 的自我推理框架的说明图。上层是基本的 LLMs，它们通过固有知识回答问题。中层是标准的检索增强型语言模型，它们使用检索到的文档帮助回答问题。底层是百度的自我推理框架，它使用自生成的推理轨迹来输出答案。

![](img/7986352d0c2616b5a01b93f6f0a65c66.png)

## 核心处理流程

### 相关性感知过程（Relevance-Aware Process, RAP）

- 选择默认的检索器（如 DPR 和 Contriever）来检索与问题相关的文档。
- 指导模型评估检索到的文档与问题的相关性，并生成解释为何这些文档被识别为相关的输出。
- 如果所有检索到的文档都不相关，模型应依据预训练阶段获得的内部知识提供答案。

### 证据感知选择过程（Evidence-Aware Selective Process, EAP）

- 要求模型从检索到的文档中明确选择关键句子作为证据，并输出选择这些句子的原因。
- 模型需要自动选择文档中的关键句子片段，并解释为何这些片段能够回答问题。

### 轨迹分析过程（Trajectory Analysis Process, TAP）

- 将前两个过程中生成的自我推理轨迹整合，形成推理片段链，以增强检索增强生成的整体性能。
- 要求模型分析这些推理轨迹，并最终输出简洁的分析和简短的答案。

## 数据生成与质量控制

此外，还包括数据生成和质量控制的过程：

- 训练数据生成：使用 GPT-4 生成答案作为基准真值，并创建正负样本对。
- 数据质量控制：通过自动验证工具和过滤不正确答案的轨迹，确保数据生成的正确性和完整性。

## 模型训练

- 使用标准的语言建模目标进行训练，通过最大化似然函数来训练自我推理生成模型。
- 提出了分阶段掩码策略的逐步训练方法，以逐步学习生成长推理轨迹。

## 实验评估

通过在四个公共数据集（两个短形式问答数据集、一个长形式问答数据集和一个事实验证数据集）上的评估，证明了该方法的优越性，能够超越现有的最先进模型，并且在仅使用 2,000 个训练样本的情况下，就能与 GPT-4 的性能相媲美。

在两个短形式问答数据集、一个长形式问答数据集和一个事实验证数据集上，与不同基线模型的性能比较。用粗体黑色表示的数字代表除了 GPT-4 之外的最佳结果。结果是基于五次运行的平均值，并且省略了标准差值（所有值均≤2%）。

![](img/4bbef88049f520d42b57546bd3383166.png)

SELF-REASONING 框架通过利用 LLM 自身生成的推理轨迹，在不依赖外部模型或工具的情况下，有效提高了 RALMs 的鲁棒性，并通过要求 LLM 明确生成文档片段和引用，增强了 RALMs 的可解释性和可追溯性。

在三个不同的数据集上的噪声鲁棒性实验结果：（a）左侧是 NQ 数据集，（b）中间是 PopQA 数据集，（c）右侧是 FEVER 数据集。Self-RAG 和 Vicuna 是具有 13B 参数大小的模型。

![](img/c1ba8cb1fc1c9407550a86b262d0d998.png)

## 附录

### GPT-4 生成短形式和长形式问答任务的自我推理轨迹 Prompt 模版

```
Instructions
# Role
You are an experienced expert, skilled in answering various questions.
# Task
Please answer the question according to the provided reference evidence as
required.
# Reference Evidence
[1] Retrieved Document {{DOCUMENT 1}[2] Retrieved Document {{DOCUMENT 2}[3] Retrieved Document {{DOCUMENT 3}[4] Retrieved Document {{DOCUMENT 4}[5] Retrieved Document {{DOCUMENT 5}
# Requirements
1. First, please judge whether the provided documents are relevant with the
question, and put it in the relevant field. If the provided content is irrelevant to
the question, explain the reason in the relevant reason field, then you can give
the answer with your internal knowledge.
2. If possible, answer the question in points and provide explanations.
3. If the content in the answer comes from different pieces of evidence, you
need to cite the sequence number of the evidence at the end of the sentence.
The citation format is shown below: [1], [1,3].
4. Place each cited piece of evidence in the cite_list field, cite content field to
store each paragraph of cited content (omitted words can be replaced by ...),
cite reason is used to store your thoughts and analysis of this content, how
this paragraph can answer the question.
5. Put the long answer content in the analysis field, and put the short
answer(no more than 10 words) in the answer field.
# Question
{{QUESTION}}
```

### GPT-4 生成事实验证任务的自我推理轨迹 Prompt 模版

```
Instructions
# Role
You are an experienced expert, skilled in answering various questions.
# Task
Please answer the question according to the provided reference evidence as
required.
# Reference Evidence
[1] Retrieved Document {{DOCUMENT 1}}
[2] Retrieved Document {{DOCUMENT 2}}
[3] Retrieved Document {{DOCUMENT 3}}
[4] Retrieved Document {{DOCUMENT 4}}
[5] Retrieved Document {{DOCUMENT 5}}
# Requirements
1. First, please judge whether the provided documents are relevant with the
claim, and put it in the relevant field. If the provided content is irrelevant to the
question, explain the reason in the relevant reason field, then you can give the
answer with your internal knowledge.
2. If possible, answer the question in points and provide explanations.
3. If the content in the answer comes from different pieces of evidence, you
need to cite the sequence number of the evidence at the end of the sentence.
The citation format is shown below: [1], [1,3].
4. Place each cited piece of evidence in the list, use cite content field to
store each paragraph of cited content (omitted words can be replaced by ...),
cite reason is used to store your thoughts and analysis of this content, how this
paragraph can answer the question.
5. Put the long answer content in the analysis field, and put the short answer
(SUPPORT/REFUTE/NOT ENOUGH INFO) in the answer field.
# Question
You should check whether the given claim is supported or refuted by above the
evidence. If supported，output SUPPORT, if controversial, output REFUTE; if
the evidence provided is not enough to make a decision or the provided
evidence is irrelevant, output NOT ENOUGH INFO.
{{CLAIM}}
```

## 案例研究

展示了 SELF-REASONING 框架在推理过程中生成的自我推理轨迹，并展示了它们如何逻辑上生成正确答案。

```
Question
When was catch me if you can made?

Retrieved document #1: The film due to her busy schedule. The original start date was
January 2002, but was pushed to February 7 in Los Angeles, California. Locations included
Burbank, Downey, New York City, Quebec City and Montreal. …

Retrieved document #2: eventually turned to him for help in catching other check forgers.
Development for the film started in 1980, but did not progress until 1997, when Spielberg&#x27;s
DreamWorks bought the film rights to Abagnale&#x27;s book. David Fincher, Gore Verbinski, Lasse
Hallström, Miloš Forman, and Cameron Crowe had all been possible candidates for director
before Spielberg decided to direct. …

Retrieved document #3: It was funded independently and had a budget of $800,000. The film
was Sommers&#x27;s directorial debut and was written by him. When production finished, the studio
that had promised to distribute the movie had gone out of business, and it was eventually
released by MCA Inc. in July 1989. The movie…
(Other retrieved documents are omitted.)

Raw LLM Answer: 2000 ❌

Standard RAG Answer: 1989 ❌

Self-Reasoning Answer:
(Relevant Aware Process)
Relevant: True
Relevant Reason: The provided documents are relevant with question.
(Evidence Aware Selective Process)

Cite content: [1] … the original start date was January 2002, but was pushed to February 7 in
Los Angeles, …
Reason to cite: This piece provides information on the commencement and location of filming
for &#x27;Catch Me If You Can&#x27;, indicating that it started in April 2002

Cite content: [3] … When production finished, the studio that had promised to distribute the
movie had gone out of business, and it was eventually released by MCA Inc. in July 1989., …
Reason to cite: This piece provides information on the release date of &#x27;Catch Me If You Can&#x27;,
indicating that it was eventually released by MCA Inc. in July.
(Trajectory Analysis Process)

Analysis:（long answer）
The film &#x27;Catch Me If You Can&#x27; was made in 2002. It started filming in April 2002 in Park Avenue,
just outside the Waldorf-Astoria Hotel, and moved to Orange, New Jersey, before returning to
Brooklyn for bank and courthouse scenes [1].

Final Answer：2002 ✅ （short answer）
```

```
https://arxiv.org/pdf/2407.19813
Improving Retrieval Augmented Language Model with Self-Reasoning
Baidu Inc., China.
```

## 推荐阅读

- 对齐 LLM 偏好的直接偏好优化方法：DPO、IPO、KTO
- 一篇搭建 AI 大模型应用平台架构的全面指南
