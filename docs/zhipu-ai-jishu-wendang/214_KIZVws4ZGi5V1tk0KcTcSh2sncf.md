
# 开源 · LongWriter：万字长文生成模型

> 来源：[https://ai.feishu.cn/docx/EjAJdSmKtoLCocxCRTfcsEACnvb](https://ai.feishu.cn/docx/EjAJdSmKtoLCocxCRTfcsEACnvb)
>
> 发布时间：2024 年 8 月 21 日
>
> 开源时间：2024 年 8 月 13 日
>
> **论文：**https://arxiv.org/abs/2408.07055
>
> **代码：**https://github.com/THUDM/LongWriter
>
> **模型：**
>
> +   Huggingface：https://huggingface.co/THUDM/LongWriter-glm4-9b
> +   魔搭：https://modelscope.cn/models/ZhipuAI/LongWriter-glm4-9b
>
> **数据**
>
> +   Huggingface：https://huggingface.co/datasets/THUDM/LongWriter-6k
> +   魔搭：https://modelscope.cn/datasets/ZhipuAI/LongWriter-6k

![](img/94dcd26515a4989854830e1b95fe5634.png)

随着大语言模型的发展，许多模型已经能够处理超过100k+ tokens的输入上下文。然而，这些模型在生成长文本时，普遍存在输出长度受限的问题。

在实际应用中，为了克服这个问题，人们普遍采用「分而治之」的方法，让模型一段一段地写。这种方法在一定程度上似乎能够解决长文本输出的问题，但往往会导致 1）消耗 tokens 量成倍甚至指数增加；2）前后内容不连贯。

如何才能让模型拥有更强的长文本输出能力呢？我们发现，模型输出长度受限的主要原因在于，监督微调（SFT）数据集缺乏足够长的输出样例。

针对这一问题，我们构建了一个 6000 多条`2k-20k words` 不等的长输出数据集 `LongWriter-6k`，并在此基础上，对 `GLM-4-9B` 进行 SFT 微调和 DPO 对齐。新的模型拥有了能够生成超过10,000字/词连贯文本的能力。

![](img/7245385c6ec0b13131be76cd16bc2c89.png)

[8月21日(1).mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/M1SxbHfAqowZ5NxD9utcndfKnSb)

![](img/87b9b40870685566e0e3e4a94b5db2f3.png)

[longwriter_demo.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/KNz6bcsmIoKulxxTZ5RcctACnZH)

## 生成长度限制的根源

我们对当前最先进的长上下文模型的最大输出长度做了测试，这些测试会明确要求模型生成不同长度的内容，例如：撰写一篇关于罗马帝国历史的10000字文章。

![](img/de1008e150ba7c8fa67c451d53b75a97.png)

从上图可以看出，所有模型生成的内容，几乎都不超过 2000 字。

原因是什么呢？

我们做了一个初步的研究，用不同最大输出长度的数据对 `GLM-4-9B-base` 模型进行 SFT 微调。结果如下：

![](img/562ea31cbc8ed0874c654882570a645d.png)

伴随着要求长度的增加，不同数据集 SFT 微调出来的模型，存在最大输出长度限制。

因此可以得出一个结论，尽管长文本模型在预训练阶段接触了更长的文本序列，但其最大生成长度实际上被 SFT 数据集中输出长度的上限所限制。换句话说，模型“读”到的内容决定了它能“写”多长。

这一发现可以解释当前模型普遍存在的 2000 字输出限制——因为现有的 SFT 数据集很少包含超过此长度的样例；而另一方面，很多数据集都是用现有 LLM 自动构建的，因此它们也继承了源模型的输出长度限制。

## 自动构建长输出数据集

为了解决 SFT 数据集中长输出文本数据缺失的问题，我们设计了一个名为「`AgentWrite`」的 pipeline，通过分解长生成任务，让现有模型来生成更长的具备连贯性的输出。

具体来说，`AgentWrite`

+   首先，会根据用户的输入生成一个详细的写作计划，包括每段内容的结构和目标字数
+   其次，模型依次完成每个子任务，并将生成的段落串联起来，最终形成完整的长文本输出。

![](img/a8406a22d7368ecb78907b7fed51e822.png)

通过这种方法，`AgentWrite` 能够生成超过 20,000 字的高质量文本。

## 教模型生成超长输出

有了一个利用 LLMs 自动生成的更长输出的数据集，我们不禁好奇：是否可以将这种生成超长输出的能力传授给LLMs，使它们能够在单次输出中完成长篇写作任务？

我们从 `GLM-4` 的 SFT 数据中筛选出了 3,000 条指令（主要是中文），另外从 `WildChat-1M`（一个包含用户与ChatGPT/GPT-4对话记录的公开日志）中选择了3,000条指令（主要是英文），通过 `AgentWrite` 并利用 `GPT-4o` 生成了6,000条长输出的SFT数据，即 `LongWriter-6k`。

在模型训练中，为了确保模型的通用能力，我们将`LongWriter-6k`与通用SFT数据（180k 条数据）结合，形成整个训练集。输出长度分布如下：

![](img/0a1ffad19580882cee8c1c1890a35bb2.png)

可以看到`LongWriter-6k`有效地补充了通用SFT数据中2,000字以上输出样例的不足，并且`LongWriter-6k`中的输出长度在2,000至10,000词之间分布相对均匀。

微调（SFT）：我们分别针对 `GLM-4-9B` 和 `Llama-3.1-8B` 做了 SFT 微调，这两个模型都是基础模型，支持最多128k tokens的上下文窗口，非常适合用于长输出的训练。我们得到两个模型：`LongWriter-9B`（`GLM-4-9B-LongWriter`的缩写）和`LongWriter-8B`（`Llama-3.1-8B-LongWriter`的缩写）。

**对齐（`DPO`）**：为了进一步提高模型的输出质量，并增强其在指令中遵循长度约束的能力，我们在经过 SFT 微调的`LongWriter-9B`模型上进行了直接偏好优化。DPO数据来自`GLM-4`的 DPO 数据（大约50,000条），同时包含4k条我们标注的长输出正负样本对。

## 结果怎么样？

我们在`LongBench-Write`上对4个专有模型和5 个开源模型进行了评测，如下表所示：

![](img/75fb12154949ef8ad0df9812ece60c22.png)

其中 `S_l` 表示输出长度是否符合要求的得分。当输出长度符合要求时，得分为100；当输出长度超过要求的4倍或低于要求的1/3时，得分会线性下降至0。

`S_q` 则是从相关性、准确性、连贯性、清晰度、广度与深度以及阅读体验等六个维度，用 `GPT-4o` 进行的打分，并取平均，代表了生成内容的质量。

最终得分 `\overline{S}` 通过 `S_l` 和 `S_q` 的平均值计算得出。

**1**、大多数当前的模型都无法满足超过2,000字的长度要求，而`LongWriter`模型在此类提示下始终提供更长且更丰富的响应。

更具体来说，在`[2k, 4k)`范围内当前的模型通常表现不佳，只有Claude 3.5 Sonnet得分尚可；而在`[4k, 20k)`范围里，几乎所有当前的模型都完全无法达到目标输出长度，甚至得分为0（意味着所有输出长度均小于要求长度的1/3）。

![](img/dbe4ad05bb057294bb4728b12c21d4af.png)

**2、`DPO`（直接偏好优化）有效提升了模型的输出质量及其在长文本生成中遵循长度要求的能力。**

通过比较`LongWriter-9B`和`LongWriter-9B-DPO`的得分，我们发现`DPO`显著提高了 `S_l`（+4%）和 `S_q` （+3%）的得分，并且这种提升在所有范围内都保持一致。这表明在长文本生成场景中，`DPO`依然能够帮助提升模型的输出质量，并能更好地使模型的输出长度与要求的长度对齐。

**3、`LongWriter`模型的输出长度限制大概10k至20k字之间，如果想让模型拥有更长输出的能力，则需要更长输出的数据集。**

![](img/729d22927323a544df61f654b04e081f.png)

在混合后的 SFT 数据集中可以看到，大部分数据都在 20k 以下，超出 20k 的数据不到 100 条。我们相信，在未来构建更长的SFT训练数据，可以进一步突破模型输出长度的限制，达到100k甚至更长的输出长度。

快速上手：[LongWriter：10k+ 输出模型](https://zhipu-ai.feishu.cn/wiki/RiF0wsjK0imjGekisUQcNSUpnsd)
