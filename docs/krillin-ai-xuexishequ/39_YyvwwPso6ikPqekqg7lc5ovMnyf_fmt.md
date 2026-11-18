
# Agent 智能体

> 来源：[https://kcn9ut4m7icc.feishu.cn/docx/KnWpdXMCcohZETx2VlbcugJxn2g](https://kcn9ut4m7icc.feishu.cn/docx/KnWpdXMCcohZETx2VlbcugJxn2g)

AI assistant that uses tools and reasoning to perform coding tasks with minimal supervision

人工智能助手，利用工具和推理在最少监督下执行编码任务

You can delegate tasks to Cursor Agent and let it work alongside you. Agent performs its work in Composer and is built on top of it. Make sure to read about Composer to best work with Agent.

您可以委托任务给 Cursor 代理，并让它与您并肩工作。代理在 Composer 中执行其工作，并建立在它之上。确保阅读有关 Composer 的内容，以便最佳地与代理协同工作。

## Tools 工具

Agent has access to multiple tools, including

代理可访问多个工具，包括

+ Reading & Writing code

阅读与编写代码

+ Searching codebase 搜索代码库

+ Call MCP servers 调用 MCP 服务器

+ Run terminal commands 运行终端命令

+ Automatic web search for up-to-date information

自动搜索最新信息

The reasoning capabilities of Agent enables some very powerful workflows where it can perform many consecutive actions without much supervision. When needed, Agent will automatically search the web to find relevant information, documentation, or examples to help with your task.

代理的推理能力使其能够执行许多连续的操作，而无需过多监督，从而实现一些非常强大的工作流程。当需要时，代理会自动搜索网络以找到相关的信息、文档或示例，以帮助您完成任务。

![](img/3c465436da839a3323d3eba3c8141577.png)

> Agent can make up to 25 tool calls before stopping. When the limit is reached, you can press “Continue” to let Agent make more tool calls (every “Continue” call is counted as one request).

> 代理在停止前可以最多调用 25 次工具。当达到限制时，您可以按“继续”以让代理进行更多工具调用（每次“继续”调用计为一个请求）。

### Terminal 终端

When Agent runs terminal commands, it uses VS Code’s terminal profiles to determine which shell to use. It iterates through the available profiles, starting with the default one, and selects the first profile that supports command detection. This means the shell used by Agent might differ from your default system shell if another compatible terminal profile is found first.

当“Cursor – Agent”运行终端命令时，它使用 VS Code 的终端配置文件来确定使用哪个 shell。它遍历可用的配置文件，从默认配置文件开始，并选择第一个支持命令检测的配置文件。这意味着“Cursor – Agent”使用的 shell 可能与您的默认系统 shell 不同，如果首先找到另一个兼容的终端配置文件。

To change which terminal profile is used:

要更改使用的终端配置文件：

1. Open Command Palette (`Cmd/Ctrl+Shift+P`)

打开命令面板（`Cmd/Ctrl+Shift+P`）

1. Search for “`Terminal: Select Default Profile`”

搜索“`Terminal: Select Default Profile`”

1. Select your preferred terminal profile

选择您首选的终端配置文件

## Yolo mode YOLO 模式

With Yolo mode enabled, Agent can execute terminal commands by itself. This especially useful when running test suites. Instruct Agent with a task and how to verify changes (running a test), and it will continue until the task is completed.

启用 Yolo 模式后，Agent 可以自行执行终端命令。这在运行测试套件时特别有用。向 Agent 下达任务以及如何验证更改（运行测试），它将一直继续，直到任务完成。

### Guardrails 护栏

You can define guardrails and allow/deny lists for certain commands you don’t want Agent to run automatically. This is done from Cursor Settings

您可以定义安全栏和允许/拒绝列表，以限制某些您不希望代理自动运行的命令。这可以通过光标设置完成。

![](img/ad8d87a47206865d873cc132a6d03a19.png)

## Rules 规则

You can direct the Agent with rules. They can auto attached to any Agent request based on glob patterns, or the Agent can grab one based on the rule description.

您可以使用规则来指导代理。它们可以自动附加到任何基于通配符模式的代理请求，或者代理可以根据规则描述抓取一个。

[Read more about how you can work with rules](https://docs.cursor.com/context/rules-for-ai)

了解更多如何使用规则的信息

## Use Agent 使用代理

Start by opening a new Composer and enable Agent mode. From there, you can give it instructions on what work to perform.

首先打开一个新的 Composer 并启用代理模式。从那里，您可以给它下达要执行的工作指令。

![](img/9ac50504f7668726890fe78f3ea72438.png)

## Models 模型

You can use `claude-3.5-sonnet`, `gpt-4o` and `o3-mini` with Agent today. We’ll be adding more models soon!

您现在可以使用 `claude-3.5-sonnet`、`gpt-4o` 和 `o3-mini` 与 Agent 一起使用。我们很快将添加更多模型！

## FAQ 常见问题解答

### What’s the difference between Agent and Composer? Agent 与 Composer 之间的区别是什么？

You can toggle between Normal and Agent mode in Composer. The main difference is that Agent will think harder, use reasoning and tools to solve problems thrown at it. Normal mode (Edit) is for single-turn edits, while Ask mode helps you understand and explore your code.

您可以在 Composer 中切换正常模式和代理模式。主要区别在于代理会思考得更深入，使用推理和工具来解决抛给它的难题。正常模式（编辑）用于单次编辑，而询问模式有助于您理解和探索您的代码。
