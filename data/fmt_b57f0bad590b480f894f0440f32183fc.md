
# SFT 数据筛选（一）：高质量篇

> 来源：[https://aicarrier.feishu.cn/docx/R0xVdhmQhoxjxQxveiXcednGnNf](https://aicarrier.feishu.cn/docx/R0xVdhmQhoxjxQxveiXcednGnNf)

文档作者：樊奇

短视频视频作者：招募中

随着大语言模型（LLM）的发展，开源社区中涌现了越来越多、越来越丰富的指令微调（又被称为有监督微调，Supervised Fine-Tuning，SFT）数据集。如何有效地利用这些 SFT 数据集，同时尽可能降低微调成本成为了一个值得讨论的问题。

同时 LIMA: Less Is More for Alignment[1] 指出，LLM 在预训练阶段就已经学习到了大部分的能力，而在对齐阶段学习对话能力与风格。所以该论文进行了相关实验，发现1000条的高质量对话数据就可以让模型在 SFT 阶段学习得很好。

因此，我们可以通过对大量的 SFT 数据进行质量筛选，选择出少量高质量的部分用于微调。一方面既有效利用了这些 SFT 数据集，另一方面又减少了微调数据量，降低了微调成本。

所以在本文中，我们将介绍 SFT 数据筛选之高质量数据筛选的相关工作，希望可以对大家有所帮助。

## IFD & Superfiltering

![](img/bda9dccb49aec7c5998d6c74f69d523d.png)

![](img/782cbf122af0fa4ea73802c13704023e.png)

IFD（Instruction Following Difficulty，指令跟随难度）[2] 提出了一种基于条件的指标，以衡量某条指令数据的难度，随后选择出 IFD 分数高于预定义阈值的部分用于模型微调。

其计算公式为 ，其中 为模型参数， 为指令， 为对应的回答， 为损失函数。 为条件回答分数，即评估模型在给定指令的条件下生成恰当的回复的能力。但是条件回答分数高并不一定因为指令难以跟随，也有可能是生成本身较为困难，因此引入了直接回答分数 来消除这种影响。

Superfiltering [3] 则进一步拓展了 IFD。该论文将 IFD 计算公式修改为 ，即不再使用损失函数，而是使用困惑度作为评价标准。此外，该论文还指出，大模型与小模型（如124M参数量的 GPT-2）得到的 IFD 分数具有一致的趋势，因此可以选用小模型来计算 IFD 分数以选择数据，进一步减少计算开销。

## DEITA

![](img/10f045ea95606a261700aef9a5d475db.png)

DEITA[4] 则是依靠 ChatGPT 扩充数据并训练打分模型，从指令复杂度和回复质量两方面对数据进行打分，并选择高分数据。

具体地，如上图 Evol Complexity 过程所示，DEITA 基于某条数据，通过 ChatGPT 不断加深指令复杂度，以得到不同指令复杂度水平的数据。并使用 ChatGPT 对这些不同指令复杂度水平的数据进行排序，以消除直接打分时的误差。随后根据排序结果赋予数据相对应的得分，并根据得分训练一个指令复杂度打分模型，用于其他数据的指令复杂度评估。Evol Quality 过程所示的数据质量同理。

最后将这两部分分数相乘，便可以得到一条数据的最终得分，从而依此进行排序和选取。

## Nuggets

![](img/0ec45c0d58148c16ca19bea285fc55e3.png)

Nuggets[5] 通过计算一条数据作为 One-shot 例子时，其对模型在验证集上的表现的贡献度来衡量数据的质量。可以看到，Nuggets 方法需要一个预定义的验证集，而该验证集的定义将会一定程度上影响整体方法的效果。

首先可以计算出模型在 Zero-shot 状态下在验证集上的得分情况，即为 `Zero Shot Score`。对于每一条候选数据，都可以计算出该条数据作为 One-shot 例子时，模型在验证集上的得分情况，即为 `One Shot Score`。用 `One Shot Score` 减去 `Zero Shot Score`，便可以计算出该条数据对于模型在验证集上的表现的贡献。然后选择出贡献度最高的数据作为训练集。

通过这种方法，虽然可能会出现 “LLM 是大语言模型的缩写” 作为 One-shot 例子时，模型回答 “今天天气怎么样？” 问题效果更好的情况，但 Nuggets 确实是在一定程度上选择出了对模型回答问题有益的数据。

## InstructionMining

![](img/18e51a6b3e9ce9e963377573dc7d5184.png)

相较于 DEITA 和 Nuggets，InstructionMining[6] 则更有工业化的味道。InstructionMining 认为数据质量与模型在验证集上的 `loss` 存在一定的比例关系。同时为了避免计算验证集上的 `loss`，InstructionMining 认为模型在验证集上的 `loss` 可以由一系列的预测器的结果的线性组合进行预测，从而进一步节省时间与计算开销。

InstructionMining 选用的预测器如上图所示，包括输入长度、输出长度、奖励模型的输出分数、困惑度、与最近 个邻居的距离、自然度、一致性和易懂度。InstructionMining 将这些预测器的结果进行线性组合，并使用线性回归的方式得到系数，从而量化数据质量。InstructionMining 的方法为 SFT 数据质量评估带来了更多的可解释性。

## Others

除了上面提到的这些方法，还有直接选择 SFT 数据中最长的数据的 `LongIsMore`[7]；直接用 ChatGPT 对 SFT 数据打分后选择高分数据的 `AlpaGasus`[8]；使用 `embedding` space 的特征进行压缩并结合 ChatGPT 打分和回复长度的 `LIFT`[9] 等方法。由于篇幅问题，此处就不做详细的展开了。

---

引用文献：

[1]: https://arxiv.org/pdf/2305.11206

[2]: https://arxiv.org/pdf/2308.12032

[3]: https://arxiv.org/pdf/2402.00530

[4]: https://arxiv.org/pdf/2312.15685

[5]: https://arxiv.org/pdf/2312.10302

[6]: https://arxiv.org/pdf/2307.06290

[7]: https://arxiv.org/pdf/2402.04833

[8]: https://arxiv.org/pdf/2307.08701

[9]: https://arxiv.org/pdf/2312.11508
