# 8.Let Claude think 让 Claude 思考

> 来源：[https://waytoagi.feishu.cn/docx/OgDXdM6FlohvWix7pjpc7ktUnme](https://waytoagi.feishu.cn/docx/OgDXdM6FlohvWix7pjpc7ktUnme)

When faced with a complex question or task, it's often beneficial to let Claude think through the problem step-by-step before providing a final answer. This technique, also known as chain of thought (CoT) prompting, can significantly improve the accuracy and nuance of Claude's responses.

当面对复杂问题或任务时，让克劳德逐步思考问题然后再给出最终答案往往是有益的。这种技术，也被称为思维链（CoT）提示，可以显著提高克劳德回答问题的准确性和细微差别。

---

# Why let Claude think? 为什么要让克劳德思考？

> Imagine being woken up and immediately asked to answer a series of complicated questions. You'd probably perform better if given some time to think through your answers first, right? The same applies to Claude!

> 想象一下被吵醒后立即被要求回答一系列复杂问题。如果给一些时间来思考答案，你可能会表现得更好，对吧？同样适用于克劳德！

> Allowing Claude to reason through a problem before responding can lead to more accurate and intelligent outputs, particularly for multifaceted or challenging queries. By explicitly instructing Claude to think step-by-step, you encourage a more methodical and thorough approach to problem-solving.

> 允许克劳德在回答问题之前进行推理，可以导致更准确和智能的输出，特别是对于多方面或具有挑战性的查询。通过明确指导克劳德逐步思考，您鼓励更有条理和彻底的解决问题方法。

> Furthermore, examining Claude's thought process can be invaluable for troubleshooting and identifying areas where your prompt instructions may be unclear or easily misinterpreted.

> 此外，检查克劳德的思维过程对于故障排除和确定您的提示指令可能不清晰或容易被误解的领域至关重要。

> It's important to note that thinking cannot happen without output! Claude must output its thinking in order to actually "think."

> 需要注意的是，没有输出就不会有思考！克劳德必须输出其思考，才能真正“思考”。

---

# **How to prompt for t****hinking **step-by-step 如何逐步提示思考

The simplest way to encourage thinking step-by-step is to include the phrase "Think step by step" in your prompt. For example:

鼓励逐步思考的最简单方式是在提示中包含短语“逐步思考”。例如：

Claude will then break down its thought process before providing a final answer:

克劳德然后会分解其思维过程，然后给出最终答案：

For more complex queries, you can guide Claude's thinking by specifying the steps it should take. Here's an example:

对于更复杂的查询，您可以通过指定应采取的步骤来引导克劳德的思考。这是一个例子：

By outlining a clear thinking process, you help Claude focus its reasoning on the most relevant information and ensure it thinks through all the necessary factors to perform well at its given task.

通过概述清晰的思维过程，您帮助克劳德将其推理集中在最相关的信息上，并确保它深思熟虑所有必要因素，以在其所面临的任务中表现出色。

---

# **Capturing **Claude's thought process 捕捉克劳德的思维过程

To make it easier to separate Claude's step-by-step reasoning from its final response, consider using XML tags like <thinking> and <answer>. You can instruct Claude to place its thought process inside <thinking> tags and its ultimate answer within <answer> tags.

为了更容易将克劳德的逐步推理与最终响应分开，考虑使用 XML 标签，如 <thinking> 和 <answer> 。 您可以指示克劳德将其思维过程放在 <thinking> 标签内，并将其最终答案放在 <answer> 标签内。

Here's an example prompt with this method:

以下是使用此方法的示例提示:

You can even prefill the <thinking> tag in the Assistant role to guide Claude as to where to begin:

您甚至可以在助手角色中预填 <thinking> 标记，以指导克劳德从何处开始：

Claude will then complete its thought process within the tags and provide its final answer:

克劳德将在标签内完成其思考过程，并给出最终答案：

Using tags makes it simple to extract just the final answer within <answer></answer> tags during post-processing if desired.

使用标签使得在后期处理时，如果需要的话，可以简单地提取 <answer></answer> 标签中的最终答案。

---

# Some considerations 一些考虑事项

While encouraging step-by-step thinking can greatly enhance Claude's responses, keep these points in mind:

鼓励逐步思考可以极大地增强克劳德的回应，但请记住以下几点：

+   Thinking cannot occur unless Claude is allowed to output its thought process. There's no way to have Claude think privately and only return the final answer.
除非允许克劳德输出其思维过程，否则无法进行思考。无法让克劳德私下思考，然后只返回最终答案。

+   Prompting for step-by-step reasoning will increase the length of Claude's outputs, which can impact latency. Consider this tradeoff when deciding whether to use this technique.
提示逐步推理将增加克劳德的输出长度，可能影响延迟。在决定是否使用这种技术时，请考虑这种权衡。