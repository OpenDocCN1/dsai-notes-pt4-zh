
# 大模型开发框架LangChain

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/IE7TdtrsvokdSMxxvbXcNdo5nag](https://qqs7y1hozd1.feishu.cn/docx/IE7TdtrsvokdSMxxvbXcNdo5nag)

大家好！今天我们将深入探讨LangChain这个强大的框架。在这个人工智能和自然语言处理快速发展的时代，LangChain为我们提供了一个革命性的工具，使我们能够更轻松地构建基于大语言模型的应用。接下来，我们将一起探索LangChain的核心概念、主要组件、实际应用场景，以及它如何改变我们构建AI应用的方式。无论你是AI开发新手，还是经验丰富的工程师，相信这次讲解都会给你带来新的见解和灵感。

## 什么是LangChain

LangChain是一个开源框架，允许从事人工智能的开发者将例如GPT-4的大语言模型与外部计算和数据来源结合起来。该框架目前以Python或JavaScript包的形式提供。LangChain提供了一套完整的工具，包括文本处理、数据处理、模型调用、聊天、工具调用等，大大提速基于大模型应用的开发。

假设，你想开发基于自己的数据、文件（可以是一本书、一个PDF文件、一个包含专有信息的数据库）的问答机器人，又或者你想快速的了解一本小说中的内容，LangChain可以将GPT-4和这些外部数据连接起来。甚至可以让LangChain帮助你采取你想采取的行动，例如发一封邮件。

作为开发者，试想一下，如果我们想开发一款基于GPT大模型的应用，我们会怎么做？我们会很容易想到GPT大模型一定提供了API接口吧，恭喜你答对了。我们作为开发者，对于API的调用一定有很丰富的经验吧，我们组织好请求参数使用HTTP请求OpenAI提供的接口。

作为开发者你一定会继续思考：有没有封装好的类似于SDK这样的开发包来简化开发流程呢？

LangChain就是这样的SDK，LangChain提供了高级抽象和工具，使开发者更容易构建复杂的AI应用。

## 为什么使用LangChain

良好的框架抽象

+ 结构化的输入输出

+ `Chain`（链）的概念 `Chain`（链）是一个非常通用的概念，它指的是将一系列`Module`节点（或其他链）以特定方式组合起来，以实现共同的用例。

+ 丰富的`Module`节点封装 `Module`构成了`Chain`（链）上每一个节点。

繁荣的社区

+ LangChain支持Python和TS语言

+ 丰富的大语言模型支持，包括Open-AI、HuggingFace、Google Vertex等等。

> 开箱即用的逻辑链

+ 大语言模型的API调用

+ 基于文档的摘要、问答

+ 基于SQL的问答

+ 集成OpenAI Function

## LangChain架构

新架构图

![](img/569d98c5351a3a5b485cc2dcf9c418b3.png)

老架构图

![](img/7aa2a65cfe402b393b9da21fd158faee.png)

1. LangChain-Core

LangChain-Core 是 LangChain 框架的核心部分，负责提供基础功能和核心逻辑，包括以下几个主要部分：

+ 模型接口（Model Interfaces）：定义与各种语言模型（如 GPT-4、BERT 等）交互的标准接口。

+ 数据处理（Data Processing）：提供预处理和后处理数据的工具和函数，如文本清理、分词等。

+ 流水线（Pipeline）：允许将多个模型和数据处理步骤串联起来，形成一个完整的处理流程。

+ 工具集成（Tool Integrations）：支持与外部工具和库的集成，如数据库、文件系统等。

2. LangChain-Community

LangChain-Community 是 LangChain 的社区贡献部分，包含来自社区开发者的插件、扩展和示例代码。这个模块旨在鼓励社区参与和共享，提升 LangChain 的功能和可用性。

+ 插件（Plugins）：社区开发的各种插件，可以扩展 LangChain 的功能。

+ 示例项目（Example Projects）：展示如何使用 LangChain 构建不同类型的应用。

+ 文档和教程（Documentation and Tutorials）：提供详细的使用指南和教程，帮助用户快速上手。

3. LangChain

LangChain 是整个框架的主项目，包含核心库和所有相关组件，是用户进行开发和部署的主要入口。它包括以下内容：

+ 完整安装包（Complete Package）：包含所有核心库和组件，提供开箱即用的体验。

+ 集成支持（Integration Support）：支持与其他流行的开发框架和工具（如 Flask、Django）的集成。

+ 示例和模板（Examples and Templates）：提供各种应用场景的示例和模板，帮助用户快速启动项目。

4. LangServe

LangServe 是 LangChain 的服务层组件，负责将语言模型和处理流程部署为可通过 API 访问的服务。它包括：

+ API 网关（API Gateway）：管理和路由 API 请求到相应的处理服务。

+ 服务管理（Service Management）：提供服务的启动、停止、监控和管理功能。

+ 负载均衡（Load Balancing）：确保高并发请求下的服务稳定性和响应速度。

5. LangSmith

LangSmith 是一个用于调试和优化语言模型驱动应用的工具集，提供深入的分析和调试功能，包括：

+ 日志记录（Logging）：详细记录模型的输入、输出和中间状态，帮助开发者追踪和调试问题。

+ 性能分析（Performance Profiling）：分析模型和处理流程的性能瓶颈，提供优化建议。

+ 错误检测（Error Detection）：自动检测并报告模型和流程中的错误和异常情况。

6. LangGraph

+ LangGraph是LangChain的增强版，专注于构建智能体及多智能体流程。其核心优势在于新增的循环流程构建功能以及内嵌的记忆机制，这些特性对于智能体的构建极为重要。

+ LangGraph为开发者提供了高度的控制能力，这在自定义智能体和流程的开发中极为关键。市面上绝大多数的智能体都是针对特定应用场景进行了定制。

+ LangGraph不仅赋予了开发者灵活定制智能体的能力，还提供了直观且易于操作的开发体验。

## LangChain核心模块

![](img/ddf14d44198384f3ffda827f0c79abed.png)

+ `Model/IO` 模块负责与语言模型的交互，包括`Prompts`（提示词）、`Output Parsers`（输出解析器）等模块。

![](img/f4c0f83b3cc13dfb9a688afcebd01709.png)

+ `Chains`

+ 将一系列`Module`节点（或其他`Chain`）以特定方式组合起来，以实现共同的用例。

+ `Memory`

+ `Memory` 模块用于在会话中存储和检索信息，让应用记住和用户对话的上下文。

+ `Agents`

+ `Agents`拓展了调用外部工具的能力，例如用户画一个半径为5cm的圆，大模型理解了用户的意图，它可以使用`Agents`调用外部画图工具画出这个圆。

+ `Callbacks`

+ `Callbacks` 模块用于在`Chain`（链）执行期间捕获和记录信息。类似于Java中的切面。例如大语言模型开始运行、结束运行这样的时机，我们都可以通过`Callbacks`获取回调，做出相应的动作。

## LangChain构建聊天机器人

首先安装Python环境，并安装`langchain`和`langchain-openai`包。

```
pip install langchain -i https://pypi.tuna.tsinghua.edu.cn/simple
pip install langchain-openai -i https://pypi.tuna.tsinghua.edu.cn/simple
```

在Jupyter Notebook中演示。

Jupyter Notebook是基于网页的用于交互计算的应用程序。其可被应用于全过程计算：开发、文档编写、运行代码和展示结果。简而言之，Jupyter Notebook是以网页的形式打开，可以在网页页面中直接编写代码和运行代码，代码的运行结果也会直接在代码块下显示的程序。

### 快速入门

```
from langchain.chains import LLMChain
from langchain_core.prompts import ChatPromptTemplate
from langchain_openai import ChatOpenAI
from langchain_core.messages import AIMessage, HumanMessage, SystemMessage

zhipuai_api_key = "你的智普Api Key"

chat = ChatOpenAI(
    model_name="glm-4",
    openai_api_base="https://open.bigmodel.cn/api/paas/v4",
    openai_api_key=zhipuai_api_key,
    streaming=False,
    verbose=True
)

messages = [
    HumanMessage(content="Hi! I'm Bob"),
]

response = chat.invoke(messages)
print(response)
```

### 消息历史记忆

```
再次提问：
model.invoke([HumanMessage(content="What's my name?")])
```

```
回复：
AIMessage(content="I'm sorry, as an AI assistant, I do not have the capability to know your name unless you provide it to me.", response_metadata={'token_usage': {'completion_tokens': 26, 'prompt_tokens': 12, 'total_tokens': 38}, 'model_name': 'gpt-3.5-turbo', 'system_fingerprint': 'fp_caf95bb1ae', 'finish_reason': 'stop', 'logprobs': None}, id='run-8d8a9d8b-dddb-48f1-b0ed-ce80ce5397d8-0')
```

为了解决这个问题，我们需要将整个对话历史传递到模型中。让我们看看这样做会发生什么：

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

```
AIMessage(content='Your name is Bob.', response_metadata={'token_usage': {'completion_tokens': 5, 'prompt_tokens': 35, 'total_tokens': 40}, 'model_name': 'gpt-3.5-turbo', 'system_fingerprint': 'fp_c2295e73ad', 'finish_reason': 'stop', 'logprobs': None}, id='run-5692718a-5d29-4f84-bad1-a9819a6118f1-0')
```

现在我们可以看到我们得到了一个很好的回复！

## 结语

LangChain为我们提供了一个强大的工具集，使我们能够更容易地构建智能应用。通过理解和运用LangChain的核心概念和组件，我们可以创造出更加智能、灵活和有用的AI系统。随着技术的不断发展，LangChain无疑将在AI应用开发中扮演越来越重要的角色。我鼓励大家继续探索LangChain的更多功能，并在实际项目中尝试应用。记住，实践是学习的最好方式。希望这个讲解能激发你的创意，帮助你开始你的LangChain之旅。如果有任何问题，随时提出。祝大家学习愉快，未来在AI应用开发的道路上取得成功！
