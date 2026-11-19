
# 拆分`Transformer`注意力，韩国团队让大模型解码提速20倍

> 来源：[https://agijuejin.feishu.cn/docx/SOYBdYEkfoaWogxhsaUc4vVAnZc](https://agijuejin.feishu.cn/docx/SOYBdYEkfoaWogxhsaUc4vVAnZc)

> 克雷西 发自 凹非寺

> 量子位 | 👸号 QbitAI

> 原文：https://mp.weixin.qq.com/s/JE0w-ksh5TRby_TRfjWbzw

只要将注意力切块，就能让大模型解码提速20倍。

来自韩国科学技术研究院、LG和DeepMind的研究人员，提出了一种新的`Transformer`架构。

不仅获得了更快的推理速度，内存开销也大幅度下降。

![](img/8e8a0fbca643308c8a066a06ed6f3737.png)

研究人员详细分析了原始`Transformer`推理速度慢的原因——

原始`Transformer`每生成`Token`就要访问一次全局`KV缓存`，消耗了大量资源。

实际上，这种方法的GPU有效利用率不到1%，其余的99%都用在了内存访问上。

![](img/46e6799fba44f61c48b782a1e6bf63a1.png)

针对这一问题，团队对`Transformer`的注意力机制进行了切块调整，提出了名为`Block Transformer`的新架构。

结果在没有明显质量损失的情况下，推理吞吐量提升了10-20倍。

有网友表示，自己之前也有过类似的思路，但结果模型的性能不足，现在这个方法看上去确实有效削减了`KV缓存`。

![](img/5ebd37fd1d59a1c677010bd196355952.png)

## “切开”`Transformer`的注意力

原始`Transformer`当中，对全局`KV`的频繁访问，导致计算复杂度高、内存占用大，但推理吞吐量却很低。

针对这一问题，作者的核心思路是将原始`Transformer`的全局注意力分解，分成块级注意力和块内注意力。

相应地，块级注意力和块内注意力分别由`Block Decoder`和`Token Decoder`进行处理。

具体的切块数量根据总`Token`数和预设的块大小决定，而块大小的选择，是全局和局部建模之间的平衡——

+   较大的块可以减少块的数量，从而降低`Block Decoder`的计算复杂度，但每个块包含更多的`token`，可能影响局部依赖的建模能力；
+   较小的块包含的`Token`更少，可以提高局部依赖的建模能力，但`Block Decoder`需要处理更多的块，可能增加计算复杂度。

![](img/753567a370820a7230a0bad71995da99.png)

△ 不同块大小的性能比较

工作流程上，`Block Transformer`拿到需要处理的序列之后，直接先进行切块，然后利用`Embedder`将每个块都转换成一个嵌入向量。

具体来说，`Embedder`可以是一个简单的查找表，将块内的`token`映射为对应的嵌入向量，然后将这些嵌入向量拼接或累加得到块嵌入向量。

完成块的向量化之后，`Block Decoder`接收`Embedder`生成的块嵌入向量序列作为输入。

在其每个自注意力层中，都会对块嵌入向量序列进行自注意力计算，捕捉块与块之间的全局依赖关系。

经过多个自注意力层的处理，块嵌入向量融合了全局上下文信息，所以，`Block Decoder`的输出是一个全局上下文感知的块嵌入向量序列。

完成块级处理之后，`Block Decoder`的输出会与块内已生成的`Token`向量一起被`Token Decoder`接收。

在`Token Decoder`中，块嵌入向量首先被转换为与`Token`嵌入向量相同维度的向量，然后在`Token Decoder`的多个自注意力层中进行处理，捕捉`Token`之间的局部依赖关系。

经过多个自注意力层的处理，`Token`嵌入向量融合了局部上下文信息和来自块嵌入向量的全局信息。

最终，`Token Decoder`的输出是一个包含了局部上下文感知的`Token`嵌入向量序列，用于生成当前块的`Token`，`Token Decoder`重复这个过程，直到生成当前块的所有`token`。

回到整体上，`Block Transformer`通过交替执行块级自回归建模和块内自回归解码，迭代生成整个输出序列。

比如在生成第`i`个块时，`Block Decoder`会根据前`i-1`个块的嵌入向量预测第`i`个块的嵌入向量，然后`Token Decoder`根据第`i`个块的嵌入向量和已生成的`Token`，生成第`i`个块的`Token`序列。

这个过程重复进行，直到生成整个输出序列。

## 推理吞吐量最高提升20倍

对注意力的切块带来的效果立竿见影，模型的推理吞吐量直接提升了10-20倍。

例如，在`decode-heavy`设置下，85M参数的`Block Transformer`吞吐量达到了每秒13.5万`Tokens`，而同等大小的原始`Transformer`仅有约6千`Tokens`。

针对更长的提示词，`Block Transformer`同样具有吞吐量优势——在提示词长度为8K的情况下，`Block Transformer`的吞吐量超过了提示词长度为2K的原始`Transformer`。

![](img/a884fdd6490a346c394f8bbd0cf0c83e.png)

吞吐量的提升并没有让质量下降，在HellaSwag、PIQA和ARC-easy等多个零样本任务上，`Block Transformer`的准确率与同等大小的原始`Transformer`相当甚至略高。

![](img/f3bb1bd498a774ad08687d3b78c11922.png)

进一步探究结果表明，`Block Transformer`这种全局-局部建模方式能在提高推理效率的同时保持较低的训练损失（图a）。

同时这种方法还能有效利用全局上下文，在PG19测试集上，取得了与原始`Transformer`相似的位置损失（图b）。

另外，在相同的训练计算量和推理吞吐量预算下，`Block Transformer`能达到比原始`Transformer`更低的训练损失，展现出了优异的训练效率（图c）。

![](img/712c11a6b9aa4a0bb910d528713bd94f.png)

除了带来性能提升之外，`Block Transformer`也降低了模型的训练成本。

使用其默认的4个`Token`的块长度，全局注意力的二次内存访问开销减少了16倍。

反复读取`KV缓存`带来的内存开销也几乎消除，1%的GPU利用率提升到了44%。

![](img/758112164ac6f741222b26934c53fe3d.png)

论文地址：`https://arxiv.org/abs/2406.02657`
