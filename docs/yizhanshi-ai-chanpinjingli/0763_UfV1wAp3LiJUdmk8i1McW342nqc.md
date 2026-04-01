# 深度解析-构建 AI Agent全流程

> 来源：[https://v11enp9ok1h.feishu.cn/docx/XRGld84xFoi563xJ6PccUGEQn9c](https://v11enp9ok1h.feishu.cn/docx/XRGld84xFoi563xJ6PccUGEQn9c)

# 背景

前一阶段**Manus**大火，被宣传为全球首款“真正**意义上的通用AI ****A**gent”，其核心能力就是基于LLM的自主任务分解与执行，根据官方测试数据，Manus 在 GAIA 基准测试中表现超越 OpenAI 同类产品，且完成任务的成本更低。

为了让读者对Agent能够有系统的认识，同时也可以对Agent的认识完成一次祛魅的过程。 本篇文章结构为：

1.  **认识**， 对AI Agent相关的一些组成进行原理性解释

1.  **设计**，基于OpenManus构建自己的MyManus，落地和验证认识中的原理

1.  **实验**，基于MyManus进行验证，对当前的Agent存在的问题有直观的感受。

1.  **改进**，结合前面的实验以及Agent当下的问题，探讨未来的方向和改进方案。

# 认识

结合Agent相关的几篇关键文章和论文，梳理下基于LLM的AI Agent的设计模式以及一些原理性的概念。ATA上关于LLM和Agent的原理性的优质文章太多，这里主要涉及Manus或OpenManus用到的部分。

## AI Agent正在重新定义软件服务

AI Agent将使软件架构的范式从面向过程迁移到面向目标，就像传统面向过程的工作流和K8S的声明式设计模式一样，

当然这两种解决的问题是不一样的， K8s通过定义期望状态而非具体步骤来管理集群，降低集群状态管理的复杂度，确保集群的稳定性和容错性。

![](img/602c343064f6d60c10c3e7c55fce6b07.png)

我们可以从Manus的官方网站看到，Manus宣称可以做如下的事情，并提供了相当丰富的案例。

+   数据分析：比如分析店铺销售数据，制定改进策略等。

+   教育：互动课程，学习资源收集，宇宙探索等

+   生活：保险策略对比，租赁合同分析，旅行计划等

+   效率：合同审查，简历筛选，网站SEO优化等。

+   研究：财报分析，政策研究

## 2.2 AI Agent的核心要素

不同的机构或团队， 对Agent的架构模块的划分略有差异，但基本上包含了：

**感知**

**记忆**

**规划**

**行动**

此外，部分团队还涉及定义（管理 Agent 角色特性）、学习（预训练、小样本学习）、认知与意识（思考、整体认知）等特色模块

![](img/838f2edd0bdaa4b5445a6e87e6ebc877.png)

Lilian Weng（翁荔）LLM Powered Autonomous Agents 这篇博客被广泛认为是 “AI Agent领域最权威的结构化综述之一” ，为开发者提供了技术框架参考， 大家应该对文中的图已经很熟悉了

作者将 AI Agent 定义为 “**规划 + 记忆 + 工具调用”** 的组合，强调LLM作为系统的核心控制器。

+   规划（Planning） ：任务分解、子任务生成及自我反思

+   记忆（Memory） ：长期记忆存储与上下文管理，用于处理复杂任务

+   工具使用（Tool Usage） ：通过API或外部工具增强Agent的能力（如搜索、文件操作，代码执行等）

# 设计

接下来围绕Agent几个核心要素进行设计，会先简单介绍一些基本的原理，然后基于OpenManus做二次设计或者展示其原来的实现。

![](img/d0b6b49bc38026284888047613ad55e0.png)

##

## LLM

在这之前概括下LLM的一些基本的原理，关于LLM的原理性文章已经很多了，这里只对LLM的推理原理做个简单回顾。

> 当下的LLM的核心依然是基于神经网络**， 相比传统的神经网络**CNN/RNN等，Transformer架构通过自注意力机制允许模型直接计算任意两个位置之间的关系，从而更高效捕捉长距离依赖，使得LLM有更强大的多层叠加能力。

> **Transformer**的并行计算能力（如自注意力的矩阵运算）同时加速了训练速度，支持更大的模型规模。

> 另外通过多层非线性激活函数（如 ReLU、GeLU）可学习更加**复杂模式， 随着参数规模和训练数据增加，LLM **会突现传统神经网络难以实现的能力（如逻辑推理、代码生成），LLM具备了涌现能力！（ 也许这就是量变产生了质变吧）

> LLM的推理简单理解为是一个 **“模式匹配 + 概率计算” **的过程，通过 **Transformer **架构和自回归生成，基于海量文本学习到的统计规律，逐步生成符合语言习惯的输出。

> 其效果依赖于训练数据的质量、模型规模以及生成策略的选择。 每一步生成都依赖于已生成的上下文，即通过已知的前序词预测下一个词，当前LLM具备上下文学习（In-Context Learning）能力，LLM通过Prompt中提供的示例或指令，无需参数更新即可适应新任务，依然是在于利用模型的模式匹配能力和统计规律，Transformer 架构中，Prompt的token位置靠前，权重更高，对后续生成的影响更大。

> LLM的涌现能力使其具备复杂任务处理能力，但同时也因训练数据的噪声和统计生成特性，容易产生幻觉。RAG通过引入外部知识减少幻觉，SFT微调通过高质量数据优化输出，Agent通过工具调用增强交互能力。这些方法可在一定程度上缓解幻觉，增强可解释性， 但可解释性仍受限于LLM的黑箱特性。Agent设计中，Prompt是核心组件之一，所有的设计都是围绕构建和管理Prompt，需结合工具调用、记忆管理等模块设计。为适应不同任务，Prompt需精细化设计并管理多个版本。

##

## Memory

前面了解到，LLM本质上就是一个具有数十亿到千亿级别参数的无状态函数， 比如可以这样：

![](img/6bc81efebb4e6c11fe1a17774d0a58fa.png)

**LLM自身不具备主动记忆功能，但可通过外部**技术（如提示工程、记忆模块）模拟记忆效果，以增强任务表现和交互连贯性。所以别想着LLM会有人类的情感啦，LLM的“连贯性”是概率驱动的，而非真正的意图或情感。

LLM就像一个失忆的绝世高手一样，虽然没有记忆，但可以武功还在， LLM回复完全基于输入的提示（prompt）和训练数据中的模式，通常我们所说的记忆是指的是用户输入提示， 分为短期记忆和长期记忆。

+   LLM的“短期记忆”通常依赖于其上下文窗口（最大Token限制），通过在输入中包含当前对话或任务的相关信息来维持连贯性。LLM的一些生产参数比如Temperature，Top-P可以影响其基于上下文生成文本内容。 Temperature主要是影响生成的确定性， 是通过控制Temperature实现生成内容的准确性与创造性二者之间的权衡。Top-P主要是发散程度，在聚焦和多样性之间的平衡。

>

+   长期记忆依赖于外部组件，虽然LLM的预训练参数“存储”了知识，通常已经过时，特定领域的知识，需外部技术（如RAG）补充长期依赖关系。短期记忆也可以转变为长期记忆， 这个对Agent的自主进化很有帮助。

我们**设计**一个简单的短期记忆模块， 记录推理和外部感知的结果信息， 每次调用LLM时都会带上（受限于LLM的最大Token）, 这样LLM就会在每次推理时“记住”之前的推理过程，以及外部工具感知的结论。相当于：每次你问问题，都要讲一遍前面的交流内容。 定义Memory类，记录每次的Message内容。

```
class Memory(BaseModel):
    messages: List[Message] = Field(default_factory=list)
    max_messages: int = Field(default=100)
```

**以下是Memory内容例子**

```
{
  &quot;messages&quot;: [
  {
                  &quot;role&quot;: &quot;user&quot;,
                  &quot;content&quot;: &quot;\n        CURRENT PLAN STATUS:\n        Plan: QuickSort Implementation Plan (ID: plan_1742277313)\n==========================================================\n\nSteps:\n1. [→] {&#x27;step_name&#x27;: &#x27;Research and understand the QuickSort algorithm&#x27;, &#x27;status&#x27;: &#x27;in_progress&#x27;, &#x27;notes&#x27;: &#x27;&#x27;}\n2. ...&quot;,
                  &quot;tool_calls&quot;: null,
                  &quot;name&quot;: null,
                  &quot;tool_call_id&quot;: null
              },
              {
                  &quot;role&quot;: &quot;user&quot;,
                  &quot;content&quot;: &quot;You can interact with the computer using PythonExecute, save important content and information files through FileSaver, open browsers with BrowserUseTool, and retrieve information using GoogleSearch.\n\nPythonExecute: ...&quot;,
                  &quot;tool_calls&quot;: null,
                  &quot;name&quot;: null,
                  &quot;tool_call_id&quot;: null
              },
              {
                  &quot;role&quot;: &quot;assistant&quot;,
                  &quot;content&quot;: &quot;&quot;,
                  &quot;tool_calls&quot;: [
                      {
                          &quot;id&quot;: &quot;call_294a4ff7396f4c91813990&quot;,
                          &quot;type&quot;: &quot;function&quot;,
                          &quot;function&quot;: {
                              &quot;name&quot;: &quot;google_search&quot;,
                              &quot;arguments&quot;: &quot;{\&quot;query\&quot;: \&quot;QuickSort algorithm explained\&quot;}&quot;
                          }
                      }
                  ],
                  &quot;name&quot;: null,
                  &quot;tool_call_id&quot;: null
              },
              {
                  &quot;role&quot;: &quot;tool&quot;,
                  &quot;content&quot;: &quot;Observed output of cmd `google_search` executed:\n[&#x27;https://www.w3schools.com/dsa/dsa_algo_quicksort.php&#x27;]...&quot;,
                  &quot;tool_calls&quot;: null,
                  &quot;name&quot;: &quot;google_search&quot;,
                  &quot;tool_call_id&quot;: &quot;call_294a4ff7396f4c91813990&quot;
              },
              {
                  &quot;role&quot;: &quot;user&quot;,
                  &quot;content&quot;: &quot;You can interact with the computer using PythonExecute...&quot;,
                  &quot;tool_calls&quot;: null,
                  &quot;name&quot;: null,
                  &quot;tool_call_id&quot;: null
              },
              {
                  &quot;role&quot;: &quot;assistant&quot;,
                  &quot;content&quot;: &quot;I have researched the QuickSort algorithm using a Google search. Here are some resources that can help us understand how it works...&quot;,
                  &quot;tool_calls&quot;: null,
                  &quot;name&quot;: null,
                  &quot;tool_call_id&quot;: null
              }
  ]
}
```

##

## Tools

人类创造、修改和利用外部物体来做超出人类身体和认知极限的事情，为 LLM 配备外部工具可以显著扩展模型功能和应用场景。

Agent的工具调用，实际上是借助LLM的能力， LLM需要先理解工具的功能和参数格式。

![](img/cc9846d30a86f61aeacc952375c5db99.png)

**上图可以简单概括为：**

1.  你有一系列工具，同时有比较详细的说明书， 你拿着说明书找LLM问一个LLM自身推理无法（准确）完成的问题， 比如：今天几号，天气怎么样， 当前数据库内容是什么， 这些预训练的LLM是无法感知到的， 需要借助外部工具。

>

1.  LLM会从你的工具说明书列表中选工具，准确地告诉你你应该用哪个工具，同时把输入参数也给你（基于说明书描述）

>

1.  你拿到后调用工具，获取感知信息， 然后再把结果信息给到LLM。

>

1.  LLM判断输出结果，并继续回答你之前的问题。

举一个基于**LangChain**的简单例子， 加了些HTTP请求埋点， 方便跟踪LangChain与LLM的IO交互。

```

import os
import httpx
from dotenv import load_dotenv
load_dotenv(&#x27;../.env.production&#x27;)
os.environ[&quot;OPENAI_API_KEY&quot;] = os.getenv(&quot;DASHSCOPE_API_KEY&quot;)
os.environ[&quot;OPENAI_API_BASE&quot;] = os.getenv(&quot;DASHSCOPE_API_BASE_URL&quot;)
print(os.getenv(&quot;DASHSCOPE_API_BASE_URL&quot;))
import json
import logging

from langchain_core.tools import tool
from langgraph.prebuilt import create_react_agent

from langchain_openai import ChatOpenAI
logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(&quot;httpx&quot;)
logger.setLevel(logging.INFO)

@tool
def string_length(text: str) -&gt; int:
    &quot;&quot;&quot;Calculate text length&quot;&quot;&quot;
    return len(text)

def print_request(request, *args, **kwargs):
    logger.info(f&quot;HTTP Request: {request.method} {request.url}&quot;)
    logger.info(f&quot;Request Headers: {request.headers}&quot;)
    logger.info(json.dumps(json.loads(request.content),indent=1))


def print_response(response, *args, **kwargs):
    logger.info(f&quot;HTTP Response: {response.status_code} {response.reason_phrase}&quot;)
    logger.info(f&quot;Response Headers: {response.headers}&quot;)
    response.read()
    logger.info(json.dumps(json.loads(response.text),indent=1))


custom_client = httpx.Client(
    base_url=os.getenv(&quot;DASHSCOPE_API_BASE_URL&quot;),
    headers={&quot;X-Custom-Header&quot;: &quot;value&quot;},
    event_hooks={
        &quot;request&quot;: [print_request],
        &quot;response&quot;: [print_response]
    }
)


llm = ChatOpenAI(model_name=&quot;qwen-max-latest&quot;, verbose=True, http_client=custom_client )

agent = create_react_agent(llm, [string_length],)

response = agent.invoke({&quot;messages&quot;: [(&quot;human&quot;, &quot;How long is &#x27;Hello World&#x27;&quot;)]},config={&quot;callbacks&quot;: []})

# 打印输出
for message in response[&quot;messages&quot;]:
    message.pretty_print()

```

```

# Step1 带着工具说明，问LLM
{
&quot;messages&quot;: [
  {
   &quot;content&quot;: &quot;How long is &#x27;Hello World&#x27;&quot;,
   &quot;role&quot;: &quot;user&quot;
  }
 ],
&quot;model&quot;: &quot;qwen-max-latest&quot;,
&quot;stream&quot;: false,
&quot;tools&quot;: [
  {
   &quot;type&quot;: &quot;function&quot;,
   &quot;function&quot;: {
    &quot;name&quot;: &quot;string_length&quot;,
    &quot;description&quot;: &quot;Calculate text length&quot;,
    &quot;parameters&quot;: {
     &quot;properties&quot;: {
      &quot;text&quot;: {
       &quot;type&quot;: &quot;string&quot;
      }
     },
     &quot;required&quot;: [
      &quot;text&quot;
     ],
     &quot;type&quot;: &quot;object&quot;
    }
   }
  }
 ]
}
# Step2 LLM根据用户请求和工具信息给出工具调用指令，包含参数
{
&quot;choices&quot;: [
  {
   &quot;message&quot;: {
    &quot;content&quot;: &quot;&quot;,
    &quot;role&quot;: &quot;assistant&quot;,
    &quot;tool_calls&quot;: [
     {
      &quot;function&quot;: {
       &quot;name&quot;: &quot;string_length&quot;,
       &quot;arguments&quot;: &quot;{\&quot;text\&quot;: \&quot;Hello World\&quot;}&quot;
      },
      &quot;index&quot;: 0,
      &quot;id&quot;: &quot;call_816c14bce4124280b61eb3&quot;,
      &quot;type&quot;: &quot;function&quot;
     }
    ]
   },
   &quot;finish_reason&quot;: &quot;tool_calls&quot;,
   &quot;index&quot;: 0,
   &quot;logprobs&quot;: null
  }
 ],
&quot;object&quot;: &quot;chat.completion&quot;,
&quot;usage&quot;: {
  &quot;prompt_tokens&quot;: 162,
  &quot;completion_tokens&quot;: 18,
  &quot;total_tokens&quot;: 180
 },
&quot;created&quot;: 1742270364,
&quot;system_fingerprint&quot;: null,
&quot;model&quot;: &quot;qwen-max-latest&quot;,
&quot;id&quot;: &quot;chatcmpl-18a82215-d19c-92ff-8bb1-7d0684f4abac&quot;
}
# Step3: Agent本地调用工具，并把结果给LLM
&quot;messages&quot;: [
  {
   &quot;content&quot;: &quot;How long is &#x27;Hello World&#x27;&quot;,
   &quot;role&quot;: &quot;user&quot;
  },
  {
   &quot;content&quot;: null,
   &quot;role&quot;: &quot;assistant&quot;,
   &quot;tool_calls&quot;: [
    {
     &quot;type&quot;: &quot;function&quot;,
     &quot;id&quot;: &quot;call_816c14bce4124280b61eb3&quot;,
     &quot;function&quot;: {
      &quot;name&quot;: &quot;string_length&quot;,
      &quot;arguments&quot;: &quot;{\&quot;text\&quot;: \&quot;Hello World\&quot;}&quot;
     }
    }
   ]
  },
  {
   &quot;content&quot;: &quot;11&quot;,
   &quot;role&quot;: &quot;tool&quot;,
   &quot;tool_call_id&quot;: &quot;call_816c14bce4124280b61eb3&quot;
  }
 ],
&quot;model&quot;: &quot;qwen-max-latest&quot;,
&quot;stream&quot;: false,
&quot;tools&quot;: [
  {
   &quot;type&quot;: &quot;function&quot;,
   &quot;function&quot;: {
    &quot;name&quot;: &quot;string_length&quot;,
    &quot;description&quot;: &quot;Calculate text length&quot;,
    &quot;parameters&quot;: {
     &quot;properties&quot;: {
      &quot;text&quot;: {
       &quot;type&quot;: &quot;string&quot;
      }
     },
     &quot;required&quot;: [
      &quot;text&quot;
     ],
     &quot;type&quot;: &quot;object&quot;
    }
   }
  }
 ]
}
# Step 4 LLM返回最终结果
&quot;choices&quot;: [
  {
   &quot;message&quot;: {
    &quot;content&quot;: &quot;The length of the text &#x27;Hello World&#x27; is 11 characters.&quot;,
    &quot;role&quot;: &quot;assistant&quot;
   },
   &quot;finish_reason&quot;: &quot;stop&quot;,
   &quot;index&quot;: 0,
   &quot;logprobs&quot;: null
  }
 ],
&quot;object&quot;: &quot;chat.completion&quot;,
&quot;usage&quot;: {
  &quot;prompt_tokens&quot;: 191,
  &quot;completion_tokens&quot;: 16,
  &quot;total_tokens&quot;: 207
 },
&quot;created&quot;: 1742270365,
&quot;system_fingerprint&quot;: null,
&quot;model&quot;: &quot;qwen-max-latest&quot;,
&quot;id&quot;: &quot;chatcmpl-1452e622-cf30-9cf8-9971-4530065952f6&quot;
}
```

##  Planning

Lilian Weng博文里Planning包含四个子模块

**Reflection（反思）**

Self-critics（自我批评）

Chain of thoughts（思维链）

Subgoal decomposition

+   Planning将任务分解为可执行的步骤，并根据反馈不断优化计划。例如，通过Subgoal decomposition将大任务拆解为小任务，通过Reflection评估结果并调整策略。

>

+   我们可以通过 Reason + Act（ReAct）和Plan-and-Solve（PS）来初步实现Agent的Planning能力。还有一些其他的模式。

>

+   比如ToT 模式（ 提示词：“生成3种不同预算方案，总预算不超过10万元，并列出每个方案的子任务”）， 基于角色的多Agent（MetaGPT）

Reason + Act 推理-行动-反馈

ReAct通过LLM交替执行推理和行动，增强了推理和外界感知（通过行动获得）之间的协同作用：推理帮助模型推断、跟踪和更新行动计划，以及处理异常，而行动则允许它与外部资源（如知识库或环境）交互，以获取额外信息。

![](img/f31ec53527cd6bef19bc6ff16110e097.png)

设计ReActAgent流程， 如下图，实现重复的Thought- Action-Observation

+   退出机制有2种方式，一种是当超过最大步数， 另一是当LLM提示要调用一个终止工具时，退出循环。

>

+   终止工具描述为：When you have finished all the tasks, call this tool to end the work。

![](img/5b9ae8a5824c629ce68a86ed178d554f.png)

设计ReAct抽象类，由主体的Agent继承， 包含llm和memory，同时有think和act两个阶段的操作，think通过调用LLM进行推理， act主要是调用工具获取额外的信息。 通过step串联， step由Agent重复调用，每次都会通过LLM的推理判断是否需要调用工具。

```

class ReActAgent(BaseModel, ABC):
    name: str = Field(..., description=&quot;Unique name of the agent&quot;)
    description: Optional[str] = Field(None, description=&quot;Optional agent description&quot;)
    # Prompts
    system_prompt: Optional[str] = Field(
        None, description=&quot;System-level instruction prompt&quot;
    )
    next_step_prompt: Optional[str] = Field(
        None, description=&quot;Prompt for determining next action&quot;
    )
    # Dependencies
    llm: LLM = Field(default_factory=LLM, description=&quot;Language model instance&quot;)
    memory: Memory = Field(default_factory=Memory, description=&quot;Agent&#x27;s memory store&quot;)

    @abstractmethod
    async def think(self) -&gt; bool:
        &quot;&quot;&quot;Process current state and decide next action&quot;&quot;&quot;

    @abstractmethod
    async def act(self) -&gt; str:
        &quot;&quot;&quot;Execute decided actions&quot;&quot;&quot;

    async def step(self) -&gt; str:
        &quot;&quot;&quot;Execute a single step: think and act.
            Thought: ...
            Action: ...
            Observation: ...
            ... (Repeated many times)
        &quot;&quot;&quot;
        should_act = await self.think()
        # 判断是否使用工具
        ifnot should_act:
            return&quot;Thinking complete - no action needed&quot;
        return await self.act()
```

### **Plan-and-Solve**

> Chain-of-Thought（CoT）使LLMs能够明确地生成推理步骤并提高它们在推理任务中的准确性。比如Zero-short-CoT将目标问题陈述结合"让我们逐步思考"这样的提示词， 作为LLM的输入。这是一种隐式生成推理链，具有一定的模糊性，存在语义理解错误， 无法生成合理的计划等问题

+   **P**lan-and-Solve是一种改进的CoT提示方法 ，通过显式分解任务为子目标并分步执行，提升复杂任务的推理能力。

+   Plan-and-Solve将传统的“黑箱”推理操作，**进一步转化为**结构化流程，使得Agent的可解释性增强， 而且还可以动态调整和反思。 是Agent实现Planning能力的很好的技术支撑。

![](img/e891121122cf6c07e6e76cded896f318.png)

参考Plan-and-Solve论文，设计一个Planner

![](img/b63f5ceb266e9a1164748b7c3d3c55cc.png)

##

## Agent

> **我们完成了Agent的Memory**，Tools，Planning的核心模块，这里还差一个LLM模型调用， 目前各个云厂商或者模型部署工具Ollama，vLLM之类的都兼容OpenAI的API接口， 某种程度上算是统一了~。

现在需要把这几个组装起来。文章开头提到了， OpenManus是个好的学习项目， 所以整体实现也是基于（抄）OpenManus实现的，暂且命名为MyManus！为了便于加深理解，实验以及后续的扩展， 做了如下改动。

+   简化了Tools，ReActAgent，Planning之间的依赖关系，但完全兼容OpenManus的工具。

>

+   进一步简化了Planning实现， 去掉了多个Plan的调度逻辑， 暂时去掉了多Agent支持，多Agent后续还会加进来。OpenManus的Agent机制比较简单， 就如下图描述，由Planning根据每一步计划的类型选择用哪个Agent，当前代码里面目前只有一个主Agent，暂时无用。 （最新的OpenManus把PlanningAgent去掉了， Planning放到了flow里面）

>

+   新增EventListenerManager，用于跟踪几个核心模块之间的协同事件，同时优化了其WebUI的文字展示~

>

+   新增两个数据开发相关的工具

+   DatabaseTool 可以做数据库相关的执行和查询操作， 为了验证Agent的问数效果。

+   Jupyter Notebook Client 可以使得Agent远程执行Python代码和指令（比如安装依赖的Python包）， 另外后续可以通过Jupyter Notebook管理敏感密钥信息。

**以下为整体的设计**

![](img/acbea61fa3f6cd83096a0e125063c80d.png)

## 3.6 Prompt

> 到这里， 你会发现前面的Memory，Tools，Planning**每一部分，其**实都是在解决最核心的问题：如何在正确阶段构建最佳的Prompt，印证了LLM部分描述提示词的重要性。 所以Prompt非常重要， 如果你发现Agent效果不好， 可以先从优化提示词开始。

系统的提示词，某种程度上决定了LLM对Agent的特质和能力判断

+   比如借助LLM生成计划，如果没有设计系统的提示词，LLM根据用户的输入，会生成完全无关的计划，思考的域越大，效果会越差。 Openmanus里面的计划系统提示词，就很明确地告知LLM的如何执行计划，如何使用工具等。

每一步的提示词，决定LLM如何使用工具

+   比如我希望LLM优先使用RemoteJupyterClient，可以重点强调其作用，尤其是加了You Can also use pip install packages when package not found 这一句，LLM可以生成pip命令，自动安装包。

**Planning**

```
PLANNING_SYSTEM_PROMPT = &quot;&quot;&quot;
You are an expert Planning Agent tasked with solving problems efficiently through structured plans.
Your job is:
1. Analyze requests to understand the task scope
2. Create a clear, actionable plan that makes meaningful progress with the `planning` tool
3. Execute steps using available tools as needed
4. Track progress and adapt plans when necessary
5. Use `finish` to conclude immediately when the task is complete


Available tools will vary by task but may include:
- `planning`: Create, update, and track plans (commands: create, update, mark_step, etc.)
- `finish`: End the task when complete
Break tasks into logical steps with clear outcomes. Avoid excessive detail or sub-steps.
Think about dependencies and verification methods.
Know when to conclude - don&#x27;t continue thinking once objectives are met.
&quot;&quot;&quot;

```

**ManusAgent**

```
SYSTEM_PROMPT = &quot;You are OpenManus, an all-capable AI assistant, aimed at solving any task presented by the user. You have various tools at your disposal that you can call upon to efficiently complete complex requests. Whether it&#x27;s programming, information retrieval, file processing, or web browsing, you can handle it all.&quot;

NEXT_STEP_PROMPT = &quot;&quot;&quot;You can interact with the computer using PythonExecute, save important content and information files through FileSaver, open browsers with BrowserUseTool, execute python code with RemoteJupyterClient and retrieve information using GoogleSearch.

PythonExecute: Execute Python code to interact with the computer system, when RemoteJupyterClient not work, try use this.

RemoteJupyterClient: Execute Python code on a remote Jupyter Server to data processing, automation tasks, etc. You Can also use pip install packages when package not found.

FileSaver: Save files locally, such as txt, py, html, etc.

BrowserUseTool: Open, browse, and use web browsers.If you open a local HTML file, you must provide the absolute path to the file.

GoogleSearch: Perform web information retrieval

Terminate: End the current interaction when the task is complete or when you can do nothing.

Based on user needs, proactively select the most appropriate tool or combination of tools. For complex tasks, you can break down the problem and use different tools step by step to solve it. After using each tool, clearly explain the execution results and suggest the next steps.

Always maintain a helpful, informative tone throughout the interaction. If you encounter any limitations or need more details, clearly communicate this to the user before terminating.
&quot;&quot;&quot;

```

**tools**

```

SYSTEM_PROMPT = &quot;You are an agent that can execute tool calls&quot;

NEXT_STEP_PROMPT = (
    &quot;If you want to stop interaction, use `terminate` tool/function call.&quot;
)

```

#

# 实验

> 好了， 终于到实验环节了， 为了验证前面的实现，做**了以下几个场景的实验。 经过我**精心雕花，我们可以借助WebUI来观察效果，Planning和ReAct Agent用的LLM都是：qwen-max-latest

+   实验一，写一个QuickSort（无计划）：生成很快，前面直接给出结果，并希望获得下一步指示，但是我们并没有给Agent其他额外指令（LLM很热情，但是感受不到你需要它） 最后LLM只选择JupyterClient验证了代码正确性。

![](img/5e667aa123a9cbfb79ca71867bdcad30.png)

+   实验二，写一个QuickSort（有计划）：Planning将该任务拆分成9个步骤，用时6分钟， 综合运用了Google Serach, PythonExecutor，JupyterClient，FileSave多个工具，并生成了代码和性能测试报告，可解释性增强许多。

![](img/6d5275d0c4875f68e15f7daefbf07059.png)

![](img/52e4b4463695fc6a3d3f0cd71086eb0b.png)

+   **实验**三，Which sales agent made the most in sales in 2009 ? Answer based on database data. （无计划）：感到惊讶， LLM自己做了规划，查看表结构， 最后的结果也正确。

![](img/df876f712fe168dc9f8ede687e33093c.png)

![](img/51b67801651f05cee04ebf8ec4c55c14.png)

+   实验四，最近7天杭州天气怎么样，请画出相关图表（有计划）：这一段最为精彩

+   1、LLM尝试使用工具爬取结构化的天气数据，但是失败了。 调用了Terminate工具结束任务。

+   2、但是ReAct Agent并没有明确失败是失败， Planning继续执行下一步，LLM这时候出现了幻觉， 从历史的数据获取数据。

+   3、拿到假的数据后， LLM充分利用了JupyterClient工具，编写代码，远程运行代码，遇到包不存在，使用PIP安装包！这其实是在创造新工具。

![](img/e67b3aac3e135dceb7e26ddddf3d5d27.png)

![](img/15ddd297267fe3fdf75aa3724a004d64.png)

```
# Action
🚀 Manus is about to execute 1 tools: [ChatCompletionMessageToolCall(id=&#x27;call_6284438a03d74103b16fc7&#x27;, function=Function(arguments=&#x27;{&quot;code&quot;:&quot;import sys\\n!{sys.executable} -m pip install matplotlib&quot;}&#x27;, name=&#x27;remote_jupyter&#x27;), type=&#x27;function&#x27;, index=0)]
# Using Tool
🔧 Activating tool: &#x27;remote_jupyter&#x27; args:{&#x27;code&#x27;: &#x27;import sys\n!{sys.executable} -m pip install matplotlib&#x27;}
```

```
# Action
🚀 Manus is about to execute 1 tools: [ChatCompletionMessageToolCall(id=&#x27;call_6284438a03d74103b16fc7&#x27;, function=Function(arguments=&#x27;{&quot;code&quot;:&quot;import sys\\n!{sys.executable} -m pip install matplotlib&quot;}&#x27;, name=&#x27;remote_jupyter&#x27;), type=&#x27;function&#x27;, index=0)]
# Using Tool
🔧 Activating tool: &#x27;remote_jupyter&#x27; args:{&#x27;code&#x27;: &#x27;import sys\n!{sys.executable} -m pip install matplotlib&#x27;}
```

单纯的ReAct模式的Agent对LLM的依赖较高， 执行效率也高，产生的结果没有过多修饰。

+   使用Plan and Solve模式下，可解释性提高, 但有时生成的计划赶不上变化和实际不符合，导致ReAct会在一个计划步骤内完成所有事情。

+   天气的实验，Agent对工具的能力依赖较高，需要考虑"兜底的逻辑"，防止LLM出现幻觉。

+   复杂的任务会上下文输入会超过LLM的限制，导致任务无法进行下去。

+   并不能稳定地产生正确的结果，有时生成的代码或者SQL能正确执行，但结论是错误的。

# 改进

## 自主化演进

+   **自我批判：**实验中可以看到， 通过LLM的规划能力，能够使得Agent更好的完成复杂的任务，在任务规划方面从基于规则、参数的规划能力逐步向基于环境的感知，实践的反思、迭代进化。

+   ReActAgent具备一定的感知，反思，迭代能力，但是无法完成复杂的任务，可解释性比PS要低。 当前我们Planning有两个问题，1是依据较少，LLM容易出现幻觉，导致步骤较多， 执行计划前需要更多信息。 2是计划执行过程中，无法动态调整， 需要能够动态改进计划。

+   **代码即为工具：**在工具使用与选择方面，向多种工具的选择规划进化，甚至更进一步地创造适用于 LLM 的工具。

+   LLM创造工具，依赖于其编码能力，比如Manus团队在描述是否使用MCP时，提到受到CodeAct启发，将代码执行视为解决问题的工具而非目标，正所谓“太极生两仪。两仪生四象，四象生八卦”。

![](img/5d4e3dd19dbb94a3b346e2ab98db5f30.png)

+   **自我成长：**如何让Agent根据之前的经验（记忆）改进行为呢？

+   **上下文长度限制和注意力问题：**ReAct Agent依靠周而复始的Thought- Action-Observation不断迭代，得到最终的结果，Agent会记录每次的结果，按时间序列组织记忆，随着步数的增多，每次给LLM的上下文会变得很长， 比如Qwen-Max 有32K的上下文长度，多轮对话或者复杂的问题处理，很容易超出LLM的上下文长度限制。而且如果每次给LLM的上下文都很大，LLM推理的性能相应的也会变慢。除了慢以外，也会影响LLM的注意力，LLM并不能稳定的利用长输入上下文信息， LLM比较容易“记住”开头和结尾的上下文，中间的部分有时会被忽略， 就和你开一个漫长的会议一样， 没有结论的会议等于白开了。

![](img/cf928ccc6f13da59a8307d8580fc74dc.png)

> **对记忆进行概括和抽象：**实际中如果让Agent每次进行下一次行为之前都要回忆一生，听起来都觉得不靠谱。所以Agent的记忆管理很重要。

+    Agent记忆， 可以结合RAG技术，实现将知识库转变为长期记忆。实现时需要区分短期工作（交互的上下文）和长期记忆， 同时能够根据用户行为习惯和反馈保存和修改记忆，对后续的Agent的体验可能会有帮助。

+    实现上可以参考MemGPT和Mem0的设计思路。 针对记忆的存储，整理和读取三个阶段，进行干预。 比如Mem0是依靠LLM来实现记忆分层和自适应能力。

![](img/d295d48281598d7cb880debe693fda4d.png)

![](img/77106d648a26673a6cb6d07ab5f33e8e.png)

+   **Agent自我学习能力评测****：** Anthropic官方关于构建AI Ag**e**nt的建议中，提到的最重要的一点就是：Make sure that you have a way to measure your results。 最近不小心看到了StreamBench这个项目， 可能会有帮助。

![](img/1772109891c27905b5530776052fadec.png)

## 多Agent

![](img/81ee96374bff3f97ca13feaf66cfea17.png)

吴恩达提到的Agent设计模式有四个（Reflection，Tool Use， Planning，Multi-Agent Collaboration），我们验证了前三个。

Multi-Agent Collaboration可以提高任务执行效率，多个智能体分工协作，同时具备复杂问题分解能力。

每个Agent各司其职，其上下文记忆对LLM也友好， 不容易偏差，可以更好的利用LLM。 凡事都有两面性，多个Agent也会带来新的问题，就如同微服务相对于单体应用一样，增加了系统的复杂度。 这就引入了Agent之间协作，信息共享，隐私安全，多智能体学习问题。下图是《Why Do Multi-Agent LLM Systems Fail?》 这篇Paper描述的三种常见的失败类别。

![](img/ec5bb8fa360062d1b3016e4eb2025ddb.png)

前文提到的Planning的问题，对于复杂问题的规划，往往会遇到困难或者因为LLM的幻觉，生成不合理的步骤。 虽然ReActAgent有时可以通过推理，行动，观察来为Planning的不合理计划兜底， 但是会降低Agent的效率。

归因为没有对Planning生成的计划进行验证。 针对这个问题，Google提出了新的框架PLanGEN, 包含三个关键的组件： 约束，验证和选择智能体。在PlanGEN框架中，智能体根据实例复杂性优化算法选择，确保更好地适应复杂的规划问题， 约束引导的迭代验证可以提升推理时算法的性能，而适应性的选择则进一步提高了在复杂规划和推理问题上的表现。

![](img/0af8254a73e6fd250c9e030af832ed52.png)

另[外《Scali](https://arxiv.org/abs/2406.07155?spm=5176.8068049.0.0.27366d192fmcKD&file=2406.07155)ng Large-Language-Model-based Multi-Agent Collaboration》这篇Paper提到了协作涌现。 受到神经网络规模法则的启发，增加神经元会导致新能力的涌现，在多智能体协作中，增加智能体是否遵循类似的原理，挺有趣的。

## 5.3 MCP

![](img/23ee18d6b17219d47c600eb7a6f2f829.png)

MCP（Model Context Protocol）是由Anthropic公司提出的一种协议，其核心动机在于显著提升AI模型（如Claude）与外部系统之间的交互能力。通过开源的方式，MCP不仅鼓励社区共同参与和完善这一协议，还旨在推动AI Agent生态的发展。从技术角度来看，MCP的设计并不复杂，但其真正的力量来自于共识——这种共识使得不同系统间的协作成为可能，并为开发者提供了一种标准化的解决方案。

借助MCP，Agent只需一次接入即可无缝适配多种工具和服务，从而大幅减少碎片化开发的重复劳动。MCP让一个Agent能够连接整个世界，实现跨系统的无缝集成能力。如果想追踪MCP当前的发展动态及其生态状况，可以关注awesome-mcp-servers项目，这里汇聚了全球开发者对这一领域的最新探索与贡献。

![](img/d5611799c81278732d6328f309bf206a.png)

**在架构设计上，MCP将Host（Client）与Server分离，工具由Server端调用，而Server端则负责统一管控这些工具的使用，**并对外提供服务。这种设计模式提高了系统的灵活性和可扩展性， 对于云服务提供商来说或者Dify这种LLMOps中间价产品，是利好的， 个人觉得可能有如下几个方面：

1.  **服务复用 **

**MCP通过标准化工具集成流程，云厂商无需再为每个客户或场景单独开发接口，客户也不需要定制开发**，而是可以直接复用现有的成熟服务模块。在保证功能完整性的同时，也降低了维护成本。

1.  **一站式平台构建 **

**依托MCP的标准化协议，云厂商能够快速搭建起“一站式”AI服务平台。例如，通过可视化管理界面和**低代码工作流配置工具，用户无需深入理解底层技术细节，便可轻松完成复杂的任务部署。

1.  **工具生态整合 **

**在MCP的支持下，客户不仅可以享受云厂商提供的专属工具，还能自由利用开源生态中的丰富资源，如数据库、浏览器自动化工具等。这些工具**通过Agent进行统一调用，形成了一套协同的工作体系。云厂商因此得以借助开源社区的力量，避免从零开始开发，从而帮助用户加速AI应用的落地进程，这是一种共生关系。

1.  **合规与安全性保障 **

通过在MCP Server端实施权限管理、审计机制以及沙箱环境的构建，能有效减少敏感数据暴露的风险。例如，在MyManus中引入JupyterClient时，就是希望为代码执行其设计了一个受控的沙箱环境，允许单租户拥有专属的环境变量设置。

也有**反对使用MCP**的， 比如LangChain发起了讨论，并对MCP是否是昙花一现发起了投票，有40%的人认为是个未来的标准， 20%的认为是昙花一现。 具体讨论可以看 MCP: Flash in the Pan or Future Standard? .

[链接](https://blog.langchain.dev/mcp-fad-or-fixture/?spm=5176.8068049.0.0.27366d192fmcKD)

当下不能过于神化任何AI相关东西，MCP解决不了LLM很多时候无法正确地使用工具问题，但还是值得投入研究的，“人们总是高估一项科技所带来的短期效益，却又低估它的长期影响”。AI最大的机会可能在于改变现有的业务流程和产品，专注特定的小规模的业务场景，深入解决具体问题，而不是追求建立大型平台。