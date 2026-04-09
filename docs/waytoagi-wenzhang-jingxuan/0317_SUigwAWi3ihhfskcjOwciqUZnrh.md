# 模型（Models）

> 来源：[https://waytoagi.feishu.cn/docx/TG9Rd7gD5oF0wKxquRec49WZnOh](https://waytoagi.feishu.cn/docx/TG9Rd7gD5oF0wKxquRec49WZnOh)

## 概述

OpenAI API 由具有不同功[能和价位的多种模型提供支持。您](https://ywh1bkansf.feishu.cn/wiki/ATYCwS5RRibGXNkvoC4ckddLnLf)还可以通过微调（fine-tuning），针对您的特定用例对我们的原始基本模型进行有限的定制。

我们还发布了开源模型，包括[Point-E](https://github.com/openai/point-e)，[Whisper](https://github.com/openai/whisper)，[Jukebox](https://github.com/openai/jukebox)和[CLIP。](https://github.com/openai/CLIP)

访问 [供研究人员的模型索引](https://platform.openai.com/docs/model-index-for-researchers) 详细了解我们的研究论文中介绍了哪些模型以及 InstructGPT 和 GPT-3.5 等模型系列之间的差异。

## GPT-4 **Limited beta**

GPT-4 是一个大型多模态模型，由于其更广泛的一般知识和高级推理能力，它可以比我们以前的任何模型更准确地解决难题。与 gpt-3.5-turbo 一样，GPT-4 针对聊天进行了优化，但也适用于传统的补全（Completion）任务。在我们的 会话补全（Completion）指南 中了解如何使用 GPT-4。

注意：GPT-4 目前处于 Limited beta 阶段，只有获得访问权限的人才能访问。请加入候补名单，以便在容量可用时获得访问权限。

对于许多基本任务，GPT-4 和 GPT-3.5 模型之间的差异并不显著。但是，在更复杂的推理情况下，GPT-4 比我们以前的任何型号都更强大。

## GPT-3.5

GPT-3.5 模型可以理解和生成自然语言或代码。我们在 GPT-3.5 系列中功能最强大、最具成本效益的型号是gpt-3.5-turbo，它已针对聊天进行了优化，但也适用于传统的补全（Completion）任务。

我们建议使用 gpt-3.5-turbo 而不是其他 GPT-3.5 模型，因为它的成本更低。

::: tip 注意 OpenAI 模型是不确定的，这意味着相同的输入可以产生不同的输出。将 温度（Temperature ） 设置为 0 将使输出大部分具有确定性，但可能会保留少量可变性。 :::

OpenAI模型是非确定性的，这意味着相同的输入可以产生不同的输出。将温度设置为 0 将使输出大部分具有确定性，但可能会保留少量可变性。

## 特定功能模型

虽[然新的 ](https://ywh1bkansf.feishu.cn/wiki/OIBbwywg0i3RXrkXMDlcDmxRnPc)gpt-3.5-turbo 模型针对会话进行了优化，但它对于传统的完成任务也非常有效。原始的 GPT-3.5 模型针对 文本补全 进行了优化。

我们用于 [创建嵌入（Embedding）](https://ywh1bkansf.feishu.cn/wiki/M5C9wX65mi9OWCkc8JHcFIkFnXf) 和 [编辑文本](https://ywh1bkansf.feishu.cn/wiki/OIBbwywg0i3RXrkXMDlcDmxRnPc) 的端点它们自己的一组专用模型。

## 找到合适的模型

使用 gpt-3.5-turbo 进行试验是了解 API 功能的好方法。在您了解要实现的目标后，您可以继续使用 gpt-3.5-turbo 或其他模型并尝试围绕其功能进行优化。

您可以使用 GPT 比较工具，让您并排运行不同的模型来比较输出、设置和响应时间，然后将数据下载到 Excel 电子表格中。

## DALL·E

DALL·E 是一个人工智能系统，可以根据自然语言的描述创建逼真的图像和艺术作品。我们目前支持在提示的情况下创建具有特定大小的新图像、编辑现有图像或创建用户提供的图像的变体的能力。

通过我们的 API 提供的当前 DALL·E 模型是 DALL·E 的第 2 代，具有比[原始模](https://platform.openai.com/docs/guides/images/introduction)型更逼真、更准确且分辨率高 4 倍的图像。您可以通过我们的实验室界面 或 API 进行试用。

# Whisper

Whisper 是一种通用的语音识别模型。它在不同音频的大型数据集上进行训练，也是一个多任务模型，可以执行多语言语音识别以及语音翻译和语言识别。 目前可通过我们的 API（模型名 whisper-1）使用 Whisper v2-large 模型。

目前，[Whisper的开源版本](https://github.com/openai/whisper) 和通过我们的API提供的版本之间没有区别。但是，通过我们的 API，我们提供了一个优化的推理过程，这[使得通过](https://arxiv.org/abs/2212.04356)我们的 API 运行 Whisper 比通过其他方式运行要快得多。有关 Whisper 的更多技术细节，您可以 阅读论文。

## 嵌入（Embedding）

嵌入（Embedding）是文本的数字表示，可用于衡量两段文本之间的相关性。我们的第二代嵌入模型 text-embedding-ada-00[2 旨在以一](https://openai.com/blog/new-and-improved-embedding-model)小部分成本取代之前的 16 种第一代嵌入（Embedding）模型。嵌入（Embedding）可用于搜索、聚类、推荐、异常检测和分类任务。您可以在 公告博客文章 中阅读有关我们最新嵌入模型的更多信息。

## 模型兼容性

此列表不包括我们的第一代嵌入（Embedding）模型，也不包括我们的 DALL·E 模型。

## 持续的模型升级

随着 [gpt-3.5-turb](https://github.com/openai/evals)o 的发布，我们的一些模型现在正在不断更新。为了减少模型更改以意外方式影响我们用户的可能性，我们还提供将在 3 个月内保持静态的模型版本。随着模型更新的新节奏，我们还让人们能够贡献评估，以帮助我们针对不同的用例改进模型。如果您有兴趣，请查看 OpenAI Evals 存储库。

以下模型是将在指定日期弃用的临时快照。如果您想使用最新的模型版本，请使用标准模型名称，如 gpt-4 或 gpt-3.5-turbo 。