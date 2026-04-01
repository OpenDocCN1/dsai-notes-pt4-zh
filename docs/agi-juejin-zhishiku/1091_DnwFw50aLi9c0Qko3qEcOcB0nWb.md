# LLM应用框架解码之：DSPy

> 来源：[https://agijuejin.feishu.cn/docx/XO8HdBVINoi2GxxEOZkcv1eYnof](https://agijuejin.feishu.cn/docx/XO8HdBVINoi2GxxEOZkcv1eYnof)

> 作者：zijie0 | RandomGenerator

> 原文：https://mp.weixin.qq.com/s/yUL6eo_cio9mOkLhMW65wg

虽然最近半年在持续做 LLM 应用相关的开发工作，不过大家可能也明显感觉到，最近一段时间那种“刷新认知”的技术或应用变化并不多，所以也一直没有足够的激情来写新文章。

# ** 模型进展**

从模型角度来说，自从去年 3 月 GPT-4 发布以来，我们好像没有看到太多令人激动的模型能力的大幅提升。虽然做大模型的人经常会提我们可以期望模型能力很快会有 10 倍的提升，但仔细拆解下，模型能力的提升的方向可能跟我们想的并不太一样。

如果把模型能力分成感知，推理 (reasoning)，生成三个方面，很多做 Agent 应用的同学最关注的还是推理能力。但最近一年模型能力的主要进展似乎集中在感知和生成方面，也就是各种多模态能力的补全。例如 Sora 的视频生成能力，GPT-4o 的语音能力，Suno.ai 的音乐生成等。多模态能力的增强，或许对于交互方式的丰富，更多互动/反馈数据的收集是有帮助的，但对于大家最期待的推理/计划能力的提升，目前看起来还没有什么直接作用。

最近一年还有进步比较大的方面是 long-context 支持的极大幅度提升，要说百倍提升倒也符合。但是如果 inference 成本和延时没有降到足够低，大家在使用 long-context 方面还是会有不少顾虑。而且在 long-context 情况下，可能进一步增加了大家对于强大推理能力的期待。越大的 context 代表的是越复杂越端到端的应用方式，如果推理能力没跟上，那么长的 context 好像用处也不大。

回到推理能力本身，这块的提升的确看起来比之前预想的要更困难一些。在 Opus 发布之后，OpenAI 也迟迟没有公布 GPT-5。业界出现了两种声音：

+   乐观的一派认为我们已经找到了提升模型推理能力的有效路径，（开始进入科幻发挥环节）比如可能找到了构建长程计划/推理的 synthetic data 的方法，或者是发现了结合 planning 能力的新模型架构，也可能是证明了 easy to hard 泛化的有效性，等等。看 OpenAI 和 Anthropic 的一些访谈里，他们都表示数据应该短期内不是瓶颈，scaling law 依然成立，只不过训练更大的模型需要更长时间的准备工作。

+   悲观的一派会更多联想到自动驾驶技术，随着数据量的指数级增长，模型能力的增长只是线性的，甚至因为后期有效数据的稀缺性导致能力增长更加平缓。当然也有从自回归 transformer 原理上就不认同这条路线能实现强推理能力的观点。

你更倾向于哪一派呢？

# ** 应用进展**

一开**始接触 LLM **的应用开发，大家肯定会对 prompt engineering 感到新奇，好像每天都能看到各种脑洞大开的技巧，从 CoT 到 ToT，从 ReAct 到 AutoGPT，从 RAG [1] 到 MemGPT 等等。但随着模型“智力”能力发展的相对平缓，做应用开发的同学也会陷入一种两难处境：

+   如果我们泛泛地去理解 scaling law/智能摩尔定律，假定模型能力一定会在接下来两年内提升 100%以上，那么我们是否应该以 agent swarm 的方式来构建应用，各种业务逻辑的串联都可以委托给 agent 自己进行 planning？但当前由于模型能力的限制，这条路径构建出来的应用往往无法达到令人满意的效果。

+   如果我们想在短期能有些确定性的应用价值产出，那么就不得不由人工来做更多的任务拆分，workflow 搭建的开发工作。这就需要承担一个风险，就是后续模型能力真的提升时，可能前期积累的很多工作成果都变得不再重要。典型的例子如 long-context 大幅提升时，很多之前针对 RAG 做的文档切分，归并总结，多跳召回等精雕细琢都可能被直接抛弃。

![](img/b71d27f0eb6b1924f021aa3b4d7236fe.png)

Flow engineering vs. Agent

所以对于模型能力发展的预判，与当前应用落地路径的选择，是一个需要权衡思考的问题。大家所说的大模型会有 10 倍的提升，到底会出现在哪个方向上？如何提前押注？

# ** 开个新坑**

我们或许也可以先做个假设，未来三年模型推理能力本身没有太大的提升了，但是目前还是有很多确定性相对高的进展，比如 inference 速度可能会越来越快，成本会越来越低，context 会越来越长，多模态的整合会越来越好等。如果我们就把 LLM 当作 system 1 来用，依靠 workflow 的构建来补上 system 2 思考的能力，或许也是一条可行的路径。

在这个假设之下，也还是有不少有趣的项目或许在不远的将来能够成为一种主流的应用形式。后续我也会找一些之前看过的比较有意思的项目来跟大家一起学习交流，分享一些自己的思考。计划的方向有：

+   Agent 框架的介绍，如知名的 AutoGen，CrewAI 等。

+   Agent 应用项目，例如目前比较火的 coding 方向有一堆热门项目，MetaGPT，OpenDevin，SWE-Agent 等。

+   更广义的 LLM 应用开发框架，比如 LangGraph，DSPy，各种可控生成框架等。

+   Evaluation 相关的项目，这也是实际应用落地中非常关键的一环。

+   Serving，fine-tune 等其它话题。

![](img/7d720b8b8518ed4c95940e0364687a2a.png)

Stack of LLM abstractions

如果大家有什么感兴趣的产品，框架，项目，也可以在评论区里提出。

#  Prompt Optimization with DSPy

今天作为开篇，我们先来聊聊 prompt 自动优化相关的**工具和框架，核心围**绕着斯坦福的知名项目 DSPy [2] 来展开。

## ** 问题**

你在日常开发 LLM 应用的流程是怎么样的？常见的流程一般是：

1.  明确需求，如输入输出内容。

1.  准备几个测试用例。

1.  写一版 prompt。

1.  观察一下测试用例的结果如何，做误差分析。

1.  针对相关问题，修改 prompt，重复实验。

在场景不是很复杂时，可能我们所需要维护的 prompt 数量并不多，这套方法还是可行的。但随着整个 pipeline 越来越复杂，我们可能会面临维护工作量和复杂度的急剧上升：

1.  根据需要进行子任务拆分，每个任务都对应一套 prompt，数量暴涨。

1.  看到新的 prompting 技术，想尝试一下效果。

1.  Pipeline 逻辑本身的修改和优化，例如加入 RAG 等。

1.  切换新模型或模型版本。

1.  业务数据出现了“concept drift”。

这时候就会显得手工实验和维护 prompt 体系变得非常脆弱且成本高昂。

## ** DSPy 的解决思路**

仔细观察前面的开发流程，会发现这个过程有点像在做机器学习中的参数优化，只不过是以手动的方式。所以 DSPy 中提出的核心思路就是基于“训练数据”来自动优化整个 pipeline。

![](img/abc83b820d112003523c737b339d9fc7.png)

DSPy 提出的开发流程

图中的一些概念我们后面会再展开详细阐述。为了便于理解，我们可以先类比一下 PyTorch：

+   评估数据集可以认为就是机器学习中的训练数据，给出了期望的输入和输出样例。

+   LLM 程序就类似于 PyTorch 中定义的神经网络结构。

+   编译这个词有点微妙，有点像 PyTorch 中的模型训练，但是 LLM 的 pipeline 又没有“反向传播”的学习算法。后面我们再讲这里的具体实现。

+   编译里面用到的指标可以对应 loss function，优化器可以对应 SGD，Adam 这些。

## ** 实例**

理论上来说，我们在 DSPy 中的工作流程一般是：

1.  收集训练数据。

1.  定义一个 program。

1.  选择评估方式和优化器。

1.  Compile！

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

## ** 原理解析**

我们来深入看下 DSPy 中的一些核心概念和其背后的工作原理。

### ** Prompt 结构抽象 **

DSPy 设计背后其实是对 prompt 的结构做了一定的抽象。包含了几个部分：

+   指令：对于 LLM 要完成任务的说明。

+   结构描述：告诉 LLM 输入和输出的结构是怎么样的。

+   样例展示：给 LLM 一些具体的例子。

当然这中间也可以穿插一些 prefix 之类的东西，但总体来说这三部分是核心，会对应到下面一些概念的设计。

### ** Signatures **

从形式上来看，signature 就是一个函数签名，通过半结构化自然语言的方式定义了模块输入和输出的结构。例如：“sentence -> sentiment”，“document -> summary”，“context, question -> answer”，“question, choices -> reasoning, selection”等。因为后续会转化为 prompt，所以这种 LLM 友好的描述方式非常合适。

![](img/8afd50e6f8a992c3ec9f97407d160dab.png)

Signatures

这个 signature 后续会转换成 prompt 中最核心的指令与结构描述部分：

![](img/f60682fa5f77ecf9d6b96a84fb666cb9.png)

从 signature 到 prompt

有意思！除了简单的 string 定义的 signature 外，我们可以用 Pydantic model 来定义，更方便加一些额外指令信息，例如：

![](img/1574005d283c4da86914efddd0f19810.png)

Class based signature

这个方法是不是容易联想到 **Instructor [3] **？DSPy 里的确也支持，后面我们会再提到这点。

### ** Modules **

DSPy 的 modules 会遵循指定的 signature（类比 PyTorch 中 module 处理的不同 tensor shape），接受输入并产生对应输出。跟 PyTorch 中的概念类似，modules 中会带有一些可以优化的“参数”，只不过这里的参数可能是 signature，demos，LM 本身等。另外 modules 也可以串联嵌套组成更大的 module（运行时最终还是一个 DAG）。

最基础的 module 是 dspy.Predict ，运作逻辑跟我们上面看到的 signature 转化为 prompt，再由 LLM 做生成一致。复杂的 pipeline 可以很方便地通过串联各种 module 来搭建，跟 PyTorch 一样，各种循环、条件控制语句等也都能直接支持。以基础的 RAG 为例：

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
            prefix=&quot;Reasoning: Let&#x27;s think step by step in order to&quot;,
            desc=&quot;${produce the &quot; + last_key + &quot;}. We ...&quot;,
        )
        self.extended_signature = signature.prepend(&quot;rationale&quot;, rationale_type, type_=str)

    def forward(self, **kwargs):
        return super().forward(signature=self.extended_signature, **kwargs)
```

大体上还是挺好理解的，就是修改了原始 signature，使得生成 prompt 时会加入额外的 CoT 引导。但这里的代码有所简化，实际上要基于一个新想法来构建一个新 module，目前看还是有一定门槛的。比如这里修改 signature 的方式，可能需要对 signature 的结构，支持的方法有一定的了解。

Module 的核心基础类 Predict 的 forward 方法里更是藏了不少玄机。代码比较复杂，简单描述一下：

1.  自身带有 lm 的配置，这个在后面讲到 optimizer 时会看到，compile 中如果选了 fine tune，那么这个 lm 会被更新。

1.  各种 LLM 的参数也会记录，不过目前来说还没有哪个 optimizer 会去修改它们。

1.  还带有 demos 的配置，这里会保存各种样例。这些样例会在 prompt 组装中用到。

1.  Signature 会转化成 Template，然后再生成 prompt，这个流程写得也挺复杂。

1.  输入会转化为 dsp.Example ，最后的 LLM 输出也会包装成 dsp.Example ，便于实现 module 串联等。

如果需要深入定制一些能力，这里有不少细节需要探究。

前面提到过，signature 可以用 Pydantic model 来定义。如果想同时实现 module 的输出结果也是 Pydantic model，那么可以使用 TypedPredictor 。具体可以看文档和代码中的例子，也非常直观。

### ** Optimizers **

前面铺垫了这么多，终于到了传说中的自动优化部分了。优化的前提是可以进行评估，所以这里所有介绍的方法都需要借助预先构建好的评估数据集和相关的评估方法（evaluation metric）。相比神经网络的优化来说，这里我们可以使用任意的评估方法，因为这里不涉及到梯度计算、反向传播之类的方法。DSPy 中内置了一些评估方法，如 dspy.evaluate.metrics.answer_exact_math ， dspy.evaluate.metrics.answer_passage_match 等。当然我们也可以自己动手写，包括用 LLM 来评估也可以，非常自由。

从可以优化的参数来说，我们编写的任何一个 LLM pipeline 程序都有很多形式各异的“旋钮”，而不仅仅是模型权重这样相对统一的形式。例如：

1.  Prompt 模板中固定的指令描述。

1.  Prompt 中给出的示范样例。

1.  LLM 的选择，不同的参数设置。

1.  LLM 的 fine tune 及相应参数。

1.  Pipeline 中相关模块的参数，最典型的如 retrieval 模块的配置。

1.  Pipeline 本身的结构，例如是否拆分任务，是否使用某些特定的循环控制流程等。

目前看 DSPy 中已经实现了上述提到的 1，2，4 这几块。借用推特上的一张图，大概效果是：

![](img/4c8a04ae3e7444c4ad1d76a6f6455442.png)

DSPy Compiler

这个效果非常神奇，回想一下前面的例子中，调用这个优化过程仅仅是简单的两行代码：

```
teleprompter = BootstrapFewShot(metric=gsm8k_metric, **config)
optimized_cot = teleprompter.compile(CoT(), trainset=gsm8k_trainset)
```

让我们来看看这个编译过程具体是如何实现的，接下来会挑选几个最有代表性的优化器来讲解。

#### ** Prompt 中的样例优化 **

DSPy 在这方面最典型的优化方法是 BootstrapFewShot 。其工作步骤如下：

1.  首先初始化“学生”和“老师”两个 LLM 程序。从代码来看，两者必须拥有相同的 pipeline 结构，所以可能的区别就是可以使用不同的 LLM？没法实现用一个更复杂的 pipeline 来“蒸馏”出一个简单的 pipeline 来……

1.  把初始的训练数据交给“老师”，形成 raw_demos。

1.  对于每个训练样本，“老师”都会尝试去生成预测。注意这个预测可以是个复杂的过程，比如先生成 reasoning，再生成 answer。

1.  检查“老师”生成的预测，如果正确，整个预测的 trace 会被加入到 augmented_demos 里。这里可以实现一个效果，如果原始样本里只有问题和回答，而我们在优化的是一个 CoT module。这个 bootstrap 的过程会把最终回答正确的那些样例保存下来，同时还把 reasoning 的内容也补上了！同理，如果是 RAG module，也能自动补上召回的 context 内容作为样例的一部分，虽然这样也会导致样例比较庞大……

1.  最后把 raw_demos 和 augmented_demos 按照配置交给“学生”，完成了优化。

后续在 LLM 程序运行时，这部分的 demos 会放入 prompt 中最终发送给 LLM。

在此基础上，我们还可以进一步做 bootstrap 样例的选择，例如：

+   BootstrapFewShotWithRandomSearch ：随机打乱初始训练集，做多次 bootstrap，挑出效果最好的。

+   BootstrapFewShotWithOptuna ：完成 bootstrap 后，通过 Optuna（贝叶斯优化）来帮忙搜索效果最好的 demo。不过这里只搜索 demo 的 index 编号，感觉跟随机没啥两样……

#### ** Prompt 中的指令优化 **

除了样例外，任务描述的说明等优化也很重要，大家平时可能也经常见到这方面的一些 trick，比如结构化描述，“Let's think step by step”，“This is very important to my career”，“给小费”等。一些其它做自动 prompt 优化的项目很多也专注在这个方面。

在 DSPy 中，主要提供了两个优化器。

COPRO ：

先使用 BasicGenerateInstruction 来生成一系列“候选 prompt”。这个 meta-prompt 大概长这样：

```
class BasicGenerateInstruction(Signature):
    &quot;&quot;&quot;You are an instruction optimizer for large language models. I will give you a ``signature`` of fields (inputs and outputs) in English. Your task is to propose an instruction that will lead a good language model to perform the task well. Don&#x27;t be afraid to be creative.&quot;&quot;&quot;

    basic_instruction = dspy.InputField(desc=&quot;The initial instructions before optimization&quot;)
    proposed_instruction = dspy.OutputField(desc=&quot;The improved instructions for the language model&quot;)
    proposed_prefix_for_output_field = dspy.OutputField(desc=&quot;The string at the end of the prompt, which will help the model start solving the task&quot;)
```

接下来对每个“候选 prompt”进行多轮迭代。先执行生成，做评估，然后把之前的 prompt 和评估结果传给 GenerateInstructionGivenAttempts ，生成下一轮的“候选 prompt”。这个做优化的 meta-prompt 长这样：

```
class GenerateInstructionGivenAttempts(dspy.Signature):
    &quot;&quot;&quot;You are an instruction optimizer for large language models. I will give some task instructions I&#x27;ve tried, along with their corresponding validation scores. The instructions are arranged in increasing order based on their scores, where higher scores indicate better quality.

    Your task is to propose a new instruction that will lead a good language model to perform the task even better. Don&#x27;t be afraid to be creative.&quot;&quot;&quot;

    attempted_instructions = dspy.InputField(format=dsp.passages2text)
    proposed_instruction = dspy.OutputField(desc=&quot;The improved instructions for the language model&quot;)
    proposed_prefix_for_output_field = dspy.OutputField(
        desc=&quot;The string at the end of the prompt, which will help the model start solving the task&quot;,
    )
```

最后在所有的“候选 prompt”中挑选效果最好的。整个流程如图所示：

![](img/fb311c9f9589c149f56092890904f627.png)

COPRO 优化过程

MIPRO ：

流程更加完善了。

1.  首先默认对所有 module 执行 BootstrapFewShot ，生成样例。

1.  接下来用一个 prompt 让 LLM 观察一下训练数据，给出总结。这里面也是增量总结，需要不少的 LLM 调用。

1.  把对于数据的观察，bootstrap 生成的 example 都一起扔给 meta-prompt 来撰写出多个“候选 prompt”。

1.  最后把 demo 选择和候选 prompt 选择一起作为一个优化问题，扔给 Optuna 来做优化，不得不说有点高级。

生成候选 prompt 的 meta-prompt 内容如下：

```
class BasicGenerateInstructionWithExamplesAndDataObservations(dspy.Signature):
    &quot;&quot;&quot;You are an instruction optimizer for large language models. I will give you a ``signature`` of fields (inputs and outputs) in English. Specifically, I will give you some ``observations`` I have made about the dataset and task, along with some ``examples`` of the expected inputs and outputs. I will also provide you with the current ``basic instruction`` that is being used for this task.

    Your task is to propose a new improved instruction and prefix for the output field that will lead a good language model to perform the task well. Don&#x27;t be afraid to be creative.&quot;&quot;&quot;

    observations = dspy.InputField(desc=&quot;Observations about the dataset and task&quot;)
    examples = dspy.InputField(format=dsp.passages2text, desc=&quot;Example(s) of the task&quot;)
    basic_instruction = dspy.InputField(desc=&quot;The initial instructions before optimization&quot;)
    proposed_instruction = dspy.OutputField(desc=&quot;The improved instructions for the language model&quot;)
    proposed_prefix_for_output_field = dspy.OutputField(
        desc=&quot;The string at the end of the prompt, which will help the model start solving the task&quot;,
    )
```

大家如果之前搞过 **AutoML [4] **，对使用 Optuna 来做优化应该也不陌生。这里还做的挺细致，包括在 evaluation 过程中按批次来做，可以提前剪枝。毕竟每次 evaluation 都是真实的 LLM pipeline 调用，成本还是挺高的。

搜索逻辑：

```
# Suggest the index of the instruction candidate to use in our trial
instruction_idx = trial.suggest_categorical(
    f&quot;{id(p_old)}_predictor_instruction&quot;,
    range(len(p_instruction_candidates)),
)
demos_idx = trial.suggest_categorical(
    f&quot;{id(p_old)}_predictor_demos&quot;,
    range(len(p_demo_candidates)),
)
```

剪枝逻辑：

```
for i in range(num_batches):
    start_index = i * batch_size
    end_index = min((i + 1) * batch_size, len(trainset))
    split_trainset = trainset[start_index:end_index]
    split_score = evaluate(candidate_program, devset=split_trainset, display_table=0)

    total_score += split_score * len(split_trainset)
    curr_weighted_avg_score = total_score / min((i + 1) * 100, len(trainset))
    trial.report(curr_weighted_avg_score, i)

    # Handle pruning based on the intermediate value.
    if trial.should_prune():
        print(&quot;Trial pruned.&quot;)
        trial_logs[trial_num][&quot;score&quot;] = curr_weighted_avg_score
        trial_logs[trial_num][&quot;pruned&quot;] = True
        trial_num += 1
        raise optuna.TrialPruned()
```

当然即使做到这样，可能也有很多可以尝试改进的地方。比如我们人工在做 evaluation 时，不仅仅会看一个分数，还会做 bad case 的分析。是不是这个错误的分析和总结也可以利用一个 meta-prompt 来生成，作为一部分信息放入后续 prompt 优化的过程中呢？

####  LLM fine-tune 优化

除了 prompt 本身优化之外，fine-tune 优化也是我们日常工作中的常用手段。DSPy 里也在 BootstrapFinetune 里做了实现，比较直观：

1.  先用 teacher 来 bootstrap 一系列 demo 作为训练样本。

1.  使用 transformers 库来 fine-tune 一个小模型，可以选择不同的模型，不同的训练参数等。这里有个选择是可以针对不同的模块分别 fine-tune，也可以 fine-tune 一个“多任务”模型。

1.  最后将 module 里的 lm 替换成 fine-tune 过的模型，完成优化。

#### ** Pipeline 结构优化 **

目前只有一个 Ensemble ，可以人工定义多种逻辑，然后放在一起集成，在运行时通过类似投票的方式来决定最终结果，也算是一种 pipeline 结构的动态选择。

```
import dspy
from dspy.teleprompt import Ensemble

# Assume a list of programs
programs = [program1, program2, program3, ...]

# Define Ensemble teleprompter
teleprompter = Ensemble(reduce_fn=dspy.majority, size=2)

# Compile to get the EnsembledProgram
ensembled_program = teleprompter.compile(programs)
```

当然这个 compile 本身没啥开销，但在运行时则会有开销放大。如果能像 NAS 一样自动搜索最优的 pipeline 结构那就高级了。比如说是不是可以把一个 module 拆成子任务，或者把几个 module 合并之后 fine-tune 一个 model 来减少开销等。

####  如何选择 Optimizer

官方给出的建议是：

1.  默认可以用 BootstrapFewShotWithRandomSearch 。

1.  数据少于 10 条，用 BootstrapFewShot 。

1.  数据有 50 条左右，用 BootstrapFewShotWithRandomSearch 。

1.  数据有 300 条左右，用 MIPRO 。

1.  如果用比较大的模型（>7B）且希望提升效率，可以用 BootstrapFinetune 。

### ** 其它模块 **

D**ata，Metric 等都**比较好理解。剩下值得一提的是 DSPy 中的 Assertions。这个模块的目标和实现方式都跟我们之前聊过的 LLM 可控生成 [5] 中的 guardrails 非常类似。比如我们希望大模型的生成内容只有 yes or no，或者生成长度不要超过 100 个字符等，的确与我们日常写代码中的各种断言非常类似。不过 DSPy program 中的断言除了检查和抛出失败外，还可以尝试帮我们自动修复。

![](img/db462ba9a0bde88a40c60dca840c05bd.png)

DSPy Assertions

从上图中也比较好理解，在程序中我们可以定义各种 assertions，在代码执行时会自动触发进行检查。当检查失败时，会把之前的生成结果，尝试修复的指令拼接起来，发送给 LLM 进行 self-refine。DSPy 中默认带了两种断言， Assertions 可以理解为“硬断言”，如果修复失败程序就会终止运行。而 Suggest 则是“软断言”，即使尝试修复没有成功也会继续执行。顺带一提前面提到的 TypedPredictor 使用的也是类似自动修复方案。

这类 assertions 的使用场景也不止一种：

+   最基础的能力是在运行过程中进行各种检查，当出现问题时动态修改 pipeline 运行逻辑，例如回到之前的某个 module 尝试重新修复执行。

+   启用 assertions 之后，还可以帮助我们在做 prompt 样例优化时，生成更高质量的样本，把那些即使结果正确，但中间步骤不符合我们预期的内容去除。

+   在 bootstrap 过程中出现的“失败案例”也可以保留下来，后续作为 prompt 的一部分发给 LLM，减少犯类似错误的情况出现。

关于最后一点的实现也挺巧妙。还记得我们前面提到 bootstrap 过程中，会把 LLM 生成的 reasoning 过程也加入到样例中吗？因为 CoT 模块改变了 signature，所以输出样例中会把新加入的字段也自动存下来（这里是 reasoning）。而我们在做 self-refine 时，同样也是动态改变了 signature，所以修复过程的输入输出也都会在新的样例中保留下来（之前错误的尝试，指令，新的输出等）。

原始 example：

```
question: At My Window was released by which American singer-songwriter?
rationale: produce the query. We know that the song &quot;At My Window&quot; was released by an American singer-songwriter, so we need to find the name of the artist.
query: &quot;At My Window&quot; singer-songwriter American artist
```

自动修复后的 example：

```
question: At My Window was released by which American singer-songwriter?
past_rationale: produce the query. We know that the song &quot;At My Window&quot; was released by an American singer-songwriter, so we need to find the name of the artist.
past_query: &quot;At My Window&quot; singer-songwriter American artist
rationale: produce the query. We can search for &quot;At My Window singer-songwriter&quot; to find the artist&#x27;s name.
feedback: Query should be short and less than 40 characters
query: At My Window singer-songwriter
```

此外，DSPy 中也提供了一些配置，可以选择在 compile 和运行期间是否启用 assertions。

## ** DSPy 总结**

总体来看，DSPy 比较好地抽象了 LLM pipeline 中的各种操作元素和优化方法，进行了系统性的设计。我们也在文中多次类比了 PyTorch，如果做个类比的话：

+   DSPy 鼓励我们通过程序语言来定义“LLM 网络”，通过复杂的 module 组合来引入“inductive bias”以更好地适应不同问题。

+   正像在 DNN 定义中，我们并不会把所有模型参数平铺在一层 FFN 中，而是会把网络做“深”。同样在 LLM 应用中，目前我们也无法通过一个平铺的大 prompt 来端到端解决问题，同样需要进行细致的拆分，把“LLM 网络”做深。

+   训练数据还是非常核心的一环，garbage in, garbage out 在 LLM 时代同样成立。

具体到优化方法上，虽然前面提到有些基础的参数如 LLM 的温度值这类也还没有支持，但从架构上来说要支持并不困难，可扩展性还是不错的。例如：

+   Module 中带有 LLM 配置，这里有很多可以优化的部分，从模型选择，到调用参数，甚至高级的 logit_bias 调整等应该也能够实现。

+   因为可以自定义 metric，理论上除了准确率以外，也能够优化像速度之类的指标。Optuna，进化算法这类完全可以支持黑盒优化。

+   对于非 LLM 模块的优化应该也可以，比如现在 compile 方法会针对 pipeline 中的所有 Predictor 模块，也可以更广泛一点把 Retrieve 这样的模块也考虑进来。

不过目前阅读代码和试用下来，如果想在项目中深入使用 DSPy 也会有些问题：

+   执行 compile 时，会有大量的 LLM 调用，成本可能会比较高。

+   如果想自己做一些灵活的定制化，像上面提到的 self-refine 的实现，门槛很高。

+   总体上来说还是偏学术界的项目，代码质量比较一般。

# ** 其它 prompt 优化项目**

本来只想写一篇 5000 字以内的短文的，没想到一下子又生成了这么多 token……最后就简单提一下其它的 prompt 优化项目：

+   gpt-prompt-engineer [6] ：自动生成 prompt，LLM 运行之后生成结果。然后在 prompt 的生成 case 之间进行两两对战，裁判也是 LLM。最后通过胜负情况得出 ELO 分排名。总体逻辑比较简单，偏随机搜索。

+   **AutoPrompt [7] **：用户输入任务与初始 prompt。LLM 生成测试样例，由人工打标或者模型生成期望输出。接下来使用当前 prompt 进行预测，做结果分析，再进行 prompt 优化，不断循环提升。

+   **SAMMO [8] **：微软推出的 prompt 优化研究项目，粗看了下论文和代码，在 prompt 修改方面做的比较细致，包括重写，精简，删除，转换格式，从样例中生成指令等等。不过流行度和文档完善程度都不如 DSPy。

![](img/dc96f24d8c5606c68b274d945ed1241b.png)

SAMMO 总览

最后给我们在做的产品打个广告。观远数据是一家国内领先的 AI + BI 产品公司，去年也推出了基于 LLM Agent 技术的 ChatBI 产品，并与若干客户合作进行深度共创，积累了不少实战落地经验。如果您所在的公司有这方面的需求，欢迎联系我们，交流相关合作机会。

### **参考资料**

[1] RAG: https://www.bilibili.com/video/BV1TC41177rC/

[2] DSPy: https://github.com/stanfordnlp/dspy

[3] Instructor: https://github.com/jxnl/instructor

[4] AutoML: https://zhuanlan.zhihu.com/p/212512984

[5] LLM 可控生成: https://zhuanlan.zhihu.com/p/642690763

[6] gpt-prompt-engineer: https://github.com/mshumer/gpt-prompt-engineer

[7] AutoPrompt: https://github.com/Eladlev/AutoPrompt

[8] SAMMO: https://github.com/microsoft/sammo