
# 第七章：Learning Path

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/RIo7dQq1zouDYixchvJcK1cVngc](https://qqs7y1hozd1.feishu.cn/docx/RIo7dQq1zouDYixchvJcK1cVngc)

Hermes Agent 功能多样，包括 `CLI` 助手、Telegram/Discord 机器人、任务自动化、强化学习训练等。本页面将根据你的经验水平和目标，帮助你确定从何处入手以及阅读哪些内容。

从这里开始

[如果您尚未安装 Hermes Agent](https://hermes-agent.nousresearch.com/docs/getting-started/installation)，请先查看安装指南，然后按照[快速入门](https://hermes-agent.nousresearch.com/docs/getting-started/quickstart)进行操作。以下所有内容均假定您已成功安装。

## 如何使用本页面

+ **了解自己的水平？** 跳转到[经验等级表](https://hermes-agent.nousresearch.com/docs/getting-started/learning-path#by-experience-level)，并按照您所在等级的阅读顺序进行。

+ **有特定目标？** 跳转到[按用例](https://hermes-agent.nousresearch.com/docs/getting-started/learning-path#by-use-case)并找到匹配的场景。

+ **只是随便看看？** 查看[主要特性](https://hermes-agent.nousresearch.com/docs/getting-started/learning-path#key-features-at-a-glance)表，快速概览 Hermes Agent 的所有功能。

## 按经验水平

## 按用例

选择与你想做的事情相符的场景。每个场景都会按照你应该阅读的顺序将你链接到相关文档。

### "我想要一个CLI编码助手"

使用 Hermes Agent 作为交互式终端助手来编写、审查和运行代码。

1. [安装](https://hermes-agent.nousresearch.com/docs/getting-started/installation)
2. [快速入门](https://hermes-agent.nousresearch.com/docs/getting-started/quickstart)
3. [CLI使用方法](https://hermes-agent.nousresearch.com/docs/user-guide/cli)
4. [代码执行](https://hermes-agent.nousresearch.com/docs/user-guide/features/code-execution)
5. [上下文文件](https://hermes-agent.nousresearch.com/docs/user-guide/features/context-files)
6. [小贴士与技巧](https://hermes-agent.nousresearch.com/docs/guides/tips)

> **提示**  
> 通过上下文文件直接将文件传递到您的对话中。Hermes Agent可以读取、编辑和运行您项目中的代码。

### "我想要一个Telegram/Discord机器人"

将 Hermes 代理作为机器人部署到您喜欢的消息平台上。

1. [安装](https://hermes-agent.nousresearch.com/docs/getting-started/installation)
2. [配置](https://hermes-agent.nousresearch.com/docs/user-guide/configuration)
3. [消息概述](https://hermes-agent.nousresearch.com/docs/user-guide/messaging)
4. [Telegram设置](https://hermes-agent.nousresearch.com/docs/user-guide/messaging/telegram)
5. [Discord设置](https://hermes-agent.nousresearch.com/docs/user-guide/messaging/discord)
6. [语音模式](https://hermes-agent.nousresearch.com/docs/user-guide/features/voice-mode)
7. [使用 Hermes 语音模式](https://hermes-agent.nousresearch.com/docs/guides/use-voice-mode-with-hermes)
8. [安全](https://hermes-agent.nousresearch.com/docs/user-guide/security)

完整项目示例，请参阅：

+ [每日简报机器人](https://hermes-agent.nousresearch.com/docs/guides/daily-briefing-bot)
+ [电报助手团队](https://hermes-agent.nousresearch.com/docs/guides/team-telegram-assistant)

### "我想自动化任务"

安排定期任务、运行批处理作业或链接代理操作。

1. [快速入门](https://hermes-agent.nousresearch.com/docs/getting-started/quickstart)
2. [定时任务调度](https://hermes-agent.nousresearch.com/docs/user-guide/features/cron)
3. [批量处理](https://hermes-agent.nousresearch.com/docs/user-guide/features/batch-processing)
4. [授权](https://hermes-agent.nousresearch.com/docs/user-guide/features/delegation)
5. [钩子](https://hermes-agent.nousresearch.com/docs/user-guide/features/hooks)

> **提示**  
> 定时任务可让Hermes Agent按计划运行任务，如每日总结、定期检查、自动报告等，无需您亲自在场。

### "我想构建自定义工具/技能"

使用您自己的工具和可重复使用的技能包扩展Hermes Agent。

1. [工具概述](https://hermes-agent.nousresearch.com/docs/user-guide/features/tools)
2. [技能概述](https://hermes-agent.nousresearch.com/docs/user-guide/features/skills)
3. [MCP（模型上下文协议）](https://hermes-agent.nousresearch.com/docs/user-guide/features/mcp)
4. [架构](https://hermes-agent.nousresearch.com/docs/developer-guide/architecture)
5. [添加工具](https://hermes-agent.nousresearch.com/docs/developer-guide/adding-tools)
6. [创建技能](https://hermes-agent.nousresearch.com/docs/developer-guide/creating-skills)

> **提示**  
> 工具是代理可以调用的单个功能。技能是将工具、提示和配置打包在一起的集合。从工具入手，逐步提升到技能。

### "我想训练模型"

使用强化学习，通过Hermes Agent的内置强化学习训练管道来微调模型行为。

1. [快速入门](https://hermes-agent.nousresearch.com/docs/getting-started/quickstart)
2. [配置](https://hermes-agent.nousresearch.com/docs/user-guide/configuration)
3. [强化学习训练](https://hermes-agent.nousresearch.com/docs/user-guide/features/rl-training)
4. [提供商路由](https://hermes-agent.nousresearch.com/docs/user-guide/features/provider-routing)
5. [架构](https://hermes-agent.nousresearch.com/docs/developer-guide/architecture)

> **提示**  
> 当你已经了解Hermes Agent处理对话和工具调用的基本原理时，强化学习训练效果最佳。如果你是新手，请先完成初学者路径。

### "我想将其用作Python库"

以编程方式将 Hermes Agent 集成到您自己的 Python 应用程序中。

1. [安装](https://hermes-agent.nousresearch.com/docs/getting-started/installation)
2. [快速入门](https://hermes-agent.nousresearch.com/docs/getting-started/quickstart)
3. [Python库指南](https://hermes-agent.nousresearch.com/docs/guides/python-library)
4. [架构](https://hermes-agent.nousresearch.com/docs/developer-guide/architecture)
5. [工具](https://hermes-agent.nousresearch.com/docs/user-guide/features/tools)
6. [会话](https://hermes-agent.nousresearch.com/docs/user-guide/sessions)

## 关键特性概览

不确定有哪些功能？以下是主要功能的快速指南：

## 接下来读什么

根据您当前所在的位置：

+ **刚刚完成安装？** → 前往 [快速入门](https://hermes-agent.nousresearch.com/docs/getting-started/quickstart) 运行你的第一次对话。

+ **快速入门完成了吗？** → 阅读 [CLI 使用方法](https://hermes-agent.nousresearch.com/docs/user-guide/cli) 和 [配置](https://hermes-agent.nousresearch.com/docs/user-guide/configuration) 来定制你的设置。

+ **掌握基础知识了吗？** → 探索 [工具](https://hermes-agent.nousresearch.com/docs/user-guide/features/tools)、[技能](https://hermes-agent.nousresearch.com/docs/user-guide/features/skills) 和 [记忆](https://hermes-agent.nousresearch.com/docs/user-guide/features/memory)，以释放代理的全部潜力。

+ **正在为团队做准备？** → 阅读 [安全](https://hermes-agent.nousresearch.com/docs/user-guide/security) 和 [会话](https://hermes-agent.nousresearch.com/docs/user-guide/sessions)，以了解访问控制和对话管理。

+ **准备好构建了吗？** → 深入阅读 [开发者指南](https://hermes-agent.nousresearch.com/docs/developer-guide/architecture)，了解内部机制并开始贡献。

+ **想要实际例子吗？** → 查看 [指南](https://hermes-agent.nousresearch.com/docs/guides/tips) 部分，了解实际项目和提示。

> **提示**  
> 你不必阅读所有内容。选择与你的目标相符的路径，按顺序点击链接，你就能迅速提高工作效率。你随时都可以回到本页面来确定下一步计划。
