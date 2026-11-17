
# LLM（廿三）：LLM 中的长文本问题

> 来源：[https://agijuejin.feishu.cn/docx/NpdNdglgoooSJSxNYELcZa4wnyh](https://agijuejin.feishu.cn/docx/NpdNdglgoooSJSxNYELcZa4wnyh)

> 作者：紫气东来

> 原文：[https://zhuanlan.z](https://zhuanlan.zhihu.com/p/640641794)hihu.com/p/640641...

近期，随着大模型技术的发展，长文本问题逐渐成为热门且关键的问题，不妨简单梳理一下近期出现的典型的长文本模型：

+ 10 月上旬， Moonshot AI 的 Kimi Chat 问世 ，这是首个支持 20 万汉字输入的智能助手产品；

+ 10 月下旬，百川智能发布 Baichuan2-192K 长窗口大模型，相当于一次处理约35 万个汉字；

+ 11 月上旬，OpenAI 发布支持 128K 上下文窗口的 GPT-4 Turbo 模型；

+ 11 月下旬，Anthropic 发布支持 200K 上下文窗口的 Claude 2.1 模型;

+ [12 月上旬，零一万物](https://link.zhihu.com/?target=https%3A//huggingface.co/01-ai/Yi-6B-200K)开源[了长文本模型 Yi-6B](https://link.zhihu.com/?target=https%3A//huggingface.co/01-ai/Yi-34B-200K)-200K 和 Yi-34B-200K 。

实际上，随着文本长度的提高，模型能够处理问题的边界也大大提高，因此研究并解决长文本问题就显得非常必要。本文将从长文本问题的本质出发，逐步分析和研究长文本实现的问题及解决办法。

## 一、长文本的核心问题与解决方向

### 1.1 文本长度与显存及计算量之关系

要研究清楚长文本的问题，首先应该搞清楚文本长度在模型中的地位与影响。那么我们便以 Decoder-base 的模型为例来进行分析。

### 1.1.1 模型参数量

Decoder-base 的模型主要包括 3 个部分：embedding, decoder-layer, head。

其中最主要部分是decoder-layer，其由 l 个层组成，每个层又分为两部分：self-attention 和 MLP。

self-attention 的模型参数有 的权重矩阵 及bias，输出矩阵 及bias，4个权重矩阵的形状为 （  表示 `hidden_size`），4个bias的形状为 。则 self-attention 的参数量为  。

MLP 由2个线性层组成，一般地，第一个线性层是先将维度从  映射到 ，第二个线性层再将维度从 。   映射到  。第一个线性层的权重矩阵  的形状为 ，偏置的形状为 。第二个线性层权重矩阵  的形状为  ，偏置形状为 。则 MLP 的参数量为   。

self-attention 和 MLP 各有一个layer normalization，包含了2个可训练模型参数：缩放参数  和平移参数  ，形状都是 。2个layer normalization的参数量为 。

由此，每个 Decoder 层的参数量为 。

此外， embedding 和 head 的参数量相同，与词表相关，为  Vh （如果是 Tied embedding，则二者共用同一个参数）。由于位置编码多样，且参数量小，故忽略此部分。

综上，  l 层模型的可训练模型参数量为  。当  h 较大时，可以忽略一次项，模型参数量近似为 。

### 1.1.2 计算量估计

如果说参数量是模型的固有属性，那么计算量便是由模型和输入共同决定，下面分析这一过程。

假设输入文本经过分词器后的 `input_ids` 数据的形状为  （  表示 `batch_size`,  表示 `sequence_length`），然后经过 embedding 层后 x 形状变为   ，由于仅仅是查表操作，计算量为 。

先分析 Decoder 中 self-attention 的计算量，计算公式如下：

1. 计算 ：矩阵乘法的输入和输出形状为  。计算量为  。

2. 矩阵乘法的输入和输出形状为 。计算量为  。

3. 计算在  上的加权  ，矩阵乘法的输入和输出形状为 。计算量为 。

4. attention后的线性映射，矩阵乘法的输入和输出形状为 。计算量为 。

接下来分析MLP块的计算，计算公式如下 ：

1. 第一个线性层，矩阵乘法的输入和输出形状为 。计算量为 。

2. 第二个线性层，矩阵乘法的输入和输出形状为 。计算量为 。

将上述计算量相加，得到每个Decoder层的计算量大约为 。

此外，另一个计算量的大头是`logits`的计算，将隐藏向量映射为词表大小。矩阵乘法的输入和输出形状为 ，计算量为 。

因此，对于一个  l 层的模型，输入数据形状为  的情况下，一次前向计算的计算量为 。

### 1.1.3 文本长度与计算量、参数量、显存的关系

忽略低次项，一次输入的tokens数为  ，则计算量与参数量的关系为 ，在实际中通常  (在长文本情况下，可能会出现  的情况，但影响不是决定性的)，因此该项可近似认为约等于 2。即在一次前向传递中，对于每个token，每个模型参数，需要进行2次浮点数运算(一次乘法法运算和一次加法运算)。考虑到后向传递的计算量是前向传递的2倍。因此一次训练迭代中，对于每个 token，每个模型参数，需要进行 `` `2*3=6` `` 次浮点数运算。

通过以上分析，我们可以得到结论：计算量主要和模型参数和 token 数相关，文本长度并不会显著增加计算量 。那么这就引出另一个问题：文本长度与显存的关系。

除了模型参数、梯度、优化器状态外，占用显存的大头就是前向传递过程中计算得到的中间激活值。这里的 激活（`activations`） 指的是：前向传递过程中计算得到的，并在后向传递过程中需要用到的所有张量。

先分析 Decoder layer 中 self-attention 的中间激活：

1. 对于 ，需要保存它们共同的输入 ，这就是中间激活。输入  的形状为 ，元素个数为 ，占用显存大小为 。

2. 对于  矩阵乘法，需要保存中间激活 ，两个张量的形状都是 ，占用显存大小合计为 。

3. 对于  函数，需要保存函数的输入 ，占用显存大小为 ，这里的  表示注意力头数。

4. 计算完  函数后，会进行`dropout`操作。需要保存一个`mask`矩阵，`mask`矩阵的形状与  相同，占用显存大小为 。

5. 计算在  V 上的attention，即 ，需要保存 ，大小为 ；以及 ，大小为 。二者占用显存大小合计为 。

6. 计算输出映射以及一个`dropout`操作。输入映射需要保存其输入，大小为 ；`dropout`需要保存`mask`矩阵，大小为 。二者占用显存大小合计为 。

因此，将上述中间激活相加得到，self-attention 的中间激活占用显存大小为 。

接下来分析分析 Decoder layer 中 MLP 的中间激活：

1. 第一个线性层需要保存其输入，占用显存大小为 。

2. 激活函数需要保存其输入，占用显存大小为 。

3. 第二个线性层需要保存其输入，占用显存大小为 。

4. 最后有一个`dropout`操作，需要保存`mask`矩阵，占用显存大小为 。

对于MLP块，需要保存的中间激活值为 。

另外，self-attention块和MLP块分别对应了一个layer normalization。每个layer norm需要保存其输入，大小为 。2个layer norm需要保存的中间激活为 。

综上，每个层需要保存的中间激活占用显存大小为 。对于  层transformer模型，还有embedding层、最后的输出层。embedding层不需要中间激活。总的而言，当隐藏维度  比较大，层数  较深时，这部分的中间激活是很少的，可以忽略。因此，对于  层模型，中间激活占用的显存大小可以近似为  ，这个结果与文本长度关系密切。

下面以GPT3-175B为例，对比下文本长度对模型参数与中间激活的显存大小的影响。假设数据类型为 `FP16` 。

| 模型名 | 参数量 | 层数 | 隐藏维度 | 注意力头数 |

| --- | --- | --- | --- | --- |

| GPT3 | 175B | 96 | 12288 | 96 |

GPT3的模型参数量为175B，占用的显存大小为 。GPT3 模型需要占用350GB的显存。

假设 GPT3 输入的  。对比不同的文本长度下占用的中间激活：

当  时，中间激活占用显存为   ，大约是模型参数显存的0.79倍;

当  `s=4096` 时，中间激活占用显存为 ，大约是模型参数显存的2.68倍。

可以看到长度仅仅到 4K，显存占用就出现了剧烈增加，同时 GPU 的 onchip memory 就显得更加捉襟见肘(因此也就出现了 `FlashAttention` 这类算法)。因此如何解决长文本带来的巨量显存开销成为关键及核心问题。

### 1.2 长文本问题的解决思路

当前，为了实现更长文本的支持，解决思路主要可以分为两个阶段：

+ 阶段一：在预训练阶段尽可能支持更长的文本长度

为实现这一阶段目标，通常采用并行化 (parallelism) 方法将显存占用分摊到多个 device，或者改造 attention 结构，避免显存占用与文本长度成二次关系。

+ 阶段二：在 SFT 或推理阶段尽可能外推到更大长度

为实现这一阶段目标，通常也是需要在两个方面进行考虑：

+ 对位置编码进行外推

+ 优化 Attention 机制

本文接下来的部分将尽可能详细深入地进行这些问题的研究。为了便于理解和接受，下文将从易到难，先介绍第二阶段的技术，然后再介绍第一阶段（同时也是考虑到直接使用开源模型者，不需要第一阶段的情况）。

## 二、长文本与位置编码

在 Transformer 结构的模型中，Attention模块的值与顺序无关，因此需要加入位置编码以确定不同位置的 token。典型的位置编码方式有两类：

+ 绝对位置编码：即将位置信息融入到输入中

+ 相对位置编码：微调Attention结构，使其能够分辨不同位置的Token

随着文本长度的增加，位置编码也会发生相应的变化，因此处理好位置编码问题是解决长文本问题的重要环节。

### 2.1 绝对位置编码及其外推

一般来说，绝对位置编码会加到输入中：在输入的第  个输入向量  中加入位置向量  得到 ，其中  仅依赖于位置 。

如下图所示，以二维向量为例来形象说明，图左中黑色箭头为输入向量  ，蓝色箭头为位置向量  （不同方法的长度与角度不同），其相加的结果为绿色箭头。在 Attention 结构中， 即相当于同时对输入向量  和位置向量  ，进行线性变换，那么 attention 值则是  和  的点积，如下图右中  和  的注意力即黄色夹角区域。

![](img/b6e2b64e12ea20ac4974bb2dc7504b71.png)

由于绝对位置编码由两部分组成，且两部分相互独立，因此无法计算相对距离。下面介绍几种典型的绝对位置编码：

#### 2.1.1 训练式编码

这种方式最为简单直接，即把位置当做词表一样，训练一个  [`max_length`, `hidden_size`] 位置向量矩阵。这种训练式的绝对位置编码，一般的认为它没有外推性，但是苏剑林大神提出过一个层次分解的拓展方法。

![](img/fb0e0d9f2d301a4f59f95407f612b08c.png)

假设已经训练好的绝对位置编码向量为 ，希望能在此基础上构造一套新的编码向量 ，其中 。为此，设

其中超参 ，  是该套位置编码的“基底”。为了保障 ，这样就能反推出各个   :

这样就最大可以表示出  个位置的编码，并且前  个位置编码跟原来模型是相容的。下图反映了经过finetune其准确率在延长的位置编码在MLM任务上是行之有效的。

![](img/19a587a0d9f8de3373fa31bfb8b59432.png)

需要说明的是，这种矩阵式的位置编码方式在当前的大模型中已经比较少采用了，仅有 GPT2 等早期模型中采用了这种方式。

#### 2.1.2 Sinusoidal 位置编码

这种方案也是 Attention Is All You Need 中提出的方法 其中  分别是位置  的编码向量的第   个分量，  是位置向量的维度。

根据以上定义，我们可以非常简单计算得到`Sinusoidal`位置编码的值，并绘制图像研究其规律。整体位置编码如下图所示：

![](img/8394cdb62b9d477a7a1ed11ec3d486aa.png)

首先研究 `Sinusoidal` 位置编码与位置  之间的关系，绘制不同维度下，函数值与位置的关系。其曲线如下图所示， 可以从图中得到几点结论：

+ 位置越远，频率越大

+ 随着维度增大，函数逐渐收敛到 0 （cos 函数收敛到 1 ）

![](img/4b3cf4ddee0eac29730f6d34f8464167.png)

![](img/e0c3011dd58c6e6d6c3a41fb83bda203.png)

研究 `Sinusoidal` 位置编码与维度分量  之间的关系，绘制不同维度分量 i 下，函数值与位置的的关系。`Sinusoidal`位置编码与维度分量的关系如下图所示，可以发现结论如下：

+ 每个分量都具有周期性，是正弦或余弦函数

+ 越靠后的分量(i 越大)，波长越长，频率越低

![](img/f999e7531dc097794197143e000135f1.png)

了解了这些基本的特性后，接下来就需要讨论更加深层次的问题：

问题一：为什么用包含各频率的正弦和余弦对？

位置编码存储的是一个包含各频率的正弦和余弦对，这样做有两个好处：

+ 可以使得不同位置的编码向量之间有一定的规律性，比如相邻位置之间的差异较小，而距离较远的位置之间的差异较大。这是由正弦和余弦函数的连续性和单调性保证的，即对于任意两个相邻的位置，它们对应的编码向量在每一个维度上都只有微小的变化，而对于任意两个距离较远的位置，它们对应的编码向量在每一个维度上都有较大的差异。

+ 可以使得编码向量在任意维度上都能保持唯一性，即不同位置在同一个维度上不会有相同的值。这是由正弦和余弦函数的周期性和相位差保证的，即对于任意两个不同的位置，它们对应的编码向量在每一个维度上都不相等。

问题二：底数对结果的影响是什么？

底数越大，位置向量能表示的序列就越长，这是大底数的好处。但是，底数大，意味着在-1到+1的范围内向量的取值越密集，造成两个位置的向量距离越近，这对后续的Self-Attention模块来说是不利的，因为它需要经历更多的训练次数才能准确地找到每个位置的信息，或者说，才能准确地区分不同的位置。长序列需要长编码。但这样又会增加计算量，特别是长编码会影响模型的训练时间。所以，那个底数并非是越大越好。

问题三：`Sinusoidal` 位置编码如何外推

三角函数式位置编码的特点是有显式的生成规律，因此可以期望于它有一定的外推性。另外一个使用它的理由是：由于

这表明位置  的向量可以表示成位置  和位置  的向量组合，这提供了位置拓展的可能性。

#### 2.1.3 其他的绝对位置编码

如递归式（如 FLOATER）和相乘式（如 PENG Bo：中文语言模型研究：(1) 乘性位置编码 ），因使用较少，在此不予赘述。

### 2.2 相对位置编码及其外推

相对位置并没有完整建模每个输入的位置信息，而是在算Attention的时候考虑当前位置与被Attention的位置的相对距离，由于自然语言一般更依赖于相对位置，所以相对位置编码通常也有着更好的表现，灵活性也更大。

#### 2.2.1 旋转位置编码 `RoPE`

实际上 `RoPE` 的诸多思想来源于 `Sinusoidal` 位置编码，区别在于 `Sinusoidal` 位置编码采用和 word embedding 相加的形式，`RoPE` 则采用了矩阵相乘的形式。

在正式介绍之前，我们需要回顾一下经典的欧拉公式

其矩阵形式为 即旋转矩阵 ，这三种表现形式表达了同样的信息，即将二维向量逆时针旋转角度  。

![](img/1d1b4eb7ead7429ec5522b16e9235075.png)

接下来我们直接看 `RoPE` 的表达式，对于位置为  的  向量，其表达式为

即逆时针旋转了  度，如上图右所示。同理，位置为  的  向量的表达式为

那么便可以通过点积，计算二者的 attention 值

即证明了相对位置关系，即旋转前的 attention 值与旋转后的 attention 值的差值仅与相对位置有关。这一点也可以从上图右中看出来，即旋转前  的夹角(橙色区域) 与旋转后  的夹角(黄色区域) 相同，即内积也相同。

这时我们就可以写出位置为  的  的完整的变换矩阵，即

从改变换矩阵也能看出，随着维度增加，旋转角度也在指数级减小，如下图所示。`RoPE` 的这一功能使模型可以通过从低维度到更高维度，将嵌入中编码的信息类型从低频（close）转变为高频（far）。

![](img/09deb28d336c336d7f57107cddc0ccba.png)

#### 2.2.2 远程衰减问题

由于 `RoPE` 中的 attention 值除了  本身外，仅和  因子相关，那么下面考察    因子的特点

那么问题就变成了积分  的渐进估计问题，通过一下函数计算积分值与位置距离的关系，并分析不同 `base` 值的影响。

下图展示了不同距离尺度上不同 `base` 值的积分结果，可以得到以下结论：

+ 除了 `base=1` 外，均有明显的远程衰减特性

+ `base` 越小，衰减得越快且幅度也更大

+ `base` 越大，衰减得越慢且幅度也越小

![](img/b05b2e8e3774bd0b6dc2a2c256a37512.png)

#### 2.2.3 RoPE 长度的内插与外推

长度外推性是一个训练和预测的长度不一致的问题。提现有两点：

+ 预测的时候用到了没训练过的位置编码（不管绝对还是相对）；

+ 预测的时候注意力机制所处理的token数量远超训练时的数量。



一旦我们在模型中有效地整合了相对位置信息，增加 LLM 上下文窗口的最直接方法就是通过位置插值 (position interpolation，PI) 进行微调。

这种方法实现很简单，如果希望将预训练阶段的位置向量范围`[0,2048]` 外推到`[0,4096]`，只需要将对应位置缩放到原先支持的区间(`[0,2048]`)内：计算公式如下，`L`为原先支持的长度(如2048)， 为需要扩展的长度(如4096)：

其过程如下图所示：

![](img/2235c6c2e038ccbb52b197c0eba9c34c.png)

下面分析一下以上操作的本质，经过这种放缩操作后，位置为 `m` 的 维度为 `i` 的旋转角变为 \frac{mL}{L^{'}}* base^{-2 \mathrm{i} / \mathrm{d}} ，即线性减小了旋转弧度，如下图第一列的上图所示（横轴为位置编码，纵轴为旋转弧度）。通过这种方式插值后，向量旋转速度变慢，周期变大，频率变慢。

除了上述的这种差值方式外，还有以下改进方式可以实现外推：

+   `NTK-aware` (Neural Tangent Kernel)

这种方式把旋转角修改为 ，其中 `𝛼` 表示 `base` 的缩放因子，在 `codellama` 中取值为 100 。其修改的方式如下图第二列下图所示（横轴为维度，纵轴为旋转角），在不同维度上修改的程度不同。这种方式保留了高频信息，即

+   高频分量旋转速度降幅低，低频分量旋转速度降幅高；

+   在高频部分进行外推，低频部分进行内插。

这是因为靠前的维度，在训练中见过非常多完整的旋转周期，位置信息得到了充分的训练，所以具有较强的外推能力。靠后的维度，在训练中无法见到完整的旋转周期，或者见到的旋转周期非常少，训练不够充分，外推性能弱，需要进行位置插值。

+   `NTK-by-parts`

该方法是基于 `NTK-Aware` 的优化，其核心思想是： 不改变高频部分，仅缩小低频部分的旋转弧度 。即不改变小维度的旋转弧度，仅减小大维度的旋转弧度，这就是`by-patrs`的含义。

该方法是基于 `NTK-Aware` 的优化，其核心思想是：不改变高频部分，仅缩小低频部分的旋转弧度。即不改变小维度的旋转弧度，仅减小大维度的旋转弧度，这就是`by-patrs`的含义。

第 `i` 个维度的旋转周期为：

其在训练长度内旋转的周期个数如下：

引入超参数 `α`,`β` ，表示旋转周期个数的约束条件，定义斜坡函数

当  ，旋转周期数量少，则为低频分组，仅进行线性插值。

当 , 选择周期介于二者之间，类似于“`NTK-aware`”插值

当 ，旋转周期数量足够多，则认为该维度为高频部分，无需改变。

综合以上条件，调整后的旋转角可以表示为

其中 , 超参数的经验取值为 `α`=1,`β`=32

+   `Dynamic NTK`

这是是一种动态插值的方法：当推理长度小于等于训练长度时，不进行插值；推理长度大于训练长度时，每一步都通过`NTK-Aware Interpolation`动态放大`base`。

表示当前的序列长度， `L` 表示模型训练长度，

当 `l`≤`L` 时，不调整旋转角

当 `l`>`L` 时，旋转角调整为 ，其中

需要说明的是下图最后一列下图的粗线表示一个范围，未体现出与长度的动态联动。

![](img/3b42b8fa7b448fc12575f290f9b2b4f0.png)

+   [`YaRN`](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2309.00071.pdf)

无论是`PI`还是`NTK`类方法，本质都是通过减小旋转弧度，降低旋转速度，来达到长度扩展的目的。 这将导致位置之间的旋转弧度差距变小，词向量之间的距离变得比原来更近，词向量之间的点乘变大，破坏模型原始的注意力分布 。所以经过插值之后，模型在原来的训练长度内的困惑度均有所提升，性能受损。

`YaRN`本质上是`NTK-by-parts` Interpolation与注意力分布修正策略的结合，仅缩小低频部分的旋转弧度，且通过温度系数修正注意力分布。只需将原来的注意力分数除以温度 `t` 即可。

以`LLaMA`为例，当长度从2048扩展至16384时，长度扩展为原来的8倍，代入公式，计算得到`t`=0.6853。回顾温度系数对注意力分布的影响，当`t`变大，注意力分布更加平滑，方差更小；当`t`变小，注意力分布更加尖锐，区分度变大，方差变大。`t`=0.6853意味着缓解注意力分布过于平滑的问题，让注意力分布方差更大。

需要说明的是，论文 [Scaling Laws of RoPE-](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2310.05209)based Extrapolation 中深入研究了 `RoPE` 位置编码的特性，其结论就是：`RoPE` 中 `base` 的放大和缩小都能获得很好的外推效果(`base`=10K 效果最差)。原因在于：

+   当 `base` 较小时(如 500)，`RoPE` 的三角函数周期变短，训练时就可以见过完整的 `cos`/`sin` 值域；

+   当 `base` 较大时(如 1000000)，`RoPE` 的三角函数周期变长，训练时虽然不能见过完整的 `cos`/`sin` 值域，但是外推时仍处于单调区间。

### 2.2.4 其他形式的编码方式及其外推

在苏神的文章 `Transformer`升级之路：12、无限外推的`ReRoPE`？ 中指出： `RoPE` 形式上是一种绝对位置编码，但实际上给 `Attention` 带来的是相对位置信息，即如下的 `Toeplitz`矩阵 ：

[这种形式的 ](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2108.12409.pdf)`bias` 似乎有种似曾相识的感觉，没错，就是 `ALiBi` 编码。严格来说，`ALiBi` 并不算位置编码，因为它并没有作用在 `embedding` 上，而是直接作用在了 `Attention` 上，通过这种构造方式既实现了远程衰减，又实现了位置的相对关系。

![](img/eecf448f747ac2a8b9e697199ff45607.png)

对于外推特性，`ALiBi` 与前文所述的方法也是不同的，体现在：

+   `事后修改` ，比如 [`NTK-RoPE`](https://link.zhihu.com/?target=https%3A//kexue.fm/archives/9675)、 [`YaRN`](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2309.00071)、 [`ReRoPE`](https://link.zhihu.com/?target=https%3A//kexue.fm/archives/9708)等，这类方法的特点是直接修改推理模型，无需微调就能达到一定的长度外推效果，但缺点是它们都无法保持模型在训练长度内的恒等性

+   `事前修改` ，如 [`ALIBI`](https://link.zhihu.com/?target=https%3A//kexue.fm/archives/9431%23ALIBI)、 [`KERPLE`](https://link.zhihu.com/?target=https%3A//kexue.fm/archives/9431%23KERPLE)、 [`XPOS`](https://link.zhihu.com/?target=https%3A//kexue.fm/archives/9431%23XPOS)以及 [`HWFA`](https://link.zhihu.com/?target=https%3A//kexue.fm/archives/9603)等，它们可以不加改动地实现一定的长度外推，但相应的改动需要在训练之前就引入，因此无法不微调地用于现成模型

## 三、长文本与 Attention 机制

`Attention` 机制也是制约长文本实现的重要因素，以下是几种典型的 `Attention` 的 方式：

![](img/b2b9e1d6b25099f0fc49bd12935035e1.png)

关于 `Attention` 机制改进的更多类型和细节，笔者在之前的文章中已经有所讨论，可参考：https://zhuanlan.zhihu.com/p/659770503

[  ](https://zhuanlan.zhihu.com/p/659770503)

在此主要想介绍一个方案 —— [`LongLora`](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2309.12307.pdf)。

回顾第一节中研究的结论，长文本影响最大的就是 `self-attention` 中的，随长度二次变化的显存占用和计算复杂度。为解决这个问题，`LongLora` 的原则是，虽然在推理过程中需要密集的全局注意力，但通过稀疏的局部注意力可以有效且高效地微调模型。

`LongLora` 在微调期间延长上下文长度，同时使用 `Lora` 方法保持高性能和低复杂性。其中最关键的是提出了 `转移短注意力（S2-Attn）` 方案。下面简要介绍这一方案：

`S2-Attn` 在微调阶段使用局部注意力而不是全局注意力。即将输入文档分解为几个不同的组，并在每个组中分别应用注意力机制（`Pattern 1`）。尽管这种方式能够在资源占用不多的情况下拓展长度，由于不同组之间缺乏信息交换，随着上下文长度的增加，会导致混乱增加。

为了解决上述问题，`S2-Attn` 引入了组大小一半的移位操作，确保相邻组之间顺利的信息交换（`Pattern 2`）。这种做法有助于模型在文本开头和结尾之间顺利交换信息，从而提高模型稳定性。

![](img/a4ae47d0beead7c4b8f89834d5c52deb.png)

而本文提出的 shift short attention 有一半的 head 会被做 shift，如下图所示，然后每个 group 内作 self-attention，从而使信息可以在不同 group 间传递。这种做法实际上将 `Pattern 1` 和 `Pattern 2` 结合起来，而没有引入额外的计算开销，使其非常适合高效处理长序列文本。

![](img/1db751149913a9d8563b772055d21967.png)

此外，`LongLoRA`相比于`Lora`还可以微调`embedding`层和`normalization`层。尽管这两项内容占的参数量很小（以`Llama 2-7B`为例，`embedding`层只占1.94%，`normalization`层更是不到十万分之四），对结果也起到了重要作用。

## 四、长文本的预训练方法

上两节主要介绍了如何在位置编码和 attention 机制方面进行文本长度的有效拓展，这两个方面都是“经济适用性”的，即只需要简单微调或者直接外推即可，接下来将是最困难，也是成本最高的部分，即讨论如何在预训练阶段提高文本长度。

为解决预训练过程中的长文本问题，思路主要有以下几个方面：

+   并行化计算，典型方法如 `sequence parallelism`

+   [`优化 attention 机制`](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1901.02860)，典型方法如 [`Transformer-XL`](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2004.05150.pdf) ，`Longformer`

+   引入 `memory` 机制，典型方法如 `Focused Transformer` ，`Memorizing Transformer`

+   引入采样机制，典型方法如 `Hierarchical transformers` ，`Dynamic-Pooling Transformer`

由于笔者精力有限，下面仅选取其中部分方法加以介绍。

### 4.1 序列并行(sequence parallel)

在并行化算法大行其道的今天，使用改思想来解决长文本问题变得自然而言，实际上 `SP` 已逐渐成为 `3D`(`DP`, `PP`, `TP`)并行之外的第 4 个维度了。关于`3D` 并行，已经有很多文章介绍了，其中与`SP`比较相关的是`TP`，`TP`的实现细节可以参考笔者之前的文章https://zhuanlan.zhihu.com/p/603908668

[  ](https://zhuanlan.zhihu.com/p/603908668)

简单来说，`SP` 就是将一段完整的文本拆分到多个设备上进行计算，设备在适当的时候进行通信和信息交互，如下图(`c`) 所示。

![](img/19394a20d399e0387bebbd9a17551c95.png)

在实现层面，借鉴了 `Ring-Allreduce` 的思想，将输入序列分割成多个块，并将每个块输入到其相应的设备中。为了计算注意力输出，将环状通信与自注意力计算相结合，实现了环自注意力（`RSA`），如下图所示。

![](img/e3cdf6bc2bdf1a1235a555482e9e71a8.png)

下面我们来深度理解一下这个过程，论文中的符号表示为

+   `B`: batch size

+   `L`: sequence length

+   `H`: hidden size of linear layers

+   `A`: attention head size

+   `Z`: number of attention heads

+   `N`: number of GPUs

对于 **，**切分后的每一个小块 ，在和 矩阵乘后得到 需要在切分维度做补全才可以得到每一个小块  的完整结果，在后续进行操作 。故需要做 `ring` 操作达到一个 `concat` 的操作。

对于 ，

需要完整对 `softmax` 结果进行 `value` 查询，需要对 根据序列并行度进行分块，用对应的块在对应的 `value` 上查询并求和，故也需要做 `ring` 操作。

![](img/8c0c6ac8b564165ec9e6a6ed125f4aef.png)

`MLP` 部分的计算就更简单了，如下所示：

![](img/89f8ada191282f28565346497a865e83.png)

### 4.2 LongLLaMA (Focused Transformer)

`LongLLaMA` 通过引入 `Focused Transformer`（`FOT`）方法，在保持性能的同时，将 `LLaMA` 的上下文长度扩展到100k！在长文本的情况下，除了第一节所研究的显存和计算量的问题外，这篇论文还提出了一个分心问题（`Distraction Issue`），即随着文本长度的增加，其中相关的 `tokens` 对不相关 `tokens` 的比例会减少，从而导致与不相关 `value` 相关的 `key` 和与相关 `value` 相关的 `key` 发生重叠，致使模型需要额外区分不同语义的 `key` 。

为此文章提出了 `Focused Transformer` （`FOT`）解决方案，其中主要使用了 `Memory Attention Layers` 以及 `CrossBatch` 技术，在 `Inference` 的过程中，绿色的 `Memory Attention Layers` 使用 `kNN` 对外部的 `Memory` 进行查询，从而有效延长了上下文长度，而 `Memory Attention Layers` 则主要使用 `CrossBatch` 进行训练。

![](img/c29c9115f16509fac69d6ba12d4251bf.png)

具体而言，`Memory Attention Layers` 中的每个 `query` 在  `∈` 中会关注局部的上下文以及 `Memory` 中使用 `kNN` 计算出的最匹配的 `k` 个`key`，而整个 `Memory` 则根据 之前处理的 `key`，`value` 进行填充。而 `CrossBatch` 则期望使得 `Memory Attention Layers` 更加关注长文本之中的“相关 `value` 的 `key`” ，`CrossBatch` 的处理借鉴了对比学习的思想，以相关文档之中的 `d-1` 个上下文作为正样本，以不相关文档之中的 `d-1` 个上下文作为负样本，通过对比学习的方式使得 `Memory Attention Layers` 可以更好的分辨相关与无关的 `key-value`。

与标准的 `Transformer` 相比，一般的 `Transformer` 的训练过程中，相关与不相关文档没有被得到有效区分（正负样本分散均匀），当文档数量扩展时，注意力变得越来越分散，而 `Focused Transformer` 则通过 `CrossBatch` 的训练目标使得模型有效的关注与区分的长文本下的相关与无关的 `key-value` 的空间结构，从而解决了分心的问题。

## 五、长文本的效果评估

### 5.1 PPL

对于 `LLM` 来说，其效果通常是通过生成连贯且上下文相关的文本的能力来衡量的。为了量化和衡量这一指标，困惑度 (`Perplexity`, `PPL`) 便成了最常见的指标。

`PPL` 是一种衡量标准，反映模型根据前面的上下文预测下一个单词的能力。`PPL` 分数越低，模型准确预测下一个单词的能力就越好。

`PPL` 是使用平均交叉熵计算的，而平均交叉熵又是使用数据集中的单词数量和根据前面的上下文预测的单词（目标单词）的概率来计算的。前面的上下文通常由目标单词之前的固定长度单词序列表示。其公式如下：

其中`H`是平均交叉熵，

`PPL` 作为一种客观的评估指标被广泛用来进行 `LLM` 的评估。但是其也存在一些问题和不足：

+   模型词汇量可能会不公平地影响`PPL`：`PPL` 在很大程度上依赖于模型的词汇量及其概括未见过的单词的能力。如果模型遇到训练数据中不存在的单词或短语，即使生成的文本有意义，其 `PPL` 分数也较高。

+   缺乏主观性考虑：`PPL` 是一种客观指标，不考虑主观因素，例如风格、创造力或特定环境下的适当性。

+   上下文理解：`PPL` 主要关注于根据前面的上下文预测下一个单词。然而，它可能无法捕捉模型对更广泛背景的整体理解。

+   语言歧义和创造力：`PPL` 并不能体现模型处理语言歧义或生成创造性和新颖输出的能力。

+   领域特异性：`PPL` 对训练数据的领域和分布很敏感。在特定领域训练的模型可能会在其领域内实现较低的复杂性，但可能需要帮助在其训练环境之外生成文本。



+ 过度拟合和泛化：PPL可能会受到过度拟合的影响，其中模型在训练数据上表现得非常好，但很难泛化到看不见的或现实世界的例子。

实际上，StreamingLLM 就很好地证明了 PPL 的局限性，因为尽管 StreamingLLM 的 PPL 值较低，但是由于其损失了大量中间信息，因此无法在“大海捞针”等测试方法中有较好的表现。

## 5.2 “大海捞针”

“大海捞针”由 Greg Kamradt 提出的大模型长文本性能测试方法，其做法是在文本语料中藏入一个与文本语料不相关的句子，然后看大模型能不能通过自然语言提问的方式（Prompt）把这句话准确地提取出来。Greg Kamradt 的“大海捞针”实验简述：

> “大海”：Paul Graham 的文章合集作为语料
>
> “针”：“The best thing to do in San Francisco is eat a sandwich and sit in Dolores Park on a sunny day.”
>
> 提问："What is the most fun thing to do in San Francisco based on my context? Don't give information outside the document"
>
> 期待模型输出的正确答案：
>
> The best thing to do in San Francisco is eat a sandwich and sit in Dolores Park on a sunny day.

Greg Kamradt 公布了他对 GPT-4 Turbo（128K）和 Claude 2.1 的测试结果：

+ GPT-4 Turbo（128K）在语料长度超过 72K 且句子（“针”）藏在文本头部的时候，准确率不佳。

![](img/6b48239ef88641eea4f0b0ea41d07974.png)

+ Claude 2.1 似乎在语料长度超过 20K 之后就开始准确率不佳，而且句子（“针”）藏在语料靠前的位置时，准确率尤其差。

![](img/8a1ff76a016b33e5ff91b0ec98628410.png)

+ 进一步的，Anthropic 发现可以通过简单的 prompt 提示就可以提高模型不愿意回答不相关内容的效果，即让模型回答问题之前，加上一句“ Here is the most relevant sentence in the context: ”即可大幅提升模型回答效果，改进模型不愿意回答不相关内容的水平。

![](img/bce6dc1b4686297d3c55f5fc021ab739.png)

+ 此外国内的 Moonshot AI 的长文本模型 Kimi Chat 也在“大海捞针”实验中发挥了令人惊艳的表现，原始报道见 [这里](https://link.zhihu.com/?target=https%3A//mp.weixin.qq.com/s/IC5-FGLVHzHHYqH6x-aNng)。

![](img/37cab2ce9bbdce0588b2a7b995b4715d.png)

+ Qwen-72B-Chat 也进行了“大海捞针”实验，测试模型在不同长度的输入下，是否能检索到文章不同位置的信息，结果同样表现良好，如下：

![](img/cd871b1a546af26ca615864edb3db301.png)

## 参考资料

[1] [Kimi Chat 公布“大海捞针”长文本压测结果，也搞清楚了这项测试的精髓](https://link.zhihu.com/?target=https%3A//mp.weixin.qq.com/s/IC5-FGLVHzHHYqH6x-aNng) (qq.com)

[2] [分析transformer模型的参数量、计算量、中间激活、KV cache - 知乎](https://zhuanlan.zhihu.com/p/624740065) (zhihu.com)

[3] [ht tps:// medium.com/@ddxzzx/why- and-how-to-achieve-longer-context-windows-for-llms-5f76f8656ea9](https://link.zhihu.com/?target=https%3A//medium.com/%40ddxzzx/why-and-how-to-achieve-longer-context-windows-for-llms-5f76f8656ea9)

[4] 图解RoPE旋转位置编码及其特性

[5] [RoPE外推的缩放法则 —— 尝试外推RoPE至1M上下文 - 知乎](https://zhuanlan.zhihu.com/p/660073229) (zhihu.com)

[6] [Transformer升级之路：1、Sinusoidal位置编码追根溯源 - 科学空间|Scientific Spaces](https://link.zhihu.com/?target=https%3A//spaces.ac.cn/archives/8231)

[7] [让研究人员绞尽脑汁的Transformer位置编码 - 科学空间|Scientific Spaces](https://link.zhihu.com/?target=https%3A//kexue.fm/archives/8130)

[8] 层次分解位置编码，让BERT可以处理超长文本

[9] [理解Transformer的位置编码_51CTO博客_transformer的位置编码](https://link.zhihu.com/?target=https%3A//blog.51cto.com/u_15588078/6531187)

[10] [NTK-Aware Scaled RoPE allows LLaMA models to have extended (8k+) context size without any fine-tuning and minimal perplexity degradation. : r/LocalLLaMA (reddit.com)](https://link.zhihu.com/?target=https%3A//www.reddit.com/r/LocalLLaMA/comments/14lz7j5/ntkaware_scaled_rope_allows_llama_models_to_have/%3Frdt%3D44479)

[11] [EXTENDING CONTEXT WINDOW OF LARGE LANGUAGE MODELS VIA POSITION INTERPOLATION](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2306.15595.pdf)

[12] 详解基于调整RoPE旋转角度的大模型长度外推方法 (qq.com)

[13] [Transformer升级之路：15、Key归一化助力长度外推 - 科学空间|Scientific Spaces](https://link.zhihu.com/?target=https%3A//kexue.fm/archives/9859)

[14] 2309.12307.pdf (arxiv.org)

[15] [GitHub - dvlab-research/LongLoRA: Code and documents of LongLoRA and LongAlpaca](https://link.zhihu.com/?target=https%3A//github.com/dvlab-research/LongLoRA)

[16] 大模型分布式训练并行技术（五）-序列并行 (qq.com)

[17] [羊驼再度进化，“长颈鹿版”LongLLaMA 来啦，上下文长度冲向 100K ，性能不减](https://link.zhihu.com/?target=https%3A//mp.weixin.qq.com/s/OysnthTQXPG_AqogQtnIcw)

[18] [https://twitter.com/GregKamradt/status/1727018183608193393](https://link.zhihu.com/?target=https%3A//twitter.com/GregKamradt/status/1727018183608193393)

[19] [LLaMA长度外推高性价比trick：线性插值法及相关改进源码阅读及相关记录](https://link.zhihu.com/?target=https%3A//mp.weixin.qq.com/s/zyUDKc8VgeDvK4AQTCmfGg)

[20] [Length Extrapolation of Transformers: A Survey from the Perspective of Position Encoding](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2312.17044.pdf)

> 况属高风晚，山山黄叶飞。 —— 王勃《山中》
