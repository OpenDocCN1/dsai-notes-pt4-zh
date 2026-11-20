
# 大模型面经——LLama2和chatGLM相对于transformer具体做了哪些优化？

> 来源：[https://geekbang.feishu.cn/docx/TVKIdaEnJoLlSXxRr2ZcKcLhnUq](https://geekbang.feishu.cn/docx/TVKIdaEnJoLlSXxRr2ZcKcLhnUq)

👨🏻 作者：喜欢卷卷的瓦力

📃 作者介绍：985海归硕士、计算机专业，前AI大厂研究院高级算法研究员，前国企研究院算法专家，现外企top10上市集团人工智能研究员，算法研究与开发经历6年，深度参与头部大模型产品研发上线，在大模型/语音识别/视觉等领域有丰富的项目经验

⏰ 收录时间：2024-05-23 11:28:42

🔗 原文链接：h[ttps://mp.weixin.q](https://mp.weixin.qq.com/s/pJTyDMyUfpLCHSB9ed750g)q.com/s/pJTyDMyU...

![](img/e4ddee8fe961d5f4b01056f8d1091a4a.png)

关注作者👸号，获取更多AI知识

本篇从模型结构、细节设计、注意力机制类型、位置编码、LayerNorm与激活函数优化角度，介绍目前较常用的框架LLama2和chatGLM原理，及其相对于transformer的优化。

本篇又是一道面经中的命题作文类题目，看似简单但暗含的细节非常多，回答的时候需要尽量注意有条理有框架。

这里给大家总结一些角度：可以从模型结构是`decoder`或是`transformer`类型的（`encoder` + `decoder`）、模型结构中更细节的设计、注意力机制类型、位置编码、`LayerNorm`、激活函数优化以及效率优化策略分别去聊。

跟前面的面经一样，本篇在写答案时也会尽量避免过于宽泛和官方的用词，并结合一些实际经验。

下面是一个快捷目录。

+ `transformer`
+ `LLama2`
+ `ChatGLM`
+ 总结

## transformer

`transformer`估计做NLP出身的小伙伴都已经非常熟悉了，这里借鉴上面提到的角度来逐一讲一下`transformer`：

### 1. 模型结构设计

`encoder`和`decoder`

`Encoder`：将输入序列转化为一组连续的、固定长度的内部向量，这些向量捕捉了输入序列中的上下文语义信息。`Encoder` 的输出是 `Decoder` 的输入。

`Decoder`：基于 `Encoder` 的输出向量和已经生成的部分输出内容进行计算，生成最终的预测结果。

`Transformer` 的 `Encoder` 组件和 `Decoder` 组件都是由多个结构完全相同的原子组件堆叠而成。

一般情况下，有多少个 `Encoder`，就有多少个 `Decoder`； 但在模型需要压缩的情况下， 考虑到效果损失和效率， 实际用到的`Encoder`个数会比 `Decoder`多。

`Encoder` 和 `Decoder` 是模型参数的主要载体。

这里直接上细节图并结合问题进行讲述：

![](img/d523ff48dfb2a35279400ee10399e6fc.png)

这里主要是考验对模型细节的熟悉，面试官可能会问的问题，包括但不限于：

`transformer`中使用的是`postNorm`还是`preNorm`，为什么

答案： 为什么大模型结构设计中往往使用`postNorm`而不用`preNorm`？

`decoder`中有`cross attention`，请讲一下这个`cross attention`的输入和输出

答案： 输入是`Encoder`的输出和历史上下文经过单向`attention`编码的向量，输出到`FFN`。

介绍一下`transformer`结构中用到的`mask`机制

答案：

1） 预训练任务`Masked Language Model (MLM)` ： 把输入的其中一部分词汇随机掩盖，模型的目标是预测这些掩盖词汇。这种训练方式使得每个位置的`BERT`都能学习到其上下文的信息。

2） `self-attention`中的`Mask` ： 在`attention`中我们不希望一个`token`去注意到为了保持序列长度一致而出现的`padding`部分， 所以`attention`中的`mask`通过 在`softmax`前每个都设为`-inf`（或者实际的场景一个很小的数就可以），然后过完`softmax`后`"padding"`部分的权重就会接近于零， 来处理掉这些无效的信息的。

3） 下游任务的`decoder`中的`Mask`： 遮盖掉`sequence`中当前位置之后信息，以防止模型利用未来信息，也就是信息泄露。`mask`掉后模型的注意力只会集中在此前的序列上。

`transformer`结构常用的参数配置及其对应的训练数据量级 （考验对`transformer`应用熟悉度）

答案： 这里举例一个常用的架构：

`transformer-base`: `e6d6` `embed_dim-512` `ffn_embed_dim-2048` `head-8`

一般是千万量级的数据训练用

### 3. 位置编码：正余弦编码

因为 `Transformer` 不采用 `RNN` 的结构，而是使用全局信息，不能利用单词的顺序信息，而这部分信息对于 NLP 来说非常重要。

所以 `Transformer` 中使用位置 `Embedding` 保存单词在序列中的相对或绝对位置

![](img/039193bde68407126bcaefbe5e968752.png)

其中，`pos` 表示单词在句子中的位置，`d` 表示 `PE`的维度 (与词 `Embedding` 一样)，`2i` 表示偶数的维度，`2i+1` 表示奇数维度 (即 `2i≤d`, `2i+1≤d`)。使用这种公式计算 `PE` 有以下的好处：

使 `PE` 能够适应比训练集里面所有句子更长的句子 ，假设训练集里面最长的句子是有 20 个单词，突然来了一个长度为 21 的句子，则使用公式计算的方法可以计算出第 21 位的 `Embedding`。

可以让模型容易地计算出相对位置 ，对于固定长度的间距 `k`，`PE(pos+k)` 可以用 `PE(pos)`计算得到。因为 `Sin(A+B)` = `Sin(A)``Cos(B)` + `Cos(A)``Sin(B)`, `Cos(A+B)` = `Cos(A)``Cos(B)` - `Sin(A)``Sin(B)`。

备注：将单词的词 `Embedding` 和位置 `Embedding` 相加，就可以得到单词的表示向量`x`，`x`就是 `Transformer` 的输入。

### 4. 注意力机制类型：`MHA`

`Multi-Head Attention` 是由多个 `Self-Attention` 组合形成的，首先将输入`X`分别传递到 `h` 个不同的 `Self-Attention` 中，计算得到 `h` 个输出矩阵`Z`。一般`h`是8，也就是8个头。

![](img/caf4dad27263fefbe1cf23e26e348703.png)

其中`self-attention`具体公式和结构图如下，

![](img/5b5c8b5497bbc1f85d74359b7a8b72a6.png)

![](img/c671f0778e0a86c679831922e6adae79.png)

这里其实又容易引申出其他问题了：

在`decoder`的`cross attention`中 `Q`、`K`、`V`分别是什么

`MHA`、`GQA`、`MQA`有什么区别

这里大家可以自行思考一下，答案基本都在文中了~

### 5. 激活函数

常用的是`GELU`激活函数，有时候考虑到效率也会用`RELU`

## LLaMa2

### 1. 模型结构设计

`llama2`的架构比较容易理解， 只用到`decoder`部分 ，具体结构跟`GPT2`类似； 另外 模型中已经变成了`preNorm` 。

![](img/53f53e352a7c5a1de419710c7cd2336f.png)

这里补充一下`llama`不同模型规模下超参数细节，有时候面试官也会为了考验熟悉程度随口一问。

![](img/42a8ea6906a5672c8eadbcb5ce5010b4.png)

### 2. 位置编码：`RoPE`

具有较好的外推性。可以直接处理任意长的问题。`LLaMA`使用了`Rotary Position Embedding`。对于`Q`的第`m`个位置向量`q`，通过以下方法注入位置编码：

![](img/8eb25905448478a4863fd8c35345aa75.png)

`RoPE`形式上是绝对位置编码，即依赖其绝对位置`m`。

绝对位置编码的优点是计算速度快等，缺点是拓展长度比较麻烦，且绝对位置并没有什么实际意义。

而相对位置编码对学习`token`之间的关系很有意义，比如距离的很远的两个`token`之间的关联大概率很小，使用相对位置编码往往能够获得更好的效果。

此外拓展长度也更容易，因为不论`context size`多长，只需关注最长距离以内的输入即可。相对位置编码的缺点是没有绝对位置编码计算速度快。

因此 `RoPE`为`q`、`k`注入的绝对位置编码，计算得到的`attention`，却变成了相对位置编码 ；这个设计是非常巧妙的，推荐大家看 大语言模型`LLM`为什么要用旋转位置编码（`RoPE`）—— 被`LLaMA`认可的长文本外推性（附代码） 这篇有详细讲解。

### 3. 注意力机制类型：`Group Query Attention`(V2 only)

简单的说，就是一定头数 `Q` 共享一组 `KV`。

![](img/376c07f3f48f6d42c5ac2a58611b362b.png)

自回归模型生成回答时，需要前面生成的`KV`缓存起来，来加速计算。

多头注意力机制(`MHA`)需要的缓存量很大，`Multi-Query Attention`指出多个头之间可以共享`KV`对。`Group Query Attention`没有像`MQA`一样极端，将`query`分组，组内共享`KV`，效果接近`MHA`，速度上与`MQA`可比较。

### 4. 归一化：`RMSNorm`

`Layernorm`换成了`RMSNorm`，而且是`preNorm`。

首先回顾一下`LayerNorm`公式

![](img/6cb710d0f35c44bbfbad47b2b50e3393.png)

`RMSNorm`发现`LayerNorm`的中心偏移没什么用(减去均值等操作)。将其去掉之后，效果几乎不变，但是速度提升了40%。最终公式为

![](img/7044ec9a2abb3bc1b1fb048976d3c65b.png)

注意除了没有减均值，加偏置以外，分母上求的`RMS`而不是方差。

`LLaMA2`在 `Attention Layer`和`MLP`的输入上使用了`RMSNorm`，相比在输出上使用，也就是用`preNorm`替换`postNorm`，训练会更加稳定 。

### 5. 激活函数：`SwiGLU`

`SwiGLU`，有时也被称为`SiLU`。公式为：

![](img/76b0ddda12294fbc2555adba42bd86d5.png)

效果类似平滑版的`ReLU`。

## ChatGLM

### 1. 模型结构设计

`ChatGLM`系列在原始`single Transformer`的基础上进行了一些修改：

1）重组了`LN`和残差连接的顺序；

2）使用单个线性层对输出`token`进行预测；

`ChatGLM`系列的亮点主要还是他的模型设计，融合了自编码、自回归、`encoder-decoder`各类思想，并且有精妙的`span`设计。

![](img/c8a2fa0bd03d70ee02dc8328ef23e0a9.png)

这里简述一下他的模板设计：

自回归： 采样`span`进行单向自回归预测，`A`为`mask`后文本，`B`为`span`

自编码： `A`、`B`互不可见，但内部可见，`B`单向可见

### 2. 注意力机制：`Flash Attention`、`MQA`（v2 only）

`Flash Attention`

`flash-attention`是一种快速、高效、可扩展的注意力机制，它利用了一种称为`hash-aware`的技术，可以根据它们的相似性将输入序列中的元素分配到不同的桶（`bucket`）中。这样，模型只需要计算桶内元素之间的注意力权重，而不是整个序列。这大大减少了计算量和内存需求，同时保持了较高的精度和表达能力。

![](img/1886ce1230211fee9a40e2739d6f345b.png)

简单的说就是， 计算`softmax`时候不需要全量`input`数据，可以分段计算； 反向传播的时候，不存储`attention matrix` (`N^2`的矩阵)，而是只存储`softmax`归一化的系数 。

`MQA`

让 `Q` 仍然保持原来的头数，但 `K` 和 `V` 只有一个头，相当于所有的 `Q` 头共享一组 `K` 和 `V` 头。

### 3. 激活函数：`GeLU`

## 总结

下面我们把上面提到的所有角度简略总结一下。

![](img/cab2e98e61b01e2ecd94d4f56a4419fc.png)
