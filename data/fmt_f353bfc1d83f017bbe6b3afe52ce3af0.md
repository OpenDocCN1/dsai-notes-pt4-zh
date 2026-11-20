
# 深度解析-构建 AI Agent全流程

> 来源：[https://v11enp9ok1h.feishu.cn/docx/XRGld84xFoi563xJ6PccUGEQn9c](https://v11enp9ok1h.feishu.cn/docx/XRGld84xFoi563xJ6PccUGEQn9c)

# 背景

前一阶段Manus大火，被宣传为全球首款“真正意义上的通用AI Agent”，其核心能力就是基于LLM的自主任务分解与执行，根据官方测试数据，Manus 在 GAIA 基准测试中表现超越 OpenAI 同类产品，且完成任务的成本更低。

为了让读者对Agent能够有系统的认识，同时也可以对Agent的认识完成一次祛魅的过程。本篇文章结构为：

1. 认识，对AI Agent相关的一些组成进行原理性解释
2. 设计，基于OpenManus构建自己的MyManus，落地和验证认识中的原理
3. 实验，基于MyManus进行验证，对当前的Agent存在的问题有直观的感受。
4. 改进，结合前面的实验以及Agent当下的问题，探讨未来的方向和改进方案。

# 认识

结合Agent相关的几篇关键文章和论文，梳理下基于LLM的AI Agent的设计模式以及一些原理性的概念。ATA上关于LLM和Agent的原理性的优质文章太多，这里主要涉及Manus或OpenManus用到的部分。

## AI Agent正在重新定义软件服务

AI Agent将使软件架构的范式从面向过程迁移到面向目标，就像传统面向过程的工作流和K8S的声明式设计模式一样，

当然这两种解决的问题是不一样的，K8s通过定义期望状态而非具体步骤来管理集群，降低集群状态管理的复杂度，确保集群的稳定性和容错性。

![](img/602c343064f6d60c10c3e7c55fce6b07.png)

我们可以从Manus的官方网站看到，Manus宣称可以做如下的事情，并提供了相当丰富的案例。

+ 数据分析：比如分析店铺销售数据，制定改进策略等。

+ 教育：互动课程，学习资源收集，宇宙探索等

+ 生活：保险策略对比，租赁合同分析，旅行计划等

+ 效率：合同审查，简历筛选，网站SEO优化等。

+ 研究：财报分析，政策研究

## 2.2 AI Agent的核心要素

不同的机构或团队，对Agent的架构模块的划分略有差异，但基本上包含了：

**感知**

**记忆**

**规划**

**行动**

此外，部分团队还涉及定义（管理 Agent 角色特性）、学习（预训练、小样本学习）、认知与意识（思考、整体认知）等特色模块

![](img/838f2edd0bdaa4b5445a6e87e6ebc877.png)

Lilian Weng（翁荔）LLM Powered Autonomous Agents 这篇博客被广泛认为是 “AI Agent领域最权威的结构化综述之一” ，为开发者提供了技术框架参考， 大家应该对文中的图已经很熟悉了

作者将 AI Agent 定义为 “规划 + 记忆 + 工具调用” 的组合，强调LLM作为系统的核心控制器。

+ 规划（Planning）：任务分解、子任务生成及自我反思

+ 记忆（Memory）：长期记忆存储与上下文管理，用于处理复杂任务

+ 工具使用（Tool Usage）：通过API或外部工具增强Agent的能力（如搜索、文件操作，代码执行等）

# 设计

接下来围绕Agent几个核心要素进行设计，会先简单介绍一些基本的原理，然后基于OpenManus做二次设计或者展示其原来的实现。

![](img/d0b6b49bc38026284888047613ad55e0.png)

## LLM

在这之前概括下LLM的一些基本的原理，关于LLM的原理性文章已经很多了，这里只对LLM的推理原理做个简单回顾。

> 当下的LLM的核心依然是基于神经网络，相比传统的神经网络CNN/RNN等，`Transformer`架构通过自注意力机制允许模型直接计算任意两个位置之间的关系，从而更高效捕捉长距离依赖，使得LLM有更强大的多层叠加能力。

> `Transformer`的并行计算能力（如自注意力的矩阵运算）同时加速了训练速度，支持更大的模型规模。

> 另外通过多层非线性激活函数（如 `ReLU`、`GeLU`）可学习更加复杂模式，随着参数规模和训练数据增加，LLM 会突现传统神经网络难以实现的能力（如逻辑推理、代码生成），LLM具备了涌现能力！（也许这就是量变产生了质变吧）

> LLM的推理简单理解为是一个 “模式匹配 + 概率计算” 的过程，通过 `Transformer` 架构和自回归生成，基于海量文本学习到的统计规律，逐步生成符合语言习惯的输出。

> 其效果依赖于训练数据的质量、模型规模以及生成策略的选择。每一步生成都依赖于已生成的上下文，即通过已知的前序词预测下一个词，当前LLM具备上下文学习（In-Context Learning）能力，LLM通过Prompt中提供的示例或指令，无需参数更新即可适应新任务，依然是在于利用模型的模式匹配能力和统计规律，`Transformer` 架构中，Prompt的token位置靠前，权重更高，对后续生成的影响更大。

> LLM的涌现能力使其具备复杂任务处理能力，但同时也因训练数据的噪声和统计生成特性，容易产生幻觉。RAG通过引入外部知识减少幻觉，SFT微调通过高质量数据优化输出，Agent通过工具调用增强交互能力。这些方法可在一定程度上缓解幻觉，增强可解释性，但可解释性仍受限于LLM的黑箱特性。Agent设计中，Prompt是核心组件之一，所有的设计都是围绕构建和管理Prompt，需结合工具调用、记忆管理等模块设计。为适应不同任务，Prompt需精细化设计并管理多个版本。

## Memory

前面了解到，LLM本质上就是一个具有数十亿到千亿级别参数的无状态函数，比如可以这样：

![](img/6bc81efebb4e6c11fe1a17774d0a58fa.png)

LLM自身不具备主动记忆功能，但可通过外部技术（如提示工程、记忆模块）模拟记忆效果，以增强任务表现和交互连贯性。所以别想着LLM会有人类的情感啦，LLM的“连贯性”是概率驱动的，而非真正的意图或情感。

LLM就像一个失忆的绝世高手一样，虽然没有记忆，但可以武功还在，LLM回复完全基于输入的提示（prompt）和训练数据中的模式，通常我们所说的记忆是指的是用户输入提示，分为短期记忆和长期记忆。

+ LLM的“短期记忆”通常依赖于其上下文窗口（最大`Token`限制），通过在输入中包含当前对话或任务的相关信息来维持连贯性。LLM的一些生产参数比如`Temperature`，`Top-P`可以影响其基于上下文生成文本内容。`Temperature`主要是影响生成的确定性，是通过控制`Temperature`实现生成内容的准确性与创造性二者之间的权衡。`Top-P`主要是发散程度，在聚焦和多样性之间的平衡。

+ 长期记忆依赖于外部组件，虽然LLM的预训练参数“存储”了知识，通常已经过时，特定领域的知识，需外部技术（如RAG）补充长期依赖关系。短期记忆也可以转变为长期记忆，这个对Agent的自主进化很有帮助。

我们设计一个简单的短期记忆模块，记录推理和外部感知的结果信息，每次调用LLM时都会带上（受限于LLM的最大`Token`），这样LLM就会在每次推理时“记住”之前的推理过程，以及外部工具感知的结论。相当于：每次你问问题，都要讲一遍前面的交流内容。定义`Memory`类，记录每次的`Message`内容。

```
class Memory(BaseModel):
    messages: List[Message] = Field(default_factory=list)
    max_messages: int = Field(default=100)
```

以下是Memory内容例子

```
{
  "messages": [
  {
                  "role": "user",
                  "content": "\n        CURRENT PLAN STATUS:\n        Plan: QuickSort Implementation Plan (ID: plan_1742277313)\n==========================================================\n\nSteps:\n1. [→] {'step_name': 'Research and understand the QuickSort algorithm', 'status': 'in_progress', 'notes': ''}\n2. ...",
                  "tool_calls": null,
                  "name": null,
                  "tool_call_id": null
              },
              {
                  "role": "user",
                  "content": "You can interact with the computer using PythonExecute, save important content and information files through FileSaver, open browsers with BrowserUseTool, and retrieve information using GoogleSearch.\n\nPythonExecute: ...",
                  "tool_calls": null,
                  "name": null,
                  "tool_call_id": null
              },
              {
                  "role": "assistant",
                  "content": "",
                  "tool_calls": [
                      {
                          "id": "call_294a4ff7396f4c91813990",
                          "type": "function",
                          "function": {
                              "name": "google_search",
                              "arguments": "{\"query\": \"QuickSort algorithm explained\"}"
                          }
                      }
                  ],
                  "name": null,
                  "tool_call_id": null
              },
              {
                  "role": "tool",
                  "content": "Observed output of cmd `google_search` executed:\n['https://www.w3schools.com/dsa/dsa_algo_quicksort.php']...",
                  "tool_calls": null,
                  "name": "google_search",
                  "tool_call_id": "call_294a4ff7396f4c91813990"
              },
              {
                  "role": "user",
                  "content": "You can interact with the computer using PythonExecute...",
                  "tool_calls": null,
                  "name": null,
                  "tool_call_id": null
              },
              {
                  "role": "assistant",
                  "content": "I have researched the QuickSort algorithm using a Google search. Here are some resources that can help us understand how it works...",
                  "tool_calls": null,
                  "name": null,
                  "tool_call_id": null
              }
  ]
}
```

## Tools

人类创造、修改和利用外部物体来做超出人类身体和认知极限的事情，为LLM配备外部工具可以显著扩展模型功能和应用场景。

Agent的工具调用，实际上是借助LLM的能力，LLM需要先理解工具的功能和参数格式。

![](img/cc9846d30a86f61aeacc952375c5db99.png)

上图可以简单概括为：

1. 你有一系列工具，同时有比较详细的说明书，你拿着说明书找LLM问一个LLM自身推理无法（准确）完成的问题，比如：今天几号，天气怎么样，当前数据库内容是什么，这些预训练的LLM是无法感知到的，需要借助外部工具。

1. LLM会从你的工具说明书列表中选工具，准确地告诉你你应该用哪个工具，同时把输入参数也给你（基于说明书描述）

1. 你拿到后调用工具，获取感知信息，然后再把结果信息给到LLM。

1. LLM判断输出结果，并继续回答你之前的问题。

举一个基于`LangChain`的简单例子，加了些HTTP请求埋点，方便跟踪LangChain与LLM的IO交互。
