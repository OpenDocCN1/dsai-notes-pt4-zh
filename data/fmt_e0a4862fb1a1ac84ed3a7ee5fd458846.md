
# 2303.09014v1_Automatic multi-step reasoning and tool-use for large language models

> 来源：`https://aithinkup.feishu.cn/docx/Stu9dtNxXoRwUax8LbVcyEYUnsb`
>
> 文档AI含量：100%。没有读过计算机科学的论文，使用AI工具的一次试验。欢迎大家给意见，我来改进。

源文件下载地址：

`https://sat2tjmapq6.feishu.cn/record/F4OgrcqhoeZgWoch9wccxA43nyh`

## 基础文档分析

### 📄 文件名

`2303.09014v1.pdf`

### 📚 主题

Automatic multi-step reasoning and tool-use for large language models

### 💎 关键词

Large language models (`LLMs`), Automatic Reasoning and Tool-use (`ART`), intermediate chain of thought (`CoT`), external tools, task library, multi-step reasoning, code execution

### 🔍 内容摘要

本文介绍了一种名为`Automatic Reasoning and Tool-use (ART)`的框架，它利用冻结的大型语言模型(`LLMs`)自动生成中间推理步骤作为程序。

`ART`从任务库中选择多步推理和工具使用的示例，以解决新任务。在测试时，`ART`会在每次调用外部工具时暂停生成，并在继续生成之前整合它们的输出。`ART`在`BigBench`和`MMLU`基准测试中，在未见任务上取得了显著的性能提升，并在大多数任务上与手工制作的`CoT`提示的性能相匹配。

此外，`ART`易于扩展，允许人类通过纠正任务特定程序中的错误或添加新工具来提高性能，通过最小的人工干预显著提高选定任务的性能。

### 🗂️ 文章大纲

+ 引言
+ `ART`框架介绍
+ 任务库(`Task Library`)
+ 工具库(`Tool Library`)
+ 人类反馈(`Human Feedback`)
+ 实验设置
+ 评估数据集
+ 实施细节
+ 基线比较
+ 结果
+ 任务库中的结果
+ 跨任务转移的测试任务
+ 提高`ART`性能的方法
+ 自我一致性(`Slef-consistency`)
+ 人工反馈(`Human Feedback`)
+ 结论

### 📍 重要段落

+ "ART, a framework that uses frozen LLMs to automatically generate intermediate reasoning steps as a program."
+ "Given a new task to solve, ART selects demonstrations of multistep reasoning and tool use from a task library."
+ "ART achieves a substantial improvement over few-shot prompting and automatic CoT on unseen tasks in the BigBench and MMLU benchmarks."
+ "ART is also extensible, and makes it easy for humans to improve performance by correcting errors in task-specific programs or incorporating new tools."

### 📃 参考文献

引用了多篇关于大型语言模型、推理链提示、工具使用、微调技术等相关研究的文献。

### 📅 文件日期

16 Mar 2023 - 根据文档中的`arXiv`提交日期确定。
