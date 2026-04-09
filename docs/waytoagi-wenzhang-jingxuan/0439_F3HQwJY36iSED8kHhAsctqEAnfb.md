# 4.Use examples 使用例子

> 来源：[https://waytoagi.feishu.cn/docx/N2vYdlKZFoxQEAxAKoucmDCPnGY](https://waytoagi.feishu.cn/docx/N2vYdlKZFoxQEAxAKoucmDCPnGY)

> Examples are one of the most powerful tools for enhancing Claude's performance and guiding it to produce your desired output. By providing a few well-crafted examples in your prompt, you can significantly improve the accuracy, consistency, and quality of Claude's responses. This technique is particularly effective for tasks that are highly detailed or require structured outputs or adherence to specific formats.

> 示例是提高 Claude 性能并引导其生成您期望输出的最强大工具之一。通过在提示中提供一些精心制作的示例，您可以显著提高 Claude 响应的准确性、一致性和质量。这种技术特别适用于需要高度详细或需要结构化输出或遵守特定格式的任务。

> This technique is also known as few-shot prompting (or one-shot prompting if only one example is provided).

> 这种技术也被称为少拍提示（如果只提供一个示例，则称为一拍提示）。

---

# Why use examples? 为什么使用示例？

Examples serve as a learning tool for Claude, demonstrating exactly what kind of output you expect. They often are far more effective than simply describing or providing instructions, as they allow Claude to generalize patterns and apply them to new inputs. Just as humans often learn best by observing and imitating, Claude can quickly grasp the desired behavior by studying relevant examples.

例子作为 Claude 的学习工具，展示了您期望的输出类型。它们通常比简单描述或提供说明更有效，因为它们允许 Claude 概括模式并将其应用于新的输入。正如人类经常通过观察和模仿来学习一样， Claude 可以通过研究相关示例迅速掌握所需的行为。

Here are some key benefits of using examples in your prompts:

在您的提示中使用示例的一些关键好处如下：

+   Improved accuracy: Examples help Claude understand precisely what you want, reducing the likelihood of misinterpretation or irrelevant responses.
提高准确性：示例帮助 Claude 准确理解您的意图，减少误解或无关回复的可能性。

+   Increased consistency: By providing a template for Claude to follow, examples ensure that responses maintain a consistent structure and style across different inputs.
提高一致性：通过为 Claude 提供一个模板，示例确保响应在不同输入中保持一致的结构和风格。

+   Enhanced performance: Well-chosen examples can significantly boost Claude's ability to handle complex or nuanced tasks, as it learns from the patterns and logic demonstrated in the examples.
提升性能：精心选择的示例可以显著提升 Claude 处理复杂或微妙任务的能力，因为它从示例中展示的模式和逻辑中学习。

---

# **Crafti**ng effective examples 制作有效示例

To get the most out of using examples in your prompts, consider the following guidelines on how to provide the most effective examples:

为了充分利用在您的提示中使用示例，考虑以下关于如何提供最有效示例的准则：

+   Relevance: Ensure that your examples closely resemble the types of inputs and outputs you expect Claude to handle. The more similar the examples are to your actual use case, the better Claude will perform.
相关性：确保您的示例与您期望 Claude 处理的输入和输出类型非常相似。示例与您实际用例越相似，Claude 的性能就会越好。

+   Diversity: Include a variety of examples that cover different scenarios, edge cases, and potential challenges. This helps Claude generalize better and handle a wider range of inputs.
多样性：包括涵盖不同场景、边缘情况和潜在挑战的各种示例。这有助于 Claude 更好地泛化并处理更广泛范围的输入。

+   Clarity: Make your examples clear, concise, and easy to understand. Use formatting tags like <example> to structure your examples and distinguish them from the rest of the prompt. Give Claude context as to what kind of example it's about to encounter when possible (e.g., Here are some examples of proper APA citations or Here are some examples of emails I've written).
清晰度：使您的示例清晰、简洁且易于理解。使用格式标记如 <example> 来构建您的示例，并将其与提示的其余部分区分开。尽可能为 Claude 提供示例即将遇到的上下文（例如， Here are some examples of proper APA citations 或 Here are some examples of emails I've written ）。

+   Quantity: While there's no hard rule for the optimal number of examples, aim to provide at least 3-5 examples to start to give Claude a solid foundation. You can always add more targeted examples if Claude's performance isn't meeting your expectations.
数量：虽然没有关于最佳示例数量的硬性规定，但至少提供 3-5 个示例以开始为 Claude 打下坚实基础。如果克劳德的表现未达到您的期望，您可以随时添加更多有针对性的示例。

To provide examples, simply include them in your prompt, clearly distinguishing them from the actual task. We recommend using <example></example> tags to wrap your examples, making it easy for Claude to differentiate between the examples and the rest of the prompt.

为了提供示例，只需将它们清晰地包含在您的提示中，与实际任务区分开。我们建议使用 <example></example> 标签来包裹您的示例，这样 Claude 就可以轻松区分示例和提示的其余部分。

Here's a prompt that demonstrates the use of examples to guide Claude's response:

这是一个示例，演示了如何使用示例来引导 Claude 的回答：

In this prompt, the example provides Claude with guidance on how to extract the author's name from a given quote, making it easy for Claude to replicate the process on a new input.

在此提示中，示例为 Claude 提供了如何从给定引用中提取作者姓名的指导，使 Claude 能够轻松地在新输入上复制该过程。

---

# Formatting outputs 格式化输出

Examples are particularly effective for tasks that require structured or formatted outputs. Sometimes, instead of providing step-by-step formatting instructions, you can simply include a few examples of the desired output format (although we recommend having both instructions and examples together, as that is likely to be more effective than one without the other).

例子特别适用于需要结构化或格式化输出的任务。有时，您可以简单地包括一些期望的输出格式示例，而不是提供逐步的格式化说明（尽管我们建议将说明和示例一起使用，因为这样比单独使用更有效）。

Suppose you want Claude to extract names and professions from a given text and format them as a list. Here's how you might prompt Claude with examples:

假设您希望让 Claude 从给定的文本中提取姓名和职业，并将它们格式化为列表。以下是您可能如何使用示例提示 Claude:

By observing these examples, Claude learns to extract the relevant information and format it as a numbered list with names and professions in the desired style.

通过观察这些例子，Claude 学会了提取相关信息，并按照所需风格将其格式化为带有姓名和职业的编号列表。

---

# A word of caution 警告

While examples are incredibly powerful, it's important to be mindful of potential pitfalls. Claude may sometimes pick up on unintended patterns in your examples, leading to overfitting or unexpected behaviors.

虽然示例非常强大，但我们要注意潜在的陷阱。Claude 有时可能会捕捉到示例中意外的模式，导致过度拟合或意外行为。

For instance, if all your example emails end with "Sincerely," Claude might learn to always sign off that way for emails it generates, even if it's not appropriate for every case. To mitigate this, ensure your examples are diverse and representative of the full range of desired outputs.

例如，如果你所有示例的邮件结尾都是“真诚地”，Claude 可能会学习到总是在它生成的邮件中以这种方式签名，即使在不适用的情况下也是如此。为了减轻这种情况，确保你的示例多样化，并且代表所需输出的全部范围。

---

# Iterating and refining 迭代和改进

Crafting the perfect set of examples often involves iteration and refinement. If Claude's performance isn't quite meeting your expectations, consider the following:

制作完美的示例集通常需要反复迭代和完善。如果 Claude 的表现没有达到您的期望，考虑以下事项：

+   Analyze the output: Look for patterns in Claude's responses that deviate from what you want. This can help you identify areas where your examples might be unclear or misleading, or where more examples might help.
分析输出：查找 Claude 的响应中偏离您期望的模式。这有助于您确定示例可能不清晰或误导的地方，或者需要更多示例的地方。

+   Add more examples: If Claude struggles with certain types of inputs, provide additional examples that specifically address those scenarios.
添加更多示例：如果 Claude 在某些类型的输入上遇到困难，请提供额外的示例，专门针对这些情景。

+   Revise existing examples: Sometimes, even small tweaks to your examples can make a big difference. Experiment with different wordings, formats, or structures to see what works best.
修改现有示例：有时，即使对示例进行小的调整也可能产生很大的影响。尝试不同的措辞、格式或结构，看看哪种效果最好。

+   Get Claude's help: Writing good examples is hard! You can ask Claude to evaluate the diversity or relevance of your examples for a given task, or generate new examples given a set of existing examples to reference.
寻求 Claude 的帮助：编写好的示例很难！您可以请 Claude 评估示例在给定任务中的多样性或相关性，或者根据一组现有示例生成新的示例以供参考。

Remember, prompt engineering is an iterative process. Don't be discouraged if your initial examples don't yield perfect results – with a bit of tweaking and experimentation, you'll be able to unlock Claude's full potential and achieve exceptional results for your applications.

记住，快速工程是一个迭代过程。如果你的初始示例没有产生完美的结果，不要灰心 - 通过一点调整和实验，你将能够释放 Claude 的全部潜力，并为你的应用程序取得出色的结果。