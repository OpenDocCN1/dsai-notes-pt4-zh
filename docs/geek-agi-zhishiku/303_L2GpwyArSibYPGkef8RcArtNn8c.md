# Unlimiformer：一个Transformers输入无限长文本思路和中文长文本摘要上的性能实验

> 来源：[https://geekbang.feishu.cn/docx/WvNjdQGdboBMbyxr0gJciWZinee](https://geekbang.feishu.cn/docx/WvNjdQGdboBMbyxr0gJciWZinee)

> 👨🏻 作者：余俊晖

> 📃 作者介绍：一名NLP炼丹师，NLP算法竞赛TOP选手

> 🛖 公众号：大模型自然语言处理

> ⏰ 收录时间：2024-05-28 17:09:42

> 🔗 原文链接：h[ttps://mp.weixin.q](https://mp.weixin.qq.com/s?__biz=Mzg4NjI0NDg0Ng==&mid=2247484059&idx=1&sn=aa71a80db1c1f239240be11b79b23c93&chksm=cf9dd61af8ea5f0c422eff1213f9fd4b3a7cfd93447d52825b2f025a94cd10635dbed2aad077#rd)q.com/s?__biz=Mz...

![](img/b23be9761e72b138344d8fdbe9da31ba.png)

关注作者公众号，获取更多AI知识

## ** ****1、前言 **

**在处理****长文本****输入时，以往方法常**采用** 截断 （如：max_le**n=512）、 分块 （将输入分成多个块，并进行逐块处理）、 长文本输入的模型 （如：Longformer、BigBird和Reformer等）。由于编码器上下文窗口的固定大小，Transformer 在其最大输入长度上受到限制。本文将介绍一种能输入无限长文本的思路。名为 Unlimiformer ，可以扩展预训练的编码器-解码器Transformer模型的输入长度，使其能够处理无限长度的输入。传统的Transformer模型因为需要对输入中的每个标记进行注意力计算，因此输入长度通常会被限制在一定的范围内。Unlimiformer通过将注意力计算分散到一个k最近邻索引中，可以处理极长的输入序列。该方法可以应用于各种长文档和多文档摘要任务中，并且可以通过注入Unlimiformer来提高已有的预训练模型的性能，而不需要额外的训练。

文章来源：https://arxiv.org/abs/2305.01625

代码链接：https://github.com/abertsch72/unlimiformer

## ** ****2、Unlimiformer **

### ** 2.1、Encoding **

为了对超长输入序列进行编码，Unlimiform**er采用了重**叠块编码的方法，并使用类似 Faiss 的库将编码后的输入存储在数据存储器中。

### ** 2.2、Retrieval-augmen**ted cross-attention

Retrieval-augmented cross-attention它在标准的cross-attention上进行了改进，使得decoder不仅仅只关注encoder输入序列的前k个token，而是检索了整个输入序列中的top-k个hidden states，然后针对这些top-k的hidden states进行attention计算。这种方法不仅可以检索整个输入序列，而且计算量和GPU内存的使用也比全局attention更加高效，同时保留了99%以上的attention质量。Retrieval-augmented cross-attention的具体实现过程中，引入了一个datastore来存储编码后的输入序列，使用kNN搜索来检索hidden states，同时通过Attention reformulation的方法来优化注意力计算过程，使得可以使用单个datastore来支持所有attention heads和decoder layers的检索，从而大大降低了时间和空间复杂度。图 2 显示了对任何序列到序列转换器架构的通用更改。完整的输入使用块中的编码器进行编码并存储在数据存储中；然后，在每个解码步骤中查询编码隐藏状态的数据存储。kNN 搜索步骤是非参数的，可以注入任何预训练的 seq2seq 转换器。搜索步骤将注意力重新制定为空间效率。在下面例子中，编码器的最大输入长度为 2 个标记。6 令牌输入以块编码并存储在数据存储中。在交叉注意之前，将 Unlimiformer 注入每个解码器层。在 Unlimiformer 中，执行 kNN 搜索以从数据存储为每个注意力头选择 2 个标记上下文；然后，使用整个输入序列的键和值计算交叉注意。

![](img/1ce05bf56bd5195368aa303637578cb8.png)

###  2.3、Attention reformulation

简单的说，Attention reformulation是一种针对transformer模型encoder-decoder结构中的attention机制进行改进的方法。具体而言，传统的transformer模型中，encoder和decoder各自有一个固定的context window，但是在不同的解码阶段，不同的信息可能是相关的，不同的attention头也可能关注不同类型的信息。因此，一个固定的context window可能会浪费精力在某些attention头并没有强烈关注的token上。Attention reformulation允许每个attention头在每个解码步骤中从完整的输入序列中选择一个独立的context window。这通过在decoder之前注入一个Unlimiformer查找来实现：在交叉注意力之前，模型在外部数据存储中执行一个k最近邻搜索，以选择每个解码器层每个attention头要关注的一组token。

## ** ****3、局限性 **

1.  需要一个外部的数据存储器来存储输入序列的编码表示，这会增加存储和计算成本。

1.  需要进行KNN搜索来选择每个注意力头的上下文窗口，这也会带来一定的计算复杂度。

1.  在处理非常长的输入序列时效果很好，但在处理较短的输入序列时可能会带来一些额外的计算开销。

## ** **4、生成式长文本摘要的插拔实践

```
from transformers import (
    AutoConfig,
    AutoModelForSeq2SeqLM,
    AutoTokenizer,
    EarlyStoppingCallback,
    set_seed, WEIGHTS_NAME,
)

...
# 常规定义模型
model = AutoModelForSeq2SeqLM.from_pretrained(
    model_args.model_name_or_path,
    from_tf=bool(&quot;.ckpt&quot; in model_args.model_name_or_path),
    config=config,
    cache_dir=model_args.cache_dir,
    revision=model_args.model_revision,
    use_auth_token=training_args.use_auth_token,
)


# 转换成Unlimiformer以兼容无限长度文本输入
from unlimiformer import Unlimiformer
from random_training_unlimiformer import RandomTrainingUnlimiformer
...
model = RandomTrainingUnlimiformer.convert_model(model, **unlimiformer_kwargs)

```

## ** **5、中文生成式长文本摘要上的实践表现

原文仅在英文的摘要数据集上进行实验，本文在NLPCC中文长文本摘要数据集上进行了实验小试对比：

| 模型 | 性能（ROUGE-L） |
| --- | --- |
| BART | 49.074 |
| UnlimiformerBart | 52.45 |

## ** ****往期相关 **

【炼丹回忆】2021全球开放数据应用创新大赛-法律咨询问答亚军方案