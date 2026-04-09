# 6.Use XML tags 使用 XML 标签

> 来源：[https://waytoagi.feishu.cn/docx/UM63d1sfOotfKFxQpdoceGLNnSg](https://waytoagi.feishu.cn/docx/UM63d1sfOotfKFxQpdoceGLNnSg)

XML tags are a powerful tool for structuring prompts and guiding Claude's responses. Claude is particularly familiar with prompts that have XML tags as Claude was exposed to such prompts during training. By wrapping key parts of your prompt (such as instructions, examples, or input data) in XML tags, you can help Claude better understand the context and generate more accurate outputs. This technique is especially useful when working with complex prompts or variable inputs.

XML 标记是一种强大的工具，用于构建提示和引导克劳德的回应。克劳德对具有 XML 标记的提示特别熟悉，因为在培训期间接触过这样的提示。通过将提示的关键部分（如说明、示例或输入数据）用 XML 标记包裹起来，您可以帮助克劳德更好地理解上下文并生成更准确的输出。在处理复杂提示或可变输入时，这种技术尤其有用。

---

# What are XML tags?

XML tags are angle-bracket tags like <tag></tag>. They come in pairs and consist of an opening tag, such as <tag>, and a closing tag marked by a /, such as </tag>. XML tags are used to wrap around content, like this: <tag>content</tag>.

XML 标签是像 <tag></tag> 这样的尖括号标签。它们成对出现，由一个起始标签组成，例如 <tag> ，以及由 / 标记的闭合标签，例如 </tag> 。XML 标签用于包裹内容，如下所示： <tag>content</tag> 。

Opening and closing XML tags should share exactly the same name. The tag name can be anything you like, as long as it's wrapped in angle brackets, although we recommend naming your tags something contextually relevant to the content it's wrapped around.

开始和结束的 XML 标签必须完全相同。标签名可以是任何你喜欢的，只要它被尖括号包围，尽管我们建议将标签命名为与其周围内容相关的上下文相关的名称。

> XML tags should always be referred to in pairs and never as just as the first half of a set (e.g., Using the document in <doc></doc> tags, answer this question. ).

> XML 标签应始终成对引用，而不应只作为一组的第一部分(例如： Using the document in <doc></doc> tags, answer this question. )。

## 💡XML tag names XML 标签名称

> There is no canonical best set of XML tag names that Claude performs particularly well with. For example, <doc> works just as well as <document>. The only time you need very specific XML tag names is in the case of function calling.

> 没有一个特定的 XML 标签名称集合是 Claude 特别擅长的。例如， <doc> 和 <document> 的效果一样好。唯一需要非常具体的 XML 标签名称的情况是在函数调用的情况下。

---

# Why use XML tags? 为什么使用 XML 标签？

There are several reasons why you might want to incorporate XML tags into your prompts:

有几个原因可能会让您想要将 XML 标记合并到您的提示中：

1.  Improved accuracy: XML tags help Claude distinguish between different parts of your prompt, such as instructions, examples, and input data. This can lead to more precise parsing of your prompt and thus more relevant and accurate responses, particularly in domains like mathematics or code generation.
提高准确性：XML 标记有助于克劳德区分提示中的不同部分，如说明、示例和输入数据。这可以导致更精确地解析您的提示，从而获得更相关和准确的响应，特别是在数学或代码生成等领域。

1.  Clearer structure: Just as headings and sections make documents easier for humans to follow, XML tags help Claude understand the hierarchy and relationships within your prompt.
更清晰的结构：正如标题和章节使文档更易于人类理解一样，XML 标记有助于克劳德理解提示中的层次结构和关系。

1.  Easier post-processing: You can also ask Claude to use XML tags in its responses, making it simpler to extract key information programmatically.
更容易的后处理：您还可以要求克劳德在其响应中使用 XML 标记，使您在程序中提取关键信息更加简单。

---

# **How to use **XML tags 如何使用 XML 标记

You can use XML tags to structure and delineate parts of your prompt from one another, such as separating instructions from content, or examples from instructions.

您可以使用 XML 标记来结构化和划分提示中的各个部分，例如将指示与内容分开，或者将示例与指示分开。

## Handling variable inputs 处理可变输入

When working with prompt templates that include variable inputs, use XML tags to indicate where the variable content should be inserted, such as in the following example:

在处理包含可变输入的提示模板时，请使用 XML 标记来指示变量内容应插入的位置，例如在以下示例中：

As a general rule, you should ways separate your variable inputs from the rest of your prompt using XML tags. This makes it clear to Claude where the examples or data begin and end, leading to more accurate responses.

通常情况下，您应该始终使用 XML 标记将变量输入与提示的其余部分分开。这样可以清楚地告诉 Claude 示例或数据的起始和结束位置，从而获得更准确的响应。

## Requesting structured output 请求结构化输出

You can ask Claude to use XML tags in its responses to make the output easier to parse and process:

您可以要求克劳德在其响应中使用 XML 标记，以使输出更容易解析和处理：

Claude's response: 克劳德的回应:

XML tags make it easier to retrieve targeted details from Claude's response by allowing for programmatic extraction of content between specific tags.

XML 标签使得通过允许在特定标签之间的内容进行程序化提取，更容易从克劳德的响应中检索到目标细节。

> When calling Claude via the API, you can pass closing XML tags (e.g., </json>) to the stop_sequences parameter to have Claude stop generating once it reaches the desired endpoint. This can save both money and time by eliminating any concluding remarks after the core response. The same is true of skipping Claude's friendly preamble by prefilling Claude's response with an opening XML tag.

> 通过 API 调用克劳德时，您可以传递闭合的 XML 标签（例如， </json> ）到 stop_sequences 参数，使得克劳德在达到所需终点后停止生成。这样可以通过消除核心响应后的任何结尾备注，节省金钱和时间。同样，通过在克劳德的响应中预先填充一个开放的 XML 标签，也可以跳过克劳德友好的序言。

---

# XML best practices XML最佳实践

To get the most out of XML tags, keep these tips in mind:

要充分利用 XML 标记，请记住以下提示：

+   Use descriptive tag names that reflect the content they contain (e.g., <instructions>, <example>, <input>).
使用反映其包含内容的描述性标记名称（例如， <instructions> ， <example> ， <input> ）。

+   Be consistent with your tag names throughout your prompts.
在提示中始终保持标记名称的一致性。

+   Always include both the opening (<tag>) and closing (</tag>) tags, including when you reference them, such as Using the document in <doc></doc> tags, answer this question.
始终包括开头（ <tag> ）和结尾（ </tag> ）标签，包括在引用它们时，比如 Using the document in <doc></doc> tags, answer this question.

+   You can and should nest XML tags, although more than five layers of nesting may decrease performance depending on the complexity of the use case.
您可以并且应该嵌套XML标签，尽管超过五层的嵌套可能会根据用例的复杂性降低性能。