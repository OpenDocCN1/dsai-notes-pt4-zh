# LLM（廿六）：从信息论的角度解释 scaling law

> 来源：[https://agijuejin.feishu.cn/docx/A0TTdQ6Nno77VvxJhlJckJawnFh](https://agijuejin.feishu.cn/docx/A0TTdQ6Nno77VvxJhlJckJawnFh)

> 作者：紫气东来

> 原文：[https://zhuanlan.z](https://zhuanlan.zhihu.com/p/687278237)hihu.com/p/687278...

1943年，当亚欧大陆东西两端都陷入焦灼的战火时，远隔大洋的美国，有两个年轻人因为这场战争走到了一起，他们就是：31 岁的图灵(Alan Turing，1912-1954) 和 27 岁的香农(Claude Shannon，1916-2001)。这也许是人类最精彩的会面之一，彼时他们分别拿着“矛”和“盾”：当时图灵在当是破译了包括希特勒通话在内的多项德军秘密通讯的密码学破译专家，而香农当时的工作是通过数学方法证明“Ｘ系统”——这是美国总统罗斯福到英国首相丘吉尔之间的加密通讯系统。

尽管因为保密的原因，二人不能在密码学上进行讨论，但却可以对当时最前沿的话题，包括机器思维、信息科学、计算理论、智能等进行讨论，事实上这些问题至今仍然是最前沿的研究领域。在这次会面不久，二人就在各自的领域取得了巨大的成就：

+   1948年，香农发表题为《通信的数学理论》的论文，正式开创了科学的信息论

+   1950年，图灵发表《计算机器和智能》，提出了“图灵测试” ，打开了人工智能的大门

时至今日，尽管模型、数据、算力都发生了翻天地覆的变化，人工智能和信息技术的发展仍然没有超越他们当年讨论的范畴。览前贤，思己任，希望在前辈们建立的大厦基础上，我辈能够薪尽火传，在自己的领域做出各自力所能及的贡献。

## 一、语言模型的信息论基础

考虑生成式的语言模型，其概率估计 通常为每个token的概率的乘积，即

![](img/3d5ada83a1778c75b8142f6570943efa.png)

有了概率之后，便可以定义更多的量了。

### 1.1 信息熵

假设随机变量 在属于集合 ，则 的熵定义为：

+   当对数底数是 时，单位是bit，

+   当对数底数是 时，单位是nat(奈特)。

![](img/2918594db12947cd1d9423dfed277832.png)

在自然语言领域，令 ，则熵还可以进一步表示为：

其中

当序列长度无限大的情况下，香农将其定义为该语言的熵，即：

根据这个定义，熵是使用无限数量的符号来计算的。在实践中，只能从有限的文本样本中近似经验熵来近似任何语言的熵。

接下来分析一下语言的信息熵的边界问题。任何分布的最小可能熵为零，对于语言来说，当一种语言只有一个符号或者某个符号的概率为 1 时，该语言的熵才能为零，即

概率分布的熵在均匀分布时最大化，对于共享同一组符号（词汇）的所有语言，当其所有符号以相等概率出现的时候其熵最大，以   V V 表示词表大小，则有：

+   如果词表大小为 27(26个字母+空格)，则

+   如果词表大小为 42000，则

这样我们就确定了语言的信息熵的上下界，接下来探究其单调性问题，假设来自于同一语言数据库（独立同分布）的长度不同的文本，则有：

其中最为关键的一步是 ，所表达的含义是随着前文长度的增加，预测的准确性会提高。因此则有：

这意味着随着数据量的增加，其理论上的经验熵值会逐渐减小，并逐步接近最真实的语言熵。

下表展示了几个主要数据集的经验熵，可以看到满足以上发现的规律，另外可以看到，大多数值仍然位于香农 1950 年估计的范围内，可见其超越时空的远见卓识。

![](img/14f306bcd6849c6c609b0cca419d9b8f.png)

### 1.2 交叉熵，相对熵

由于语言中缺乏无限数量的文本，该语言的真正分布是未知的。语言模型旨在从示例文本中学习接近语言经验分布的分布。为了测量两个分布的“接近度”，通常使用交叉熵，通常作为模型的 loss 。数学上，预测分布 Q 相对于真实分布 P 的交叉熵定义如下：

其中 即 𝑄 对 𝑃 的 KL[ 散度（亦](https://www.zhihu.com/search?q=KL%20%E6%95%A3%E5%BA%A6&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A687278237%7D)称相对熵）， 其表征了两个分布间的差异大小。下面来尝试解读一下上式的含义：

+   由于 ，则 ，则作为 loss 时，[交叉熵损失](https://www.zhihu.com/search?q=%E4%BA%A4%E5%8F%89%E7%86%B5%E6%8D%9F%E5%A4%B1&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A687278237%7D)可以可以看作两部分：一部分是训练预料本身的熵，另一部分是模型拟合的损失；

+   从信息论的角度来说， H(P) 表示对某个语[料库所代](https://www.zhihu.com/search?q=%E5%B9%B3%E5%9D%87%E4%BD%8D%E6%95%B0&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A687278237%7D)表的 𝑃 分布进行编码所需要的平均位数；

+   从信息论的角度来说， 表示使用 𝑄 的编码对 𝑃 分布进行编码所需要的额外位数

需要注意的是，由于经验熵 H(P) 是不可优化的，当我们以最小化交叉熵损失为目标来训练语言模型时，真正的目标是最小化分布的 KL 散度，这是语言模型从语言的经验分布学习到的。

### 1.3 PPL 与 BPC

Perplexity (PPL, 困惑度) 是衡量概率分布或概率模型对样本的预测程度。在数学上，语言模型的困惑度定义为：

直观地说，困惑度可以理解为不确定性的度量。语言模型的困惑度可以看作是预测下一个符号时的困惑程度。考虑一个熵为 3 比特的语言模型，其中每个比特编码两种概率相等的可能结果。这意味着在预测下一个符号时，该语言模型必须在可能的 选项中做出选择。因此，我们可以认为这个语言模型的困惑度为 8。

Bits-per-character (BPC， 字符位数) 是最近语言模型中经常报告的另一个指标。它精确地测量它所命名的数量：对字符进行编码所需的平均位数。这也来源于香农对语言熵的解释：

> if the language is translated into binary digits (0 or 1) in the most efficient way, the entropy is the average number of binary digits required per letter of the original language.

根据这个定义，熵是 BPC 的平均数。在实践中，如果每个人都使用不同的词表或模型，则很难比较不同模型的结果。为了保持一致性，使用 BPC 是一个比较好的选择。

实际上，BPC 建立了压缩的下限。如果文本的 BPC 为 1.2，则不能将其压缩到每个字符小于 1.2 位。例如，如果文本有 1000 个字符（如果每个字符使用 1 个字节表示，则大约 1000 个字节），则其压缩版本至少需要 1200 位或 150 个字节。

BPC 特定于字符级语言模型。当有单词级语言模型时，则通常采用 bits-per-word (BPW, 单词位数) ，即编码单词所需的平均比特数。关于二者的关系，Graves 提供了一个简单的转化公式：如果平均而言，一个单词需要 比特来编码，而一个单词包含 个字符，那么它应该需要平均 比特来编码一个字符。例如，一个单词在数据集中平均有 5.6 个字符，因此单词级别的困惑度计算公式为  。

因此，在讨论语言的熵或者困惑度时，应该要特别说明是单词(word)、字符(character)还是子单词(subword)级别的。

### 1.4 条件熵与信息增益

在信息论中，条件熵描述了在已知第二个随机变量  {\displaystyle X} 的值的前提下，随机变量  {\displaystyle Y} 的信息熵还有多少。基于

条件的 的信息熵，用 表示 条件熵(Conditional entropy) 为：

另外，如果随机变量 ，则其 联合熵(Joint entropy) 为：

联合熵与条件熵有如下关系成立： ，证明如下：

互信息(Mutual information) 是一个随机变量包含另一个随机变量信息量的度量，也可以说是在给定一个随机变量的条件下，原随机变量的不确定性的减少量，根据熵的连锁规则，有：

据此定义互信息：

![](img/56a51691e5f008db2296d133c0eeed8a.png)

信息增益(Info Gain)指的是经验熵和经验条件熵的差值，经验熵和经验条件熵是通过样本进行极大似然估计得出来的，是一个估计值的差。信息增益是互信息的无偏估计。

### 1.5 率失真理论（Rate Distortion theory）

在通信系统中，冗余度压缩编码以及信道编码，目的都是对信息进行可靠无差错的传输，信息熵没有变化，也就是是保熵的。但是在实际场景中，没有必要把信息的所有内容都保留下来的，即要进行有损压缩。

失真度量（失真函数）是这样一个映射：

它将源字母-恢复字母对映射到一个非负的实数，来代表失真多少。当原始字母集与映射的字母集是离散的，可以用一个矩阵来表示失真的大小，而在连续的情况下，这就是一个函数。失真函数永远都是非负的。如果失真是有限的，则函数为有界函数。

![](img/3b15c6a75b207d492e9dec9b7ccbaadc.png)

失真函数在输入输出联合空间中取统计平均为

在给定信源分布与转移概率分布时，上式为信道传输失真总体的平均度量。

现在想做的事情是， 经过信道之后，得到 ，我们希望 尽可能小，以此来压缩比特的传输。如果互信息为0，则相当于不压缩。压缩下有个前提，就是对失真函数的约束。

![](img/69033ff4f9bf6a7e1bcf52625dc9bf9b.png)

**率失真函数的定义 **：针对信源 和失真度量 ，信息的率失真函数 定义为：

由于无法操作信源，能做的是最小化针对所有可能的条件分布 ，并且使得联合分布满足要求的失真约束。这个函数求得的最小值，并不一定是可操作得到的，只是数学上的一个最小值。

在均方失真度量下，高斯信源的率失真函数是：

图像如下，这个结果告诉我们，“不存在在灰色区域之外运行的压缩系统”。实际压缩系统越接近红色（下限）边界，其性能就越好。作为一般规则，此边界只能通过增加编码块长度参数来实现。

![](img/57ceb54dfd4b09138e34ba9d9facd7ff.png)

上式的证明过程如下：

## 二、 scaling law 的定性解释

好了，在上一节不厌其烦地使用信息论对语言进行定义和分析后，本节讲尝试使用以上理论对 scaling law，由于 scaling law 的内涵比较深刻和复杂以及便于理解，本节将只在定性层面进行解释，更加严格的分析将放在最后一节。

### 2.1 关于 loss 的解释

scaling law 的最核心部分实际上是对于 loss 的估计和判断，即有如下经验公式：

其中 表示无法通过训练减小的损失， 表示计算量， ， 为系数。

![](img/879747398e54f731fe45815ee7a38143.png)

下边来使用上节的理论来解释以上经验公式，由于在当前模型训练都采用的是交叉熵损失，即

然后我们可以不严谨地发现对应关系，即：

+   将 看做 ，即该部分是语言本身的熵，与模型及训练方法无关

+   将 看做 ，该部分与模型与数据均相关，且关系如下：

+   如果数据量 不变，增大参数量 ，由于拟合能力更强，则 变小

+   如果参数量 不变，增加独立同分布数据量 ，则更利于模型拟合，则  变小

+   如果同时增大数据量 和参数量 ，则原理同上

由此，比较定性地解释了 loss 的 scaling 的规律，实际上这种规律有更加准确的经验公式：

这里还可以顺便回答一些问题，

**问题一： **为什么 scaling law 在 Transformer 模型上才起作用？为什么不是 LSTM，RWKV，Mamba 等 RNN 系的模型？

一个解释的角度就是，因为 Transformer 的 self-attention 结构可以有效捕获长文本之间的依赖关系，可以有效拟合语言的真实分布。而 RNN 系的模型，由于衰减因子的存在，无法在较长的范围内拟合语言的真实分布，即 RNN 系模型的  更大。另一个原因则是工程上的，即 RNN 系模型无法实现高效的并行化训练，因此能够参与训练的数据量 更小，进一步导致了  更大。

**问题二： **为什么清洗数据很重要？

在一段正常的文本里加入一些乱码，会显著增加熵值（因为乱码是无法被准确预测的），即相当于增加了系统固定的  。同时，噪声数据和重复数据也会使训练数据的分布偏离真实的语言分布，进而导致拟合过程的 变大，由此会影响最终的训练效果。

**问题三： **词表大小 (vocab size) 如何影响训练效果？

根据第一节的推导，词表大小决定了信息熵 的上界。同时随着词表的增大，每个 token 出现的概率减小，即 增大，这会导致 loss 的增大（如下图所示），但是增大词表又会提高编码效率，那么该如何进行取舍呢？

考虑影响 loss 的另一个因素 ，在模型规模不大时，即使增大词表，也有足够的数据使其训练充分，则大词表在一定范围内是有收益的。但是当模型规模过大时，增大词表时，达到同样拟合效果的数据需要更多，周期也更长，其开销将会大于收益。

![](img/9b8b5f6b7b2ca96d264665a140a3bf43.png)

### 2.2 如何确定 N，D 的相对关系

在讨论数据量 和参数量 的关系之前，需要再回顾一下计算量 与二者的关系，即：  该关系的由来笔者在之前的文章中进行过详细的推导，有兴趣可以查看，在此不予赘述。https://zhuanlan.zhihu.com/p/640641794

[  ](https://zhuanlan.zhihu.com/p/640641794)

事实上，上式的系数 6 在模型规模较大的时比较[接近真实值，在模型较小时偏](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2401.02954.pdf)差较大，因此 Deepseek LLM 将该式描述为 ，其中典型 的计算方式如下：

在不同模型规模下，其关系如下表所示：

在弄清楚 与 的关系后，接下来就需要进一步研究 之间的关系，即分配关系。

在此不加证明地引用论文 [An Information-Theoretic Analysis](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2212.01365.pdf) of Compute-Optimal Neural Scaling Laws 中使用信息论方法，所得到的简化后的 的优化解：

其中 是超参数，则 ，在 Chichilla 论文里该比例的经验值是 20。

![](img/97db677dadb0b2100f7411bb9e94b2a2.png)

### 2.3 如何指导预训练过程的超参数的设置

有了以上的结论之后，我们至少可以知道非常重要的几点信息，包括但不限于：

+   在训练之前预测模型可以达到的大致效果(loss)

+   在预算确定的情况下，如何分配参数量和数据量

即使在完成以上两步之后还有一个重要工作要做，即：如何指导训练过程？其中和训练相关的最主要的两个参数就是 Batch size 和 Learning rate。

通常[而言，在模型确定的情况下，增大 Batch s](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/1812.06162.pdf)ize ，会使数据更加接近真实分布，进而使得梯度估计的噪声较小，因此可以允许更大的 Learning rate。这个关系在论文 An Empirical Model of Large-Batch Training 也得到了验证。

![](img/2d4f3e87e7eb3cba943bc0ee8c402df0.png)

那么如果模型参数量增加了，Learning rate 应该怎么调整呢？通常应该调小，理由如下：

+   梯度下降的稳定性： 在梯度下降过程中，学习率决定了参数更新的步长。对于大型模型，由于参数空间更大，为了确保更新步长的稳定性，避免在最优解附近震荡或发散，通常需要选择更小的学习率。较小的学习率可以帮助模型在复杂的参数空间中更细致地搜索，从而找到更好的解。

+   局部最小值和鞍点： 深度学习中的损失函数通常具有许多局部最小值和鞍点。较大的模型意味着更复杂的损失，因此更容易遇到难以跨越的鞍点。较小的学习率有助于模型在这些区域中缓慢而稳定地更新，而不是大步跳跃，可能会更容易找到全局最小值。

+   参数更新的累积效应： 在大型模型中，由于参数数量众多，即使是很小的学习率也可能导致显著的参数更新。因此，较小的学习率有助于控制参数更新的累积效应，避免过度更新导致的性能下降。

事实上，在 scaling law 中也有同样的经验的发现，以下是 Deepseek LLM 的经验值：

![](img/88e863ff5dd4bd967257a12839f8cf17.png)

## 三、更加严格的幂律的证明

### 3.1 信息压缩与率失真

OpenAI 的成员 Jack Rae 曾在 Stanford MLSys Seminar 的访谈时进行了一个名为 Compression for AGI 的主题分享，其核心观点为： AGI 基础模型的目标是实现对有效信息最大限度的无损压缩。 即 LLM = Compression （GPT 的 Next Token Prediction 本质上是对训练数据的无损压缩）。通过论证压缩即智能，GPT 的训练过程是对数据的无损压缩，从而证明了 GPT 拥有智能。下面通过有损信息压缩的角度一探究竟：

将一组向量 进行有损编码为二进制，然后在失真 范围内恢复成原向量。编码后的序列长度为 。

对于0 均值的高斯分布  的率失真的近似值（每个向量所需的平均比特数）为

其中 表示协方差矩阵 的估计值。

对于 个向量，需要 比特，由于最优编码本对数据 是自适应的，我们还必须用额外的   比特来表示，因此总体编码长度为

根据第一节的介绍，熵  ，但是在真实场景下，真实分布 是很难获取的。另一方面，从概念上讲熵等同于无损编码数据所需的最小位数，因此可以使用最小无损编码长度来表示熵。而对于连续随机变量，无损编码是无法实现的，因此利用有损数据编码中的编码长度作为连续随机变量的熵。

假设 的秩为 , 则上式可以改写为

根据上式，通过实验测量，可以得到下面的图像关系，即大[致满足](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2401.17139.pdf)“幂率”关系，详细实验过程可参考 这里 。

![](img/0fbc7e3bb6332f18d3ba2d3255961448.png)

### 3.2 幂律的证明

接下来将讨论如何从信息理论上推导出缩放定律。LLM 具有通过扩展句子片段生成完整句子的强大能力，假设预训练数据集为  ，然后以 中的句子（段落）作为基本元素，并将其集合表示为  。

假设任何语料库中都有  种可能理解的技能。假定语料库中的每个文本片段都会链接到所有相关的技能，这样就会形成一个双方图，即技能图。

![](img/1575e2634b0e2ffa48ca66d6fc1fc510.png)

假设每种技能 的度数与 成正比，其中我们按照技能在双方图中的度数进行排序。因此，通过对度进行归一化处理，技能分布遵循幂律，即 其中 为定值。

既然我们希望 LLM 能够理解（学习）技能，那么理解技能意味着什么呢？直观地说，一个句子的描述将是一个（二进制）字符串，如果我们真的 "理解 "了它，那么它的长度应该尽可能短，即进行了充分地“压缩”。

在信息论中，代码 是将 映射到二进制字符串空间（码字空间）的函数，其中没有一个码字 是另一个码字 的前缀。代码 的期望码长是分布 下二进制符号的平均数量

根据信息论的标准结果，任何预期代码长度都大于 。

假设密度函数为 。考虑 LLM 在训练过程中的行为，对于随机初始化模型, 假定所有技能的条件熵都是常数 ，对于任何技能 ，特定的 LLM   只有在 时才能理解该技能，其中 。

对于所有技能，我们根据它们的出现概率来排列它们的“难度”。也就是说，对于任意的技能 和   ，如果 ，我们称技能 比  更容易。假设模型从最简单到最难依次学习技能，只有它完全理解了一种技能，它才会学习下一种技能。

假设一个 LLM 掌握 种技能， ， ，那么条件熵  与  服从幂律关系

参数的scaling law，假设每 个神经元都能理解一种技能。则 。

接下来讨论 FLOPs 的 scaling law.

假设在开始理解每个技能时，每次浮点计算都会减少 条件熵。假设每次浮点计算有概率 来学习到技能 。那么对于技能 则需要 次浮点计算才能够理解。由于技能是按顺序学习的，因此经过 flops 后的优化模型可以学到的技能数如下： 则有 。

以上说明了条件熵服从幂律关系，根据第一节的理论 ，由于信息压缩的过程即是对互信息的最小化的过程，即可以将 看作一个定值，则熵也服从幂律。

以上证明过程主要参考 [The Information of Large Lang](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2402.03471v1.pdf)uage Model Geometry ，感兴趣可以参看原文。

## 参考资料

[1] [E](https://link.zhihu.com/?target=https%3A//thegradient.pub/understanding-evaluation-metrics-for-language-models/)valuation Metrics for Language Modeling (thegradient.pub)

[2] [T](https://link.zhihu.com/?target=https%3A//arxiv.org/html/2402.03471v1)he Information of Large Language Model Geometry (arxiv.org)

[3] 2212.01365.pdf (arxiv.org)

[4] [https:// people.math.harvard.edu /~ctm/home/](https://link.zhihu.com/?target=https%3A//people.math.harvard.edu/~ctm/home/text/others/shannon/entropy/entropy.pdf)text/others/shannon/entropy/entropy.pdf

[5] [Generating Sequences With Recur](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1308.0850)rent Neural Networks

[6] 基于量子化假设推导模型的尺度定律（Scaling Law）

[7] [https:// arxiv.org/](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2010.14701.pdf)pdf/2010.1470 1.pdf

[8] [https:// arxiv.org/](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2001.08361.pdf)pdf/2001.0836 1.pdf

[9] [https:// arxiv.org/](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2203.15556.pdf)pdf/2203.1555 6.pdf

[10] [https:// arxiv.org/](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/1812.06162.pdf)pdf/1812.0616 2.pdf

[11] [https:// arxiv.org/](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2401.02954.pdf)pdf/2401.0295 4.pdf

[12] [https:// arxiv.org/](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2303.08774.pdf)pdf/2303.0877 4.pdf

[13] [htt](https://link.zhihu.com/?target=https%3A//www.researchgate.net/figure/Evolution-of-the-average-loss-during-training-with-varying-vocabulary-size-A-step_fig3_329276137)ps://www. researchgate.net/figure /Evolution-of-the-average-loss-during-training-with-varying-vocabulary-size-A-step_fig3_329276137

[14] [https:// arxiv.org/](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/1503.02406.pdf)pdf/1503.0240 6.pdf

[15] [Large Language Model Evaluatio](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2401.17139.pdf)n via Matrix Entropy

[16] [Self-Supervised Learning via Ma](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2210.11464.pdf)ximum Entropy Coding

[17] [M](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2305.17326.pdf)atrix Information Theory for Self-Supervised Learning

[18] [Segmentation of Multivariate Mixed Da](https://link.zhihu.com/?target=https%3A//people.eecs.berkeley.edu/~yima/psfile/Ma-PAMI07.pdf)ta via Lossy Data Coding and Compression

[19] 信息论——信息速率失真函数与熵压缩编码（一）

> 笛声依约芦花里。白鸟成行忽惊起。别来闲整 钓鱼竿 。思 入水 云寒。 —— 潘阆 《酒泉子·长忆西湖》