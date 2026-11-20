
您可以通过将 Python 代码括在三个反引号中来编写和执行它。另请注意，您可以访问以下模块来帮助用户向朋友发送消息：

```python
import message
message.write(to="John", message="Hey，想在下班后见面吗？")
```

[在游乐场开放](https://platform.openai.com/playground/p/default-code-api?mode=chat)

**警告：执行模型生成的代码本质上并不安全，任何试图执行此操作的应用程序都应采取预防措施。特别是，需要沙盒代码执行环境来限制不受信任的代码可能造成的危害。**

## [策略：让模型访问特定功能](https://platform.openai.com/docs/guides/prompt-engineering/tactic-give-the-model-access-to-specific-functions)

聊天完成[ API 允](https://platform.openai.com/docs/guides/function-calling)[许在请求中传递](https://cookbook.openai.com/examples/how_to_call_functions_with_chat_models)功能描述列表。这使得模型能够根据提供的模式生成函数参数。生成的函数参数由 API 以 JSON 格式返回，可用于执行函数调用。然后，可以将函数调用提供的输出反馈到以下请求中的模型中以关闭循环。这是使用OpenAI模型调用外部函数的推荐方式。要了解更多信息，请参阅我们的介绍性文本生成指南中的函数调用部分以及OpenAI Cookbook 中的更多函数调用示例。

## [策略：系统地测试变更](https://platform.openai.com/docs/guides/prompt-engineering/strategy-test-changes-systematically)

有时很难判断更改（例如新指令或新设计）是否使您的系统变得更好或更差。看几个例子可能会暗示哪个更好，但由于样本量较小，很难区分真正的改进或随机运气。也许这种变化有助于某些输入的性能，但会损害其他输入的性能。

评估程序（或“evals”）对于优化系统设计非常有用。好的评估是：

+   代表现实世界的使用情况（或至少是多样化的）
+   包含许多测试用例以获得更大的统计能力（有关指南，请参阅下表）
+   易于自动化或重复

输出的评估可以由计算机、人类或两者混合来完成。计算机可以使用客观标准（例如，具有单个正确答案的问题）以及一些主观或模糊标准自动进行评估，其中模型输出由其他模型查询进行评估。OpenAI Evals是一个开源软件框架，提供用于创建自动化评估的工具。

当存在一系列可能的输出被认为质量同样高时（例如，对于答案很长的问题），基于模型的评估会很有用。通过基于模型的评估可以实际评估的内容与需要人类评估的内容之间的界限是模糊的，并且随着模型变得更加强大而不断变化。我们鼓励进行实验，以确定基于模型的评估对您的用例的效果如何。

### 策略：参考黄金标准答案评估模型输出

假设已知问题的正确答案应参考一组特定的已知事实。然后我们可以使用模型查询来计算答案中包含多少必需的事实。

例如，使用以下系统消息：

**系统**

您将获得由三引号分隔的文本，该文本应该是问题的答案。检查答案中是否直接包含以下信息： - 尼尔·阿姆斯特朗是第一个登上月球的人。- 尼尔·阿姆斯特朗 (Neil Armstrong) 首次在月球上行走的日期是 1969 年 7 月 21 日。对于每个点，请执行以下步骤： 1 - 重述该点。2 - 提供最接近这一点的答案的引文。3 - 考虑不知道主题的阅读引文的人是否可以直接推断出该点。在做出决定之前解释一下原因或原因。4 - 如果 3 的答案是“是”，则写“是”，否则写“否”。最后，计算有多少个“是”答案。将此计数提供为 `{"count": <在此处插入计数>}`。

[在游乐场开放](https://platform.openai.com/docs/guides/prompt-engineering/strategy-test-changes-systematically?mode=chat)

这是一个满足这两点的示例输入：

**系统**

<在上面插入系统消息>

**用户**

"""尼尔·阿姆斯特朗因成为第一个踏上月球的人类而闻名。这一历史性事件发生在 1969 年 7 月 21 日，阿波罗 11 号任务期间。"""

[在游乐场开放](https://platform.openai.com/playground/p/default-model-based-eval-1?mode=chat)

以下是仅满足一个点的示例输入：

**系统**

<在上面插入系统消息>

**用户**

“”“尼尔·阿姆斯特朗走下登月舱时创造了历史，成为第一个在月球上行走的人。“”

[在游乐场开放](https://platform.openai.com/playground/p/default-model-based-eval-2?mode=chat)

这是一个不满足任何条件的示例输入：

**系统**

<在上面插入系统消息>

**用户**

“69 年夏天，一次伟大的航行，阿波罗 11 号，像传奇之手一样大胆。阿姆斯特朗迈出一步，历史展开，“一小步，”他说，为了一个新世界。“”

[在游乐场开放](https://platform.openai.com/playground/p/default-model-based-eval-3?mode=chat)

这种基于模型的评估有许多可能的变体。考虑以下变体，它跟踪候选答案和黄金标准答案之间的重叠类型，并且还跟踪候选答案是否与黄金标准答案的任何部分相矛盾。

**系统**

使用以下步骤响应用户输入。在继续之前充分重申每个步骤。即“第一步：原因...”。步骤 1：逐步推理所提交答案中的信息与专家答案相比是否是：不相交、相等、子集、超集或重叠（即存在交集，但不是子集/超集）。第2步：逐步推理提交的答案是否与专家答案的任何方面相矛盾。步骤 3：输出一个 JSON 对象，结构如下：`{"type_of_overlap": "disjoint" or "equal" or "subset" or "superset" or "overlapping", "contradiction": true or false}`

[在游乐场开放](https://platform.openai.com/docs/guides/prompt-engineering/strategy-test-changes-systematically?mode=chat)

这是一个示例输入，其答案不合格，但与专家答案并不矛盾：

**系统**

<在上面插入系统消息>

**用户**

问题：“”“尼尔·阿姆斯特朗最著名的事件是什么？它发生在哪一天？假设 UTC 时间。“”提交的答案：“”“他没有在月球上行走过吗？“”专家解答: """尼尔·阿姆斯特朗最著名的是第一个登上月球的人。这一历史性事件发生在 1969 年 7 月 21 日。"""

[在游乐场开放](https://platform.openai.com/playground/p/default-model-based-eval-4?mode=chat)

这是一个示例输入，其答案与专家答案直接矛盾：

**系统**

<在上面插入系统消息>

**用户**

问题：“”“尼尔·阿姆斯特朗最著名的事件是什么？它发生在哪一天？假设 UTC 时间。“”提交的答案：“”“1969 年 7 月 21 日，尼尔·阿姆斯特朗成为第二个在上面行走的人继巴兹·奥尔德林之后登上月球。"""专家解答："""尼尔·阿姆斯特朗最著名的是第一个登上月球的人。这一历史性事件发生在 1969 年 7 月 21 日。"""

[在游乐场开放](https://platform.openai.com/playground/p/default-model-based-eval-5?mode=chat)

下面是一个带有正确答案的示例输入，它还提供了比必要的更多的细节：

**系统**

<在上面插入系统消息>

**用户**

问题：“”“尼尔·阿姆斯特朗最著名的事件是什么？它发生在哪一天？假设 UTC 时间。“”提交的答案：“”“1969 年 7 月 21 日大约 02:56 UTC，尼尔·阿姆斯特朗成为第一个人类踏上月球表面，标志着人类历史上的一项里程碑式的成就。“”专家解答：“”“尼尔·阿姆斯特朗最著名的是第一个登上月球的人。这一历史性事件发生在7月21日， 1969年。

[在游乐场开放](https://platform.openai.com/playground/p/default-model-based-eval-6?mode=chat)

## [其他资源](https://platform.openai.com/docs/guides/prompt-engineering/other-resources)

如需更多灵感，请访问[OpenAI Cookbook](https://cookbook.openai.com/)，其中包含示例代码以及第三方资源的链接，例如：

+   [提示库和工具](https://cookbook.openai.com/related_resources#prompting-libraries--tools)
+   [提示指南](https://cookbook.openai.com/related_resources#prompting-guides)
+   [视频课程](https://cookbook.openai.com/related_resources#video-courses)
+   关于高级提示以提高推理能力的论文
