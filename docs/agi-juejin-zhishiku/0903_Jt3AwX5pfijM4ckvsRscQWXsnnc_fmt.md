
# [RAG大赏] Atlas: (无中生)有监督微调RAG系统

> 来源：[https://agijuejin.feishu.cn/docx/DTHKd8kkooKvZrxarhNcfFzJn5f](https://agijuejin.feishu.cn/docx/DTHKd8kkooKvZrxarhNcfFzJn5f)
>
> 作者：[咸鱼王](https://www.zhihu.com/people/wow-kong)
>
> 原文：`https://zhuanlan.zhihu.com/p/665455467`

Atlas是一种 需要微调LLM 而非 prompt engineering based 的 RAG 方案，其优点是通过 `Object Function` 设计，无需有监督预料即可完成模型和 `Retriever` 的训练。尽管深度学习领域正在往力大砖飞的方向发展，我们仍可以从这些颇具美感的设计中受益良多。

![](img/d44ce9c0a816ec2da9fe8fa0775c882e.png)

**论文链接：**

[Atlas: Few-shot Learning with Retrieval Augmented Language Models](https://link.zhihu.com/?target=https%3A//www.jmlr.org/papers/volume24/23-0037/23-0037.pdf)

**开源代码：**

`https://github.com/facebookresearch/atlas`

## 01 背景

引入 RAG 的目的基本有二：

1. 帮助模型解决/缓解知识密集型任务下的幻觉问题。哪怕是强如 GPT4，在非外挂知识库的情况下，也会情不自禁的“胡言乱语”，给用户带来极大的信任危机。通过引入外部知识，可以限制模型尽量在 `Reference` 内进行指令执行，从而避免/缓解模型的幻觉问题。

1. 帮助模型与真实世界实时交互。 LLM 的预料收集与处理、训练是一个非实时流程，RAG 可以帮助模型与真实世界进行实时信息交互。

RAG 的引入本质上是将 LLM 的部分压力转嫁给了检索系统，当检索系统准确率较低、信息质量较差甚至相互矛盾时，反而会产生负面效果。同样的，如果 LLM 不能合理利用 `Reference`，妥善处理 `Reference` 中的虚假信息、冲突信息，即便 retriever 效果再好，结果也不会理想。

因此笔者认为，能够较好理解复杂 instruction 的大规模 LLM，更适合 prompt engineering 的 RAG 方案；无法直接遵循复杂 instruction 的较小 LLM，更适合 fine-tune 的方案。本篇论文的作者就提供了后者的一个典型应用：11B 模型 + `fine_tune` 即可“秒杀”千亿级别模型。

## 02 方法

事实上，RAG 效果的好坏并不仅仅依赖于 LLM，不论是 prompt engineering 还是 fine-tune，更多只是教会模型合理利用 reference 进行生成，对于百亿或以上级别的模型而言，这是一点即通的事情，很多时候瓶颈反而在于如何更加精准的检索到 `Reference`，也即检索系统或 retriever 的优化。本篇论文的重点就在于如何不借助有监督数据，（在仅基于 `src-tgt` 平行语料训练 RAG 系统的过程中）自适应的得到一个更加垂域化、效果更好的 retriever。

Atlas 是典型的 FiD（Fusion in decoder）框架，并且 LLM 与 retriever 协同训练。LLM 选用了体量较小的 T5 模型，retriever 初始化自基于 MoCo 预训练的 Contriver 模型，MoCo 相关可见笔者之前的文章，此处不再赘述。接下来将着重介绍作者给出的训练 retriever 的 4 种 `Object function`。

`AttentionDistillation` (`ADist`) 从注意力的角度进行设计，基本思想是 LLM 对各个 documents 的“关注”程度，间接体现出了 query 与各 documents 的相似度或相关性。“关注”程度显而易见，可以通过 `cross_attention` 计算得到。因此 `ADist` 的目的，就是以 LLM 为教师，教会 retriever 区分 query 与 document 的相似度。具体而言，就是通过 `attention_prob`、`qd_sim_prob` 的 KL 散度，训练 retriever。这里的 `attention_prob`。具体公式如下：

```
KL(p_{ATTN}||p_{RETR})=\Sigma_{k=1}^Kp_{ATTN}(\mathbf{d}_k)log(p_{ATTN}(\mathbf{d}_k)/p_{RETR}(\mathbf{d}_k))
```

`End-to-end training of Multi-Document Reader and Retriever` (`EMDR2`) 启发自 EM 算法，如下式。该式等同于计算 \Sigma_{k=1}^Kp(\mathbf{d}_k|\mathbf{q},\mathbf{a};\Theta,\Phi) ，即通过最大化该后验来更新 retriever 的参数。在计算 `loss` 时，固化的 LLM 会依赖各个 document 分别计算 token distribution。

```
-log[\Sigma_{k=1}^Kp_{LM}(\mathbf{a}|\mathbf{q},\mathbf{d}_k)p_{RETR}(\mathbf{d}_k|\mathbf{q})]
```

`Likelihood Distillation` (`LDist`) 通过预估各个 document 能给 LLM 预测正确结果带来多少增益，来训练 retriever。具体如下式，通过分别依赖于各个 document 计算 ground truth 的 log prob，并沿 documents 进行归一化，即可得到 `p_{LDist}(\mathbf{d}_k)`，其涵义就是各 document \mathbf{d}_k 对输出 ground truth 的相对增益，说白了 `p_{LDist}(\mathbf{d}_k)` 就是评价 \mathbf{d}_k 有多有用、与 query 有多相关。通过让 retriever 的相似度拟合该分布，即可训练 retriever 学会这种相似度判断。

```
p_{LDist}(\mathbf{d}_k)=exp(logp_{LM}(\mathbf{a}|\mathbf{d}_k,\mathbf{q})/\Sigma_{i=1}^Kexp(logp_{LM}(\mathbf{a}|\mathbf{d}_i,\mathbf{q}))
```

`Leave-one-out Likelihood Distillation` (`LOOP`) 和 `LDist` 的思想十分相似，拟合的是 reference 去掉 \mathbf{d}_k 后对 LLM 预测 ground truth 带来的减益，所以叫 leave-one-out，具体如下式。在计算 `loss` 时，同样是拟合下式与 retriever 相似度分布的 KL 散度来训练 retriever。

```
p_{LOOP}(\mathbf{d}_k)=exp(-logp_{LM}(\mathbf{a}|D_K\backslash\{\mathbf{d}_k\},\mathbf{q})/\Sigma_{i=1}^Kexp(-logp_{LM}(\mathbf{a}|D_K\backslash\{\mathbf{d}_k\},\mathbf{q}))
```

Atlas 通过 LLM 与 retriever 联合训练，实现了：1. 使 LLM 适应基于 reference 进行生成的 generation 模式；2. 以指导 LLM 输出正确结果为目标，自适应调教 retriever。本篇文章的核心和亮点主要在于 retriever 的训练方案设计，LLM 训练方式不再赘述，主要包括 `NextTokenPrediction` 和 MLM。

另外，retriever 训练时如何妥善处理 `embedding_table` 的动态更新，也是老生常谈的一个问题，这里同样不作为本篇文章重点。Atlas 的做法是固化 `document_encoder`，只更新 `query_encoder` 部分，如此一来 `embedding table` 就无需再更新，缺点就是随着 `query_encoder` 的不断迭代偏移，二者可能不再处于同一特征空间，对最终索引效果有一定负面影响。

## 03 实验

首先看下本篇文章重点介绍的 4 个 `Object function` 到底是真功夫还是花拳绣腿。如下图，经消融实验评测，首先可以看出加入 RAG 对模型的显著增益（15 个点以上）；其次，64-shot 下各 `loss` 相比固化 retriever 都能带来 1 个点以上的提升，其中 `LDist` 效果最为亮眼；但比较尴尬的是 1024-shot 下各 `loss` 反而基本呈现了负增益，作者将其归因于经过训练后模型掌握了从 reference 使用、聚合信息的能力。考虑到 `LDist` 效果相对最为稳定，作者在后续的实验中只使用了 `LDist`。

![](img/3b029a68eb8cf19e44376d2d78cfa1f8.png)

作者又在不同量级、不同训练语料数下验证了 Atlas 的效果（测试集为 MMLU）。可以看出，Atlas 稳定显著优于非 RAG 模式。

![](img/ead2d06d5091e20732dc5074420983ae.png)

同样以 MMLU 为测试集，作者对比了 Atlas 与其他模型的差异，可以看出 Atlas 以小博大，在 5-shot/transfer 下甚至优于 GPT-3。

![](img/c88ccca9de5aff5d79df6b674c162afa.png)

在 NQ、triviaQA 测试下，Atlas-11B 在 64-shot 和全量微调下取得 SOTA。

![](img/c99db4a680a4b4ed2b712cfd8e995ab9.png)

## 04 总结

以指导 LLM 输出 ground truth 为目标训练 retriever，其目标比仅基于语义相似度的训练方式更加直接、深刻、本质，该目标背后的意义可能包含了语义相似，也可能包含了 reference 是否合理、常规、信息充足等 targets。

整体来说，或许 retriever 的 `loss` 计算流程相对复杂、繁琐，难以在一般场景中得到广泛应用，但仍不失为一种值得借鉴与深思的方案。
