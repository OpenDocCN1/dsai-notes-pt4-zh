# 4.自注意力机制

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/EOLHdpsLloUSNUxOtvBcevrsnvd](https://qqs7y1hozd1.feishu.cn/docx/EOLHdpsLloUSNUxOtvBcevrsnvd)

# **Attention I**s All You Need 注意力是你所需要的一切

## 摘要

The dominant sequence transduction models are based on complex recurrent or convolutional neural networks that include an encoder and a decoder. The best performing models also connect the encoder and decoder through an attention mechanism. We propose a new simple network architecture, the Transformer, based solely on attention mechanisms, dispensing with recurrence and convolutions entirely. Experiments on two machine translation tasks show these models to be superior in quality while being more parallelizable and requiring significantly less time to train. Our model achieves 28.4 BLEU on the WMT 2014 Englishto-German translation task, improving over the existing best results, including ensembles, by over 2 BLEU. On the WMT 2014 English-to-French translation task, our model establishes a new single-model state-of-the-art BLEU score of 41.0 after training for 3.5 days on eight GPUs, a small fraction of the training costs of the best models from the literature.

**注意力****是你所需要的一切**主导的序列转导模型是基于复杂的递归或卷积神经网络，包括一个编码器和一个解码器。性能最好的模型还通过注意机制将编码器和解码器连接起来。我们提出了一个新的简单的网络结构–Transformer，它只基于注意力机制，完全不需要递归和卷积。在两个机器翻译任务上的实验表明，这些模型在质量上更胜一筹，同时也更容易并行化，需要的训练时间也大大减少。我们的模型在WMT 2014英德翻译任务中达到了28.4 BLEU，比现有的最佳结果（包括合集）提高了2 BLEU以上。在WMT 2014英法翻译任务中，我们的模型在8个GPU上训练了3.5天后，建立了新的单模型最先进的BLEU得分，即41.0分，这只是文献中最佳模型的训练成本的一小部分。

## 1 介绍

**Recurrent n**eural networks, long short-term memory [12] and gated recurrent [7] neural networks in particular, have been firmly established as state of the art approaches in sequence modeling and transduction problems such as language modeling and machine translation [29, 2, 5]. Numerous efforts have since continued to push the boundaries of recurrent language models and encoder-decoder architectures [31, 21, 13].

**循环****神经网**络，特别是长短时记忆[12]和门控递归[7]神经网络，已经被牢固地确立为序列建模和转换问题的最先进方法，如语言建模和机器翻译[29, 2, 5]。此后，大量的努力继续推动着递归语言模型和编码器-解码器架构的边界[31, 21, 13]。

**Recur****r****ent models typically fac****t****or computatio**n along the symbol positions of the input and output sequences. Aligning the positions to steps in computation time, they generate a sequence of hidden states  ，as a function of the previous hidden state   and the input for position t. This inherently sequential nature precludes parallelization within training examples, which becomes critical at longer sequence lengths, as memory constraints limit batching across examples. Recent work has achieved significant improvements in computational efficiency through factorization tricks [18] and conditional computation [26], while also improving model performance in case of the latter. The fundamental constraint of sequential computation, however, remains.

**循****环****模型通常沿着输入和输****出****序列的**符号位置进行计算。将位置与计算时间的步骤相一致，它们产生一连串的隐藏状态 ，作为前一个隐藏状态 和位置t的输入的函数。这种固有的顺序性排除了训练实例内的并行化，这在较长的序列长度上变得至关重要，因为内存限制了跨实例的批处理。最近的工作通过因式分解技巧[18]和条件计算[26]在计算效率方面取得了重大改进，同时在后者的情况下也提高了模型性能。然而，顺序计算的基本约束仍然存在。

**Attention** mechanisms have become an integral part of compelling sequence modeling and transduction models in various tasks, allowing modeling of dependencies without regard to their distance in the input or output sequences [2, 16]. In all but a few cases [22], however, such attention mechanisms are used in conjunction with a recurrent network.

**注意力机制已经成为各种任务中引人注目的序列建模和转导模型的一个组成部分，允许对依赖关系进行建模，**而不考虑它们在输入或输出序列中的距离[2, 16]。然而，除了少数情况[22]，这种注意力机制都是与循环网络一起使用的。

**In this wo**rk we propose the Transformer, a model architecture eschewing recurrence and instead relying entirely on an attention mechanism to draw global dependencies between input and output. The Transformer allows for significantly more parallelization and can reach a new state of the art in translation quality after being trained for as little as twelve hours on eight P100 GPUs.

**在这项**工作中，我们提出了Transformer，这是一个避免循环的模型结构，而是完全依靠注意力机制来得出输入和输出之间的全局依赖关系。Transformer允许更多的并行化，并且在8个P100 GPU上经过短短12小时的训练，就可以达到翻译质量的新水平。

## 2 背景

The goal of reducing sequential computation also forms the foundation of the Extended Neural GPU [20], ByteNet [15] and ConvS2S [8], all of which use convolutional neural networks as basic building block, computing hidden representations in parallel for all input and output positions. In these models, the number of operations required to relate signals from two arbitrary input or output positions grows in the distance between positions, linearly for ConvS2S and logarithmically for ByteNet. This makes it more difficult to learn dependencies between distant positions [11]. In the Transformer, this is reduced to a constant number of operations, albeit at the cost of reduced effective resolution due to averaging attention-weighted positions, an effect we counteract with Multi-Head Attention as described in section 3.2.

**减少顺序计算的**目标也构成了扩展神经GPU[20]、ByteNet[15]和ConvS2S[8]的基础，它们都使用卷积神经网络作为基本构件，对所有输入和输出位置并行计算隐藏表征。在这些模型中，将来自两个任意输入或输出位置的信号联系起来所需的操作数量随着位置之间的距离增长，对于ConvS2S是线性增长，对于ByteNet是对数增长。这使得学习遥远位置之间的依赖关系更加困难[11]。在Transformer中，这被减少到一个恒定的操作数，尽管代价是由于注意力加权位置的平均化而降低了有效的分辨率，如第3.2节所述，我们用多头注意力抵消了这种影响。

**Self-atten**tion, sometimes called intra-attention is an attention mechanism relating different positions of a single sequence in order to compute a representation of the sequence. Self-attention has been used successfully in a variety of tasks including reading comprehension, abstractive summarization, textual entailment and learning task-independent sentence representations [4, 22, 23, 19].

**自注意力，有时也被称为内部注意力，是一种与单个序列的不同位置相关的注意力机制，以计算该序列的表示。自**注意力已被成功地用于各种任务，包括阅读理解、抽象概括、文本连带和学习与任务无关的句子表示[4, 22, 23, 19]。

**End-to-end memory networks are based on a recurrent attention mechanism instead of sequencealigne**d recurrence and have been shown to perform well on simple-language question answering and language modeling tasks [28].

**端**到端记忆网络是基于递归注意机制力，而不是序列排列的递归，并已被证明在简单语言问题回答和语言建模任务中表现良好[28]。

**To the best of our knowledge, however, the Transformer is the first transduction model relying entirely on self-attention to compute representations of its input and output **without using sequencealigned RNNs or convolution. In the following sections, we will describe the Transformer, motivate self-attention and discuss its advantages over models such as [14, 15] and [8].

**然而，据我们所知，Transformer是第一个完全依靠自注意力来计算其输入和输出的表征而不使用序列对齐的RNN或卷积的转导模**型。在下面的章节中，我们将描述Transformer，激励自注意力，并讨论它与[14, 15]和[8]等模型相比的优势。

## 3 模型架构

**Most**** ****competitive neural sequen****c****e**** ****tran**sduction models have an encoder-decoder structure [5, 2, 29]. Here, the encoder maps an input sequence of symbol representations  to a sequence of continuous representations  . Given z, the decoder then generates an output sequence   of symbols one element at a time. At each step, the model is auto-regressive [9], consuming the previously generated symbols as additional input when generating the next.

**大多数****有****竞争力的神经序列转****导****模****型****有一个编码****器**-解码器结构[5, 2, 29]。这里，编码器将输入的符号表示序列 映射为连续表示序列 。给定z后，解码器产生一个输出序列 的符号，每次一个元素。在每一步，该模型都是自动循环的[9]，在生成下一步时，消耗先前生成的符号作为额外的输入。

The Transformer follows this overall architecture using stacked self-attention and point-wise, fully connected layers for both the encoder and decoder, shown in the left and right halves of Figure 1, respectively.

Transformer遵循这一整体架构，在编码器和解码器中都使用了堆叠式自注意力和点式全连接层，分别在图1的左半部和右半部显示。

### 3.1 编码器和解码器堆栈

**E**ncoder: The encoder is composed of a stack of N = 6 identical layers. Each layer has two sub-layers. The first is a multi-head self-attention mechanism, and the second is a simple, position-wise fully connected feed-forward network. We employ a residual connection [10] around each of the two sub-layers, followed by layer normalization [1]. That is, the output of each sub-layer is LayerNorm(x + Sublayer(x)), where Sublayer(x) is the function implemented by the sub-layer itself. To facilitate these residual connections, all sub-layers in the model, as well as the embedding layers, produce outputs of dimension

**编码器。编****码****器是由N**=6个相同的层堆叠而成。每层有两个子层。第一层是一个多头的自注意力机制，第二层是一个简单的、位置上的全连接前馈网络。我们在两个子层的每一个周围采用一个残差连接[10]，然后进行层的正常化[1]。也就是说，每个子层的输出是LayerNorm(x + Sublayer(x))，其中Sublayer(x)是由子层本身实现的函数。为了方便这些剩余连接，模型中的所有子层以及嵌入层都会产生尺寸为 的输出。

Decoder: The decoder is also composed of a stack of N = 6 identical layers. In addition to the two sub-layers in each encoder layer, the decoder inserts a third sub-layer, which performs multi-head attention over the output of the encoder stack. Similar to the encoder, we employ residual connections around each of the sub-layers, followed by layer normalization. We also modify the self-attention sub-layer in the decoder stack to prevent positions from attending to subsequent positions. This masking, combined with fact that the output embeddings are offset by one position, ensures that the predictions for position i can depend only on the known outputs at positions less than i.

**解码器。解码器也是由N=6个相同层的堆栈组成。除了每个编码器层的两个子层之外，解码器还插入了第三个子层，它对编码器堆栈的输出进行多头注意力。与编码器类似，我们在每个**子层周围采用剩余连接，然后进行层的归一化。我们还修改了解码器堆栈中的自注意力子层，以防止位置关注后续位置。这种屏蔽，再加上输出嵌入偏移一个位置的事实，确保对位置i的预测只取决于小于i的位置的已知输出。

### 3.2 注意力

**An attention function can be described as mapping a query and a set of key-value pairs to an output, where the query, keys, values, and output are all vecto**rs. The output is computed as a weighted sum of the values, where the weight assigned to each value is computed by a compatibility function of the query with the corresponding key.

**注意力函数可以被描述为将一个queries和一组keys、values对映射到一个输出，其中queries、key**s、values和输出都是向量。输出被计算为数值的加权和，其中分配给每个数值的权重是由查询与相应的键的兼容性函数计算的。

#### 3.2.1 缩放点积注意力

**We ****c****a****ll our particular attention “Scal****e****d** Dot-Product Attention” (Figure 2). The input consists of queries and keys of dimension  , and values of dimension  . We compute the dot products of the query with all keys, divide each by  , and apply a softmax function to obtain the weights on the values.

**我****们把我们的特殊关注称为 “缩放点积关注****”**（图2）。输入包括维度为 的values。我们计算queries与所有keys的点积，将每个keys除以 ，并应用softmax函数来获得数值的权重。

**In practice, we compute the attention function on a set of queries simultaneously, packed together in**to a matrix Q. The keys and values are also packed together into matrices K and V . We compute the matrix of outputs as:

**在**实践中，我们同时计算一组queries的注意函数，并将其打包成矩阵Q，keys和values也被打包成矩阵K和V。我们计算输出的矩阵为:

**The t****w****o most c**ommonly used attention functions are additive attention [2], and dot-product (multiplicative) attention. Dot-product attention is identical to our algorithm, except for the scaling factor of  . Additive attention computes the compatibility function using a feed-forward network with a single hidden layer. While the two are similar in theoretical complexity, dot-product attention is much faster and more space-efficient in practice, since it can be implemented using highly optimized matrix multiplication code.

**两****种最常用的注意力函数是加法注意力[2]，和点乘法（乘法）注意力。除了 个比例因子外，点乘法注意力与我们**的算法相同。加法注意力使用具有单个隐藏层的前馈网络来计算兼容性函数。虽然两者在理论上的复杂性相似，但点积式注意力在实践中要快得多，而且空间效率更高，因为它可以用高度优化的矩阵乘法代码来实现。

**W****hil****e**** for small values of **** **** the two mechanisms perform similarly, additive attention outperforms dot product ****a****t**tention without scaling for larger values of  [3]. We suspect that for large values of  , the dot products grow large in magnitude, pushing the softmax function into regions where it has extremely small gradients. To counteract this effect, we scale the dot products by  .

**虽然对于**** ****的****小值来说，这两种机制的表现相似，但对****于**** ****的****大**值来说，加法注意力优于点积注意力，且不存在扩展性[3]。我们怀疑对于大的 值来说，点积的幅度很大，把softmax函数推到它的梯度极小的区域。为了抵消这种影响，我们将点积的比例定为 。

#### 3.2.2 多头注意力

**I****n****stea****d**** o****f**** per****f****orming a single attention function with  -dimensional keys, values and q****uer**ies, we found it beneficial to linearly project the queries, keys and values h times with different, learned linear projections to  ,  and  dimensions, respectively. On each of these projected versions of queries, keys and values, we then perform the attention function in parallel, yielding  -dimensional output values. These are concatenated and once again projected, resulting in the final values, as depicted in Figure 2.

**我们发现，与其用**** ****维的keys、values和queries来执行一个单一的注意函数，****不****如用****不****同****的****、学****习过的线性投影将queries、keys和values分别投影到 ,  和 维，这样做是有益的。在这些投射的查询、键和值的每个版本上，我们再平行地执行注意函数，产生d v d_v****d**v维的输出值。这些值被串联起来，并再次进行投影，得到最终的数值，如图2所示。

Multi-head attention allows the model to jointly attend to information from different representation subspaces at different positions. With a single attention head, averaging inhibits this.

多头注意允许模型在不同的位置上共同关注来自不同表征子空间的信息。在单头注意的情况下，平均化抑制了这一点。

**Where the projections ar**e parameter matrices

其中投影是参数矩阵

In this work we employ   parallel attention layers, or heads. For each of these we use 。

由于每个头的维度减少，总的计算成本与全维度的单头注意相似。

#### 3.2.3 注意力在我们模型中的应用

**T**he Transformer uses multi-head attention in three different ways:

**　• In “encoder-decoder attention” layers, the queries come from the previous decoder layer, and the memory keys and values come from the output of the encoder. This ****allows every position in the decoder to attend over all positions in the input sequence. This mimics the typical encoder-decoder atte****ntion mechanisms in sequence-to-sequence models such as [31, 2, 8].

• The encoder contains self-attention layers. In a self-attention layer all of the keys, values and queries come from the same place, in **this case, the output of the previous layer in the encoder. Each position in the encoder can attend to all positions in the previous layer of the encoder.

• Similarly, self-attention layers in the decoder allow each position in the decoder to attend to all positions in the decoder up to and including that position. We need to prevent leftward information flow in the decoder to preserve the auto-regressive property. We implement this inside of scaled dot-product attention by masking out (setting to −∞) all values in the input of the softmax which correspond to illegal connections. See Figure 2.

**Transformer 以三种不同的方式使用多头关注。

- 在 "编码器-解码器注意 "层中，queries来自前一个解码器层****，而****key**s和values来自编码器的输出。这允许解码器中的每个位置都关注输入序列中的所有位置。这模仿了序列到序列模型中典型的编码器-解码器注意力机制，如[31, 2, 8]。

- 编码器包含自注意力层。在自注意力层中，所有的keys、values和queries都来自同一个地方，在这种情况下，就是编码器中前一层的输出。编码器中的每个位置都可以关注到编码器前一层的所有位置。

- 同样，解码器中的自注意力允许解码器中的每个位置关注解码器中的所有位置，直到并包括该位置。我们需要防止解码器中的信息向左流动，以保持自动回归的特性。我们通过掩盖（设置为-∞）softmax输入中对应于非法连接的所有值，在缩放点积注意力内部实现这一点。见图2。

### 3.3 定位前馈网络

**In addi**tion to attention sub-layers, each of the layers in our encoder and decoder contains a fully connected feed-forward network, which is applied to each position separately and identically. This consists of two linear transformations with a ReLU activation in between.

**除了注意子层之外，我们的编码器和解码器中的每一层都包含一个全连接的前馈网络，该网**络分别适用于每个位置，并且完全相同。这包括两个线性变换，中间有一个ReLU激活。

**While ****t****he linear transformat****i****o**ns are the same across different positions, they use different parameters from layer to layer. Another way of describing this is as two convolutions with kernel size 1. The dimensionality of input and output is  , and the inner-layer has dimensionality  .

**虽****然****线性变换在不同****的****位**置上是相同的，但它们在不同的层上使用不同的参数。另一种描述方式是将其作为两个内核大小为1的卷积。输入和输出的维度为 ，内层的维度为 。

### 3.4 嵌入和Softmax

**Similarly to other sequence transduction models, we use learned e****m****beddings to convert the input tokens and output tokens to vectors of dimension  . We also use the usual learned linear transformation and softmax function to**** ****c**onvert the decoder output to predicted next-token probabilities. In our model, we share the same weight matrix between the two embedding layers and the pre-softmax linear transformation, similar to [24]. In the embedding layers, we multiply those weights by  .

**与其他序列转换模型类似，我们使用学****习****的嵌入将输入标记和输出标记转换为维度为 的向量。我们还使用通常的学习线性变换和softmax函数将解码器输出转****换****为**预测的下一个标记概率。在我们的模型中，我们在两个嵌入层和pre-softmax线性变换之间共享相同的权重矩阵，与[24]相似。在嵌入层中，我们将这些权重乘以 。

### 3.5 位置编码

**Since our model contains no recurrence and no convolution, in order for the model to make use of the order of the sequence, we must inject some information about the relative or absolute position of the tokens in the sequence. To this end,** we add “positional encodings” to the input embeddings at the bottoms of the encoder and decoder stacks. The positional encodings have the same dimension model as the embeddings, so that the two can be summed. There are many choices of positional encodings, learned and fixed [8].

**由于我们的模型不包含递归和卷积，为了使模型能够利用序列的顺序，我们必须注入一些关于序列中标记的相对或绝对位置的信息。为此，我们在编码器****和解码器堆栈的底部为输入嵌入添加 “位置编码”。位置编码与嵌入具**有相同的维度dmodel，因此这两者可以相加。有很多位置编码的选择，有学习的，也有固定的[8]。

In this work, we use sine and cosine functions of different frequencies:

在这项工作中，我们使用不同频率的正弦和余弦函数。

**where pos**** ****is the position and i i****s**** **the dimension. That is, each dimension of the positional encoding corresponds to a sinusoid. The wavelengths form a geometric progression from 2π to 10000 · 2π. We chose this function because we hypothesized it would allow the model to easily learn to attend by relative positions, since for any fixed offset k,  can be represented as a linear function of  .

**其中pos是位置，i是维度。也就是说，位置编码的每个维度对应于一个正弦波。波长形成一个从2π到100****0****0·2π的几****何****级数。我们选**择这个函数是因为我们假设它能使模型很容易地学会通过相对位置参加，因为对于任何固定的偏移量k， 可以被表示为 的线性函数。

**We also **experimented with using learned positional embeddings [8] instead, and found that the two versions produced nearly identical results (see Table 3 row (E)). We chose the sinusoidal version because it may allow the model to extrapolate to sequence lengths longer than the ones encountered during training.

**我们还试验了用学习的位置嵌入[8]来代替，发现这两个版本产生了几乎相同的结果（见表3行（E））**。我们选择了正弦波版本，因为它可能允许模型推断出比训练期间遇到的长度更长的序列。

## 4 为什么是自注意力

**In this section we compare various aspects of self-attention layers to the recurrent and co****n****volutional layer****s**** commonl****y**** u****sed for mapping one variable-length sequence of symbol representati**ons  to another sequence of equal length   , with  ,  , such as a hidden layer in a typical sequence transduction encoder or decoder. Motivating our use of self-attention, we consider three desiderata.

**在这一节中，我们将自注意力层的各****个****方面与通常用于将一个变****长****的符号表示序列 映射到另一个等****长****的序****列****  的递归和卷积层进行比较，其中 ,  , 如典**型序列转换编码器或解码器的隐藏层。我们使用自我关注的动机是考虑到三个方面的问题。

**One is the total computational complexity per layer. Another is the amount of comput**ation that can be parallelized, as measured by the minimum number of sequential operations required.

**一个是每层的总计算复杂性。另一个是可以并行化**的计算量，以所需的最小顺序操作数来衡量。

The third is the path length between long-range dependencies in the network. Learning long-range dependencies is a key challenge in many sequence transduction tasks. One key factor affecting the ability to learn such dependencies is the length of the paths forward and backward signals have to traverse in the network. The shorter these paths between any combination of positions in the input and output sequences, the easier it is to learn long-range dependencies [11]. Hence we also compare the maximum path length between any two input and output positions in networks composed of the different layer types.

**第三是网**络中长距离依赖关系之间的路径长度。学习长距离的依赖关系是许多序列转导任务中的一个关键挑战。影响学习这种依赖关系能力的一个关键因素是前向和后向信号在网络中必须穿越的路径的长度。在输入和输出序列的任何位置组合之间的这些路径越短，就越容易学习长距离的依赖关系[11]。因此，我们也比较了由不同层类型组成的网络中任何两个输入和输出位置之间的最大路径长度。

As noted in Table 1, a self-attention layer connects all positions with a constant number of sequentially executed operations, whereas a recurrent layer requires O(n) sequential operations. In terms of computational complexity, self-attention layers are faster than recurrent layers when the sequence length n is smaller than the representation dimensionality d, which is most often the case with sentence representations used by state-of-the-art models in machine translations, such as word-piece [31] and byte-pair [25] representations. To improve computational performance for tasks involving very long sequences, self-attention could be restricted to considering only a neighborhood of size r in the input sequence centered around the respective output position. This would increase the maximum path length to O(n/r). We plan to investigate this approach further in future work.

**如表1所示，自注意力层以恒定数量的顺序执行操作连接所有位置，而递归层需要O(n)顺序操作。就计算复杂度而言，当序列长度n小于表示维度d时，自注意层比递归层更快，这在机器翻译中最先进的模型所使用的句子表示中是最**常见的情况，如词片[31]和字节对[25]表示。为了提高涉及超长序列的任务的计算性能，自我关注可以被限制为只考虑输入序列中以各自输出位置为中心的大小为r的邻域。这将使最大路径长度增加到O(n/r)。我们计划在未来的工作中进一步研究这种方法。

**A sin****g****le convolutional layer with kernel width k < n does not connect all pairs of input and output positions. Doing so requires a stack of O(n****/****k) convoluti****o****nal **layers in the case of contiguous kernels, or   in the case of dilated convolutions [15], increasing the length of the longest paths between any two positions in the network. Convolutional layers are generally more expensive than recurrent layers, by a factor of k. Separable convolutions [6], however, decrease the complexity considerably, to  . Even with   , however, the complexity of a separable convolution is equal to the combination of a self-attention layer and a point-wise feed-forward layer, the approach we take in our model.

**一个内核宽度为k<n的单一卷积层并不能连接所有的输入和输出位置对。在连续核****的****情****况****下**，这样做需要堆叠 O(n/k)个卷积层，或者在扩张卷积的情况下需要堆叠  ，增加网络中任何两个位置之间最长路径的长度。卷积层通常比递归层更昂贵，是k的倍数。然而，可分离卷积[6]大大降低了复杂性，达到 。然而，即使在k=n的情况下，可分离卷积的复杂度也等于自注意力层和点状前馈层的组合，也就是我们模型中采用的方法。

**As side b**enefit, self-attention could yield more interpretable models. We inspect attention distributions from our models and present and discuss examples in the appendix. Not only do individual attention heads clearly learn to perform different tasks, many appear to exhibit behavior related to the syntactic and semantic structure of the sentences.

**作为**副作用，自注意力可以产生更多可解释的模型。我们从我们的模型中检查了注意力的分布，并在附录中提出和讨论了一些例子。不仅个别注意力头明显学会了执行不同的任务，许多人似乎表现出与句子的句法和语义结构有关的行为。

## 5 训练

**T**his section describes the training regime for our models.

**本节描述了我们模型的训练制度。**

### 5.1 训练数据和批处理

We trained on the standard WMT 2014 English-German dataset consisting of about 4.5 million sentence pairs. Sentences were encoded using byte-pair encoding [3], which has a shared sourcetarget vocabulary of about 37000 tokens. For English-French, we used the significantly larger WMT 2014 English-French dataset consisting of 36M sentences and split tokens into a 32000 word-piece vocabulary [31]. Sentence pairs were batched together by approximate sequence length. Each training batch contained a set of sentence pairs containing approximately 25000 source tokens and 25000 target tokens.

**我们在由大**约450万个句子对组成的标准WMT 2014英德数据集上进行训练。句子使用字节对编码[3]，它有一个共享的源-目标词汇，大约有37000个标点。对于英语-法语，我们使用了明显更大的WMT 2014英语-法语数据集，包括3600万个句子，并将代币分成32000个词件词汇[31]。句子对按照大致的序列长度被分到一起。每个训练批次包含一组句子对，其中包含大约25000个源标记和25000个目标标记。

### 5.2 硬件和时间表

**We trained our models on one machine with 8 NVIDIA P100 GPUs. For our base models using the hyperparameters described throughout the paper, each training step took about 0.4 seco**nds. We trained the base models for a total of 100,000 steps or 12 hours. For our big models,(described on the bottom line of table 3), step time was 1.0 seconds. The big models were trained for 300,000 steps (3.5 days).

**我们在一台有8个NVIDIA P100 GPU的机器上训练我们的模型。对于我们的基础模型，使用本文所述的超参数，每个训练步骤大约需要0.**4秒。我们总共训练了100,000步或12小时的基础模型。对于我们的大模型，（在表3的最下面一行描述），步骤时间为1.0秒。大模型被训练了30万步（3.5天）。

### 5.3 优化器

**We used the Adam ****o****p****t****imize****r**** [17] with  ，  and  . We varied the learning** rate over the course of training, according to the formula:

**我们使用亚当优化器[17]， **** ****，**** ****且**** **。在训练过程中，我们改变了学习率，根据公式：

**This ****c****o**rresponds to increasing the learning rate linearly for the first warmup_steps training steps, and decreasing it thereafter proportionally to the inverse square root of the step number. We used   .

**这****相****当**于在第一个warmup_steps训练步骤中线性增加学习率，此后按步骤数的反平方根比例减少。我们使用 。

### 5.4 正则化

**W****e employ three types of regularization during training:

Residual Dropout We apply dropout [27] to the output of each sub-layer, before i****t**** **is added to the sub-layer input and normalized. In addition, we apply dropout to the sums of the embeddings and the positional encodings in both the encoder and decoder stacks. For the base model, we use a rate of  .

**L****a****be**l Smoothing During training, we employed label smoothing of value   [30]. This hurts perplexity, as the model learns to be more unsure, but improves accuracy and BLEU score.

**我们****在****训练过程**中采用了三种类型的正则化。

Residual Dropout 我们对每个子层的输出应用了dropout[27]，然后再将其添加到子层的输入并进行规范化处理。此外，我们对编码器和解码器堆栈中的嵌入和位置编码的总和进行剔除。对于基础模型，我们使用 的比率。

**标****签平滑在训练中，我们采用了标签平滑值 [30]**。这损害了迷惑性，因为模型学会了更多的不确定性，但提高了准确率和BLEU得分。

## 7 结论

**In this work, we presented the Transformer, the first sequence transduction model based entirely on attenti**on, replacing the recurrent layers most commonly used in encoder-decoder architectures with multi-headed self-attention.

**在这项工作中，我们提出了Transformer，这是第一个完全基于**注意力的序列转换模型，用多头的自注意力取代了编码器-解码器架构中最常用的递归层。

**For trans**lation tasks, the Transformer can be trained significantly faster than architectures based on recurrent or convolutional layers. On both WMT 2014 English-to-German and WMT 2014 English-to-French translation tasks, we achieve a new state of the art. In the former task our best model outperforms even all previously reported ensembles.

**对于翻译任务，Transformer的训练速度明显快于基于递归或卷积层的架构。在WMT 2014英译德和**WMT 2014英译法的翻译任务上，我们实现了新的技术状态。在前一项任务中，我们的最佳模型甚至超过了之前报道的所有组合。

**We are excited about the future of attention-based models and plan to apply them to other tasks. We plan to extend the Transformer to problems involving input and output modalities o**ther than text and to investigate local, restricted attention mechanisms to efficiently handle large inputs and outputs such as images, audio and video. Making generation less sequential is another research goal of ours.

**我们对**基于注意力的模型的未来感到兴奋，并计划将其应用于其他任务。我们计划将Transformer扩展到涉及文本以外的输入和输出模式的问题，并研究局部的、有限的注意力机制，以有效地处理大型输入和输出，如图像、音频和视频。使生成的顺序性降低是我们的另一个研究目标。