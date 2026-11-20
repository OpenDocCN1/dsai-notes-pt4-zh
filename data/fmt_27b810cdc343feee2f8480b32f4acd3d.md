
# Claude Skills完全指南：从概念到实战

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/NvCndUkGVoGIwUxpFKkco8lznfb](https://qqs7y1hozd1.feishu.cn/docx/NvCndUkGVoGIwUxpFKkco8lznfb)

这篇文章会回答这些问题：

1. Skills到底是什么？和MCP、Subagent有什么区别？
2. 为什么Simon Willison说它可能比MCP更重要？
3. 怎么创建自己的第一个Skill？
4. 怎么设计一个Skill库？我自己是怎么做的？
5. 有哪些值得关注的资源和仓库？

---

## Part 1：理解Skills

### Skills是什么？一句话说清

Skills是模块化的能力包，包含指令、脚本和资源，让Claude在需要时自动加载和使用。

就这么简单。

但这句话有几个关键词需要解释：

"模块化"：Skills是一个个独立的文件夹，每个Skill做一件事。比如"生成PPT"是一个Skill，"审校文章"是另一个Skill。

"能力包"：每个Skill文件夹里可以包含：

+ `SKILL.md`（核心指令文件，必需）
+ `scripts/`（可执行脚本，可选）
+ `references/`（参考文档，可选）
+ `assets/`（模板和资源，可选）

"自动加载"：你不需要手动告诉Claude"现在用XX Skill"。Claude会根据你的任务描述，自动判断需要哪个Skill，然后加载。

举个例子。

以前你让Claude帮你审校文章，可能需要这样说：

> "帮我审校这篇文章。注意检查事实准确性，去掉AI味的表达，比如'不是...而是...'这种套话，把长句拆成短句，段落不要太长，像手机屏幕3-5行这样，加粗不要太多，每200-300字1-2处就够了，还要检查是否像真人在说话......"

每次审校都要说一遍，烦，Token也烧得厉害。

现在用Skills，我提前把这些规则写进"AI味审校"这个Skill里。下次我只需要说：

> "帮我审校这篇文章"

Claude自动识别到需要审校能力，加载"AI味审校"Skill，按照我定义的规则执行。

这就是Skills的核心价值：把重复的指令打包，按需加载。

### 渐进式披露：这个设计是真的聪明

用Skills之前，我一直有个疑问：如果我装了50个Skill，Claude启动时全部加载，那Token不是照样爆炸？

研究了一圈才发现，Anthropic用了一个很聪明的设计：渐进式披露（Progressive Disclosure）。

#### 什么是渐进式披露？

简单说，就是分阶段、按需加载。

一个Skill包含很多内容：核心指令、参考文档、执行脚本、模板资源。但Claude不会一次性把所有内容都加载进上下文。它采用三层加载机制：

第一层：元数据（Metadata）—— 总是加载

内容：`SKILL.md`文件开头的YAML部分，就两个字段：`name`和`description`。

```
---
name: ai-proofreading
description: 系统化降低AI检测率，增加人味。使用场景：审校文章、降低AI味、初稿完成后。
---
```

加载时机：Claude启动时就加载所有Skills的元数据。

Token成本：每个Skill大约100 tokens。就算你装了50个Skills，也就5000 tokens。

作用：让Claude知道有哪些Skills可用，什么时候该用哪个。

第二层：指令（Instructions）—— 触发时加载

内容：`SKILL.md`的主体部分，详细的操作指南。

加载时机：当用户请求匹配某个Skill的`description`时，Claude才加载这个Skill的完整内容。

Token成本：通常在3000-5000 tokens。

作用：告诉Claude具体怎么做。

第三层：资源（Resources）—— 引用时加载

内容：`scripts/`目录里的脚本、`references/`目录里的参考文档、`assets/`目录里的模板。

加载时机：只有当`SKILL.md`中的指令引用这些文件时才加载。

Token成本：几乎无限——脚本执行后只有输出进入上下文，代码本身不占Token。

作用：提供确定性的执行能力和详细的参考资料。

#### 算一笔账

说这个设计聪明，是有数据支撑的。

传统方式：所有规则写在`CLAUDE.md`里，每次对话都加载。我之前的写作`CLAUDE.md`有3000多行，大约4万tokens。每次对话都烧4万tokens，不管需不需要。

Skills方式：

+ 平时只加载元数据：50个Skills × 100 tokens = 5000 tokens
+ 需要审校时，额外加载审校Skill：+3000 tokens
+ 需要配图时，额外加载配图Skill：+2000 tokens
+ 一次对话通常只用1-2个Skills：总共约10000 tokens

节省了75%的Token消耗。

而且，这还没算脚本的优势。

#### 脚本的魔法

Skills可以包含可执行脚本。比如我的"图片配图与上传"Skill里有一个Python脚本，负责把图片上传到图床。

当Claude执行这个脚本时：

1. Claude生成一条bash命令：`python scripts/upload_image.py image.png`
1. 脚本在本地执行
1. 只有执行结果（图床URL）返回给Claude

脚本代码本身不进入上下文。

所以你可以写一个500行的Python脚本，处理各种边界情况、错误处理、日志记录。Claude只需要知道"执行这个脚本"，不需要理解每一行代码。

这是Skills比传统Prompt方式更强的地方：可以封装确定性的执行能力。

### Skills vs MCP vs Subagent：终于搞清楚了

这个问题被问过很多次。MCP、Skills、Subagent，看起来都是"扩展Claude能力"的东西，到底有什么区别？

我研究了一圈，终于理顺了。

#### 一句话区分

MCP让Claude能碰到外部系统。Skills告诉Claude碰到之后怎么用。Subagent是派一个人出去干活。

#### 详细解释

##### MCP（Model Context Protocol）

MCP是什么？一个连接协议。它让Claude能够访问外部系统：数据库、API、文件系统、各种SaaS服务。

你可以把MCP想象成"给Claude发工具"。

比如GitHub MCP，让Claude能够读取仓库、创建PR、管理Issues。Notion MCP，让Claude能够读写Notion页面。

MCP的核心价值是连接。它解决的问题是"Claude能访问什么数据"。

##### Skills

Skills是什么？使用手册。它告诉Claude拿到数据之后怎么用。

比如你用GitHub MCP连接了仓库，Claude能读代码了。但"怎么做代码审查"——检查哪些方面、用什么标准、输出什么格式——这些是Skills的工作。

你可以把Skills想象成"教Claude怎么用工具"。

Skills的核心价值是程序化知识。它解决的问题是"Claude应该怎么做"。

##### Subagent

Subagent是什么？派出去干活的人。

当你让Claude Code派一个Subagent去做任务时，Claude会新开一个独立的对话会话。这个Subagent有自己的上下文窗口、自己的系统提示、自己的工具权限。它干完活，把结果带回来。

你可以把Subagent想象成"派一个助手出去"。

Subagent的核心价值是并行执行和上下文隔离。它解决的问题是"怎么处理复杂的多步骤任务"。

#### 对比表

#### 什么时候用哪个？

用MCP：当你需要连接外部系统。

+ 查询数据库
+ 调用第三方API
+ 读写Notion、Jira、GitHub等

用Skills：当你有重复性的工作流程。

+ 代码审查流程
+ 文章审校流程
+ 报告生成流程
+ 任何"每次都要说一遍"的规则

用Subagent：当任务复杂、需要并行执行。

+ 审查整个代码仓库（耗时长）
+ 同时处理多个独立任务
+ 需要防止上下文污染

#### 它们可以组合使用

这三个不是竞争关系，是互补关系。

一个复杂的工作流可能同时用到三者：

1. MCP连接Salesforce，拉取销售数据
1. Skills定义数据分析流程：怎么计算增长率、怎么生成报告
1. Subagent并行处理不同区域的数据分析

我自己的写作场景：

+ 用Skills定义审校流程
+ 用脚本（在Skills里）上传图片到图床
+ 未来可能用MCP连接我的素材库数据库

### 为什么Simon Willison说Skills比MCP更重要？

Simon Willison是一个在AI圈很有影响力的技术博主。他写过很多关于LLM的深度分析文章。

2025年10月Skills发布时，他写了一篇文章：《Claude Skills are awesome, maybe a bigger deal than MCP》。

![](img/5870692589c133b8245cea0b39e32960.png)

他的核心论点是：Skills可能比MCP更重要。

为什么？

#### 理由1：Token效率

这是最直接的理由。

MCP有一个问题：Token消耗太高。

> "GitHub官方的MCP服务器，单独就要消耗数万个tokens。"

为什么？因为MCP需要预先加载所有能力的描述。你连接一个MCP服务器，Claude就要知道这个服务器能做什么、每个功能怎么调用、参数是什么。这些描述加起来，动辄几万tokens。

Skills不一样。平时只加载元数据（100 tokens/Skill），需要时才加载完整内容。

> "Skills通过让LLM自行探索工具，优雅地避免了这一问题。"

#### 理由2：简洁即优势

MCP是一个完整的协议规范。要实现一个MCP服务器，你需要：

+ 理解协议结构
+ 写服务端代码
+ 配置JSON
+ 处理通信

Skills呢？

> "Skills只是Markdown加上一点YAML元数据，和一些可选的脚本。"

会写文档就能写Skills。这个门槛差距太大了。

#### 理由3：跨平台兼容

MCP服务器是特定于宿主的。你为Claude Code写的MCP服务器，不一定能直接用在其他地方。

Skills不一样。它就是文件夹，里面是Markdown和脚本。

> "Skills不依赖Anthropic专有技术。你可以把同一个Skill文件夹指向`Codex CLI`、`Gemini CLI`，两者虽然没有Skills系统的原生支持，但仍可正常运作。"

事实上，OpenAI已经在`Codex CLI`里采用了相同的架构。Skills正在成为事实标准。

#### 理由4：生态预测

Simon Willison预测：

> "我预测Skills将带来比去年MCP热潮更壮观的寒武纪大爆发。"

为什么？因为门槛低。

写一个MCP服务器需要后端开发能力。写一个Skill只需要会写Markdown。

当创作门槛足够低，社区贡献就会爆发式增长。

#### 我的观察

用了三个月Skills，我认同Simon Willison的判断。

Skills的设计确实更符合LLM的本质——用文本描述能力，让模型理解并执行。

而不是用复杂的协议和代码来定义能力。

MCP更像是传统软件工程的思路：定义接口、实现服务、处理通信。

Skills更像是LLM原生的思路：写清楚怎么做，让模型自己去做。

简洁、高效、可组合。

用了三个月，我觉得Simon Willison的判断是对的。

### 谁在用Skills？适合谁？

#### 企业采用情况

2025年12月18日，Anthropic发布了Skills开放标准，同时公布了一批企业合作伙伴：

+ Atlassian：Jira工作流自动化
+ Canva：设计模板生成
+ Cloudflare：安全配置审查
+ Figma：设计系统规范
+ Notion：文档模板和工作流
+ Ramp：财务报告生成
+ Sentry：错误分析流程

这些公司都在用Skills来封装他们的专业知识和工作流程。

#### 真实案例：Sionic AI

Sionic AI每天运行1000+个ML实验。他们遇到一个问题：知识流失。

一个研究员花了3天测试了50种参数组合，发现4000字符块大小让某个指标优于其他配置。但这个发现写在Slack线程里，90%的人没看到。

三周后，另一个研究员又花了3天测试相同的东西。

他们用Skills解决了这个问题。

两个命令的知识管理系统：

1. `/advise` - 实验前咨询。搜索过往实验记录，找到相关的参数配置和失败案例。
1. `/retrospective` - 实验后沉淀。自动从对话历史中提取核心发现，生成结构化的Skill文件。

效果：

+ 重复实验率：40% → 5%
+ 参数调优时间：3天 → 1小时
+ 知识沉淀耗时：30分钟 → 30秒

#### 三类适合用Skills的人

第一类：有固定工作流的人

如果你的工作有重复性的流程，每次都要说一遍相同的规则，Skills就很适合你。

比如：

+ 写代码要遵循特定的代码规范
+ 写文章要遵循特定的风格指南
+ 生成报告要遵循特定的格式

这些规则打包成Skill，一次创建，反复使用。

第二类：团队协作的人

Skills可以分享。团队共用一套Skills，就意味着共用一套工作流程和标准。

新人入职，不需要学习所有规则，只需要用团队的Skills。

第三类：Token烧得多的人

如果你的`CLAUDE.md`已经很长了，每次对话都加载大量上下文，Skills可以帮你节省Token。

按需加载，只加载需要的部分。

#### 不适合用Skills的情况

+ 一次性任务（直接写Prompt就行）
+ 需要实时外部数据（用MCP）
+ 复杂的多步骤并行任务（用Subagent）

---

## Part 2：实战Skills

### 如何创建第一个Skill

先说一个核心观点：你不需要自己写Skill。

但把这些东西写成`SKILL.md`文件？这事让AI干就行。

> **你要做的是：**

1. 想清楚你要解决什么问题
1. 把你的工作流程说清楚
1. 提供足够的context和参考资料

#### Skill的基本结构

一个Skill就是一个文件夹，里面至少有一个`SKILL.md`文件：

```
my-skill/
└── SKILL.md
```

`SKILL.md`长这样：

```
---
name: hello-skill
description: A simple greeting skill. Use when user says hello or asks for a greeting.
---

# Hello Skill

When user greets you, respond with a warm, personalized greeting.

## Guidelines

- Be friendly and natural
- If user mentions their name, use it
- Keep it brief (1-2 sentences)
```

就这么简单。Claude Code会自动发现并加载这个Skill（2.1版本支持热重载）。

#### SKILL.md的关键字段

让我解释一下`SKILL.md`的结构。

YAML Frontmatter（必需）

文件必须以YAML frontmatter开头，包含两个必需字段：

```
---
name: skill-name
description: What this skill does and when to use it.
---
```

`name`：Skill的唯一标识。

+ 最多64个字符
+ 只能用小写字母、数字、连字符
+ 不能以连字符开头或结尾
+ 不能有连续的连字符

好的例子：`ai-proofreading`、`code-review`、`report-generator`

坏的例子：`AI-Proofreading`（大写）、`-my-skill`（连字符开头）

`description`：告诉Claude什么时候用这个Skill。

+ 最多1024个字符
+ 要包含"做什么"和"什么时候用"
+ 触发关键词很重要

好的`description`：

```
description: |
  系统化降低AI检测率，增加人味的三遍审校能力。
  使用场景：审校文章、降低AI味、初稿完成后、用户说&quot;太AI了&quot;。
```

坏的`description`：

```
description: 审校文章
```

（太简短，Claude不知道什么时候该用）

Markdown主体（可选但建议有）

Frontmatter之后是Markdown内容，也就是Skill的详细指令。

这部分没有格式限制，但建议包含：

+ 核心目标
+ 执行步骤
+ 示例输入/输出
+ 注意事项

官方建议：主体部分控制在500行以内。如果需要更多内容，放到`references/`目录下。

#### 进阶：添加脚本和参考文档

一个更完整的Skill结构：

```
my-skill/
├── SKILL.md                 # 核心指令
├── scripts/
│   └── process.py           # 可执行脚本
├── references/
│   └── DETAILED_GUIDE.md    # 详细参考文档
└── assets/
    └── template.md          # 模板资源
```

`scripts/` ：可执行脚本。

当`SKILL.md`中引用脚本时，Claude会执行它。脚本代码不进入上下文，只有执行结果进入。

这意味着你可以写复杂的脚本来处理确定性任务，而不占用Token。

`references/` ：参考文档。

当任务需要更多信息时，Claude会读取这些文档。采用渐进式披露，平时不加载。
