
# 翻译技术｜提升 ChatGPT 翻译质量的简单 prompt

> 来源：[https://my.feishu.cn/docx/FnojdXQPjoSfi7xsWzYc31J3nJd](https://my.feishu.cn/docx/FnojdXQPjoSfi7xsWzYc31J3nJd)

内容来源：

@宝玉 https://baoyu.io/blog/prompt-engineering/translator-gpt-prompt-v2

@王真真翻译圈 撰文 https://mp.weixin.qq.com/s/Z1RVboQZRg1ybyoQhxn_dg

因为 ChatGPT 3.5 可以免费使用，所以只测试了 ChatGPT 3.5 的 prompt，但是 ChatGPT 3.5 的能力要比 ChatGPT 4 弱很多，若有 ChatGPT 4 的，可以参考 ChatGPT 4 的 prompt，效果很好！大家可以试试。

## 参考 prompt：ChatGPT 3.5

```
你是一位专业中文翻译，擅长英译中翻译，并且擅长对翻译结果进行二次修改和润色成通俗易懂的中文。我希望你能帮我将以下英文段落直译翻译成中文，而后根据直译结果重新意译和润色。

规则：
    - 这些字幕包含机器学习或 AI 等专业知识相关，注意翻译时术语的准确性；
    - 保留特定的英文术语、数字或名字，并在其前后加上空格，例如：“生成式 AI 产品”，“不超过 10 秒”；
    - 首先第一步按照字面意思直译翻译这一段文本内容；
    - 然后基于直译结果重新意译，意译时务必对照原始英文，不要添加也不要遗漏内容，并以让翻译结果通俗易懂，符合中文表达习惯。
```

举例：

**英文原文**

Yeah, I will talk about large-language models in 2023. Why do we need that suffix in 2023? What we call large is misleading in that large models of today will be small models only in a few years. Along with the change in the perception of scale, many insights, observations, and conclusions we make along the way with the current large-language models will be outdated. And in some cases, invalidated. Fortunately, insights based on church principles or that are fundamental tend to stay relatively longer than the advanced ideas that look so fancy. In this talk, I would like to share such fundamental ideas that I have observed working in the field for the past four years, and mostly by observing some of the brightest minds that I'm fortunate to have interacted with. My hope is that whatever I talk about today is fundamental enough that it stays relevant at least for a few years. Let's get started. There's one unique aspects of large language models, and that is some abilities only emerge at certain scale. And this makes really important to just have a different perspective in viewing all this field. So here's what l mean. Details don't matter here. X-axis is some scale. You can think of it as a model parameter count, training data, training compute, whatever. You just think about some scale. Y-axis is also not that important. Some performance metrics of the task that you care about. And we have seen over and over this kind of pattern where small models just don't have any chance of solving it, 0% or random guessing. And at some scale, suddenly the models solve this task, and sometimes very well. And this kind of phenomenon, we call it an emergence, a phase change like sudden emergence of this capability. And this is very unique for this large language models. And I think this brings many important perspectives change for AI researchers.

**直译结果**

在2023年，我将谈论大型语言模型。为什么我们需要在2023年加上这个后缀呢？我们所谓的“大型”可能会误导，因为今天的大型模型在未来几年可能仅仅被视为小型模型。随着对规模感知的变化，我们与当前大型语言模型一同获得的许多见解、观察和结论将会过时，有些情况下甚至会被推翻。幸运的是，基于基本原则或根本性的见解通常比那些看起来很花俏的先进思想更具持久性。在这次演讲中，我想分享一些我在过去四年在这一领域中观察到的基本思想，大部分是通过与我有幸互动的一些最聪明的头脑。我希望今天我所谈论的内容足够基本，至少能在未来几年保持相关性。让我们开始吧。大型语言模型有一个独特之处，那就是一些能力只在特定规模下才会显现出来。这使得以不同的角度来看待这一领域变得非常重要。这就是我所说的。细节并不重要。X轴代表某种规模，可以将其视为模型参数数量、训练数据、训练计算等等。只要考虑某种规模即可。Y轴也不那么重要，代表了您关心的任务的某些性能指标。我们一次又一次地看到这种模式，其中小型模型根本没有解决这个任务的机会，成功率为0%或纯属随机猜测。而在某个规模下，突然之间，模型能够解决这个任务，有时做得非常出色。我们称这种现象为“出现”或“阶段性改变”，就像这种能力的突然出现一样。而这对于大型语言模型来说是非常独特的。我认为这给AI研究者带来了许多重要的视角变化。

**意译和润色后：**

2023年，我将谈论大型语言模型。为什么我们需要在这个年份加上这个后缀呢？之所以称之为“大型”，这个称谓可能会误导，因为今天的大型模型将来可能只能算是小型模型。随着我们对规模的认知不断发展，我们与当前的大型语言模型共同获得的许多见解、观察和结论将会变得陈旧，有些甚至可能被推翻。幸运的是，基于基本原则或根本性的观点通常比那些看似高级但稍纵即逝的思想更加持久。在这次演讲中，我想分享我在过去四年中在这一领域中所观察到的一些基本思想，大部分是通过与我有幸互动的一些最聪明的头脑所得出的。我希望今天我所谈论的内容足够基本，至少在未来几年仍然具有相关性。让我们开始吧。大型语言模型有一个独特之处，即某些能力只有在特定规模下才会显现。这使得以不同的视角来看待这一领域变得至关重要。我来解释一下。不用关心具体的细节。X轴代表某种规模，可以将其视为模型参数数量、训练数据、训练计算等等。只要考虑某种规模即可。Y轴也不是最重要的，代表了您关心的任务的某些性能指标。我们一次又一次地看到这种模式，其中小型模型根本没有解决这个任务的机会，成功率为0%或纯属随机猜测。然而，在某个规模下，模型突然能够解决这个任务，有时做得非常出色。我们将这种现象称为“出现”或“阶段性改变”，就像这种能力的突然显现一样。而这种情况对于大型语言模型来说非常独特，我认为这为AI研究者带来了许多重要的视角变化。

虽然直译翻译的也可以，但是意译后的效果明显更好一些！

## 参考 prompt：ChatGPT 4（复杂版）

```
你是一位精通简体中文的专业翻译，我希望你能帮我将以下英文视频字幕翻译成中文。

规则：
    - 这些字幕可能和机器学习或 AI 等专业知识相关，注意翻译时术语的准确性：译文需要通俗、简洁、易懂；
    - 保留特定的英文术语或名字，并在其前后加上空格，例如：“中 English 文”；
    - 字幕在语音识别时可能有错别字，请注意纠错；
    - 本条消息包含完整的字幕内容，但你不需要翻译，只需要回复 OK。我会分段在后续消息中发送给你翻译，每次你只需要翻译一段。
    - 翻译时采用以下步骤，每一步都完整打印结果：
        1. 第一步，按照字面意思直译翻译这一段文本内容；
        2. 第二步，参照第一步直译的结果，结合上下文，对内容进行意译；
        3. 第三步，参照直译和意译的结果，结合上下文，采用创意的方式对结果进行翻译；
        4. 第四步，现在假设你是个中学语文老师，阅读上面三个翻译结果，然后融合所有翻译结果的优点，重写翻译结果，忠于原意，符合上下文，通俗易懂。
```

## 参考 prompt：ChatGPT 4（精简版）

```
你是一位精通简体中文的专业翻译，曾参与《纽约时报》和《经济学人》中文版的翻译工作，因此对于新闻和时事文章的翻译有深入的理解。我希望你能帮我将以下英文新闻段落翻译成中文，风格与上述杂志的中文版相似。

规则：
    - 翻译时要准确传达新闻事实和背景；
    - 保留特定的英文术语或名字，并在其前后加上空格，例如：“中 UN 文”；
    - 分成两次翻译，并且打印每一次结果：
        1. 根据新闻内容直译，不要遗漏任何信息；
        2. 根据第一次直译的结果重新意译，遵守原意的前提下让内容更通俗易懂，符合中文表达习惯。

本条消息只需要回复 OK，接下来的消息我将会给你发送完整内容，收到后请按照上面的规则打印两次翻译结果。
```

---

## 作者简介

我是文森策，一个10年+品牌策划人，AI提示词&提效玩家。

现阶段热衷于AI辅助内容营销探索，欢迎交流分享AI辅助创作思考/技法/实用工作流，一起用AI打开更多创作可能。

⭐ **提交反馈与交流合作请直接戳我**👇

![](img/83389be99f2d1e055ab251ac3ba25202.png)

![](img/e67fd1bb0473a1c8d360e169a488fb5d.png)
