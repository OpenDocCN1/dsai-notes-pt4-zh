# 3.Be clear & direct 言之有物，直截了当

> 来源：[https://waytoagi.feishu.cn/docx/E5IIdT1WNofB52xSmBUcgXtZnTf](https://waytoagi.feishu.cn/docx/E5IIdT1WNofB52xSmBUcgXtZnTf)

When interacting with Claude, providing clear and direct instructions is essential for achieving the best results. Think of Claude like a smart but new employee who has no context on what to do aside from what you explicitly tell them. Just as when you instruct a human for the first time on a task, the more you explain exactly what you want in a straightforward manner, the better and more accurate Claude's response will be.

与克劳德互动时，提供清晰直接的指示对于实现最佳结果至关重要。把克劳德想象成一个聪明但是没有上下文的新员工，除了你明确告诉他们的内容外，他们不知道该做什么。就像第一次指导人类执行任务时一样，你解释得越详细，以直接的方式表达你想要什么，克劳德的回应就会越好、更准确。

---

# **Provide detailed cont****ext and in**structions 提供详细的背景和指示

To ensure Claude understands your task, provide as much context and detail as possible. Include any specific rules or requirements for completing the task correctly. Consider the following example where we ask Claude to remove personally identifiable information (PII) from a given text:

为了确保克劳德理解你的任务，尽可能提供更多的背景和细节。包括完成任务所需的任何具体规则或要求。考虑以下示例，我们要求克劳德从给定的文本中删除个人身份信息（PII）。

While this prompt may work for simple cases, it lacks the necessary details for Claude to consistently deliver the desired output. Here is a more detailed and clearly written version.

尽管这个提示可能适用于简单情况，但缺乏克劳德始终提供所需输出的必要细节。这是一个更详细和清晰写作的版本。

In this improved prompt, we:

在这个改进的提示中，我们：

1.  Provide context upfront about the nature of the task
事先提供有关任务性质的上下文

1.  Define what constitutes PII (names, phone numbers, addresses)
定义个人身份信息的内容（姓名、电话号码、地址）

1.  Give specific, enumerated step-by-step instructions on how Claude should carry out its task
给出具体的、逐步列举的指导，告诉克劳德应该如何执行任务

1.  Give guidelines to Claude for how it should format its output
为克劳德提供指导，告诉它应该如何格式化输出

You can also use this technique to control other aspects of Claude's output. For example, by default, Claude often includes a friendly preamble before providing the requested output. If you prefer to receive only the essential information, you can instruct Claude to skip the preamble and get straight to the point.

您还可以使用这种技术来控制克劳德输出的其他方面。例如，默认情况下，克劳德在提供请求的输出之前经常包含友好的序言。如果您希望只收到基本信息，您可以指示克劳德跳过序言，直接进入主题。

Here is an example where Claude outputs a preamble:

这是一个克劳德输出导论的示例

Here is an example where the prompt asks for Claude to skip the preamble:

这里是一个示例，提示要求克劳德跳过序言部分：

By explicitly asking Claude to skip the preamble, you can receive a more concise response that focuses solely on the requested output.

通过明确要求克劳德跳过序言，您可以获得更简洁的回应，专注于所请求的输出。

---

# **Use numbere**d lists or bullet points 使用编号列表或项目符号

When providing instructions for complex tasks, consider breaking them down into numbered steps or bullet points. This format makes it easier for Claude to follow the instructions and ensures that all requirements are met.

在提供复杂任务的说明时，考虑将其分解为编号步骤或项目符号。这种格式使得克劳德更容易遵循说明，并确保满足所有要求。

Here is an example prompt without this technique:

这是一个没有使用这种技巧的示例提示：

Here is the same prompt with numbered steps:

这里是带有编号步骤的相同提示：

By presenting the instructions as numbered steps, you will both naturally write with more detail and better ensure that Claude executes its task to your standards.

通过将指令呈现为编号步骤，你将更自然地写出更详细的内容，并更好地确保克劳德按照你的标准执行任务。

---

# **Be specif**ic about what you want 具体说明你想要什么

If you want Claude to provide a definitive answer or take a specific stance on a topic, make that clear in your prompt. When instructions are vague or open-ended, Claude may provide a more general response.

如果你希望克劳德提供一个明确的答案或在某个话题上采取明确的立场，请在你的提示中明确说明。当指示模糊或开放时，克劳德可能会提供更一般性的回答。

Here is an example of an open-ended question that causes Claude to equivocate:

这里有一个导致克劳德含糊其辞的开放性问题的例子：

Here is the same request, but with a more detailed and nuanced ask that successfully pushes Claude to have an opinion:

这是同样的请求，但更详细和微妙，成功地促使克劳德表达观点：

---

# **The golde**n rule of clear prompting 明确提示的黄金法则

When crafting your prompts, follow the golden rule of clear prompting: show your prompt to a friend or colleague and ask them to follow the instructions themselves to see if they can produce the exact result you want. If your friend is confused, Claude will likely be confused as well.

在制作提示时，请遵循清晰提示的黄金法则：向朋友或同事展示您的提示，并要求他们按照说明自己操作，看看他们是否能够产生您想要的确切结果。如果您的朋友感到困惑，克劳德很可能也会感到困惑。

Remember, Claude is a powerful tool, but it relies on your guidance to deliver the best results. By providing clear, direct, and well-structured prompts, you can unlock Claude's full potential and achieve your desired outcomes more consistently.

请记住，克劳德是一个强大的工具，但它需要您的指导才能发挥最佳效果。通过提供清晰、直接和结构良好的提示，您可以释放克劳德的全部潜力，更加稳定地实现您的期望结果。