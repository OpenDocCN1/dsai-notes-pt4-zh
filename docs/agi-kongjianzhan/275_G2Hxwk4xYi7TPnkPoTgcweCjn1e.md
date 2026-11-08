# 六分钟，Langchain 作者到底在红衫 AI 峰会上说了什么？

> 来源：[https://agispace.feishu.cn/docx/LN74dFuLSoWrh0xDVcKcTnQ4nPh](https://agispace.feishu.cn/docx/LN74dFuLSoWrh0xDVcKcTnQ4nPh)

> 作者：特工少女

> 👸号：特工宇宙

> 原文链接：https://mp.weixin.qq.com/s/41ndAw7jFwRJKO0pMVv1dw

![](img/093d98c7f66f073d0252bb1a98c35b8d.png)

[上一期我们分享了吴恩达教授，在红杉 AI ](http://mp.weixin.qq.com/s?__biz=Mzk0NTYzNDQ5NQ==&mid=2247488311&idx=1&sn=7dd2c61032eb1eda7fe35017fe690d25&chksm=c3133ee7f464b7f13d8c6696ad352304c84e70d47996c274bd4ee0abdd0e6ef7bd23476c3278&scene=21#wechat_redirect)峰会的分享内容：Agent > GPT5？吴恩达最新演讲：四种 Agent 设计范式（通俗易懂版），分享后，吴恩达教授介绍了 Harrison 大佬，即 Langchain 的作者。

熟悉 Agent 的朋友应该对 Langchain 不陌生，Langchain 是全球最大的生成式 AI 编排平台，在 Github 上高达 81k+ stars.（当吴恩达问在座的朋友谁使用过 Langchain 时，几乎所有人都举起了手。）

![](img/f3ded45931caf82d2099e65cddb7f378.png)

今天我们就来看看，Harrison 在短短的六分钟里，到底讲了什么？

以下是大佬发言（为了保证可读性，我们在尽量保证原意的情况下，融入了一些我们的理解）👇

![](img/9fb665e48456a70eade2b22c56eda9c5.png)

Langchain 是一个用于构建任何 LLM 应用的开发框架，但大家最常用的，就是用 Langchain 构建 Agent.

我们已经从各种渠道，或多或少理解了什么是 Agent，所以我这里就不详细讲了。

（此时 Harrison 的 PPT 上放了这张经典老图，来自 Lilian Weng 的博客）Agent 使用 LLM 以插件、规划、行动等各种形式和外部世界交互。

![](img/f48a115d260e8f5e9bd3fad8d213e900.png)

近**段时间，开发者花费了大量时间，接**受并证明了 Agent 的生产价值。所以我今天谈论的主题就是 Agent. 包括Agent 的三部分：Planning（规划）、UX（用户体验）、Memory（模型记忆能力）。

![](img/d6db2d1572395a24dd97e272ab2e73e2.png)

首先是 Planning（规划），刚刚吴恩达教授在他的演讲中很好的讲述了这部分内容。

如果你想循环调用 LLM，在每个步骤中，隐含地要求了 LLM 进行推理并规划下一步做什么。不过现在 LLM 模型能力还不够好，无法可靠地做到这一点。所以我们在 Planning 这一步，可以看到大家做的一些基本策略，（屏幕上）有很多论文和 Prompt 的策略，强制 LLM 做一些 Planning，比如要么预先明确每个步骤，要么最后加个 Reflection（反思）来检查是否正确的完成了所有任务。

![](img/4c2e20e2a74d4835416efca9025dff8f.png)

这里有一个有趣的问题是，**在未来，是这些 Prompt 策略**一直存在，还是会被内置到模型 API 中（就像 Sam 说的那样）？这些策略是短期的最优解，还是会长期存在的，Agent 的必要组成？这是个开放性的问题，我没有答案。

另一个类似的东西，就是 Flow Engineering（流程工程），它没有用更好的模型，更好的 Prompt 技巧，而是通过这种类似状态机的东西构建了 LLM 的流程，取得了很好的效果。

![](img/9b3c6709546440d091998f55a0fa4b35.png)

![](img/13a172edc2bc6fc70ebd60a62d25f3f8.png)

接**下来我想聊聊的是 Agent 的用**户体验。我很喜欢 Davin（就是那个之前爆火的 AI 程序员）的用户体验，尤其是 Rewind（倒带，回到之前任意时刻并编辑代码或其所属状态）的功能，以便让它做一个更加明智的决定。

我认为这是一个非常强大的用户体验设计，Langchain 也尝试在做这方面事情。

![](img/764f45f75710823d4d86b9558a5cb301.png)

![](img/ef12dec4c6efd6c6e0649a26aa4bdff4.png)

最后我想分享的是关于模型记忆，举个例子，我用 AI 以特定风格写一篇📕文章，你可以看到，我只是用简单的自然语言来进行纠错，然后我下次回到这个应用时，它会记住我的纠错，记住我想要的风格，可以快速的继续使用它，第三次回到应用依然能记住之前我所有的要求，我把这种能力归为 Procedural Memory（程序性记忆）。

![](img/bf8e09c6335bbba5647bad2498603bc6.png)

同时，另一种记忆叫做 Personalized Memory（个性化记忆），即记住所有关于（使用该 AI 的）用户的所有事件，可能不一定会带来更正确的结果，但可以让用户的整体体验更佳。

比如我在尝试在构建和探索模型记忆能力时，做的这个案例。我们做了一个 AI 日程伴侣，你可以看到，我跟 ta 说，我去上了新东方烹饪学校，它会记得我以前跟他说过的，我喜欢吃西红柿炒番茄

![](img/ede99862e0e25687981d604752c5e5ad.png)

**我认为无论是程序性记忆，还是个性化**记忆，都对 Agent 的发展非常重要。

以上就是全部内容，如若有任何疑问或建议，欢迎来我们 Agent 爱好者社区交流。

![](img/28c3f52d50b4f3e92b64fb07f24a522b.png)

![](img/e6febc9c60229732f7f4de2bc72d7f2b.png)