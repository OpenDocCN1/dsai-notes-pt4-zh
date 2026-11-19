
# 自我蒸馏方法-减轻大模型微调过程中的灾难性遗忘

> 来源：[https://geekbang.feishu.cn/docx/A7G1duAkwo5hOuxDUhQc3mEZn0g](https://geekbang.feishu.cn/docx/A7G1duAkwo5hOuxDUhQc3mEZn0g)

> 👨🏻 作者：刘聪NLP

> 📃 作者介绍：AIGC前沿知识分享&落地经验总结

> 🛖 知乎主页：https://www.zhihu.com/people/LiuCongNLP

> ⏰ 收录时间：2024-05-23 11:28:42

> 🔗 原文链接：h[ttps://mp.weixin.q](https://mp.weixin.qq.com/s/caqRWBHRo9m9ivu66B288A)q.com/s/caqRWBHR...

![](img/0fe48b2fe6a4282ac04fc15bfb30f84a.png)

关注作者👸号，获取更多AI知识

## 写在前面

大家好，我是刘聪NLP。

大模型在指定任务上进行微调后，会取得较为不错的效果，但同时可能带来模型原有能力的下降。今天给大家带来一篇通过自我蒸馏减轻大模型微调时的灾难性遗忘的方法-`SDFT`（Self-Distillation Fine-Tuning）。

```
Paper: https://arxiv.org/abs/2402.13669
Github: https://github.com/sail-sg/sdft
```

特定任务微调后导致模型遵循通用指令能力变弱的主要原因是任务数据集的信息分布与原始LLM的信息分布之间存在差距。目前主流解决大模型微调后灾难行遗忘的方法是在微调过程中加入通用的指令数据。

而自我蒸馏方法主要是通过模型本身对任务数据进行生成引导，构建自我蒸馏数据集，改变任务数据的信息分布，减少与原始模型信息分布的差距，如下图所示。

![](img/98ff741ed3cc85cb215c3a5f3b5d6240.png)

## 方法介绍

大模型`SFT`过程，就是将指令和输入的上下文内容，映射到相应的输出上，最小化数据信息分布与语言模型信息分布之间的差异，如下：

其中，  表示模型训练参数，  表示输入上下文内容，  表示指令内容，  表示模型输出。

`SDFT`方法首先根据原始大模型对微调指令数据进行生成回复内容修改，将任务数据的指令回复结果映射到大模型分布内的回复结果，

在重写过程中，减少对大模型的额外要求，仅让其重新回复结果，自我蒸馏提示模板如下图所示，

![](img/ac32987c71a71a27deaac4bfe0f8ab03.png)

自我蒸馏提示模板

然后为了确保蒸馏的回复内容质量，采用简单的启发式方法来评估蒸馏的回复内容。例如，在数学推理问题中，如果可以从蒸馏的回复内容  中提取出最终答案，则采用蒸馏的回复内容；否则保留原始回复内容。

![](img/1ce4dcae09a6e16b7f0a7e69f7c2b74a.png)

PS: 这里跟作者交流过，实际上仅数学任务采用了这种责令，其他不好判断的任务默认蒸馏效果准确。

最后，采用蒸馏后的回复内容替换原始回复内容用于大模型微调，

![](img/a99021e19b831a0c5e7c5f732c76e6ea.png)

## 实验结果

所有实验均利用`Llama-2-chat-7b`模型，采用`Lora`方法训练，学习率初始为1e-4，按照余弦调度策略衰减到0，训练批次大小为8。

数据集涉及单任务和多任务两种数据：

+   单任务：`OpenFunctions`、`GSM8K`和`MagiCoder`；

+   多任务：`Alpaca`、`Dolly`和`LIMA`；

模型在评估过程中，利用`Advbench`榜单进行安全性评估，利用`AlpacaEval`榜单进行实用性评估，利用`OpenLLM`榜单进行知识评估。

如下表所示，普通微调虽然可以增强模型在目标任务上的效果，但也会导致在其他任务上性能显著下降。而`SDFT`可以有效缓解这种性能下降，甚至会有效果提示。

![](img/efb3f70cf4860adc55de7bc014d404ae.png)

如下表所示，在Chat模型上进行普通任务微调，会导致模型对齐效果丧失，也就是安全性下降，而`SDFT`方法可以有效缓解。

![](img/1eda0afa50b28d2f18bd2b9c586c98ae.png)

单任务

![](img/83fed5803bb1c84cbe1f9af47b5aaa1e.png)

多任务

有趣的是，虽然微调会对下游任务有影响，但对模型本身的知识能力影响较小。

![](img/ebd39392c6b8ab6d6f70d7f64f397194.png)

知识能力评估

分析自我蒸馏数据占比对微调的影响，如下图所示，当自我蒸馏数据占比越高时，效果越好。

![](img/04d991f69a4cd5da3e385fa44d928337.png)

如果将自我蒸馏数据与原始数据混合进行训练，发现与混合比例为50%时持平或略低，也可以从侧面体现反应数据质量的作用大于数据数量。

![](img/963e03228717bd7ac884804495002364.png)

虚线为1+1数据结果

对微调后模型与原始模型指令生成结果及嵌入向量进行分析，发现普通微调方法随着数据量增加，偏移越严重，而`SDFT`方法微调后的模型嵌入偏移更小。

![](img/d4891f1e772e56ee7cb1f08871c7ec8e.png)

![](img/fd887de804327c6b3898c3c6b7aa8f00.png)

## 写在最后

自我蒸馏方法在不引入额外数据的情况下，可以极大程度的减轻模型的遗忘现象。后期可以利用外部模型，将自我蒸馏数据保留机制进行完善，说不定会有意想不到的效果。

往期推荐：

+   [Yi技术报告细节分享](https://mp.weixin.qq.com/s?__biz=Mzg5MTU1NTE1OQ==&mid=2247488248&idx=1&sn=da8b5d9b5aa374d56f1641a1e4938283&scene=21#wechat_redirect)

+   大模型增量预训练新技巧-解决灾难性遗忘

+   如何提高LLMs的文本表征(Text Embedding)能力?

+   DEITA-大模型指令微调的数据高效筛选方法

+   大模型微调技巧 | 高质量指令数据筛选方法-MoDS

+   辟谣！微软撤回声称ChatGPT为20B参数的论文，并给出解释。

+   [如何看待微软论文声称 ChatGPT](https://mp.weixin.qq.com/s?__biz=Mzg5MTU1NTE1OQ==&mid=2247487327&idx=1&sn=593923b9ca8fd01a58e769b52d002884&scene=21#wechat_redirect)是 20B (200亿) 参数量的模型？

+   大模型微调技巧-在Embeeding上加入噪音提高指令微调效果

+   如何从数据集中自动识别高质量的指令数据

+   BaiChuan2技术报告细节分享&个人想法

+   大模型LLM微调经验总结&项目更新

+   [打造LLM界的`Web UI`](https://mp.weixin.qq.com/s?__biz=Mzg5MTU1NTE1OQ==&mid=2247486578&idx=1&sn=8fc505e81e2a479bfaf9b5b15d120808&scene=21#wechat_redirect)

+   是我们在训练大模型，还是大模型在训练我们？

+   [`Llama2`技术细节&开源影响](https://mp.weixin.qq.com/s?__biz=Mzg5MTU1NTE1OQ==&mid=2247486512&idx=1&sn=73d2b6dd73051e85bf6b9f630919f3d9&scene=21#wechat_redirect)

+   [大模型时代-行业落地再思考](https://mp.weixin.qq.com/s?__biz=Mzg5MTU1NTE1OQ==&mid=2247486493&idx=1&sn=d58830d103a21e141a20d308f2ad3a83&scene=21#wechat_redirect)

+   垂直领域大模型的一些思考及开源模型汇总

+   如何评估大模型-LLMs的好坏？

+   总结|Prompt在NER场景的应用
