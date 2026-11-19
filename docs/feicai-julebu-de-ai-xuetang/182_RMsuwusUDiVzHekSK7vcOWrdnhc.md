
# 大型语言模型中提示工程方法在不同NLP任务中的调研

> 来源：[https://feicaiclub.feishu.cn/docx/DwKTdtf42oTE2Lx06HwcH6x8ngf](https://feicaiclub.feishu.cn/docx/DwKTdtf42oTE2Lx06HwcH6x8ngf)

![](img/071acf62c33d040a8f2bcc94a3002232.png)

[A SURVEY OF PROMPT ENGINEERING METHODS IN LARGE LANGUAGE MODELS FOR DIFFERENT NLP TASKS.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Vjfkb5NpUowAxPxhwxCcJn8unaf)

---

## 论文总结：《A SURVEY OF PROMPT ENGINEERING METHODS IN LARGE LANGUAGE MODELS FOR DIFFERENT NLP TASKS》

### 摘要

大型语言模型（LLM）在众多自然语言处理（NLP）任务中表现出色。提示工程通过编写自然语言指令（提示）来有组织地从LLM中提取知识，显著提升其在各种NLP任务上的表现。本文总结了不同的提示技术，并根据其应用的NLP任务进行分类。我们审阅了44篇研究论文，总结了39种提示方法在29个NLP任务上的应用。

### 引言

+ **背景**：LLM在医学、法律、金融等多个领域取得显著进展，当前研究主要关注通过提示增强其推理能力。
+ **提示工程**：提示工程利用LLM的嵌入知识，不需要广泛的参数重训练或微调，通过自然语言交互实现NLP任务。
+ **目标**：本文总结不同提示技术，并根据NLP任务进行分类，提供一个全面的资源和分类法。

## 提示工程技术

### 基本提示

+ **描述**：直接向LLM提出查询，不进行额外的工程改进。
+ **应用**：各种NLP任务，如基本问答和文本生成。

### 链式思维提示（CoT）

+ **描述**：通过中间推理步骤引导模型，增强其解决复杂问题的能力。
+ **应用**：数学问题解决和常识推理任务中显著提升性能。

### 自我一致性

+ **描述**：生成多个推理路径，通过一致性评估选择最可靠的答案。
+ **应用**：数学问题解决、常识推理和多跳推理任务。

### 集成优化（ER）

+ **描述**：结合多个生成的推理和答案，通过投票选择最终答案。
+ **应用**：上下文无关问答任务。

### 自动链式思维（Auto-CoT）

+ **描述**：自动生成高质量的训练数据点，消除人工数据点筛选过程。
+ **应用**：数学问题解决、多跳推理和常识推理任务。

### 复杂链式思维（Complex CoT）

+ **描述**：使用复杂数据点作为训练示例，提高模型的推理性能。
+ **应用**：数学问题解决、表格数学问题解决和多跳推理任务。

### 思维程序（PoT）

+ **描述**：生成Python程序，计算部分由Python解释器完成，减少模型负担。
+ **应用**：数学问题解决、表格数学问题解决和上下文问答任务。

### 从少到多（Least-to-Most）

+ **描述**：将问题分解为子问题，逐步解决，确保每个子问题的答案依赖于前一个子问题。
+ **应用**：常识推理、语言任务完成、数学问题解决和上下文问答任务。

### 符号链式思维（CoS）

+ **描述**：使用符号表示中间推理步骤，替代自然语言描述。
+ **应用**：空间问答任务。

### 结构化链式思维（SCoT）

+ **描述**：使用程序结构（如顺序、分支和循环）组织中间推理步骤，提高代码生成准确性。
+ **应用**：代码生成任务。

### 计划与解决（PS）

+ **描述**：先制定计划分解问题，再按计划执行，改进推理质量。
+ **应用**：数学问题解决、常识推理和多跳推理任务。

## 提示工程在不同NLP任务中的应用

本文通过分类法详细描述了提示工程在各类NLP任务中的应用及其性能表现。

### 数学问题解决

+ **应用**：GSM8K、MATH、SVAMP等数据集。
+ **技术**：PoT、Complex CoT、Contrastive CoT等在提高模型数学问题解决能力方面表现突出。

### 逻辑推理

+ **应用**：Word Sorting、Logical Deduction、Temporal Sequences等数据集。
+ **技术**：CoC、Analogical Reasoning等技术在逻辑推理任务中表现优异。

### 常识推理

+ **应用**：CSQA、Date Understanding、Sports Understanding等数据集。
+ **技术**：DecomP、Maieutic Prompting等技术在常识推理任务中效果显著。

### 多跳推理

+ **应用**：StrategyQA、HotpotQA、MuSiQue等数据集。
+ **技术**：Active-Prompt、CoK等技术在多跳推理任务中效果显著。

## 结论

本文系统性地回顾了提示工程技术，涵盖了定义、分类、实际应用和未来研究方向。提示工程通过增强LLM的推理能力，在多个NLP任务中展示了其潜力和应用价值。未来，提示工程将继续发展，推动LLM在更多领域中的应用。
