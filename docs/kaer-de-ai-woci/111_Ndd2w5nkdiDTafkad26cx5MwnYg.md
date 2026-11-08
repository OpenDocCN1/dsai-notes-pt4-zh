# OpenClaw新9种神级技能！

> 来源：[https://aiwarts101.feishu.cn/docx/GuzjdI692oRbvwxTJHicVNEGnJe](https://aiwarts101.feishu.cn/docx/GuzjdI692oRbvwxTJHicVNEGnJe)

https://github.com/kepano/defuddle

1.  Defuddle

Obsidian创始人kepano写的，用只有一个，就是从任何一个乱七八糟的网页里，提取出最干净的Markdown文本。它牛就牛在还能自动生成结构化的元数据，比如作者，发布日期这些。甚至连脚注，数学公式，和一些特殊的引用块都能适配。

1.  bb-browser

https://github.com/epiral/bb-browser

@yan5xu 做的，通过Chrome 插件+CDP直接自动化本地浏览器。

用人话说就是，不需要我们去折腾Cookie或者模拟请求的无头浏览器了，直接用你已经登录好的状态，在浏览器自己的控制台里跑代码。

1.  OpenCLI

https://github.com/jackwener/opencli

@jakevin7 做的，它是上一个技能的威震天加强版，一口气把B站，知乎，小某书，X，reddit，weibo，github，v2ex啥的通通做成CLI（命令行界面）了，也就是说OpenClaw在执行任务时，不再需要去模拟我们人类繁琐的浏览器操作，而是可以直接通过指令，精准地提取数据，提交表单，或者监控更新，增删改查都能做到。

这样就可以一句话查看HackerNews的热帖，写入Obsidian

一句话查看X大佬某个更新的所有时间线，写入Obsidian

1.  lossless-claw

https://github.com/Martian-Engineering/lossless-claw

随着我们跟龙虾的对话越来越长，一个新的问题就出现了，它会开始遗忘。

OpenClaw的作者新推荐了一种无损失上下文管理方式，它会把你和龙虾的每一条对话，都持久化地存到一个数据库里，然后在后台，自动把那些旧的对话，层层打包成树状结构的摘要，来控制Token的消耗。同时，它还给龙虾配备了专属的搜索工具，让它在遇到忘记的细节时，能随时调取原始的对话记录。

1.  Self-Improving进阶版｜Self-Improving + Proactive Agent

https://clawhub.ai/ivangdavila/self-improving

原本的self-improving-agent我之前分享过，像一个学习日志系统，它会记录下自己犯过的错，然后提醒自己不要再犯。

而这个进阶组合，更像一个拥有长期记忆和行为策略的系统。它会记录错误，会定义出一套分层式的记忆结构，让龙虾定期复盘自己的行为，选择性淡忘过时的模式，维护一套长期的行动准则。

1.  openclaw-control-center

https://github.com/TianyiDataScience/openclaw-control-center

如果你跟我一样养了七八只龙虾的话，那就很需要一个控制中心了

这个面板可以看到每个任务消耗了多少资源，每只龙虾现在健不健康，有没有卡住。我还可以直接查看和修改每只龙虾的记忆，人设和任务文档。

1.  Openclaw-backup

https://clawhub.ai/alex3alex/openclaw-backup

这个Skill能定时备份整个龙虾的配置文件和记忆库。

就算你不小心把龙虾养死了，也能通过备份迅速恢复。

1.  cc-connect

https://github.com/chenhg5/cc-connect

我愿称之为超级连接器，把主流的7个Agent， Claude Code，Codex，Gemini CLI，OpenCode等接到9个im平台， 飞书，Discord，🐧啊，啥的都有。

都支持切换模型，调整推理强度，设置权限模式和定时任务。

1.

那如果只是简单的想把Claude Code和codex连接到飞书的话，

可以用藏师傅的Claude-to-im Skills

https://github.com/op7418/Claude-to-IM