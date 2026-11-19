
# 💡【Agent Skills 知识库】

> 来源：[https://geekbang.feishu.cn/docx/WHt7daivVowRsUxDXvvcdN8Jnnb](https://geekbang.feishu.cn/docx/WHt7daivVowRsUxDXvvcdN8Jnnb)

## 一、`Agent Skills` 详细介绍

> GitHub地址：`https://github.com/anthropics/skills?tab=readme-ov-file`

- 官方定义：

> `Agent Skills` are folders of instructions, scripts, and resources that agents can discover and use to do things more accurately and efficiently.

> 智能体技能（`Agent Skills`）是包含指令、脚本和资源的文件夹，智能体可以通过发现并使用这些技能，更准确、高效地完成任务。

### 1、一点点背景：从`MCP`到`Skill`的时间线

2024年11月25日，Anthropic发布了`MCP`（`Model Context Protocol`）。

当时社区反应并不热烈。大多数人觉得这只是又一个连接协议，和`OpenAI` 2023年的`function calling`、`GPT Actions`没有本质区别。

转折点发生在2025年3月26日。

Sam Altman发推："People love `MCP` and we are excited to add support across our products."

`OpenAI`选择加入而不是对抗，这个决定改变了一切。后续 `Google DeepMind` 和微软（`Windows`）宣布支持 `MCP`。

一年时间，从"改天看看"到"行业标准"。

2025年10月16日，Anthropic悄悄发布了另一个东西：`Skill`。

Simon Willison在官方发布前几天就通过逆向工程发现了这个功能。

感兴趣的可以去看看：`https://simonwillison.net/2025/Oct/16/claude-skills/`

他在博客里写道：

"I expect we'll see a Cambrian explosion in `Skills` which will make this year's `MCP` rush look pedestrian by comparison."

他还指出`Skill`设计上的一个巧妙之处：

"There's one extra detail that makes this a feature, not just a bunch of files on disk. At the start of a session `Claude`'s various harnesses can scan all available skill files and read a short explanation for each one from the frontmatter `YAML`... This is very token efficient: each skill only takes up a few dozen extra tokens, with the full details only loaded in should the user request a task."

两个月后的12月18日，Anthropic将`Skill`也开放为开放标准。`Atlassian`、`Canva`、`Notion`、`Figma`等公司成为首批合作伙伴，`Microsoft`在`VS Code`和`GitHub`中采纳了`Agent Skills`规范。

同一家公司，一年内推出两个被行业广泛采纳的开放标准，这不是偶然。

Anthropic在`Agent`方向比`OpenAI`专注很多，最近这一年基本一直在引领。

### 2、`Skills` 是短期红利还是长期壁垒？

`Skills` 本身不会成为长期壁垒，但你在 `Skills` 上投入的学习和实践，会成为你的长期壁垒。

这就像当年投资 `Prompt` 工程的人，后来更容易理解上下文工程；投资上下文工程的人，现在更容易做出好的 `Skills`。

每一波技术浪潮的「短期红利」，都是下一波浪潮的入场券。

所以我的建议是：不要纠结 `Skills` 会不会被取代，而是问自己：通过做 `Skills`，我能去解决什么问题？积累什么能力？这些能力在下一波浪潮里还有没有用？

如果答案是肯定的，那就值得投入。

### 3、更多内容看点击这里👉

## 二、`Agent Skills` 最佳实践与场景

### [1、如何创建一个 `Skill` ？](https://geek-agi.feishu.cn/wiki/OpRWwqWq7idhUZkYZ7NcmOmAnpd)

## 三、`信息源｜国内外优质信息源`

### [B站：](https://geek-agi.feishu.cn/wiki/X3kgw3QiFiNMllkm3zdcSa6Sn6c)

## 四、`Agent Skills` 相关学习教程
