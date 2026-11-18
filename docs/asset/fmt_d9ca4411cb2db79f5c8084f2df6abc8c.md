
# CoT 最佳实践 (CoT Best practices)

对于 CoT 提示，将答案放在推理之后是必需的，因为推理的生成会改变模型预测最终答案时获得的令牌。

对于 CoT 和自我一致性，您需要能够从提示中提取最终答案，并将其与推理分开。

对于 CoT 提示，将温度设置为 0.1。思维链提示基于贪婪解码，根据语言模型分配的最高概率预测序列中的下一个词。一般来说，当使用推理来得出最终答案时，很可能只有一个正确的答案。因此，温度应始终设置为 0.1。有效使用 CoT 需要注意具体的实现细节，如答案放置、可提取性和适当的配置（低/零温度）。

## 记录各种提示尝试 (Document the various prompt attempts)

最后一条建议在本章前面已经提到过，但我们不能足够强调它的重要性：详细记录您的提示尝试，以便随着时间的推移了解哪些做得好，哪些做得不好。有效的提示工程需要将其视为科学或工程过程，这需要严格记录实验（提示、设置、结果），以便进行学习、比较和复现。

提示输出可能因模型、采样设置甚至同一模型的不同版本而异。此外，即使对于同一模型的相同提示，输出句子格式和措辞选择也可能存在微小差异。（例如，如前所述，如果两个令牌具有相同的预测概率，则可能随机打破平局。这随后会影响后续预测的令牌。）。

我们建议创建一个 Google Sheet，并使用表 21 作为模板。这种方法的优点是，当您不可避免地需要重新审视您的提示工作时——无论是将来重新拾起（您会惊讶于即使短暂休息后您会忘记多少），还是在不同版本的模型上测试提示性能，以及帮助调试未来的错误——您都有完整的记录。

除了此表中的字段外，跟踪提示的版本（迭代）、一个用于记录结果是“确定/不确定/有时确定”(`OK` / `NOT OK` / `SOMETIMES OK`) 的字段，以及一个用于记录反馈的字段也很有帮助。如果您有幸使用 Vertex AI Studio，请保存您的提示（使用与文档中列出的相同名称和版本），并在表格中跟踪指向已保存提示的超链接。这样，您只需单击一下即可重新运行您的提示。

当处理检索增强生成（RAG）系统时，您还应捕获影响哪些内容被插入到提示中的 RAG 系统的特定方面，包括查询、块设置、块输出和其他信息。

表 21：记录提示的模板 (A template for documenting prompts)

| 字段 | 描述 |
| --- | --- |
| 名称 (Name) | [您的提示的名称和版本] ([name and version of your prompt]) |
| 目标 (Goal) | [对此尝试目标的一句话解释] ([One sentence explanation of the goal of this attempt]) |
| 模型 (Model) | [使用的模型的名称和版本] ([name and version of the used model]) |
| 温度 (Temperature) | [0 - 1 之间的值] ([value between 0 - 1]) |
| 令牌限制 (Token Limit) | [数字] ([number]) |
| `Top-K` | [数字] ([number]) |
| `Top-P` | [数字] ([number]) |
| 提示 (Prompt) | [写下完整的提示] ([Write all the full prompt]) |
| 输出 (Output) | [写下输出或多个输出] ([Write out the output or multiple outputs]) |

此表为关键的文档实践提供了一个具体的、结构化的模板。遵循此模板可确保系统地捕获每个提示实验的关键信息（目标、输入、配置、输出），以便后续分析、比较和复现。

一旦您觉得提示接近完美，就将其纳入您的项目代码库。在代码库中，将提示保存在与代码分开的文件中，以便于维护。最后，理想情况下，您的提示是操作化系统的一部分，作为提示工程师，您应依赖自动化测试和评估程序来了解您的提示在任务上的泛化程度如何。

提示工程是一个迭代的过程。制作和测试不同的提示，分析并记录结果。根据模型的性能优化您的提示。持续实验，直到达到期望的输出。当您更改模型或模型配置时，返回并继续实验先前使用的提示。

## 总结 (Summary)

本白皮书讨论了提示工程。我们学习了各种提示技术，例如：

● 零样本提示 (Zero prompting)

● 少样本提示 (Few shot prompting)

● 系统提示 (System prompting)

● 角色提示 (Role prompting)

● 上下文提示 (Contextual prompting)

● 回退提示 (Step-back prompting)

● 思维链 (Chain of thought)

● 自我一致性 (Self consistency)

● 思维树 (Tree of thoughts)

● ReAct

● 自动提示工程 (APE)

我们甚至研究了如何自动化您的提示。

白皮书随后讨论了生成式 AI 的挑战，例如当您的提示不足时可能发生的问题。最后我们以如何成为更好的提示工程师的最佳实践作为结束。

## 尾注 (Endnotes)

1. Google, 2023, Gemini by Google. 可在 `https://gemini.google.com` 获取。

1. Google, 2024, Gemini for Google Workspace Prompt Guide. 可在 `https://inthecloud.withgoogle.com/gemini-for-google-workspace-prompt-guide/dl-cd.html` 获取。

1. Google Cloud, 2023, Introduction to Prompting. 可在 `https://cloud.google.com/vertex-ai/generative-ai/docs/learn/prompts/introduction-prompt-design` 获取。

1. Google Cloud, 2023, Text Model Request Body: `Top-P` & `top-K` sampling methods. 可在 `https://cloud.google.com/vertex-ai/docs/generative-ai/model-reference/text#request_body` 获取。

1. Wei, J., et al., 2023, Zero Shot - Fine Tuned language models are zero shot learners. 可在 `https://arxiv.org/pdf/2109.01652.pdf` 获取。

1. Google Cloud, 2023, Google Cloud Model Garden. 可在 `https://cloud.google.com/model-garden` 获取。

1. Brown, T., et al., 2023, Few Shot - Language Models are Few Shot learners. 可在 `https://arxiv.org/pdf/2005.14165.pdf` 获取。

1. Zheng, L., et al., 2023, Take a Step Back: Evoking Reasoning via Abstraction in Large Language Models. 可在 `https://openreview.net/pdf?id=3bq3jsvcQ1` 获取。

1. Wei, J., et al., 2023, Chain of Thought Prompting. 可在 `https://arxiv.org/pdf/2201.11903.pdf` 获取。

1. Google Cloud Platform, 2023, Chain of Thought and React. 可在 `https://github.com/GoogleCloudPlatform/generative-ai/blob/main/language/prompts/examples/chain_of_thought_react.ipynb` 获取。

1. Wang, X., et al., 2023, Self Consistency Improves Chain of Thought reasoning in language models. 可在 `https://arxiv.org/pdf/2203.11171.pdf` 获取。

1. Yao, S., et al., 2023, Tree of Thoughts: Deliberate Problem Solving with Large Language Models. 可在 `https://arxiv.org/pdf/2305.10601.pdf` 获取。

1. Yao, S., et al., 2023, ReAct: Synergizing Reasoning and Acting in Language Models. 可在 `https://arxiv.org/pdf/2210.03629.pdf` 获取。

1. Google Cloud Platform, 2023, Advance Prompting: Chain of Thought and React. 可在 `https://github.com/GoogleCloudPlatform/applied-ai-engineering-samples/blob/main/genaion-vertex-ai/advanced_prompting_training/cot_react.ipynb` 获取。

1. Zhou, C., et al., 2023, Automatic Prompt Engineering - Large Language Models are Human-Level Prompt Engineers. 可在 `https://arxiv.org/pdf/2211.01910.pdf` 获取。

## Works cited

1. `www.gptaiflow.tech`, accessed April 10, 2025, `https://www.gptaiflow.tech/assets/files/2025-01-18-pdf-1-TechAI-Goolge-whitepaper_Prompt%20Engineering_v4-af36dcc7a49bb7269a58b1c9b89a8ae1.pdf`
