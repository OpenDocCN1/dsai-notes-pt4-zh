
# 1小时速通LangChain

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/IYEndSRzVoVFWhx8LBecRzMankd](https://kq4b3vgg5b.feishu.cn/docx/IYEndSRzVoVFWhx8LBecRzMankd)

## LangChain如何接入在线大模型并构建链路

LangChain归属于LangChain AI公司，LangChain作为其中的一个核心项目，开源发布在Gitub上：https://github.com/langchain-ai/langchain

从LangChain的GitHub版本迭代历史上看，从2023年1月16日起已经经历了320个大小版本的迭代，并且仍然以高频率的更新在加速项目的功能上线，从整体上看，其关注度和社区活跃度是非常高的。LangChain给自身的定位是：用于开发由大语言模型支持的应用程序的框架。它的做法是：通过提供标准化且丰富的模块抽象，构建大语言模型的输入输出规范，利用其核心概念`chains`，灵活地连接整个应用开发流程。而针对每个功能模块，都源于对大模型领域的深入理解和实践经验，开发者提供出来的标准化流程和解决方案的抽象，再通过灵活的模块化组合，才有了目前这样一款在大模型应用开发领域内被普遍高度认可的通用框架。

![](img/4cd7c6fac049b55fb0f1c50911c8b9fd.png)

### 为什么需要这样做？

首先，我们需考虑当前大模型的发展态势。尽管OpenAI的GPT系列模型作为大模型领域的领军人物，在很大程度上了影响了大模型的使用规范和基于大模型进行应用开发的范式，但并不意味着所有大模型间的使用方式完全相同。例如，我们熟悉的OpenAI GPT模型API调用方式对于Baichuan2模型就不适用。因此，对于每个新模型都要花费大量时间学习其特定规范再进行应用探索，这种工作效率显然是十分低下的。

其次，必须谈论的是大模型目前面临的局限，如知识更新的滞后性、外部API调用能力、私有数据连接方式以及输出结果的不稳定性等问题。在应用开发中，如何找到这些问题的有效解决策略？

![](img/716a1c1ec7b6c8ba40449edb2a743fa0.png)

上述提到的每个限制都紧密关联于大模型本身的特性。尽管理论上可以通过重新训练、微调来增强模型的原生能力，这种方法确实有效，但实际上，大多数开发者并不具备进行这样操作所需的技术资源、时间和财力，选择这条路径一定会导致方向越来越偏离目标。比如关于大模型的Agents、函数调用等功能，每一步都需大量的研发投入，而且最终实现后的应用效果，也取决于研发人员的个人技术能力。在这种情况下，既然大家都有不同的想法和解决方案，那LangChain就来集中做这件事，提供一个统一的平台和明确的定义，来实现应用框架的快速搭建，这就是LangChain一直想要做到，且正在做的事情。

### LangChain的做法

从本质上分析，LangChain还是依然采用从大模型自身出发的策略，通过开发人员在实践过程中对大模型能力的深入理解及其在不同场景下的涌现潜力，使用模块化的方式进行高级抽象，设计出统一接口以适配各种大模型。到目前为止，LangChain抽象出最重要的核心模块如下：

1. `Model I/O` ：标准化各个大模型的输入和输出，包含输入模版，模型本身和格式化输出；
1. `Retrieval` ：检索外部数据，然后在执行生成步骤时将其传递到 `LLM`，包括文档加载、切割、`Embedding`等；
1. `Chains` ：链条，LangChain框架中最重要的模块，链接多个模块协同构建应用，是实际运作很多功能的高级抽象；
1. `Memory` ：记忆模块，以各种方式构建历史信息，维护有关实体及其关系的信息；
1. `Agents` ：目前最热门的Agents开发实践，未来能够真正实现通用人工智能的落地方案；
1. `Callbacks` ：回调系统，允许连接到 `LLM` 应用程序的各个阶段。用于日志记录、监控、流传输和其他任务；

![](img/9d2410ec1b7aa2da0f5a813e695c9fb6.png)

从上图中可以看到，LangChain框架涵盖了模型输入输出的标准化、外部工具接入的规范、上下文记忆功能，以及对数据库、SQL、CSV等多种数据源的连接标准。通过核心的`"Chain"`高级抽象，定义了不同形式下标准的链接方法，这就能够允许开发者根据实际的应用需求和数据流向快速构建出一套完整的应用程序。这个过程类似于搭建积木，可以灵活适应不同的任务需求。

![](img/fed083beda9db5c4883e279283bc8f14.png)

也正因为如此，LangChain中涉及的概念和模块化是非常多的，每个模块都有其独特的使用场景和使用方法，那么如何去搭这个“积木”，就需要我们对其每个核心模块都要有一个比较清楚的认知。所以我们课程的安排还是逐个拆解LangChain的功能模块，每一部分都尽可能的给大家做详细的介绍和实操，并在接下来的项目部分，进行整体的一个串联，届时大家将能够清晰的明确如何根据自己的实际业务情况，选择合适的构造模块和构造方法。

在整体上理解了LangChain之后，我们首先从`Model I/O`模块进行深入的探讨和实践。

## 1. LangChain核心模块：Model I/O

LangChain的`Model I/O`模块提供了标准的、可扩展的接口实现与大语言模型的外部集成。所谓的`Model I/O`，包括模型输入（`Prompts`）、模型输出（`OutPuts`）和模型本身（`Models`），简单理解就是通过该模块，我们可以快速与某个大模型进行对话交互，整个内部逻辑就相当于我们最熟悉的这个过程：输入Prompt，得到大模型针对该Prompt的推理结果。如下示例为OpenAI的 GPT 系列模型的API 调用规范：

```
response = openai.ChatCompletion.create(
  model="gpt-3.5-turbo",
  messages=[
    {"role": "user", "content": "请问，什么是机器学习？"}
  ]
)
```

而我们前面提到了，LangChain项目的定位为一个应用开发框架，所以如果仅仅集成到这样的对话交互程度，那相较于直接使用OpenAI的API调用又有何异呢？所以在这个模块中，LangChain同样抽象出一个`chain`，用于进一步简化和增强交互流程。在LangChain的Model I/O模块设计中，包含三个核心部分：`Prompt Template`（对应下图中的`Format`部分），`Model`（对应下图中的`Predict`部分）和`Output Parser`（对应下图中的`Parse`部分）。

整个`Model I/O`工作流如下图所示：

### Format：Prompt Template

对于`Prompt Template`第一部分，传统上我们创建提示词是通过手工编写来实现的，在这个过程中会利用各种提示工程技巧，如Few-Shot、链式推理（CoT）等方法，以提高大模型的推理性能。然而，在应用开发中，一个关键的考量是提示词不能是一成不变的。其原因在于，应用开发需要适应多变的用户需求和场景。固定的提示词限制了模型的灵活性和适用范围。例如，如果我们正在开发一个天气查询应用，用户可能会以多种方式提出查询，如“今天的天气怎么样？”或“明天纽约的温度是多少度？”。如果提示词是固定的，它可能只能处理一种特定类型的查询，而无法适应这种多样性的需求。

而`Prompt Template`，就像`ReAct`一样，将API的使用、问题解答过程等复杂逻辑封装成了一套结构化的格式。我们只需准备具体的外部函数信息和用户查询，即可生成定制化的提示词，引导模型按照既定逻辑进行思考和回答，从而实现外部函数的调用过程，即：

```
# 将一个插件的关键信息拼接成一段文本的模版。
TOOL_DESC = """{name_for_model}: Call this tool to interact with the {name_for_human} API. What is the {name_for_human} API useful for? {description_for_model} Parameters: {parameters}"""

# ReAct prompting 的 instruction 模版，将包含插件的详细信息。
PROMPT_REACT = """Answer the following questions as best you can. You have access to the following APIs:

{tool_descs}

Use the following format:

Question: the input question you must answer
Thought: you should always think about what to do
Action: the action to take, should be one of [{tool_names}]
Action Input: the input to the action
Observation: the result of the action
... (this Thought/Action/Action Input/Observation can be repeated zero or more times)
Thought: I now know the final answer
Final Answer: the final answer to the original input question

Begin!

Question: {query}"""
```

因此，引入`Prompt Template`可以支持变量和动态内容的插入，使得同一个应用可以根据不同的输入动态调整提示词，从而更好地响应用户的具体需求。LangChain通过这种方式来提高应用的通用性和用户体验。

### Predict：Models

在`Predict`部分，实质上是处理模型从接收输入到执行推理的整个过程。考虑到存在两种主要类型的大模型——`Base`类模型和`Chat`类模型，LangChain在其`Model I/O`模块中对这两种模型都进行了抽象，分别归类为`LLMs`（`Large Language Models`）和`Chat Models`。我们还是以`OpenAI` 的 `Completion` 和 `Chatcompletions`方法为例：

```
# Base类模型
client.completions.create(
  model="gpt-3.5-turbo-instruct",
  prompt="Say this is a test",
)

# 聊天模型
client.chat.completions.create(
  model="gpt-3.5-turbo",
  messages=[
    {"role": "system", "content": "你是一位乐于助人的AI智能小助手"},
    {"role": "user", "content": "你好，请你介绍一下你自己。"}
  ]
)
```

`LLMs`是简化的大语言模型抽象，即基于给定的Prompt提供内容生成的功能。而`Chat Models`则专注于聊天API的抽象，需要维护上下文的记忆（聊天记录），呈现出更接近对话或聊天形式的交互。

### Parse：Output Parser

我们知道，大模型的输出是不稳定的，同样的输入Prompt往往会得到不同形式的输出。在自然语言交互中，不同的语言表达方式通常不会造成理解上的障碍。但在应用开发中，大模型的输出可能是下一步逻辑处理的关键输入。因此，在这种情况下，规范化输出是必须要做的任务，以确保应用能够顺利进行后续的逻辑处理。

输出解析器 `Output Parser`就是一个帮助结构化语言模型响应的抽象，可以获取格式指令或者进行更深层次的解析。这我们会在后面的实践中直观的体验到。

整体而言，在`Model I/O`模块的抽象中，其一能够让开发者快速的接入不同的大模型，比如`OpenAI`、`ChatGLM`、`Qwen`等，按照既定规范执行模型推理。其二通过输入和输出的模板化处理，使其更贴合于应用开发的最佳实践。接下来，我们就逐步的介绍上述三个流程在LangChain下是如何进行集成和操作的。

## 2. Installing LangChain

> LangChain Docs：https://python.langchain.com/v0.2/docs/how_to/installation/

```
! pip install langchain
```
