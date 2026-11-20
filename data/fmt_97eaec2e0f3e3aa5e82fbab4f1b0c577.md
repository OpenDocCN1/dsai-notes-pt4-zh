
# LangGraph 构建 AI 智能聊天机器人

> 来源：[https://v11enp9ok1h.feishu.cn/docx/TwtDdSfXHodObLxvHb5crH5AnEh](https://v11enp9ok1h.feishu.cn/docx/TwtDdSfXHodObLxvHb5crH5AnEh)

## 概述

> 我们将介绍如何设计和实现由 LLM 驱动的聊天机器人的示例。此[聊天机器人](https://python.langchain.ac.cn/docs/concepts/chat_models/)将能够进行对话，并记住之前与聊天模型的交互。

请注意，我们构建的此聊天机器人将仅使用语言模型进行对话。您可能正在寻找其他几个相关概念

+ [`对话式 RAG`](https://python.langchain.ac.cn/docs/tutorials/qa_chat_history/)：在外部数据源上启用聊天机器人体验

+ [`代理`](https://python.langchain.ac.cn/docs/tutorials/agents/)：构建可以采取行动的聊天机器人

本教程将涵盖基础知识，这些知识将对这两个更高级的主题有所帮助，但如果您愿意，也可以直接跳到那里。

![](img/e01cc656f63096bcfa97f0c4885f1a41.png)

## 设置

### Jupyter Notebook

> 本指南（以及文档中的大多数其他指南）使用 Jupyter Notebook，并假定读者也是如此。Jupyter Notebook 非常适合学习如何使用 LLM 系统，因为通常情况下，事情可能会出错（意外输出、API 宕机等），并且在交互式环境中浏览指南是更好地理解它们的好方法。

本教程和其他教程可能最方便在 [Jupyter Notebook](https://jupyter.pythonlang.cn/install) 中运行。有关如何安装的说明，请参阅此处。

### 安装

在本教程中，我们将需要 `langchain-core` 和 `langgraph`。本指南需要 `langgraph` >= 0.2.28。

```
pip install langchain-core langgraph>0.2.27
```

### LangSmith

> 您使用 [LangChain](https://smith.langchain.com/) 构建的许多应用程序将包含多个步骤，其中包含多次 LLM 调用。随着这些应用程序变得越来越复杂，能够检查链或代理内部究竟发生了什么变得至关重要。最好的方法是使用 `LangSmith`。

在上面的链接注册后，请确保设置您的环境变量以开始记录追踪

```
export LANGSMITH_TRACING="true"
export LANGSMITH_API_KEY="..."
```

或者，如果在 Notebook 中，您可以使用以下命令设置它们

```
import getpass
import os

os.environ["LANGSMITH_TRACING"] = "true"
os.environ["LANGSMITH_API_KEY"] = getpass.getpass()
```

## 快速入门

> 首先，让我们学习如何单独使用语言模型。`LangChain` 支持许多不同的语言模型，您可以互换使用它们 - 选择您想要使用的模型！

选择 [`聊天模型`](https://python.langchain.ac.cn/docs/integrations/chat/)：`OpenAI`

`pip install -qU "langchain[openai]"`

```
import getpass
import os

if not os.environ.get("OPENAI_API_KEY"):
  os.environ["OPENAI_API_KEY"] = getpass.getpass("Enter API key for OpenAI: ")

from langchain.chat_models import init_chat_model

model = init_chat_model("gpt-4o-mini", model_provider="openai")
```

让我们首先直接使用模型。`ChatModel` 是 `LangChain` "Runnables" 的实例，这意味着它们公开了一个用于与之交互的标准接口。要简单地调用模型，我们可以将消息列表传递给 `.invoke` 方法。

```
from langchain_core.messages import HumanMessage
model.invoke([HumanMessage(content="Hi! I'm Bob")])
```

API 参考：[`HumanMessage`](https://python.langchain.ac.cn/api_reference/core/messages/langchain_core.messages.human.HumanMessage.html)

```
AIMessage(content='Hi Bob! How can I assist you today?', additional_kwargs={'refusal': None}, response_metadata={'token_usage': {'completion_tokens': 10, 'prompt_tokens': 11, 'total_tokens': 21, 'completion_tokens_details': {'accepted_prediction_tokens': 0, 'audio_tokens': 0, 'reasoning_tokens': 0, 'rejected_prediction_tokens': 0}, 'prompt_tokens_details': {'audio_tokens': 0, 'cached_tokens': 0}}, 'model_name': 'gpt-4o-mini-2024-07-18', 'system_fingerprint': 'fp_0705bf87c0', 'finish_reason': 'stop', 'logprobs': None}, id='run-5211544f-da9f-4325-8b8e-b3d92b2fc71a-0', usage_metadata={'input_tokens': 11, 'output_tokens': 10, 'total_tokens': 21, 'input_token_details': {'audio': 0, 'cache_read': 0}, 'output_token_details': {'audio': 0, 'reasoning': 0}})
```

模型本身没有任何状态概念。例如，如果您提出后续问题

```
model.invoke([HumanMessage(content="What's my name?")])
```

```
AIMessage(content="I'm sorry, but I don't have access to personal information about users unless it has been shared with me in the course of our conversation. How can I assist you today?", additional_kwargs={'refusal': None}, response_metadata={'token_usage': {'completion_tokens': 34, 'prompt_tokens': 11, 'total_tokens': 45, 'completion_tokens_details': {'accepted_prediction_tokens': 0, 'audio_tokens': 0, 'reasoning_tokens': 0, 'rejected_prediction_tokens': 0}, 'prompt_tokens_details': {'audio_tokens': 0, 'cached_tokens': 0}}, 'model_name': 'gpt-4o-mini-2024-07-18', 'system_fingerprint': 'fp_0705bf87c0', 'finish_reason': 'stop', 'logprobs': None}, id='run-a2d13a18-7022-4784-b54f-f85c097d1075-0', usage_metadata={'input_tokens': 11, 'output_tokens': 34, 'total_tokens': 45, 'input_token_details': {'audio': 0, 'cache_read': 0}, 'output_token_details': {'audio': 0, 'reasoning': 0}})
```

让我们看一下示例 [`LangSmith 追踪`](https://smith.langchain.com/public/5c21cb92-2814-4119-bae9-d02b8db577ac/r)

+ 我们可以看到它没有将之前的对话回合纳入上下文，并且无法回答问题。这会带来糟糕的聊天机器人体验！

+ [`为了解决这个问题`](https://python.langchain.ac.cn/docs/concepts/chat_history/)我们需要将整个 对话历史记录 传递到模型中。让我们看看当我们这样做时会发生什么

```
from langchain_core.messages import AIMessage

model.invoke(
    [
        HumanMessage(content="Hi! I'm Bob"),
        AIMessage(content="Hello Bob! How can I assist you today?"),
        HumanMessage(content="What's my name?"),
    ]
)
```

API 参考：[`AIMessage`](https://python.langchain.ac.cn/api_reference/core/messages/langchain_core.messages.ai.AIMessage.html)

```
AIMessage(content='Your name is Bob! How can I help you today, Bob?', additional_kwargs={'refusal': None}, response_metadata={'token_usage': {'completion_tokens': 14, 'prompt_tokens': 33, 'total_tokens': 47, 'completion_tokens_details': {'accepted_prediction_tokens': 0, 'audio_tokens': 0, 'reasoning_tokens': 0, 'rejected_prediction_tokens': 0}, 'prompt_tokens_details': {'audio_tokens': 0, 'cached_tokens': 0}}, 'model_name': 'gpt-4o-mini-2024-07-18', 'system_fingerprint': 'fp_0705bf87c0', 'finish_reason': 'stop', 'logprobs': None}, id='run-34bcccb3-446e-42f2-b1de-52c09936c02c-0', usage_metadata={'input_tokens': 33, 'output_tokens': 14, 'total_tokens': 47, 'input_token_details': {'audio': 0, 'cache_read': 0}, 'output_token_details': {'audio': 0, 'reasoning': 0}})
```

现在我们可以看到我们得到了很好的回应！

这是聊天机器人会话交互能力背后的基本思想。那么我们如何最好地实现这一点呢？

## 消息持久化

> `LangGraph` 实现了内置的持久化层，使其成为支持多轮对话的聊天应用程序的理想选择。

> 将我们的聊天模型包装在最小的 `LangGraph` 应用程序中，使我们能够自动持久化消息历史记录，从而简化多轮应用程序的开发。

`LangGraph` 附带一个简单的内存 [`检查点`](https://github.langchain.ac.cn/langgraph/concepts/persistence/) 程序，我们在下面使用它。请参阅其 文档 以了解更多详细信息，包括如何使用不同的持久化后端（例如，`SQLite` 或 `Postgres`）。

```
from langgraph.checkpoint.memory import MemorySaver
from langgraph.graph import START, MessagesState, StateGraph

# Define a new graph
workflow = StateGraph(state_schema=MessagesState)

# Define the function that calls the model
def call_model(state: MessagesState):
    response = model.invoke(state["messages"])
    return {"messages": response}

# Define the (single) node in the graph
workflow.add_edge(START, "model")
workflow.add_node("model", call_model)

# Add memory
memory = MemorySaver()
app = workflow.compile(checkpointer=memory)
```

API 参考：[`MemorySaver`](https://github.langchain.ac.cn/langgraph/reference/checkpoints/#langgraph.checkpoint.memory.MemorySaver) | [`StateGraph`](https://github.langchain.ac.cn/langgraph/reference/graphs/#langgraph.graph.state.StateGraph)

我们现在需要创建一个 `config`，每次都将其传递到 `runnable` 中。此配置包含不直接属于输入的信息，但仍然有用。在这种情况下，我们想要包含一个 `thread_id`。它应该看起来像

```
config = {"configurable": {"thread_id": "abc123"}}
```
