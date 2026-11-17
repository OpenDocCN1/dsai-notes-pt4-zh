
# LLM应用框架解码之：DSPy

> 来源：[https://agijuejin.feishu.cn/docx/XO8HdBVINoi2GxxEOZkcv1eYnof](https://agijuejin.feishu.cn/docx/XO8HdBVINoi2GxxEOZkcv1eYnof)

> 作者：zijie0 | RandomGenerator

> 原文：https://mp.weixin.qq.com/s/yUL6eo_cio9mOkLhMW65wg

虽然最近半年在持续做 LLM 应用相关的开发工作，不过大家可能也明显感觉到，最近一段时间那种“刷新认知”的技术或应用变化并不多，所以也一直没有足够的激情来写新文章。

## 模型进展

从模型角度来说，自从去年 3 月 GPT-4 发布以来，我们好像没有看到太多令人激动的模型能力的大幅提升。虽然做大模型的人经常会提我们可以期望模型能力很快会有 10 倍的提升，但仔细拆解下，模型能力的提升的方向可能跟我们想的并不太一样。

如果把模型能力分成感知，推理 (reasoning)，生成三个方面，很多做 Agent 应用的同学最关注的还是推理能力。但最近一年模型能力的主要进展似乎集中在感知和生成方面，也就是各种多模态能力的补全。例如 Sora 的视频生成能力，GPT-4o 的语音能力，Suno.ai 的音乐生成等。多模态能力的增强，或许对于交互方式的丰富，更多互动/反馈数据的收集是有帮助的，但对于大家最期待的推理/计划能力的提升，目前看起来还没有什么直接作用。

最近一年还有进步比较大的方面是 long-context 支持的极大幅度提升，要说百倍提升倒也符合。但是如果 inference 成本和延时没有降到足够低，大家在使用 long-context 方面还是会有不少顾虑。而且在 long-context 情况下，可能进一步增加了大家对于强大推理能力的期待。越大的 context 代表的是越复杂越端到端的应用方式，如果推理能力没跟上，那么长的 context 好像用处也不大。

回到推理能力本身，这块的提升的确看起来比之前预想的要更困难一些。在 Opus 发布之后，OpenAI 也迟迟没有公布 GPT-5。业界出现了两种声音：

+ 乐观的一派认为我们已经找到了提升模型推理能力的有效路径，（开始进入科幻发挥环节）比如可能找到了构建长程计划/推理的 synthetic data 的方法，或者是发现了结合 planning 能力的新模型架构，也可能是证明了 easy to hard 泛化的有效性，等等。看 OpenAI 和 Anthropic 的一些访谈里，他们都表示数据应该短期内不是瓶颈，scaling law 依然成立，只不过训练更大的模型需要更长时间的准备工作。

+ 悲观的一派会更多联想到自动驾驶技术，随着数据量的指数级增长，模型能力的增长只是线性的，甚至因为后期有效数据的稀缺性导致能力增长更加平缓。当然也有从自回归 transformer 原理上就不认同这条路线能实现强推理能力的观点。

你更倾向于哪一派呢？

## 应用进展

一开始接触 LLM 的应用开发，大家肯定会对 prompt engineering 感到新奇，好像每天都能看到各种脑洞大开的技巧，从 CoT 到 ToT，从 ReAct 到 AutoGPT，从 RAG [1] 到 MemGPT 等等。但随着模型“智力”能力发展的相对平缓，做应用开发的同学也会陷入一种两难处境：

+ 如果我们泛泛地去理解 scaling law/智能摩尔定律，假定模型能力一定会在接下来两年内提升 100%以上，那么我们是否应该以 agent swarm 的方式来构建应用，各种业务逻辑的串联都可以委托给 agent 自己进行 planning？但当前由于模型能力的限制，这条路径构建出来的应用往往无法达到令人满意的效果。

+ 如果我们想在短期能有些确定性的应用价值产出，那么就不得不由人工来做更多的任务拆分，workflow 搭建的开发工作。这就需要承担一个风险，就是后续模型能力真的提升时，可能前期积累的很多工作成果都变得不再重要。典型的例子如 long-context 大幅提升时，很多之前针对 RAG 做的文档切分，归并总结，多跳召回等精雕细琢都可能被直接抛弃。

![](img/b71d27f0eb6b1924f021aa3b4d7236fe.png)

Flow engineering vs. Agent

所以对于模型能力发展的预判，与当前应用落地路径的选择，是一个需要权衡思考的问题。大家所说的大模型会有 10 倍的提升，到底会出现在哪个方向上？如何提前押注？

## 开个新坑

我们或许也可以先做个假设，未来三年模型推理能力本身没有太大的提升了，但是目前还是有很多确定性相对高的进展，比如 inference 速度可能会越来越快，成本会越来越低，context 会越来越长，多模态的整合会越来越好等。如果我们就把 LLM 当作 system 1 来用，依靠 workflow 的构建来补上 system 2 思考的能力，或许也是一条可行的路径。

在这个假设之下，也还是有不少有趣的项目或许在不远的将来能够成为一种主流的应用形式。后续我也会找一些之前看过的比较有意思的项目来跟大家一起学习交流，分享一些自己的思考。计划的方向有：

+ Agent 框架的介绍，如知名的 AutoGen，CrewAI 等。

+ Agent 应用项目，例如目前比较火的 coding 方向有一堆热门项目，MetaGPT，OpenDevin，SWE-Agent 等。

+ 更广义的 LLM 应用开发框架，比如 LangGraph，DSPy，各种可控生成框架等。

+ Evaluation 相关的项目，这也是实际应用落地中非常关键的一环。

+ Serving，fine-tune 等其它话题。

![](img/7d720b8b8518ed4c95940e0364687a2a.png)

Stack of LLM abstractions

如果大家有什么感兴趣的产品，框架，项目，也可以在评论区里提出。

## Prompt Optimization with DSPy

今天作为开篇，我们先来聊聊 prompt 自动优化相关的工具和框架，核心围绕着斯坦福的知名项目 DSPy [2] 来展开。

### 问题

你在日常开发 LLM 应用的流程是怎么样的？常见的流程一般是：

1. 明确需求，如输入输出内容。

1. 准备几个测试用例。

1. 写一版 prompt。

1. 观察一下测试用例的结果如何，做误差分析。

1. 针对相关问题，修改 prompt，重复实验。

在场景不是很复杂时，可能我们所需要维护的 prompt 数量并不多，这套方法还是可行的。但随着整个 pipeline 越来越复杂，我们可能会面临维护工作量和复杂度的急剧上升：

1. 根据需要进行子任务拆分，每个任务都对应一套 prompt，数量暴涨。

1. 看到新的 prompting 技术，想尝试一下效果。

1. Pipeline 逻辑本身的修改和优化，例如加入 RAG 等。

1. 切换新模型或模型版本。

1. 业务数据出现了“concept drift”。

这时候就会显得手工实验和维护 prompt 体系变得非常脆弱且成本高昂。

### DSPy 的解决思路

仔细观察前面的开发流程，会发现这个过程有点像在做机器学习中的参数优化，只不过是以手动的方式。所以 DSPy 中提出的核心思路就是基于“训练数据”来自动优化整个 pipeline。

![](img/abc83b820d112003523c737b339d9fc7.png)

DSPy 提出的开发流程

图中的一些概念我们后面会再展开详细阐述。为了便于理解，我们可以先类比一下 PyTorch：

+ 评估数据集可以认为就是机器学习中的训练数据，给出了期望的输入和输出样例。

+ LLM 程序就类似于 PyTorch 中定义的神经网络结构。

+ 编译这个词有点微妙，有点像 PyTorch 中的模型训练，但是 LLM 的 pipeline 又没有“反向传播”的学习算法。后面我们再讲这里的具体实现。

+ 编译里面用到的指标可以对应 loss function，优化器可以对应 SGD，Adam 这些。

### 实例

理论上来说，我们在 DSPy 中的工作流程一般是：

1. 收集训练数据。

1. 定义一个 program。

1. 选择评估方式和优化器。

1. `Compile`！

官网上最简单的示例如下：

```
import dspy
from dspy.datasets.gsm8k import GSM8K, gsm8k_metric
from dspy.teleprompt import BootstrapFewShot
from dspy.evaluate import Evaluate

# Set up the LM
turbo = dspy.OpenAI(model=&#x27;gpt-3.5-turbo-instruct&#x27;, max_tokens=250)
dspy.settings.configure(lm=turbo)

# 载入训练数据
gsm8k = GSM8K()
gsm8k_trainset, gsm8k_devset = gsm8k.train[:10], gsm8k.dev[:10]

# 定义 program
class CoT(dspy.Module):
    def __init__(self):
        super().__init__()
        self.prog = dspy.ChainOfThought(&quot;question -&gt; answer&quot;)

    def forward(self, question):
        return self.prog(question=question)

# 定义优化器
config = dict(max_bootstrapped_demos=4, max_labeled_demos=4)
teleprompter = BootstrapFewShot(metric=gsm8k_metric, **config)

# Compile！
optimized_cot = teleprompter.compile(CoT(), trainset=gsm8k_trainset)

# 效果评估
evaluate = Evaluate(devset=gsm8k_devset, metric=gsm8k_metric, num_threads=4, display_progress=True, display_table=0)
evaluate(optimized_cot)
```

是不是很简单很清爽？都看不到哪里可以自己手动写 prompt……

### 原理解析

我们来深入看下 DSPy 中的一些核心概念和其背后的工作原理。

#### Prompt 结构抽象

DSPy 设计背后其实是对 prompt 的结构做了一定的抽象。包含了几个部分：

+ 指令：对于 LLM 要完成任务的说明。

+ 结构描述：告诉 LLM 输入和输出的结构是怎么样的。

+ 样例展示：给 LLM 一些具体的例子。

当然这中间也可以穿插一些 prefix 之类的东西，但总体来说这三部分是核心，会对应到下面一些概念的设计。

#### Signatures

从形式上来看，`signature` 就是一个函数签名，通过半结构化自然语言的方式定义了模块输入和输出的结构。例如：“sentence -> sentiment”，“document -> summary”，“context, question -> answer”，“question, choices -> reasoning, selection”等。因为后续会转化为 prompt，所以这种 LLM 友好的描述方式非常合适。

![](img/8afd50e6f8a992c3ec9f97407d160dab.png)

Signatures

这个 `signature` 后续会转换成 prompt 中最核心的指令与结构描述部分：

![](img/f60682fa5f77ecf9d6b96a84fb666cb9.png)

从 signature 到 prompt

有意思！除了简单的 `string` 定义的 `signature` 外，我们可以用 `Pydantic` model 来定义，更方便加一些额外指令信息，例如：

![](img/1574005d283c4da86914efddd0f19810.png)

Class based signature

这个方法是不是容易联想到 `Instructor` [3] ？DSPy 里的确也支持，后面我们会再提到这点。

#### Modules

DSPy 的 modules 会遵循指定的 `signature`（类比 PyTorch 中 module 处理的不同 tensor shape），接受输入并产生对应输出。跟 PyTorch 中的概念类似，modules 中会带有一些可以优化的“参数”，只不过这里的参数可能是 `signature`，`demos`，`LM` 本身等。另外 modules 也可以串联嵌套组成更大的 module（运行时最终还是一个 DAG）。

最基础的 module 是 `dspy.Predict` ，运作逻辑跟我们上面看到的 `signature` 转化为 prompt，再由 LLM 做生成一致。复杂的 pipeline 可以很方便地通过串联各种 module 来搭建，跟 PyTorch 一样，各种循环、条件控制语句等也都能直接支持。以基础的 RAG 为例：

```
class RAG(dspy.Module):
    def __init__(self, num_passages=3):
        super().__init__()
        self.retrieve = dspy.Retrieve(k=num_passages)
        self.generate_answer = dspy.ChainOfThought(GenerateAnswer)

    def forward(self, question):
        context = self.retrieve(question).passages
        prediction = self.generate_answer(context=context, question=question)
        return dspy.Prediction(context=context, answer=prediction.answer)
```

DSPy 中自带了一些 modules 实现，基本都是常见 prompting 技术的封装，例如 CoT，ReAct 等。以 CoT 的实现为例，我们来看下底层是怎么实现的：

```
class ChainOfThought(Predict):
    def __init__(self, signature, **config):
        super().__init__(signature, **config)
        *_keys, last_key = signature.output_fields.keys()
        # 这里修改了 signature，加入了额外的 CoT 引导
        rationale_type = dspy.OutputField(
            prefix="Reasoning: Let's think step by step in order to",
            desc="${produce the " + last_key + "}. We ...",
        )
        self.extended_signature = signature.prepend("rationale", rationale_type, type_=str)

    def forward(self, **kwargs):
        return super().forward(signature=self.extended_signature, **kwargs)
```

大体上还是挺好理解的，就是修改了原始 `signature`，使得生成 prompt 时会加入额外的 CoT 引导。但这里的代码有所简化，实际上要基于一个新想法来构建一个新 module，目前看还是有一定门槛的。比如这里修改 `signature` 的方式，可能需要对 `signature` 的结构，支持的方法有一定的了解。

Module 的核心基础类 `Predict` 的 `forward` 方法里更是藏了不少玄机。代码比较复杂，简单描述一下：

1. 自身带有 `lm` 的配置，这个在后面讲到 `optimizer` 时会看到，`compile` 中如果选了 fine tune，那么这个 `lm` 会被更新。

1. 各种 LLM 的参数也会记录，不过目前来说还没有哪个 `optimizer` 会去修改它们。

1. 还带有 `demos` 的配置，这里会保存各种样例。这些样例会在 prompt 组装中用到。

1. `Signature` 会转化成 `Template`，然后再生成 prompt，这个流程写得也挺复杂。

1. 输入会转化为 `dsp.Example` ，最后的 LLM 输出也会包装成 `dsp.Example` ，便于实现 module 串联等。

如果需要深入定制一些能力，这里有不少细节需要探究。

前面提到过，`signature` 可以用 `Pydantic` model 来定义。如果想同时实现 module 的输出结果也是 `Pydantic` model，那么可以使用 `TypedPredictor` 。具体可以看文档和代码中的例子，也非常直观。

#### Optimizers
