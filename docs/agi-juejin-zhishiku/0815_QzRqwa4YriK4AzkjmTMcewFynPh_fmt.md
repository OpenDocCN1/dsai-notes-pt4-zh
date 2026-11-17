
# 从零复现Llama3代码库爆火，大神Kapathy一键三连，GitHub狂揽2k+

> 来源：[https://agijuejin.feishu.cn/docx/OJctdLnjoo4nKuxgNRScYYWenDb](https://agijuejin.feishu.cn/docx/OJctdLnjoo4nKuxgNRScYYWenDb)

> 每次操作一个向量和矩阵乘法

> 西风 发自 凹非寺

> 量子位 | 👸号 QbitAI

让大神 Andrej Karpathy 一键三连❤️（点赞 + 转发 + 评论），一个教你从头开始实现 `Llama3` 的代码库爆火。

X 上转赞收藏量超 6.8k，GitHub 揽星 2k+。

![](img/ad361bc3f9ea3d85b02ffecd4761d47d.png)

火就火在，它教你从头用 Meta 开源的权重进行推理，详细解释和展开了注意力机制中多个头的矩阵乘法、位置编码以及所有中间层。

换句话说，他解释了每行代码都在干啥。

![](img/318c31a4b9382b4035d07ff4f04f77d6.png)

Karpathy 看后直呼打造者 Nishant Aklecha（后文暂称 “纳哥”）是个有品的人：

![](img/41081b8d8b73cb65228bc8d4d7d2f976.png)

> 完全展开后，比起模块相互嵌套和调用时，更容易理解每一步具体在做什么。

![](img/1fc6951346f2b3852ad1592870158aab.png)

网友们对其也是赞不绝口，纷纷致敬：

![](img/4027afff3caa052893091499af218962.png)

![](img/66170276188028727236d849e8118065.png)

![](img/ef2bb04e80471c0affffdce8ef661089.png)

话不多说，一起来看纳哥是如何手把手教的。

（量子位在不改变原意的基础上，进行了编译整理）

在运行纳哥提供的文件前，大伙儿需要预先下载 Meta 官方提供的 `Llama3` 模型权重。

![](img/6784ab291479189867f8c63f2dca75cb.png)

纳哥表示自己没搞分词器，推荐用 Karpathy 的现成简洁版 `BPE` 代码。

![](img/c05516ab19ebdbd9f6a1bd70d5ff5c53.png)

![](img/485a2ecc2f6eb21c504db7e91d17b3a8.png)

PS：

“字节级（byte-level）”`BPE` 算法，在 `UTF-8` 编码的字符串上运行，广泛应用于大模型分词。Karpathy 提供的这个代码库包含两个分词器，都能在给定文本上训练分词器的词汇表和合并规则、将文本编码为 `token`、将 `token` 解码为文本。

![](img/68b3ffaf817992d9f57cfab36af3cd31.png)

读取模型文件的方式通常取决于 model classes 的编写方式以及 `class` 中变量的命名。但由于纳哥是从头开始实现 `Llama3`，所以将逐个张量地读取文件内容。

![](img/28fd6a12d3d824c491f4b39857f755ac.png)

![](img/48a1e83f31e3a4b41b108e07eab56aff.png)

通过此配置可以推断出模型的结构和参数信息，例如模型包含的 `Transformer` 层数、多头注意力块中的头数，以及词汇表的大小等细节。

![](img/a376c21e18436e7f45daa75068f8d0eb.png)

将文本转换为 `token` 时，纳哥使用 `tiktoken` 作为分词器。

![](img/22effa09f7be0b897da0c3c10ef0075a.png)

![](img/bc369ea7fbd81ff5ca8b9a708c5b773e.png)

接下来，纳哥展示了在代码中将 `token` 转换为高维的嵌入表示。这是代码库中唯一使用内置神经网络模块的部分。

[17×1]的 `token` 矩阵变成了 [17×4096] 的嵌入矩阵。也就是说，每个 `token` 被转换为一个长度为 4096 的嵌入向量，总共有 17 个这样的嵌入向量。

![](img/d97bd0107c953cfb7e2474e94cbcfb9d.png)

![](img/32b35eb358215d14a1fd05ae982142fa.png)

然后，纳哥对嵌入进行 `RMS` 归一化。经过这一步后，嵌入的形状不会改变，只有数值被归一化了。纳哥强调需要一个 `norm_eps`，避免意外将 `RMS` 值设为 0 导致除以 0 的错误。

以下是公式：

![](img/b1a83f664f61d1b77ddf259a8abe5713.png)

![](img/5d84a7134134b34026bdd116a7455ef6.png)

构建 `Transformer` 的第一层，进行归一化处理，从模型字典中访问 `layer.0`（即第一层）。归一化之后，张量的形状仍然是 [17×4096]，与嵌入时相同，但数值已被归一化。

![](img/85b14227d3bf276646c2fd1d7a8b9ae1.png)

![](img/793fb98cbebc715bede8a449abe5d199.png)

跟着纳哥从头实现注意力机制，加载 `Transformer` 第一层的注意力头。

从模型中加载 `query`、`key`、`value` 和 `output` 向量时，它们的形状分别是 [4096×4096]、[1024×4096]、[1024×4096] 和 [4096×4096]。

纳哥表示乍一看有点奇怪，因为理想情况是每个注意力头的 `q`、`k`、`v` 和 `o` 向量是独立的。而代码作者将它们捆绑在一起，是为了方便并行计算注意力头的矩阵乘法。

把所有这些向量解包开来：

![](img/47141e1cd3e7af1d6b86570db0c7ee49.png)

![](img/836eabb9247376822a6febd2cdf41e5f.png)

下一步，纳哥将从多个注意力头中解包 `query`，解包后的形状是 [32x128x4096]，32 是 `Llama3` 中的注意力头数量，128 是 `query` 向量的大小，4096 是 `token` 嵌入的大小。

![](img/8f468ec8646c4a4a8b72917698fde9d7.png)

在这里，纳哥访问了第一层第一个注意力头的 `query` 权重矩阵，`query` 权重矩阵的大小是 [128×4096]。

![](img/246bb3241b4b2bfb421f7e28a403499d.png)

将 `query` 权重矩阵与 `token` 嵌入相乘，获得每个 `token` 的 `query` 向量。结果的形状为 [17×128]，有 17 个 `token`，每个 `token` 对应一个长度为 128 的 `query` 向量。

![](img/e680a53d5df7f7fb3c71188f0387ea3c.png)

![](img/52cde15132cec81c1b34df77261b988b.png)

接下来需要位置编码。

现在已经为 `prompt` 中的每个 `token` 生成了 `query` 向量，但每个单独的 `query` 向量并不知道它在 `prompt` 中的具体位置。

例如，`query`：“the answer to the ultimate question of life, the universe, and everything is”（生命、宇宙和一切的终极问题的答案是）。

在这个 `prompt` 中，使用了三次”`the`”，需要根据它们在 `prompt` 中的位置，使这三个”`the`”`token` 的 `query` 向量有所不同（每个向量的大小为 [1×128]）。

通过使用 `RoPE`（旋转位置嵌入）来进行这些旋转操作。

![](img/15b599edacdd27d24b4945b410187131.png)

![](img/b2db1acba4712c0bead0b293891d142d.png)

上一步中，纳哥将 `query` 向量分成对，并对每一对应用一个旋转角度偏移。

由此，得到的向量大小为 [17x64x2]，这是将长度为 128 的 `query` 向量对每个 `prompt` 中的 `token` 分成 64 对。这 64 对中的每一对都会根据 m*(theta) 进行旋转，其中 m 是要旋转 `query` 的 `token` 的位置。

![](img/55d6dd453317f7873c036d079ed36795.png)

使用复数的点积来旋转一个向量：

![](img/45f0fc425652f0c26f4d413377e39e9b.png)

![](img/0e453df25aac9860d95699ef5fde1da8.png)

![](img/6147b9501e52de3c572ac2e11719c2a8.png)

![](img/0357958bb3f8ea226dfab2b1e6c1d58b.png)

现在每个 `token` 的 `query` 元素都有一个复数（角度变化向量），可以将 `query` 向量（之前分成的对）转换为复数，然后通过点积根据位置旋转 `query` 向量。

![](img/deaf55c4ea6ff5d559cb3754b5dad2f1.png)

获得旋转后的向量后，可以通过将复数重新视为实数来得到成对的 `query` 向量。

![](img/5fd4ef9b77caa9e1a2878fb140b59afb.png)

旋转后的对现在已经合并，有一个新的 `query` 向量（旋转后的 `query` 向量），其形状为 [17×128]，其中 17 是 `token` 的数量，128 是 `query` 向量的维度。

![](img/919b523aab38b9274bac2c4d192fba96.png)

`key` 与 `query` 几乎相同。

![](img/346fd23de4e2c98ea88ba45aae26075c.png)

纳哥表示自己不会详细讲解 `key` 的数学原理，只需要记住以下几点：

`key` 生成的 `key` 向量维度也是 128；`key` 的权重只有 `query` 的四分之一，这是因为 `key` 的权重在同一时间内被 4 个头共享，来减少计算量；`key` 也会旋转添加位置信息，原因与 `query` 相同。

![](img/cabdf01b6cabeb1df1a2b9d814239115.png)

![](img/40a5f1c19646c7dd5c2ad760c7ee32da.png)

此时，纳哥已经为每个 `token` 获得了旋转后的 `query` 和 `key`。每个 `query` 和 `key` 现在的形状都是 [17×128]。

![](img/7ae99b0766820061e8eb297b2f19545f.png)

下一步，纳哥将对 `query` 矩阵和 `key` 矩阵进行相乘操作。这样做会生成一个评分矩阵，将每个 `token` 关联起来。这些评分描述了每个 `token` 的 `query` 与每个 `token` 的 `key` 之间的相关性，这就是自注意力机制。

注意力评分矩阵（`qk_per_token`）的形状为 [17×17]，其中 17 是 `prompt` 中的 `token` 数量。

![](img/e32d6e7d27c73f4397ac1b91781e591a.png)

![](img/e0bc131b63a482a266962509d63d9b0c.png)

接下来需要对 `query key` 评分进行掩码处理。在 `Llama3` 的训练过程中未来 `token` 的 `qk` 评分是被掩码的，只通过过去的 `token` 来预测 `token`。

因此，在推理时，要将未来的 `token` 评分设置为 0。

![](img/c6011c8a38764b4071b07537b4a29673.png)

![](img/bf0ca21ed6c76c51642afff320a6b63f.png)

![](img/55413f15beae08589f295ca8aca90e92.png)

![](img/b5b2d4a2e152fd0675a74d20a6411a81.png)

![](img/4de787bd89ed5e2f6897b9f031456f94.png)

![](img/881b98922ae562493a41f19bdc6741fa.png)

![](img/f6aaf7fe3759003cb23fd9c05abcf799.png)

![](img/ab8634608246ace6f00887723d9e76fe.png)

接下来是 `value`，接近注意力机制的最后一步。

![](img/7474c38641ee604c6033af8de6490e80.png)

这些评分（0-1）用于确定每个 `token` 使用多少 `value` 矩阵。

和 `key` 一样，`value` 的权重也在每 4 个注意力头之间共享，所以下面 `value` 权重矩阵的形状是 [8x128x4096]。

![](img/5d829ad2fc4cb34dd6c3040735eef57c.png)

第一层，第一个注意力头的 `value` 权重矩阵如下所示：

![](img/5a65f41010edc4ac8530d4ef3120f777.png)

然后是 `value` 向量。

![](img/f8837d9d0658463ad349ada9446657e2.png)

使用 `value` 权重来获取每个 `token` 的注意力值，矩阵的大小是 [17×128]，其中 17 是 `prompt` 中的 `token` 数量，128 是每个 `token` 的 `value` 向量的维度。

![](img/abd8a451ad4c33e0481a54e442c38513.png)

注意力：与每个 `token` 的 `value` 相乘后得到的注意力向量的形状为 [17×128]。

![](img/7fff5f22b6fb7eff4724f711564e2e6c.png)

![](img/271038632395bb3b4a1b4db360ff4cb9.png)

现在有了第一层第一个头的注意力 `value`。然后纳哥运行一个循环，对第一层的每个头执行与上面的计算完全相同的数学运算。

![](img/f14262c3dca10316a13f5c71b3dc6f5d.png)

![](img/97d25c9b9fd74a5781594a529192d465.png)

然后得到了第一层所有 32 个头的 `qkv_attention` 矩阵，接下来将所有注意力得分合并成一个大小为 [17×4096] 的大矩阵。

![](img/633f9796329ec9ac382b2c927a92bf6b.png)

![](img/6bd79b02b581d29804ee272076598031.png)

对于第 0 层注意力机制的最后步骤，其一是将注意力得分矩阵与权重矩阵相乘。

![](img/74492a2d0941d69bf14a178de6db277c.png)

![](img/49f337b3b015ce092431d680b328052d.png)

这是一个简单的线性层，所以只需进行矩阵乘法。

![](img/011c502f76c29e1fee1d93e990a6ccbb.png)

![](img/f43c12629818e47365e561c8b556752e.png)

现在得到了注意力机制后的嵌入 `value` 变化，应该被添加到原始的 `token` 嵌入中。

![](img/fd437c0308fe30ea09453c9f374c58cc.png)

对嵌入增量进行归一化处理，然后通过嵌入增量运行一个前馈神经网络。

![](img/95c6469809bc044ffd131ca527478c0c.png)

![](img/557ae308fc87f8941a7c12e62babf61f.png)

在 `Llama3` 中，加载前馈权重并实现前馈网络。使用了一种名为 `SwiGLU` 的前馈网络，这种网络结构在模型需要的时候，能够有效地增加非线性。

![](img/89dc57bbe4736e899e5cf6c2a9db2e5c.png)

![](img/30fd3cfe7395a6d1515cc7cef043337d.png)

现在完成了第一层之后每个 `token` 的新嵌入。现在只剩下 31 层了，只需通过一个循环来完成。

纳哥表示可以将这个编辑后的嵌入想象成包含了第一层中所有查询信息的嵌入。随着层数的增加，每一层都会对输入的信息进行越来越复杂的处理，直到最终得到一个能够全面了解下一个需要预测的 `token` 的嵌入。

![](img/d8708fed7c85c42460070e4d720d8856.png)

之前做的所有事情，对每一层都重复一次。

![](img/29c9aae09c486ea10264547234066130.png)

![](img/c4c811e6a6da8ee073d5187841cd7dcb.png)

然后得到了最终的嵌入，这是模型对下一个 `token` 的最优预测。这个嵌入的形状与常规的 `token` 嵌入相同，为 [17×4096]，其中 17 是 `token` 的数量，4096 是嵌入的维度。

![](img/35c306f136c9b00c1f4aaab91b84686b.png)

![](img/76b0731540a17d847372046972d5dcb5.png)

最后，将嵌入解码成 `token` 值。

![](img/e9f4c9fac7e20f59ada16388e7ab7056.png)

使用输出解码器将最终的嵌入转换成一个 `token`。

![](img/21fa1637d0c10822e66ca9f2f8e8696d.png)

接下来看纳哥使用最后一个 `token` 的嵌入来预测下一个 `value`，希望预测的结果是 42。

因为根据《银河系漫游指南》一书中的说法，42 是 “生命、宇宙及一切的终极问题的答案”。大多数 LLM 在这里都会回答 42，这将验证整个代码的正确性。

![](img/c4af80b329b88c2175811074bed87e9c.png)



# 模型预测下一个 token 的编号

模型预测下一个 token 的编号为 2983。这个编号对应数字 42 吗？

![](img/1c8da68d195b931a843feb4303adb5ea.png)

![](img/8291bc57152bb26731952f354b0cf128.png)

![](img/99a9563e883da2887de19d6f06cfc90d.png)

OK，结束。

## Nishant Aklecha 简介

简单介绍一下 Nishant Aklecha。

![](img/e4813324289c48adf65861da04147be3.png)

Nishant Aklecha 是构建和改进定制语言模型平台 Glaive AI 的研究员，曾任职于摩根士丹利，负责训练和微调大语言模型。

![](img/98e80c45e7f7b5732612ecde994b1c86.png)

此外，他还和朋友一同创立了一个研究实验室，名为 A10（AAAAAAAAAA）。

![](img/df7766b9ec227b64b8e5d6c55d4fd6ad.png)

他们的目标可以总结成一句话：让研究变得更加触手可及。

![](img/cb8b1f4b48d01113e7a39289e492138a.png)

除了放出这个代码库，Nishant Aklecha 可谓好人做到底。

网友想更好地理解这个代码库的内容，Nishant 直接一个 YouTube 视频甩了过来：

![](img/d144ce34648ba0e8ed69416bbb6dc79f.png)

之前 Nishant Aklecha 还曾写过一篇 Blog，详解了潜在一致性模型（LCM），同样收获了不少好评。

![](img/e1d6871fcc6917d52869757e397f7260.png)

啥也不说了，感兴趣的家人们赶紧码住吧。

![](img/9ae9f16627d2362bda810cf511456ff7.png)

GitHub 链接：`github.com/naklecha/ll…`

## 参考链接

[1] `x.com/naklecha/st…`

[2] `naklecha.notion.site/explained-l…`

[3] `www.youtube.com/watch?v=o29…`

[4] `www.youtube.com/watch?v=eMl…`
