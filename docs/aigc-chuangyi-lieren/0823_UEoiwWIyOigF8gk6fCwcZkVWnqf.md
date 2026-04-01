# Claude Code Plugin Marketplace：Anthropic 正在“扼杀”一个个提示词归集项目

> 来源：[https://aigcreative.feishu.cn/docx/IWHpdGxeWoOyLLxVCDucgPkznCc](https://aigcreative.feishu.cn/docx/IWHpdGxeWoOyLLxVCDucgPkznCc)

> 来源：https://x.com/jiangydev/status/2011947305239200115

看到优秀的 sub-agents 和 skills 推荐，还是手动拷贝到工程中吗？

作者更新了提示词，本地怎么更新？

提示词修改后效果不佳，怎么回滚到原来？

# 一、提示词缺乏管理

## 古老的传播方式——拷贝

毫不夸张的说，如果不是互联网发达，就差用 U 盘拷贝提示词了。

为什么这么调侃？

这是我看到的：

+   把洋洋洒洒的 1000 字提示词放在推文里，请大家复制

+   用 iCloud 云同步提示词，再复制粘贴到项目里

+   当源提示词更新优化后，我不知道：什么时候改了？为什么改？改了什么？

对我来说，是不可接受的。**样式和目录结构的丢失**，也进一步加剧使用的难度。

## 重复的轮子建设

近期 skills 的讨论火热，出现了不少“小红书文章转推特”、“PPT生成”等 skills，为什么大家都在做相同的东西？

每个人的影响力都是一个圈子，一个想法的出现落地，到推广到其他圈子需要时间，这段时间内就会百家争鸣，最终导致重复建设。

如果有一个市场，这里有所有品类的商品，你可以先来找，找不到的话，再考虑自己生产。

> 注：并不是说，大家只能用相同的提示词，这里更强调的是“模版”的共享、学习。相同的用户群体使用相同的提示词，也是没问题的。众人拾柴火焰高，如果将大家的智慧集中到“提示词优化”，那么模版的质量会更高。同时，也减少初期投入探索精力，减少踩坑。

## 眼花缭乱的提示词

GitHub 上充斥着不少 skills、agent、plugin 仓库，其中不乏成百上千 star 的“高质量”资源。痛苦的是，每个仓库都想做个大而美的集合，最终却像一锅乱炖，到底哪些是真的好用的？

现在产生“垃圾”的成本很低，以至于很快就能填满道路，这也迫使我们放缓脚步慢慢寻路。

## 未知的安全威胁

mcp、skills 的出现，使得 Claude Code 能拥有的能力更强大，随之而来的，还有未知威胁。威胁可能来自 MCP 工具泄漏用户数据、未审查的 skills 执行风险命令。

危机四伏的互联网，我们无法持续地监测和消除威胁，到底谁来保证我们的安全？

# 二、个人探索

当遇到上面的问题后，我开始寻找恰当的方式管理提示词。

## 第一阶段：

我把提示词 git 仓库 clone 到本地的 ~/.claude/ 目录中，然后用软连接将 git 仓库链接到 ~/.claude/commands/ 等目录下。

这确实能解决更新问题，也可以写个脚本定时 git pull 更新 git 仓库中的提示词。

## 第二阶段：

早在2025年8月，当时都还没有 plugin，有类似 aitmpl 这种 templates 网站（包括 skillsmp），但这些并不能解决我的问题。

我意识到可以做个“提示词共享的服务”。它会具备以下能力：

+   command / sub-agents / mcp 共享、实时更新、有版本迭代能力

+   评分机制

+   settings 分发（包括 permission、env、公告等）

+   链接分享

+   安全审查

+   可视化页面

经过1个月，我在9月份做出了一个雏形，它能共享提示词以及分发 settings，也算解决了大部分的痛点。

# 三、插件市场的出现

10月，Claude Code 推出了 Plugin 和 Marketplace，这确实是个很大的进步，它基于 git 仓库做出了插件市场。Plugin 将 commands / sub-agents / mcp / skills，甚至是 hooks 都打包在了一起，放在 marketplace 里。

下图是一个 Plugin 的目录结构，以及 Marketplace 的使用方式：

Claude Code Plugin Marketplace

此时我已经感受到，我做的东西被替代了，而且现有的各种提示词仓库，没有官方支持的情况下，大概率是昙花一现。

## 3.1 Anthropic 官方（已内置）

👉 Git 地址：

anthropics/claude-plugins-official

Claude Code official plugin marketplace

## 3.2 如何安装三方市场

这里我们以

[@gmickel](https://x.com/@gmickel)

的工作流插件为例

👉 Git 地址：

gmickel/gmickel-claude-marketplace

安装市场：

shell

```
/plugin marketplace add https://github.com/gmickel/gmickel-claude-marketplace
```

安装插件：

shell

```
/plugin install flow-next
```

安装后就可以使用了，截图：

# 四、总结

Claude Code 的迭代速度非常之快，昨天大家都还在写 agent、workflow，今天就在吹 skills 了。还有最近发布的 Cowork，直接把别人的创业项目干掉了😅

我们能做什么？

1.  保持持续学习的心态，跟上 AI 的脚步，多听多看多做

1.  把更多的精力放在提示词上，写提示词就像创作，创作的灵感是你自己的东西，永远不会被替代

多说一句：现在还没看到有人做 settings 的管理，以及提示词安全审查，也许还可以卷一下～

也给 Anthropic 产品经理一个建议：再做个可视化页面吧，名字也帮你想好了【Claude Store】，把链接分享、安全审查都做进去，干脆再出个收费的盈利模式🤣

<https://aigcreative.feishu.cn/sync/K4Gvd9vUQsy9Oubwqaeclu0Fnbg>

# 知识库动态