
# Claude Sonnet4.6编程追平Opus了，价格只要1/5，老金算了笔账

> 来源：[https://my.feishu.cn/docx/PnaMdLyuVoqfMMxolGhcqT7AnWb](https://my.feishu.cn/docx/PnaMdLyuVoqfMMxolGhcqT7AnWb)

![](img/bc8e81fba262d8598fbabe635caeb68f.png)

OSWorld 72.5%——昨晚（2月17日）刷到这个数据的时候，老金我反复确认了三遍。

Anthropic发布了Claude Sonnet 4.6，编程能力首次追平Opus，价格却只要1/5。

## 编程能力：Sonnet首次超越Opus

![](img/570b4a2db1dfa57f572ca8b11742f875.png)

先看两组关键数据。

SWE-bench Verified：79.6%。

OSWorld-Verified：72.5%。

OSWorld是测试AI操作电脑能力的benchmark。

72.5%这个分数，超越了Sonnet 4.5的61.4%，甚至超越了Opus 4.5的66.3%。

这是Sonnet系列首次在编程能力上超越Opus。

用户反馈也印证了这一点。

1、比Sonnet 4.5 preferred 70%

2、比Opus 4.5 preferred 59%

超过一半的开发者觉得Sonnet 4.6比Opus 4.5还好用。

![](img/ea7eb72d7489d36eaa184d2f0b4d2f65.png)

根据官方说明，核心改进有三条。

### 1、先读后写

之前的版本拿到需求就直接写代码。

Sonnet 4.6会先完整阅读上下文，理解整个代码库再动手。

### 2、逻辑精简

之前AI写代码经常重复逻辑。

Sonnet 4.6会主动合并重复代码，不搞冗余。

### 3、少吹牛多干活

之前的Opus 4.5经常"假装成功"。

Sonnet 4.6减少了幻觉，更靠谱。

GitHub的VP产品负责人在推特上确认：

"Sonnet 4.6在复杂代码修复方面表现出色，尤其是需要搜索整个代码库的场景。"

## 价格：只有Opus的1/5

这是老金我觉得最狠的地方。

Claude Sonnet 4.6：$3 input / $15 output。

Claude Opus 4.6：$15 input / $75 output。

输入便宜5倍，输出便宜5倍。

性能追平Opus，价格只要1/5。

老金我算了笔账。

假设用Opus 4.6跑代码审查，每小时消耗50万tokens。

用Opus：每小时7.5美元，用Sonnet：每小时1.5美元。

一年省下来：2万美元以上。

对于企业用户来说，这就是白赚的利润。

Anthropic官方也说了：

"以前需要用Opus的性能，现在Sonnet就能给你。"

## 100万token上下文窗口

Sonnet 4.6还带来了一个测试版功能—— 100万token上下文窗口。

100万token能干嘛？

1、一次性加载整个代码库

2、一次性分析所有历史需求文档

3、一次性读完几百篇论文

之前128K token已经很强了，现在直接提升到100万，8倍的差距。

而且不仅能读这么多，还能在整个上下文范围内有效推理。

这对于需要处理大型项目的开发者来说，是实实在在的能力提升。

## 计算机使用能力提升近5倍

16个月前Anthropic刚推出计算机使用能力时，OSWorld分数是14.9%。

现在Sonnet 4.6达到了72.5%。

16个月，从14.9%到72.5%，提升近5倍。

![](img/e4a1ac36aef6fb932631758de330e8c4.png)

根据官方演示，现在Sonnet 4.6可以帮你做这些事。

1、自动操作Excel表格

2、帮你填写网页表单

3、跨应用协调工作流

比如你说"帮我看看日历，然后给今天没空的人发邮件说改天"。

Sonnet 4.6能自己理解上下文，完成一系列操作。

## 怎么选

老金我直接说结论。

### 日常开发任务：选Sonnet 4.6。

1、价格便宜5倍

2、编程能力追平甚至超越Opus

3、响应更快

### 复杂推理任务：仍选Opus 4.6。

1、最深层的推理能力

2、多Agent协作

3、需要极致准确性的场景

Anthropic官方也承认，Opus仍然是深度推理的首选。

但对于80%的日常开发任务，Sonnet 4.6完全够用了。

## 怎么用

现在Sonnet 4.6已经上线。

免费用户：`claude.ai`默认就是Sonnet 4.6。

Pro用户：`claude.ai`默认也是Sonnet 4.6。

开发者可以通过Claude API、Claude Code、Claude Cowork直接使用。

如果之前用Opus跑日常任务，现在可以直接换成Sonnet 4.6。

省下来的钱，够买好几年咖啡了。

## 老金建议

这次Sonnet 4.6的更新，老金我给90分。

扣10分是因为1M token还是测试版，暂时用不了。

但就目前的能力来说，这已经是Sonnet系列最强版本了。

价格不变，能力直接翻倍。

个人开发者可以省一大笔钱。

企业用户省下来的可就是真金白银了。

根据benchmark数据和用户反馈，这次升级值得认真考虑。

## 参考来源

Anthropic官方发布：`anthropic.com/news/claude-sonnet-4-6`

TechCrunch报道：`techcrunch.com/2026/02/17/anthropic-releases-sonnet-4-6`

MacRumors评测：`macrumors.com/2026/02/17/anthropic-releases-claude-sonnet-4-6`

VentureBeat分析：`venturebeat.com/technology/anthropics-sonnet-4-6-matches-flagship-ai-performance-at-one-fifth-the-cost`

IT Pro报道：`itpro.com/technology/artificial-intelligence/anthropic-promises-opus-level-reasoning-claude-sonnet-4-6-model-at-lower-cost`

`https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc`
