
# 多智能体（Multi-Agent）系统是怎么一回事？

> 来源：[https://agijuejin.feishu.cn/docx/Cd6cdKjSaoPrhSxeEz4cxPEXnNf](https://agijuejin.feishu.cn/docx/Cd6cdKjSaoPrhSxeEz4cxPEXnNf)

> 作者：猕猴桃 | 探索AGI

> 原文：[https://mp.weixin.qq.com/s/E0HoYJ1_HO_TS8ZTBu6rZw](https://mp.weixin.qq.com/s/E0HoYJ1_HO_TS8ZTBu6rZw)

嘿，大家好！这里是一个专注于AI智能体的频道！

今天来简单聊聊Multi-agent系统，明天会分享一个简单的多智能体系统实战～。Multi-Agent系统到底是啥？想象一下：如果有一个团队，每个成员都有自己的角色和任务，但又能互相协作，共同完成一个目标，那会是怎样的体验？Multi-Agent系统就是这样一个概念。在AI的世界里，每个智能体都有自己的个性和角色，它们通过语言模型（LLM）来驱动，相互之间还能交流协作。

那Multi-Agent系统怎么协作呢？每个智能体都有自己的任务和上下文，它们可以访问各种工具来执行任务。比如，一个智能体生成代码，另一个智能体审查代码，然后它们可以相互讨论，共同改进代码。经过几轮迭代，就能得到最佳结果。这种方式不仅能产生更令人满意的输出，还能减少幻觉（Hallucinations）和偏见等问题。

接下来，让我们聊聊Multi-Agent系统的优点。首先是分工明确，每个智能体都有自己的任务和指令，由不同的语言模型支持，专注于自己的工作，效率自然高。这种模块化意味着复杂问题可以被分解成小块，由专门的智能体和模型来处理。这样，每个智能体都能独立评估和改进，不会影响整个系统。同时，不同的观点混合在一起，我们可以提炼输出并避免偏见。最后，一旦Multi-Agent系统建立起来，就可以在不同的场景下重复使用，形成一个智能体生态系统。

![](img/2286b67abaa3151b69cbeeb67c06633d.png)

一个标准的Multi-Agent系统架构包括：

+ Agent：有明确角色和上下文的智能体，运行在LLM上。
+ Connections：智能体之间如何连接。
+ Orchestration：定义智能体如何协同工作。
+ Human：大多数情况下，我们需要人参与决策和评估结果。
+ Tools：智能体用来执行特定任务的工具。
+ LLM：所有这些都依赖于特定的大模型进行推理。

现在市面上有很多框架可以帮助我们构建multi-agent应用，比如OpenAI Assistant、Autogen、Dragonscale的multi-agent系统、CrewAI和LangGraph。这些框架每天都在进化，新的框架也在不断涌现，但它们都有一个共同点——让构建和管理多个智能体变得更简单。

好了，这就是我今天想分享的内容。如果你对构建AI智能体感兴趣，别忘了点赞、关注噢~
