
# 基于大模型（LLM）的Agent 应用开发

> 来源：[https://agijuejin.feishu.cn/docx/SmhydrERtoU4YJxNC🐧cOaxNnmc](https://agijuejin.feishu.cn/docx/SmhydrERtoU4YJxNC🐧cOaxNnmc)

> 作者：半吊子全栈工匠 | 喔家ArchiSelf

> 原文：https://mp.weixin.qq.com/s/hxFrD4urtc36TPmq88AQ3Q

目前，业界一般认为基于大模型的应用集中在两个方向上：RAG 和 Agent，无论哪一种应用，设计、实现和优化能够充分利用大模型(LLM)潜力的应用都需要大量的努力和专业知识。随着开发人员开始创建日益复杂的LLM应用程序，开发流程不可避免地变得更加复杂。这种流程的潜在设计空间可能是巨大而复杂的，《如何构建基于大模型的App》一文给出了一种探索中的大模型应用开发基础框架，基本可以适用于RAG 和Agent。但是，对于面向Agent的大模型应用开发，有没有其独特之处呢？有没有聚焦于Agent的大模型应用开发框架呢？

那么，什么又是Agent 呢？

## 1. 什么是Agent

这里的Agent 指的是智能体，可以追溯到明斯基的《society of mind》一书。在那本书中，明斯基对Agent的定义有点抽象——“社会中某个个体经过协商后可求得问题的解,这个个体就是agent”。在计算机领域，agent是一种通过传感器感知其环境，并通过执行器作用于该环境的实体，因此，可以把实体定义为一种从感知序列到实体动作的映射。一般认为，Agent是指驻留在某一环境下，能持续自主地发挥作用，具备自主性、反应性、社会性、主动性等特征的计算实体。

智能，是Agent 与环境相互作用的涌现属性。

### 1.1 Agent 的结构与特性

Agent 的一般结构如下图所示：

![](img/eb30869de69e3f54e61282a6c057f6cc.png)

Agent 的主要特性有：

● 自主性（Autonomy） ：运行无需人类或其它 Agent 的直接干预，对其自身行为及内部状态进行某种控制。

● 社会性（Social Ability） 能通过某种 通信与其它 Agent（或人类）进行交互。交互主要有 三种类型：协作（Cooperation）、协调（Coordination）和协商 （Negotiation）。

● 反应性（Reactivity）：能感知环境（可以是物理世界、一个经图形用户接口连接的用户、一系列其 它Agent、Internet 或所有这些的组合），并能对环境的变化及时作出反应。

● 主动性（Pro-activeness）：不但能对环境作出反应，能够积极主动地做出使其目标得以实现的行为。

如果尝试对Agent做一点儿形式化表达，可能是这样的：

`Agent` = `platform`+ `agent program`

`platform` = `computing device` + `sensor`+ `action`

`agent program` 是 `agent function` 的真子集

### 1.2 大模型领域中的Agent

在大模型领域，大模型替代了传统agent 中的规则引擎以及知识库，Agent提供了并寻求推理、观察、批评和验证的对话通道。特别是当配置了正确的提示和推理设置时，单个LLM就可以显示出广泛的功能 ，不同配置的Agent之间的对话可以帮助以模块化并以互补的方式将这些广泛的LLM功能结合起来。

开发人员可以轻松、快速地创建具有不同角色的Agent，例如，使用Agent来编写代码、执行代码、连接人工反馈、验证输出等。通过选择和配置内置功能的子集，Agent的后端也可以很容易地进行扩展，以允许更多的自定义行为。

## 2. 什么是Multi-Agent

Multi-Agent(多智能体系统) 是指由多个自主个体组成的群体系统，其目标是通过个体间的相互信息通信和交互作用。

一般地，Multi-Agent由一系列相互作用的Agent及其相应的组织规则和信息交互协议构成，内部的各个Agent之间通过相互通信、合作、竞争等方式，完成单个Agent不能完成的，大量而又复杂的工作，是“系统的系统”。

### 2.1 Multi-Agent 的系统分类和特点

Multi-Agent 系统（MAS） 主要可以分成以下类别：

![](img/71ff8fe64c2760b32edb73f163482880.png)

Multi-Agent系统的主要具有以下的特点：

1. 自主性。 在Multi-Agent系统中，每个Agent都能管理自身的行为并做到自主 地 合作或者竞争。

2. 容错性。Agent可以共同形成合作的系统用以完成独立或者共同的目标，如果某几个智能体出现了故障，其他智能体将自主地适应新的环境并继续工作，不会使整个系统陷入故障状态。

3. 灵活性和可扩展性。Multi-Agent系统本身采用分布式设计，Agent具有高内聚低耦合的特性，使得系统表现出极强的可扩展性。

4. 协作能力。Multi-Agent系统是分布式系统，Agent之间可以通过合适的策略相互协作完成全局目标。

### 2.2 大模型领域中的Multi-Agent

具体而言， 在基于大模型的应用领域中，当复杂任务被分解成更简单的子任务时，LLM已经被证明了拥有解决复杂任务的能力。Multi-Agent 的通信与协作可以通过“对话”这一直观的方式实现这种子任务的分拆和集成。

为了使基于大模型的Agent适合于Multi-Agent的对话，每个Agent都可以进行对话，它们可以接收、响应和响应消息。当配置正确时 ，Agent可以自动与其他代理进行多次对话，或者在某些对话轮次中请求人工输入，从而通过人工反馈形成RLHF。可对话的Agent设计利用了LLM通过聊天获取反馈并取得进展的强大能力，还允许以模块化的方式组合LLM的功能。

## 3. 基于大模型的常见Agent 和 Multi-Agent 系统

### 3.1 单Agent 系统

基于大模型的常见单Agent 系统包括：

+ `AutoGPT`：`AutoGPT`是一个AI代理的开源实现，它试图自动实现一个给定的目标。它遵循单Agent范式，使用了许多有用的工具来增强AI模型，并且不支持Multi-Agent协作。

+ `ChatGPT+` (`code interpreter` or `plugin`) ：`ChatGPT`是一种会话AI Agent，现在可以与`code interpreter`或插件一起使用。`code interpreter`使`ChatGPT`能够执行代码，而插件通过管理工具增强了`ChatGPT`。

+ `LangChain Agent`：`LangChain`是开发基于LLM应用的通用框架。`LangChain`有各种类型的代理，`ReAct Agent`是其中一个著名的示例。`LangChain`所有代理都遵循单Agent范式，并不是天生为交流和协作模式而设计的。

+ `Transformers Agent`：`Transformers Agent` 是一个建立在`Transformer`存储库上的实验性自然语言API。它包括一组经过策划的工具和一个用来解释自然语言和使用这些工具的Agent。与 `AutoGPT`类似，它遵循单Agent范式，不支持Agent间的协作。

### 3.2 Multi-Agent 系统

基于大模型的常见Multi-Agent 系统包括：

+ `BabyAGI`：`BabyAGI` 是一个用`Python`脚本实现的人工智能任务管理系统的示例。在这个已实现的系统中，使用了多个基于LLM的代理。例如，有一个Agent用于基于上一个任务的目标和结果创建新任务，有一个Agent用于确定任务列表的优先级，还有一个用于完成任务/子任务的Agent。`BabyAGI`作为一个Multi-Agent系统，采用静态Agent对话模式，一个预定义的Agent通信顺序。

+ `CAMEL`：`CAMEL` 是一个agent 通信框架。它演示了如何使用角色扮演来让聊天Agent相互通信以完成任务。它还记录了Agent的对话， 以进行行为分析和能力理解，并采用初始提 示技术来实现代理之间的自主合作。但是，`CAMEL`本身不支持工具的使用，比如代码执行。虽然它被提议作为多代理会话的基础设施，但它只支持静态会话模式。

+ `Multi-Agent Debate`：`Multi-Agent Debate`试图构建具有多代理对话的LLM应用程序，是鼓励LLM中发散思维的有效方式，并改善了LLM的事实性和推理。在这两种工作中 ，多个LLM推理实例被构建为多个Agent来解决与Agent争论的问题。每个Agent都是一个LLM推理实例，而不涉及任何工具或人员，并且Agent间的对话需要遵循预定义的顺序。

+ `MetaGPT`：`MetaGPT` 是一种基于Multi-Agent对话框架的LLM自动软件开发应用程序。他们为各种`gpt`分配不同的角色来协作开发软件，针对特定场景制定专门的解决方案。

在了解了Agent 和 Multi-Agent 的基本概念以及常见系统之后，如何开发一个基于大模型的Agent应用呢？上个月（2023年9月），微软提出了一个`Autogen` 的开源框架，为开发LLM的Agent 应用提供了有价值的参考。

## 4. 基于Multi-Agent的LLM 应用开发框架：`AutoGen`

`AutoGen` 是一个用于简化 LLM 工作流的编排、优化和自动化的开发框架。它提供了可定制和可对话的Agent，利用 LLM 的最强功能，如 `GPT-4`，同时通过与人和工具集成以及通过自动聊天在多个Agent之间进行对话来解决它们的局限性。

### 4.1 `Autogen` 的典型示例

`Autogen` 使用Multi-Agent会话启用复杂的基于 LLM 的工作流，典型的示例如下：

![](img/fb42444ab842304dc4f6564427c8b9a4.png)

左图代表基于`AutoGen`生成的可定制Agent，可以基于LLM、工具、人，甚至它们的组合。右上代表了Agent可以通过对话来解决任务，右下表示`Autogen`支持许多额外的复杂对话模式。

### 4.2 `Autogen` 的 一般用法

使用 `AutoGen`，构建一个复杂的Multi-Agent会话系统可以归结为:

+ 定义一组具有专门功能和角色的Agent。

+ 定义Agent之间的交互行为，例如，当一个代理从另一个代理接收到消息时应该回复什么。

这两个步骤都是模块化的，使这些Agent可重用和可组合。例如，要构建一个基于代码的问答系统，可以设计Agent及其交互，这样的系统可以减少应用程序所需的手动交互次数。一个解决代码中问题的工作流如下图所示：

![](img/3634b4e10614e3ded9a54eb43cfe5d98.png)

`commander`接收用户提出的问题，并与`writer`和`saftguard`协调。 `writer`编写代码并进行解释，`tguard`确保安全，`commander`执行代码。 如果出现问题，该过程可以重复，直到问题得到解决。

## 5. `Autogen`的核心特性：可定制的Agent

`AutoGen` 中的Agent具有由 LLM、人工、工具或这些元素混合启用的功能。例如:

+ 可以通过高级推理特性轻松配置Agent中 LLM 的使用和角色(通过组聊天自动解决复杂任务)。

+ 人工智能和监督可以通过具有不同参与级别和模式的Agent来实现，例如，使用 `GPT-4` + 多个人工用户的自动任务解决。

+ Agent具有对 LLM 驱动代码/函数执行的本机支持，例如，通过代码生成、执行和调试自动解决任务，使用提供的工具作为函数。

### 5.1 助理Agent

使用 `AutoGen` 助理Agent的一种简单方法是调用助理Agent和用户Agent之间的自动聊天，可以很容易地构建一个增强版的 `ChatGPT` + `Code Interpreter` + 插件（如下图所示），该插件具有可定制的自动化能力，可以在定制环境中使用，还可嵌入到更大的系统中。

![](img/ec8947cbf450304719dfca84feb97c06.png)

在上图中，助理Agent扮演人工智能助理的角色，比如必应聊天。用户代理Agent扮演用户的角色，并模拟用户的行为，例如代码执行。`AutoGen` 将两个Agent之间的聊天进行自动化处理，同时允许人工反馈或干预。用户Agent无缝地与人类互动，并在适当的时候使用工具。

### 5.2 Multi-Agent 会话

以Agent会话为中心的设计有许多好处，包括:

+ 自然地处理模糊性、反馈、进展和协作。

+ 启用与编码相关的有效任务，如通过来回故障排除来使用工具。

+ 允许用户通过聊天Agent无缝地选择加入或退出。

+ 通过多位专家的合作实现集体目标。

`AutoGen` 支持自动聊天和多样化的通信模式，使其易于编排复杂、动态的工作流程和实验的多功能性，在下图中，使用了一个称为“ `GroupChatManager`”的特殊Agent来支持多个Agent之间的群组聊天。

![](img/a8592c61d8a22f79ebe16b4b12c01f41.png)

`GroupChatManager`是一个 特殊代理，它重复以下三个步骤: 选择一个演讲者(在本例中为 `Bob`) ，请演讲者做出回应，并将选定演讲者的信息广播给所有其他代理。

总而言之，`AutoGen`被设计为构建 LLM应用程序的通用基础设施。其对话模式几乎支持现有LLM系统的所有模式类型，在“静态”模式下 ，无论输入如何，Agent的拓扑结构都保持不变。`AutoGen`允许灵活的对话模式，包括可以根据不同的应用程序需求进行定制的静态和动态模式。其Multi-Agent系统可以执行LLM生成的代码，允许在系统执行过程中的人员参与。

## 6. `Autogen` 使用示例

`Autogen` 在github上提供了很多有意思的示例，这里以`https://github.com/microsoft/autogen/blob/main/notebook/agentchat human feedback.ipynb` 为例，简单介绍一下如何使用`Autogen` 生成基于Multi-Agent会话的应用实例——代码生成、执行、调试和人工反馈的任务解决。

### 6.1 环境设置

`AutoGen`要求`Python` 版本大于3.8，安装如下：

`pip install pyautogen`

只需几行代码，您就可以快速实现强大的体验：

```
import autogen
config_list = autogen.config_list_from_json("OAI_CONFIG_LIST")
```

`config_list` 的参考文件如下：

```
config_list = [

{

'model': 'gpt-4',

'api_key': '<your OpenAI API key here>',

}, # OpenAI API endpoint for gpt-4

{

'model': 'gpt-4',

'api_key': '<your first Azure OpenAI API key here>',

'api_base': '<your first Azure OpenAI API base here>',

'api_type': 'azure',

'api_version': '2023-06-01-preview',

}, # Azure OpenAI API endpoint for gpt-4

{

'model': 'gpt-4',

'api_key': '<your second Azure OpenAI API key here>',

'api_base': '<your second Azure OpenAI API base here>',

'api_type': 'azure',

'api_version': '2023-06-01-preview',

}, # another Azure OpenAI API endpoint for gpt-4

{

'model': 'gpt-3.5-turbo',

'api_key': '<your OpenAI API key here>',

}, # OpenAI API endpoint for gpt-3.5-turbo

{

'model': 'gpt-3.5-turbo',

'api_key': '<your first Azure OpenAI API key here>',

'api_base': '<your first Azure OpenAI API base here>',

'api_type': 'azure',

'api_version': '2023-06-01-preview',

}, # Azure OpenAI API endpoint for gpt-3.5-turbo

{

'model': 'gpt-3.5-turbo',

'api_key': '<your second Azure OpenAI API key here>',

'api_base': '<your second Azure OpenAI API base here>',

'api_type': 'azure',

'api_version': '2023-06-01-preview',

}, # another Azure OpenAI API endpoint for gpt-3.5-turbo

]

### 6.2 助理Agent 和用户代理Agent 的创建
