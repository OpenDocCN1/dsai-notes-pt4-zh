
# 第一章 智能体架构与案例

## 工具

工具实际就是`API`，分为build something及use something，支撑了智能体通往现实世界。假如实现现实世界完整表达的`API`需要`k`个，而这`k`个已经被实现，那么`Agent`理论上就可以达到全能。当然，前提是它要能够无错、娴熟的基于这些`API`写出完整的代码，而这代码自动撰写的missing piece就是`MetaGPT`。

## 直觉

直觉如呼吸、吞咽、心跳等不需要在脑内有任何显性思考的部分，如你要喝水，会从手边拿起一个矿泉水，然后拧开它的盖子，之后开始喝水；全流程无需特意思考。现实世界的大部分任务都需要直觉，通过直觉系统，能将任务完成度从之前最优的30%多提升到接近90%。

## 运控

运控黑色部分主要指运动控制，对应具身智能相关特性。

如果你看了介绍还是不明白，用一个具体的GPTs的案例向你说明什么是agent智能体代理。

## 智能体举例

https://chat.openai.com/g/g-O8Y8pE85i-findgpts

这是作者制作的一个GPTs导航，帮助用户用LLM的模糊语义搜索找到全网公开的GPTS以及其体验链接

![](img/abb6bc1eab75d0441cebc26429250302.png)

![](img/693b2670fd9e4c403bb24c093647f12e.png)

![](img/76f843ed643b71166e398f061647b720.png)

这里的记忆可以分为`chatgpt`训练过程中的语料库的语义记忆，我强调进行[GPTs导航站收录](https://zw73xyquvv.feishu.cn/wiki/TPxXwK3XaiuzMvkDeoYcYNU9n5c)设计时[匹配出的工作记忆/情节](https://github.com/h4r5h1t/webcopilot)记忆，用户主动上传的`knowledge`（比如爬取 ）文档中所有的`GPTs`和对应的`describe`的长期记忆。

目前LLM的自主规划能力很弱，所以我用一套`sop`工作步骤让他能够有更多的思考空间（时间换准确率）。

这里`findGPTs`会先解构用户的描述，用LLM的基础能力去泛化找到其他语义相似的keywords进行检索，检索这一步采用了`webcopilot`开源的`action`进行浏览器访问（这个是强于openai提供的bing search的效率的）。

![](img/45a946a8135fb5bd91d750005620adf8.png)

这一步也是openai api插件开发者和普通GPTs开发者拉开差距的地方，这里就体现了LLM `tools`工具调用的能力。

拿到了搜索引擎返回的 [GPTs list，llm就开始观察和思考哪个](https://zhuanlan.zhihu.com/p/619063970) 是最高匹配度的GPTs，如果将来openai支持访问gpts的`API`那么这个`agent`就可以自动的提炼`describe`直接去询问对应的GPTs并调用`API`返回搜索结果即为agent `action`行动的结果。

比较类似的超级入口可以参考HuggingGPT火了：一个ChatGPT控制所有AI模型，自动帮人完成AI任务。

> prompt开源：

> step1:根据用户的输入（描述的功能以及需求）进一步补全用户所需的gpts description并提炼`{English key words}`。

> step2:利用`webcopilot`对`{English key words}`进行使用`{site:chat.openai.com/g+{English key words}}`检索，例如以"game"触发webcopilot action搜索`<site:chat.openai.com/g Game>`。

> step3:把搜索结构根据`<Open AI gpts+description+link>`格式返回一个list。

> rule：高匹配度以及使用人数的gpts排序优先，多个关键词为或的关系。

> step4：访问搜索结果排名最高的网址例如：https://chat.openai.com/g/g-Sug6mXozT-game-time并返回用户的输入（描述的功能以及需求）对应的结果。

## 相关资料

[基于大语言模型的AI Agents—Part 1 | Breezedeus.com](https://www.breezedeus.com/article/ai-agent-part1)

[【直播回放】MetaGPT作者深度解析直播回放_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Ru411V7XL/?spm_id_from=333.337.search-card.all.click)

## 热门智能体案例

### 流行的GPTs：

[GitHub - AgentOps-AI/BestGPTs: Top ranked OpenAI GPTs](https://github.com/AgentOps-AI/BestGPTs)

### Openai time line 案例迅速体验：

1. [AgentGPT](https://agentgpt.reworkd.ai/zh):基于浏览器的 AutoGPT 实现，可通过无代码平台访问

帮我写一个openai近期新闻的时间线。

1. https://app.tavily.com/chat 基于浏览器的GPT Research实现，可通过无代码平台访问

![](img/c7e852c3a02148152f60184a21761959.png)

[Tavily AI --- 塔维利人工智能.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/F2Dtb9WMpooS8JxggUacJDmzn7b)

1. https://www.perplexity.ai/ LLM驱动的agent搜索引擎

![](img/1a5cfbafa2deb3230fad7c4343bffcde.png)

[Timeline of the OpenAI turmoil --- OpenAI风波的时间表.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/VeUCbLsApoK2URxBuL6cbtHknQ1)

### 代码产品：

1. Cursor - The AI-first Code Editor：AI优先的代码编辑器。在专为与人工智能进行配对编程而设计的编辑器中更快地构建软件。

1. Chatgpt `Code Interpreter`：代码解释器（`Code Interpreter`）可以让普通用户（非程序员）用自然语言完成以前需要写代码来完成的复杂工作。

### 更多仓库以及产品：

https://github.com/e2b-dev/awesome-ai-agents

![](img/f0c7a73d83e59b7d60be585baf819905.png)

## 智能体的宏观机会

99%的互联网入口将由App变为智能体：人类感知的字节数降低到极限

![](img/b3dff8f7e450ffbf20ab457995e2de5a.png)

社会协作变革：虚拟员工进入企业，红杉预计智能体数量五年后和人类1比1

![](img/e410687937f255ae3b7b964d6e968c48.png)

## AI Agent 与 Sy1 & Sy2

### Agent：LLM时代的新软件

1. Agent是新的高级物种，为了实现类人智能，大语言模型实现了人脑的20%，我们需要构建`AgentStack`补齐剩余的80%。

1. 补齐之后，Agent就可以直接供给生产力，并且同类问题的平均解决成本在人类的1%以内，效率平均在100倍以上。

1. 99%的互联网入口将由App变为Agent：人类感知的字节数降低到极限。

1. 信息技术革命沿着，门户->搜索->推荐->Agent演变；本质是人类需要操作的字节数在迅速变少。比如一个调研需求，人类需要搜索、阅读100个页面再做总结，但Agent可以自己做完这个流程，人类阅读结果就可以。以此类推，订票、购物、搜索、IM等大部分流程都可以交给Agent做。

1. 软件的需求依然会持续存在，但入口将被Agent接管。软件仅做为数字世界的一部分被感知、做为部分`API`被使用。

### Sy1&Sy2给Agent的启发

@`Andrej Karpathy`：

> 第一件事是关于系统 1 与系统 2 类型思维的概念，这个概念是由《思考，快与慢》这本书所普及的，所以这个区别是什么？这个想法是你的大脑可以以两种不同的模式运行。系统 1 思维是你的大脑的快速、本能和自动化的部分，所以例如，如果我问你 `2+2` 等于多少，你实际上并没有做数学计算。

![](img/d146ae2b2d235aac48d0d40e06ac0f9e.png)

> 你只是告诉我它等于四，因为这是可用的，已经存在于你的大脑中，是本能的。但是当我告诉你 `17*24` 等于多少时，你并没有准备好这个答案，所以你会启动你的大脑的另一部分，这部分更加理性，更加缓慢，进行复杂的决策，感觉更有意识。你不得不在脑海中解决这个问题，然后给出答案。

> 另一个例子是，如果你们中的一些人可能下象棋，当你进行快速象棋比赛时，你没有时间思考，所以你只是根据直觉移动棋子。这时大部分由你的系统 1 来处理。但如果你在比赛中，有更多时间来思考，那么你会更多地参与到决策中，你会感到自己在建立可能性的树，这是一个更有意识、更费力的过程。你必须在脑海中解决问题并给出答案。

![](img/c1a08377166ad23de8336d5614e6e9a2.png)

> 现在事实证明，`LLM`目前只有系统 1。它们只有这个本能部分。它们不能思考并推理出各种可能性。它们只是按顺序输入单词，并且基本上这些语言模型有一个神经网络，可以给出下一个单词，所以它就像右边的漫画一样，你只是在跟踪轨迹，这些语言模型基本上只是不断发出声音，这就是它们按照序列采样单词的方式。每个这样的块大致需要相同的时间，所以这基本上是 `LLM` 在系统 1 设置中的工作方式，所以很多人认为，赋予 `LLM` 系统 2 的能力会给人们带来灵活的时间，让它们更深入地思考问题，反思和重新表述，然后以更有信心的答案回来，所以你可以想象将时间表示为 `x` 轴，某种响应的准确度表示为 `y` 轴，当你绘制它时，你希望它是一个单调递增的函数。

### 相关资料：

[OpenAI官宣第一笔收购！开源「我的世界」8人顶级团队加盟，押注AI智能体](https://mp.weixin.qq.com/s/8STgkzD4hxtTUeUSoh6xEQ)

GPT 是否会带来失业潮？解读 OpenAI 爆火论文

## 第二章课程任务

根据时间轴感兴趣的部分观看教程视频，并输出一篇文字笔记（知乎/📕/即刻/csdn/GitHub）分享你的学习感想。

[【直播回放】MetaGPT作者深度解析直播回放_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Ru411V7XL/?spm_id_from=333.337.search-card.all.click&vd_source=199360a57d436da9caa4d615e469c770)

这个视频讲述了MetaGPT作者关于模型训练和优化的深入解析。作者分享了关于大语言模型发展路径的实际路径，以及`ZeroShot`的逻辑和效果应用，还涉及了智能体和环境的认知互动等。视频内容包括智能体架构、代码结构、环境配置等方面的讲解。

+ 0:01 🌍大语言模型的发展路径

+ 9:09 💡BERT的有效性和训练成本

+ 18:23 🎮强化学习在模型推理中的应用

+ 27:33 🗣️直觉、语音交互和多智能体协作

+ 36:46 🔍智能体的需求和发展前景

+ 45:55 🤖MetaGPT：AI智能体框架

+ 55:11 📊MMLU榜单、GPT-4性能和商业机会

+ 64:24 🌟多智能体框架的愿景和影响

+ 73:34 💼创业公司和MetaGPT软件公司实例

+ 82:47 🔧智能体的功能和设计理念

+ 91:59 🔬Hugging GPT的工作原理和开源竞争

## 第三章：MetaGPT框架组件介绍

### Agent组件介绍

#### Agent概念概述

在`MetaGPT`看来，我们把`Agent`想象成环境中的数字人，其中

`Agent = 大语言模型（LLM）` + 观察 + 思考 + 行动 + 记忆

这个公式概括了智能体的功能本质。为了理解每个组成部分，让我们将其与人类进行类比：

1. **大语言模型（`LLM`）**：LLM作为智能体的“大脑”部分，使其能够处理信息，从交互中学习，做出决策并执行行动。

1. **观察**：这是智能体的感知机制，使其能够感知其环境。智能体可能会接收来自另一个智能体的文本消息、来自监视摄像头的视觉数据或来自客户服务录音的音频等一系列信号。这些观察构成了所有后续行动的基础。

1. **思考**：思考过程涉及分析观察结果和记忆内容并考虑可能的行动。这是智能体内部的决策过程，其可能由`LLM`进行驱动。

1. **行动**：这些是智能体对其思考和观察的显式响应。行动可以是利用`LLM`生成代码，或是手动预定义的操作，如阅读本地文件。此外，智能体还可以执行使用工具的操作，包括在互联网上搜索天气，使用计算器进行数学计算等。

1. **记忆**：智能体的记忆存储过去的经验。这对学习至关重要，因为它允许智能体参考先前的结果并据此调整未来的行动。

在`MetaGPT`中定义的一个agent运行示例如下：

![](img/f9aa91b559cce70f27aa0a18072fa58c.png)

+ 一个agent在启动后他会观察自己能获取到的信息，加入自己的记忆中。

+ 下一步进行思考，决定下一步的行动，也就是从`Action1`，`Action2`，`Action3`中选择执行的`Action`。

+ 决定行动后，紧接着就执行对应行动，得到这个环节的结果。

而在`MetaGPT`内 `Role` 类是智能体的逻辑抽象。一个 `Role` 能执行特定的 `Action`，拥有记忆、思考并采用各种策略行动。基本上，它充当一个将所有这些组件联系在一起的凝聚实体。目前，让我们只关注一个执行动作的智能体，并看看如何实现一个最简单的 Agent。

#### 实现一个单动作Agent

下面将带领大家利用`MetaGPT`框架实现一个生成代码的Agent `SimpleCoder`，我们希望这个Agent能够根据我们的需求来生成代码。

要自己实现一个最简单的`Role`，只需要重写`Role`基类的 `_init_` 与 `_act` 方法。

在 `_init_` 方法中，我们需要声明 Agent 的`name`（名称）`profile`（类型）。

我们使用 `self._init_action` 函数为其配备期望的动作 `SimpleWriteCode`，这个 `Action` 应该能根据我们的需求生成我们期望的代码。

在 `_act` 方法中，我们需要编写智能体具体的行动逻辑，智能体将从最新的记忆中获取人类指令，运行配备的动作，MetaGPT将其作为待办事项 (`self.rc.todo`) 在幕后处理，最后返回一个完整的消息。

##### 需求分析

要实现一个 `SimpleCoder`，我们需要分析这个`Agent`它需要哪些能力。

![](img/b9a6f38a3bc1602abddb4e7163433450.png)

首先我们需要让他接受用户的输入的需求，并记忆我们的需求，接着这个Agent它需要根据自己已知的信息和需求来编写我们需要的代码。

##### 编写SimpleWriteCode动作

在 MetaGPT 中，类 `Action` 是动作的逻辑抽象。用户可以通过简单地调用 `self._aask` 函数 来获取 `LLM` 的回复。

`self._aask` 函数如下：

```python
async def _aask(self, prompt: str, system_msgs: Optional[list[str]] = None) -> str:
    """Append default prefix"""
    if not system_msgs:
        system_msgs = []
    system_msgs.append(self.prefix)
    return await self.llm.aask(prompt, system_msgs)
```

它将调用你预设好的 `LLM` 来根据输出的提示词生成回答。

下面是实现`SimpleWriteCode`的具体代码：

```python
import re
import asyncio
from metagpt.actions import Action

class SimpleWriteCode(Action):

    PROMPT_TEMPLATE = """
    Write a python function that can {instruction} and provide two runnnable test cases.
    Return ```python your_code_here ``` with NO other texts,

    your code:

    """

    def __init__(self, name="SimpleWriteCode", context=None, llm=None):

        super().__init__(name, context, llm)

    async def run(self, instruction: str):

        prompt = self.PROMPT_TEMPLATE.format(instruction=instruction)
```
