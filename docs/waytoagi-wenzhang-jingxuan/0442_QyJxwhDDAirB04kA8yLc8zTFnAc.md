# 7.Chain prompts 链式提示

> 来源：[https://waytoagi.feishu.cn/docx/RL6NdETGroCgBtxpdSacVxHcnxb](https://waytoagi.feishu.cn/docx/RL6NdETGroCgBtxpdSacVxHcnxb)

You can think of working with large language models like juggling. The more tasks you have Claude handle in a single prompt, the more liable it is to drop something or perform any single task less well. Thus, for complex tasks that require multiple steps or subtasks, we recommend breaking those tasks down into subtasks and chaining prompts to ensure highest quality performance at every step.

你可以把与大型语言模型合作想象成是杂耍。在单个提示中处理的任务越多，Claude 落下或者执行任何单个任务的风险就越大。因此，对于需要多个步骤或子任务的复杂任务，我们建议将这些任务分解为子任务，并链接提示，以确保在每个步骤上获得最高质量的性能。

---

# What is prompt chaining? 什么是提示链接？

Prompt chaining involves using the output from one prompt as the input for another prompt. By chaining prompts together, you can guide Claude through a series of smaller, more manageable tasks to ultimately achieve a complex goal.

提示链接涉及使用一个提示的输出作为另一个提示的输入。通过链接提示，您可以引导 Claude 完成一系列较小、更易管理的任务，最终实现复杂的目标。

Prompt chaining offers several advantages:

串联提示提供了几个优势：

+   Improved accuracy and consistency in the generated output at each distinct step
在每个独立步骤中生成的输出的准确性和一致性得到改善

+   Easier troubleshooting by isolating specific subtasks that may be particularly error-prone or challenging to handle
通过隔离可能特别容易出错或难以处理的特定子任务，更容易进行故障排除

---

# **When to **use prompt chaining 何时使用提示链接

Consider using prompt chaining in the following scenarios:

在以下情况下考虑使用提示链接：

1.  Multi-step tasks: If your task requires multiple distinct steps, such as researching a topic, outlining an essay, writing the essay, then formatting the essay, chaining prompts can help ensure each step of the task has Claude's full focus and is executed at a high level of performance.
多步骤任务：如果您的任务需要多个明确的步骤，比如研究一个主题，列出一篇文章的提纲，写文章，然后格式化文章，链接提示可以帮助确保任务的每个步骤都得到 Claude 的充分关注，并以高水平执行。

1.  Complex instructions: When a single prompt contains too many instructions or details, Claude may struggle to follow them consistently. Breaking the task into a series of chained subtasks can improve performance for each subtask.
复杂指令：当一个单一提示包含太多指令或细节时， Claude 可能会难以始终遵循它们。将任务分解为一系列链接的子任务可以提高每个子任务的性能。

1.  Verifying outputs: You can use chaining to ask Claude to double-check its own outputs with a given rubric and improve its response if needed, ensuring higher quality results. For example, after generating a list of items, you can feed that list back to Claude and ask it to verify the list's accuracy or completeness.
验证输出：您可以使用链接功能要求 Claude 使用给定的评分标准来反复检查自己的输出，并在需要时改进其响应，确保获得更高质量的结果。例如，在生成项目列表后，您可以将该列表反馈给克劳德，并要求其验证列表的准确性或完整性。

1.  Parallel processing: If your task has multiple independent subtasks, you can create separate prompts for each subtask and run them in parallel to save time.
并行处理：如果您的任务有多个独立的子任务，您可以为每个子任务创建单独的提示，并并行运行它们以节省时间。

---

# **Tips for **effective prompt chaining 有效提示链接的技巧

1.  Keep subtasks simple and clear: Each subtask should have a well-defined objective and simple instructions. This makes it easier for Claude to understand and follow.
保持子任务简单明了：每个子任务都应该有明确定义的目标和简单的说明。这样可以让 Claude 更容易理解和遵循。

1.  Use XML tags: Enclosing inputs and outputs in XML tags can help structure the data and make it easier to extract and pass on to the next step when chaining prompts.
使用 XML 标签：将输入和输出封装在 XML 标签中可以帮助结构化数据，并在链接提示时更容易提取和传递到下一步。

---

# **Examples 示例**

Here are a few examples showcasing how to use chaining prompts and breaking tasks into subtasks:

以下是一些示例，展示如何使用链接提示和将任务分解为子任务：

## Answering questions using a document and quotes 使用文档和引用回答问题

##

Here we want Claude to, given a document and a question, generate an answer using relevant quotes from the document.

在这里，我们希望 Claude 能够根据文档和问题生成答案，使用文档中相关的引用。

### Prompt 1: Extracting the quotes 提示1：提取引用

### Prompt 2 (using {{QUOTES}} output from Prompt 1): Answering the question 提示 2（使用来自提示 1 的 {{QUOTES}} 输出）：回答这个问题

## Validating outputs 验证输出

In this example, the goal is to have Claude identify grammatical errors in an article, then double-check that the list of errors is complete.

在这个例子中，目标是让 Claude 识别文章中的语法错误，然后再次检查错误列表是否完整。

### Prompt 1: Generating a list of errors 提示 1：生成错误列表

### Prompt 2 (using {{ERRORS}} output from Prompt 1): Double checking that the list is comprehensive 请使用 Prompt 1 中的 {{ERRORS}} 输出来确认列表是否全面。

## Parallel processing 并行处理

In this example, the goal is to have Claude explain a concept to readers at three different levels (1st grade, 8th grade, college freshman) by first creating an outline, then expanding it into a full explanation.

在这个例子中，目标是让 Claude 向读者以三个不同的层次（一年级、八年级、大一新生）解释一个概念，首先创建一个大纲，然后将其扩展为全面的解释。

### Prompt 1 (create three different versions, one for each reading level): Create an outline 提示 1（为每个阅读水平创建三个不同版本）：创建一个大纲

### Prompt 2 (using {{OUTLINE}} output from Prompt 1, one per reading level): Create full explanations using the outline 提示 2（使用从提示 1 中得到的 {{OUTLINE}} 输出，每个阅读水平一个）: 使用提纲创建完整的解释