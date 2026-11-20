
# 吴恩达：从 Agent 到 Agentic，超越基础模型的下一代 AI

> 来源：[https://agijuejin.feishu.cn/docx/CRkRdG9TYoS1jwxeQB1cn70jnid](https://agijuejin.feishu.cn/docx/CRkRdG9TYoS1jwxeQB1cn70jnid)

> 作者：非子爱 | 子非AI

> 原文：`https://mp.weixin.qq.com/s/Gl74YZn4ylxSHAkwUFB-FA`

![](img/88068179087afb7bc10dbefc6de021ba.png)

## Agentic AI：超越基础模型的下一代 AI — 来自吴恩达的洞察

> “与其争论哪些工作才算是真正的 `Agent`，不如承认系统可以具有不同程度的 `Agentic` 特性。” —— 吴恩达

2024年 Snowflake 峰会开发者日上，人工智能领域的领军人物吴恩达 (Andrew Ng) 发表了题为“AI 代理工作流及其推动 AI 进展的潜力 (How Agentic workflows could drive more AI progress than even the next generation of foundation models)”的演讲，为我们揭开了 `Agentic AI` 的神秘面纱，并指出这可能是比下一代基础模型更具潜力的 AI 发展方向。

## Agentic AI：从 Agent 到 Agentic，开启 AI 新纪元

近年来，大型语言模型 (LLM) 如 GPT-3、GPT-4 等的出现，为人工智能领域带来了前所未有的突破。然而，传统的 LLM 交互方式更像是一种“非代理型工作流程 (`Non-agentic workflow (zero-shot)`)”，用户输入指令，模型输出结果，缺乏迭代优化的空间，犹如一位才华横溢的作家，却被迫只能按照固定模板写作，无法自由挥洒其创作才能。

![](img/70e3463e1d0539fe916447b17a403794.png)

`Agentic AI` 的出现打破了这一僵局，它不再将 AI 系统视为被动接收指令的 `Agent`，而是赋予其主动思考、规划和执行任务的能力，使其更像是一个能够自主决策的智能体。而实现这一目标的关键，就是“代理型工作流程 (`Agentic workflow)`”。

1. **目标设定：**明确任务目标，例如“写一篇关于 `Agentic AI` 的文章”。
2. **规划分解：**将任务分解成多个子任务，例如“确定主题、搜集资料、撰写内容、修改润色”等。
3. **迭代执行：**依次执行每个子任务，并根据反馈结果进行调整和优化，最终完成目标。

### `LLM-based agents`：`Agentic Workflow` 大显身手的舞台

吴恩达在演讲中，特别以“基于 LLM 的智能体 (`LLM-based agents`)”为例，展示了 `Agentic Workflow` 如何提升 AI 系统的性能表现。

传统的 LLM 交互方式类似于“一次性 prompting”，用户输入一个指令，模型生成一个输出，缺乏迭代优化的空间，难以处理复杂的任务。而 `Agentic Workflow` 则将 LLM 视为一个智能代理，通过多轮对话和反馈机制，引导 LLM 逐步完善输出结果。

为了验证 `Agentic Workflow` 的效果，吴恩达的团队进行了一项基于代码生成基准测试集 `HumanEval` (`Coding Benchmark (HumanEval)`) 的实验。

![](img/6a8d76307f6efd169e2677e7d25988af.png)

实验结果表明，即使是性能相对较弱的 GPT-3.5 模型，在采用 `Agentic Workflow` 后，其代码生成性能也能超过 GPT-4。下图展示了不同模型在 `HumanEval` 基准测试中的性能对比：

![](img/75379734aa8230f6d402ec05250e2bc3.png)

从图中可以看出，采用 `Agentic Workflow` 的模型 (橙色点) 普遍取得了优于 `Zero-shot` 方法的性能表现。这充分证明了 `Agentic AI` 在突破性能瓶颈方面的巨大潜力。

## `Agentic` vs `Agent`：一场认知的跃迁

在理解 `Agentic AI` 的过程中，我们需要厘清一个重要的概念区别：`Agent` 和 `Agentic`。

吴恩达在其博客文章中精辟地指出，`Agent` 是一个名词，意味着一种非黑即白的二元划分，而 `Agentic` 则是一个形容词，代表着一种程度概念。

传统的 AI 系统，例如我们熟悉的机器学习算法，大多可以被归类为 `Agent`。它们接收输入，根据预设的规则进行处理，最终输出结果。然而，`Agentic AI` 则更进了一步，它不再局限于被动执行指令，而是能够主动感知环境、理解目标，并自主选择行动方案，表现出不同程度的自主性和智能性。

这种从 `Agent` 到 `Agentic` 的转变并非一蹴而就，而是一个渐进式的演化过程。正如机器学习领域的发展历程，从早期的线性回归到如今的深度学习，`Agentic AI` 也需要经历不断的迭代和优化，才能最终实现其全部潜力。

更重要的是，`Agentic` 这一概念的提出，意味着我们不再纠结于 AI 系统是否真正具备了“智能”，而是将关注点放在了系统能够表现出多少“智能”上。这是一种认知上的重大转变，也是 `Agentic AI` 区别于传统 AI 的本质特征。

## `Agentic AI` 的应用：从代码生成到视觉任务，重塑各个领域

### `Vision Agent`: 让 AI 看懂世界

在 Snowflake 峰会的演讲中，吴恩达还展示了他的团队开发的一款名为“`Vision Agent`”的视觉智能体。

![](img/f78d034f0dac8fd3aef687d3a67a9eef.png)

`Vision Agent` 可以像一位经验丰富的程序员一样，根据用户的自然语言指令编写代码，完成各种视觉任务，例如目标检测、图像分割、视频分析等。

例如，用户可以输入指令“帮我找到所有带红色帽子的人”，`Vision Agent` 就会自动编写代码，识别图像或视频中所有符合条件的目标。

![](img/80b3f8a75d3f8791c810ebaaea96e631.png)

`Vision Agent` 的工作流程如下：

1. **接收指令：**用户输入自然语言指令，例如“计算鲨鱼和最近的冲浪板之间的距离”。
2. **生成代码：**`Vision Agent` 根据指令自动生成代码，完成图像处理和分析任务。
3. **执行代码：**代码被执行，并输出结果，例如鲨鱼和冲浪板之间的距离。

![](img/9e54342eccbd88cf0639bbd718fa454f.png)

`Vision Agent` 的核心是一个名为“`Coder Agent`”的模块，它负责将自然语言指令转换为可执行的代码。

![](img/7c1fbcf9a77573e4d8b50e4b9ea808d5.png)

`Coder Agent` 的工作原理如下：

1. **规划：**根据指令，制定一个计划，列出完成任务所需的步骤。
2. **检索工具：**为每个步骤检索所需的工具，例如图像处理函数、目标检测模型等。
3. **生成代码：**将计划和工具组合成可执行的代码。

为了进一步提高代码的质量和可靠性，`Vision Agent` 还引入了一个名为“`Tester Agent`”的模块，它负责对 `Coder Agent` 生成的代码进行测试和评估。

![](img/ccdf6a0124c883715bc32b3b9d7d9c2c.png)

`Tester Agent` 的工作原理如下：

1. **执行代码：**执行 `Coder Agent` 生成的代码。
2. **检查结果：**检查代码的执行结果是否符合预期。
3. **反馈错误：**如果发现错误，将错误信息反馈给 `Coder Agent`，以便其进行修正。

通过 `Coder Agent` 和 `Tester Agent` 的协同工作，`Vision Agent` 能够自动生成高质量的代码，并完成各种视觉任务。

以下是一些 `Vision Agent` 的应用示例：

- 检测图像中的人脸，并判断是否佩戴口罩：
- 分析视频，识别交通事故：

![](img/a3f9d274114ba222aec6849e9d995004.png)

## 吴恩达的呼吁：拥抱 `Agentic AI`，共创智能未来

吴恩达认为，`Agentic AI` 的出现是人工智能领域的一场重大变革，它将从根本上改变我们与 AI 的交互方式，也将为人类社会带来巨大价值。

他呼吁开发者积极拥抱 `Agentic AI`，探索其应用边界，共同推动 AI 技术的发展。同时，他也提醒人们关注 `Agentic AI` 可能带来的伦理和社会影响，例如算法偏见、隐私泄露等问题，并呼吁各界共同努力，确保 AI 技术安全可控地发展，为人类创造更加美好的未来。

## 参考资料

- Andrew Ng 在 2024 Snowflake 峰会开发者日上的演讲：`https://www.youtube.com/watch?v=q1XFm21I-VQ`
