# 关于GPTs

> 来源：[https://aigcreative.feishu.cn/docx/ZYJHdGil4o10AUxH2x4ccpMBnCb](https://aigcreative.feishu.cn/docx/ZYJHdGil4o10AUxH2x4ccpMBnCb)

# 1、什么是GPTs？

GPTs（Generative Pre-trained Transformers）是一系列由OpenAI开发的自然语言处理模型，旨在通过深度学习技术理解和生成人类语言。GPT模型的核心思想是使用大量的文本数据对模型进行预训练，使其学习语言的广泛模式和结构，然后在特定任务上进行微调，以适应特定的应用需求。

### GPT模型的关键特征：

1.  Transformer架构：GPT模型基于Transformer架构，这是一种专为处理序列数据（如文本）设计的深度学习模型。Transformer通过注意力机制（Attention Mechanism）能够处理长距离依赖关系，并有效地捕捉序列中的上下文信息。

1.  大规模预训练：GPT模型在大量的文本数据上进行预训练，学习语法、词汇、事实知识、常识以及不同领域的专业知识。这种预训练过程使得GPT模型具有广泛的语言理解和生成能力。

1.  微调（Fine-tuning）：虽然预训练的GPT模型已经具备了强大的语言处理能力，但通常需要在特定任务的数据上进行微调，以优化其在该任务上的表现。微调过程涉及调整模型的参数，使其更好地适应特定应用的需求。

1.  生成能力：GPT模型能够生成连贯、自然的文本，这使得它们在内容创作、对话系统、文本摘要等应用中特别有用。

1.  多任务学习：由于GPT模型在大规模数据集上进行预训练，它们能够在没有针对性微调的情况下，对多种不同的任务展现出一定程度的理解和执行能力。

### GPT系列的发展：

+   GPT-1：OpenAI首次引入了GPT概念，展示了利用预训练和微调的方法在多项自然语言处理任务上取得令人瞩目的成绩。

+   GPT-2：扩大了模型规模和训练数据，进一步提高了文本生成的质量和多样性，展现了模型在无监督学习方面的潜力。

+   GPT-3：目前是最广为人知的版本之一，以其庞大的模型规模（1750亿个参数）和在多种任务上的出色表现而闻名。GPT-3展示了令人惊叹的少量学习（few-shot learning）能力，即在极少的示例指导下就能够理解并执行新任务。

+   后续版本：OpenAI持续在探索更先进的GPT模型，包括提升模型性能、优化训练效率和减少环境影响等方面的工作。

GPT系列模型因其强大的语言理解和生成能力，在各个领域中都找到了应用，包括自动文本生成、对话系统、内容摘要、机器翻译、代码生成等。同时，它们在开创性研究和商业应用中也发挥着重要作用。

# 2、如何搭建GPTs？

# 3、GPTs提示词模版：

Act like a [Specify a role]

你应该像[指定角色]一样操作

I need a [What do you need?]

我需要一个 [你需要什么？]

You will [Enter a task]

您将 [输入任务]

In the process, you should [Enter details]

在此过程中，你应该 [输入详细信息]

Please [Enter exclusion]

请[输入排除项]

input the final result in a [Select a format]

以[选择格式]输入最终结果

Here is an example: [Enter an example]

下面是一个示例：[输入示例]

**案例：**

Act like a SEO Professional Writer,

你是一个SEO专家

I need a optimized blog post,

我需要一篇优化的博客文章

you will research keywords and incorporate them naturally into the content,

你将研究关键字并将它们自然地融入内容中

in the process, you should focus on readability, relevance and proper keyword placement,

在这个过程中，你应该关注可读性、相关性和正确的关键词位置

Please avoid keyword stuffing or over-optimisation

请避免关键字堆砌或过度优化

input the final result in a well structured format,

以结构良好的格式输入最终结果

here is an example: title "Top 10 Tips for Effective SEO Writing: Boost Your Content's Visibility"

这里有一个例子：标题“有效SEO写作的10大技巧：提高你的内容的知名度”

完整prompt：

Act like a SEO Professional Writer, I need a optimized blog post, you will research keywords and incorporate them naturally into the content, in the process, you should focus on readability, relevance and proper keyword placement, please avoid keyword stuffing or over-optimisation, input the final result in a well structured format, here is an example: title "Top 10 Tips for Effective SEO Writing: Boost Your Content's Visibility"

<https://aigcreative.feishu.cn/sync/K4Gvd9vUQsy9Oubwqaeclu0Fnbg>