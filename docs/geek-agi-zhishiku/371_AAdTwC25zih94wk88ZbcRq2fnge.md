
# 为什么现在的LLM都是Decoder-only架构？从理论、训练效率与工程实现等多维度分析

> 来源：[https://geekbang.feishu.cn/docx/DmcVdqi01okXX0xS5s2ctR6Rn7g](https://geekbang.feishu.cn/docx/DmcVdqi01okXX0xS5s2ctR6Rn7g)

> 👨🏻 作者：喜欢卷卷的瓦力

> 📃 作者介绍：985海归硕士、计算机专业，前AI大厂研究院高级算法研究员，前国企研究院算法专家，现外企top10上市集团人工智能研究员，算法研究与开发经历6年，深度参与头部大模型产品研发上线，在大模型/语音识别/视觉等领域有丰富的项目经验

> ⏰ 收录时间：2024-05-23 11:28:42

> 🔗 原文链接：[https://mp.weixin.qq.com/s/_wPu0HUYrNxA4scVqtvEMg](https://mp.weixin.qq.com/s/_wPu0HUYrNxA4scVqtvEMg)

![](img/733bbe001d8fbbd23f8d8e7b6cd06972.png)

关注作者👸号，获取更多AI知识

本篇从理论、训练效率以及工程实现角度来阐述当前主流`Decoder-only`架构的优越性。

`LLM`是“Large Language Model”的简写，目前一般指百亿参数以上的语言模型，主要面向 文本生成 任务。跟小尺度模型（10亿或以内量级）的“百花齐放”不同，目前`LLM`的一个现状是`Decoder-only`架构的研究居多，像OpenAI一直坚持`Decoder-only`的`GPT`系列就不说了，即便是Google这样的并非全部押注在`Decoder-only`的公司，也确实投入了不少的精力去研究`Decoder-only`的模型，如`PaLM`就是其中之一。

那么为什么`Decoder-only`架构会成为`LLM`的主流选择呢？

下面从理论、训练效率以及工程实现角度来说明。

## 一、理论

我们知道所有NLP任务都可以分解为“输入”跟“输出”两部分，我们可以把处理“输入”的模型叫做`Encoder`，生成“输出”的模型叫做`Decoder`，那么所有任务都可以从“`Encoder-Decoder`”的视角来理解，而 不同模型之间的差距在于`Encoder`、`Decoder`的注意力模式以及是否共享参数：

| 模型 | `Encoder` | `Decoder` | 是否 |
| --- | --- | --- | --- |
| `GPT` | 单向 | 单向 | 是 |
| `UniLM` | 双向 | 单向 | 是 |
| `T5` | 双向 | 单向 | 是 |

这里的`GPT`就是`Decoder-only`的代表作；`UniLM`则是跟`GPT`相似的`Decoder`架构，但它是混合的注意力模式；`T5`则是`Encoder-Decoder`架构的代表作，主要是Google比较感兴趣。

### 1. 验证双向注意力/参数量带来的影响

那么首先通过对比实验确认一个问题：“ 以往总是认为`Encoder-Decoder`架构效果好于`Decoder-only`架构，这是双向注意力导致的还是参数翻倍导致的？ ”

根据苏神在10亿参数规模下做的`GPT`和`UniLM`对比实验结果来看，`UniLM`相比`GPT`并无任何优势，甚至某些任务更差，那么可以初步得到结论：“ 输入部分的注意力改为双向不会带来收益，`Encoder-Decoder`架构的优势很可能只是源于参数翻倍 。”

由此也可以推断出，在同等参数量、同等推理成本下，`Decoder-only`架构很可能是最优选择。

### 2. 双向注意力可能存在的低秩问题

为什么双向注意力不能带来收益，目前普遍的一个说法是， 双向注意力带来的 低秩 问题会导致效果下降 。下面来细说这一观点：

`Attention`矩阵本身就是由一个低秩分解矩阵`softmax`而来，具体来说就是一个`n*d`的矩阵与`d*n`的矩阵相乘后再加`softmax`，其中`n`是远远大于`d`的， 这种形式的`Attention`矩阵会因为低秩问题而带来表达能力下降 。

![](img/0b559921831185d1f7e4c5e105908c9f.png)

`Decoder-only`架构的`attention`矩阵是一个下三角，三角阵的行列式等于它对角线元素之积，由于`softmax`的存在，对角线必然都是正数，所以它的行列式必然是正数，即`Decoder-only`架构的`Attention`矩阵一定是满秩的！满秩意味着理论上有更强的表达能力，也就是说，`Decoder-only`架构的`Attention`矩阵在理论上具有更强的表达能力，改为双向注意力反而会变得不足。

### 3. `Decoder-only`的`zero-shot`能力更强

## 二、训练效率与工程实现

### 1. `Encoder-only` 架构更统一，训练、推理速度都更快

在多轮对话或者长序列的时候，`encoder`基本不会使用`casual attention`，那么基于`history context`重新计算一遍开销是很大的；而`decoder-only`采用`casual attention`，之前的计算结果可以保留，只需要计算新的`token`。

### 2. `Encoder-Decoder`架构在巨大参数量下进行并行运算优化时非常容易出问题

`Encoder-Decoder`的架构，当模型参数在百亿量级的时候，必须要并行运算，而`encoder`的架构因为更复杂一旦开始`scale`，任何一个非常小的问题都会被放大。

## 参考文献

[1] 苏剑林. (Mar. 17, 2023). 《为什么现在的`LLM`都是`Decoder-only`的架构？》.
Retrieved from [https://kexue.fm/archives/9529 ]

[2] 为什么现在的`LLM`都是`Decoder only`的架构？
Retrieved from [https://www.zhihu.com/question/588325646]
