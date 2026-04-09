# 快速开始

> 来源：[https://waytoagi.feishu.cn/docx/XNzmddkQ5ohP2Yximc7cImx7nwb](https://waytoagi.feishu.cn/docx/XNzmddkQ5ohP2Yximc7cImx7nwb)

## 概述

OpenAI API 可以应用于几乎所有涉及生成自然语言、代码或图像[的任](https://ywh1bkansf.feishu.cn/wiki/R70MwasSpik2tgkCr7dc9eTmn0o)务。我们提供了一系列不同能力级[别的 模型，适用于不同任务](https://ywh1bkansf.feishu.cn/wiki/ATYCwS5RRibGXNkvoC4ckddLnLf)的，并且能够 微调（Fine-tune）您自己的自定义模型。这些模型可以用于从内容生成到语义搜索和分类的所有领域。

## 提示词 Prompts

设计提示词本质上就是对模型进行“编程”，这通常是通过提供一些指令或几个示例来完成。这与大多数其他 NLP 服务不同，后者是为单个任务设计的，例如情绪分类或命名实体识别。相反，补全（Completions）和聊天补全（Chat Completions）几乎可用于任何任务，包括内容或代码生成、摘要、扩展、对话、创意写作、风格转换等。

## 标记 Token

我们的模型通过将文本分解为标记（Token）来理解和处理文本。Token 可以是单词，也可以是字符块。例如，单词“hamburger”被分解成标记“ham”、“bur”和“ger”，而很短且常见的单词像“pear”是一个 Token。许多 Token 以空格开头，例如“ hello”和“ bye”。

在给定的[ API ](https://platform.openai.com/tokenizer)请求中处理的 Token 数量取决于您的输入和输出长度。作为一个粗略的经验法则，对于英文文本，1 个Token大约相当于 4 个字符或 0.75 个单词。要记住的一个限制是，您的文本提示词和生成的补全合起来不能超过模型的最大上下文长度（对于大多数模型，这是 2048 个 Token，或大约 1500 个单词）。可以查看我们的 分词器工具 来了解有关文本如何转换为 Token 的更多信息。

## 模型 Models

API 由一组具有不同功能和价位的模型提供支持。GPT-4是我们最新和最强大的模型。GPT-3.5-Turbo 是为 ChatGPT 提供支持的模型，此模型专为对话模式进行了优化。要了解有关这些模型以及其他我们提供的内容的更多信息，请访问模型文档。

## 温度 Temperature

简单来说，temperature 的参数值越小，模型就会返回越确定的一个结果。如果调高该参数值，大语言模型可能会返回更随机的结果，也就是说这可能会带来更多样化或更具创造性的产出。我们目前也在增加其他可能 token 的权重。在实际应用方面，对于质量保障（QA）等任务，我们可以设置更低的 temperature 值，以促使模型基于事实返回更真实和简洁的结果。 对于诗歌生成或其他创造性任务，你可以适当调高 temperature 参数值。

---

翻译自 OpenAI官方文档链接：https://platform.openai.com/docs/quickstart