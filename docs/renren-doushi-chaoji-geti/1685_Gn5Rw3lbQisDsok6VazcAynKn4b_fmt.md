
# 2023-05-24 LangChain 应用框架思维导图分享

> 来源：[https://xuqiwei1986.feishu.cn/docx/CwQ8dC2pFoFVsOxOuEWcYRuInDf](https://xuqiwei1986.feishu.cn/docx/CwQ8dC2pFoFVsOxOuEWcYRuInDf)

> 🔗 原文链接：https://mp.weixin.qq.com/s/JoHAUSsjm3O75YhxO0jvDw

原创 何文斯 2023-05-24 20:59 发表于 江苏

作为AI相关从业者，想要了解如何开发搭建LLM应用，最直接有效的方式就是阅读`LangChain`的官方文档，里面可以找到大量的LLM应用示例。

![](img/90ef2229746fef619efc3286c0289b1f.png)

推特上的@pwang_szn 整理了完整的`LangChain`学习框架，包括`Prompt`（提示），`Index`（索引），`Model`（模型），`Chain`（链），`Agent`（代理），`Memory`（记忆）六部分内容。

![](img/d3e7781730ae8e51df89c0b9b3bf0e81.png)

![](img/9de7cdec17c15af3c4abd7050362dc52.png)

为了方便学习，我将所有内容做了翻译，并汇总整理到了一张思维导图上：

![](img/3b071fe60607fa5634abd7050362dc52.png)

以下是各个章节拆分整理后的内容：

### `Prompt`（提示）

![](img/28ae28fc30fcd760ff52c244a505ba19.png)

### `Model`（模型）

![](img/74d3986fd069378fb050000de64c5e74.png)

### `Index`（索引）

![](img/d00a188097d7f8f2f73bd32adb4753be.png)

### `Memory`（记忆）

![](img/122a1589fe55d7e2106cea8dc1dd7c64.png)

### `Chain`（链）

![](img/c5ade9c4f23d1651b48428d5c017ca7b.png)

### `Agent`（代理）

![](img/da56ca0f21b9bb337ba8fff929c1320f.png)

看不清是不是？没关系，我已经将以上所有资源都整理好放在百度网盘了，永久有效：`https://pan.baidu.com/s/1sqpYnM44ZE9NC3OjD2yEBg` 密码: fkmv

![](img/8722e6102af4d52040f4907e7b710e3e.png)

最后说下我的理解，`LangChain`提供的是一个连接大模型与应用的中间框架，一方面可以方便应用层实现快速开发，更重要的是提供了应用与LLM之间的“交互思路”。

举例来说，理解`LangChain`中的`Prompt Template`，可以帮助开发者理解提示词工程的相关技术技巧；理解`LangChain`中的`Chain`的实现机制，可以帮助理解如何将大模型与各种外部工具及数据连接。

纵然`LangChain`很好用，但实际到了复杂的应用场景时，无论是`LangChain`还是`LlamaIndex`，这类应用框架所提供的能力时常是无法满足需求的，这其中往往还有大量的工程问题需要解决。

所以，不要被`LangChain`已提供的模块和组件限制了思路，毕竟这个开源项目推出的时间也就半年（2022年10月）。如果以`LangChain`版本发布的速度来观察（几乎每周都有新版本发布），不难看出针对大模型如何应用仍有很大的探索空间。

感谢阅读，欢迎分享
