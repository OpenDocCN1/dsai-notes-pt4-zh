
# 一泽Eze:我深入拆解了 Claude 曝光的内置提示词，不愧是官方最佳实践

> 来源：[`https://agijuejin.feishu.cn/docx/EKikdOQEno0rd6xelIBcEyjenBe`](https://agijuejin.feishu.cn/docx/EKikdOQEno0rd6xelIBcEyjenBe)

![](img/fbede16fc8b0468caf571540963ba6c8.png)

幸好，今天下班路上刷了一眼 X 。

看到 Anthropic 的 DevRel（开发者关系）老哥 @Alex Albert 前天发了篇推。

还好，错过就亏大发了……

![](img/9dee0f6e31d30c3fe1bbe9670b26bd1d.png)

这是全球顶级大模型厂商，官方下场，用他们的最佳实践，教我们"什么才是优秀到爆炸的提示词"。

很强，又学到很多。

简直覆盖了提示词设计的方方面面。

全方位、无死角地为 Prompt Engineer，开拓提示词设计思维。

**待我细细与你们拆解分析。**

> 想直接围观原文的，戳这里：`https://docs.anthropic.com/en/release-notes/system-prompts#july-12th-2024`

## 阅读指南

由于原始 Prompt 没有按语义功能进行分段。

为了更好的可读性，我根据原文顺序以及内容主旨，进行了拆解与翻译，方便大家量子速读、快速理解。

如果不方便仔细阅读，也可以直接往下拉，下面有更简洁、贴心的思维导图，以及深度解析。

希望能够帮助你真正理解提示词的设计理念。

## 原始提示词的拆解与翻译

> 太长不看，可以直接往下拉
