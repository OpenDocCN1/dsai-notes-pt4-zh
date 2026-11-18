
# API 要 200 刀，老金找到零费用替代方案 `Agent-Reach`，实测 9 大平台!

> 来源：[https://my.feishu.cn/docx/X6k5d3nMdoyQSwxI0wqcKMoIneC](https://my.feishu.cn/docx/X6k5d3nMdoyQSwxI0wqcKMoIneC)

Twitter/X 的 `API` 定价一直是个老大难问题。

2 月 6 号还出了个按量计费，算下来也不便宜。

![](img/52d27df0873b72e0490521244151bad2.png)

老金我看到这个价格表的时候，第一反应是：啊？这不是逼着大家找替代方案吗？

然后就在 `GitHub Trending` 上刷到了 `Agent-Reach` 这个项目。

两天 1100+ 星，MIT 协议（最宽松的开源许可，随便用随便改）全开源。

一句话总结：让你的 AI Agent 免费看到整个互联网。

## Agent 到底缺什么

先说个事实。

你用 `Claude Code` 也好，用 `Cursor` 也好，用 `OpenClaw` 也好。

这些 AI 编程 Agent 有一个共同的短板：它们看不到活的互联网。

什么意思？

你让它帮你分析某个 Twitter 用户的最新推文，它做不到。

你让它帮你搜 Reddit 上的技术讨论，它也做不到。

你让它帮你看 B 站某个 UP 主的最新视频内容，还是做不到。

不是 AI 不够聪明，是根本没有眼睛。

社交平台把内容锁在 `API` 付费墙后面，不掏钱就别想拿数据。

老金我之前想用 Twitter `API` 做点数据分析。

还没开始用呢，一个月小两千块人民币就没了。

`Agent-Reach` 解决的就是这个问题。

## 一条命令装好九个平台

`Agent-Reach` 的定位很清楚：它是个脚手架工具，不是框架。

区别在哪？

框架会包一层又一层，最后你连底层工具长什么样都不知道。

脚手架只管帮你把环境搭好，实际干活的还是上游工具。

安装就两行命令：

```
pip install agent-reach
agent-reach install --env=auto
```

装完之后跑一下 `agent-reach doctor`，它会告诉你哪些平台能用、哪些还要配置。

老金我实际装了一遍，9 个平台全部跑通了。

下面按照实际体验来说，哪些省心哪些折腾。

### 装完就能用的（零配置）

`YouTube`——底层调 `yt-dlp`，视频字幕评论都能抓，装完直接好使。

`GitHub`——调的 `gh` `CLI`，仓库、`Issue`、`PR` 原生支持。

`RSS`——内置的，没什么好说的。

网页——走 `Jina Reader`，给个 `URL` 就能提取正文。

这四个是真正的开箱即用，不用操心。

### 需要简单配置的

`B` 站——配个代理就行，国内平台终于也能被 Agent 读了。

`Reddit`——配代理 + `Exa` 搜索引擎，技术讨论一搜就有。

### 需要折腾一会的

`Twitter/X`——这个是重头戏。

不走官方 `API`，用 `Cookie` 认证模拟浏览器登录。

你得从浏览器开发者工具里手动复制 `auth_token` 和 `ct0` 两个 `Cookie` 值，贴到配置文件里。

听起来简单，但老金我实际操作的时候踩了个坑。

`Chrome` v130 以上版本加了 `Cookie` 加密，像 `rookiepy` 这种自动提取工具已经用不了了。

只能手动从 `DevTools` 里复制。

![](img/861521f792da7b49d9d2aebecbc89d34.png)

`Exa` 搜索——功能很好，全网语义搜索，AI 友好。

但 `Windows` 上有个编码 bug，`mcporter` 输出的 `Unicode` 字符（比如 ✔）会导致 `Python` 的 `GBK` 解码崩溃。

老金我（的 `Claude Code`）改了 `agent-reach` 的源码才修好。

如果你是 `Mac` 或 `Linux` 用户就没这个问题。

![](img/94ad24cd96787a41bcddfbcc6510a367.png)

📕——原文档说要 `Docker`，但其实不需要。

`npm` 装个 `rednote-mcp`，扫个码就行。

```
npm install -g rednote-mcp
rednote-mcp init  # 扫码登录
```

`Cookie` 保存在本地，下次直接用。

## 老金实测的三个发现

老金我实际装完跑了一遍，不是看文档猜的，是真踩过坑的。

### 第一个 `doctor` 命令是真好用

装完第一件事就跑 `agent-reach doctor`。

它会列出所有 9 个渠道的状态，绿的就是能用的，灰的需要配置。

哪个工具没装、哪个 `Cookie` 过期、哪个代理没设，全给你标出来。

老金我就是靠这个一个个把 9 个平台全部调通的。

### 第二个 `Windows` 用户要有心理准备

`agent-reach` 的底层工具很多是给 `Mac` / `Linux` 设计的。

`Windows` 上会遇到一些奇葩问题。

比如 `Exa` 搜索在 `Windows` 上跑不通，不是没配置，是 `Python` 的 `GBK` 编码解不了 `mcporter` 输出的 `Unicode` 字符。

老金我改了源码加了 `encoding='utf-8'` 才修好。

还有 `mcporter` 这个工具，在 `Windows` 上 `subprocess` 找不到 `.CMD` 文件。

这种问题你 `Google` 都搜不到，因为 `Mac` 用户根本遇不到。

如果你是 `Windows` 用户，碰到装好了但 `doctor` 一直说没配置的情况，大概率是这类兼容性问题。

### 第三个 `Cookie` 认证靠谱但要注意

`Twitter` 和 📕 都是 `Cookie` 认证。

老金我用了几天，`Twitter` 读推文没出过问题。

但 `Cookie` 是有有效期的，过期了要重新提取。

### 建议拿小号操作，别用主号冒险。

## 架构拆解

老金我看了一下源码结构，设计思路很清晰。

每个平台一个 `Python` 文件。

`twitter.py` 管 `Twitter`，`youtube.py` 管 `YouTube`，各管各的。

加新平台就加一个文件，删平台就删一个文件。

模块化做得很干净。

每个模块里有个 `check()` 方法。

这个方法就是给 `agent-reach doctor` 用的。

它会检查依赖工具装没装、认证信息配没配。

实际的数据读写不经过 `Agent-Reach`。

装完之后，你的 Agent 直接调用上游工具。

比如读 `Twitter` 就直接调 `xreach`，读 `YouTube` 就直接调 `yt-dlp`。

中间没有包装层，性能损耗为零。

这种脚手架设计有个好处：上游工具更新了，你不用等 `Agent-Reach` 跟着更新。

工具链的每一环都可以独立替换。

## 必须说的风险

老金我得说实话，这个项目不是没有风险。

### `Cookie` 认证的封号风险

`Twitter` 和 📕 用的都是 `Cookie` 认证。

说白了就是模拟你的浏览器登录状态去抓数据。

平台检测到异常行为，是有可能封号的。

官方文档也明确建议：用专用小号。

### `Cookie` 和 `Token` 本地存储 - 龙虾爱好者请特别注意

认证信息存在 `~/.agent-reach/config.yaml` 里，权限设置为 600。

只有你自己能读。

但如果你的电脑被入侵，这些认证信息就暴露了。

用完之后建议定期检查这个文件。

### 不是所有平台都稳定

`xreach CLI` 绕过官方 `API` 的方式，`Twitter` 随时可能改接口。

到时候可能需要等社区更新。

这种依赖第三方工具的方案，稳定性没法跟官方 `API` 比。

老金我觉得这些风险可以接受，但你得知道。

## 跟官方 `API` 的成本对比

老金我帮你算一笔账。

`Twitter/X` 官方 `API` 的价格：

免费 `tier` 极度受限，基本废了，只能发推不能读。

2 月 6 号新出了按量计费，算下来也不便宜。

`Agent-Reach` 方案的价格：$0/月。

当然，免费的代价是前面说的那些风险。

但对于个人开发者和小团队来说，每月省 $200 起，这个账太好算了。

## 两种安装方式

除了 `pip` 安装，`Agent-Reach` 还支持作为 `Skill` 安装：

```
npx skills add Panniantong/Agent-Reach@agent-reach
```

这种方式适合已经在用 `Skill` 体系的用户。

装完之后跟其他 `Skill` 一样管理，统一维护。

老金我建议大多数人用 `pip` 安装就行。

简单直接，不依赖额外的包管理器。

## 老金我的判断

`Agent-Reach` 解决了一个真实的痛点：AI Agent 访问互联网的成本太高。

两天 1100+ 星说明开发者社区确实有这个需求。

`GitHubDaily`、`HelloGitHub`、`阮一峰 weekly` 都推荐了这个项目。

`SitePoint` 和 `Substack` 也做了专题报道。

但老金我得提醒你：

这不是一个成熟的商业产品，是一个开源脚手架。

`Cookie` 认证方案有风险，平台接口可能变。

适合愿意折腾、能接受偶尔抽风的开发者。

如果你每个月在 `Twitter API` 上花 $200+，值得试试。

如果你就是想让 `Claude Code` 或者 `Cursor` 能读读网页内容，也值得试试。

但如果你在做企业级产品，需要 99.9% 的稳定性，还是老老实实买官方 `API`。

项目地址：https://github.com/Panniantong/Agent-Reach

如果你也在为 `API` 费用头疼，试一下 `Agent-Reach`，回来告诉老金我效果怎么样。

用的时候记得拿小号，别拿主号冒险。

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>
