
# AI编程大战白热化：Claude Opus 4.6和GPT-5.3-Codex同一天发布，谁才是真正的王者？

> 来源：[https://my.feishu.cn/docx/ZxOldmgkUoO0iUxOdZGcGkTRn5b](https://my.feishu.cn/docx/ZxOldmgkUoO0iUxOdZGcGkTRn5b)

今天凌晨，AI圈炸了。

`Anthropic`和`OpenAI`在同一天发布了各自的王牌模型。

`Claude Opus 4.6` vs `GPT-5.3-Codex`。

正面硬刚，火药味十足。

老金我熬夜测了两个模型，今天给你们做个全面对比。

`Claude Code`和`Codex Cli`中均已上线。

![](img/6db130a801058717caa8d59a74c6197b.png)

![](img/997bd177b64876d3490eb7821a609c81.png)

## 先说结论：各有所长

`Claude Opus 4.6`：100万`token`上下文、`Agent Teams`、长上下文能力碾压

`GPT-5.3-Codex`：25%更快、自我构建、超越编码

两家都说自己是"最强"。

但"最强"这个词，得看你用在哪。

## Claude Opus 4.6：100万token上下文

这是`Opus 4.6`最炸裂的升级。

之前`Opus 4.5`的上下文是20万`token`。

现在直接翻了5倍，到100万`token`。

老金我给你算笔账：

100万`token`大概等于75万个英文单词。

换成中文，差不多是50万字。

一本《三体》全集才40万字。

**这意味着什么？**

你可以把整个项目的代码库一次性喂给它。

不用再担心"上下文不够用"的问题。

`Claude`能同时看到你项目里的所有文件，理解整体架构。

## GPT-5.3-Codex：AI自己造AI

再来看个`GPT-5.3-Codex`最让人震惊的地方。

OpenAI官方说法：

"`GPT-5.3-Codex`是我们第一个帮助构建自己的模型。"

![](img/55ed042827ba48b4733812fcc95df669.png)

什么意思？

`Codex`团队用`Codex`来：

+ 调试自己的训练过程

+ 管理自己的部署

+ 诊断测试结果和评估

老金我不知道该兴奋还是该害怕。

AI开始自己造AI了。

这个反馈循环一旦跑起来，进化速度会越来越快。

## 长上下文能力：Claude完胜

这是`Opus 4.6`最硬核的技术突破。

**什么是`Context Rot`？**

就是AI模型在处理长文本时，性能会随着上下文变长而下降。

简单说：喂的内容越多，AI越容易"犯迷糊"。

Anthropic给出了一组数据：

在`MRCR v2`测试中（专门测AI在海量文本里找信息的能力）：

`Opus 4.6`得分76%。

`Sonnet 4.5`只有18.5%。

![](img/d78009bb73ff277316d599a3784f613e.png)

76% vs 18.5%，这不是量变，是质变。

老金我翻译一下：以前你给AI喂10万字的文档，它可能只能记住开头和结尾。

现在`Opus 4.6`能真正"读懂"整个文档，不会漏掉中间的关键信息。

## 速度对比：GPT-5.3更快

OpenAI说`GPT-5.3-Codex`比`GPT-5.2`版本快了25%。

![](img/c4b3a94fa46b8b1534a633e2db42e760.png)

这个提升很实在。

写代码的时候，等待时间少了，心流状态更容易保持。

![](img/ec37cc33eee57d83479b7df1ac326138.png)

`Claude Opus 4.6`的速度和`Opus 4.5`差不多。

考虑到上下文翻了5倍，能保持同样的速度已经不错了。

## Agent Teams vs 自我构建

`Claude`的`Agent Teams`：

让多个`Claude`同时工作，各自负责不同的任务，然后自动协调。

一个负责前端，一个负责后端，一个负责测试，一个负责文档。

四个`Claude`同时开工，效率翻倍。

启用方式：`CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1`

![](img/390301ac921ec0a922fb32a620cd3b3d.png)

**`GPT-5.3`的自我构建**：

AI参与自己的开发过程。

这意味着OpenAI的迭代速度会越来越快。

今天的`5.3`帮助构建明天的`5.4`，`5.4`帮助构建`5.5`...

两个方向都很有意思。

`Claude`走的是"团队协作"路线。

OpenAI走的是"自我进化"路线。

它还放了拿`Codex 5.3`做的两款游戏，你别说，你还真别说。

让处于游戏研发的我，背后发起一丝丝凉意，太快了，进步太快了。

赛车游戏，包含不同的赛车手，八张地图，甚至还有可以用空格键使用的道具。

![](img/21e086b4ba355c8a335972ef4043a164.png)

潜水游戏，你将探索各种珊瑚礁，收集所有鱼类以完成你的鱼类图鉴，同时还要管理氧气、压力和应对各种危险

![](img/441b7222e494a8ba94902fb4cd26d07c.png)

## 跑分对比

**`Claude Opus 4.6`**：

+ `Terminal-Bench 2.0`：最高分

+ `GDPval-AA`：比`GPT-5.2`高144 Elo点

+ `Humanity's Last Exam`：第一

+ `BrowseComp`：第一

![](img/d0ba5e303893bb7e1835a432804ff99a.png)

**`GPT-5.3-Codex`**：

+ 自我构建能力：首个参与自身开发的AI模型

+ 25%速度提升（相比`GPT-5.2`）

+ 被标记为"高能力"网络安全任务

+ 具体跑分数据尚未公布

![](img/d83e3df0d1bfb403db948d79670dea59.png)

两家都说自己是第一。

跑分这东西，看看就好。

实际用起来怎么样，还得自己测。

## 超越编码：两家都在扩展边界

**`Claude`的扩展**：

+ `Claude in Excel`：直接在`Excel`里调用`Claude`

+ `Claude in PowerPoint`：直接在`PPT`里做演示文稿

+ `128k输出tokens`：一次性生成更长的内容

**`GPT-5.3`的扩展**：

OpenAI说："`GPT-5.3-Codex`从一个能写代码和审查代码的`Agent`，变成了一个几乎能做开发者和专业人士在电脑上能做的任何事情的`Agent`。"

包括做幻灯片、电子表格等。

两家都在从"编程工具"向"通用工作助手"进化。

## 定价对比

**`Claude Opus 4.6`**：

+ 输入：`5美元/百万token`

+ 输出：`25美元/百万token`

+ 超过`20万token`的长上下文：10美元/37.5美元

**`GPT-5.3-Codex`**：

+ 目前只在付费ChatGPT计划中可用

+ API访问计划中，预计会有25%更快的交互

两家定价策略不同。

`Claude`走API路线，按`token`收费。

OpenAI走订阅路线，包含在ChatGPT Plus/Pro里。

## 市场格局

**`Anthropic`**：

+ 44%的企业在生产环境中使用

+ `Claude Code` 11月突破10亿美元收入

+ 正在谈200亿美元融资，估值3500亿美元

**`OpenAI`**：

+ 三天前刚发布`Codex`桌面应用

+ `GPT-5.3-Codex`是对`Claude Code`的直接回应

+ 仍然是市场领导者

两家现在是贴身肉搏的状态。

OpenAI出一招，`Anthropic`就接一招。

对我们用户来说，这是好事——竞争越激烈，产品越好用。

## 老金我的选择建议

选`Claude Opus 4.6`如果你：

+ 需要处理大型代码库（100万`token`上下文）

+ 需要长上下文能力（不会"忘记"前面的内容）

+ 想用`Agent Teams`做并行开发

+ 需要在`Excel`/`PowerPoint`里用AI

选`GPT-5.3-Codex`如果你：

+ 已经是ChatGPT Plus/Pro用户

+ 需要网络安全相关的高能力任务

+ 喜欢OpenAI的生态系统

**老金我的实际做法**：两个都用。

不同任务用不同工具。

大型项目、需要长上下文的用`Claude`。

快速迭代、简单任务用`GPT`。

## 最后说一句

2026年2月5日，会被记住。

这一天，两大AI巨头同时发布了各自的王牌模型。

AI编程工具的竞争进入了白热化阶段。

`Claude Code`、`Cursor`、`Windsurf`、`OpenAI Codex`，各家都在拼命卷。

对我们用户来说，这是最好的时代。

你们觉得呢？`Claude`还是`GPT`？

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>
