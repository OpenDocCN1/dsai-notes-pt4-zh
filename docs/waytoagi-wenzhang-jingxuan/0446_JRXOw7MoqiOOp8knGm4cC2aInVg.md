# 11.Ask Claude for rewrites 要求 Claude 进行重写

> 来源：[https://waytoagi.feishu.cn/docx/EMiXddze3oDah5xrhnXcuEFInXg](https://waytoagi.feishu.cn/docx/EMiXddze3oDah5xrhnXcuEFInXg)

Sometimes, even with a well-crafted prompt, Claude's response might not quite hit the mark or be completely accurate. In these cases, you can leverage Claude's ability to revise its own work by asking for a rewrite. By providing clear instructions and a rubric for the desired output, you can guide Claude to generate content that better aligns with your goals.

有时候，即使有一个精心设计的提示， Claude 的回答可能并不完全准确或符合要求。在这种情况下，您可以利用 Claude 修订自己作品的能力，要求重新写作。通过提供清晰的指示和期望输出的评分标准，您可以引导 Claude 生成更符合您目标的内容。

---

# **When to **ask for a rewrite 何时要求重新写作

Consider requesting a rewrite when:

考虑在以下情况下要求重新写作：

+   The response is close to what you need but lacks specific details or formatting
响应接近您所需的内容，但缺乏具体细节或格式

+   The output doesn't fully adhere to the constraints outlined in your initial prompt
输出未完全遵循初始提示中概述的约束条件

+   You are unsure if the result is accurate and want Claude to double check to make sure (consider it a mini one-off LLM-graded eval)
您不确定结果是否准确，希望 Claude 进行双重检查以确保（将其视为迷你一次性评估）

Rewrites are particularly useful when your prompt contains multiple constraints, such as a specific format (e.g., a poem or list) and specific content topics. Sometimes, Claude may prioritize one constraint over another, especially if there are many complex steps to follow. Asking for a rewrite allows you to emphasize the importance of whatever constraints Claude initially missed.

重写特别有用的情况是在您的提示包含多个约束时，例如特定格式（例如诗歌或列表）和特定内容主题。有时， Claude 可能会优先考虑其中一个约束，特别是如果存在许多复杂的步骤需要遵循。请求重写可以让您强调 Claude 最初忽略的任何约束的重要性。

---

# **How to** request a rewrite 如何请求重写

When sending requests through the API (including the functions in Claude for Sheets), you must always include the previously generated text in your prompt. Here's an example prompt template:

通过 API 发送请求（包括 Claude for Sheets 中的功能），您必须始终在您的提示中包括先前生成的文本。以下是一个示例提示模板：

For lower volume use cases, you can paste the paragraph to be rewritten in place of {{P**ARAGRAPH}}. **To automate this process, consider setting up a prompt chain so that Claude's response to a previous prompt gets substituted for the {{PARAGRAPH}} placeholder.

对于较低的使用情况，您可以将要重写的段落粘贴到 {{PARAGRAPH}} 的位置。要自动化此过程，请考虑设置提示链，以便 Claude 对先前提示的响应被替换为 {{PARAGRAPH}} 占位符。

When using Claude for Sheets, you can structure your Sheet so that each row contains a different set of instructions to be substituted for the {{INSTRUCTIONS}} placeholder. This allows you to generate multiple types of rewrites simultaneously.

在使用 Claude 表格时，您可以构建您的表格，使每一行包含一组不同的指令，以替换 {{INSTRUCTIONS}} 占位符。这样可以同时生成多种类型的重写。

---

# **Crafting effective ****rewrite i**nstructions 制定有效的重写指导

To get the most out of Claude's rewrite capabilities, provide clear and specific instructions that outline your expectations for the revised content. Consider including:

为了充分利用 Claude 的重写能力，请提供清晰明确的指示，概述您对修改后内容的期望。考虑包括：

+   The desired format (e.g., paragraph, list, dialogue)
所需格式（例如，段落，列表，对话）

+   The target tone or style (e.g., formal, conversational, humorous)
目标语气或风格（例如，正式的，口语的，幽默的）

+   Any additional information or context to include
包括的任何额外信息或上下文

+   Specific areas of the original response to focus on or modify
要专注或修改的原始响应的特定领域

+   A rubric for Claude to asses the quality and accuracy of its previous response, and to understand what a high quality result entails
为 Claude 评估其先前响应的质量和准确性以及了解高质量结果所需的评分标准

Here's an example of how you might request a rewrite to emphasize certain constraints:

这是一个示例，说明您可能如何请求重写以强调某些约束条件：

By breaking down the prompt into sequential steps and providing targeted rewrite instructions, you can guide Claude to generate output that better meets your specific needs.

通过将提示分解为顺序步骤，并提供有针对性的重写指导，您可以引导 Claude 生成更符合您特定需求的输出。

> 📘A note on fact-checking 关于事实核查的说明

> If** you're a**sking Claude to rewrite a response to ensure factual accuracy, consider utilizing the "I don't know" technique to prevent hallucinations. This approach encourages Claude to acknowledge when it lacks the necessary information to make a correction rather than generating a potentially inaccurate "fix."

> 如果您要求 Claude 重新撰写回复以确保事实准确性，请考虑利用“我不知道”的技巧来防止产生幻觉。这种方法鼓励 Claude 承认当它缺乏必要信息进行更正时，而不是生成潜在不准确的“修复”。

---

# Iterative rewrites 迭代重写

Sometimes, a single rewrite may not be enough to achieve the perfect output. In these cases, consider engaging in an iterative process, requesting multiple revisions based on evolving criteria. With each round of feedback, Claude can learn more about your preferences and refine its responses accordingly.

有时，单次重写可能不足以实现完美的输出。在这些情况下，考虑参与迭代过程，根据不断发展的标准请求多次修订。通过每一轮反馈， Claude 可以更多地了解您的偏好，并相应地完善其回应。

Remember, the key to successful rewrites is providing clear, actionable guidance. The more specific your instructions, the more likely Claude is to generate content that aligns with your vision.

记住，成功重写的关键在于提供清晰、可操作的指导。您的指示越具体， Claude 生成与您愿景一致内容的可能性就越大。

---

# Allow Claude to say "I don't know" 允许 Claude 说“我不知道”

One effective way to reduce hallucinations is to explicitly give Claude permission to say "I don't know," especially when asking fact-based questions (also known as "giving Claude an out"). This allows Claude to acknowledge its limitations and avoid generating incorrect information.

一种有效减少幻觉的方法是明确允许 Claude 说“我不知道”，尤其是在问基于事实的问题时（也称为“给克劳德一个出口”）。这样可以让 Claude 承认自己的局限性，避免产生不正确的信息。

Here's an example prompt that encourages Claude to admit when it doesn't have the answer:

这是一个鼓励 Claude 承认自己不知道答案的示例提示：

By giving Claude an explicit "way out," you can reduce the likelihood of it generating inaccurate information.

通过为 Claude 提供明确的“出路”，您可以减少生成不准确信息的可能性。

> **Reminder 提醒**

> While Claude has read a lot on the internet and knows things about the real world, it does not have internet access. Claude was trained on data that can be over a year out of date. It also does not know anything about current events.

> 尽管 Claude 在互联网上阅读了很多内容，对现实世界的事情也有所了解，但它并没有互联网访问权限。 Claude 接受的训练数据可能已经过时一年以上。它也不知道任何当前事件。