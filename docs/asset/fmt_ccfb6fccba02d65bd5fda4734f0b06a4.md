
# Agent Swarm 一人公司最佳实践：YC 创始人 GStack 项目实战

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/FNL5dE676opBGmxXzzJc4sQ1nKd](https://kq4b3vgg5b.feishu.cn/docx/FNL5dE676opBGmxXzzJc4sQ1nKd)

---

## §1 开场：一人公司时代与 Agent Swarm

### 1.1 一人公司的十亿美元想象

2026 年初，科技圈最热的话题不是某个新框架，也不是某个新模型——而是一个曾经听起来荒谬的预言正在逼近现实：

> "We're going to see 10-person companies with billion-dollar valuations pretty soon... in my little group chat with my tech CEO friends there's this betting pool for the first year there is a one-person billion-dollar company."

> —— Sam Altman，OpenAI CEO

这不是一个人的幻想。Anthropic CEO Dario Amodei 在 2025 年的 Code with Claude 开发者大会上被问到"一人十亿美元公司何时出现"时，给出了明确回答："2026 年"，置信度 70-80%。他还具体指出了最可能的领域：量化交易、开发者工具、自动化客服。

更重要的是，这个预言已经有了真实的前奏——2025 年 6 月，以色列开发者 Maor Shlomo 独自创建的 AI 应用平台 Base44，在上线仅 6 个月后以 8000 万美元现金被 Wix 收购，月净利润达 18.9 万美元，用户超 25 万。一个人，半年，接近亿美元退出。

![](img/c2fa29ebea5fbe93c928ca006e7c0904.png)

> TechCrunch 报道：[AI agents could birth the first one-person unicorn](https://techcrunch.com/2025/02/01/ai-agents-could-birth-the-first-one-person-unicorn-but-at-what-societal-cost/)

一人公司的想象力正在被 AI Agent 释放出来。但请注意一个关键区别：这里说的不是"一个人用 ChatGPT 聊天写代码"——而是一个人指挥一群专业化的 AI Agent，像管理一支 20 人团队一样运作。一个 Agent 做产品策划、一个 Agent 做架构设计、一个做代码审查、一个做 QA 测试、一个做安全审计、一个做发布部署……

![](img/94eb10f3c3ba9f8050f064ac7dc06540.png)

这种模式有一个正式的名字：Agent Swarm（智能体蜂群）。它和"让一个 AI 什么都干"的根本区别在于专业化分工——就像真实公司里的 CEO 不应该去写测试用例、QA 工程师不应该去做产品决策一样，每个 Agent 有明确的角色边界和专业约束。

问题来了：如何让一群 AI Agent 高效协作，而不是各自为政？ 答案藏在一个 2026 年最重要的工程概念里——

#### Agent Harness：从"能用"到"可靠"的关键基础设施

2025 年证明了 AI Agent 能工作。2026 年的核心命题是让 Agent 可靠地工作。业界对此有一个精辟的总结：

> "The model is commodity. The harness is moat."

> （模型是大宗商品，Harness 才是护城河。）

什么是 Agent Harness？用一个类比：如果 AI 模型是引擎，那 Harness 就是整辆车——方向盘、刹车、仪表盘、安全带。引擎再强大，没有车的约束和引导，它哪也去不了。

![](img/9ccbc33e01920e0af4ad1d569cde782e.png)

Anthropic 自己在 2025 年 11 月发表了一篇重要的工程博客《Effective Harnesses for Long-Running Agents》，首次系统化地定义了 Harness 架构的核心组件：

| 组件 | 作用 | 对应到开发实践 |
| --- | --- | --- |
| `CLAUDE.md` | 全局配置——定义 Agent 的身份、规则、约束 | 项目根目录的"宪法文件" |
| `SKILL.md` | 专业角色定义——规定 Agent 在特定任务中的行为边界 | 每个 slash command 背后的指令集 |
| Progress Files | 跨会话记忆——让新的上下文窗口知道之前做了什么 | `claude-progress.txt` 等状态文件 |
| `Git` | 状态追踪——原子提交记录每步变更，可回溯 | 每个 Agent 独立分支、clean commit |

> Anthropic 博客原文：[Effective Harnesses for Long-Running Agents](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents)

Harness 的威力有多大？LangChain 在 Terminal Bench 2.0 基准测试中，不换模型仅改善 Harness，就从第 30 名跃升到前 5 名（52.8% → 66.5%）。Vercel 做了一个反直觉的实验——删掉 80% 的可用工具，反而得到了更快的响应、更少的 token 消耗、更高的成功率。Manus 在 6 个月内重写了 5 次 Harness，模型完全没换，但每次可靠性都显著提升。

这意味着什么？你不需要等下一代更聪明的模型——你现在就可以通过设计更好的 Harness 来大幅提升 AI 协作效果。这才是一人公司真正可行的底层逻辑：不是因为模型变强了，而是因为我们学会了如何约束和编排它们。

那么，有没有人把 Agent Swarm + Harness Engineering 做成一个现成的、开箱即用的工具，让任何人都能上手？

有。而且做这件事的人，来头不小——

### 1.2 GStack 架构全景：28 个 Skill 的虚拟公司

2026 年 3 月中旬，Y Combinator CEO Garry Tan（陈嘉兴）做了一件让整个开发者社区震动的事——他把自己使用 `Claude Code` 的完整工作配置开源了。

这个项目叫 GStack。上线 48 小时突破 10,000 星，不到一周超过 33,000 星，截至今天已经积累了 47,900+ 星。

> GStack 仓库地址：https://github.com/garrytan/gstack

但 GStack 不只是"又一个 GitHub 热门项目"——它是目前最完整的 Agent Harness 实战样本。在深入 Garry Tan 的故事之前，我们先看看这个项目在技术上到底有多酷。

#### 项目解剖：不写代码的代码仓库

GStack 的核心是 28 个 Markdown 文件——没有复杂的框架代码，没有精巧的算法实现。它的"代码"就是自然语言。但这 28 个文件构成了一个完整的虚拟公司组织架构：

| 部门 | Agent 角色 | 对应 Skill | 做什么 |
| --- | --- | --- | --- |
| CEO 办公室 | YC 产品顾问 | `/office-hours` | 6 个灵魂拷问挑战你的产品假设，不写代码只做思考 |
| CEO 办公室 | CEO / 创始人 | `/plan-ceo-review` | 四种范围模式审视产品方向（扩张/收缩/维持/选择性扩张） |
| 工程部 | 工程总监 | `/plan-eng-review` | 锁定架构、产出数据流图 + 测试矩阵 |
| 设计部 | 高级设计师 | `/plan-design-review` `/design-consultation` | 0-10 分打分 + AI-slop 检测 + 完整设计系统 |
| 开发部 | 偏执的高级工程师 | `/review` | 专找"CI 能过但生产会炸"的 bug |
| QA 部 | QA 工程师 | `/qa` `/qa-only` | 打开真实 Chromium 浏览器逐页面点击测试 |
| 安全部 | 首席安全官 | `/cso` | OWASP Top 10 + STRIDE 威胁建模审计 |
| 运维部 | 发布工程师 | `/ship` `/land-and-deploy` | 同步主分支 → 跑测试 → 开 PR → 部署 |
| 调查组 | 排查工程师 | `/investigate` | 数据流追踪式 root cause 分析 |
| 文档组 | 文档工程师 | `/document-release` | 自动更新文档匹配代码变更 |

加上 `/canary`（金丝雀监控）、`/benchmark`（性能基线）、`/browse`（浏览器操控）、`/codex`（跨模型审查）、`/retro`（周度复盘）等辅助工具，合计 28 个 Skill——覆盖了从灵感到上线的完整软件工程生命周期。

> 完整 Skill 文档：GStack Skills Deep Dives

#### 核心配置：`CLAUDE.md` 与 `ETHOS.md`

GStack 的两个灵魂文件是 `CLAUDE.md` 和 `ETHOS.md`：

`CLAUDE.md` 是整个项目的"宪法"——它告诉 `Claude Code`：你有哪些可用的 Skill、遇到问题该怎么做、如何管理浏览器自动化。所有 28 个 Skill 在这里注册，相当于这支虚拟团队的"通讯录"。

> [`GStack CLAUDE.md` 源码](https://github.com/garrytan/gstack/blob/main/CLAUDE.md)

`ETHOS.md` 则是团队的"价值观手册"——它定义了三条核心设计哲学，会被自动注入到每个 Skill 的提示词中：

> [`GStack ETHOS.md` 源码](https://github.com/garrytan/gstack/blob/main/ETHOS.md)

ETHOS 开篇就亮出了 GStack 的世界观：

> "The Golden Age — A single person with AI can now build what used to take a team of twenty. The engineering barrier is gone. What remains is taste, judgment, and the willingness to do the complete thing."

翻译过来：一个人加上 AI 就能做到过去 20 人团队的产出。工程门槛已经消失，剩下的只有品味、判断力，以及把事情做完整的意愿。

#### 运行模式：从单线程到 Agent Swarm

GStack 支持两种运行模式：

模式一：单线程冲刺（本课采用）

在一个 `Claude Code` 会话中依次调用不同的 Skill。适合学习和轻量开发：

```
你：/office-hours → 产品灵魂拷问 → 设计文档
你：/plan-ceo-review → CEO 审视 → 范围锁定
你：/plan-eng-review → 工程审视 → 架构锁定
你：开始 Build → 代码实现
你：/review → 代码审查 → 自动修复
你：/ship → 创建 PR
```

模式二：多线程并行（Conductor 模式）

通过 `Conductor` 同时运行多个 `Claude Code` 会话，每个会话在独立工作空间中执行不同 Skill。这就是真正的 Agent Swarm：

```
┌─────────────────────────────────────────────────────────────┐
│  Session 1: /office-hours    → 产品思考                       │
│  Session 2: /review          → 审查另一个模块的代码             │
│  Session 3: Build feature-B  → 实现新功能                      │
│  Session 4: /qa              → 测试已完成的功能                 │
│  Session 5: /cso             → 安全审计                        │
│                                                               │
│  全部同时运行，互不干扰，通过 `Git` 同步状态                      │
└─────────────────────────────────────────────────────────────┘
```

Garry Tan 声称用这种模式，边全职运营 YC、边在 60 天内管理产出了 60 万行生产代码（35% 是测试代码），日均 10,000-20,000 行。他的 GitHub 贡献图显示 2026 年已有 1,237 次提交。

#### 应用场景与定制化价值

GStack 不是只能做 Web 应用——它的 Harness 架构适用于任何需要软件工程流程的场景：

| 场景 | 核心 Skill 组合 | 价值 |
| --- | --- | --- |
| SaaS 产品开发 | `office-hours` → `plan` → `build` → `review` → `qa` → `ship` | 完整冲刺，从想法到 PR |
| API 服务构建 | `plan-eng-review` → `build` → `cso` → `ship` | 架构锁定 + 安全审计 |
| 客户项目交付 | `plan-ceo-review` → `build` → `review` → `qa` | 需求对齐 + 质量保障 |
| 已有项目维护 | `investigate` → `review` → `qa` → `ship` | 排查 → 修复 → 测试 → 上线 |
| 技术选型评估 | `office-hours` + `plan-eng-review` | 灵魂拷问 + 架构评审 |
| 安全合规审计 | `cso` + `review` | OWASP + STRIDE 全覆盖 |

而且，GStack 最大的价值不在于直接使用它的 28 个 Skill——而在于它给了你一套可复制的模式。每个 `SKILL.md` 本质上就是一个"如何定义 AI 协作角色"的范本。看懂了它的写法，你可以为自己的业务场景编写任意角色：客服专家、数据分析师、法律顾问、市场策划……只要能用自然语言描述清楚角色边界，就能变成一个可靠的 AI 协作伙伴。

> 这也是本课最终的教学目标：不只是会用 GStack 的 28 个现成 Skill，更要学会用 GStack 的思路给自己的工作流定制 Agent——这才是一人公司的真正打开方式。

说了这么多技术细节，你可能已经迫不及待想动手了。但在此之前，让我们花两分钟了解一下 GStack 的"破圈现象"和它背后的人——这不是八卦，而是理解它设计哲学的必要背景。

### 1.3 近 5 万颗星背后的争议：一堆 Markdown 凭什么？

GStack 的爆发速度，在 GitHub 开源史上都算得上罕见——Star 增长曲线几乎是一条垂直线。

但伴随热度而来的是质疑。视频博主 Mo Bitar 发布了一条标题为"AI is making CEOs delusional"的视频，直言 GStack 本质上"就是一堆文本文件里的提示词"。TechCrunch 的报道标题也颇为意味深长："Why Garry Tan's `Claude Code` setup has gotten so much love, and hate"。Hacker News 上数百条评论分成了旗帜鲜明的两派。

那么问题来了：一堆 Markdown 文件，凭什么值近 5 万颗星？

如果你读到了前面 §1.1 关于 Harness Engineering 的分析，答案已经很清晰了——GStack 的价值不在于它写了多少代码，而在于它用 Markdown 构建了一个经过实战验证的 Agent Harness。就像业界所说的"The model is commodity, the harness is moat"——在 AI 模型日趋同质化的今天，如何约束和编排 AI 才是真正的技术壁垒。GStack 提供了一个现成的最佳实践模板。这也正是它引发争议的原因——很多人还没有意识到，约束 AI 的能力比使用 AI 的能力更稀缺。

![](img/f2b185ae920ee9d84aa1ed70b71772f0.png)

#### 本课你将收获

学完这门课，你会得到三样东西：

| # | 产出 | 具体内容 |
| --- | --- | --- |
| 1 | 一个可上线的 SaaS 产品 | 用 GStack 七步冲刺做出的 ShipLog（产品发布日志 + 用户反馈平台），含 Landing Page 和反馈墙功能 |
| 2 | 一份实战速查表 | 28 个 Skill 按工作流阶段分组，标注优先级和使用场景，日常开发随时可查 |
