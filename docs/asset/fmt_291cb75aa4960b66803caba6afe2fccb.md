
+ 除了全自动化执行，作者们也在 `critic` 和 `curriculum` 两个模块引入了可选的人工介入机制，帮助 `agent` 更好地处理程序错误，以及引导他们一步步做更复杂的任务。Human as a tool 可能也会成为一种流行模式。

![](img/62acbe763882263f2d3c7604f443b02e.png)

## Skill library 的实现

这几个模块结合起来就达到了上面的效果，看起来都没有用 `MineDojo` 里收集的额外数据，也不用做任何训练，非常神奇。

在 limitation 环节，作者提到了 `agent` 会出现无法实现正确的代码以及 hallucination 的问题，未来或许能通过 GPT 自身能力的提升或者通过微调开源 LLM 来克服。我的一个感觉是好像整个项目还没有把 minecraft 相关的文档以及其它用户经验分享利用起来，这些知识应该可以减少一些出错和 hallucination 的问题？

还有一个思考是，这类无梯度架构的能力边界会在哪里？有哪些效果是必须通过模型的参数训练更新才能达到的？目前看起来很多 `fine tune` 提供的优势好像主要是效率角度的提升。例如无梯度架构的模型每次都要从小本本里去翻找相关记忆，但训练过的模型可以直接从参数中找出 `context`；或者本来要用工具去跑一下代码才知道结果，训练过的模型可以直接给出结果；或者原先的任务做的不熟练，经常做错需要订正，训练过后就能一次做对了。是否有无梯度架构完全做不了的，而一定要用数据训练后才能做到的事情（例如扩展词表）？

不过即使从提升效率，增加准确性的角度来看，这两者也是可以考虑结合起来的。比如 Generative Agents 里就有“自我反思”的步骤来沉淀重要的事件信息。我们是不是也可以延展一下，把“自我反思”改成调用自我 `fine tune` 的工具。其中的训练数据就可以根据上面提到的无梯度架构的局限性角度出发，将出错最多的代码，`retrieve` 频率最高的信息，调用最频繁的工具及其结果，人工介入最多的 `workflow`，以及模型自认为最重要的信息等等，都作为训练数据，来训练一个 `fine tune` 的模型。这样就可以在无梯度架构的基础上，通过 `fine tune` 来进一步提升 `agent` 的进化。

## Tree of Thought

`ReAct` 作者“尧舜禹”大佬的新作，前两天又刷屏了。作者延展了 chain-of-thought 的思路，模拟人类在解决复杂问题时的提出多个可能方向，思考深挖，根据可行性选择路径，碰到问题时回溯到之前其它想法等思维过程，构建起了一棵自主探索生长的“思维树”。在算 24 点等一系列复杂的任务上，`tree-of-thought` 的表现都大大超过了之前的 `chain-of-thought`。

![](img/b58be7a3d77f04fa5907b7e322f66109.png)

直观理解 tree-of-thought

作者也将代码公开在了 `GitHub` 上，感兴趣的同学可以深入学习研究。个人看下来感觉目前构建 tree 的 proposal/value prompt 需要针对具体任务来设计，需要比较深入的领域知识。不知是否有可能通过更加自动化的方式，结合 LLM 和相关外部工具来自动生成这些 prompt，那样或许会有更广的应用场景。

![](img/1cd790cb9ba5cc27eb2f8974c4e22f55.png)

BFS ToT 的核心逻辑

这个工作的设计思路还是挺有趣的，有种把 LLM 作为 CPU，在上面跑了更高级的树搜索算法的感觉。很多经典算法都是这类对人类系统性思考解决某类问题的高度抽象，或许未来会有更多这个方向上的探索，甚至真的把 Prompt 设计成一种编程语言 [14] 。

## Status of GPT

最后再推荐一下推特第一人工智能网红 Karpathy 在微软 Build 大会上的演讲，如果你没怎么跟进 LLM 方面的进展，那么这个演讲绝对是最好的总结视频之一。表述非常清晰易懂，覆盖了各类主要进展，有细节也有 intuition，真是不可多得的佳作。个人感觉收获比较多的几个点包括：

+ 为什么需要 `RLHF`，直观来看就是效果更好。一个可能的原因是要生成一个高质量的文本，比判别一下几个文本质量的好坏要难得多。“我虽然不会写，但点评下别人的作品还是容易的” :)

+ 模型 `fine tune` 会损失多样性，这个之前 Karpathy 也在推特上分享过。包括之前的“灾难性遗忘”感觉也没有很好的解决办法。

+ 人工生成的文本跟 LLM 生成文本的比较非常精彩，一定要看。人类在产出最终文本前其实涉及了很多中间过程，可能包括问题拆解，资料查阅，反思，撰写，润色等等，而模型没有各种内在的心理活动，它的所有思考过程都需要一个一个 `token` 显示地生成出来。所以上面看到的很多工作也都是利用 prompting 和模型生成的交互来重建人类的 system 2 思考过程。

+ LLM 只想模仿，不想成功。所以提醒一下模型模仿高智商的思考者很重要，让它记得需要完成的任务目标也很重要。举例：`Let's work this out in a step by step way to be sure we have the right answer`。

+ 提到了微软的 `Guidance` [15] 项目，实现了 constrained prompting，让模型能够更稳定地按照特定的格式进行输出。

+ `fine tune` 方面提到了各种 `PEFT` 技术 [16] ，这方面最近最火热的应该是这个 `QLoRA` [17] ，可以在 48GB 显存上 `fine tune` 一个 65B 的模型，着实惊人。

![](img/544004bcd411f3752f1b51954ef0de45.png)

第 23 分钟左右的精华部分

今天的分享就到这里啦，happy hacking！

### 参考资料

[1] LLM 应用开发全栈指南: https://zhuanlan.zhihu.com/p/629589593

[2] How to Train LLMs from Scratch: https://wandb.ai/site/llm-whitepaper

[3] MosaicML: https://www.mosaicml.com/

[4] The-Stack-Dedup: https://huggingface.co/datasets/bigcode/the-stack-dedup

[5] SentencePiece: https://github.com/google/sentencepiece

[6] HumanEval: https://huggingface.co/datasets/openai_humaneval

[7] FastTransformer: https://github.com/NVIDIA/FasterTransformer

[8] Triton: https://github.com/triton-inference-server/server

[9] AutoGPT 与 LLM Agent 解析: https://zhuanlan.zhihu.com/p/622947810

[10] Gorilla: https://arxiv.org/pdf/2305.15334.pdf

[11] LangChain 文档中的例子: https://python.langchain.com/en/latest/modules/agents/tools/examples/human_tools.html

[12] 开源了代码: https://github.com/ShishirPatil/gorilla

[13] MineDojo: https://minedojo.org/

[14] Prompt 设计成一种编程语言: https://lmql.ai/

[15] Guidance: https://github.com/microsoft/guidance

[16] PEFT 技术: https://github.com/huggingface/peft

[17] QLoRA: https://github.com/artidoro/qlora
