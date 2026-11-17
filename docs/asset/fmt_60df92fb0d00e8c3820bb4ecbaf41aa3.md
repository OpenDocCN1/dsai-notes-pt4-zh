
# DSPy 自动优化

## 自动优化概述

前面铺垫了这么多，终于到了传说中的自动优化部分了。优化的前提是可以进行评估，所以这里所有介绍的方法都需要借助预先构建好的评估数据集和相关的评估方法（evaluation metric）。相比神经网络的优化来说，这里我们可以使用任意的评估方法，因为这里不涉及到梯度计算、反向传播之类的方法。DSPy 中内置了一些评估方法，如 `dspy.evaluate.metrics.answer_exact_math`，`dspy.evaluate.metrics.answer_passage_match` 等。当然我们也可以自己动手写，包括用 LLM 来评估也可以，非常自由。

## 可优化参数

从可以优化的参数来说，我们编写的任何一个 LLM pipeline 程序都有很多形式各异的“旋钮”，而不仅仅是模型权重这样相对统一的形式。例如：

1. Prompt 模板中固定的指令描述。
1. Prompt 中给出的示范样例。
1. LLM 的选择，不同的参数设置。
1. LLM 的 fine tune 及相应参数。
1. Pipeline 中相关模块的参数，最典型的如 retrieval 模块的配置。
1. Pipeline 本身的结构，例如是否拆分任务，是否使用某些特定的循环控制流程等。

目前看 DSPy 中已经实现了上述提到的 1，2，4 这几块。借用推特上的一张图，大概效果是：

![](img/4c8a04ae3e7444c4ad1d76a6f6455442.png)

## DSPy Compiler

这个效果非常神奇，回想一下前面的例子中，调用这个优化过程仅仅是简单的两行代码：

```
teleprompter = BootstrapFewShot(metric=gsm8k_metric, **config)
optimized_cot = teleprompter.compile(CoT(), trainset=gsm8k_trainset)
```

让我们来看看这个编译过程具体是如何实现的，接下来会挑选几个最有代表性的优化器来讲解。

### Prompt 中的样例优化

DSPy 在这方面最典型的优化方法是 `BootstrapFewShot`。其工作步骤如下：

1. 首先初始化“学生”和“老师”两个 LLM 程序。从代码来看，两者必须拥有相同的 pipeline 结构，所以可能的区别就是可以使用不同的 LLM？没法实现用一个更复杂的 pipeline 来“蒸馏”出一个简单的 pipeline 来……

1. 把初始的训练数据交给“老师”，形成 `raw_demos`。

1. 对于每个训练样本，“老师”都会尝试去生成预测。注意这个预测可以是个复杂的过程，比如先生成 reasoning，再生成 answer。

1. 检查“老师”生成的预测，如果正确，整个预测的 trace 会被加入到 `augmented_demos` 里。这里可以实现一个效果，如果原始样本里只有问题和回答，而我们在优化的是一个 `CoT` module。这个 bootstrap 的过程会把最终回答正确的那些样例保存下来，同时还把 reasoning 的内容也补上了！同理，如果是 `RAG` module，也能自动补上召回的 context 内容作为样例的一部分，虽然这样也会导致样例比较庞大……

1. 最后把 `raw_demos` 和 `augmented_demos` 按照配置交给“学生”，完成了优化。

后续在 LLM 程序运行时，这部分的 demos 会放入 prompt 中最终发送给 LLM。

在此基础上，我们还可以进一步做 bootstrap 样例的选择，例如：

+ `BootstrapFewShotWithRandomSearch` ：随机打乱初始训练集，做多次 bootstrap，挑出效果最好的。

+ `BootstrapFewShotWithOptuna` ：完成 bootstrap 后，通过 `Optuna`（贝叶斯优化）来帮忙搜索效果最好的 demo。不过这里只搜索 demo 的 index 编号，感觉跟随机没啥两样……

### Prompt 中的指令优化

除了样例外，任务描述的说明等优化也很重要，大家平时可能也经常见到这方面的一些 trick，比如结构化描述，“Let's think step by step”，“This is very important to my career”，“给小费”等。一些其它做自动 prompt 优化的项目很多也专注在这个方面。

在 DSPy 中，主要提供了两个优化器。

#### COPRO

先使用 `BasicGenerateInstruction` 来生成一系列“候选 prompt”。这个 meta-prompt 大概长这样：

```
class BasicGenerateInstruction(Signature):
    """You are an instruction optimizer for large language models. I will give you a ``signature`` of fields (inputs and outputs) in English. Your task is to propose an instruction that will lead a good language model to perform the task well. Don't be afraid to be creative."""

    basic_instruction = dspy.InputField(desc="The initial instructions before optimization")
    proposed_instruction = dspy.OutputField(desc="The improved instructions for the language model")
    proposed_prefix_for_output_field = dspy.OutputField(desc="The string at the end of the prompt, which will help the model start solving the task")
```

接下来对每个“候选 prompt”进行多轮迭代。先执行生成，做评估，然后把之前的 prompt 和评估结果传给 `GenerateInstructionGivenAttempts`，生成下一轮的“候选 prompt”。这个做优化的 meta-prompt 长这样：

```
class GenerateInstructionGivenAttempts(dspy.Signature):
    """You are an instruction optimizer for large language models. I will give some task instructions I've tried, along with their corresponding validation scores. The instructions are arranged in increasing order based on their scores, where higher scores indicate better quality.

    Your task is to propose a new instruction that will lead a good language model to perform the task even better. Don't be afraid to be creative."""

    attempted_instructions = dspy.InputField(format=dsp.passages2text)
    proposed_instruction = dspy.OutputField(desc="The improved instructions for the language model")
    proposed_prefix_for_output_field = dspy.OutputField(
        desc="The string at the end of the prompt, which will help the model start solving the task",
    )
```

最后在所有的“候选 prompt”中挑选效果最好的。整个流程如图所示：

![](img/fb311c9f9589c149f56092890904f627.png)

COPRO 优化过程

#### MIPRO

流程更加完善了。

1. 首先默认对所有 module 执行 `BootstrapFewShot`，生成样例。

1. 接下来用一个 prompt 让 LLM 观察一下训练数据，给出总结。这里面也是增量总结，需要不少的 LLM 调用。

1. 把对于数据的观察，bootstrap 生成的 example 都一起扔给 meta-prompt 来撰写出多个“候选 prompt”。

1. 最后把 demo 选择和候选 prompt 选择一起作为一个优化问题，扔给 `Optuna` 来做优化，不得不说有点高级。

生成候选 prompt 的 meta-prompt 内容如下：

```
class BasicGenerateInstructionWithExamplesAndDataObservations(dspy.Signature):
    """You are an instruction optimizer for large language models. I will give you a ``signature`` of fields (inputs and outputs) in English. Specifically, I will give you some ``observations`` I have made about the dataset and task, along with some ``examples`` of the expected inputs and outputs. I will also provide you with the current ``basic instruction`` that is being used for this task.

    Your task is to propose a new improved instruction and prefix for the output field that will lead a good language model to perform the task well. Don't be afraid to be creative."""

    observations = dspy.InputField(desc="Observations about the dataset and task")
    examples = dspy.InputField(format=dsp.passages2text, desc="Example(s) of the task")
    basic_instruction = dspy.InputField(desc="The initial instructions before optimization")
    proposed_instruction = dspy.OutputField(desc="The improved instructions for the language model")
    proposed_prefix_for_output_field = dspy.OutputField(
        desc="The string at the end of the prompt, which will help the model start solving the task",
    )
```

大家如果之前搞过 `AutoML` [4]，对使用 `Optuna` 来做优化也不陌生。这里还做的挺细致，包括在 evaluation 过程中按批次来做，可以提前剪枝。毕竟每次 evaluation 都是真实的 LLM pipeline 调用，成本还是挺高的。

搜索逻辑：

```
# Suggest the index of the instruction candidate to use in our trial
instruction_idx = trial.suggest_categorical(
    f"{id(p_old)}_predictor_instruction",
    range(len(p_instruction_candidates)),
)
demos_idx = trial.suggest_categorical(
    f"{id(p_old)}_predictor_demos",
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
        print("Trial pruned.")
        trial_logs[trial_num]["score"] = curr_weighted_avg_score
        trial_logs[trial_num]["pruned"] = True
        trial_num += 1
        raise optuna.TrialPruned()
```

当然即使做到这样，可能也有很多可以尝试改进的地方。比如我们人工在做 evaluation 时，不仅仅会看一个分数，还会做 bad case 的分析。是不是这个错误的分析和总结也可以利用一个 meta-prompt 来生成，作为一部分信息放入后续 prompt 优化的过程中呢？

### LLM fine-tune 优化

除了 prompt 本身优化之外，fine-tune 优化也是我们日常工作中的常用手段。DSPy 里也在 `BootstrapFinetune` 里做了实现，比较直观：

1. 先用 `teacher` 来 bootstrap 一系列 demo 作为训练样本。

1. 使用 `transformers` 库来 fine-tune 一个小模型，可以选择不同的模型，不同的训练参数等。这里有个选择是可以针对不同的模块分别 fine-tune，也可以 fine-tune 一个“多任务”模型。

1. 最后将 module 里的 `lm` 替换成 fine-tune 过的模型，完成优化。

### Pipeline 结构优化

目前只有一个 `Ensemble`，可以人工定义多种逻辑，然后放在一起集成，在运行时通过类似投票的方式来决定最终结果，也算是一种 pipeline 结构的动态选择。

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

当然这个 compile 本身没啥开销，但在运行时则会有开销放大。如果能像 `NAS` 一样自动搜索最优的 pipeline 结构那就高级了。比如说是不是可以把一个 module 拆成子任务，或者把几个 module 合并之后 fine-tune 一个 model 来减少开销等。

### 如何选择 Optimizer

官方给出的建议是：

1. 默认可以用 `BootstrapFewShotWithRandomSearch`。

1. 数据少于 10 条，用 `BootstrapFewShot`。

1. 数据有 50 条左右，用 `BootstrapFewShotWithRandomSearch`。

1. 数据有 300 条左右，用 `MIPRO`。

1. 如果用比较大的模型（>7B）且希望提升效率，可以用 `BootstrapFinetune`。
