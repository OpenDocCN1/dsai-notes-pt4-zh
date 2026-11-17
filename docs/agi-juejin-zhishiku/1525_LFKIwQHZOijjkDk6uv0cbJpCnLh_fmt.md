
# Langchain Agent - 实用技巧汇总

> 来源：<https://agijuejin.feishu.cn/docx/JwNKd7fbfoWdnCxzemZc6RLMnSz>

> 作者：lucas大叔

> 原文：<https://zhuanlan.zhihu.com/p/697005461>

## 重复的工具使用（Repeated tool use with agents）

当知道用户输入所需的工具使用具体顺序时，`Chain` 非常有效。但是对于某些场景，使用工具的次数取决于输入。在这些情况下，我们希望模型自己决定使用工具的次数和顺序。`Agent` 支持这种自主式的模型工具使用。

`LangChain` 有许多内置的针对不同的用例优化过的 agent，此处将使用 `tool calling agent`。这通常是最可靠的 agent 类型，也是大多数场景的推荐使用的 agent 类型。“Tool calling”在本示例中指一种特定类型的模型 API，允许显式地将工具定义传递给模型并获得显式的工具调用。

![](img/211e59de9fb2a85db89ab54fc0ef7f21.png)

### 环境配置

安装下列 packages

```
%pip install --upgrade --quiet langchain langchainhub
```

### 创建工具

首先创建需要调用的工具。在本示例中，我们将从函数创建自定义工具。

```
from langchain_core.tools import tool


@tool
def multiply(first_int: int, second_int: int) -> int:
    """Multiply two integers together."""
    return first_int * second_int


@tool
def add(first_int: int, second_int: int) -> int:
    "Add two integers."
    return first_int + second_int


@tool
def exponentiate(base: int, exponent: int) -> int:
    "Exponentiate the base to the exponent power."
    return base**exponent


tools = [multiply, add, exponentiate]
```

### 创建 prompt

```
from langchain import hub
from langchain.agents import AgentExecutor, create_tool_calling_agent

# Get the prompt to use - you can modify this!
prompt = hub.pull("hwchase17/openai-tools-agent")
```

### 创建 Agent

需要使用带有 `tool calling` 能力的模型，本示例展示基于 OpenAI 的实现。

安装依赖

```
pip install -qU langchain-openai
```

设置环境变量

```
import getpass
import os

os.environ["OPENAI_API_KEY"] = getpass.getpass()


from langchain_openai import ChatOpenAI

llm = ChatOpenAI(model="gpt-3.5-turbo-0125")
# Construct the tool calling agent
agent = create_tool_calling_agent(llm, tools, prompt)
# Create an agent executor by passing in the agent and tools
agent_executor = AgentExecutor(agent=agent, tools=tools, verbose=True)
```

### 调用 Agent

```
agent_executor.invoke(
    {
        "input": "Take 3 to the fifth power and multiply that by the sum of twelve and three, then square the whole result"
    }
)
```

中间过程，可以看到模型自主决定调用工具及次数

```
> Entering new AgentExecutor chain...

Invoking: `exponentiate` with `{'base': 3, 'exponent': 5}`
responded: [{'text': "Okay, let's break this down step-by-step:", 'type': 'text'}, {'id': 'toolu_01CjdiDhDmMtaT1F4R7hSV5D', 'input': {'base': 3, 'exponent': 5}, 'name': 'exponentiate', 'type': 'tool_use'}]

243
Invoking: `add` with `{'first_int': 12, 'second_int': 3}`
responded: [{'text': '3 to the 5th power is 243.', 'type': 'text'}, {'id': 'toolu_01EKqn4E5w3Zj7bQ8s8xmi4R', 'input': {'first_int': 12, 'second_int': 3}, 'name': 'add', 'type': 'tool_use'}]

15
Invoking: `multiply` with `{'first_int': 243, 'second_int': 15}`
responded: [{'text': '12 + 3 = 15', 'type': 'text'}, {'id': 'toolu_017VZJgZBYbwMo2KGD6o6hsQ', 'input': {'first_int': 243, 'second_int': 15}, 'name': 'multiply', 'type': 'tool_use'}]

3645
Invoking: `multiply` with `{'first_int': 3645, 'second_int': 3645}`
responded: [{'text': '243 * 15 = 3645', 'type': 'text'}, {'id': 'toolu_01RtFCcQgbVGya3NVDgTYKTa', 'input': {'first_int': 3645, 'second_int': 3645}, 'name': 'multiply', 'type': 'tool_use'}]

13286025So 3645 squared is 13,286,025.

Therefore, the final result of taking 3 to the 5th power (243), multiplying by 12 + 3 (15), and then squaring the whole result is 13,286,025.

> Finished chain.
```

输出结果

```
{'input': 'Take 3 to the fifth power and multiply that by the sum of twelve and three, then square the whole result',
 'output': 'So 3645 squared is 13,286,025.\n\nTherefore, the final result of taking 3 to the 5th power (243), multiplying by 12 + 3 (15), and then squaring the whole result is 13,286,025.'}
```

## 在多个工具间选择（Choosing between multiple tools）

本节介绍如何扩展 `chain`，使其能够从多个工具中选择要调用的工具。我们将重点放在 `chain` 上，因为 `agent` 默认可以在多个工具之间进行路由。

使用一个工具和多个工具之间的主要区别在于，我们不能提前确定模型将调用哪个工具，因此不能把特定工具硬编码到 `chain` 中。相反，我们将添加 `call_tools`，这是一个 `RunnableLambda`，它接收包含工具调用的 `AIMessage` 输出，并将其路由到正确的工具。

环境配置、创建工具同上节，不再赘述。

```
from operator import itemgetter
from typing import Dict, List, Union

from langchain_core.messages import AIMessage
from langchain_core.runnables import (
    Runnable,
    RunnableLambda,
    RunnableMap,
    RunnablePassthrough,
)

tools = [multiply, exponentiate, add]
llm_with_tools = llm.bind_tools(tools)
tool_map = {tool.name: tool for tool in tools}


def call_tools(msg: AIMessage) -> Runnable:
    """Simple sequential tool calling helper."""
    tool_map = {tool.name: tool for tool in tools}
    tool_calls = msg.tool_calls.copy()
    for tool_call in tool_calls:
        tool_call["output"] = tool_map[tool_call["name"]].invoke(tool_call["args"])
    return tool_calls


chain = llm_with_tools | call_tools
```

测试乘法问题

```
chain.invoke("What's 23 times 7")

[{'name': 'multiply',
  'args': {'first_int': 23, 'second_int': 7},
  'id': 'toolu_01Wf8kUs36kxRKLDL8vs7G8q',
  'output': 161}]
```

测试加法问题

```
chain.invoke("add a million plus a billion")

[{'name': 'add',
  'args': {'first_int': 1000000, 'second_int': 1000000000},
  'id': 'toolu_012aK4xZBQg2sXARsFZnqxHh',
  'output': 1001000000}]
```

测试指数问题

```
chain.invoke("cube thirty-seven")

[{'name': 'exponentiate',
  'args': {'base': 37, 'exponent': 3},
  'id': 'toolu_01VDU6X3ugDb9cpnnmCZFPbC',
  'output': 50653}]
```

## 使用不支持 tool calling 的模型（Using models that don't support tool calling）

本节将构建不依赖于任何特殊的模型 API（例如 `tool calling`）的 `chain`，提示模型直接调用工具。

环境配置、创建工具同上节，不再赘述。

### 创建 prompt

编写 `prompt` 指定模型可以访问的工具、工具的参数以及模型期望的输出格式。本示例中我们将指示模型输出格式为 `{"name": "...", "arguments": {...}}` 的 JSON 数据块。

```
from langchain.tools.render import render_text_description

rendered_tools = render_text_description([multiply])
rendered_tools
```

> 'multiply: multiply(first_int: int, second_int: int) -> int - Multiply two integers together.'

```
from langchain_core.prompts import ChatPromptTemplate

system_prompt = f"""You are an assistant that has access to the following set of tools. Here are the names and descriptions for each tool:
{rendered_tools}
Given the user input, return the name and input of the tool to use. Return your response as a JSON blob with 'name' and 'arguments' keys."""

prompt = ChatPromptTemplate.from_messages(
    [("system", system_prompt), ("user", "{input}")]
)
```

### 增加输出解析

使用 `JsonOutputParser` 将模型的输出解析为 JSON 格式。

```
from langchain_core.output_parsers import JsonOutputParser
from langchain_openai import ChatOpenAI

model = ChatOpenAI(model="gpt-3.5-turbo", temperature=0)
chain = prompt | model | JsonOutputParser()
chain.invoke({"input": "what's thirteen times 4"})
```

解析结果

```
{'name': 'multiply', 'arguments': {'first_int': 13, 'second_int': 4}}
```

### 调用工具

将模型生成的 `arguments` 传递给 `chain`，作为 `chain` 的一部分来调用该工具。



```python
from operator import itemgetter

chain = prompt | model | JsonOutputParser() | itemgetter("arguments") | multiply
chain.invoke({"input": "what's thirteen times 4"})
```

## 从多个工具选择

假设有多个工具希望`chain`能够从中选择，如果想要运行模型选择的工具，可以使用根据模型输出返回工具的函数。具体而言，函数将返回其自己的子链，该子链获取模型输出的`arguments`部分并将其传递给所选的工具：

```python
tools = [add, exponentiate, multiply]


def tool_chain(model_output):
    tool_map = {tool.name: tool for tool in tools}
    chosen_tool = tool_map[model_output["name"]]
    return itemgetter("arguments") | chosen_tool
    
rendered_tools = render_text_description(tools)
system_prompt = f"""You are an assistant that has access to the following set of tools. Here are the names and descriptions for each tool:
{rendered_tools}
Given the user input, return the name and input of the tool to use. Return your response as a JSON blob with 'name' and 'arguments' keys."""

prompt = ChatPromptTemplate.from_messages(
    [("system", system_prompt), ("user", "{input}")]
)

chain = prompt | model | JsonOutputParser() | tool_chain
chain.invoke({"input": "what's 3 plus 1132"})
```

### 输出结果

```
1135
```

## 返回工具输入

上小节的`chain`只返回了工具输出，返回工具输入也是有帮助的。通过`RunnablePassthrough.assign` 工具输出，我们可以很容易地使用LCEL实现这一点。接受`RunnablePassrthrough` 组件的任何输入（假设是一个字典），并向其添加一个键，同时仍然传递当前输入中的所有内容：

```python
from langchain_core.runnables import RunnablePassthrough

chain = (
    prompt | model | JsonOutputParser() | RunnablePassthrough.assign(output=tool_chain)
)
chain.invoke({"input": "what's 3 plus 1132"})
```

### 输出结果

```
{'name': 'add',
 'arguments': {'first_int': 3, 'second_int': 1132},
 'output': 1135}
```

## 人在环路（Human-in-the-loop）

有时我们不信任模型自己调用的某些工具，这种情况下可以做的一件事是在调用工具之前需要人类批准。

### 创建chain

```python
from langchain_openai import ChatOpenAI

llm = ChatOpenAI(model="gpt-3.5-turbo-0125")

from operator import itemgetter
from typing import Dict, List

from langchain_core.messages import AIMessage
from langchain_core.runnables import Runnable, RunnablePassthrough
from langchain_core.tools import tool


@tool
def count_emails(last_n_days: int) -> int:
    """Multiply two integers together."""
    return last_n_days * 2


@tool
def send_email(message: str, recipient: str) -> str:
    "Add two integers."
    return f"Successfully sent email to {recipient}."


tools = [count_emails, send_email]
llm_with_tools = llm.bind_tools(tools)


def call_tools(msg: AIMessage) -> List[Dict]:
    """Simple sequential tool calling helper."""
    tool_map = {tool.name: tool for tool in tools}
    tool_calls = msg.tool_calls.copy()
    for tool_call in tool_calls:
        tool_call["output"] = tool_map[tool_call["name"]].invoke(tool_call["args"])
    return tool_calls


chain = llm_with_tools | call_tools
chain.invoke("how many emails did i get in the last 5 days?")
```

### 调用结果

```
[{'name': 'count_emails',
  'args': {'last_n_days': 5},
  'id': 'toolu_012VHuh7vk5dVNct5SgZj3gh',
  'output': 10}]
```

### 增加人类批准

可以在`tool_chain`函数中添加简单的人类批准步骤：

```python
import json


def human_approval(msg: AIMessage) -> Runnable:
    tool_strs = "\n\n".join(
        json.dumps(tool_call, indent=2) for tool_call in msg.tool_calls
    )
    input_msg = (
        f"Do you approve of the following tool invocations\n\n{tool_strs}\n\n"
        "Anything except 'Y'/'Yes' (case-insensitive) will be treated as a no."
    )
    resp = input(input_msg)
    if resp.lower() not in ("yes", "y"):
        raise ValueError(f"Tool invocations not approved:\n\n{tool_strs}")
    return msg
    
chain = llm_with_tools | human_approval | call_tools
chain.invoke("how many emails did i get in the last 5 days?")
```

### 人工干预

```
Do you approve of the following tool invocations

{
  "name": "count_emails",
  "args": {
    "last_n_days": 5
  },
  "id": "toolu_01LCpjpFxrRspygDscnHYyPm"
}

Anything except 'Y'/'Yes' (case-insensitive) will be treated as a no. yes
```

### 返回结果

```
[{'name': 'count_emails',
  'args': {'last_n_days': 5},
  'id': 'toolu_01LCpjpFxrRspygDscnHYyPm',
  'output': 10}]
```

### 测试人工拒绝情况

```python
chain.invoke("Send sally@gmail.com an email saying 'What's up homie'")
```

### 人工干预，拒绝工具调用

```
Do you approve of the following tool invocations

{
  "name": "send_email",
  "args": {
    "message": "What's up homie",
    "recipient": "sally@gmail.com"
  },
  "id": "toolu_0158qJVd1AL32Y1xxYUAtNEy"
}

Anything except 'Y'/'Yes' (case-insensitive) will be treated as a no. no
```

### 返回结果

```
ValueError: Tool invocations not approved:

{
  "name": "send_email",
  "args": {
    "message": "What's up homie",
    "recipient": "sally@gmail.com"
  },
  "id": "toolu_0158qJVd1AL32Y1xxYUAtNEy"
}
```

## 参考文档

https://python.langchain.com/docs/use_cases/tool_use/agents/
