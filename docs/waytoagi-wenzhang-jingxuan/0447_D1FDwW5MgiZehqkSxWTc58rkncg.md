# 12.Long context window tips 长上下文窗口提示

> 来源：[https://waytoagi.feishu.cn/docx/NA8JdZXn1oMefbxjBvEcKrDVnOd](https://waytoagi.feishu.cn/docx/NA8JdZXn1oMefbxjBvEcKrDVnOd)

Claude's extended context window (200K tokens for Claude 3 models) enables it to handle complex tasks that require processing large amounts of information. Claude's extended context window also enables you to simplify workflows that previously required splitting inputs to fit within shorter context windows. By combining inputs into a single prompt, you can streamline your process and take full advantage of Claude's capabilities.

克劳德的扩展上下文窗口（克劳德 3 模型的 200K 标记）使其能够处理需要处理大量信息的复杂任务。克劳德的扩展上下文窗口还使您能够简化以前需要将输入拆分以适应较短上下文窗口的工作流程。通过将输入组合成单个提示，您可以简化流程并充分利用克劳德的功能。

For example, if your previous application required splitting a long document into multiple parts and processing each part separately, you can now provide the entire document to Claude in a single prompt. This not only simplifies your code but also allows Claude to have a more comprehensive understanding of the context, potentially leading to better results.

例如，如果您以前的应用程序需要将长文档拆分为多个部分并分别处理每个部分，现在您可以在单个提示中向克劳德提供整个文档。这不仅简化了您的代码，还使克劳德能够更全面地理解上下文，可能会带来更好的结果。

---

# **Structu**ring long documents 结构化长篇文档

When working with long documents (particularly 30K+ tokens), it's essential to struct[ure your ](https://docs.anthropic.com/claude/docs/use-xml-tags)prompts in a way that clearly separates the input data from the instructions. We recommend using XML tags to encapsulate each document. This structure is how Claude was trained to take long documents, and is thus the structure that Claude is most familiar with:

在处理长篇文档（特别是超过 30,000 个标记的文档）时，将提示结构化以清楚地将输入数据与说明分开至关重要。我们建议使用 XML 标记来封装每个文档。这种结构是 Claude 被训练以处理长篇文档的方式，因此是 Claude 最熟悉的结构：

XML

```
Here are some documents for you to reference for your task:

&lt;documents&lt;document index=&quot;1&quot;&lt;source
(a unique identifying source for this item - could be a URL, file name, hash, etc)
&lt;/source&lt;document_content
(the text content of the document - could be a passage, web page, article, etc)
&lt;/document_content&lt;/document&lt;document index=&quot;2&quot;&lt;source
(a unique identifying source for this item - could be a URL, file name, hash, etc)
&lt;/source&lt;document_content
(the text  content of the document - could be a passage, web page, article, etc)
&lt;/document_content&lt;/document
...
&lt;/documents
[Rest of prompt]
```

This structure makes it clear to Claude which parts of the prompt are input data and which are instructions, improving its ability to process the information accurately. You can also add tags to house other metadata, such as <title> or <author>.

这种结构让 Claude 清楚地知道提示的哪些部分是输入数据，哪些是说明，从而提高了其准确处理信息的能力。您还可以添加标记来存放其他元数据，如 <title> 或 <author> 。

---

# Document-query placement 文档查询位置

Notice in the above example of long document prompt structure that the documents come first and the rest of the prompt comes after. For situations with long documents or a lot of additional background content, Claude generally performs noticeably better if the documents and additive material are placed up top, above the detailed instructions or user query.

请注意在上述长文档提示结构的示例中，文档排在前面，其余提示内容在后面。对于长文档或大量附加背景内容的情况，如果将文档和附加材料放在顶部，放在详细说明或用户查询之上，克劳德通常表现得更好。

This is true of all Claude models, from legacy models to the Claude 3 family.

所有的克劳德模型都是如此，从传统模型到克劳德 3 家族。

---

# Tips for document q&a 文档问答的技巧

When using Claude for document question-answering tasks, keep these tips in mind:

在使用克劳德进行文档问答任务时，请记住以下建议：

+   Place the question at the end of the prompt, after the input data. As mentioned, this has been shown to significantly improve the quality of Claude's responses.
将问题放在提示的末尾，在输入数据之后。如前所述，这已经被证明显著改善了克劳德的回答质量。

+   Ask Claude to find quotes relevant to the question before answering, and to only answer if it finds relevant quotes. This encourages Claude to ground its responses in the provided context and reduces hallucination risk.
要求克劳德在回答之前找到与问题相关的引语，并且只有在找到相关引语时才回答。这鼓励克劳德将其回答扎根于所提供的背景，并且减少了幻觉风险。

+   Instruct Claude to read the document carefully, as it will be asked questions later. This primes Claude to pay close attention to the input data with an eye for the task it will be asked to execute.
指示克劳德仔细阅读文件，因为稍后会有问题。这使克劳德能够密切关注输入数据，以便能够执行后续的任务。

Here's an example prompt that incorporates these tips:

这里有一个示例提示，结合了这些提示:

---

# **Multipl**e choice question generation 多项选择题生成

When using Claude to generate multiple choice questions based on a given text, providing example question-answer pairs from other parts of the same text can significantly improve the quality of the generated questions. It's important to note that generic multiple choice examples based on external knowledge or generated from an unrelated document do not seem to be nearly as effective.

当使用 Claude 根据给定文本生成多项选择题时，提供来自同一文本其他部分的示例问题-答案对可以显著提高生成的问题质量。重要的是要注意，基于外部知识或从不相关文档生成的通用多项选择示例似乎不太有效。

Here's an example prompt for multiple choice question generation:

这里是一个用于生成多项选择题的示例提示：

By providing example questions and answers from the same text, you give Claude a better understanding of the desired output format and the types of questions that can be generated from the given content.

通过提供相同文本中的示例问题和答案，您可以让 Claude 更好地理解所需的输出格式以及可以从给定内容中生成的问题类型。

> F[or more information on t](https://www.anthropic.com/news/prompting-long-context)his specific task, see Anthropic's blog post Prompt engineering for a long context window.

> 有关此特定任务的更多信息，请参阅Anthropic的博文“为长上下文窗口进行提示工程”。