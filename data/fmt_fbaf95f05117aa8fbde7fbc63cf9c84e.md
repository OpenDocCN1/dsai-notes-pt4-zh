
# 1. AI Agent 的基本概念与架构

在 OpenAI 提出的人工智能最后阶段：AI系统将变得足够先进和智能，可以完成整个组织的工作，并将组织归类为智能路线图的最后一步。

人类[的优势是能够吸收相对大量的信息，过滤掉不重要的细节](https://arxiv.org/pdf/2210.03629)，并根据关键信息做出决策。比如，在处理一件事情之前，我们通常会先将大问题分解为一个个小的假设，然后尝试通过观察逐步支持或反驳这些假设。从这个现实的观点出发，启发 AI Agent 早期范式的一篇论文 REACT: SYNERGIZING REASONING AND ACTING IN LANGUAGE MODELS 中使用“思维链提示”来模仿这个概念，它将多步骤问题分解为中间步骤：

• 发起一项行动，让大模型观察所选环境的反馈  
• 在流程中收集所有信息并使用它来决定下一步采取什么合适的行动  
• 迭代地执行此操作来解决更大、更复杂的任务，使用一种称为“推理跟踪”的方法，该方法涉及跟踪整个过程所经历的步骤或阶段以得出结论或解决方案  

如下图所示，整个过程是一个动态循环。代理不断从环境中学习，通过其行动影响环境，然后根据环境的反馈继续调整其行动和策略。这种模式特别适用于那些需要理解和生成自然语言的应用场景，如聊天机器人、自动翻译系统或其他形式的自动化客户支持。

一个这是更加感官性的认知如下图所示：

![](img/886523b2c1fc38b116abd352f3640628.png)

所以从这个过程中，我们就可以抽象出 AI Agent 的最经典，同时也是目前任何一套 Agent 框架的基本框架，如下图所示：

![](img/a5e83377c9b3e041e02430e4afc01868.png)

图片来源：[https://lilianweng.github.io/posts/2023-06-23-agent/](https://lilianweng.github.io/posts/2023-06-23-agent/)，强烈建议大家通篇阅读。

这套智能代理架构是指自主代理的结构化设计，自主代理是能够独立感知环境、做出决策并采取行动以实现特定目标的系统或实体。该架构描述了代理的各个组件如何交互以促进智能行为。该架构包含四个关键组件：

• 规划（Planning）：该组件将代理置于动态环境中，使其能够根据其目标和收集的信息制定策略并规划未来的行动。  
• 记忆（Memory）：该组件使智能体能够回忆过去的行为、经历和结果，这对于学习和适应至关重要。  
• 行动（Action）：该组件将智能体的决策转化为具体的行动，执行计划的任务以达到预期的结果。  
• 工具（Tools）：拥有一名仅拥有 LLM 的代理人就像使用一台没有任何额外设备的计算机一样。工具让代理能够使用互联网、获取特殊知识或与擅长特定事物的几种不同的人工智能模型一起工作，从而使代理变得更加有用。

人工智能代理的特点是其主动性和决策能力。与被动工具不同，它们主动参与环境，做出选择并采取行动来实现其指定目标。在企业环境中，人工智能代理通过自动化日常任务和分析复杂数据来提高效率，从而使员工能够专注于战略和创造性工作。这些代理补充而不是取代人类的努力，促进提高劳动力的生产力和效率。

让我们想象一个中国市场销售经理李华和他的人工智能助理的场景。

李华的工作日以检查电子邮件开始。他收到了来自潜在客户张伟的邮件，张伟对他公司提供的高效解决方案感兴趣。李华的人工智能助手直接连接到他的电子邮件系统，并且实时监控这些互动。根据李华过去的回复习惯和公司提供的信息库，人工智能助手草拟了一封详细的回复。邮件中不仅总结了公司的高效解决方案及其优势，还根据张伟的需求定制了相关建议。

李华审阅了这份草稿邮件，加入了一些个性化的语句，以显得更加友好和专业，然后发送给了张伟。随后，人工智能建议的后续步骤包括安排与张伟的电话会议、发送一份详细的产品介绍手册，或者如果一周内没有得到回复，提醒李华进行跟进。李华同意了这些建议，人工智能助手随即整理他的日程，通过电子邮件发送产品手册，并在他的电子日历中设置了跟进提醒。通过让人工智能处理这些日常但关键的任务，李华可以将更多精力投入到其他重要的业务拓展活动中。

这个过程中 AI Agent 展现出来的关键能力：

• AI Agent 利用大模型固有的语言理解能力来解释指令、上下文和目标。这使它们能够根据人类的提示自主或半自主地运行。  
• AI Agent 可以使用各种工具（阅读邮件，计算器、搜索引擎等）来收集信息并采取行动来完成分配的任务。它们的能力超出了单纯的语言处理范围。  
• AI Agent 能够展示复杂的推理技术，可以建立逻辑联系来得出结论和解决问题，而不仅仅是简单的文本理解。  
• AI Agent 可以通过将上下文和目标集成到其语言生成能力中，生成用于特定目的的定制文本，例如电子邮件、报告和营销材料。  
• AI Agent 代理可以完全自主或半自主运行，需要与用户进行不同级别的交互。  

人工智能代理的好处不仅仅是效率。它们营造协作环境，降低人为错误的风险，并腾出宝贵的时间进行创造性和战略性思考。从本质上讲，人工智能代理不仅仅是工具，更是工具。他们是补充人类能力并推动创新的合作伙伴。

# 3. 从零到一实现一个 L2 级 Agent

对于 L2 级的 Agent 级的 Agent，本质上就是一个具备推理能力的聊天机器人，我们可以直接调用某个大模型构建多轮对话的问答流程。这里我们就从环境配置开始，从零到一带领大家在自己的电脑上开发一个能与大模型交互的多轮问答机器人。

## 环境配置

首先，我们需要在本地配置一个 Python 的开发环境，建议使用 Anaconda3 的 Jupyter Lab，当然，大家也可以选择使用 Pycharm 或者 Vscode 工具。其次，需要有一个有效的 OpenAI `API_Key`，今天直播体验课为大家准备了限量免费使用的 `API_Key`，详细的安装教程和 `API_KEY` 的免费领取，可以添加专属助理：

![](img/fbe7af3dfeed1deb064692e5f265b0c6.png)

在准备好 `API_key` 并顺利启动 Jupyter 之后，即可测试能否顺利调用 OpenAI 大模型，测试代码如下：

```
# ! pip install openai
```

```
# ! setx OPENAI_API_KEY "your_api_key_here"
```

注意：配置完成后，需要重启电脑，方可使环境变量生效。

```
from openai import OpenAI
```

通过调用 `models.list()` 方法，可以查看到当前的 API_key 都可以调用哪些模型。

```
print(client.models.list())
```

输出示例：

```
SyncPage[Model](data=[Model(id='o1-mini-2024-09-12', created=1725648979,
object='model', owned_by='system'), Model(id='gpt-4o-mini',
created=1721172741, object='model', owned_by='system'), Model(id='dall-e-2',
created=1698798177, object='model', owned_by='system'), Model(id='gpt-4-1106-
preview', created=1698957206, object='model', owned_by='system'),
Model(id='gpt-4o-mini-2024-07-18', created=1721172717, object='model',
owned_by='system'), Model(id='gpt-3.5-turbo-instruct', created=1692901427,
object='model', owned_by='system'), Model(id='babbage-002',
created=1692634615, object='model', owned_by='system'), Model(id='davinci-
002', created=1692634301, object='model', owned_by='system'),
Model(id='whisper-1', created=1677532384, object='model', owned_by='openai-
internal'), Model(id='dall-e-3', created=1698785189, object='model',
owned_by='system'), Model(id='gpt-4o-realtime-preview-2024-10-01',
created=1727131766, object='model', owned_by='system'), Model(id='text-
embedding-3-small', created=1705948997, object='model', owned_by='system'),
Model(id='gpt-3.5-turbo-16k-0613', created=1685474247, object='model',
owned_by='openai'), Model(id='gpt-3.5-turbo-16k', created=1683758102,
object='model', owned_by='openai-internal'), Model(id='gpt-4-0125-preview',
created=1706037612, object='model', owned_by='system'), Model(id='gpt-4o-
realtime-preview', created=1727659998, object='model', owned_by='system'),
Model(id='gpt-4-turbo-preview', created=1706037777, object='model',
owned_by='system'), Model(id='omni-moderation-latest', created=1731689265,
object='model', owned_by='system'), Model(id='gpt-4o-2024-05-13',
created=1715368132, object='model', owned_by='system'), Model(id='omni-
moderation-2024-09-26', created=1732734466, object='model',
owned_by='system'), Model(id='tts-1-hd-1106', created=1699053533,
object='model', owned_by='system'), Model(id='chatgpt-4o-latest',
created=1723515131, object='model', owned_by='system'), Model(id='gpt-4',
created=1687882411, object='model', owned_by='openai'), Model(id='gpt-4-0613',
created=1686588896, object='model', owned_by='openai'), Model(id='o1-mini',
created=1725649008, object='model', owned_by='system'), Model(id='gpt-3.5-
turbo', created=1677610602, object='model', owned_by='openai'), Model(id='gpt-
3.5-turbo-0125', created=1706048358, object='model', owned_by='system'),
Model(id='o1-preview', created=1725648897, object='model', owned_by='system'),
Model(id='o1-preview-2024-09-12', created=1725648865, object='model',
owned_by='system'), Model(id='gpt-4-turbo', created=1712361441,
object='model', owned_by='system'), Model(id='tts-1-hd', created=1699046015,
object='model', owned_by='system'), Model(id='text-embedding-ada-002',
created=1671217299, object='model', owned_by='openai-internal'), Model(id='gpt-
4-turbo-2024-04-09', created=1712601677, object='model', owned_by='system'),
Model(id='gpt-3.5-turbo-1106', created=1698959748, object='model',
owned_by='system'), Model(id='gpt-4o-audio-preview', created=1727460443,
object='model', owned_by='system'), Model(id='gpt-4o-audio-preview-2024-10-
01', created=1727389042, object='model', owned_by='system'), Model(id='tts-1',
created=1681940951, object='model', owned_by='openai-internal'), Model(id='tts-
1-1106', created=1699053241, object='model', owned_by='system'), Model(id='gpt-
4o-2024-08-06', created=1722814719, object='model', owned_by='system'),
Model(id='gpt-3.5-turbo-instruct-0914', created=1694122472, object='model',
owned_by='system'), Model(id='text-embedding-3-large', created=1705953180,
object='model', owned_by='system'), Model(id='gpt-4o-realtime-preview-2024-12-
17', created=1733945430, object='model', owned_by='system'), Model(id='gpt-
4o', created=1715367049, object='model', owned_by='system'), Model(id='gpt-4o-
mini-realtime-preview', created=1734387380, object='model',
owned_by='system'), Model(id='gpt-4o-mini-realtime-preview-2024-12-17',
created=1734112601, object='model', owned_by='system'), Model(id='gpt-4o-2024-
11-20', created=1731975040, object='model', owned_by='system'), Model(id='gpt-
4o-audio-preview-2024-12-17', created=1734034239, object='model',
owned_by='system'), Model(id='gpt-4o-mini-audio-preview', created=1734387424,
object='model', owned_by='system'), Model(id='gpt-4o-mini-audio-preview-2024-
12-17', created=1734115920, object='model', owned_by='acmr-
```
