
# 在开发者控制台中生成更好的提示<译文>

> 来源：[https://my.feishu.cn/docx/UvyQdnUDkobWQNx84fgc61Q0nxo](https://my.feishu.cn/docx/UvyQdnUDkobWQNx84fgc61Q0nxo)

原文来源：[C](https://www.anthropic.com/news/prompt-generator)laude 官网-Generate better prompts in the developer console

您现在可以在人类学控制台中生成生产就绪的提示模板。描述您想要实现的目标，`Claude`将使用思维链推理等提示工程技术来创建有效、精确和可靠的提示。

此功能旨在帮助不熟悉提示工程的用户，以及为有经验的提示工程师节省时间。通过向提示生成器提供有关您的任务和所需输出格式的详细信息，您将获得最佳结果。

尽管生成的提示并不总是产生完美的结果，但它们通常优于那些刚接触提示工程的人创建的手写提示。生成的提示模板也是可编辑的，允许您快速调整它们以获得最佳性能。

https://youtu.be/Ig3HlWNHcpY

## 推广最佳实践

这个新功能生成的提示模板利用了我们的许多提示工程最佳实践。其中之一是角色设置，鼓励`Claude`在所选任务中采用专家的特征。在我们的内容审核示例中，角色设置如下所示：

```
You will be acting as a content moderator to classify chat transcripts as either approved or rejected based on a provided content moderation policy.
```

另一种实践是思维推理链，在这个过程中，`Claude`在回答之前有时间和空间来收集自己的想法。这允许对复杂的查询做出更彻底和合理的回应。当被要求根据客户之前的交易生成产品推荐提示时，这是如下实现的：

```
In a &lt;scratchpad&gt;, brainstorm 3 different product recommendations you could make to this customer based on their transaction history. For each potential recommendation, provide a brief rationale explaining why you think it would be a good fit for this customer.
```

此外，模板通常将“变量”（可以插入自定义数据的输入字段）放置在XML标记之间。这遵循了另一个关键的最佳实践，通过提供清晰的结构清楚地描绘了提示的不同部分。当被要求将代码转换为Python的提示时，我们看到更长、更模糊的`{{CODE}}`变量由XML标记标记，而简单的`{{LANGUAGE}}`变量位于内联。

```
Your task is to translate a piece of code from another programming language into Python.

Here is the code to translate:

&lt;code&gt;
{{CODE}}
&lt;/code&gt;

The code is written in {{LANGUAGE}}.
```

在某些情况下，您会看到`Claude`编写示例输入和输出，以便围绕它认为您想要的答案类型为自己提供明确的方向。您可以编辑这些示例以匹配所需的输出格式。

## 幕后黑手

提示生成器基于一个长提示，它本身使用了许多已经提到的技术。

+   它包含许多任务描述和提示模板的示例，以向`Claude`展示如何从任务描述到提示模板。

+   它鼓励`Claude`在编写模板之前计划好它将产生的模板的结构，让`Claude`有时间收集它的想法。

+   它有一个强大的“脊柱”，由标记每个部分开头和结尾的XML标记组成，以增强易读性。

您可以在此Colab笔记本中看到完整的提示。

## 提示模板作为评估工具

您将从提示生成器中获得的模板中的变量将采用车把表示法，如前面的内容审核示例所示：

```
Here is the policy you should enforce:
&lt;content_policy&gt;
{{CONTENT_POLICY}}
&lt;/content_policy&gt;

Here is the chat transcript to review and classify:
&lt;transcript&gt;
{{TRANSCRIPT}}
&lt;/transcript&gt;
```

在此示例中，您可以上传您的内容标准和一系列不同的聊天记录，以查看`Claude`的行为。此过程允许您确保您的应用程序将在各种情况下做出适当的响应。

https://youtu.be/aQpbp-Ra2Rk

## 客户聚光灯：ZoomInfo

上市平台ZoomInfo使用`Claude`提出可操作的建议并为客户创造价值。他们使用即时生成有助于显著减少构建RAG应用程序的MVP所需的时间，同时提高输出质量。

ZoomInfo首席数据科学家斯宾塞·福克斯表示：“Anthropic的新提示生成器功能使我们能够更快地达到生产就绪的输出。它突出了我没有用来提升性能的技术，并大大减少了调整我们应用程序所花费的时间。”“我们构建了一个新的RAG应用程序，并在短短几天内达到了MVP，将改进提示所需的时间减少了80%。”

## 开始行动

要开始使用`Claude`构建生产就绪提示，请访问Anturpic API。
