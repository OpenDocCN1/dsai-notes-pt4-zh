
# 10个Agent典型应用项目

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/SAq6dzOqFoGdnfx1SeZcd9Lknpf](https://qqs7y1hozd1.feishu.cn/docx/SAq6dzOqFoGdnfx1SeZcd9Lknpf)

## AutoG-T

是一个基于`ChatG-T`完成多目标复杂任务的应用。是综合网络搜索、大语言模型调用、文件读写、`python`和`shell`脚本执行等功能框架，综合这些功能，可以自动帮我们完成多目标复杂的任务。

### trigger prompt

```
    "user": "Determine which next command to use, and respond using the format specified above:"
```

## BabyAGI

> 是一个 AI 支持的任务管理系统示例，它根据初始任务或目标，利用`OpenAI`创建任务列表，并对任务进行优先级排序和执行任务。其背后的主要思想是基于先前任务的结果和预定义的目标创建任务，然后使用 `OpenAI` 的能力根据目标创建新任务。这是原始的任务驱动的自驱`Agent`（2023 年 3 月 28 日）的简化版本。

## SuperAGI

一个开发优先的开源自主人工智能代理框架。使开发人员能够快速可靠地构建、管理和运行有用的代理。

### 创建Agent

智能体是`SuperAGI`中执行任务的主要实体。要创建一个：

```
from superagi_client import AgentConfig

agent_config = AgentConfig(
    name="Sample Agent",
    description="A descriptive purpose of the agent.",
    goal=["Define a specific task for the agent"],
    instruction=["Provide additional guiding instructions"],
    agent_workflow="Goal Based Workflow",
    constraints=[],
    tools=[{"name": "ToolName"}],
    iteration_interval=500,
    max_iterations=10,
    model="G-T-4"
)

agent = client.create_agent(agent_config=agent_config)
```

### 运行Agent

```
agent_id = agent['agent_id']
run_agent = client.create_agent_run(agent_id=agent_id)
```

智能体启动成功之后，会返回一个执行ID:`run_id`

这将检索与指定`agent_id`关联的运行的状态。若要筛选特定的运行或状态，可以使用 `AgentRunFilter` 类型。

## ShortG-T

`ShortG-T` 是一个由开发者 RayVentura 创建的开源项目，致力于在有限的字数内生成高质量、连贯的文本。该项目基于 `OpenAI` 的 `G-T-2` 模型，通过微调和优化，能够在极短的文本序列中产生令人印象深刻的结果。

### 技术分析

#### 基础模型：`G-T-2`

> `G-T-2`（Generative Pre-trained Transformer 2）是 `OpenAI` 在2019年发布的预训练语言模型。它使用自注意力机制，能够理解整个输入序列的信息，并生成与上下文相关的响应。在 `ShortG-T` 中，`G-T-2` 的强大语义理解和生成能力被进一步利用，以适应更紧凑的文本生成任务。

#### 微调与优化

`RayVentura` 对原始的 `G-T-2` 进行了特定场景的数据集微调，使其更适合处理短文本任务。这包括对数据集的选择、训练策略的调整以及可能的参数优化，以确保在有限字符数量下也能保持良好的语义准确性和流畅性。

### 应用场景

> **创意写作：**为作家和创意人员提供灵感，快速生成故事开头、诗歌片段或新闻标题。

> **自动摘要：**针对长篇文章，可以生成简明扼要的总结。

> **智能助手：**在对话系统中，用于生成简洁而有意义的回答。

> **营销文案：**快速构思广告标语或社交媒体帖子。

> **教育工具：**帮助学生理解概念，通过简短的例子或解释来阐述复杂的知识点。

### 特点

> **高效：**能在短短几句话内传达关键信息。

> **适应性强：**可以根据不同的主题和风格进行微调，满足不同应用场景的需求。

> **开放源码：**开放社区的持续贡献和支持，使得模型不断改进。

> **易于集成：**提供清晰的文档和示例代码，便于其他开发人员将其整合到自己的项目中。

可以使用以下代码来生成简短回答：

```
import openai

# 设置你的OpenAI API密钥
openai.api_key = "YOUR_OPENAI_API_KEY"

def get_short_G-T_answer(question, model="G-T-4"):
    response = openai.Completion.create(
        engine=model,
        prompt=question,
        max_tokens=7,  # 设置生成的最大token数，7 tokens大约对应3个单词
        n=1,  # 生成1个可能的回答
        stop=None,  # 可以设置停止符，例如".", "!"等，根据需要使用
        temperature=0.5,  # 设置生成的文本的随机性，0.0为完全随机，1.0为完全按概率分布
    )
    return response.get("choices")[0].get("text")

# 示例问题和使用
question = "What is the capital of the United States?"
answer = get_short_G-T_answer(question)
print(answer)
```

## ChatDev

是一个基于大型语言模型（`LLMs`）的框架，旨在通过多智能体协作创建定制化软件，使用自然语言概念来革新数字世界。

### 库的特性和优势

1. **高度定制化和扩展性：**`ChatDev` 提供了一个易于使用的框架，可以高度定制和扩展，适合研究集体智能。

1. **多智能体组织结构：**它通过不同角色的智能代理（如 `CEO`、`CTO`、程序员等）形成多智能体组织结构，参与设计、编码、测试和文档化等专业功能研讨会。

1. **实验共学模块：**2024年1月，`ChatDev`集成了体验式共学模块，采用创新方法让指导者和助手代理积累面向快捷方式的经验，有效解决新任务，减少重复错误，提高效率。

### 库应用场景

1. **软件开发：**`ChatDev` 作为一个 `SaaS` 平台，使得软件开发人员和创新企业家能够高效、低成本、低门槛地构建软件。

1. **增量开发：**支持在现有代码上进行开发的增量开发功能，允许代理在现有代码基础上进行进一步开发。

1. **人机交互：**通过“人机交互”模式，用户可以扮演审阅者的角色，对程序员的工作提出建议，促进人机协作。

以下是一个简单的Python示例，展示如何使用`random`模块生成一个随机的聊天机器人回复。

```
import random

# 定义一个包含不同回复的列表
responses = [
    "当然可以",
    "我能做到",
    "我正在尝试",
    "我不能这么做",
    "对不起，我不明白",
    "请再重复一次",
    "请问具体问题是什么？",
    "你好，我现在正在线上"
]

# 定义一个函数，返回随机的回复
def get_random_response():
    return random.choice(responses)

# 测试函数
print(get_random_response())
```

## MetaG-T

是一个基于AI Agent的大模型框架，专注于软件开发，从需求分析到代码实现等实现了全流程的覆盖。

### 优势

+ 多`Agent`组成协作处理更复杂的任务。

+ 通过将`SOP`融入到多智能体协作中，解决了现有方法在处理复杂任务时存在的幻觉问题。

+ 通过结构化协调和模块化输出，能够有效地解决复杂的多智能体协作问题。

+ 智能体可以主动从环境中获取相关知识，而不是简单地通过对话获取信息。这种设计更符合人类组织的运作模式。

### 劣势

+ 偶尔会引用不存在的资源文件，如图像和音频。

+ 在执行复杂任务时，它容易调用未定义或未导入的类或变量。

+ 角色和流程还比较固定，无法实现动态扩展，如扩展UI设计角色。

## Camel

是`Communicative Agents for “Mind” Exploration of Large Scale Language Model Society`的缩写，大意为大规模语言模型社会的“心智”探索通讯智能体。

### 贡献点：

> 1）引入了一种新的沟通式智能体（通过角色扮演实现），让智能体在最少的人工干预下通过自主合作，共同完成任务；

> 2）提供了一种为研究多智能体合作行为和能力的可扩展方法，阐述了实现自主合作的挑战，并提供了应对这些挑战的策略。

> 3）开源了`CAMEL`智能体，其中包含各种智能体的实现、数据生成流程、数据分析工具和收集的数据集，以支持沟通时智能体等领域的研究及更多其他相关领域的研究。

### 实现原理

#### 角色扮演框架

> `CAMEL`专注于以任务为导向的角色扮演，包含一个`AI`助理（`AI Assistant`）和一个`AI`用户（`AI User`）。在多智能体系统接收到人类用户的初步想法和角色分配后，任务指定智能体将提供详细的描述，使想法更加具体化。

> 然后，`AI`助理和`AI`用户将通过多轮对话合作完成指定的任务，直到`AI`用户确定任务完成为止。另一方面，`AI`用户负责向`AI`助理提供指令，并引导对话朝着任务完成的方向进行；另一方面，`AI`助理则需要遵循`AI`用户的指示，做出回答并提供具体的解决方案。

#### 人类输入和任务说明

> 角色扮演会话将从人类的想法和选择的角色开始。想开发一个用于股市的交易机器人，然后选择了一个`Python`程序员和一个股票交易员两个角色。

> 在确定了想法和角色之后，任务指定智能体将根据输入的想法，为`AI`助理角色提出一个具体的任务，以帮助`AI`用户角色完成任务。引入任务指定智能体主要原因为对话智能体通常需要一个具体的任务提示来实现任务，而对于非领域专家来说，这可能具有挑战性或费时。

以下是简单的代码使用示例

```
conda create --name camel python=3.10
conda create --name camel python=3.10
git clone -b v0.1.0 https://github.com/camel-ai/camel.git
cd camel
pip install -e .
```

## JARVIS

处理任务规划、模型选择、任务执行和内容生成。通过访问`HuggingFace`中心中的数十个专用模型，利用`ChatG-T`的推理能力将最佳模型应用于给定任务。

> 系统的工作流程由四个阶段组成：

+ **任务规划：**使用`ChatG-T`分析用户的请求，了解他们的意图，并将其分解为可能的可解决的任务。

+ **模型选择：**为了解决计划的任务，`ChatG-T`根据描述选择`Hugging Face`上托管的专家模型。

+ **任务执行：**调用并执行每个选定的模型，并将结果返回给`ChatG-T`。

+ **响应生成：**最后，使用`ChatG-T`整合所有模型的预测，并生成响应。

## OpenAGI

是指开放式人工智能，即一种可以从经验中学习和自我完善的`AI`系统。

`AGI` 研究平台专门设计提供复杂的多步任务以及相应的数据集、评估方法和各种可扩展模型，这些模型可以被综合以有效地解决这些任务。

### 以下是一个简单的代码示例

```
class SimpleEnvironment:
    def __init__(self):
        self.agents = []
        self.state = {}

    def add_agent(self, agent):
        self.agents.append(agent)

    def step(self):
        for agent in self.agents:
            action = agent.act(self.state)
            self.state = self.apply_action(action)

    def apply_action(self, action):
        # 这里应该包含对环境状态的更新逻辑
        # 例如，移动Agent的位置，或是触发某些效果
        return self.state  # 这里简化地返回当前状态

class SimpleAgent:
    def __init__(self):
        pass

    def act(self, state):
        # 这里应该包含Agent的决策逻辑
        # 例如，基于当前状态选择一个随机动作
        return "action"  # 返回一个动作

# 创建环境和Agent
env = SimpleEnvironment()
agent = SimpleAgent()

# 将Agent添加到环境中并开始交互
env.add_agent(agent)
env.step()
```

## XAgent

是一个开源的基于大型语言模型（`LLM`）的自主智能体，能够自动解决各种任务。它旨在成为一个通用的智能体，应用于各种任务。

### XAgent的特性:

+ **自主/自治：**`XAgent`可以无须人工干预自动完成各种任务.

+ **安全：**`XAgent`旨在安全运行，所有的`action`约束在`docker`容器中执行.

+ **可扩展性：**`XAgent` 做了可扩展性设计，可以容易地增加新工具加强`agent`的能力，甚至可以增加新的`agenti`.

+ **界面化：**`XAgent` 为用户提供友好的`agent`交互界面，当然也可以使用命令行接口与`agent`交互。

+ **人机协作:** `XAgent` 可以与人协作处理任务，不仅是在解决复杂任务遵循人类的指导，当`agent`遇到挑战时也能够寻求人类的帮助。

### XAgent的构成：

+ **`Dispatcher`**

> 负责动态实例化和派发任务给不同的`agent`，允许用户新增`agent`并改善它们的能力

+ **`Planner`**

> 负责生成和修正任务计划。将任务拆分为子任务并为它们生成里程碑，允许`agent`一步一步地完成任务

+ **`Actor`**

> 负责采取行动达成目标和完成子任务。`actor`使用各式各样的工具处理子任务，同时也可以与人类合作完成任务
