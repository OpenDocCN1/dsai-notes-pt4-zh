
# LLM高效微调

> 来源：[https://v11enp9ok1h.feishu.cn/docx/HeledkWOXoycoUxvTwOckS00nXd](https://v11enp9ok1h.feishu.cn/docx/HeledkWOXoycoUxvTwOckS00nXd)

## LoRA

+   Adapter Tuning 在 PLM 基础上添加适配器层会引入额外的计算，带来推理延迟问题

+   Prefix Tuning 难以优化，其性能随可训练参数规模非单调变化，更根本的是，为前缀保留部分序列长度必然会减少用于处理上下游任务的序列长度

> LoRA 论文提出了一种计算和存储高效的低秩(Low-Rank)表示方法，具体地Transformer 等神经网络包含许多执行矩阵乘法的密集层，这些权重矩阵通常具有满秩

+   对于预训练模型的权重矩阵  通过低秩分解(Low-Rank Decomposition) 来表示约束其更新。

+   其中 `B\in R^{d\times r},A\in R^{r\times k},r\ll\min(d,k)`

+   训练开始，用随机高斯分布初始化 `A`, 用 `0` 矩阵初始化 `B`, 保证训练开始旁路矩阵依然是 `0` 矩阵

![](img/c1d5217b7af02e987e40769a0614bba3.png)

![](img/9a2c9970347ad56327bb4b252dc56ddb.png)

[LORA LOW-RANK ADAPTATION OF LARGE.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/R4UxbZD5woBPZWxTHdlcG2kjnbe)

## Prefix-Tuning

Prefix-Tuning 固定 PLM 的所有参数，只更新优化特定任务的 prefix。因此，在生产部署时，只需要存储一个大型 PLM 的副本和一个学习到的特定任务的 prefix, 每个下游任务只产生非常小的额外的计算和存储开销。

## Full VS Emb-only

+   embedding-only 表示只有在 `embedding` 层添加前缀 `token`，而 `full` 表示每一层都添加前缀 `token`；

+   实验表明：prefix-tuning > embedding-only > discrete prompting

## Prifix VS Infix

+   prefix-tuning 表示可训练参数放在开头，infix-tuning 表示可训练 `token` 放在中间位置

+   结果表明：prefix-tuning > infix-tuning

![](img/3a4b5959fe5f697faf4ef33ad62508c3.png)

![](img/b3841626cf090ac417da0e20ff8ce163.png)

[Prefix-Tuning Optimizing Continuous Prompts for Generation.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/SSubbvej0oUXn3xGaJxcOamqnyh)

## Prompt-Tuning

prompt-tuning可以认为是prefix-tuning的简化版。固定整个预训练模型参数，只允许将每个下游任务的额外 `se` 个可更新的 `tokens` (不同于 prefix-tuning, 这里的 `tokens` 是真实的文本) 前置到输入文本中，也没有使用额外的编码层或任务特定的输出层。

> 一系列对比实验，都在说明：随着预训练模型参数的增加，一切的问题都不是问题，最简单的设置也能达到极好的效果。

+   Prompt长度影响：模型参数达到一定量级时，Prompt长度为1也能达到不错的效果，Prompt长度为20就能达到极好效果。

+   Prompt初始化方式影响：Random Uniform 方式明显弱于其他两种，但是当模型参数达到一定量级，这种差异也不复存在。

+   预训练的方式：LM Adaptation 的方式效果好，但是当模型达到一定规模，差异又几乎没有了。

+   微调步数影响：模型参数较小时，步数越多，效果越好。同样随着模型参数达到一定规模，zero shot 也能取得不错效果。

![](img/8b54511c12c967a1189ffec88f40f0f6.png)

![](img/07fd88bcb3e981bb3208d95d42f5ea29.png)

[The Power of Scale for Parameter-Efficient Prompt Tuning.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/RkPmbZVs4oyXZtxp7STcw54Dn9d)

## P-tuning-V1版本

> P-Tuning 的方法思路与 Prefix-Tuning 很相近，P-Tuning 利用少量连续的 `embedding` 参数作为 `prompt` 使 GPT 更好的应用于 NLU 任务，而 Prefix-Tuning 是针对 NLG 任务设计；同时，P-Tuning 只在 `embedding` 层增加参数(使用 `MLP+LSTM` 初始化), 而 Prefix-Tuning 在每一层都添加可训练参数(使用 `MLP` 初始化)。

![](img/bb67f4fcf52b41195a0b8584d52fc7da.png)

P-Tuning 提出将 Prompt 转换为可以学习的 `Embedding` 层，只是考虑到直接对 `Embedding` 参数进行优化会存在这样两个挑战：

+   Discretenes: 对输入正常语料的 `Embedding` 层已经经过预训练，而如果直接对输入的 `prompt` `embedding` 进行随机初始化训练，容易陷入局部最优。

+   Association: 没法捕捉到 `prompt` `embedding` 之间的相关关系。作者提出使用 `MLP+LSTM` 的方式对 `prompt` 进行初始化处理

![](img/9ee602e6be9bb02b5d818061a9d6fc8d.png)

[GPT Understands, Too.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/H2NQbknIUo36ooxrnc3cGTACntg)

## P-tuning-V2版本

![](img/1d228cf6bbdf14df50c6e0875b54e46b.png)

> V2 的思路和 prefix-tuning 相似，在模型的每一层都应用连续的 `prompts` 并对 `prompts` 参数进行更新优化。同时该方法是针对 NLU 任务优化和适配的。

![](img/763ea8b1ebab4bc6eeb24657d029f7b6.png)

[P-Tuning v2 Prompt Tuning Can Be.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/CwBLb3espoKBW4xgCr8czAxznQh)

## QLoRA

可以认为是 LoRA 的升级版本，体现在：

+   `NormalFloat4` (NF4): 一种论文新提出的数据类型，对于正态分布权重而言信息理论上是最优的新数据类型；可以简单认为是 尽可能减少优化过程中数据精度损失的作用；

+   双重量化 (Double Quantization) : 通过量化常数来减少平均内存占用；

+   分页优化器 (Paged Optimizers) : 用于管理内存峰值。

![](img/8c6853d3373f309aa616a4ffaa4f2dc8.png)

![](img/4246c3e8efae7b5495c1fce095b0c28f.png)

[QLORA Efficient Finetuning of Quantized LLMs.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/EF1EbVRFBou6fpx6QWuchr0Jn3e)

## LLM高效微调技术

[Prefix-Tuning Optimizing Continuous Prompts for Generation.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/P6NpbX1W4o6CX9xrlAfcXz83nkb)

![](img/3ed926579fa043624eedec6250ce0f41.png)

[GPT Understands, Too.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/WVAbbeRU3oqjnlxeHyBcH4RQn6d)

![](img/d5275d28353c5fd13d8da7ebaeae2b80.png)

[LORA LOW-RANK ADAPTATION OF LARGE.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/KWLfbIcfLoeGddxcshVchgIEn1c)

![](img/64418b27214a8f75278275e251a3a852.png)

[P-Tuning v2 Prompt Tuning Can Be.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/LjcMbqKR2oAQ80xXW7sc1jMUn7d)

![](img/bd6ff869a41ffd39745a4cee7291e1e7.png)

[QLORA Efficient Finetuning of Quantized LLMs.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Jh2dbTdr0oePYSxGq5iclgOVnge)

![](img/d06b632458475f8478166c8bf4d53afa.png)

[The Power of Scale for Parameter-Efficient Prompt Tuning.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Qcb5bZkyconvZbxs8hGc4n5xn0Z)

<https://v11enp9ok1h.feishu.cn/sync/Ipw1d27gNsv3OAbo3N7cks4ZnJf>
