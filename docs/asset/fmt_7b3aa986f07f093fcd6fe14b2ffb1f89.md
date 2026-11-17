
# Gemma 2 技术报告与模型评估

## 知识蒸馏与后期训练

根据 Gemma 2 技术报告，知识蒸馏用于预训练 90 亿模型，而 270 亿模型则是从头开始预训练的。

在后期训练中，Gemma 2 团队生成了来自教师 (报告中未指定，但可能是 Gemini Ultra) 的多样化补全集，然后使用这些合成数据通过 `SFT` 训练学生模型。这也是许多开源模型的基础，如 `Zephyr` 和 `OpenHermes`，它们完全基于较大大语言模型的合成数据进行训练。

+ [Zephyr](https://hf.co/HuggingFaceH4/zephyr-7b-beta)
+ [OpenHermes](https://hf.co/teknium/OpenHermes-2.5-Mistral-7B)

尽管有效，但这种方法存在缺点，因为学生和教师之间的模型容量不匹配可能导致 训练-推理不匹配，即学生在推理期间生成的文本与训练期间看到的文本不同。

为解决这个问题，Gemma 2 团队采用了“在线蒸馏”，其中学生从 `SFT` 提示生成补全。这些补全用于计算教师和学生 `logits` 之间的 `KL` 散度。通过在整个训练过程中最小化 `KL` 散度，学生能够准确地模拟教师的行为，同时最小化训练-推理不匹配。

+ [“在线蒸馏”](https://arxiv.org/pdf/2306.13649)

这种方法非常有趣，正如我们在社区中看到的那样，在线 `DPO` 等在线方法会产生更强的模型，而在线蒸馏的一个优势在于只需要教师的 `logits`，因此无需依赖奖励模型或大语言模型作为评审员来改进模型。我们期待看到这种方法在未来几个月中是否会在微调人员中变得更受欢迎！

## 模型合并

模型合并是一种将两个或多个大语言模型合并成一个新模型的技术。这是相对较新和实验性的，可以不使用加速器进行。`Mergekit` 是一个流行的开源工具包，用于合并大语言模型。它实现了线性、`SLERP`、`TIES`、`DARE` 和其他合并技术。

+ [模型合并](https://hf.co/blog/mlabonne/merge-models)
+ [Mergekit](https://github.com/arcee-ai/mergekit)

根据技术报告，Gemma 2 使用了 `Warp`，这是一种新型合并技术，分三个独特阶段进行合并：

+ [Warp](https://arxiv.org/abs/2406.16768)

1. 指数移动平均 (`EMA`)：在强化学习 (`RL`) 微调过程中应用。
1. 球形线性插值 (`SLERP`)：在多个策略的 `RL` 微调后应用。
1. 向初始化线性插值 (`LITI`)：在 `SLERP` 阶段之后应用。

## Gemma 2 的评估

Gemma 模型的表现如何？以下是根据技术报告和新版开源 LLM 排行榜对其他开源开放模型的性能比较。

+ [开源 LLM 排行榜](https://hf.co/spaces/HuggingFaceH4/open_llm_leaderboard)

### 技术报告结果

Gemma 2 的技术报告比较了不同开源 LLM 在之前开源 LLM 排行榜基准上的性能。

该报告还比较了小型语言模型的性能。

### 开源 LLM 排行榜结果

注意：我们目前正在新的开源 LLM 排行榜基准上单独评估 Google Gemma 2，并将在今天晚些时候更新此部分。

## 如何提示 Gemma 2

基础模型没有提示格式。像其他基础模型一样，它们可以用于继续输入序列的合理延续或零样本/少样本推理。指令版本有一个非常简单的对话结构：

```
<start_of_turn>user
knock knock<end_of_turn>
<start_of_turn>model
who is there<end_of_turn>
<start_of_turn>user
LaMDA<end_of_turn>
<start_of_turn>model
LaMDA who?<end_of_turn><eos>
```

必须精确地复制此格式才能有效使用。稍后我们将展示如何使用 `transformers` 中的聊天模板轻松地复制指令提示。
