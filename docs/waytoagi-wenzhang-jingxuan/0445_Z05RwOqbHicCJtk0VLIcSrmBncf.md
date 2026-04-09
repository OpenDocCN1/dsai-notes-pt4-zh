# 10.Control output format (JSON mode) 控制输出格式（JSON 模式）

> 来源：[https://waytoagi.feishu.cn/docx/DvR1dF5JXoQFQTxs4MQciQrYnon](https://waytoagi.feishu.cn/docx/DvR1dF5JXoQFQTxs4MQciQrYnon)

Claude is highly capable of producing output in a wide variety of formats. By providing clear instructions, examples, and prefilled responses, you can guide Claude to generate responses that adhere to your desired structure and style.

Claude 能够以多种格式生成输出，通过提供清晰的指导、示例和预填的响应，您可以引导 Claude 生成符合您期望的结构和风格的响应。

---

# **Specif**ying the desired format 指定所需格式

One of the simplest ways to control Claude's output is to simply state the format you want. Claude can understand and follow instructions related to formatting, and format outputs such as:

控制 Claude 输出的最简单方法之一是简单地说明您想要的格式。 Claude 可以理解并遵循与格式相关的指示，并格式化输出，例如：

+   JSON

+   XML

+   HTML

+   Markdown

+   CSV

+   Custom formats 自定义格式

For example, if you want Claude to generate a haiku in JSON format, you can use a prompt like this:

例如，如果你想让 Claude 以 JSON 格式生成一首俳句，你可以使用这样的提示：

---

# **Prefilli**ng Claude's response 充填克劳德的回答

A[nother ](https://docs.anthropic.com/claude/docs/prefill-claudes-response)effective way to control Claude's output format is to prefill the Assistant field with the beginning of the desired format. This technique is particularly useful when working with structured formats like JSON or HTML.

另一种有效控制 Claude 输出格式的方法是在所需格式的开头用 Assistant 字段预填充。当处理结构化格式如 JSON 或 HTML 时，这种技术特别有用。

For example, to generate a recipe in JSON format:

例如，要生成 JSON 格式的食谱：

By prefilling the response with the opening JSON curly brace, you give Claude a clear starting point for generating the recipe in the desired format. This also is another technique you can use to have Claude skip its friendly preamble text.

通过在响应中预先填充开头的 JSON 大括号，您为生成所需格式的食谱提供了 Claude 一个清晰的起点。这也是您可以使用的另一种技术，让 Claude 跳过友好的序言文本。

---

# Providing examples 提供例子

#

In addition to explicit instructions, providing examples of the desired output format can help Claude better understand your requirements. When including examples, make it clear that Claude should follow the formatting of the examples provided (otherwise Claude may pick up other details from the provided examples, such as content or writing style).

除了明确的指示之外，提供所需输出格式的示例可以帮助 Claude 更好地理解您的要求。在包括示例时，要清楚表明 Claude 应当遵循所提供示例的格式（否则 Claude 可能会从所提供示例中获取其他细节，比如内容或写作风格）。

Here is an example prompt showcasing this technique:

这里是展示这种技术的示例提示：

---

# **Tips for **better output control 更好的输出控制提示

+   Be as specific as possible in your instructions
在你的指示中尽可能具体

+   Use clear and consistent formatting in your prompts
在您的提示中使用清晰和一致的格式

+   Provide multiple examples when possible to reinforce the desired format
在可能的情况下提供多个示例以加强所需格式

+   Experiment with different combinations of techniques to find what works best for your use case
尝试不同的技术组合，找到最适合您用例的方法