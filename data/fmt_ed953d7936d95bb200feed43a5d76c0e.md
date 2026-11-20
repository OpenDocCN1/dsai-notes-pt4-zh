
# 如何利用人工智能模型编写人物角色Prompt

> 来源：[https://agijuejin.feishu.cn/docx/Znitdb7b6op1mcxhpEYcUv1znTb](https://agijuejin.feishu.cn/docx/Znitdb7b6op1mcxhpEYcUv1znTb)

> 作者：德里克文

> 原文： [https://zhuanlan.zhihu.com/p/652018197](https://zhuanlan.zhihu.com/p/652018197)

昨晚很荣幸受到百度文心一言APP运营团队的邀请，我作为特邀演讲嘉宾给众多团队的成员分享了我利用文心一言语言大模型，创作商纣王Prompt的经验和创作思路，在这里我也将我的演讲PPT和大致的思路想法分享给大家。

我的几个人物Prompt可以在百度文心一言APP灵感中心的角色版块找到【分别为商纣王、樱木花道、流川枫】，欢迎小伙伴去体验哦！

![](img/7e531d204ad1f7ef79ef670b01d4c362.png)

## 创建人设型Prompt需要掌握什么?

### 1. 语言大模型原理简述

首先我们需要知道，语言大模型是一种基于深度学习技术的数学模型，用于对语言的概率分布进行建模。它们在大量的文本数据上进行训练，可以执行广泛的任务，包括文本总结、翻译、情感分析等。

语言大模型的基本原理是使用神经网络对语言进行建模。它们通常采用循环神经网络（RNN）或长短时记忆网络（LSTM）等深度学习架构进行建模。这些神经网络可以处理变长的序列数据，因此非常适合用于处理自然语言。

在训练过程中，语言大模型会使用大量的文本数据来训练神经网络，从而学习到语言的概率分布。这样，模型就可以根据上下文信息预测下一个单词或句子的概率分布，从而生成新的文本。

概率分布是一个基础概念，所以我们需要基于这个基础概念去进行思考。

![](img/0552568aec947ea0b7b47d010f3feaa2.png)

### 2. 提示词(Prompt)结构

有几个基础原则,满足这些结构：

背景: 在背景信息中，可以加入更多的细节信息，以帮助模型更好地理解问题。

输入数据: 对于输入数据，可以提供更多的信息，以使模型更好地理解问题。

指令:指令应该明确、简洁、具体。它应该告诉模型需要完成的任务，

输出指示: 输出指示应该告诉模型输出的类型或格式

## 编写商纣王的创意来源

![](img/0995e9c5c59b18595eb5e299d0a6cb16.png)

![](img/9e0cb1e8c50fa1fff18fcea1d747e484.png)

## 商纣王的提示词Prompt

![](img/8d2333e16163857104c2bfced007bb88.png)

![](img/fa11966c7b20a15709f36aa7ff71ac08.png)

## 关于信息取舍

在实际应用过程中，往往最终模型出现的效果不是想要的，因此需要进行不断的测试和删减。

![](img/688437b015290e9bad2d16047d284913.png)

![](img/1f89d25126e3bfe363499bcc5f1fceca.png)

![](img/380c8b5882f15c14513e9f4d201f5b4d.png)

## 案例解析

在会上我详细展开说了每一项的注意点和归纳的几个主要内容，同时也用案例来进行补充说明。

![](img/e8a3f0c7d7a2f15a7e8ecef93c0deabe.png)

![](img/e92f192c9abb9ef03cc8ca3ec817a402.png)

![](img/6fe8264a466b3227dcf4ff72ef3e5f63.png)

![](img/33dbb9719c43adedb3e6173eabbd3ce5.png)

![](img/d3839cd8cfd5dc6bc6f8d28e5f18744a.png)

![](img/f2a57a15511b79a25570cba9618077e5.png)

![](img/85608e192719832a52941dc7f190e72e.png)

![](img/565ebe8d793ae0731466a0a734275548.png)

![](img/9eda02e3605252c14977638afd543420.png)

![](img/d0dfdf62c0914021e461068790d5f0fe.png)

![](img/950f7c51e3c62205c625db778ecf9781.png)

![](img/ffa5d645d52a3985835ed8618cda8277.png)

最后进行了调试过程中的复盘，发现调试中错误的地方怎么进行调整。

![](img/a3a161ccebefc7c020135194881b1f1a.png)

![](img/17857483dce7fd5371d738804a13b323.png)

![](img/4744a9307448c6c62c81b6f9755325be.png)

## 对产品方的建议

在会后的问答环节，我也站在普通用户角度给产品开发方一些建议。

1. 希望后续能够将这些提示词结构化，通过成熟的逻辑文章进行关键字的改动，可以让更多人体会自己捏角色的乐趣

2. 不仅仅是文字的呈现，而应该结合各种效果进行视觉呈现，让这个产品更加有趣。

![](img/0cfdfa0b1eb15de05cce624ff356e19d.png)

![](img/c7c84ab37864dfc49b56c1745c597efa.png)

以上是我在本次分享会的PPT分享的要点，也分享给各位，谢谢

---

我是 [德里克文](https://www.zhihu.com/search?q=%E5%BE%B7%E9%87%8C%E5%85%8B%E6%96%87&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A3052677444%7D)[，一个对AI](https://www.zhihu.com/search?q=%E5%AE%A4%E5%86%85%E8%AE%BE%E8%AE%A1%E5%B8%88&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A3052677444%7D)绘画，人工智能有强烈兴趣，从业多年的室内设计师！如果对我的文章内容感兴趣，请帮忙关注点赞收藏，谢谢！
