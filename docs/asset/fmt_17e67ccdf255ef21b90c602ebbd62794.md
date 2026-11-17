
# 大模型微调实践——Prefix tuning与P-tuning v2的原理、区别与代码解析最终章

> 来源：[https://geekbang.feishu.cn/docx/AAvBdc7Rxo0jdlxYguecQGpqn7g](https://geekbang.feishu.cn/docx/AAvBdc7Rxo0jdlxYguecQGpqn7g)

> 👨🏻 作者：喜欢卷卷的瓦力

> 📃 作者介绍：985海归硕士、计算机专业，前AI大厂研究院高级算法研究员，前国企研究院算法专家，现外企top10上市集团人工智能研究员，算法研究与开发经历6年，深度参与头部大模型产品研发上线，在大模型/语音识别/视觉等领域有丰富的项目经验

> ⏰ 收录时间：2024-05-23 11:28:42

> 🔗 原文链接：https://mp.weixin.qq.com/s/RFr_ZLOMGR7LLmNXt0MtyQ

![](img/7961ccf2fde862519af6538dcdc2458d.png)

关注作者👸号，获取更多AI知识

本文承接上篇 `P-tuning`，介绍 `prefix-tuning` 与 `P-tuning v2` 并列出其区别，最后对整个系列的技术做总结。

本篇是本系列最后一篇，没看过前面两篇的强烈建议去看一下。

[大模型微调实践——Prompt tuning、PET、Prefix tuning、P-tuning的原理、区别与代码解析(一)](http://mp.weixin.qq.com/s?__biz=Mzg4MTkwMTQ4NA==&mid=2247484100&idx=1&sn=9a16611524c7953361717769284e7802&chksm=cf5fa807f8282111d1c8912ce305072f3e5558506d45b4d634d8e315cb84bdb7b5d92a7747c6&scene=21#wechat_redirect)

[大模型微调实践——Prompt tuning、PET、Prefix tuning、P-tuning的原理、区别与代码解析(二)](http://mp.weixin.qq.com/s?__biz=Mzg4MTkwMTQ4NA==&mid=2247484109&idx=1&sn=5757e1179c610e69e777b555b60787b0&chksm=cf5fa80ef82821189852897cee5741ea0cdc637ceb3b7e91bdd85715d37d74417814cec55071&scene=21#wechat_redirect)

因为 `prefix tuning` 和 `P-tuning v2` 非常相似，所以放在一起来介绍。简易目录如下：

+   `Prefix-tuning`
+   `P-tuning v2`

跟以前一样，具体还是会分为论文、原理、实现细节以及对应关键代码进行详细介绍，因为 `prefix tuning` 和 `P-tuning v2` 在官方库中基本是合并在一起的，所以代码放在一起讲，最后对所有方法做一个总结区分。

在开始前，我们再回忆一下自动构建模板微调模型的关键点：

1. 对 `virtual token` 的处理：随机初始化或者基于一个可训练学习的模型编码

2. 需要参与微调的参数：某一层、全量或者其他选择

3. 调整适配下游任务的方式

那么可以对应这三点看微调技术之间的区别就比较清晰。

## Prefix tuning

### 一、论文

`Prefix-Tuning: Optimizing Continuous Prompts for Generation`

这篇论文本质也是把传统人工设计模版中的真实 `token` 替换成可微的 `virtual token`；该方法在输入 `token` 之前构造一段任务相关的 `virtual tokens` 作为 `Prefix`；然后，在训练的时候只更新 `Prefix` 部分的参数，而 `PLM` 中的其他部分参数固定。

### 二、原理与具体实现

#### 1. 适配不同任务的 `prefix` 构造形式

针对自回归架构模型：在句子前面添加前缀，得到 `z = [PREFIX; x; y]` ，合适的上文能够在固定 `LM` 的情况下去引导生成下文（比如：`GPT3` 的上下文学习）。

针对编码器-解码器架构模型：`Encoder` 和 `Decoder` 都增加了前缀，得到 `z = [PREFIX; x; PREFIX0; y]` 。`Encoder` 端增加前缀是为了引导输入部分的编码，`Decoder` 端增加前缀是为了引导后续 `token` 的生成

![](img/95a17f3280d445480be47991219b1d24.png)

#### 2. 对 `virtual token` 的编码方式

同时，为了防止直接更新 `Prefix` 的参数导致训练不稳定和性能下降的情况，在 `Prefix` 层前面加了 `MLP` 结构，训练完成后，只保留 `Prefix` 的参数。

![](img/f5b18b037d28758836857716e44675fb.png)

除此之外，通过消融实验证实，只调整 `embedding` 层的表现力不够，将导致性能显著下降，因此，在每层都加了 `prompt` 的参数，改动较大。

![](img/e719271d482f81b12c2da51a8a3a7cf7.png)

## P-tuning v2

### 一、论文

`P-Tuning v2: Prompt Tuning Can Be Comparable to Fine-tuning Universally Across Scales and Tasks`

之前的 `Prompt Tuning` 和 `P-Tuning` 等方法存在两个主要的问题：

第一，缺乏模型参数规模和任务通用性。

缺乏规模通用性：`Prompt Tuning` 论文中表明当模型规模超过 100 亿个参数时，提示优化可以与全量微调相媲美。但是对于那些较小的模型（从 100M 到 1B），提示优化和全量微调的表现有很大差异，这大大限制了提示优化的适用性。

缺乏任务普遍性：尽管 `Prompt Tuning` 和 `P-tuning` 在一些 `NLU` 基准测试中表现出优势，但提示调优对硬序列标记任务（即序列标注）的有效性尚未得到验证。

第二，缺少深度提示优化，在 `Prompt Tuning` 和 `P-tuning` 中，连续提示只被插入 `transformer` 第一层的输入 `embedding` 序列中，在接下来的 `transformer` 层中，插入连续提示的位置的 `embedding` 是由之前的 `transformer` 层计算出来的，这可能导致两个可能的优化挑战。

由于序列长度的限制，可调参数的数量是有限的。

输入 `embedding` 对模型预测只有相对间接的影响。

考虑到这些问题，作者提出了 `Ptuning v2`，它利用深度提示优化（如：`Prefix Tuning`），对 `Prompt Tuning` 和 `P-Tuning` 进行改进，作为一个跨规模和 `NLU` 任务的通用解决方案。

该方法在每一层都加入了 `Prompts tokens` 作为输入，而不是仅仅加在输入层，这带来两个方面的好处：

更多可学习的参数（从 `P-tuning` 和 `Prompt Tuning` 的 0.01% 增加到 0.1%-3%），同时也足够参数高效。

加入到更深层结构中的 `Prompt` 能给模型预测带来更直接的影响。

### 二、原理与具体实现

具体做法基本同 `Prefix Tuning`，可以看作是将文本生成的 `Prefix Tuning` 技术适配到 `NLU` 任务中，然后做了一些改进：

#### 移除重参数化的编码器

以前的方法利用重参数化功能来提高训练速度和鲁棒性（如：`Prefix Tuning` 中的 `MLP`、`P-Tuning` 中的 `LSTM`）。在 `P-tuning v2` 中，作者发现重参数化的改进很小，尤其是对于较小的模型，同时还会影响模型的表现。

#### 针对不同任务采用不同的提示长度

提示长度在提示优化方法的超参数搜索中起着核心作用。在实验中，发现不同的理解任务通常用不同的提示长度来实现其最佳性能，这与 `Prefix-Tuning` 中的发现一致，不同的文本生成任务可能有不同的最佳提示长度。

#### 引入多任务学习

先在多任务的 `Prompt` 上进行预训练，然后再适配下游任务。多任务学习对我们的方法来说是可选的，但可能是相当有帮助的。一方面，连续提示的随机性给优化带来了困难，这可以通过更多的训练数据或与任务相关的无监督预训练来缓解；另一方面，连续提示是跨任务和数据集的特定任务知识的完美载体。我们的实验表明，在一些困难的序列任务中，多任务学习可以作为 `P-tuning v2` 的有益补充。

#### 回归传统的分类标签范式

标签词映射器（`Label Word Verbalizer`）一直是提示优化的核心组成部分，它将 `one-hot` 类标签变成有意义的词，以利用预训练语言模型头。尽管它在 `few-shot` 设置中具有潜在的必要性，但在全数据监督设置中，`Verbalizer` 并不是必须的，它阻碍了提示调优在我们需要无实际意义的标签和句子嵌入的场景中的应用。因此，`P-Tuning v2` 回归传统的 `CLS` 标签分类范式，采用随机初始化的分类头（`Classification Head`）应用于 `tokens` 之上，以增强通用性，可以适配到序列标注任务。

### 三、代码详解

代码部分把 `prefix tuning` 和 `P-tuning v2` 放在一起写（`Hugging face` 官方库也是合在一起的），因为 `virtual token` 是插入进每一层的，那么需要重点关注的是他怎么进入每一层的：

本质上是通过 `past_key_values` 进入 `attention` 运算内部，在每一层中运算的。

这里先列出 `Hugging face` 原生库中怎么实现的，然后再写一段仿真代码便于比较直观简易的理解。

#### Huggingface 官方版本

##### 1. 设置 `virtual_token` 的数量及 `past_key_values` 维度

![](img/5e5828443c392a9ca1abd2df246acd6b.png)

##### 2. `prefix encoder` 类的定义

这里如果 `self.prefix_projection` 为 `True`，就是 `prefix tuning`；如果 `self.prefix_projection` 为 `False`，就是 `P-tuning v2`。

![](img/5ac3720f6e71abbb879c7575ae33e830.png)

##### 3. `past_key_values` 如何进入各层参与训练

这里从类 `ChatGLMModel` 中 `forward` 函数开始，逐步深挖 `past_key_values` 的相关运算。

###### 1）类 `ChatGLMModel` 中 `forward` 函数

![](img/6907b4d1ccfa7a5a951cf641fc059177.png)

从上述代码 `kv_caches=past_key_values` 中可以看到，关键是 `encoder` 函数中的 `kv_caches` 变量，我们继续挖。

###### 2）`encoder` 类

这里具体以类 `GLMTransformer` 中的 `forward` 函数为例。

```
hidden_states,                  attention_mask,                  rotary_pos_emb,                  kv_cache=kv_caches[index],                  use_cache=use_cache              )          hidden_states, kv_cache = layer_ret          if use_cache:              presents = presents + (kv_cache,)
```

我们重点关注 `kv_cache` 变量，可以看到这里分别加入了每一层并在 `layer` 函数中进行运算。

###### 3）`layers` 类

这里以 `GLMBlock` 为例，列出其 `forward` 函数。

![](img/eb21da7f6469065e2b6177b620947214.png)

###### 4）`self_attention` 类

终于找到了，我们直接看 `forward` 函数。

![](img/6dd61d08df4394f882e9b6188e160aae.png)

可以看到 `kv_cache` 实际是通过跟 `key_layer` 和 `value_layer` 连接，并入每一层运算的。

为了便于大家结合维度更深入理解，下面提供仿真代码来解释计算过程。

+   仿真代码

仿真代码这边就不需要关注 `prefix encoder` 类了，只需要记住加上 `MLP` 就是 `prefix tuning`，不加就是 `P-tuning v2`；这里就只写 `past_key_values` 的运算过程。

下面上代码，注意看运算向量维度的变化。

![](img/91344449cd83a126e6a33a4f3c5be30e.png)

![](img/5895c4f5ae4e2760706479150fc7b7a2.png)

至此，我们就把 `Prompt Tuning`、`Pattern-Exploiting Training`（`PET`）、`prefix tuning`、`P-tuning`、`P-tuning v2` 全部介绍完了。最后我们先来做个粗略的总结：

`Prompt Tuning`：是个比较宽泛的概念，指按照特定模板去微调大模型，这个模板可以有两种形式：离散的或是连续的；

`PET`：这种模板是人工构建的，也就是离散的，其中被 `MASK` 掉的是自然语言；

`prefix tuning`、`P-tuning`、`P-tuning v2`：这种模板是连续的，可单独求导求参数。

针对 `prefix tuning`、`P-tuning`、`P-tuning v2`，我们再做比较细致的区别，根据我们前面说的自动构建模板微调模型的三个关键点：

1. 对 `virtual token` 的处理：随机初始化或者基于一个可训练学习的模型编码

2. 需要参与微调的参数：某一层、全量或者其他选择

3. 调整适配下游任务的方式

我们列出具体区分的表格。

![](img/4362e3271a52bbf0bf5d3b03f6d04e59.png)
