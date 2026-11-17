
# Langchain Agent - 自定义agent

> 来源：[https://agijuejin.feishu.cn/docx/Yic8dYVc3ozUQNx8RnGcYFz4nab](https://agijuejin.feishu.cn/docx/Yic8dYVc3ozUQNx8RnGcYFz4nab)

> 作者：[lucas大叔](https://www.zhihu.com/people/lucasda-shu)

> 原文：[https://zhuanlan.zhihu.com/p/694458324](https://zhuanlan.zhihu.com/p/694458324)

本文将详细介绍如何创建自定义的agent。我们将使用OpenAI工具调用创建agent，首先创建没有记忆功能的agent，然后展示如何添加记忆功能。

## 加载LLM

```
from langchain_openai import ChatOpenAI

llm = ChatOpenAI(model="gpt-3.5-turbo", temperature=0)
```

## 定义工具

在构建自定义agent时，需要提供可以使用的工具列表。除了实际调用的函数外，工具还包括以下部分：

+ `name`（`str`）：必填项，提供给agent的工具集合中的唯一名称
+ `description`（`str`）：可选项，但建议提供。因为agent根据描述决定工具使用
+ `args_schema`（`Pydantic BaseModel`）：可选项，但建议提供。用于提供更多的信息（例如，few-shot样例）或期望参数的验证

```
from langchain.agents import tool


@tool
def get_word_length(word: str) -> int:
    """Returns the length of a word."""
    return len(word)


get_word_length.invoke("abc")

tools = [get_word_length]
```

## 创建Prompt

由于OpenAI Function Calling已经针对工具使用进行了微调，几乎不需要任何关于如何推理或如何输出格式的说明。我们只需要两个输入变量：`input`和`agent_scratchpad`。`input`是包含用户目标的字符串。`agent_scratchpad`是消息序列，包含先前的工具调用和相应的工具输出。

```
from langchain_core.prompts import ChatPromptTemplate, MessagesPlaceholder

prompt = ChatPromptTemplate.from_messages(
    [
        (
            "system",
            "You are very powerful assistant, but don't know current events",
        ),
        ("user", "{input}"),
        MessagesPlaceholder(variable_name="agent_scratchpad"),
    ]
)
```

## 绑定工具到LLM

agent如何知道它可以使用哪些工具？在这种情况下，我们依赖于OpenAI工具调用LLM，它将工具作为单独的参数，并经过专门训练，知道何时调用这些工具。

为了将工具传递给agent，只需要将它们格式化为OpenAI工具格式，并将它们传递给模型。（通过`bind`函数，确保它们在每次调用模型时都会被传递。）

```
llm_with_tools = llm.bind_tools(tools)
```

## 创建Agent

把这些部分放在一起，就可以创建agent了。我们将导入两个最新的实用程序：用于将中间步骤（agent action、工具输出对）格式化为可以发送给模型的输入消息的组件；用于将输出消息转换为agent action/agent finish的组件。

```
from langchain.agents.format_scratchpad.openai_tools import (
    format_to_openai_tool_messages,
)
from langchain.agents.output_parsers.openai_tools import OpenAIToolsAgentOutputParser

agent = (
    {
        "input": lambda x: x["input"],
        "agent_scratchpad": lambda x: format_to_openai_tool_messages(
            x["intermediate_steps"]
        ),
    }
    | prompt
    | llm_with_tools
    | OpenAIToolsAgentOutputParser()
)

from langchain.agents import AgentExecutor

agent_executor = AgentExecutor(agent=agent, tools=tools, verbose=True)
list(agent_executor.stream({"input": "How many letters in the word eudca"}))
```

## 增加记忆

前面已经实现了agent，但是没有状态，不记得之前交互的内容。这意味着不能很容易的问后续问题，增加记忆可以解决这个问题。

为此需要做两件事：

1. 在`prompt`中添加记忆变量的位置
2. 跟踪聊天历史

首先在`prompt`添加关键字为`chat_history`的消息占位符，放在新的用户输入之前（以跟随对话流程）。

```
from langchain_core.prompts import MessagesPlaceholder

MEMORY_KEY = "chat_history"
prompt = ChatPromptTemplate.from_messages(
    [
        (
            "system",
            "You are very powerful assistant, but bad at calculating lengths of words.",
        ),
        MessagesPlaceholder(variable_name=MEMORY_KEY),
        ("user", "{input}"),
        MessagesPlaceholder(variable_name="agent_scratchpad"),
    ]
)
```

然后创建`list`跟踪聊天历史

```
from langchain_core.messages import AIMessage, HumanMessage

chat_history = []
```

将以上实现组装在一起

```
agent = (
    {
        "input": lambda x: x["input"],
        "agent_scratchpad": lambda x: format_to_openai_tool_messages(
            x["intermediate_steps"]
        ),
        "chat_history": lambda x: x["chat_history"],
    }
    | prompt
    | llm_with_tools
    | OpenAIToolsAgentOutputParser()
)
agent_executor = AgentExecutor(agent=agent, tools=tools, verbose=True)
```

执行时，我们需要追踪输入和输出作为对话历史

```
input1 = "how many letters in the word educa?"
result = agent_executor.invoke({"input": input1, "chat_history": chat_history})
chat_history.extend(
    [
        HumanMessage(content=input1),
        AIMessage(content=result["output"]),
    ]
)
agent_executor.invoke({"input": "is that a real word?", "chat_history": chat_history})
```

## 参考文档

[https://python.langchain.com/docs/modules/agents/how_to/custom_agent/](https://link.zhihu.com/?target=https%3A//python.langchain.com/docs/modules/agents/how_to/custom_agent/)

[https://python.langchain.com/docs/modules/tools/custom_tools/](https://link.zhihu.com/?target=https%3A//python.langchain.com/docs/modules/tools/custom_tools/)
