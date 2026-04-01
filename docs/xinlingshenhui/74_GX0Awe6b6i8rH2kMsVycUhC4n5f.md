# GPT-4o 和 GPT-4 与 Gemini 1.5 ⭐ 的比较及性能分析

> 来源：[https://my.feishu.cn/docx/V3twdbRvOoXBJNx9BeCcnyU8n6g](https://my.feishu.cn/docx/V3twdbRvOoXBJNx9BeCcnyU8n6g)

![](img/10ebe5a3b2f8d7cb2896131dd23c5a87.png)

OpenAI 最近发布的 GPT-4o 为 AI 语言模型以及我们如何与它们交互的新时代奠定了基础。

最令人印象深刻的是支持与 ChatGPT 进行实时互动，并且可以中断对话。

尽管演示过程中出现了一些问题，但我还是忍不住对团队所取得的成就感到震惊。

最棒的是，演示结束后，OpenAI 立即允许访问 GPT-4o API。

在本文中，我将报告基于我创建的英语数据集对 Google GPT-4o、GPT 4 以及 Gemini 和 Unicorn 模型的分类能力进行的独立分析。

这些模型中哪个模型在理解英语方面最强大？

![](img/78ec53e38c7a69d675c14b05d9b0f741.png)

## GPT-4o 有什么新功能？

首先是全模型的概念，专为文本、音频和视频的感知和处理而设计。

看来 OpenAI 已将重点转向让 GPT-4 可供大众使用，甚至让免费用户也可以使用 GPT-4 级别的语言模型。

OpenAI 还宣布 GPT-4o 提高了 50 多种语言的质量和速度，承诺以更低的成本提供更全面、全球可访问的 AI 体验。

他们还提到，付费订阅者将获得比免费用户多五倍的功能。

此外，还将发布ChatGPT桌面版，让大众更方便地体验实时音频、视频和文本界面。

## 如何使用 GPT-4o API

新的 GPT-4o 模型遵循 OpenAI 的现有 API，使其向后兼容且易于使用。

> 从 openai 导入 OpenAI OPENAI_API_KEY = “<your-api-key>” def openai_chat_resolve（response：dict，strip_tokens = None）-> str：如果 strip_tokens 为 None：strip_tokens = [] 如果 response 和 response.choices 和 len（response.choices）> 0：content = response.choices[0].message.content.strip（）如果 content 不是 None 或 content ！=''：如果 strip_tokens：对于 strip_tokens 中的 token：content = content.replace（token，''）返回内容引发异常（f'无法解析响应：{response}'）def openai_chat_request（prompt：str，model_name：str，temperature=0.0）：message = {'role'：'user'，'content'：prompt} client = OpenAI（api_key=OPENAI_API_KEY）返回客户端.chat.completions.create（模型=model_name，消息=[消息]，温度=温度，）响应=openai_chat_request（prompt =“你好！”，model_name =“gpt-4o-2024-05-13”）答案=openai_chat_resolve（响应）打印（答案）

GPT-4o 也可通过 ChatGPT 界面获得：

![](img/7da9ddfecf4a16b181702dfc50283f0b.png)

## 官方评级

OpenAI 博客发布了对 MMLU 和 HumanEval 等知名数据集的评估。

![](img/c28bd1ce7da3d58f8263bfe3b43fa3a1.png)

从图中可以看出，GPT-4o 的性能属于该领域最先进的水平，考虑到新型号更便宜、速度更快，这听起来非常有前途。

然而，在过去的一年里，我看到许多模型声称在已知数据集上具有最先进的语言性能。

事[实上](https://arxiv.org/pdf/2402.01781)，其中一些模型在这些公共数据集上进行了部分训练（或过度训练），导致了不切实际的排行榜分数。如果您有兴趣，请查看这篇文章。

因此，在鲜为人知的数据集（例如我创建的数据集）上对这些模型的性能进行独立分析非常重要。

## 我的测试数据集🔢。

我创建了一个专题[数据集](https://github.com/LarsChrWiik/lars_datasets/tree/main/topics_dataset_50)，我们可以用它来评估不同法学硕士的分类性能。

该集合由分布在 50 个主题的 200 个句子组成，其中一些主题密切相关，使分类任务变得复杂。

我手动创建并用英语标记了整个数据集。

然后，我使用 GPT4 (gpt-4-0613) 将数据集翻译成多种语言。

+   **然而，在本次评估**中，我们将仅评估数据集的英文版本这意味着结果不应受到使用相同语言模型创建数据集和预测主题而产生的潜在偏差的影响。

## 工作成果📊

我决定评估以下模型：

+   **GPT-4o**： gpt-4o-2024-05-13

+   **GPT-4**：GPT-4-0613

+   **GPT-4-Turbo** : gpt-4-turbo-2024-04-09

+   **双子座 1.5 专业版**：gemini-1.5-pro-preview-0409

+   **双子座 1.0**：gemini-1.0-pro-002

+   **Palm 2 Unicorn** : text-unicorn@001

语言模型的任务是将数据集中的每个句子与正确的主题相匹配。

这使我们能够计算每种语言的准确率和每种模型的错误率。

由于模型大多分类正确，因此我绘制了每个模型的错误率。

请记住，错误率越低表明模型性能越好。

![](img/ad8813ba92056004036a01f01a6208bb.png)

根据该图，我们可以得出结论，GPT-4o 在所有模型中具有最低的错误率——只有 2 个错误。

我们还看到 Palm 2 Unicorn、GPT-4 和 Gemini 1.5 接近 GPT-4o，表明它们的性能较高。

有[趣的是，](https://platform.openai.com/docs/models)GPT-4 Turbo 的工作原理与 GPT-4-0613 类似。有关 OpenAI 模型的更多信息可以在模型页面上找到。

最后，Gemini 1.0 滞后，考虑到其价格范围，这是可以预料的。

对独特的英语数据集进行的分析揭示了这些尖端语言模型的最先进的功能。

OpenAI 的最新产品 GPT-4o 在测试的模型中具有最低的错误率，支持了 OpenAI 对其性能的声称。

人工智能社区和用户应继续使用不同的数据集进行独立评估，因为它们比标准化基准提供的模型的实际性能更清晰。

请注意，数据集非常小，结果可能会因数据集而异。我们只使用了英文数据集，但是多种语言的比较就得等到下次了。