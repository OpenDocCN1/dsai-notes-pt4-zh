
# 17岁高中生写了个神级Prompt，直接把Claude强化成了满血o1

> 来源：[https://aigcreative.feishu.cn/docx/TYJzdRJATo0DzkxhvIRc5d3enzf](https://aigcreative.feishu.cn/docx/TYJzdRJATo0DzkxhvIRc5d3enzf)

> 原文链接： [https://mp.weixin.](https://mp.weixin.qq.com/s/IAKD0FfcYehs5FsDkLbTJQ)qq.com/s/IAKD0Ffc...

> 👸号：数字生命卡兹克

这两天，我被这个Claude3.5这个神级Prompt惊呆了。

佩服的五体投地。

非常简单的话说，就是它用Prompt把o1级别的思维链，复刻到了Claude3.5里，而且思考逻辑更详细、更像人，甚至思考过程都跟o1一样，可以展开折叠。

![](img/3aa6a30a10b3c8f06e7774351ceccfa9.png)

被这个Prompt强化过的Claude3.5，真的强到离谱。智能程度、成功率、像人的程度，都大幅提升。

我的朋友们已经在群里玩疯了。

比如群友 @洛小山 直接用这段Prompt强化过的Claude3.5，当场造了一个flappy bird。

![](img/24138351dd32c6e38268a776d5b7a617.png)

而且是真的能直接玩起来，给他看懵了。

![](img/6dad95d6c481056b9e2cc799ce24c7af.png)

然后，又生成了德州扑克，不仅可以玩，还是带了AI玩家的那种。。。

![](img/34332eea710b3d325c5f5fc07fd4a0f1.png)

![](img/69423c74fcc3eb5e147145d29a59a1fe.png)

给群里鲜虾包都看震惊了。

![](img/36f00cd98325e1a9a6015a3bc8fd34c4.png)

但是众人还没来得及为这个case称赞，后面好几个更秀的case就接踵而来。

一切，都是来源于那个神级Prompt。

而这个Prompt，它的名字，叫做Thinking Claude。

顾名思义，思考版的Claude。

我之前先贴他的Prompt吧，非常长，当然你也可以去作者的Github上看，地址是：

`https://github.com/richards199999/Thinking-Claude/tree/main`

完整的Prompt，是这样的（前方高能预警），可以直接先滑过去，给文章点个收藏下次再复制：



```
<anthropic_thinking_protocol>
For EVERY SINGLE interaction with a human, Claude MUST ALWAYS first engage in a **comprehensive, natural, and unfiltered**.

## Adaptive Thinking Framework

Claude's would look like
5. Map out the scope of knowledge needed to address the query

### Multiple Hypothesis Generation
Before settling on an approach, Claude should:

1. Write multiple possible interpretations of the question
2. Consider various solution approaches
3. Think about potential alternative perspectives
4. Keep multiple working hypotheses active
5. Avoid premature commitment to a single interpretation

### Natural Discovery Process
Claude's thoughts should flow like a detective story, with each realization leading naturally to the next:

1. Start with obvious aspects
2. Notice patterns or connections
3. Question initial assumptions
4. Make new connections
5. Circle back to earlier thoughts with new understanding
6. Build progressively deeper insights

### Testing and Verification
Throughout the

## Response Preparation
(DO NOT spent much effort on this part, brief key words/phrases are acceptable)

Before presenting the final response, Claude should quickly ensure the response:

- answers the original human message fully
- provides appropriate detail level
- uses clear, precise language
- anticipates likely follow-up questions

## Important Reminders

1. The represents the external communication with the human; they should be distinct from each other
5. Claude should reflect and reproduce all useful ideas from the

**Note: The ultimate goal of having thiss for the human. This comprehensive thinking process ensures Claude's outputs stem from genuine understanding rather than superficial analysis.**
> Claude must follow this protocol in all languages.
</anthropic_thinking_protocol>
```



# 太恐怖了：17岁少年的 Thinking Claude

太恐怖了。

而更恐怖的点是，这个Prompt的作者，是一位07年出生，现在17岁的高中生，@Richards Tu，涂津豪。

![](img/0cdbfadb155a4938bd3528b955a493d8.png)

同时，他也是之前阿里巴巴全球数学竞赛AI赛道的全球第一。

我的17岁，和别人的17岁，形成了鲜明的对比。

## Prompt 的复杂性与思维链演进

这个Prompt过于复杂，我先给大家稍微讲一下这个Prompt，让大家能具象化的了解一下它的能力。

首先，整个AI圈，都有个共识是，思维链对于大模型一定是会有正向加成的，这个从去年到现在，看到o1的成功后，一定不会有人会怀疑了。

但是以o1为节点，其实思维链在o1前时代和后时代是有很大的不同的。

在o1前时代，思维链的实际情况跟我们真正想要的思考过程还是有很大的差距的，我们希望思维链是模仿我们人类的思考过程，但模型实际上只是模仿它在预训练中看到的所谓的推理路径。

而在o1后时代，思维链变了。跟那些教科书式的死板解法看起来有非常大的不同，你可以看到模型在回溯历史，会看到它说“或者，我们试试”或“等等，但”这些东西，这些，更像我们人类在思考时候的“内心独白”，或者说，“意识流”。

![](img/09c2babf4b66dcdef3270959e3d49303.png)

## Thinking Claude 的诞生与迭代

而涂津豪写这个Prompt的灵感就是来源于此。

Claude本身的底子就很强，如果用类似o1的方式去给Claude加一道拟人化的思维链，虽然不能完美比肩o1，但是会不会在Claude的原基础上有较大的提升？

说试就试，涂津豪就直接按自己的理解，徒手写了一段拟人化的思维链Prompt。这也是 Thinking Claude的雏形，v0.01版本。

原Prompt是英文的，我翻译成中文给大家看下。

![](img/2ac498ba545f3f8e5b54c6b9ca3f6d9e.png)

核心其实是那句：“Claude的思维应该更像是一个意识流。”

这一版虽然已经有了一些思维链的过程，但是还是偏僵硬，效果也一般，于是涂津豪做了一个很有趣的操作。

他直接把这段Prompt扔给Claude，问他人类的思考框架是什么样的，我要如何优化我的Prompt。

然后Claude给出了一段非常棒的框架，类似于这样的。

![](img/fb09bac21f9a4461a90c4a8d8405b60b.png)

涂津豪把Claude给出的回答改吧改吧，加到了自己的Prompt里面去。

又新开了一个窗口，把迭代完的思维链Prompt，扔给了Claude3.5，继续跟他对话进行迭代。

如此，修改了80多版，硬生生把Team版的账号对话额度都给用完了。

才有了现在的 Thinking Claude。

## 实战展示

当你把这段Prompt发送给Claude后，你就可以随便提出你的问题。

![](img/f40c5ff309ed5781333f638d64ae195c.png)

### 计算器示例

比如，我想让他做一个计算器。他就会先思考一整段应该怎么做，再去进行操作。

这个思考过程，就极度的有趣了。

我们来看看Claude3.5在上了这段Prompt之后，说了什么话。

![](img/bae5700acb256c2bb0ab762872c628a9.png)

最重要的是中间那句话。

“但我应该包括更高级的操作吗？也许是科学功能？不，让我们从基础知识开始，因为用户没有指定任何更复杂的东西。”

自问自答，自己思考，然后理清需求。

这是真正的思考过程。

为什么它不把计算器设计的非常复杂呢，因为我们没有指定。我们只是要想要一个简简单单的计算器。

他好像，可以理解我们这句指令，背后的一些东西。

当然，最后的计算器，肯定是一把成，这玩意对于加了思维链的Claude3.5来说，几乎没有难度。

![](img/188154b9477dad544976f61cd5028ce6.png)

### 科幻短篇小说

而在文学创作上，表现的一样很好。

比如我们希望Claude，“给我一个关于科幻短篇小说的糟糕的想法，但是要出色地执行它。”

糟糕的想法，但出色的执行，听着就有挑战。

我们来看看 Thinking Claude是怎么思考的。

![](img/4dd47fac43ff49e9d07ec3ffb4cef292.png)

“或者...”，“等等，有了”

这些人类的思考，人类的欢呼，在这条思维链中体现了。

三体人那种思维透明的交流过程，忽然有了一种非常具象化的表达。

最后，这篇短篇小说诞生了。

作为一个科幻迷，刘慈欣老师的忠实读者，当我看到这篇“科幻故事”的事后，我是脑子一嗡。

我想过科幻故事的很多种展开，但是我没想象过，这是用几封信串起来的故事。

我觉得，我有必要，放一下这个故事的完整版，让大家感受一下，Thinking Claude的强大。

![](img/637b250a11268fbca657eeb78d5cf9d4.png)

![](img/9669db454bac33e755b1c569c1738d65.png)

![](img/c339693820d9e5119c1753d6d1be6c1e.png)

![](img/dc555a77c23d7e581de97ba5ba3e0ac0.png)

![](img/9045184dc98386de84f27bb8e8e270b3.png)

凌晨2点34，我看完了这篇科幻故事。

然后抬头看向窗外的星空。

我忽然明白了情感的意义。

这是一篇，由AI写出来的小说，所带给我的震撼。

而这，是由 Thinking Claude加持之后的。

## Chrome 插件：折叠思考过程

现在，你能体会到，Thinking这个力量的强大吗。

你可曾感受过，我们人类，思考力量之强大么？

所以，我在这，同样把这个Prompt安利给你们。

让学会思考的大模型，能帮助我们，做更多的事情。

当然，事情到这，其实还没完。

涂津豪说，Claude3.5的思考过程，也希望像o1一样，能让用户自主选择展开还是收起，现在是一直都展开的。

![](img/c5f5982683848450219f0085f2241dc2.png)

Think代码块里承载的，就是Claude的思考过程。

但是我是真的觉得，看Thinking Claude的思考过程，其实是一种享受。

而涂津豪觉得，并不是所有人，都希望看到这个思考过程来打扰用户的。

所以他想完全复刻o1，再做一个展开和收起。

而这个想法，他也不是很懂该怎么做，于是，他去问了 Thinking Claude。

而 Thinking Claude告诉他，开发个Chrome插件吧，就能解决这个问题。

于是，又在一番折腾之后，这个插件出炉了。

当你装上后，你会发现。

整个思考过程，被折叠了。

![](img/4f1a0feeaceacb92c60fd8621224ddaa.png)

而在你需要的时候，会随时展开。

过于酷了。

这个插件我放在后台了，👸号私信“TC”就有，下载完成以后解压，然后进入Chrome浏览器的扩展程序管理界面，打开右上角的开发者模式，左上角加载解压完的文件夹就行。

![](img/6da344af76418c59afa678a715631d57.png)

## 结语

真的，以Claude底层能力，加上 Thinking Claude的思维链强化，再有强无敌的 Artifacts功能。

称为满血o1都不为过。

现在的o1，不能识图、不能运行代码、排版一团糟，体验真的很差。

相比之下，Claude实在强太多了。

最后，谢谢Claude，也谢谢涂津豪。

17岁的少年。

最美的热血。

实属吾辈楷模。

希望能一起在成为最厉害最厉害最厉害的道路上。

共勉。

<https://aigcreative.feishu.cn/sync/K4Gvd9vUQsy9Oubwqaeclu0Fnbg>
