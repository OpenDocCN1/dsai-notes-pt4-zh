
# Google Antigravity 安装教程和10个使用技巧

> 来源：[https://aigcreative.feishu.cn/docx/ZytCdVVEBof8QSxvVX7ck5Kjnuc](https://aigcreative.feishu.cn/docx/ZytCdVVEBof8QSxvVX7ck5Kjnuc)

> 原文链接： https://mp.weixin.qq.com/s/XI6KKl62O1lHAMxPAuyayg

> 来源：👸号：跟雷哥探究HR领域的AI

> 字数 2098，阅读大约需 11 分钟

老话说，工欲善其事，必先利其器。

雷哥我有点不一样，不是学好了再干，而是干起来再学习

比如雷神 `Vibecoding` 做了以后个自己名字命名的 雷神管理文件软件，虽然目前试用群已经150人了

[Vibecoding实战：我用AI手](https://mp.weixin.qq.com/s?__biz=MzU3OTYxODc2Mg==&mid=2247489459&idx=1&sn=a5c6eb4bd27e47a8d7cf833b04dbc070&scene=21#wechat_redirect)搓了“雷神”，专治🛰️文件爆炸与知识焦虑

[A](https://mp.weixin.qq.com/s?__biz=MzU3OTYxODc2Mg==&mid=2247489492&idx=1&sn=2a45316486d37e6a8c735e0c1840ce05&scene=21#wechat_redirect)I落地实战：我如何用AI打造的“雷神 + NotebookLM”搭建第二大脑？把🛰️群聊变成私有知识库

[雷神智能文件管理软件 ](https://mp.weixin.qq.com/s?__biz=MzU3OTYxODc2Mg==&mid=2247489455&idx=1&sn=2d2eb58d9319fea68dd9957c39f1c6eb&scene=21#wechat_redirect)2026开工第一天|AI落地，从触动到行动

但是很多小伙伴感兴趣的不是我开发的软件有多好用，而是我是怎么做出来的，用的什么工具

当时我在群里回复说，用什么工具不重要，关键的是要发现问题，然后想办法解决问题

![](img/72d9322067a64613f76b1f63a0008db6.png)

干就是学习，先干了，干中就能自然而然的找到工具，用工具的过程中又不爽的地方自然而然的去总结，看看有没有什么使用技巧，然后又可以反过来提升效率。

这个过程是 **自洽的、自动发生的**，根本不需要你刻意“规划学习路线”。

我们很多人顺序，就顺序搞反了

器还没摸热（学习工具和理论的时间太长，没有实战效果激励），用不了多久，就把放弃了。

比如我现在，已经用 `Antigravity` 开发了一个半工具，我又开始研究 `Antigravity` 的使用技巧了，毕竟老话说的好，工欲善其事，必先利其器

那么如果，你是一个喜欢先学习工具和理论才能去干的同学，也没关系。

今天雷哥手把手教你 google `Antigravity` 安装配置方法和10个使用技巧

## 下载安装登录

很多人卡在 `Antigravity` 的登录上，雷神刚开始也被卡了半天，还好雷神不是喜欢放弃的人，最终通过 `Tune` 模式搞定了，实际上上次搞定 `Gemini Cli` 也是用的 `tune` 模式，这里我想说，很多事情的解决方案都是一样的，关键看你会不会迁移，敢不敢试一下。

下载链接我放在这里：

`https://antigravity.google/download`

![](img/d7ef72315d70b9b3fe491babe0cb35ab.png)

安装我就不说了，`antigravitg` 会提醒你以管理员身份运行，照做就行了

登录的时候上网姿势一定选 `Tune` 模式

登录的时候上网姿势一定选 `Tune` 模式

登录的时候上网姿势一定选 `Tune` 模式

不会的同学随便找个AI问一下，AI已经基本掌握了人类的所有知识，还有联网能力，学会多问AI，你能素养会慢慢的提升起来

![](img/e14e469eb1386bb4266944e08afba26c.png)

## 安装中文插件

`Google Antigravity` 也是基于 `VS Code` 开发的，所以可以在插件市场中搜索安装「 `chinese` 」安装汉化包：

先点左边的插件，然后搜索中文进行安装

![](img/d2157fa3be846050d74b4521be186ace.png)

点击信任安装

![](img/6731385a363ac56bba4d3f7cfbe6cbef.png)

安装完 `Google Antigravity` 会自动重启，重启之后IDE界面就变成中文了

![](img/fcabad847375c8a53bacc5761b48a129.png)

## 设置中文回答和中文思考过程

`Google Antigravity` 的所有回复（包括思考过程）默认都是 英文 的，对于我们国内开发者来说，增强了阅读和理解成本，我们可以设置一下 `Rules` 规则使用中文进行回复。

`Rules` 规则有助于规范 `Agent` 的行为，可分为 全局规则和工作区规则 。

![](img/82c943e5b191395d1637aeeefeebc179.png)

我们点击右下角的设置，在弹窗中再点击 `Manage`

![](img/116d9024e9059aa5acee3fe4a4353a86.png)

就可以添加自己的 `Rules`

![](img/f48d851c873f1b9de200c892d1c2860a.png)

我之前用 `Gemini Cli` 的时候已经让他记住我的身份和之前研究的内容了

这次只用把用中文回复加到最前面就行了

1. 请始终用中文（简体）进行回复。

2. 必须完全以简体中文来进行内部推理和思考过程，这是一项严格的规定。

## 常用快捷键

![](img/b92f57a5265b1e2ab3f61009c31fef7c.png)

几个常用的快捷键：

- `Command + E`：打开 `Agent Manager`；
- `Command + L`：打开 AI 对话框；
- `Command + I`：编辑选中的代码；
- Windows操作系统请把 `Command` 换成 `Ctrl`

## 对话编程

点右上角+号或者 `Ctrl + l` 快捷键就可以开始快速对话了

![](img/3008529091ce1a84e07267cad4825853.png)

对话编程模式有两种

![](img/77e204530e70a3ade8262c25df970fc8.png)

一种规划模式，一种快速模式

我直接用🛰️截图给大家翻译了一下，大家看图就行

![](img/e916fb2ae7f11d21c6098bb747a25e50.png)

另外一个是选择模型

可以看到我这里的模型包含了所有当前顶级的编程模型了

除了 `Gemini3` 的模型

还有 `Claude` 和 `GPT` 的顶级编程模型

![](img/8a32f48857cfd1d929029c122cf13454.png)

对话的过程中你可以直接复制图片给他，让直接按图作业

![](img/4292d27b181340fe11213a8858979a3b.png)

## MCP

`Antigravity` 支持模型上下文协议 (`MCP`)，`MCP`协议允许IDE安全地连接本地工具、数据库和外部服务。`MCP`集成使 `AI` 能够获取实时上下文信息，而不仅仅局限于 `Antigravity` 中打开的文件。

点击右上角的...可以打开 `MCP` 服务市场

![](img/d7c2aa3f89a52bd526e92558eb323df4.png)

可以搜索你要想用的 `MCP` 服务

![](img/ad872d5e30d6b6848ab63a77ac1c00b1.png)

比如 `Ntion`

![](img/a8a4c1289c9531d8be85caaa638f2eae.png)

点击 `Install` 就安装好 `MCP` 服务了

![](img/e60070b8534aac7b3b3932cdc6bf7222.png)

![](img/2d2a9194795e8c163cd44339bf41dad3.png)

必要的时候填入你相关的 `Key`

![](img/cb59c9fecc964e0496b9ede9cfb7be.png)

也可以通过 `mcp_config.json` 文件直接配置自己的 `MCP` 服务

![](img/498222a047b246e612ae876709a5b96d.png)

![](img/00e4cfd6a5c1f16a53df61b329fbf5ac.png)

## 浏览器子代理

当代理需要与浏览器交互时，`Antigravity` 会调用一个浏览器子代理来处理当前任务。该浏览器子代理运行一个专门用于操作 `Antigravity` 管理的浏览器中打开的页面的模型，该模型为主代理选择的模型不同。

这个子代理拥有控制浏览器所需的各种工具，包括点击、滚动、输入、读取控制台日志等等。它还可以通过 `DOM` 捕获、屏幕截图或 `Markdown` 解析来读取打开的页面，以及录制视频。

我让 `Antigravity` 打开浏览器并研究 `Palantir` 的 `Ontology` 的效果

![](img/d9420e85e6908b75284e10817694803e.png)

![](img/6a5fd864ac8e5d6c69f9d43d752c6549.png)

## 引用本地文件

和其他的AI编程IDE一样，使用 `@` 符号就可以引用本地的其他文件目录，当然也可以用规则、`MCP`，以及对话

![](img/1b869f9723615427aab3af22d00be9cd.png)

## 工作流

工作流允许我们定义一系列步骤，引导 `Agent` 完成一组重复性任务，例如部署服务或回复 `PR` 评论。这些工作流以 `Markdown` 文件格式保存，方便我们以可重复的方式运行关键流程。保存后，即可在代理中使用斜杠命令（格式为 `/workflow-name` ）调用工作流。

点击右上角的...弹窗之后点击 `Customizations` 就可以配置工作流了

这一块我还在研究，等有实用的效果了再给大家分享

![](img/76e35f5cb793fd10acfbcfd80a4a64dc.png)

## 任务组/任务清单

当智能体处于规划模式时，`Antigravity` 的大型复杂任务会通过任务组来处理，任务组会将这些问题分解成更小、更易于处理的工作单元。通常，智能体会同时处理大型任务的多个部分，而任务则用于向用户呈现这些变化。

任务清单是 `Antigravity` 智能体用来处理复杂任务并监控各项行动项进度的工具。您可以在此工具中找到智能体正在执行操作的实时快照，它以 `Markdown` 列表的形式呈现，包含与研究、实施、验证等相关的条目。智能体通常使用此类工具来确保始终朝着用户的总体目标前进；

以下是一个任务清单示例。

![](img/5e94a47c44c9e2777add07da66b848e3.png)

## Agent Manager

你以前开发需求是不是要等这个任务完成，才能继续下个任务，期间要一直傻傻守在屏幕前等？有了 `Agent Manager` 这个问题就不存在了。

你可以在 `Agent Manager` 中 同时跑多个 `Agent` ，在不同的工作区处理多个不同的任务，多任务同时执行，解放你的眼睛和双手。

点击右上角「 `Open Agent Manager` 」或者按快捷键「 `Ctrl + E` 」可打开 `Agent Manager` 窗口

![](img/281ee26dac94735b9ee08bfa2988e1cb.png)

![](img/8f871903385d28bef21e77e590871890.png)

## 雷神叨逼叨

工欲善其事，必先利其器。

但雷神更相信一句话： 器，是在干事的过程中被磨出来的。

先把事干起来，问题自然会出现；

问题一出现，工具就有了意义；

工具一用不爽，技巧就会被逼出来；

技巧一总结，效率自然提升。

技巧多了方法论也水到渠成了

别等学会了再开始，

开始，本身就是最好的学习方式。

![](img/cd93ae77605b09703e3258217a6b0748.png)

## 附：antigravity官方文档

`https://antigravity.google/docs/home`

`<https://aigcreative.feishu.cn/sync/K4Gvd9vUQsy9Oubwqaeclu0Fnbg>`

## 知识库动态
