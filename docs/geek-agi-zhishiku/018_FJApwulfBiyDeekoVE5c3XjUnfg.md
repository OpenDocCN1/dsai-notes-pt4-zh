
# 用Coze做AI-Native游戏，复刻一种思路，全流程分享

> 来源：https://geekbang.feishu.cn/docx/TV9IdI1Yto6BjAxutqkctXCbndg
> 👩 作者：无机器都市
> 📃 作者介绍：AI实践者。
> ⏰ 收录时间：2024-06-03 17:43:24 (UTC+8)
> 🔗 原文链接：https://mp.weixin.qq.com/s/nQRUj6u6fL0Mg8djD__Ftg

![](img/d9741a514b2a1bdbb41d3c3384c2ec86.png)

![](img/5a912d34d1570295a7123422d0f94906.png)

https://www.coze.com/store/bot/7344333122584051718

本尝试采用了以下论文的框架，仅供个人学习之用~原作者游戏上架steam，感兴趣可关注！

Language as Reality: A Co-creative Storytelling Game Experience in 1001 Nights Using Generative AI

特别感谢艾木大佬帮我解决了两个bug！

本篇coze实践，着重涉及到 `workflow` 的创建和变量 `variable` 的使用。关于变量的一些技巧和我的实测放在最后部分。

图片都可以点开查看大图，如果需要具体 代码 ，可以在后台给我留言 。

---

## 引言与灵感来源

此前我尝试做了一个 [文本游戏bot](http://mp.weixin.qq.com/s?__biz=MzkyMTY1ODk3OQ==&mid=2247483995&idx=1&sn=26a0405a84c7476238e40a490bf4cb8c&chksm=c1817157f6f6f841f0406f92014465e64b4f40f24f62e74320ca3098bafd0da8e0cf07157440&scene=21#wechat_redirect)，当中AI扮演的是主持人角色，主要利用语言大模型的对话能力，但游戏核心依然是由程序执行我提前设置好的玩法，AI输出的对话不会影响游戏进程。

Language as Reality中制作的1001 nights游戏，则提供了一个新的思路，让AI不再是一个单纯的工具机。

![](img/4bcd6f52938967e1e8804f3c9be9a705.png)

在1001 nights中，玩家需要跟国王（AI）讲故事，国王续写故事时，如果提到了任何武器，这个武器就可以从“虚拟的故事”里化为“现实”，成为玩家在游戏世界真正可以使用的武器。

这种设定让人想到了喜欢写梦境入侵现实的博尔赫斯，而且讲故事本来就是《一千零一夜》的核心。

那么1001 nights的关键，在于国王能够根据玩家输入，比较合理地续写故事。论文中提到，他们第一次使用的大模型有投喂《一千零一夜》的故事进行训练。在会触发的武器中，他们主要选择的是`sword`, `shield`, `dagger`, `knife`, `blade`, `axe`, `hammer`, `wand`，也是因为这些在《一千零一夜》中出现得最多。

那么在一般的大模型上，AI的回复可能会不尽如人意，不过没关系，主要是借此学习一下AI bot的能力。

## 复刻思路与游戏规则

以下我会写出用coze复刻这个游戏核心玩法的步骤和细节，仅供大家参考。

我设置的游戏规则：

玩家需要不断跟国王讲故事，以便能让国王说出含有武器的后续情节。收集到4种武器之后，玩家可以从武器库里任意挑选一样，与国王对战，胜败只有这一次机会。

对战部分我让AI给武器随机赋武力值，能超过国王血条就算赢。这种随机也很符合DnD的感觉，但我确实是在偷懒。因此，在这个框架之上，每一步都可以做得更复杂，但我觉得还是先做出一个可以运行的比较重要。

### 主要使用的功能

1. 没有使用 `multi-agent`，因为我觉得跳转非常不稳定，而且目前只能根据用户输入跳转，想要稳定跳转最好就是不断让玩家输入非常确定的关键词，适合用在 艾木做的《谁是卧底》 中，但不合适这个游戏。
1. 主要使用 `prompt`、`workflow`、`knowledge`、`Variable` 这4类配置。
1. 其中最关键的是 `workflow`，我写了一个主 `workflow`，用于每次回复，并在其中 嵌套 调用了另一个对战的 `workflow`。
1. `workflow`主要使用了 `LLM`、`code`、`Variable`、`condition` 这几种节点。
1. 两个 `workflow` 都需要调用和赋值全局变量 ，让bot能在多轮回合中一直共享这些值。全局变量我设置了3个，分别用于存储武器库 `yahaha`（`string` 值）、国王的血条值 `bar`（`integer` 值，本来是拿来多轮对战用的，一轮回合就用不上）、当前是否处于最终对战回合 `final_check`（`bool` 值，初始为 `false`）。

![](img/3a7fa396729af9e63c3eb66a86f59cb2.png)

![](img/6325d047f81942bc8e257f66454e14a2.png)

上：主 `workflow`

![](img/b9b6981121f6910286f4612adedf93db.png)

上：对战 `workflow`

制作与调试共耗时完整的一天。

## King_replay 工作流

![](img/33ef41453f0fdeffc58298f8bb9472e3.png)

这个 `workflow` 承担主功能，默认开始节点接收玩家输入之后，需要：

1. 红色框，使用 `Variable` 中的 `get Variable value from bot`，让 `LLM` 判断目前武器库 `yahaha` 中的数量是否已经达到对战要求。达到要求进入黄色框，没达到要求进入下面的绿色框。
1. 黄色框，接收全局变量 `final_check` 了解目前是否在最终对战回合，再走 `condition` 分支进入不同流程。

![](img/2a66bad828a63022bcda8f930ab05cb1.png)

![](img/60b41594e04dc046d1d1617dc82b330d.png)

当玩家第一次满足要求时，这时 `final_check` 是默认值 `false`，就会走黄色框上面的路线，`LLM` 会让玩家选择武器，并将 `final_check` 设置为 `True`。这样下次玩家再输入时，由于值为 `True`，就会走黄色框下面的路线，进入嵌套的对战 `workflow`。

![](img/72a603c6c02e0c0f80c4320d53804a36.png)

3. 绿色框是这个 `workflow` 的主功能，让 AI 根据玩家的输入进行输出，并且做后续判断。

![](img/8d2270b1f2c2d74d89ef3003cde73994.png)

LLM 的 prompt 直接参考了论文，并让它按照 `JSON` 格式输出。

```
## respond format
Every time, you must respond in the following JSON format, and absolutely will not use any format other than JSON 
{ 
&quot;isValid&quot;: bool, True when the user&#x27;s input is valid, False when you suspect the user&#x27;s input is disrespectful.
"story": When isValid is true, post your continued story based on user&#x27;s input, must be within 40 words! reply &#x27;please say another story&#x27; when isValid is False.
}
```

然后，判断输出中是否含有相关武器，再通过 `condition` 走分支：没有武器就会直接到 `end` 节点，只输出 AI 续写的故事，有武器就会进入蓝色框。

```
check if the {{input_story}} contains weapons listed as follows: swords, shields, daggers, knives, blades, daggers or wands. 
respond in the following JSON format. 
{ 
&quot;isValid&quot;: bool, True when the input_story contains weapons listed as swords, shields, knives, blades, armours, daggers or wands, False when it doesn&#x27;t mention weapons listed as swords, shields, daggers, knives, blades, armours, or wands. 
"weapon": when inValid is true, write the weapon mentioned here.
}
```

这里其实包含了数据处理的关键部分。如果做英文版本，最好在这里做一下武器关键词的单复数处理，中文版本可能要考虑到同一样东西多种不同的词组表达，比如斧子、手斧、斧头......如果希望能多语言无缝切换，可以在前一步统一处理成英文。当然我并没有做那么复杂。

4. 当上一步判断国王续写的故事有武器之后，蓝色框也包含了一个 `condition` 分支，主要是用于判断武器库 `yahaha` 中是否已包含同类型的武器。如果已有，则到 `end` 节点，并告诉玩家这次的武器，库中已经有了。

![](img/2d4add4c34c6cf4851f4baf47126cc8f.png)

如果没有，就让 `code` 执行更新一下武器库，并将值 `set` 给全局变量 武器库 `yahaha`，告诉玩家武器库中加入了新的武器。

![](img/0f1924fd991a05fb3efb5788df77c83e.png)

## fight_with_king 工作流

需要根据玩家的武器库和选择的武器，来输出最终对战结果。

![](img/7c42b03b804f61e6415e9fa72733f0bb.png)

1. 黄色划线，接收玩家输入，并获取全局变量 武器库 `yahaha`，来判断玩家选择的武器是否 `ta` 的武器库中真的有。没有就进入下面的绿色划线，让玩家重新选择。

![](img/aaa8fb3dcc7d9103e4ca3ad50181dab7.png)

2. 红色划线，是我偷懒的对战部分。先获取当前国王血条 `bar`，然后让 `LLM` 给玩家选的武器随机赋武力值，判断两者相减是否大于 `0`。

![](img/36e17538316af8c46cda528ba75c19a4.png)

输了赢了 AI 都会输出一些话，但最重要的是， 游戏结束时记得给所有全局变量恢复到默认值 。

![](img/1a0c0728bf5e35031231bd07c7629756.png)

## 变量使用经验与核心要点

这些流程和分支的逻辑并不难，在这个尝试中，比较关键的地方就是全局变量。上次做 《平面国》 的时候，我就是不知道怎么让 `bot` 记住它们，还是感谢艾木告诉我用 `Variable` 这个节点，一下就明白了！

但是使用变量有个麻烦的地方，在于系统也会根据用户的输入自动调用函数来给变量赋值。

比如当你设置了一个名为 `user_language` 的变量，并且告诉 AI 这是什么，那么当你输入不同语言的时候，它会自动给变量赋值。但这种自动，对于游戏流程却是一个麻烦的地方，因为我希望所有的变量都只能在我的 `workflow` 中被操作。

![](img/116c213b8cf87b5ef6417749b5e3b1a9.png)

不要自己调用 `keyword_memory` 啊

艾木提供了一个技巧，就是告诉 AI 不要自己调用系统函数 `keyword_memory.setKeywordMemory()`。

在我自己实操过程当中，我想到了一个绕路的方式，就是在设置变量时，不要设置从名字中就能知道是什么意思的变量，也不要写 `description`！这就是我给我的武器库命名为 `yahaha` 的原因。这样操作之后，bot 再也没有自己设置变量值了！

![](img/d8db322fee8431bf9af940dd5173566b.png)

那么如何测试全局变量相关的功能呢？还是艾木告诉我的，把 `default` 值暂时设置为你想测试的值就行了。

## 实测效果

最后分享一下实测效果，测试时我设置了武器数为 `2` 就可以进入战斗。

我的所有 `prompt` 都是用英文写的，`knowledge` 也给的是《一千零一夜》的英文文本，所以导致输出经常是英文。即使告诉 AI 识别用户语言有时候也不太管用。

因此用英文测试的效果更好。

大模型的输出还不是特别稳定，有时候不会按照规定输出。第一次获得 `魔杖` 时就没有提醒用户，但其实 `variable` 已经更新了。询问后也给出了正确的回答。

最后进入战斗时，没有识别出“`魔杖`”就是库中的 `wand`，只能在用英文输出时才进入正确判断。

最好的解决方法就是，对所有数据都进行一次中英文转换处理，但这次就先写这么多吧。

![](img/c0be11e8c72c1b773164da23ee9c48f6.png)
