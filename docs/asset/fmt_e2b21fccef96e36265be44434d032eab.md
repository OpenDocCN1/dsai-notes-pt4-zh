
# 如何快速提高大模型的向量表征效果？

> 来源：`https://geekbang.feishu.cn/docx/COgQdo1pGoy82exmk0zc2K21nBf`
>
> 👨🏻 作者：刘聪`NLP`
>
> 📃 作者介绍：AIGC前沿知识分享&落地经验总结
>
> 🛖 知乎主页：`https://www.zhihu.com/people/LiuCongNLP`
>
> ⏰ 收录时间：2024-05-23 11:28:42
>
> 🔗 原文链接：`https://mp.weixin.qq.com/s/I_yFVeAbUgycN9KkDP1zpQ`

![](img/32979221dc41124ae57fdb9820d21c6a.png)

关注作者👸号，获取更多AI知识

大家好，我是刘聪`NLP`。

`RAG`已经成为了大模型落地的重要场景，那么如何`retrieval`到正确的证据片段变的十分重要。

目前一般采用采用向量匹配的方式来增加语义召回的结果。在`METB`榜单上，英文向量表征已被大模型（生成模型）占领，中文向量表征还是以`BERT`模型（编码模型）为主。

![](img/2ce6542f790af9a3be87671f2b7178f7.png)

英文榜单

![](img/ab5ba2ff03106d0b589c66573d4395cc.png)

中文榜单

> 之前写过一篇大模型这么强，为什么不使用能`LLMs`直接进行文本表征?

> 大模型的训练方式都是基于文本续写形式来训练的，损失函数主要预测下一个`Token`是否准确，而不是判断整个句子表征的好坏。因此直接用当前的大模型来对文本进行表征，效果并不理想。

今天刷到一篇论文《Simple Techniques for Enhancing Sentence Embeddings in Generative Language Models》，主要就是通过提示工程的方法，来增加大模型的直接生成文本表征能力。

```
paper：https://arxiv.org/abs/2404.03921
```

是否还记得想当年的`PromptBERT`，用过增加提示模板的方法，来直接提高`BERT`模型的文本表征效果。而对于生成模型，利用`[MASK]`标记显然是不合理的，毕竟大模型在预训练阶段也没有涉及`[MASK]`标记。

那么如何更好地将生成模型预测下一个`Token`的方式，与生成一个向量的偏差更小呢？

主要设计了三种方式，将句子用一个“word”表示（`PromptEOL`）、用“something”表示（`PromptSTH`）或者说可以总结成什么内容（`PromptSUM`）。

+   `PromptEOL` -> `This sentence : "[X]" means in one word:" `

    `PromptEOL` -> 这句话：“[X]”的意思是一个词：”

+   `PromptSTH` -> `This sentence : "[X]" means something `

    `PromptSTH` -> 这句话：“[X]”是什么意思

+   `PromptSUM` -> `This sentence : "[X]" can be summarized as `

    `PromptSUM` -> 这句话：“[X]”可以概括为

从结果上来看，在直接生成向量时，`PromptEOL`方式效果更好，说明与与大模型本身预训练阶段更加吻合。

![](img/90c9d0f942f9760237e559ecff984d83.png)

但相较于`BERT`模型直接生成向量来说，还是存在差距，

![](img/5653fe7a2e8e3c87848e5e6df998a238.png)

那是不是大模型直接生成向量的效果就是不理想呢？

参考`COT`和`ICL`方式，提出`Pretended Chain of Thought`(假思维链)和`Knowledge Enhancement`(知识增强)方法：

+   `假思维链` -> `After thinking step by step , this sentence : “[X]” means in one word:“ `

    假思维链 ->

+   `知识增强` -> `The essence of a sentence is often captured by its main subjects and actions, while descriptive terms provide additional but less central details. With this in mind , this sentence : “[X]” means in one word:“ `

    知识增强 -> 句子的本质通常由其主要主语和动作来捕捉，而描述性术语则提供额外但不太中心的细节。考虑到这一点，这句话“[X]”的意思是一个词：”

从结果上来看，通过提示词工程，不仅可以激发大模型的推理能力，大模型的向量表征能力同样可以被激发出来。

![](img/b44b28c2e9c6e19bed1bff0c5c88f2ee.png)

好了好了，这一篇有感而发贴（水贴）。

主要就是想表达，其实也许不是大模型 直接表征 能力不行，也许是真的没用对，一些巧思，往往可以有意想不到的结果。

## 往期推荐：

+   RAG系统中答案无关片段对`LLM`s生成答案有何影响？

+   [`InternLM2技术报告`](https://mp.weixin.qq.com/s?__biz=Mzg5MTU1NTE1OQ==&mid=2247488805&idx=1&sn=7d760f16e4752b82332f99b588e81895&scene=21#wechat_redirect)

+   `Qwen1.5-MoE`模型：2.7B的激活参数量达到7B模型的性能

+   RAG与`Long-Context`之争—没必要争

+   [`角色扮演大模型的碎碎念`](https://mp.weixin.qq.com/s?__biz=Mzg5MTU1NTE1OQ==&mid=2247488277&idx=1&sn=eb1b190b527f15e2cba0c59af4d91199&scene=21#wechat_redirect)

+   自我蒸馏方法-减轻大模型微调过程中的灾难性遗忘

+   [`Yi技术报告细节分享`](https://mp.weixin.qq.com/s?__biz=Mzg5MTU1NTE1OQ==&mid=2247488248&idx=1&sn=da8b5d9b5aa374d56f1641a1e4938283&scene=21#wechat_redirect)

+   大模型增量预训练新技巧-解决灾难性遗忘

+   如何提高`LLM`s的文本表征(`Text Embedding`)能力?

+   `DEITA`-大模型指令微调的数据高效筛选方法

+   大模型微调技巧 | 高质量指令数据筛选方法-`MoDS`

+   辟谣！微软撤回声称`ChatGPT`为20B参数的论文，并给出解释。

+   [`如何看待微软论文声称 ChatGPT 是 20B (200亿) 参数量的模型？`](https://mp.weixin.qq.com/s?__biz=Mzg5MTU1NTE1OQ==&mid=2247487327&idx=1&sn=593923b9ca8fd01a58e769b52d002884&scene=21#wechat_redirect)

+   大模型微调技巧-在`Embeeding`上加入噪音提高指令微调效果

+   如何从数据集中自动识别高质量的指令数据

+   `BaiChuan2`技术报告细节分享&个人想法

+   大模型`LLM`微调经验总结&项目更新

+   [`打造LLM界的Web UI`](https://mp.weixin.qq.com/s?__biz=Mzg5MTU1NTE1OQ==&mid=2247486578&idx=1&sn=8fc505e81e2a479bfaf9b5b15d120808&scene=21#wechat_redirect)

+   是我们在训练大模型，还是大模型在训练我们？

+   [`Llama2`技术细节&开源影响](https://mp.weixin.qq.com/s?__biz=Mzg5MTU1NTE1OQ==&mid=2247486512&idx=1&sn=73d2b6dd73051e85bf6b9f630919f3d9&scene=21#wechat_redirect)

+   [`大模型时代-行业落地再思考`](https://mp.weixin.qq.com/s?__biz=Mzg5MTU1NTE1OQ==&mid=2247486493&idx=1&sn=d58830d103a21e141a20d308f2ad3a83&scene=21#wechat_redirect)

+   垂直领域大模型的一些思考及开源模型汇总

+   如何评估大模型-`LLM`s的好坏？

+   总结|`Prompt`在`NER`场景的应用
