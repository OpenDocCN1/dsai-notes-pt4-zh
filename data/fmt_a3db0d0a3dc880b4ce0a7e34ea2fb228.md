
# GPT-5.2 Codex来了：能独立跑7+小时的AI程序员，老金手把手教你玩转

> 来源：[https://my.feishu.cn/docx/A0EOd3CIGo4Zp5x2BFLcRvB4nhe](https://my.feishu.cn/docx/A0EOd3CIGo4Zp5x2BFLcRvB4nhe)

12月18号，OpenAI悄咪咪放了个大招。

GPT-5.2 Codex。

老金当天晚上就去折腾了一番，研究了两天才敢写这篇。

因为这玩意儿跟Claude Code完全是两个物种，不搞清楚核心区别容易误导大家。

![](img/4be2cb382438f0811e79043efd793c40.png)

---

## 最核心的区别：异步 vs 同步

先说结论，这是GPT-5.2 Codex和Claude Code最本质的差别。

Claude Code是同步交互式：

+ 你发一条消息，它回一条消息
+ 你在本地终端盯着看
+ 每一步都要你确认才能继续
+ 适合边写边调的日常开发

GPT-5.2 Codex是异步自主式：

+ 你给一个任务，它后台自己干
+ 在云端沙箱里独立跑7+小时不断
+ 中间不需要你盯着
+ 适合扔个任务出去吃饭，回来收结果

看出来了吗？

Claude Code像个贴身助理，你说一句它做一句。

Codex像个远程员工，你发个任务单它自己去搞。

这不是优劣问题，是使用场景完全不同。

## 为啥能跑7+小时？Context Compaction

这是Codex的核心黑科技。

普通AI的问题你们都知道：聊着聊着就忘了前面说啥。改个大项目，改到一半它突然问你"请问你想改哪个文件"——我刚才不是说了吗！

Codex怎么解决的？

Context Compaction（上下文压缩）技术：

+ 当上下文窗口快满的时候，自动压缩历史内容
+ 保留跟当前任务相关的关键信息
+ 丢弃无关的细节
+ 压缩后继续干活，周而复始

这样就能支持几百万token的超长会话。

官方数据：单次任务能连续跑7+小时，中间不断片。

Hacker News上有老哥实测，让Codex从零搭一个完整的SaaS应用，3小时17分钟自己搞定了，中间没干预过。

## 性能数据说话

SWE-bench是专门测AI解决真实GitHub issue能力的，不是玩具题，是真实项目里的真实bug。

GPT-5.2 Codex的成绩：

+ SWE-bench Pro：**56.4%**（SOTA，行业第一）
+ SWE-bench Verified：约80%
+ Terminal-Bench 2.0：64%

**对比一下**：

+ Claude Opus 4.5：SWE-bench Verified 80.9%
+ Claude Sonnet 4.5：SWE-bench Verified 72.7%
+ Gemini 3 Flash：SWE-bench Verified 78%
+ 老版GPT-4o：SWE-bench才33%

注意这里有个细节：

SWE-bench Pro和SWE-bench Verified是两个不同的测试集。Pro更难，问题更复杂；Verified相对标准化。

GPT-5.2 Codex在Pro上拿了56.4%的SOTA，在Verified上约80%跟Claude Opus 4.5差不多。

总结：编程能力确实是第一梯队，但不是吊打所有人，Claude也很强。

## 安全审计：这个真牛逼

GPT-5.2 Codex有个隐藏技能让老金眼前一亮：发现安全漏洞。

官方案例：它在React源码里发现了一个安全漏洞，编号`CVE-2025-55182`，CVSS评分10.0（最高危）。

不是那种一眼能看出来的SQL注入，是需要深度分析代码逻辑才能发现的问题。

做安全的同学应该懂这意味着什么——以前代码审计得人工一行行看，现在让AI先过一遍，效率直接起飞。

OpenAI还搞了个"Trusted Access Pilot"计划，邀请验证过的安全研究人员用Codex做漏洞挖掘。

## 价格怎么说

当然老金还是推荐直接使用中转站，国内直连，并且现在大部分的中转站都直接一个Key支持了Claude、Codex和Gemini了。

**ChatGPT Plus用户**（$20/月）：

+ 直接用Codex，不额外收费
+ 打开`codex.openai.com`登录就能用

**API调用**：

+ 输入：$1.75/百万token
+ 输出：$14/百万token

对比Claude API价格：

+ Claude Sonnet输入$3/百万token，输出$15/百万token
+ Claude Opus输入$15/百万token，输出$75/百万token

GPT-5.2 Codex的API价格比Claude Sonnet还便宜点，比Opus便宜很多。

但实际花多少钱看你用法。长时间异步任务消耗的token量会比较大，但你省下了盯着屏幕的时间。

## 和Claude Code怎么选？

老金两个都在用，说点真话。

### 选Claude Code的场景

+ 日常开发，边写边调
+ 需要实时反馈的任务
+ 本地代码库操作
+ MCP生态里有你要用的工具

### 选GPT-5.2 Codex的场景

+ 大型重构、迁移任务
+ 复杂bug排查
+ 安全审计、代码审查
+ 不想盯着屏幕等的任务

老金现在的用法：

日常改几行代码、写新功能，用Claude Code；

拿到一个"把这个老项目升级到新框架"的任务，扔给Codex，我去喝咖啡。

两个配合用，效率拉满。

## 怎么用上？

### 1、终端

安装指令：

```
npm -g i @openai/codex@latest
```

启动终端后，输入`codex`启动

![](img/e57fb7cb1b0e2191413a893c488f3295.png)

不需要每次允许的方法，输入 `/`

![](img/11db66e4d09e3795ec4bd0a39a8a1875.png)

选择全部允许：

![](img/608a48815fd79983ef724a8c2b1bdd99.png)

### 2. 使用VS Code插件

在扩展（`Ctrl+Shift+X`）中，搜索`codex`

![](img/5ae166f2906087a955f41ca8151cae4a.png)

如下点击选择操作：

![](img/df23d284c9e8d068e756d3f6a14ef8b6.png)

### Plus用户

1. 打开`codex.openai.com`
2. 登录你的Plus账号
3. 直接开干

### 用API

1. 到OpenAI官网申请API Key
2. 模型名：`gpt-5.2-codex`（别写错）
3. 支持400K输入 / 128K输出的上下文窗口

老金建议：有Plus的先在Codex里体验，感受一下异步执行是什么感觉，再决定要不要深度用API。

## 老金实测

拿了个真实项目试了试，任务是把一个3000行的Python文件拆成模块化结构。

**Codex的做法**：

1. 先分析整体结构，列出拆分计划
2. 一个模块一个模块地创建
3. 自动处理`import`依赖
4. 生成单元测试
5. 跑测试确认没改坏

**结果**：耗时2小时47分钟，中间没断过，代码风格保持一致。

关键是这2小时47分钟我在干别的事，不是盯着屏幕等。

换以前用Claude Code，这活儿我得在旁边守着，虽然Claude可能更快，但我的时间也搭进去了。

这就是异步的价值。

## 最后说两句

GPT-5.2 Codex不是"更强的Claude Code"，是完全不同的工具。

Claude Code是同步协作——你和AI一起干活。

Codex是异步委派——你甩手让AI自己干。

两个都是第一梯队的AI编程工具，选哪个看你的工作方式：

+ 喜欢掌控感、实时调整的，用Claude Code
+ 喜欢甩任务、多线程干活的，用Codex
+ 最聪明的，两个都用

AI编程工具这一年卷得太厉害了，但工具不重要，会不会用才重要。

Plus用户直接上Codex体验异步执行的感觉，值得一试。

## 参考来源

+ OpenAI官方博客：GPT-5.2 Codex发布
+ [SWE-bench排行榜](https://www.swebench.com)
+ [Hacker News讨论帖](https://news.ycombinator.com/)
+ [OpenAI API定价页](https://openai.com/pricing)

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>
