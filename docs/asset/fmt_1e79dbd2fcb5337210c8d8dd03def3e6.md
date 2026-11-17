
# 04. 理解提示工程基础

> 来源：[https://gotoaiworld.feishu.cn/docx/PhF9dn8FeooBdKxRxe8cNFatnQf](https://gotoaiworld.feishu.cn/docx/PhF9dn8FeooBdKxRxe8cNFatnQf)

## [介绍](https://microsoft.github.io/generative-ai-for-beginners/#/translations/zh/04-prompt-engineering-fundamentals/README?wt.mc_id=academic-105485-koreyst&id=%e4%bb%8b%e7%bb%8d)

本模块涵盖了在生成式 AI 模型中创建有效提示词的基本概念和技巧。你向大型语言模型（LLM）编写提示词的方式同样重要。精心设计的提示词能够获得更高质量的回复。但“提示词”和“提示工程”这些术语到底是什么意思？我该如何改进发送给 LLM 的提示词输入？这些问题将在本章及下一章中为你解答。

生成式 AI 能够根据用户请求创造新的内容（例如文本、图像、音频、代码等）。它通过使用像 OpenAI 的 GPT（“生成式预训练变换器”）系列这样的大型语言模型实现，这些模型经过自然语言和代码的训练。

用户现在可以通过类似聊天的熟悉方式与这些模型交互，无需任何技术专长或培训。这些模型是基于提示词的——用户发送文本输入（提示词），模型返回 AI 回复（完成内容）。用户可以通过多轮对话不断“与 AI 聊天”，不断优化提示词，直到回复符合预期。

“提示词”现已成为生成式 AI 应用的主要编程接口，告诉模型该做什么，并影响返回回复的质量。“提示工程”是一个快速发展的研究领域，专注于设计和优化提示词，以实现规模化的稳定且高质量的回复。

## [学习目标](https://microsoft.github.io/generative-ai-for-beginners/#/translations/zh/04-prompt-engineering-fundamentals/README?wt.mc_id=academic-105485-koreyst&id=%e5%ad%a6%e4%b9%a0%e7%9b%ae%e6%a0%87)

本课将介绍什么是提示工程、它为何重要，以及如何为特定模型和应用目标设计更有效的提示词。我们将理解提示工程的核心概念和最佳实践，并了解一个交互式 Jupyter Notebook “沙盒”环境，在那里可以看到这些概念如何应用于实际示例。

完成本课后，你将能够：

1. 解释什么是提示工程及其重要性。

2. 描述提示词的组成部分及其用途。

3. 学习提示工程的最佳实践和技巧。

4. 使用 OpenAI 端点将所学技巧应用于实际示例。

## [关键词](https://microsoft.github.io/generative-ai-for-beginners/#/translations/zh/04-prompt-engineering-fundamentals/README?wt.mc_id=academic-105485-koreyst&id=%e5%85%b3%e9%94%ae%e8%af%8d)

提示工程：设计和优化输入以引导 AI 模型生成期望输出的实践。

分词（Tokenization）：将文本转换为模型可以理解和处理的更小单元（称为“`token`”）的过程。

指令调优大型语言模型（Instruction-Tuned LLMs）：经过特定指令微调以提升回复准确性和相关性的大型语言模型。

## [学习沙盒](https://microsoft.github.io/generative-ai-for-beginners/#/translations/zh/04-prompt-engineering-fundamentals/README?wt.mc_id=academic-105485-koreyst&id=%e5%ad%a6%e4%b9%a0%e6%b2%99%e7%9b%92)

提示工程目前更多是一门艺术而非科学。提升直觉的最佳方式是多加练习，采用结合应用领域专业知识、推荐技巧和模型特定优化的反复试验方法。

本课配套的 Jupyter Notebook 提供了一个沙盒环境，你可以在学习过程中或课后代码挑战中尝试所学内容。执行练习需要：

1. Azure OpenAI API 密钥——已部署 LLM 的服务端点。

2. `Python` 运行环境——用于执行 Notebook。

3. 本地环境变量——请先完成 [`SETUP`](https://microsoft.github.io/generative-ai-for-beginners/#/translations/zh/00-course-setup/SETUP?wt.mc_id=academic-105485-koreyst) 步骤以准备环境。

Notebook 附带了入门练习，但鼓励你添加自己的`Markdown`（描述）和代码（提示请求）部分，尝试更多示例或想法，培养提示设计的直觉。

## [图解指南](https://microsoft.github.io/generative-ai-for-beginners/#/translations/zh/04-prompt-engineering-fundamentals/README?wt.mc_id=academic-105485-koreyst&id=%e5%9b%be%e8%a7%a3%e6%8c%87%e5%8d%97)

想在深入学习前先了解本课的整体内容吗？看看这份图解指南，它帮助你把握主要主题和每个主题的关键要点。课程路线图将带你从理解核心概念和挑战，到用相关提示工程技巧和最佳实践应对这些挑战。请注意，本指南中的“高级技巧”部分涉及本课程下一章的内容。

![](img/980c3ac26dbdea765549eca40228f695.png)

## [我们的创业项目](https://microsoft.github.io/generative-ai-for-beginners/#/translations/zh/04-prompt-engineering-fundamentals/README?wt.mc_id=academic-105485-koreyst&id=%e6%88%91%e4%bb%ac%e7%9a%84%e5%88%9b%e4%b8%9a%e9%a1%b9%e7%9b%ae)

[现在，让我们谈谈本主题](https://educationblog.microsoft.com/2023/06/collaborating-to-bring-ai-innovation-to-education?WT.mc_id=academic-105485-koreyst)如何与我们创业项目的使命相关——将 AI 创新带入教育。我们希望构建基于 AI 的个性化学习应用——所以让我们思考不同用户如何“设计”提示词：

+ 管理员可能会让 AI 分析课程数据以识别覆盖的空白点。AI 可以总结结果或用代码进行可视化。

+ 教育者可能会让 AI 为特定受众和主题生成课程计划。AI 可以以指定格式构建个性化计划。

+ 学生可能会让 AI 辅导他们攻克难题。AI 现在可以根据学生水平提供课程、提示和示例。

这只是冰山一角。查看[教育提示词库](https://github.com/microsoft/prompts-for-edu/tree/main?WT.mc_id=academic-105485-koreyst)——由教育专家策划的开源提示词库，了解更多可能性！试着在沙盒或 OpenAI Playground 中运行这些提示词，看看效果！

## [什么是提示工程？](https://microsoft.github.io/generative-ai-for-beginners/#/translations/zh/04-prompt-engineering-fundamentals/README?wt.mc_id=academic-105485-koreyst&id=%e4%bb%80%e4%b9%88%e6%98%af%e6%8f%90%e7%a4%ba%e5%b7%a5%e7%a8%8b%ef%bc%9f)

我们从定义提示工程开始，它是为特定应用目标和模型设计和优化文本输入（提示词），以提供稳定且高质量回复（完成内容）的过程。可以把它看作一个两步走的过程：

+ 为特定模型和目标设计初始提示词

+ 反复优化提示词以提升回复质量

这必然是一个反复试验的过程，需要用户的直觉和努力才能获得最佳结果。那么它为何重要？为回答这个问题，我们先了解三个概念：

+ 分词 = 模型如何“看待”提示词

+ 基础 LLM = 基础模型如何“处理”提示词

+ 指令调优 LLM = 模型如何“理解任务”

### [分词](https://microsoft.github.io/generative-ai-for-beginners/#/translations/zh/04-prompt-engineering-fundamentals/README?wt.mc_id=academic-105485-koreyst&id=%e5%88%86%e8%af%8d)

LLM 将提示词视为一串`token`，不同模型（或同一模型的不同版本）对同一提示词的分词方式可能不同。由于 LLM 是基于`token`（而非原始文本）训练的，提示词的分词方式直接影响生成回复的质量。

想了解分词如何工作，可以试试下面的OpenAI Tokenizer工具。复制你的提示词，观察它如何被转换成`token`，注意空格和标点的处理方式。请注意，此示例使用的是较旧的 LLM（GPT-3），用新模型测试可能结果不同。

### [概念：基础模型](https://microsoft.github.io/generative-ai-for-beginners/#/translations/zh/04-prompt-engineering-fundamentals/README?wt.mc_id=academic-105485-koreyst&id=%e6%a6%82%e5%bf%b5%ef%bc%9a%e5%9f%ba%e7%a1%80%e6%a8%a1%e5%9e%8b)

提示词分词后，[基础 LLM](https://blog.gopenai.com/an-introduction-to-base-and-instruction-tuned-large-language-models-8de102c785a6?WT.mc_id=academic-105485-koreyst)（或基础模型）的主要功能是预测序列中的下一个`token`。由于 LLM 训练于海量文本数据，它们对`token`之间的统计关系有较好把握，能较自信地做出预测。请注意，它们并不理解提示词或`token`的含义，只是看到一个可以“补全”的模式。它们会持续预测序列，直到用户干预或达到预设条件。

想看看基于提示词的完成是如何工作的吗？将上述提示词输入 Azure [OpenAI Studio](https://oai.azure.com/playground?WT.mc_id=academic-105485-koreyst) 的Chat Playground，使用默认设置。系统将提示词视为信息请求，你应该会看到符合该上下文的回复。

但如果用户想看到符合某些条件或任务目标的特定内容呢？这时就需要用到指令调优的 LLM。

### [概念：指令调优 LLM](https://microsoft.github.io/generative-ai-for-beginners/#/translations/zh/04-prompt-engineering-fundamentals/README?wt.mc_id=academic-105485-koreyst&id=%e6%a6%82%e5%bf%b5%ef%bc%9a%e6%8c%87%e4%bb%a4%e8%b0%83%e4%bc%98-llm)

[指令调优 LLM](https://blog.gopenai.com/an-introduction-to-base-and-instruction-tuned-large-language-models-8de102c785a6?WT.mc_id=academic-105485-koreyst)是在基础模型基础上，通过示例或输入/输出对（例如多轮“消息”）进行微调，这些示例包含明确指令，AI 的回复尝试遵循这些指令。

这采用了如`RLHF`等技术，训练模型遵循指令并从反馈中学习，使其生成更适合实际应用且更符合用户目标的回复。

我们来试试——回到上面的提示词，将系统消息改为以下指令作为上下文：

> 为二年级学生总结你收到的内容。结果保持为一段话，包含3-5个要点。

看看结果如何调整以反映期望的目标和格式？教育者现在可以直接将此回复用于课堂幻灯片。

## [为什么需要提示工程？](https://microsoft.github.io/generative-ai-for-beginners/#/translations/zh/04-prompt-engineering-fundamentals/README?wt.mc_id=academic-105485-koreyst&id=%e4%b8%ba%e4%bb%80%e4%b9%88%e9%9c%80%e8%a6%81%e6%8f%90%e7%a4%ba%e5%b7%a5%e7%a8%8b%ef%bc%9f)

既然我们知道提示词是如何被 LLM 处理的，接下来谈谈为什么需要提示工程。原因在于当前 LLM 存在一些挑战，使得可靠且一致的完成内容难以在不投入提示构建和优化工作的情况下实现。例如：

1. 模型回复具有随机性。相同的提示词在不同模型或模型版本中可能产生不同回复，甚至同一模型在不同时间也可能产生不同结果。提示工程技巧能帮助我们通过更好的约束减少这些差异。

2. 模型可能编造回复。模型预训练于庞大但有限的数据集，缺乏训练范围外的知识。因此，它们可能生成不准确、虚构或与已知事实直接矛盾的内容。提示工程技巧帮助用户识别并减轻此类编造，例如要求 AI 提供引用或推理。

3. 模型能力各异。新模型或新一代模型功能更强，但也带来成本和复杂性的独特权衡。提示工程能帮助我们制定最佳实践和工作流程，抽象差异并适应模型特定需求，实现可扩展且无缝的应用。

让我们在 OpenAI 或 Azure OpenAI Playground 中实际体验：

+ 用相同提示词测试不同 LLM 部署（如 OpenAI、Azure OpenAI、Hugging Face）——你发现了哪些差异？

+ 用相同提示词多次测试同一 LLM 部署（如 Azure OpenAI Playground）——这些差异如何变化？

### [编造示例](https://microsoft.github.io/generative-ai-for-beginners/#/translations/zh/04-prompt-engineering-fundamentals/README?wt.mc_id=academic-105485-koreyst&id=%e7%bc%96%e9%80%a0%e7%a4%ba%e4%be%8b)

在本课程中，我们用“[编造](https://www.microsoft.com/ai/responsible-ai?WT.mc_id=academic-105485-koreyst)”一词指代 LLM 有时因训练限制或其他约束而生成事实错误信息的现象。你可能在流行文章或研究论文中听过“幻觉”（hallucinations）一词，但我们强烈建议使用“编造”，以避免将机器行为拟人化，赋予其人类特质。这也符合负责任 AI 指南的术语规范，避免使用在某些语境下可能被视为冒犯或不包容的词汇。

想了解编造是如何发生的吗？想象一个提示词，指示 AI 生成一个不存在主题的内容（确保该主题不在训练数据中）。例如——我尝试了这个提示词：

### [火星战争2076年课程计划](https://microsoft.github.io/generative-ai-for-beginners/#/translations/zh/04-prompt-engineering-fundamentals/README?wt.mc_id=academic-105485-koreyst&id=%e7%81%ab%e6%98%9f%e6%88%98%e4%ba%892076%e5%b9%b4%e8%af%be%e7%a8%8b%e8%ae%a1%e5%88%92)

#### [课程目标](https://microsoft.github.io/generative-ai-for-beginners/#/translations/zh/04-prompt-engineering-fundamentals/README?wt.mc_id=academic-105485-koreyst&id=%e8%af%be%e7%a8%8b%e7%9b%ae%e6%a0%87)

+ 了解2076年火星战争的背景和起因

+ 分析战争的主要事件和关键战役

+ 探讨战争对地球和火星社会的影响

+ 培养学生的批判性思维和历史分析能力

#### [课程大纲](https://microsoft.github.io/generative-ai-for-beginners/#/translations/zh/04-prompt-engineering-fundamentals/README?wt.mc_id=academic-105485-koreyst&id=%e8%af%be%e7%a8%8b%e5%a4%a7%e7%ba%b2)
