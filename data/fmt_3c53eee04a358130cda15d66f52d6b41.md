
# Langchain Agent - 基本概念

> 来源：[https://agijuejin.feishu.cn/docx/Ay1Id8UsQocZ8vxTlT2c5x0fnac](https://agijuejin.feishu.cn/docx/Ay1Id8UsQocZ8vxTlT2c5x0fnac)

> 作者：[lucas大叔](https://www.zhihu.com/people/lucasda-shu)

> 原文：[https://zhuanlan.zhihu.com/p/694452841](https://zhuanlan.zhihu.com/p/694452841)

Agent的核心思想是使用语言模型选择要采取的行动（action）序列。在`chain`中，行动序列是硬编码在代码中。在`agent`中，语言模型被当做推理引擎，确定要采取哪些行动以及按什么顺序执行。下面将逐一介绍`Agent`中的关键组件。

## Schema

Langchain有几个抽象概念让使用`agent`变得容易。

### AgentAction

表示`agent`应该采取的行动的数据类，它包含 `tool` 属性（应该调用的工具的名称）和 `tool_input` 属性（该工具的输入）。

### AgentFinish

表示`agent`准备返回给用户时的最终结果，它包含 `return_values` 键值映射，其中包含最终的`agent`输出。它通常包含 `output` 键，其中包含作为`agent`响应的字符串。

### Intermediate Steps

表示以前的`agent`行动和CURRENT `agent`运行的相应输出。这些内容传给接下来的迭代非常重要，这样`agent`就知道它已经完成了什么工作，它的类型格式为 `List[Tuple[AgentAction, Any]]`。需要注意的是`observation`当前保留为 `Any` 类型，以保证最大限度地灵活性。在实践中，`observation`通常是字符串。

## Agent

这是[决定下一步行动的chain](https://link.zhihu.com/?target=https%3A//python.langchain.com/docs/modules/agents/agent_types/)，通常包含LLM、`prompt`和输出解析器。不同的`agent`有不同的推理提示风格、不同的编码输入方式和不同的输出解析方式。内置`agent`的完整列表请参阅 `agent types`。如果你需要进一步的掌控，还可以构建自定义`agent`。

### Agent Inputs

`Agent`的输入是键值映射，只有一个必需的键：`intermediate_steps`，它对应于上述的`Intermediate Steps`。

通常，`PromptTemplate`负责将这些键值对转换为最适合传给LLM的格式。

### Agent Outputs

输出是`agent`要执行的下一个行动或发送给用户的最终响应（`AgentActions` 或 `AgentFinish`）。具体来说，输出可以为 `Union[AgentAction, List[AgentAction], AgentFinish]` 类型格式。

输出解析器负责获取原始LLM输出，并将其转换为这三种类型之一。

### AgentExecutor

`agent`执行器是`agent`的`runtime`。`AgentExecutor`实际调用`agent`、执行它选择的行动、将行动输出传递回`agent`并重复执行直到`AgentFinish`。执行伪代码大致如下所示：

```
next_action = agent.get_action(...)
while next_action != AgentFinish:
    observation = run(next_action)
    next_action = agent.get_action(..., next_action, observation)
return next_action
```

这个流程看起来很简单，但`runtime`可以处理一些复杂问题，包括：

1. 处理`agent`选择不存在的工具
1. 处理工具错误
1. 处理`agent`生成无法解析为工具调用的输出
1. 所有级别（`agent`决策、工具调用）的日志和可观测内容输出到`stdout`或LangSmith

## Tools

工具是`agent`可以调用的函数，`Tool`抽象包含两个组件：

1. 工具的输入`schema`。这告诉LLM需要什么参数来调用该工具。如果没有输入`schema`，工具不知道什么是正确的输入。这些参数应合理地命名和描述。
1. 执行的函数，通常是调用的`python`函数。

设计工具时有两点重要的注意事项：

1. 给予`agent`访问正确工具的权限
1. 以对`agent`最有帮助的方式描述工具

不[综合考虑这两点](https://link.zhihu.com/?target=https%3A//python.langchain.com/docs/integrations/tools/)，将无法建立有效的`agent`。如果不给予`agent`访问正确工具集的权限，它将永远无法实现你给它的目标。如果不能很好地描述这些工具，`agent`将不知道如何正确地使用它们。

LangChain提供了广泛的内置工具，你也可以轻松定义自己的工具（包括自定义描述）。查看内置工具的完整列表，请参阅 工具集成部分 。

## Toolkits

对于许多常见任务，`agent`将需要一组相关工具。为[此，LangChain](https://link.zhihu.com/?target=https%3A//python.langchain.com/docs/integrations/toolkits/)提出了工具包的概念 - 将用于完成特定目标的3-5个工具集成为一组。例如，GitHub工具包包含搜索GitHub问题、读取文件、发表评论等工具。

LangChain提供了广泛的工具包集合，可供开始使用。查看内置工具包的完整列表，请参阅 工具包集成部分 。

## 参考文档

[https://python.langchain.com/docs/modules/agents/concepts/](https://python.langchain.com/docs/modules/agents/concepts/)

发布于 2024-04-28 20:06 ・IP 属地江苏
