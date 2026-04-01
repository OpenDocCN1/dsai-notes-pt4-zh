# Langchain Agent - Agent类型

> 来源：[https://agijuejin.feishu.cn/docx/LhNcdcDWsoQcLFxiAAGcMhgSnxb](https://agijuejin.feishu.cn/docx/LhNcdcDWsoQcLFxiAAGcMhgSnxb)

> 作者：[lucas大叔](https://www.zhihu.com/people/lucasda-shu)

> 原文： [https://zhuanlan.z](https://zhuanlan.zhihu.com/p/694458202)hihu.com/p/694458...

## Agent类型

从几个维度将所有可用的agent进行了分类。

### 预期模型类型（ Intended Model Type）

无论agent是为Chat模型（接收消息，输出消息）还是LLM（接收字符串，输出字符串）而设计的，主要影响的是提示策略的使用。你可以使用一个不同类型的模型来运行agent，但它可能不会产生相同质量的结果。

### 支持聊天历史（ Supports Chat History）

Agent是否支持聊天历史。如果支持，那意味着它可以用作聊天机器人。如果不支持，那么它更适合单一任务。支持聊天历史通常需要更好的模型，因此早期针对较差模型的agent类型可能不支持此功能。

###  支持多输入工具（ Supports Multi-Input Tools）

Agent是否支持多输入工具。如果工具只需要单个输入，通常对于LLM来说更容易知道如何调用它。因此，一些早期针对较差模型的agent类型可能不支持此功能。

###  支持并行函数调用（ Supports Parallel Function Calling）

让LLM同时调用多个工具可以极大地加快agent的速度，无论是否有任务需要这样做。然而，对于LLM来说，这样做非常具有挑战性，因此某些agent类型不支持此功能。

###  必要的模型参数（ Required Model Params）

Agent是否需要模型支持任何额外的参数。一些agent类型利用诸如OpenAI函数调用之类的功能，这样做需要其他的模型参数。如果不需要任何参数，那就意味着所有的操作都通过prompt完成。

以下是何时适宜用何种类型agent的总结：

| Agent Type | Intended Model Type | Supports Chat History | Supports Multi-Input Tools | Supports Parallel Function Calling | Required Model Params | When to Use |
| --- | --- | --- | --- | --- | --- | --- |
| Tool Calling | Chat | ✅ | ✅ | ✅ | tools | 如果你正使用 tool-calling模型 |
| OpenAI Tools | Chat | ✅ | ✅ | ✅ | tools | 如果你正在使用最近的OpenAI模型（从1106版本开始）。建议使用通用工具调用代理。 |
| OpenAI Functions | Chat | ✅ | ✅ |  | functions | 如果你正在使用OpenAI模型，或者一个已经经过微调以用于函数调用并暴露了与OpenAI相同的函数参数的开源模型。建议使用通用工具调用代理。 |
| XML | LLM | ✅ |  |  |  | 如果你正在使用 Anthropogenic 模型，或其他擅长处理 XML 的模型。 |
| Structured Chat | Chat | ✅ | ✅ |  |  | 如果你需要支持具有多个输入的工具。 |
| JSON Chat | Chat | ✅ |  |  |  | 如果你正在使用擅长处理 JSON 的模型。 |
| ReAct | LLM | ✅ |  |  |  | 如果你正在使用简单模型。 |
| Self Ask With Search | LLM |  |  |  |  | 如果你正在使用简单模型并且只有一个搜索工具。 |

## Tool calling agent

工具调用允许模型检测何时应调用一个或多个工具，并以应传递给这些工具的输入作出响应。在 API 调用中，你可以描述工具，并让模型智能地选择输出结构化对象，比如包含调用这些工具所需参数的 JSON。工具 API 的目标是比通用文本补全或聊天 API 更可靠地返回有效和有用的工具调用。

我们可以利用这种结构化输出，结合你可以将多个工具绑定到工具调用聊天模型并允许模型选择调用哪个工具的事实，创建agent重复调用工具并接收结果，直到查询得到解决。

这是 OpenAI 工具代理的更通用版本，它被设计用于 OpenAI 特定风格的工具调用。它使用 LangChain 的 ToolCall 接口来支持更广泛的模型实现，除 OpenAI 外，例如 Anthropic、Google Gemini 和 Mistral等模型。

### 配置

任何支持工具调用的模型都可以使用该代理。下面以OpenAI为例，演示tool calling agent的使用，演示中使用了Tavily，你可以替换为其他内置工具或添加自定义工具。使用Tavily之前，需要先注册API key，并将其设置为 process.env.TAVILY_API_KEY 。

**安装依赖**

```
pip install -qU langchain-openai
```

**配置环境变量**

```
import getpass
import os
from langchain_openai import ChatOpenAI

os.environ[&quot;OPENAI_API_KEY&quot;] = getpass.getpass()
llm = ChatOpenAI(model=&quot;gpt-3.5-turbo-0125&quot;)
```

### 工具初始化

创建可以搜索网页的工具

```
from langchain.agents import AgentExecutor, create_tool_calling_agent
from langchain_community.tools.tavily_search import TavilySearchResults
from langchain_core.prompts import ChatPromptTemplate

tools = [TavilySearchResults(max_results=1)]
```

### 创建Agent

```
prompt = ChatPromptTemplate.from_messages(
    [
        (
            &quot;system&quot;,
            &quot;You are a helpful assistant. Make sure to use the tavily_search_results_json tool for information.&quot;,
        ),
        (&quot;placeholder&quot;, &quot;{chat_history}&quot;),
        (&quot;human&quot;, &quot;{input}&quot;),
        (&quot;placeholder&quot;, &quot;{agent_scratchpad}&quot;),
    ]
)

# Construct the Tools agent
agent = create_tool_calling_agent(llm, tools, prompt)
```

### 运行Agent

```
# Create an agent executor by passing in the agent and tools
agent_executor = AgentExecutor(agent=agent, tools=tools, verbose=True)
agent_executor.invoke({&quot;input&quot;: &quot;what is LangChain?&quot;})
```

### 使用聊天历史

这类agent可以选择接收表示以前会话轮次的聊天消息，利用以前的历史进行对话式回应。

```
from langchain_core.messages import AIMessage, HumanMessage

agent_executor.invoke(
    {
        &quot;input&quot;: &quot;what&#x27;s my name? Don&#x27;t use tools to look this up unless you NEED to&quot;,
        &quot;chat_history&quot;: [
            HumanMessage(content=&quot;hi! my name is bob&quot;),
            AIMessage(content=&quot;Hello Bob! How can I assist you today?&quot;),
        ],
    }
)
```

## OpenAI tools

较新的OpenAI模型经过微调可以检测什么时候应该调用一个或多个函数，并用应该传递给该函数的输入进行响应。在API调用中，你可以描述函数并让模型智能地选择输出包含调用这些函数的参数的JSON对象。OpenAI工具API的目标是比通用文本补全或聊天 API 更可靠地返回有效和有用的函数调用。

OpenAI将调用单个函数的能力称为函数，将调用一个或多个函数的能力称作工具。

> 在OpenAI Chat API中，函数现在被认为是一种过时的选择，已经被工具取代。

> 如果你正在使用OpenAI模型创建agent，你应该使用OpenAI Tools agent，而不是OpenAI functions agent。

> 使用工具允许模型在适当时请求调用多个功能。

> 在某些情况下，有助于显著减少agent实现目标所需的时间。

### 安装依赖

```
%pip install --upgrade --quiet  langchain-openai tavily-python

from langchain import hub
from langchain.agents import AgentExecutor, create_openai_tools_agent
from langchain_community.tools.tavily_search import TavilySearchResults
from langchain_openai import ChatOpenAI
```

### 初始化工具

```
tools = [TavilySearchResults(max_results=1)]
```

### 创建Agent

```
# Get the prompt to use - you can modify this!
prompt = hub.pull(&quot;hwchase17/openai-tools-agent&quot;)

# Choose the LLM that will drive the agent
# Only certain models support this
llm = ChatOpenAI(model=&quot;gpt-3.5-turbo-1106&quot;, temperature=0)

# Construct the OpenAI Tools agent
agent = create_openai_tools_agent(llm, tools, prompt)
```

### 运行Agent

```
# Create an agent executor by passing in the agent and tools
agent_executor = AgentExecutor(agent=agent, tools=tools, verbose=True)
agent_executor.invoke({&quot;input&quot;: &quot;what is LangChain?&quot;})
```

### 使用聊天历史

```
from langchain_core.messages import AIMessage, HumanMessage

agent_executor.invoke(
    {
        &quot;input&quot;: &quot;what&#x27;s my name? Don&#x27;t use tools to look this up unless you NEED to&quot;,
        &quot;chat_history&quot;: [
            HumanMessage(content=&quot;hi! my name is bob&quot;),
            AIMessage(content=&quot;Hello Bob! How can I assist you today?&quot;),
        ],
    }
)
```

## OpenAI functions

OpenAI API已经弃用了 functions ，转而使用 tools 。[两者之](https://link.zhihu.com/?target=https%3A//python.langchain.com/docs/modules/agents/agent_types/openai_functions_agent/)间的区别在于， tools API允许模型一次性请求调用多个函数，这在某些架构中可以减少响应时间。建议使用工具代理来调用OpenAI模型。

使用方法与OpenAI tools类似，进一步了解请参阅 这里 。

## XML Agent

有些语言模型（例如Anthropic的Claude）特别擅长推理/编写XML。这里介绍了如何使用在提示时使用XML的agent。

> Tips:

> - 只能与常规的LLMs一起使用，不能与聊天模型一起使用。

> - 只能与非结构化工具一起使用；即只能接受单个字符串输入的工具。

### 安装依赖

```
from langchain import hub
from langchain.agents import AgentExecutor, create_xml_agent
from langchain_anthropic.chat_models import ChatAnthropic
from langchain_community.tools.tavily_search import TavilySearchResults
```

### 初始化工具

```
tools = [TavilySearchResults(max_results=1)]
```

### 创建Agent

将使用langchain内置的create_xml_agent构造器。

```
# Get the prompt to use - you can modify this!
prompt = hub.pull(&quot;hwchase17/xml-agent-convo&quot;)

# Choose the LLM that will drive the agent
llm = ChatAnthropic(model=&quot;claude-2.1&quot;)

# Construct the XML agent
agent = create_xml_agent(llm, tools, prompt)
```

### 运行Agent

```
# Create an agent executor by passing in the agent and tools
agent_executor = AgentExecutor(agent=agent, tools=tools, verbose=True)
agent_executor.invoke({&quot;input&quot;: &quot;what is LangChain?&quot;})
```

### 使用聊天历史

```
from langchain_core.messages import AIMessage, HumanMessage

agent_executor.invoke(
    {
        &quot;input&quot;: &quot;what&#x27;s my name? Only use a tool if needed, otherwise respond with Final Answer&quot;,
        # Notice that chat_history is a string, since this prompt is aimed at LLMs, not chat models
        &quot;chat_history&quot;: &quot;Human: Hi! My name is Bob\nAI: Hello Bob! Nice to meet you&quot;,
    }
)
```

### 自定义XML Agent

这里提供自定义XML agent实现的示例，以便了解 create_xml_agent 内部是如何工作的。

```
from langchain.agents.output_parsers import XMLAgentOutputParser

# Logic for going from intermediate steps to a string to pass into model
# This is pretty tied to the prompt
def convert_intermediate_steps(intermediate_steps):
    log = &quot;&quot;
    for action, observation in intermediate_steps:
        log += (
            f&quot;&lt;tool&gt;{action.tool}&lt;/tool&gt;&lt;tool_input&gt;{action.tool_input}&quot;
            f&quot;&lt;/tool_input&gt;&lt;observation&gt;{observation}&lt;/observation&gt;&quot;
        )
    return log


# Logic for converting tools to string to go in prompt
def convert_tools(tools):
    return &quot;\n&quot;.join([f&quot;{tool.name}: {tool.description}&quot; for tool in tools])
```

构建可运行的agent通常涉及以下几个方面：

1.  中间步骤的数据处理。这些步骤需要以语言模型可以识别的方式表示，应该与prompt中的指令紧密地耦合。

1.  prompt本身。

1.  模型，如果需要的话，要包括停止标记。

1.  输出解析器 - 应该与prompt指定的格式一致。

```
agent = (
    {
        &quot;input&quot;: lambda x: x[&quot;input&quot;],
        &quot;agent_scratchpad&quot;: lambda x: convert_intermediate_steps(
            x[&quot;intermediate_steps&quot;]
        ),
    }
    | prompt.partial(tools=convert_tools(tools))
    | llm.bind(stop=[&quot;&lt;/tool_input&gt;&quot;, &quot;&lt;/final_answer&gt;&quot;])
    | XMLAgentOutputParser()
)

agent_executor = AgentExecutor(agent=agent, tools=tools, verbose=True)
agent_executor.invoke({&quot;input&quot;: &quot;what is LangChain?&quot;})
```

## JSON Chat Agent

有些语言模型在编写JSON方面表现特别出色。该agent使用JSON格式化其输出，以便支持聊天模型。

### 安装依赖

```
from langchain import hub
from langchain.agents import AgentExecutor, create_json_chat_agent
from langchain_community.tools.tavily_search import TavilySearchResults
from langchain_openai import ChatOpenAI
```

### 初始化工具

```
tools = [TavilySearchResults(max_results=1)]
```

### 创建Agent

```
# Get the prompt to use - you can modify this!
prompt = hub.pull(&quot;hwchase17/react-chat-json&quot;)

# Choose the LLM that will drive the agent
llm = ChatOpenAI()

# Construct the JSON agent
agent = create_json_chat_agent(llm, tools, prompt)
```

### 运行Agent

```
# Create an agent executor by passing in the agent and tools
agent_executor = AgentExecutor(
    agent=agent, tools=tools, verbose=True, handle_parsing_errors=True
)
agent_executor.invoke({&quot;input&quot;: &quot;what is LangChain?&quot;})
```

### 使用聊天历史

```
from langchain_core.messages import AIMessage, HumanMessage

agent_executor.invoke(
    {
        &quot;input&quot;: &quot;what&#x27;s my name?&quot;,
        &quot;chat_history&quot;: [
            HumanMessage(content=&quot;hi! my name is bob&quot;),
            AIMessage(content=&quot;Hello Bob! How can I assist you today?&quot;),
        ],
    }
)
```

## Structured chat

结构化聊天agent能够使用多输入工具。

### 安装依赖

```
from langchain import hub
from langchain.agents import AgentExecutor, create_structured_chat_agent
from langchain_community.tools.tavily_search import TavilySearchResults
from langchain_openai import ChatOpenAI
```

### 初始化工具

```
tools = [TavilySearchResults(max_results=1)]
```

### 创建Agent

```
# Get the prompt to use - you can modify this!
prompt = hub.pull(&quot;hwchase17/structured-chat-agent&quot;)

# Choose the LLM that will drive the agent
llm = ChatOpenAI(temperature=0, model=&quot;gpt-3.5-turbo-1106&quot;)

# Construct the JSON agent
agent = create_structured_chat_agent(llm, tools, prompt)
```

### 运行Agent

```
# Create an agent executor by passing in the agent and tools
agent_executor = AgentExecutor(
    agent=agent, tools=tools, verbose=True, handle_parsing_errors=True
)

agent_executor.invoke({&quot;input&quot;: &quot;what is LangChain?&quot;})
```

### 使用聊天历史

```
from langchain_core.messages import AIMessage, HumanMessage

agent_executor.invoke(
    {
        &quot;input&quot;: &quot;what&#x27;s my name? Do not use tools unless you have to&quot;,
        &quot;chat_history&quot;: [
            HumanMessage(content=&quot;hi! my name is bob&quot;),
            AIMessage(content=&quot;Hello Bob! How can I assist you today?&quot;),
        ],
    }
)
```

## ReAct

[本节展示如何](https://link.zhihu.com/?target=https%3A//react-lm.github.io/)使用agent实现 ReAct 逻辑。

### 安装依赖

```
from langchain import hub
from langchain.agents import AgentExecutor, create_react_agent
from langchain_community.tools.tavily_search import TavilySearchResults
from langchain_openai import OpenAI
```

### 初始化工具

```
tools = [TavilySearchResults(max_results=1)]
```

### 创建Agent

```
# Get the prompt to use - you can modify this!
prompt = hub.pull(&quot;hwchase17/react&quot;)

# Choose the LLM to use
llm = OpenAI()

# Construct the ReAct agent
agent = create_react_agent(llm, tools, prompt)
```

### 运行Agent

```
# Create an agent executor by passing in the agent and tools
agent_executor = AgentExecutor(agent=agent, tools=tools, verbose=True)
agent_executor.invoke({&quot;input&quot;: &quot;what is LangChain?&quot;})
```

### 使用聊天历史

```
# Get the prompt to use - you can modify this!
prompt = hub.pull(&quot;hwchase17/react-chat&quot;)

# Construct the ReAct agent
agent = create_react_agent(llm, tools, prompt)
agent_executor = AgentExecutor(agent=agent, tools=tools, verbose=True)

from langchain_core.messages import AIMessage, HumanMessage

agent_executor.invoke(
    {
        &quot;input&quot;: &quot;what&#x27;s my name? Only use a tool if needed, otherwise respond with Final Answer&quot;,
        # Notice that chat_history is a string, since this prompt is aimed at LLMs, not chat models
        &quot;chat_history&quot;: &quot;Human: Hi! My name is Bob\nAI: Hello Bob! Nice to meet you&quot;,
    }
)
```

## Self-ask with search

本节展示如何使用搜索agent实现自问自答。

### 安装依赖

```
from langchain import hub
from langchain.agents import AgentExecutor, create_self_ask_with_search_agent
from langchain_community.llms import Fireworks
from langchain_community.tools.tavily_search import TavilyAnswer
```

### 初始化工具

我们将初始化想要使用的工具，它提供答案（而不是文档）。

该agent只能使用一个工具，并且它的名称必须为“Intermediate Answer”。

```
tools = [TavilyAnswer(max_results=1, name=&quot;Intermediate Answer&quot;)]
```

### 创建Agent

```
# Get the prompt to use - you can modify this!
prompt = hub.pull(&quot;hwchase17/self-ask-with-search&quot;)

# Choose the LLM that will drive the agent
llm = Fireworks()

# Construct the Self Ask With Search Agent
agent = create_self_ask_with_search_agent(llm, tools, prompt)
```

### 运行Agent

```
# Create an agent executor by passing in the agent and tools
agent_executor = AgentExecutor(agent=agent, tools=tools, verbose=True)
agent_executor.invoke(
    {&quot;input&quot;: &quot;What is the hometown of the reigning men&#x27;s U.S. Open champion?&quot;}
)
```

## OpenAI assistants

[Assistants API ](https://link.zhihu.com/?target=https%3A//platform.openai.com/docs/assistants/overview)允许用户在自己的应用中构建AI助手。Assistant具有指令，并可以利用模型、工具和知识来响应用户查询。Assistant API目前支持三种类型的工具：代码解释器（Code Interpreter）、检索（Retrieval）和函数调用（Function calling）。

你可以使用OpenAI工具或自定义工具与OpenAI Assistant进行交互。当仅使用OpenAI工具时，可以直接调用Assistant并获得最终答案。当使用自定义工具时，可以用内置的AgentExecutor或者自定义的执行器运行Assistant和工具执行循环。

下面展示与Assistant进行交互的不同方式。作为一个简单的例子，我们构建一位可以编写和运行代码的数学辅导员。

### 仅使用OpenAI工具

```
from langchain.agents.openai_assistant import OpenAIAssistantRunnable

interpreter_assistant = OpenAIAssistantRunnable.create_assistant(
    name=&quot;langchain assistant&quot;,
    instructions=&quot;You are a personal math tutor. Write and run code to answer math questions.&quot;,
    tools=[{&quot;type&quot;: &quot;code_interpreter&quot;}],
    model=&quot;gpt-4-1106-preview&quot;,
)
output = interpreter_assistant.invoke({&quot;content&quot;: &quot;What&#x27;s 10 - 4 raised to the 2.7&quot;})
```

### 使用任意工具的langchain agent

现在我们用自定义工具重新实现该功能，本例中使用E2B sandbox runtime工具。

```
%pip install --upgrade --quiet  e2b duckduckgo-search

import getpass
from langchain_community.tools import DuckDuckGoSearchRun, E2BDataAnalysisTool

tools = [E2BDataAnalysisTool(api_key=getpass.getpass()), DuckDuckGoSearchRun()]

agent = OpenAIAssistantRunnable.create_assistant(
    name=&quot;langchain assistant e2b tool&quot;,
    instructions=&quot;You are a personal math tutor. Write and run code to answer math questions. You can also search the internet.&quot;,
    tools=tools,
    model=&quot;gpt-4-1106-preview&quot;,
    as_agent=True,
)
```

**使用AgentExecutor**

OpenAIAssistantRunnable与AgentExecutor兼容，因此我们可以将其作为agent直接传递给执行器。AgentExecutor处理调用的工具并将工具输出上传回Assistant API。此外，它还内置了LangSmith跟踪功能。

```
from langchain.agents import AgentExecutor

agent_executor = AgentExecutor(agent=agent, tools=tools)
agent_executor.invoke({&quot;content&quot;: &quot;What&#x27;s the weather in SF today divided by 2.7&quot;})
```

**自定义执行**

使用LCEL我们可以轻松地编写自己的执行循环运行assistant，完全掌控执行动作。

```
agent = OpenAIAssistantRunnable.create_assistant(
    name=&quot;langchain assistant e2b tool&quot;,
    instructions=&quot;You are a personal math tutor. Write and run code to answer math questions.&quot;,
    tools=tools,
    model=&quot;gpt-4-1106-preview&quot;,
    as_agent=True,
)

from langchain_core.agents import AgentFinish


def execute_agent(agent, tools, input):
    tool_map = {tool.name: tool for tool in tools}
    response = agent.invoke(input)
    while not isinstance(response, AgentFinish):
        tool_outputs = []
        for action in response:
            tool_output = tool_map[action.tool].invoke(action.tool_input)
            print(action.tool, action.tool_input, tool_output, end=&quot;\n\n&quot;)
            tool_outputs.append(
                {&quot;output&quot;: tool_output, &quot;tool_call_id&quot;: action.tool_call_id}
            )
        response = agent.invoke(
            {
                &quot;tool_outputs&quot;: tool_outputs,
                &quot;run_id&quot;: action.run_id,
                &quot;thread_id&quot;: action.thread_id,
            }
        )

    return response

response = execute_agent(agent, tools, {&quot;content&quot;: &quot;What&#x27;s 10 - 4 raised to the 2.7&quot;})
print(response.return_values[&quot;output&quot;])
```

### 使用已有线程

要使用已有线程，只需在调用agent时传入“thread_id”。

```
next_response = execute_agent(
    agent,
    tools,
    {&quot;content&quot;: &quot;now add 17.241&quot;, &quot;thread_id&quot;: response.return_values[&quot;thread_id&quot;]},
)
print(next_response.return_values[&quot;output&quot;])
```

### 使用已有Assistant

要使用已有Assistant，直接用assistant_id初始化OpenAIAssistantRunnable。

```
agent = OpenAIAssistantRunnable(assistant_id=&quot;&lt;ASSISTANT_ID&gt;&quot;, as_agent=True)
```

## 参考文档

Types | ️ LangChain

Tool calling agent | ️ LangChain

OpenAI tools | ️ LangChain

OpenAI functions | ️ LangChain

XML Agent | ️ LangChain

JSON Chat Agent | ️ LangChain

Structured chat | ️ LangChain

ReAct | ️ LangChain

Self-ask with search | ️ LangChain

OpenAI assistants | ️ LangChain

发布于 2024-04-29 22:52 ・IP 属地江苏