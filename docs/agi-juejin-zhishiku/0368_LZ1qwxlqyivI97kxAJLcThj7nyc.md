# 如何快速提高大模型的向量表征效果？

> 来源：[https://agijuejin.feishu.cn/docx/UsOFdxskmoYQXXxZgEicIFORnJf](https://agijuejin.feishu.cn/docx/UsOFdxskmoYQXXxZgEicIFORnJf)

> 刘聪NLP｜[阅读原文](https://mp.weixin.qq.com/s/I_yFVeAbUgycN9KkDP1zpQ)

> 转载请联系原作者取得授权

大家好，我是刘聪NLP。

RAG已经成为了大模型落地的重要场景，那么如何retrieval到正确的证据片段变的十分重要。

目前一般采用采用向量匹配的方式来增加语义召回的结果。在METB榜单上，英文向量表征已被大模型（生成模型）占领，中文向量表征还是以BERT模型（编码模型）为主。

![](img/b03f55fa2dace294d89294a786a8b027.png)

英文榜单

![](img/175ba0cdff09d8d4bc5ea095b7e2b6c4.png)

中文榜单

> 之前写过一篇大模型这么强，为什么不使用能LLMs直接进行文本表征?

> 大模型的训练方式都是基于文本续写形式来训练的，损失函数主要预测下一个Token是否准确，而不是判断整个句子表征的好坏。因此直接用当前的大模型来对文本进行表征，效果并不理想。

今天刷到一篇论文《Simple Techniques for Enhancing Sentence Embeddings in Generative Language Models》，主要就是通过提示工程的方法，来增加大模型的直接生成文本表征能力。

```
paper：https://arxiv.org/abs/2404.03921
```

是否还记得想当年的PromptBERT，用过增加提示模板的方法，来直接提高BERT模型的文本表征效果。而对于生成模型，利用[MASK]标记显然是不合理的，毕竟大模型在预训练阶段也没有涉及[MASK]标记。

那么如何更好地将生成模型预测下一个Token的方式，与生成一个向量的偏差更小呢？

主要设计了三种方式，将句子用一个“word”表示（PromptEOL）、用“something”表示（PromptSTH）或者说可以总结成什么内容（PromptSUM）。

+   PromptEOL -> This sentence : "[X]" means in one word:"

+   PromptSTH -> This sentence : "[X]" means something

+   PromptSUM -> This sentence : "[X]" can be summarized as

从结果上来看，在直接生成向量时，PromptEOL方式效果更好，说明与与大模型本身预训练阶段更加吻合。

![](img/ad3dbbcbdb10bf8d5ed624672dd47925.png)

但相较于BERT模型直接生成向量来说，还是存在差距，

![](img/1a857d5836bf7f71a0b6c1550ef8add8.png)

那是不是大模型直接生成向量的效果就是不理想呢？

参考COT和ICL方式，提出Pretended Chain of Thought(假思维链)和Knowledge Enhancement(知识增强)方法：

+   **假思维链 -> After thinking step by step , t**his sentence : “[X]” means in one word:“

+   **知识增强 -> The essence of a sentence is often captured by its main subjects and actions, while desc**riptive terms provide additional but less central details. With this in mind , this sentence : “[X]” means in one word:“

从结果上来看，通过提示词工程，不仅可以激发大模型的推理能力，大模型的向量表征能力同样可以被激发出来。

![](img/b8887ceec172a5a85ae6db8b606880a1.png)

好了好了，这一篇有感而发贴（水贴）。

**主要就是想**表达，其实也许不是大模型 直接表征 能力不行，也许是真的没用对，一些巧思，往往可以有意想不到的结果。

[**欢迎多多关注**](https://mp.weixin.qq.com/s?__biz=Mzg5MTU1NTE1OQ==&mid=2247486601&idx=1&sn=dd2f338675de1448981bcfbe1efa71e3&scene=21#wechat_redirect)公众号「NLP工作站」， 加入交流群 ，交个朋友吧，一起学习，一起进步！