
# LangChain 核心工具接口大升级！

> 来源：[https://agijuejin.feishu.cn/docx/YOshdRVG2oEI6KxP6qXcHb3kn9b](https://agijuejin.feishu.cn/docx/YOshdRVG2oEI6KxP6qXcHb3kn9b)

## 👀快看👀

> **作者：**[**沧海九粟**](https://space.bilibili.com/28357052)

> **原文链接：**🦜🔗 LangChain 核心工具接口大升级！

> 示例代码：https://github.com/webup/notebooks/blob/main/langchain-tool-use-upgrade.ipynb

## ⚡️摘要⚡️

> 分享的主要内容是 LangChain 平台的核心工具接口的升级。

## 🤖分集🤖

### LangChain 核心工具接口大升级！

视频首先介绍了Lanchain和LangGraph之间的关系,以及LangChain中工具调用的重要性。然后详细介绍了如何在LangChain中使用工具,包括如何对工具的输入输出进行管理以及如何处理异常。最后,通过`BuyTools`方法可以直接导入普通的函数,并展示了如何使用LangChain最新的工具函数来构建多种模型。

![](img/58d8253f9c11071535a40a12b362dd39.png)

[🦜🔗 LangChain 核心工具接口大升级！.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/AfGBbNV4CoBeB4xwP42cHfb4nTf)

## ✏️课代表✏️

+ 工具的重要性：在大型语言模型中，工具的使用能够提高信息的提取和生成的结构化能力。

+ 工具接口升级：LangChain 平台对核心工具接口进行了升级，支持更多的模型和工具的使用，提供了更加灵活和强大的工具调用方式。

+ 简化工具集成：通过`buy_tools`和`as_tool`方法，可以将普通函数和 `RENDERABLE` 对象轻松转换为工具，简化了工具集成的过程。

+ 输入输出控制：通过注解和流，可以更精细化地控制工具的输入输出，例如隐藏不必要的参数，增加中间结果的携带。

+ 异常处理：提供了三种异常处理方法，包括捕获异常、`fallback` 机制和重试机制，以提高系统的鲁棒性和错误处理能力。

+ 最佳实践：推荐使用更优的模型作为 `fallback` 选项，以及利用错误信息进行重试，以提高工具调用的准确性和效率。

+ 社区交流：鼓励社区成员在评论区进行交流互动，共同探讨如何更好地使用和优化工具的应用。
