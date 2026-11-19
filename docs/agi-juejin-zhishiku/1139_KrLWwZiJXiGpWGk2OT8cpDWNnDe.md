
# SFT和RL究竟为LLM带来了什么

> 来源：[https://agijuejin.feishu.cn/docx/IHbLdzBHqokSYmxeDvvcgcSHnub](https://agijuejin.feishu.cn/docx/IHbLdzBHqokSYmxeDvvcgcSHnub)

> 作者：咸鱼王

> 原文：[https://zhuanlan.zhihu.com/p/669265454](https://zhuanlan.zhihu.com/p/669265454)

Pretrain+`SFT`+`RL`已经逐渐成为孵化`LLM`的标准流程：`SFT`解锁Instruction following、`RL`强化helpfulness、`honesty`、`safety`偏好。但Instruction Tuning和Reforcing Learning究竟从什么角度影响、多大程度上影响了`LLM`却鲜有深入探究。本文以META这篇论文为引，结合相关文章和笔者的个人见解，尝试对`SFT`、`RL`的作用做进一步分析。

![](img/f7944dbbba6b5ae4f10c28597a37804f.png)

论文链接：

[UNDERSTANDING THE EFFECTS OF RLHF ON LLM GENERALISATION AND DIVERSITY](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2310.06452.pdf)

## 01 实验设置

本文从泛化性、多样性两个角度，以`LLAMA-7B`、`OPT`为基座模型，验证`SFT`、`RLHF`、`BoN`（Best Of N）对模型带来的影响。

### 1.1 对比方案

+ `SFT`：基于In-domain数据集进行指令微调。
+ `BoN`：基于In-domain数据集训练`RM`，利用`RM`从`SFT`模型的`K`个`outputs`中挑选`top1`作为最终结果，本质是一种生成后排序的方法。
+ `RLHF`：基于In-domain数据集训练`RM`，并基于`SFT`进行`RLHF`。

### 1.2 数据集

+ `Summarisation`
+ [训练集/In-domain测试集： TL;DR](https://link.zhihu.com/?target=https%3A//openaipublic.blob.core.windows.net/summarize-from-feedback/website/index.html%23/tldr)
+ [Out-of-domain测试集： CNN/DailyMail](https://link.zhihu.com/?target=https%3A//openaipublic.blob.core.windows.net/summarize-from-feedback/website/index.html%23/cnndm)
+ `Instruction Following`
+ [训练集/In-domain测试集： AlpacaFarm](https://link.zhihu.com/?target=https%3A//huggingface.co/datasets/tatsu-lab/alpaca_farm/viewer/alpaca_human_preference)
+ [Out-of-domain测试集： AlpacaEval 、Sequential Instructions](https://link.zhihu.com/?target=https%3A//huggingface.co/datasets/tatsu-lab/alpaca_eval)

### 1.3 评测方法

+ 泛化性：基于`GPT-4`进行自动评测，在`context`中给出`input`、`model_output`、`reference`，将`model_output pick rate`作为评测分数，经作者验证，该场景下模型评测与人工评测一致性在70%左右。作者将模型在`ID`、`OOD`下分数之差作为泛化性分数。
+ 多样性
+ `distinct N-grams`：模型输出结果中，去重后的`N-grams`个数
+ `Sentence-BERT embedding`：模型输出结果在语义空间的平均余弦距离

## 02 结论：RLHF使泛化性提升

### 2.1 实验

作者首先基于`Summarisation`任务进行了泛化性测试，具体如下图。不难看出，几种方法从`ID`迁移到`OOD`时，`pick rate`分数不约而同有所下降，两个比较反常识的现象是：`pick rate`分数`BoN`>`RLHF`>`SFT`，泛化性`BoN`≈`SFT`>`RLHF`。作者并未对`RLHF`表现不佳的原因做深入探讨和分析，但作者后续尝试了多种参数量和数据划分逻辑，结论均一致。从`BoN`、`SFT`泛化性分数基本一致可以看出，至少`RM`迁移至`OOD`时是基本无损的（`BoN`本质上和`SFT`是同一种方法，只是语料分布不同，如果`RM`泛化性较差，`BoN`的泛化性分数应比`SFT`更差）。

![](img/f6aeb305ebf2f97c2acb7e06cbd417cc.png)

在`Instruction Following`上的结论相对令人"喘了一口气"，具体如下图。首先在`ID`和两个`OOD`测试集上，均表现为`RLHF`>`BoN`>`SFT`。另外，在domain偏移相对不大的`AlpacaEval`上，3种方法泛化性差异相似，而在更难的`Sequential Instructions`任务中，`RLHF`则较明显的优于`BoN`和`SFT`。

![](img/78388f1702faa266dc835b236d4aeb61.png)

从3组实验来看，作者给出的`RLHF`使泛化性提升的结论似乎并不强烈，但结合日常使用的经验，`RLHF`后的模型的确在指令理解、响应上更胜一筹。之所以只有在`Sequential Instructions`下有明显提升，笔者认为，或许是因为`RLHF`能够在整体上给模型泛化性带来提升，但并不能保证每种任务类型下都是增益，因此在`Instruction Following`这种复杂多样的场景下有所提升，而在如`Summarisation`的某些垂类任务里有所下降。

### 2.2 从token2token重要性角度剖析SFT带来的影响

在给出更多的猜想之前，笔者想先引入另一篇文章。该篇文章从`inputToken`对`responseToken`重要性的角度，来分析指令微调后，`prompt`对`LLM`的影响：

[https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2310.00492.pdf](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2310.00492.pdf)

定义`input`第 `n` 个token为 `x_n` ，`output`第 `m` 个token为 `y_m` ，则 `x_n` 对 `y_m` 的重要性为 `I_{n,m}=p(y_m|Z_m)-p(y_m|Z_{m,/n})` ，其中 `Z_m` 指`input+output`的前 `m-1` 个token。简而言之， `I_{n,m}` 意为删掉 `x_n` 前后，LLM生成 `y_m` 的置信度偏移，偏移越大则越重要。文中作者利用P值检验，验证 `I_{n,m}` 和LLM是否遵循指令强相关，也即 `I_{n,m}` 越高，LLM越倾向于响应Instruction中对应的intents，验证流程不再赘述。

作者依据 `I_{n,m}` ，绘制了`LLAMA`和`Vicuna`的 `(instruction+context, response)` 重要性显著图，如下所示。可以看出经过`SFT`的`Vicuna`，`InstructionTokens`（红框）更亮且亮的更多，`ContextTokens`亮的更少（蓝框），说明经过指令微调以后，模型能更精准区分`prompt`中的`instruction`和`context`，且以更高程度、更长时间让`InstructionTokens`干预`response`的生成。

![](img/e5bf84de6643fc7b3646508f07fc1c5c.png)

此外，作者也对`prompt`中 `I_{n,m}` 的分布进行了统计，如下图所示，重要性随着位置的变化呈现高-低-较高的趋势，即 `LLM`更容易忽略`prompt`中尾部的`tokens`（尤其是中间），经过`SFT`后有所改善，这也和我们平时的经验一致。

![](img/0114dbcea7f7f732d859ddb360aa7120.png)

另外作者还发现， `LLM`有时会“假装”遵循了指令，如下图，模型在执行某个文本纠错指令时，生成`response`的过程中实际仅参考了前3个`tokens`（这3个`tokens`并没有实际意义），而`context`部分无意义的`token`又扮演了`instruction`的角色，全程指引`LLM`完成生成（蓝框，对应的`token`为"`\n\nInput:`"和"`\n\nOutput:`"）。

![](img/8b3ddb1dc4c1fe30a0414f4cab5e8e44.png)

这让笔者想到一个例子：给定`Leetcode`题让`LLM`写代码，理想情况是`LLM`读题、理解题意、然后写出答案，但实际上`LLM`可能看到 `http://Leetcode.xxx` 就不再关注后面的内容了。笔者认为，这个问题本质上是`LLM`在训练过程中对某些模式产生了过拟合，我们以往期望模型完成某项任务时，通常是将任务或逻辑具象化为数据，再用数据训练模型，让模型从数据中统计、抽象、还原出数据背后的逻辑。

实际上，碍于采样方式、标注人力等因素，我们展示给模型的数据往往只是真实世界分布中一个相当小、相当片面的子集，很难、也不可能涵盖所有情况。模型能否透过这部分“狭隘”的数据，还原出和我们期望完全一致的逻辑是不得而知的，因此一叶知秋有时只是我们的一厢情愿。再举个例子，我们想训练模型分辨马，但训练图片中马往往与草同时出现，所以模型可能误以为任务是分辨草，此时模型对训练数据产生了错误的解读，或说对某些模式产生了过拟合，在`ID`测试集下看不出问题，`OOD`下就会立刻现出原形（比如草里的猪也识别成了马）。

### 2.3 从token2token重要性角度看待RLHF带来的泛化性提升

2.2主要探讨了`SFT`对模型的影响，虽然该篇文章的作者没有对`RLHF`后的模型做类似实验，但`RLHF`带来的泛化性提升或许可以借鉴这种解读方式。笔者认为， `RLHF`本质上是一种更有力地、矫正或规范模型输出偏好的训练方式，目前多数`chat`模型所强调的`helpfulness`、`honesty`、`safety`只是期望偏好的个例，我们也可以利用`RLHF`让`chat`模型学习身份偏好、语气偏好、情感偏好等等。

然而不论是`SFT`还是`RLHF`，`SFT`模型和`RM`模型的训练本质都是基于`CE`的方式，相比`RL`这种`reward`导向的方法，对误差相对不那么敏感。例如"我很喜欢草莓"和"我不喜欢草莓"在`SFT`训练中`loss`不会太大，但`RM`能敏锐的捕捉这种语义偏差，并通过`reward`的方式放大误差、“痛击”`LLM`。

我们通常将泛化性理解为模型在`OOD`下的适应能力，笔者想在`Instruction Following`场景下对泛化性做进一步细化：在不涉及到知识迁移的情况下，泛化性可以理解为`LLM`对低频指令及指令描述的响应能力。结合之前的一些实验结论可以猜想，`LLM`在经由`RLHF`强化`helpfulness`过程中，也在强化模型对指令的识别、理解和遵循能力：通过`reward`导向，最大程度上鼓励响应尽可能多的指令的生成方式、打击未遵循、漏遵循指令的生成方式，使模型比`SFT`模型对`instructionTokens`更加敏感，从而倾向于让`instructionTokens`以更长时间、更高程度干预`response`的生成。

但`RLHF`也绝非一劳永逸的方案，`RM`仍然和`SFT`一样都是基于`CE`训练的模型，能够通过`RM`放大误差、影响`LLM`的前提是`RM`能够识别这种误差，因此 `RLHF`的泛化能力依旧受限于`RM`的泛化能力。好在我们可以通过构建`comparisons`的方式让`RM`更轻易的感知到这种偏差，且`RM`作为`NLU`的模型本身就更易于训练。

## 03 结论：RLHF后多样性下降

### 3.1 实验

多样性评测上，作者仅基于`Summarisation`进行了实验，`Instruction Following`则因为不同模型的输出长度等偏好不统一，不具备较好的可比性。作者从`per-input`、`across-input`两个角度进行了实验，分别如下图(上)、下图(下)所示，二者区别在于是否先沿`input`维度做`reduce_mean`。

假设有模型 `π` 、输入 `input_i` ，则 `π` 的 `K` 个输出为 `O^i_π:={y_j∼π(y|x_i)|j=1...K}`：

1. `PerInputDiversity_D(π):=Σ_{i=1}^ND(O_π^i)/N` ，即先沿`input`维度对 `K` 个输出做`reduce_mean`，再对整个测试集做`reduce_mean`。
2. `CrossInputDiversity_D(π):=D(⋃_{i=1}^{N}O^i_π[1])` ，即取每个`input`的`top1`输出做`reduce_mean`。

![](img/69e4401b22c47b1827c8fafbef2d6ead.png)

![](img/9e144e2a1486f1840ed3c7e575f7e621.png)

两种评测方式的结论基本一致，在多样性表现上`RLHF`均低于`SFT`，但`across-input`的`diff`要远小于`per-input`的`diff`。这个结论也比较容易理解，`per-input`由于存在多个`beams`（文中 `K=16`），在广度上要远大于`across-input`，`per-input`下的`diff`更大也间接反应`RLHF`后的模型`beams`更加集中。此外，作者将`across-input`的`diff`更小归因于`RLHF`导致的"mode collapse"：即便`inputs`不同，模型也趋向于输出固定模式或风格的结果。

笔者认为，`RLHF`导致模型多样性降低的问题应辩证看待： 多样性降低是`RLHF`做风格矫正、强化后带来的必然后果，甚至有时让`LLM`惯于按照某种风格/格式进行输出，也是使用`RLHF`目的之一。实际上`SFT`也能基于大量数据达到类似效果，但效率和力度没有`RL`这么高。此外在多数场景下，我们确实需要一个对轻微扰动不敏感的模型。

### 3.2 从mode collapse看待模型多样性降低

`Mysteries of mode collapse` 较为详细的描述了模式坍塌这一问题，文中枚举了许多范例，展示`RLHF`带来的一些莫名其妙的输出倾向，而这些倾向不同于上一节所说的、我们乐于见到的稳定性/鲁棒性提升，而是模型对某些模式过拟合的表现。

以下图为例，作者分别基于`davinci`和`text-davinci-002`进行随机数生成实验，发现`RLHF`后的模型存在明显的随机数生成偏好，笔者在`GPT-4`上也进行了相似的实验，结论基本一致。

![](img/0f4cc34a2ed064de9976063618da18f3.png)

作者在文章中也提到，模型对可能在指令微调阶段出现过的`prompts`更容易出现模式坍塌的问题。笔者认为，2.2节中`LLM`“假装”遵循指令的例子就是模式坍塌的一个表现，模式坍塌不是`RLHF`才会有的问题，只是`RLHF`更强的矫正力度使这一现象更加明显，因此，模式坍塌本质上或许就是`LLM`对某些特征的过拟合。这种过拟合可能是因为`RM`的过拟合、并通过`RLHF`传递给了`LLM`，也有可能是`SFT`的过拟合、使`LLM`在`RL`阶段跳不出`SFT`的圈子，也有可能是`RL`阶段语料分布的不够广。

## 04 总结

本文从泛化性、多样性的角度对`SFT`、`RLHF`的影响进行了简单分析。`SFT`后的模型能够更加精准的识别`InstructionTokens`、并依赖这些指令进行生成，而`RLHF`作为一种更有力的训练方式，能够进一步强化`LLM`对指令的识别及响应能力，因此在泛化性提升上，`RLHF`>`SFT`。

在多样性上，输出模式、风格的集中是`RLHF`的必然后果，因此`RLHF`后`LLM`的多样性会有所降低。同时，`RLHF`对误差更加敏感也是一把双刃剑，在训练过程中可能会对某些模式或特征产生过拟合，也即模式坍塌现象。
