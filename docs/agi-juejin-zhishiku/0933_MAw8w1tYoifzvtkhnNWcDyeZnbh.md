
# RAG葡语实践——以哈利波特问答为例

> 来源：[https://agijuejin.feishu.cn/docx/GsChdhi7EopsTLxKJwbcu4odnqg](https://agijuejin.feishu.cn/docx/GsChdhi7EopsTLxKJwbcu4odnqg)

> 作者：Meta

> 原文：[https://zhuanlan.zhihu.com/p/680217428](https://zhuanlan.zhihu.com/p/680217428)

今天介绍一篇关于RAG实践的文章《The Chronicles of RAG: The Retriever, the Chunk and the Generator》，研究者们以哈利波特的葡语版本为例，构建问答数据集，在基础`Baseline`的基础上对检索过程进行一系列优化，提升了整个RAG系统的能力。将分数从`Baseline`方法的57.88%提升到了98.61%（这里的分数指的是相对理论极限分数的占比）。下面是对整个过程的一些详细介绍。

## 1、数据集构建

![](img/830de49dfa1681666bbbf1095c27178b.png)

将哈利波特全书（共140000`tokens`）按照1000`tokens`作为`chunk size`，拆分为140个`chunk`，然后使用`GPT-4`采用如下的`prompt`生成`QA`的数据集，要求答案需要从关联的上下文中得出。

![](img/d202bf30f32c76a1dbc3a08c8a807bbd.png)

## 2、评价指标

采用`LLM`将结果与`GT`对的关联程度进行评价，分为5个等级，并赋予一定的分数。

![](img/0cbcb097bf2ac2360a5cacf04717d49b.png)

![](img/0b718a8152f1dced2554417024514630.png)

同时以该标准对多个`LLM`在本数据集的分数上限进行了比较。

这里假设每次都检索到了正确的关联文本块，基于文本块的内容进行问答，可以看到几个`LLM`的极限分数大致都在7.5左右。

![](img/5bf51545bd1d346a9b071cfaefb3358c.png)

之后就采用相对极限分数的下降情况，来衡量后续的一些方法的性能。

![](img/9169a17698cad142eb4c61db43a8e093.png)

## 3、基础实验

### 3.1、不提供上下文——直接问答

由于`LLM`也是通过大量的网络文本进行训练，他们本身也包含一定对于哈利波特的知识，只是可能对于其中的一些细节比较模糊。这里实验了各个`LLM`直接对问题进行回答，不提供相关上下文的效果，可以看到直接回答问题相比极限分数有较大的下降。说明需要采用一定的手段将与答案相关的文本输入给大模型，才能取得更好的效果。

![](img/7c2149b07755c21bffab1340aa1d3dc6.png)

### 3.2、长文本的“大海捞针”实验

这里也针对哈利波特数据集做了长文本的大海捞针实验，实验结果也表明在上下文过长时，检索性能较差。

![](img/536ef7b9071a3db5e0a4edf594d71ac3.png)

这也侧面说明了使用RAG对相关上下文进行精简的必要性。关于大海捞针实验一些有趣的实验现象，可以参考笔者之前的文章《 大模型的“Lost in the middle”现象探究 》。

### 3.3、`Naive RAG`的结果

![](img/343fbc1900602ce591fc09e29dae7650.png)

从结果可以看出，比直接不参考任何上下文进行回答，效果好一些，但是相比最理想的情况，还是降低了20%。

![](img/88d1759427f8b3a129fb7c0cdd3cd7d1.png)

## 4、针对检索过程的优化

这部分主要探究了如何对检索过程进行优化，从而提升RAG系统的整体能力。

### 4.1、稀疏检索方式`BM25`

`Pyserini` `BM25`的性能更高一些，后续的`BM25`都采用该实现。

![](img/b659f0b559ce1645071f86742170398a.png)

### 4.2、混合检索——`BM25`+向量召回

![](img/1370e1302dea74594571e1711b835fa1.png)

其中向量召回方式比较了`OpenAI`官方的`ADA-002`嵌入模型和领域训练的自定义`ADA-002`模型。可以看出自定义的嵌入模型对于正负样本对的区分度更高。

![](img/c63bd66aa6bb6d0db0da2af31c925480.png)

结合混合检索的实验也验证了`BM25`+自定义的`ADA-002`的嵌入方式能取得更好的效果。

![](img/a9bce0362cc43981afc6b6a22adacbb2.png)

### 4.3、加上`Rerank`

采用粗排+精排的二阶段召回方式，

![](img/1587dc4005397a55639621420ff592d3.png)

最终的实验结果表明，采用`BM25`进行粗排+`Rerank`模块进行精排，能够取得最好的效果，`Top9`的召回率能达到100%。

![](img/3f41f07206a0c7b41d9eb9baedaef1c1.png)

## 5、整体结论

### 5.1 检索质量影响整体效果

检索能力强，整体的性能也高。

![](img/0b9233d438930047782a39d107af45a0.png)

### 5.2、召回`Top k`的影响

召回`Top3`能取得最好的结果。

![](img/b19e62e886c24bce0a7f1865f6e021eb.png)

### 5.3、最终结果

通过一系列对检索的优化，使得最终的RAG系统性能接近理想情况。

![](img/3e0242d5211ef0d2a86d1ad8a3ddaa9e.png)

## 参考

1. Finardi, Paulo, et al. "The Chronicles of RAG: The Retriever, the Chunk and the Generator." arXiv preprint `arXiv:2401.07883` (2024).
