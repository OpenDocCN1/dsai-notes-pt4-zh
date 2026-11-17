
## Azure AI 代理技术支持实践示例

```
instructions=&quot;&quot;&quot;You are a technical support agent...&quot;&quot;&quot;
```

明确规定了代理的角色和行为准则

+   工具集成：

```
functions = FunctionTool(user_functions)
toolset.add(functions)
```

使代理能调用外部提交工单的函数

+   消息处理：

```
run = agent_client.runs.create_and_process(...)
```

异步处理用户请求并执行相关操作

1.  典型交互示例

```
用户 &gt; 我的账户无法登录
代理 &gt; 请提供您的邮箱和问题详情...
用户 &gt; email@example.com, 错误代码500
代理 &gt; 已提交工单ticket-3a7b42.txt
```

1.  扩展性

+   可轻松添加更多支持功能（如知识库查询）

+   支持多轮对话上下文保持

+   可集成到 Web 服务或客服系统

> 这个实现展示了如何将 Azure AI 服务与实际业务功能（工单系统）结合，创建智能化的技术支持解决方案。代码结构清晰，遵循了良好的资源管理实践（如 `with` 语句和清理操作）。

可能的输出

```
You&#x27;re chatting with: support-agent (asst_CM8ryFG8Mc4GToL1LXBWInSg)
Enter a prompt (or type &#x27;quit&#x27; to exit): I have a technical problem
Last Message: Could you please provide your email address and a description of the problem you&#x27;re experiencing? That way, I can create a support ticket for you.
Enter a prompt (or type &#x27;quit&#x27; to exit): alex@contoso.com
Last Message: Thanks for providing your email address! Please describe the technical problem you&#x27;re facing so I can include it in the support ticket.
Enter a prompt (or type &#x27;quit&#x27; to exit): my computer won&#x27;t start
Last Message: Your support ticket has been submitted successfully! Your ticket ID is **80b33b**, and the details have been saved in the file **ticket-80b33b.txt**. Our team will get back to you shortly.
Enter a prompt (or type &#x27;quit&#x27; to exit): quit

Conversation Log:

MessageRole.USER: I have a technical problem

MessageRole.AGENT: Could you please provide your email address and a description of the problem you&#x27;re experiencing? That way, I can create a support ticket for you.

MessageRole.USER: alex@contoso.com

MessageRole.AGENT: Thanks for providing your email address! Please describe the technical problem you&#x27;re facing so I can include it in the support ticket.

MessageRole.USER: my computer won&#x27;t start

MessageRole.AGENT: Your support ticket has been submitted successfully! Your ticket ID is **80b33b**, and the details have been saved in the file **ticket-80b33b.txt**. Our team will get back to you shortly.

Deleted agent
(labenv) PS /home/user1-52717560/ai-agents/Labfiles/03-ai-agent-functions/Python&gt; dir                                

    Directory: /home/user1-52717560/ai-agents/Labfiles/03-ai-agent-functions/Python

UnixMode         User Group         LastWriteTime         Size Name
--------         ---- -----         -------------         ---- ----
drwxr-xr-x user1-5271 user1-5271   7/4/2025 08:46         4096 __pycache__
                 7560 7560
-rw-r--r-- user1-5271 user1-5271   7/4/2025 08:45         3744 agent.py
                 7560 7560
-rw-r--r-- user1-5271 user1-5271   7/4/2025 08:19           29 requirements.txt
                 7560 7560
-rw-r--r-- user1-5271 user1-5271   7/4/2025 08:47           91 ticket-80b33b.txt
                 7560 7560
-rw-r--r-- user1-5271 user1-5271   7/4/2025 08:45          912 user_functions.py
                 7560 7560

(labenv) PS /home/user1-52717560/ai-agents/Labfiles/03-ai-agent-functions/Python&gt; cat ticket-80b33b.txt                                                       
Support ticket: 80b33b
Submitted by: alex@contoso.com
Description:
My computer won&#x27;t start.
(labenv) PS /home/user1-52717560/ai-agents/Labfiles/03-ai-agent-functions/Python&gt;
```

> 这个输出完整展示了基于之前代码的技术支持 AI 代理的工作流程和实际效果。让我们结合代码逐步解析：

1.  代理初始化阶段

+   代码中`create_agent()`创建的`support-agent`已成功启动

+   显示代理 ID：asst_CM8ryFG8Mc4GTOLLXBWInSg

+   符合代码中的打印语句：

```
print(f&quot;You&#x27;re chatting with: {agent.name} ({agent.id})&quot;)
```

1.  多轮对话流程

+   第一轮

+   用户输入："I have a technical problem"

+   代理响应要求提供邮箱和问题描述

+   对应代码中配置的指令：

```
instructions=&quot;&quot;&quot;...get their email address and a description...&quot;&quot;&quot;
```

+   第二轮

+   用户提供邮箱："alex@contoso.com"

+   代理继续询问具体问题

+   说明代码正确处理了分步信息收集

1.  工单提交阶段

+   用户描述问题："my computer won't start"

+   代理自动触发`submit_support_ticket()`函数：

+   生成工单 ID：80b33b（来自`uuid.uuid4()`）

+   创建文件`ticket-80b33b.txt`

+   文件内容完全符合`submit_support_ticket()`函数的格式：

```
text = f&quot;Support ticket: {ticket_number}\nSubmitted by: {email_address}\nDescription:\n{description}&quot;
```

1.  输出验证

+   终端显示的对话日志与`messages.list()`输出一致

+   文件系统确认工单文件已创建（通过`dir`和`cat`命令验证）

+   证明代码中的文件操作部分正常工作：

```
file_path.write_text(text)  # 在`submit_support_ticket()`中
```

1.  资源清理

+   会话结束时正确删除代理

+   对应代码中的清理逻辑：

```
agent_client.delete_agent(agent.id)
```

> 整个交互过程完美展示了：

1.  自然语言理解（解析用户问题）

1.  分步信息收集（邮箱→问题描述）

1.  自动功能调用（提交工单）

1.  结果反馈（返回工单 ID 和文件信息）

1.  状态持久化（保存工单文件）

> 验证了代码中所有关键组件（`Agent` 配置、函数调用、文件操作、对话管理）都按设计正常工作。最后的文件内容也证明自定义函数`submit_support_ticket()`被正确调用并传入了收集到的参数（邮箱+问题描述）。

# 使用语义内核开发 AI 代理

https://learn.microsoft.com/zh-cn/training/modules/develop-ai-agent-with-semantic-kernel/

## 什么是 AI 代理以及它们如何运作？

AI 代理是使用生成式 AI 解释数据、做出决策并代表用户或其他应用程序执行任务的程序。 它们依赖于大型语言模型 （LLM） 来执行任务，与传统程序不同，AI 代理可以自主运行，处理复杂的工作流和自动化过程，而无需持续的人工监督。 它们可以通过使用自然语言处理来理解和响应指令。

## 什么是语义内核 SDK？它如何帮助开发 AI 代理？

语义内核是一个开源 SDK，使开发人员可以轻松地将最新的 AI 模型集成到其应用程序中。 它包含一个名为“语义内核代理框架”的组件，该框架允许开发人员快速创建能够使用自然语言处理完成任务的功能代理。 语义内核 SDK 提供了一系列核心组件，如 AI 服务连接器、内存连接器、函数和插件、提示模板和过滤器，它们可以单独或组合使用，以构建强大的 AI 应用程序。

## 语义内核代理框架的关键概念是什么？

语义内核代理框架简化了代理的创建，并在集成人工输入的同时实现了多代理协作。 其核心概念包括：

+   代理： AI 代理的抽象，允许在对话中完成任务和人工交互。

+   代理线程： 管理聊天状态并存储聊天。

+   代理聊天： 多代理交互的基础，允许结构化对话和协作。

+   代理通道： 用于自定义代理开发，允许不同类型的代理参与 AgentChat。

+   代理消息： 代理通信的统一结构，提供与现有 AI 工作流的无缝通信和集成。

+   模板化： 使用动态提示配置来塑造代理行为。

+   函数和插件： 允许开发人员通过合并自定义函数来扩展代理功能。

## 什么是 Azure AI 代理？它有什么优势？

`AzureAIAgent` 类提供了一种无缝的方法来使用 Azure AI 代理服务构建和交互 AI 代理。 它通过在语义内核代理框架中提供更结构化和直观的界面来抽象管理 AI 代理的复杂性。 主要优势包括：

+   简化的代理创建： 允许开发人员使用最少的配置来定义 AI 代理，而无需管理底层基础架构。

+   自动工具调用： 代理可以自动调用和执行工具，与 Azure AI 搜索、必应、Azure Functions 等无缝集成。

+   线程和聊天管理： 提供用于管理聊天状态的内置机制，确保多代理交互流畅。

+   安全企业集成： 允许使用无密钥身份验证和可自定义存储选项进行安全合规的 AI 代理开发。

## 如何使用语义内核创建 Azure AI 代理？

要使用 `AzureAIAgent`，您需要执行以下步骤：

1.  创建一个 Azure AI Foundry 项目。

1.  将项目连接字符串添加到语义内核应用程序代码。

1.  创建 `AzureAIAgentSettings` 对象。

1.  创建 `AzureAIAgent` 客户端。

1.  在客户端提供的代理服务上创建代理定义。

1.  根据定义创建代理。

`AzureAIAgent` 对象封装了通常与内核一起使用的所有核心功能，例如函数执行、规划和内存访问，充当一个独立的代理运行时。

## AzureAIAgent 的关键组件是什么？

`AzureAIAgent` 对象依赖于以下组件才能正常运行：

+   `AzureAISAgentSettings`： 一个自动包含环境配置中的 Azure AI 代理设置的对象。

+   `AzureAIAgent` 客户端： 用于管理与 Azure AI Foundry 项目连接的对象，允许访问与项目关联的服务和模型。

+   代理服务： `AzureAIAgent` 客户端还包含代理服务，有助于简化为项目创建、管理和运行代理的过程。

+   代理定义： 通过 Azure AI 项目客户端创建的 Azure AI 代理模型，指定要使用的 AI 部署模型以及代理的名称和说明。

+   `AzureAIAgentThread`： 自动维护代理和用户之间的聊天历史记录和状态，并允许您将消息添加到线程并使用代理从 LLM 调用响应。

这些组件协同工作，使您能够使用说明创建代理，以定义其用途并从 AI 模型调用响应。

## 什么是插件？它们如何增强 Azure AI 代理的功能？

在语义内核中，插件允许 AI 代理使用现有 API 执行其自身无法执行的任务。 插件通过函数调用工作，允许 AI 请求和使用特定函数。 语义内核将请求路由到代码库中的相应函数，并将结果返回给 LLM，以便 LLM 可以生成最终响应。 插件还需要提供描述其行为的详细信息（包括输入、输出和副作用），以便 AI 可以正确调用函数。

## 如何将插件添加到 Azure AI 代理？

要将插件与 `AzureAIAgent` 一起使用，您需要执行以下步骤：

1.  定义插件： 通过定义类并使用 `@kernel_function` 修饰符注释其方法来创建插件。 `kernel_function` 修饰符允许语义内核知道此函数可由 AI 调用，并且还支持一个 description 属性来帮助 AI 了解如何使用函数。

1.  将插件添加到您的代理： 定义插件后，您可以通过创建插件的新实例并将其添加到代理的插件集合中来将其添加到 `AzureAIAgent`。

1.  调用插件的函数： 您可以通过在代理的消息线程上使用提示来调用插件的函数。

通过执行这些步骤，您可以扩展 `AzureAIAgent`，使其能够与 API 交互并执行更高级的任务，从而使 AI 更强大且能够高效地处理实际应用程序。

## 练习-使用语义内核 SDK 开发 Azure AI 代理

https://github.com/MicrosoftLearning/mslearn-ai-agents/blob/main/Instructions/04-semantic-kernel.md

`semantic-kernel.py`
