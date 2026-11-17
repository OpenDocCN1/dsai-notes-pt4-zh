
# 基于大模型的Agent进行任务规划的10种方式

> 来源：[https://agijuejin.feishu.cn/docx/N8OsdLcnsope5xxj0w1cXhiunHe](https://agijuejin.feishu.cn/docx/N8OsdLcnsope5xxj0w1cXhiunHe)
>
> 作者：是莫尔索 | 莫尔索随笔
>
> 原文：https://mp.weixin.qq.com/s/-McKWDsnyGGl7Ep-l_FuXQ
>
> 基于大模型的 Agent 基本组成应该包含规划（planning)、工具（Tools)、执行(Action)，和记忆(Memory)四个方面，上一篇中多轮对话中让AI保持长期记忆的8种优化方式（附案例和代码）重点讲了进行长记忆管理的 8 种方案，本节将从 Agent 概念、ReAct 框架、示例、以及一些论文思路来具体聊下任务规划的话题，同时会辅以代码帮助理解，欢迎大家一起探讨。

在 OpenAI AI 应用研究主管 Lilian Weng 的博客《大语言模型（LLM）支持的自主式代理》(https://lilianweng.github.io/posts/2023-06-23-agent/)中，将规划能力视为关键的组件之一，用于将任务拆解为更小可管理的子任务，这对有效可控的处理好更复杂的任务效果显著。

![](img/0fc39ffa4e1e21f0a74e4803890f779b.png)

基于大语言模型（LLM）的自主代理组成

## 人是如何做事的？

在日常工作中，我们通常将一项任务拆解成几个步骤：制定计划、执行计划、检查结果，然后将成功的作为标准，未成功的则留待下一次循环解决，这种方法已经被证明是高效完成任务的经验总结。

而这就是 `PDCA`(https://zh.wikipedia.org/zh-sg/PDCA)，是美国质量管理专家休哈特博士提出的，由戴明采纳、宣传，获得普及，所以又称戴明环。基于 PDCA 模型可以将一般项目分为四个阶段，即 Plan(计划)——Do(执行)——Check(检查)—— Act(处理)。把一件事情做出计划、然后实施计划、检查计划分析哪些出现了问题并提出解决方案、然后成功的纳入标准，不成功的进入下一个循环去解决，循环往复，从而形成一套标准化的流程。

![](img/ac83c5bc970598baa07b20a2df14dfb2.png)

PDCA模型

## Agent 是怎样工作的

而 Agent 的执行过程与人做事的方式类似，其中最有效的就是 `ReAct` 框架的思路，它来自论文《ReAct：在语言模型中协同推理与行动》(https://arxiv.org/abs/2210.03629)，作者发现让 Agent 执行下一步行动的时候，加上大模型自己的思考过程，并将思考过程、执行的工具及参数、执行的结果放到提示词中，就能使得模型对当前和先前的任务完成度有更好的反思能力，从而提升模型的问题解决能力。

比如 `斯坦福 AI 小镇项目`(https://github.com/joonspk-research/generative_agents)、`AutoGPT`(https://github.com/Significant-Gravitas/AutoGPT)等去年大火的 Agent 的项目，都可以看到 `ReAct` 的思路，`LangChain` 框架中直接以 `ReAct` 为基础定义了几种代表性 Agent（`CONVERSATIONAL_REACT_DESCRIPTION`、`ZERO_SHOT_REACT_DESCRIPTION` 等）。

> `Thought`: ...
>
> `Action`: ...
>
> `Observation`: ...
>
> ...(重复以上过程，即表示 `ReAct` 的工作过程）

## 构建 Agent

下面通过代码构建一个最基本的 Agent，不过这里有必要提前了解几个关键概念，有助于更好地理解 Agent 的工作过程。

+   `AgentAction`：它主要包含两部分信息，`tool` 表示 Agent 将要调用的工具的名称，`tool_input` 表示传递给这个工具的具体输入。

+   `AgentFinish`：它有一个 `return_values` 参数，是一个字典，该字典的 `output` 值表示要返回给用户的字符串信息。

+   `intermediate_steps`：表示 Agent 先前的操作及其相应的结果。它是一个列表，列表中的每个元素是一个包含 `AgentAction` 和其执行结果的元组，这些信息对于未来的决策非常重要，因为它让 Agent 了解到目前为止已经完成了哪些工作。

### 定义工具

定义 Agent 要调用的工具，一个用于获取句子中不同汉字的数量的函数，同时将工具函数绑定到模型上

```
def count_unique_chinese_characters(sentence):
    """用于计算句子中不同汉字的数量"""
    unique_characters = set()
    for char in sentence:
        if '\u4e00' <= char <= '\u9fff':
            unique_characters.add(char)
    return len(unique_characters)

# 将工具函数绑定到模型上
llm_with_tools = llm.bind(functions=[format_tool_to_openai_function(count_unique_chinese_characters)])
```

### 定义 Agent

定义一个 Agent，用于处理用户输入、大模型输出及对输出内容解析：

```
prompt = ChatPromptTemplate.from_messages(
    [
        ("user", "{input}"),
        MessagesPlaceholder(variable_name="agent_output"),
    ]
)

llm = ChatOpenAI(model="gpt-3.5-turbo", temperature=0)
# 构建一个Agent，它将处理输入、提示、模型和输出解析
agent = (
    {
        "input": lambda x: x["input"],
        "agent_output": lambda x: format_to_openai_function_messages(
            x["intermediate_steps"]
        ),
    }
    | prompt
    | llm_with_tools
    | OpenAIFunctionsAgentOutputParser()
)
```

### Agent 执行

下面就是 Agent 的执行过程，主体实际是一个主循环，直到输出预期结果，实际情况中会对循环次数进行控制，以防止无休止执行下去，耗光资源

```
# 用于存储中间结果
intermediate_steps = []
while True:
    # 调用Agent并处理输出
    output = agent.invoke(
        {
            "input": sentence,
            "intermediate_steps": intermediate_steps,
        }
    )
    # 检查是否完成处理，如果完成则退出循环
    if isinstance(output, AgentFinish):
        final_result = output.return_values["output"]
        break
    else:
        print(f"工具名称: {output.tool}")
        print(f"工具输入: {output.tool_input}")
        tool = {"count_unique_chinese_characters": count_unique_chinese_characters}[output.tool]
        observation = tool.run(output.tool_input)
        intermediate_steps.append((output, observation))
# 打印最终结果
print(final_result)
```

一个最基本的 Agent 就构建结束了，这就是当下大家在各种文章上看到的产品层面构建 Agent（代理、智能体、智能代理、AI 代理等）的底层原理，其实很简单。

## 更复杂的任务

仔细观察上面的例子，很显然，只提供了一个工具，而且任务足够简单，当然可以清晰的执行，但是如果面对很复杂的任务场景，任务粒度拆解不细，导致执行步骤无法穷尽，循环就始终无法结束，这样的 Agent 也就无法完成复杂问题了。所以 Agent 要真正可用，任务拆解和规划是极为关键的一步，所以这方面也成为热门研究方向，下面将常见的思路简单介绍下（后台回复【论文】获取论文合集）：

1.  `Zero-Shot`（零样本提示，来自论文《微调后的语言模型是零样本学习者》(https://arxiv.org/abs/2109.01652)）：在提示词中简单地加入“一步一步思考”，引导模型进行逐步推理。

1.  `Few-Shot`（小样本提示，来自论文《语言模型是小样本学习者》(https://arxiv.org/abs/2005.14165)）：给模型展示解题过程和答案，作为样例（如果只提供一个样例，又叫 `One-Shot`），以便于解答新问题。

1.  `COT`（思维链，来自论文《链式思维提示在大型语言模型中引发推理》(https://arxiv.org/abs/2201.11903)），思维链提示即将一个复杂的多步骤推理问题细化为多个中间步骤，然后将这些中间答案组合起来共同解决问题。其有效性已在这篇论文《揭示思维链背后的奥秘：一个理论视角》(https://arxiv.org/abs/2305.15408)中得到验证。

1.  `Auto CoT`（来自论文《大型语言模型中的自动思维链提示》(https://arxiv.org/abs/2210.03493)）：大模型在解题前自动从数据集中查询相似问题进行自我学习，但需要专门的数据集支持。

1.  `Meta CoT`（来自论文《Meta-CoT：在大型语言模型中使用通用链式思考提示应对混合任务场景》(https://arxiv.org/abs/2310.06692)）：在 `Auto CoT` 的基础上，先对问题进行场景识别，进一步优化自动学习过程。

1.  `Least-to-Most`（来自论文从简到难的提示使大型语言模型能够进行复杂推理(https://arxiv.org/abs/2205.10625)）：该策略的核心是把复杂问题划分成若干简易子问题，并依次解决，在处理每个子问题时，前一个子问题的解答有助于下一步。比如提示词中加入“针对每个问题，首先判断是否需分解子问题。若不需，则直接回答；若需，则拆分问题后，整合子问题解答，以得出最优、最全面及最确切的答案。”，启用大模型的思维模式，细化问题，从而获得更佳的结果。

1.  `Self-Consistency CoT`（来自论文自洽性可以提高语言模型中思维链的推理能力(https://arxiv.org/abs/2203.11171)）：在多次输出中选择投票最高的答案。自洽性利用了一个复杂推理问题通常有多种不同的思路，但最终可以得到唯一正确答案的本质，自洽性提升了思维链在一系列常见的算术和常识推理基准测试中的性能，比如在提示词中加入"对于每个问题，你将思考 5 种不同的想法，然后将它们结合起来，输出措辞最佳，最全面和最准确的答案。"

1.  `TOT`（全称 `Tree of Thoughts`，思维树，来自论文《思维树：利用大型语言模型进行深思熟虑的解决问题》(https://arxiv.org/abs/2305.10601)）：构建一个树状结构来存储各步推理过程中产生的多个可能结果作为末梢节点。在进行状态评估以排除无效结果之后，基于这些末梢节点继续进行推理，从而发展出一棵树。接着，利用深度优先搜索（`DFS`）或广度优先搜索（`BFS`）算法连接这些节点，形成多条推理链。最终，将这些推理链提交至一个大模型以评估哪个结果最为合适。

1.  `GOT`（全称 `Graph of Thoughts`，思维图谱，来自论文《思维图谱：用大型语言模型解决复杂问题》( https://arxiv.org/abs/2308.09687)）：思维图谱是将大型语言模型的输出抽象成一个灵活的图结构，其中思考单元作为节点，节点间的连线代表依赖关系。这种方式模拟人类解决问题的思维组合，它能合并多条推理链，自然回溯到有效的推理链，并行地探索独立的推理链，更贴近人类思维方式，从而增强了推理能力。

1.  `Multi-Persona Self-Collaboration`（来自论文《释放大型语言模型中的认知协同：通过多重人格自我协作的任务解决代理》(https://arxiv.org/abs/2307.05300)）：模拟多个角色协作解决问题。

在这些技巧中，`Zero-Shot`、`Few-Shot`、`Self-Consistency` 和 `Least-to-Most` 在提示词层面易于应用且效果显著。

上述论文中的思路已经在很多开源或商业 Agent 产品中采用了，就像我在这篇文章提到的 2024 年，基于大模型的 Agent 如何在企业落地？ 当下的 AI 产品经理必须主动的去读论文。

我创建了【AI 产品】和【AI 开发】交流群，用于 AI 产品构建和分享交流，如果你负责的工作和这个领域强相关并且感兴趣，欢迎后台回复【入群】加入交流，务必备注期望加入群组，否则不予通过！

如果觉得内容不错，欢迎点个关注，分享和在看～
