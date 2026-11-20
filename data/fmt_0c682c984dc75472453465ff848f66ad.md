
# `Agent`开发者坦白：窘境中前行

> 来源：[https://geekbang.feishu.cn/docx/ABn3dnqwOofKKux2l5YcMpman3e](https://geekbang.feishu.cn/docx/ABn3dnqwOofKKux2l5YcMpman3e)

> 👩 作者：产品二姐

> 📃 作者介绍：AI产品经理，AGI应用创业探索中，坚持用原创记录AI应用开发的科技博主，10+大厂、外企产品经理经验。

> ⏰ 收录时间：2024-06-03 16:37:35 (UTC+8)

> 🔗 原文链接：<https://mp.weixin.qq.com/s?__biz=MzUyODY0MTQ4OA==&mid=2247484852&idx=1&sn=07ff2eb63fe4c0a7fd34d70b480f9fe8&chksm=fa6c790bcd1bf01d638fe4f79649893996d54f8bcd0d3619c7cce55387c70e89c89ff2ff16b4&token=1686081273&lang=zh_CN#rd>

![](img/fba94383675841ff99c1bb4a30e761f1.png)

欢迎关注👸号👆

## 1. 引言

上一篇文章 产品经理研读：`Agent`的九种设计模式(图解+代码) 阅读量到了4.1万，大家对`Agent`的热情可见一斑。今天这篇有点泼冷水的意思：如果你正在或准备通过 AI `Agent` 来解决实际问题，不管是产品经理，开发者还是管理者，都需要清醒的意识到以下几点：

1.`Agent` 的规划能力依赖于`prompt` 工程能力，它比想象中更重要、执行起来也更琐碎。

2. 目前 `LLM` 的数学、逻辑推理能力在 `COT` 的基础上也仅能勉强达到及格水平，所以不要让`Agent`一次性做复杂的推理性规划工作，而是把复杂任务人工拆解后再教给`Agent`。

3.`Agent` 的 `Action` 能力强烈依赖于基座模型的 `function calling` 能力。在规划 `Agent` 之前，对模型的 `function calling` 能力要充分调研。

4.十天前加州伯克利大学新鲜出炉了 `function calling` 榜单( `https://gorilla.cs.berkeley.edu/leaderboard.html` ) ，其中表现最好的 `GPT4` 准确率是 86%，还不够理想。

5.`Agent` 的记忆力分为短期记忆和长期记忆。

- 短期记忆由 `prompt`负责，类似 `Plan and resolve` 模式中的 “碎碎念”，告诉 `Agent`已完成了啥，原始目标是啥。

- 在长期记忆中，事实性记忆用`RAG`实现，程序性记忆可用微调实现。

6.`Agent` 的反思能力依赖于它的记忆能力。

而上述几点正好对应着 `Agent` 的四大能力：规划、反省、记忆、执行。我用一张图来表示，其中绿色代表对 `Agent` 开发友好，黄色代表经过一定努力可以达成目标期望，红色代表对 `Agent` 应用开发有一些难以逾越的阻碍因素，需要靠产品降级来解决。

![](img/9b8aef73bb7c2a6d135c5d2056b528a6.png)

我 将这张图命名为 `Agent` 开发者的窘境 ： 一方 面感觉 `LLM` 无所不能、充满希望，另一方面又感觉每个地方都差那么一点点。 但这种窘境之下，总有可以努力的方法来达成目标。 网上有一个段子视频，大概是形容 `Agent` 和人之间合作的情形：`Agent`一往无前，开发者在身后费力把控。

![](img/38a722d1c217249f101519aa0ad82fcd.png)

搞笑之余，它阐明了当下 `Agent` 开发的关键，即：

如何对 `Agent` 有更好的控制力。

这也是本文的核心内容。

## 2. 如何让你的 `Agent` 更可控

开始之前，声明本文所说的 `Agent` 特指具备自主规划，独立执行能力的 `Agent`。这个概念下，类似 `Coze` 这种需要人工搭建工作流的 `Agent`，算是半个 `Agent`，绝没有贬低的意思，只是不在今天的讨论范围内。

### 2.1 概述

下图是一个 `Agent` 的工作过程。

![](img/abfbe623744f48efbb085531b66e365f.png)

`Agent` 的工作过程和人是极其相似的，都要经过 思考→执行→输出， 这三步反复迭代，且每一步都要经过`LLM`。按照 `Agent` 开发窘境里的各种能力，我大概展示出每一步的准确率漏斗，接下来我们看看每一步能做什么以达到 `Agent` 的最高可用性。

### 2.2 对 `Agent` 的思考能力的控制：即 `prompt` 工程优化

为了 `Agent` 开发，我重新回炉了 `Prompt` 工程，有了两个新的认知，也许对你有启发。

1. ”打靶“式的开发者的提示词实践

如果你是用户，写提示词就像玩盲盒: 你对结果有所期待，但不要求确定。你需要了解的是提示词框架，推荐 `WayToAGI` 的框架练习就好： `https://waytoagi.feishu.cn/wiki/AgqOwLxsHib7LckWcN9cmhMLnkb`

如果你是 `Agent` 的开发者，写提示词就像打靶: 你有一个确定的目标，达到目标才能截止。面向 AI 开发者、AI 产品经理，要懂 tech，可以参考: `https://www.promptingguide.ai/zh/techniques/cot`

比如下面这道头部模型公司的面试题中就严格限制了输出的期待，甚至是格式，而你就需要以此靶心设计提示词，并尽量做到稳定发挥。

![](img/1bfd0f2f708558c881cebce57674e0c3.png)

虽 然这道题并非针对规划能力，但打靶这个类比是适用于大多数`Agent`开发场景。 在 `Agent` 规划能力的控制上，我们希望通过提示词打到两个靶心：

+ 规划合乎逻辑。

+ 能合理利用你自己定制的 `tool`，以便进行后续的 `function calling`。

拿如下例子来说明。

![](img/f90994707ee9a207b2b145c1f05e7f9f.png)

(来源： `https://huggingface.co/datasets/chats-bug/agent_action_plan?row=3` )

其中：

+ `GOAL`，`ATTENTION`，`PERFORMANCE EVALUATION` 是依照 `plan and resolve` 模式的模板写的，`GOAL`，`ATTENTION` 体现的是规划能力，`PERFORMANCE EVALUATION` 体现的是反思能力。所有的提示词模板可以在 产品经理研读：`Agent`的九种设计模式(图解+代码) 里提到的提示词样例基础上修改，

+ `ROLE`，`EXAMPLES` 是用来控制 `Agent` 的规划合乎逻辑。

+ `CONSTRAINTS`，`TOOLS` 是用来控制 `Agent` 合理利用 手头工具。

那么这时候就会产生一个不太合理的现象： 每次使用 `Agent`，除了 `INITIALIZATION` 里的 user request 之外，我们 给 大模型 发的都是几乎一样的 `prompt` 。 如果 user request 数量增加，会有巨大的 `token` 消耗。 仔细想想，这种几乎一样的 `prompt` 实际上是一种“程序性记忆”，于是就有了 `Instruction prompting` 来进行固化这种程序性记忆，以降低成本。

2. 固化 `Agent` 程序性记忆—通过 `Instruction prompting` 微调小模型

`Instruction prompting`：中文叫指令微调，主要是通过高质量的数据对基座模型进行微调，提高模型的指令遵循能力。微调的数据中包含：task instruction, input, ground truth output，下图是 `REWOO` 设计模式所使用的指令微调数据，数据集中提供了大约 2000 条数据( 来源： `https://huggingface.co/datasets/rewoo/planner_instruction_tuning_2k` )，经过微调后的模型就可以无需再输入冗长的 `prompt`，从而节约 `token` 成本 。

![](img/40e29a7645c1467b88b0b8195ff0427c.png)

这里顺便再普及一下 `Prompt engineering` 中和 `Instruction prompting` 平行的两个概念：

1.`Hard Prompting` ：在应用层出现，就是常规意义上的 `prompt`：给定已知模型，输出期待的答案，这是人工给定的。

2.`Soft prompting` ：由模型层自动生成，早些时候也叫 `Auto prompt`，需要使用一定量的 `prompt` 样本作为数据对基座模型进行 `prompt` 微调，之后模型就可以将用户的输入转换为新的 `prompt`，这个工程叫 `Soft prompting`。常见的如 `Prompt tuning`，`Prefix tuning`，`P tuning`。（参考`https://huggingface.co/docs/peft/conceptual_guides/prompting`） 。`Soft prompting` 与 `Instruction prompting` 听起来原理差不多，但不同的是：`Soft prompting` 并不改变基座模型的参数，而是将基座模型参数冻结，在其上又增加了少量参数，因此训练成本低，不过只能做一些微小的特定任务。

在上述 `prompt` 工程中，可以总结出 `prompt` 生命周期，

![](img/5b889aa16bd3b6a66777e0f1af57c1e7.png)

也就是说，在产品实验阶段，通过 `hard prompting` 进行验证，上线验证成功后，通过 `Soft prompting` 和 `Instruction following` 进行微调，形成 `Agent` 的程序性记忆，就像健身一样：通过反复练习(`hard prompting`)之后，形成了肌肉记忆(`Instruction`/`Soft prompting`)，这个时候身体的肌肉组织会产生变化（`LLM`参数改变）。

上述就是如何通过 `prompt` 工程来控制 `Agent` 的规划能力。在非推理任务中，通过基本的提示词工程能够达到较为稳定的输出。但在推理类任务(比如类似旅行规划中花费要低于预算（数学），订酒店时不能将非夫妻的异性安排在同一房间等)中，`Prompt` 的规划能力就弱了，大多数论文的准确率在 60% 左右，还达不到商用的标准。当然如果推理场景比较单一，是可以尝试 `COT`+`Few Shot` 看看效果（ 参考 `https://arxiv.org/pdf/2210.11416` ）。

以上就是对 `Agent` 规划能力的控制方法，主要通过 `prompting` 工程来实现，接下来看看如何控制 `Agent` 的执行力。

### 2.3 对 `Agent` 执行能力的控制：选择合适的 `function calling` 模型

模型的 `function calling` 能力是指能否根据规划出来的 `Action` 描述，按照 `tools` 中指定的 API request 格式，生成正确的 request。在加州伯克利大学的 `function calling` 榜单上，他们也给出了 `Gorilla` 模型的 demo，大家可以去试试，左侧是 `Action` 描述和按照 `function calling` 模型指定的 JSON 格式对 API 的描述。点击 `Submit` 后，就可以生成 API request。

![](img/c9202ca73cf13daf809268289f6910c2.png)

可以说，`Agent` 的 执行能力强烈依赖于模型的 `function calling` 能力 ，我们试过同样的 `Action` 描述，在不同的模型中表现完全不同，选择哪个模型是关键因素。从排行榜中，可以看出：

1.模型参数越大，准确率越高 。但 `Berckly` 自己的 `Gorrila` 模型是个例外，仅 6.91B 的参数就可以 PK `GPT4`, `Claude`。不过考虑到这是 `Berckly` 自己在当裁判，结果准确可能会少有偏袒。

2.同时考虑得分和成本，`llama3`，`Mistral Medium`，`Gorilla` 三个是优秀的候选对象。

3.排行榜对国产模型没有评测 。虽然我们自己试了智谱的 `function calling`，但因为样本不足，不予评价，欢迎大家留言，如果有同学想要在国内发表论文，不妨考虑一下这个话题，惠泽众生：）

看起来对于普通开发者，对 `Agent` 的执行能力的控制比较有限，我们能做的就是准确描述 API 的功能、各参数的命名，以及描述。一般在模型厂商的官网上有详细描述。比如：

- OpenAI 的 `function calling` 使用指南 `https://platform.openai.com/docs/guides/function-calling`

- Google 的 `function calling` 使用指南

`https://cloud.google.com/vertex-ai/generative-ai/docs/multimodal/function-calling#function-description-bp`

4. 由于执行准确率有限（评测最高分为 86.76)，充分测试的同时也要考虑到 `function calling` 失败的备选方案，比如：

- 客服案例中，`function calling` 失败直接对接到人工。

- API 参数缺失，考虑根据 API 返回的错误信息，转换成用户语言继续追问用户。

以上是对 `Agent` 执行能力的控制，接下来是比较轻松的 `Agent` 输出能力控制，实际上就是 `function calling` 的反向能力，即：将 API 返回的 JSON response 正确解读为自然语言，这比 `function calling` 要简单，就像读答案比问问题要简单一些。

### 2.4 对 `Agent` 的输出能力控制：API 增强生成的 `prompt` 工程

到输出的阶段，`prompt` 中将 `API` response 加上其他上下文一并教给大模型，`API` 的 response 中各字段的名字和描述可以作为补充，大体上就能生成合理的答案，这一步骤的准确率很高，大概 95%，就不再赘述。

## 3. 总结

在写这篇文章之前，有几位同学顺着上一篇文章 产品经理研读：`Agent`的九种设计模式(图解+代码) 找过来，和我探讨他们在 `Agent` 开发过程中的问题。和大家一样，我也在起步阶段。在各种论文结果、产品效果、市场宣传的媒体环境中，大家对 `Agent` 充满热情，同时也充斥着焦虑，这一篇有点让大家冷静一下的意思。不过在我看来，这是件好事，正因为如此，那些坚持长线积累的产品才有机会跨越周期，做出真正好用的 `Agent` 产品。
