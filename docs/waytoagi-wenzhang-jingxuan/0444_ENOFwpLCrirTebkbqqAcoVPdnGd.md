# 9.Prefill Claude's response 预先填写 Claude 的回答

> 来源：[https://waytoagi.feishu.cn/docx/BKNfdsgxRoUHC2xD1PDcNETenhg](https://waytoagi.feishu.cn/docx/BKNfdsgxRoUHC2xD1PDcNETenhg)

When using Claude, you have the unique ability to guide its responses by prefilling the Assistant message. This powerful technique allows you to direct Claude's actions, control the output format, and even help Claude stay in character during role-play scenarios. In some cases where Claude is not performing as expected, a few prefilled sentences can vastly improve Claude's performance.

Claude具有通过填充消息来引导其响应的独特能力。这种强大的技术允许您引导 Claude 的行为，控制输出格式，甚至在角色扮演场景中帮助 Claude 保持性格。在一些情况下，如果 Claude 的表现不如预期，填充几个句子可以大大改善 Claude 的表现。

> [Check out our blo](https://www.anthropic.com/news/claude-2-1-prompting)g post Long context prompting for Claude 2.1 to see an example of highly effective prefilling.

> 查看我们的博客文章《Claude 2.1 的长上下文提示示例》，了解高效填充的示例。

---

# Why prefill Claude's response? 为什么要填充 Claude 的回应？

Prefilling Claude's response offers several key benefits:

给 Claude 预填写响应带来了几个重要好处：

1.  Increased steerability: By providing some initial text for Claude to continue from, you can steer Claude's response in a desired direction. This is particularly useful when you want Claude to focus on a specific topic, generate a particular type of content, or act a certain way.
增强的可控性：通过为 Claude 提供一些初始文本，以便让 Claude 在预期的方向上继续，你可以引导 Claude 的响应，特别适用于当你想让 Claude 专注于特定话题，生成特定类型的内容，或采取特定方式时。

1.  Control output format: Prefilling allows you to specify the exact format you want Claude to use for its output. This is especially handy when working with structured data formats like JSON or XML. For more details on this, see our guide on controlling output format.
控制输出格式：预填写允许你指定你希望 Claude 用于输出的确切格式，特别适用于处理 JSON 或 XML 等结构化数据格式。有关更多细节，请参阅我们的控制输出格式指南。

1.  Maintain character consistency: In role-play scenarios, prefilling Claude's response can help Claude stay in character throughout a long conversation. By consistently reminding Claude of its role in the Assistant message, you can better ensure that Claude maintains the desired persona. Check out keep Claude stay in character for more details.
保持角色一致性：在角色扮演场景中，预先填充 Claude 的回应可以帮助 Claude 在漫长的对话中保持角色一致。通过不断提醒 Claude 在 Assistant 消息中的角色，您可以更好地确保 Claude 保持所需的人设。查看保持 Claude 角色一致性以获取更多详细信息。

---

# **How to prefi**ll Claude's response 如何预先填充克劳德的回应

To prefill Claude's response, simply include the desired initial text in the Assistant message when making an API request. Here's an example prompt:

要预先填充 Claude 的回应，只需在进行 API 请求时在 Assistant 消息中包含所需的初始文本。以下是一个示例提示：

In this example, by starting the Assistant message with {, we constrain Claude's output to be the rest of the requested JSON schema.

在这个例子中，通过以 Assistant 消息开头，我们限制了 Claude 的输出为所请求的 JSON 模式的其余部分。

Here is how the above prompt would be written in code in the Messages API format:

这是如何在消息 API 格式中编写上述提示的代码

```
import anthropic

client = anthropic.Anthropic(
    # defaults to os.environ.get(&quot;ANTHROPIC_API_KEY&quot;)
    api_key=&quot;my_api_key&quot;,
)
message = client.messages.create(
    model=&quot;claude-2.1&quot;,
    max_tokens=1000,
    temperature=0,
    messages=[
        {
            &quot;role&quot;: &quot;user&quot;,
            &quot;content&quot;: &quot;Please extract the name, size, price, and color from this product description and output it within a JSON object.\n\n&lt;description&gt;The SmartHome Mini is a compact smart home assistant available in black or white for only $49.99. At just 5 inches wide, it lets you control lights, thermostats, and other connected devices via voice or app—no matter where you place it in your home. This affordable little hub brings convenient hands-free control to your smart devices.\n&lt;/description&gt;&quot;
        }
        {
            &quot;role&quot;: &quot;assistant&quot;,
            &quot;content&quot;: &quot;{&quot;
        }
    ]
)
print(message.content)
```

---