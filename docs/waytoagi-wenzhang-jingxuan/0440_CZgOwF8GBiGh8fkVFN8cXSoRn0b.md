# 5.Give Claude a role 给 Claude 一个角色

> 来源：[https://waytoagi.feishu.cn/docx/IpJadpznsoCYIGxb3qqce4Unnly](https://waytoagi.feishu.cn/docx/IpJadpznsoCYIGxb3qqce4Unnly)

Claude is a highly capable AI assistant, but sometimes it benefits from having additional context to understand the role it should play in a given conversation. By assigning a role to Claude, you can prime it to respond in a specific way, improve its accuracy and performance, and tailor its tone and demeanor to match the desired context. This technique is also known as role prompting.

Claude 是一个非常有能力的 AI 助手，但有时候为了更好地理解其在特定对话中应扮演的角色，它会受益于额外的上下文。通过为 Claude 分配一个角色，您可以让它以特定方式回应，提高其准确性和性能，并调整其语气和态度以匹配所需的上下文。这种技术也被称为角色提示。

---

# How role prompting works 角色提示的工作原理

Role prompting is a powerful technique that can enhance Claude's performance in two main ways:

角色提示是一种强大的技术，可以通过两种主要方式增强 Claude 的性能：

1.  Improved accuracy and performance: Particularly in certain complex situations, such as solving complex math problems or writing code, assigning a relevant role to Claude can significantly improve its performance and understanding of the task at hand.
提高准确性和性能：特别是在某些复杂情况下，比如解决复杂的数学问题或编写代码时，为 Claude 分配一个相关的角色可以显著提高其性能和对手头任务的理解。

1.  Tailored tone and style: By specifying a role, you can influence Claude's tone, demeanor, and communication style to better suit your needs. For example, asking Claude to act as a kindergarten teacher will result in a very different response compared to asking it to be a graduate professor.
量身定制的语调和风格：通过指定一个角色，您可以影响 Claude 的语调、态度和沟通风格，以更好地满足您的需求。例如，要求 Claude 扮演幼儿园老师会得到与要求其扮演研究生教授完全不同的回应。

---

# **When to **use role prompting 何时使用角色提示

While role prompting is not always necessary, it can be incredibly useful in the following scenarios:

虽然角色提示并非总是必要的，但在以下情况下可能非常有用：

+   Highly technical tasks: If you need Claude to perform complex tasks related to logic, mathematics, or coding, assigning an appropriate role can help it excel at the task, even if it might have struggled without the role prompt. Even if Claude isn't struggling, role prompting might still improve performance to new levels.
高度技术性任务：如果您需要 Claude 执行与逻辑、数学或编码相关的复杂任务，分配适当的角色可以帮助它在任务中表现出色，即使没有角色提示，它可能会遇到困难。即使 Claude 没有困难，角色提示可能仍然可以将性能提升到新的水平。

+   Specific communication styles: When you require a particular tone, style, or level of complexity in Claude's responses, role prompting can be an effective way to achieve the desired output.
特定的沟通风格：当您需要 Claude 的回应具有特定的语气、风格或复杂程度时，角色提示可以是实现期望输出的有效方式。

+   Enhancing baseline performance: Unless you are severely limited by token count, there is rarely a reason not to use role prompting if you want to try improving Claude's performance beyond its baseline capabilities.
提升基准性能：除非您受到令牌数量的严格限制，否则如果您想尝试将 Claude 的性能提升到基准能力之外，很少会没有使用角色提示的理由。

---

# Role prompting examples 角色提示示例

Here are a few examples that demonstrate the power of role prompting:

这里有几个示例，展示了角色提示的强大功能：

## Solving a logic puzzle 解决逻辑谜题

Let's consider the following logic puzzle:

让我们考虑以下逻辑谜题：

There are two ducks in front of a duck, two ducks behind a duck and a duck in the middle. How many ducks are there?

Without role prompting, Claude might struggle to provide the correct answer or make errant assumptions that result in only a partial answer:

没有角色提示， Claude 可能会有困难提供正确答案，或者做出错误的假设，导致只能得到部分答案。

However, by assigning the role of a logic bot, Claude's performance improves significantly and it is able to catch the nuance that multiple answers are possible:

然而，通过指定逻辑机器人的角色， Claude 的表现显著提高，能够捕捉到多个答案可能的微妙之处：

## Explaining a concept to different audiences 向不同受众解释一个概念

Role prompting can be used to adjust Claude's communication style based on the intended audience. Consider the following prompts and how Claude's output differs depending on the assigned role:

可以使用角色提示来根据预期受众调整 Claude 的沟通风格。考虑以下提示以及 Claude 的输出如何根据分配的角色而异：

---

# **Tips for **effective role prompting 有效角色提示的技巧

To get the most out of role prompting, keep these tips in mind:

要充分利用角色提示，请记住以下提示：

1.  Be specific: Provide clear and detailed context about the role you want Claude to play. The more information you give, the better Claude can understand and embody the desired role.
具体化：提供关于您希望 Claude 扮演的角色的清晰详细的背景信息。您提供的信息越多， Claude 就能越好地理解和扮演所需的角色。

1.  Experiment and iterate: Try different roles and variations of your prompts to find the best approach for your specific use case. Prompt engineering often involves experimentation and iteration to achieve optimal results.
实验和迭代：尝试不同的角色和提示的变化，找到适合您特定用例的最佳方法。提示工程通常涉及实验和迭代，以实现最佳结果。