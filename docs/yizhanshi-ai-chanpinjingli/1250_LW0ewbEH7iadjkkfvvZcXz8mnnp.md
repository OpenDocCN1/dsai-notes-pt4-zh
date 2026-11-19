
# MCP 蓝图：MCP 中的测试、安全和沙盒（上篇）

> 来源：[https://v11enp9ok1h.feishu.cn/docx/Q8ind5RQdoTYrFx9PAEcDbKLnyg](https://v11enp9ok1h.feishu.cn/docx/Q8ind5RQdoTYrFx9PAEcDbKLnyg)

## 回顾

在深入研究 MCP 速成课程的第 6 部分之前，让我们简要回顾一下本课程上一部分所涵盖的内容。

![](img/ebdd8db4f22501804c367d7f7d7b78c6.png)

在第五部分中，我们探讨了采样，这是 MCP 最强大的功能之一。采样使服务器能够从客户端的语言模型中请求补全，从而有效地逆转了通常的控制流。

以下是采样工作流程的分解图：

![](img/7b96c961a61a93408ea9b38fb91b5ea2.png)

更具体地说，我们了解了采样如何使 MCP 服务器能够将部分推理委托给客户端，从而实现协作、LLM 辅助执行。

![](img/fe09d4421b230dec7410b284d87670d6.png)

我们在客户端构建了自定义采样处理程序，配置了模型首选项，并探索了 LLM 采样的实际用例，例如文本摘要、自然语言到 SQL 的转换以及问答分析。

![](img/6480455db39a55ea2d3ed46c44d3846e.png)

在编写服务器函数来讨论回退链和错误处理时，我们了解了采样如何处于编排和推理的交叉点，以及为什么它是构建自适应智能系统的关键。

在第 5 部分结束时，我们得到了端到端协作智能的蓝图，其中涉及服务器的工具执行和客户端的 LLM 完成。

如果您还没有学习第五部分，我们强烈建议您先阅读一下。您可以在下面阅读：

## 这部分内容

在本章和下一章中，我们将更进一步，解决构建现实世界系统的最重要但经常被忽视的方面之一：测试、安全和沙盒。

目前为止，我们已经了解了客户端如何触发工具、从 LLM 中采样以及编排多步骤逻辑。但是，当您的服务器必须处理不安全的输入、运行第三方工具或执行用户控制的逻辑时，会发生什么情况？

您如何验证行为、确保安全并隔离风险？

这正是本章所涵盖的内容。

我们将探索如何不仅执行逻辑，而且还测试、保护和包含该逻辑，以便系统能够在开放、不可预测的环境中安全运行。

在 A 部分（本部分）中，我们将探讨：

+   MCP 中的测试是什么？MCP 检查器有何帮助？
+   MCP 系统中安全性的重要性
+   MCP 中的关键漏洞
+   现实世界的威胁，如提示注入、工具中毒、服务器模拟和过度能力暴露。
+   如何使用 MCP 根定义和强制执行边界。
+   应对关键威胁的可能解决方案

每个主题都会有示例和演练支持，以便您不仅可以理解理论，还可以在自己的堆栈中自信地实现它。

在 B 部分中，我们将继续介绍基于 Docker 的沙盒，展示如何将服务器容器化以实现运行时安全性以及跨 Claude Desktop、Cursor IDE 和自定义客户端的无缝本地部署。

+   什么是沙盒以及它为何如此重要？
+   使用 Docker 完全容器化 `FastMCP` 服务器
+   如何使用 Docker 标志强制执行运行时限制和安全边界？
+   如何将 Claude Desktop、Cursor 和自定义客户端连接到沙盒容器？

与往常一样，每个概念都会有具体的例子、演练和实用技巧作为支持，以帮助您掌握想法和实施。

让我们开始吧！

## 使用 MCP Inspector 测试 MCP 服务器

`MCP Inspector` 是一款专为测试和调试 MCP 服务器而构建的工具。它提供了一个基于 Web 的界面，用于与服务器交互、查看可用工具、发送测试输入并实时验证结果。

它对于测试服务器端实现特别有用，无需连接到客户端 LLM 或 Claude 或 Cursor 等应用程序，让您可以验证行为而无需承担任何与 LLM 相关的成本。

根据设计，`MCP Inspector` 可以与任何 MCP 服务器传输一起工作。

### 启动 MCP Inspector 并与服务器交互

运行 Inspector 最简单的方法是使用`npx`（Node 的包运行器），它会从 NPM（Node 的包管理器）获取 Inspector。您无需克隆任何仓库。只需确保您的系统上安装了最新的 Node.js（v22 或更高版本）。

---

要根据您的操作系统下载并在系统上设置 Node.js，请查看下面链接的官方安装指南：

使用以下命令启动 MCP Inspector：

```
npx @modelcontextprotocol/inspector
```

首次运行此命令时，它将下载并启动 MCP Inspector。后续运行时，它将直接启动它。

`http://localhost:6274` 默认情况下，它将在 `http://127.0.0.1:6274`（端口 6274）上启动 Web UI。运行后，您会在终端中看到一条消息，请保持此终端打开并运行该命令。您将看到类似以下内容：

![](img/c7d70f6164600e2783eb2aa47b790c83.png)

现在，使用 CLI 中显示的地址在浏览器中打开 MCP Inspector，其中包含预填充的会话令牌，例如`http://localhost:6274/?MCP_PROXY_AUTH_TOKEN=<token>`：

![](img/65f8fb6af51b369a275ed55112e0722c.png)

这将直接带您进入 MCP Inspector 界面。

接下来，我们需要连接到我们的服务器。

---

作为参考，我们将使用课程最后一部分（采样）中的服务器脚本：

```python
from fastmcp import FastMCP, Context

mcp = FastMCP(name=&quot;Document Assistant&quot;)

@mcp.tool()
async def summarize_document(document_text: str, ctx: Context) -&gt; str:
    &quot;&quot;&quot;Generate a summary of the given document text.&quot;&quot;&quot;
    # Request the client&#x27;s LLM to summarize the document
    response = await ctx.sample(
        messages=f&quot;Summarize the following document:\n{document_text}&quot;,
        system_prompt=&quot;&quot;&quot;You are an expert summarizer.
                      Extract the key ideas and summarize them.&quot;&quot;&quot;,
        temperature=0.7,
        max_tokens=300,
        model_preferences=&quot;gpt-4o&quot;)
    # Extract the generated summary text from the response
    summary = response.text.strip()
    return f&quot;Summary:\n{summary}.&quot;


if __name__ == &quot;__main__&quot;:
    mcp.run()   # stdio
```

在用户界面中，您可以选择连接到 MCP 服务器。有一个下拉菜单可供选择传输方式。请查看以下屏幕截图：

![](img/fc3a3a2c7ddeac93bc216c8359447f2f.png)

我们需要根据服务器配置选择传输方式。如果使用 `STDIO`，请提供服务器脚本的完整文件路径；如果 MCP Inspector 是从项目目录启动的，则只需提供文件名即可。我们的参考服务器使用`stdio`传输方式，因此我们将传输类型设置为 `STDIO`。

> 👉重要的是确保如果传输是 `STDIO`，那么无论在何处启动 MCP Inspector，服务器所需的所有包都是可用的，无论是在系统级别还是在激活的虚拟环境中。

如果是 `SSE`，即，我们需要选择传输类型为 `SSE` 并指定服务器的 URL：`mcp.run(transport="sse", host="127.0.0.1", port=8000)`
