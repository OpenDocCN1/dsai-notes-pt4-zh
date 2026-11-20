
# 20 个Claude Code隐藏命令

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/Rda5dDV7koEzqqxpi7YcK2IEnrd](https://qqs7y1hozd1.feishu.cn/docx/Rda5dDV7koEzqqxpi7YcK2IEnrd)

> 本文来源：👸号作者：渔夫 AIDally

## 简介

今天，分享用了一段时间 Claude Code，挑出了日常高频实用的命令，几乎每天都在用。许多时候，Claude Code 好不好用，很大程度取决于你知不知道这些隐藏命令。

![](img/6b7d3a4f20cfc5fe8b3f36d0a401c056.png)

## 隐藏命令

1、`/model [model]` 切换到 `opus` 模型

`model opusplan`，`opus` 负责深度思考和完整规划，自动切 `Sonnet` 执行代码。

这个命令对小白非常友好，质量最高，且省 token，也是社区公认“神技”，几乎所有大项目第一步都用。

2、`/plan` 开启之后，Claude Code 进入只读规划模式，这时候呢它只会分析代码仓库，输出计划，思考，不会动你任何代码。

接手新项目，如刚入职想对复杂项目重构时，先 `plan` 再动手了。

3、`/fast`

这个命令，是开启高速模式，响应速度大幅提升，适合快速迭代和调试。注意：`Fast Mode` 跑的还是同一个模型，只是 API 设置做了速度优化，成本更高，用完记得关，不然 token 费用蹭蹭涨。

4、`/compact` 一键压缩上下文，可以带参数指定保留什么，比如 `/compact 保留错误处理逻辑`。平时当上下文用量超过 80% 时必打的，可以配合 `/context` 监控使用。

5、`/review`

写好的代码，准备提交时，可以先 `review` 代码审查，减少上线时 bug，安全，格式，性能等情况出现。改完代码后几乎会执行，以前是项目负责人，现在换成 `claude review`。

6、`/rewind`

有后悔药的命令，执行后可以安全回滚到上一个 checkpoint。比如修改了代码出了问题直接撤回，不用手动 `git reset`，减少 `git` 学习成本。

7、`/clear`

清空当前会话，重置所有上下文。比如要切换新任务时，执行改命令。

8、`/init`

初始化项目，自动生成 `CLAUDE.md`，常用来新项目规范文件，可以制定规则。

9、`/agents`

用来管理子 `agent`，比如在实现并行任务或多步循环。开启后，可以同时派出多个 AI 去干不同的事，互相独立，并行推进。一个 `ai` 干完自动交接，不用你盯着，在大项目，复杂任务用这个能省很多时间。

10、`/doctor`

自动诊断环境，依赖，会话问题。遇到奇怪 bug 先跑这个。

11、`/todos`

会列出当前会话中的 `todo` 项，任务管理神器啊。

12、`/diff` 查看代码变更，准备 `push` 代码时，确认修改前必看，也很常用。命令依赖 `gh` CLI，需要安装一下，mac 用户执行，授权就行：

```
brew install gh && gh auth login
```

![](img/2c155db97b0c0ae3d296ac1a5beccd14.png)

13、`/help`

请求帮助，会显示所有可用命令，包括自定义命令。有很多隐藏功能都在这里找到。

14、`/context`

实时显示当前 `token` 使用率和上下文状态，防止爆内存，很实用的。

![](img/8b934dbde0c1658af9ef3c114c8f722c.png)

15、`/security-review`

主要是对未提交的改动做安全审查。注意也是命令依赖 `gh` CLI。

## 实用组合与技巧

🚀 超级实用 Tips（1）`/model opus` 规划 → 确认计划 → `/fast` + `Sonnet` 执行 → `/review` → `/diff` 确认 → 接受或 `/rewind`

![](img/da77cb7c57efa5a45811e6870a7ef6bd.png)

（2）自定义 slash commands

对于重度使用者，可以自定义命令。首先在项目根目录建 `.claude/commands/` 文件夹，放 `.md` 文件就能自己造命令了。文件名就是命令名。比如 `refactor.md` 就变成 `/refactor`。

（3）`loop`

该命令很有意思，如你告诉它"每隔 X 分钟，帮我做一件事"，它就会一直循环做，不需要你手动触发。使用场景如。

![](img/dce7082c76e783a32f5547266841a411.png)

更多场景：

+ `/loop 5m /review`：每 5 分钟自动跑一次代码审查

+ `/loop 每周检查依赖项漏洞`

+ `/loop 每隔10分钟提醒我，我的拉取请求是否已获批准并可以合并`

+ `/loop 每天上午9点深入研究AI在生产应用中的市场趋势`

+ `/loop 每天早晨循环检查ollama中最新未解决的问题并向我汇报`

好了，今天就分享到这里，Claude 除了好用，缺点就是贵了点，推荐成本控制。✦ 小迭代用 `/fast` + `Sonnet`，`Opus` 只用来规划，不仅省钱又高效，很实用组合拳。你最常用哪个命令？整理为一份清单，可保存：

![](img/39e4c89ad340242bc65fe824ed91d946.png)

## 参考

[https://code.claude.com/docs/en/command](https://code.claude.com/docs/en/command)
