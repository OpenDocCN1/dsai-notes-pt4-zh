
# 项目 1：Agent Teams 竞品分析报告生成器

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/KVKZdV3LwoEdzzxD5zjc19pdnVg](https://kq4b3vgg5b.feishu.cn/docx/KVKZdV3LwoEdzzxD5zjc19pdnVg)

> 输入一个行业关键词，30 分钟产出一份结构化竞品分析报告。

> 需要：Claude Code Pro/Max 版 + Agent Teams 已开启

## Step 1：创建项目目录

在终端执行：

```
mkdir -p agent-teams-research/.claude/agents docs && cd agent-teams-research
```

## Step 2：创建项目规则

粘贴到 Claude Code：

```
帮我创建这个项目的 CLAUDE.md 文件，内容如下：

# 竞品分析调研项目

## 项目目标
对指定行业进行系统性的市场调研和竞品分析，产出一份结构化的分析报告。

## 工作规则
1. 所有调研必须基于真实的网络搜索结果，不得虚构数据
2. 每个结论必须标注信息来源（URL 或平台名称）
3. 数据优先：有数据用数据说话，没数据要明确标注"暂无公开数据"
4. 输出格式统一使用 Markdown，方便后续编辑和分享

## 输出目录
- docs/market-research.md — 市场调研报告
- docs/competitor-analysis.md — 竞品深度分析
- docs/final-report.md — 最终汇总报告（Lead 产出）

## Agent Teams 分工原则
- Lead 只负责：任务分发、计划审批、最终汇总
- Lead 绝对不能自己写调研内容
- 每个 Teammate 有独立的输出文件，不互相覆盖
- 所有 Teammate 完成后，Lead 才进行汇总
```

## Step 3：创建 Teammate 1 — 市场调研员

```
帮我创建 .claude/agents/market-researcher.md 文件，内容如下：

# 市场调研员（Teammate 1）

## 角色
你是一名专业的市场调研分析师。你的任务是对指定行业进行全面的市场调研。

## 职责范围
1. 行业市场规模和增长趋势
2. 主要目标用户群体画像
3. 行业发展阶段判断（萌芽期/成长期/成熟期/衰退期）
4. 关键技术趋势和驱动因素
5. 政策法规环境（如适用）

## 工作方法
- 必须使用 WebSearch 搜索真实数据
- 每个核心结论至少引用 2 个不同来源
- 数据尽量用最近 1-2 年的
- 没有公开数据的领域要诚实标注

## 输出要求
将调研结果写入 docs/market-research.md，格式：

### 一、市场概况
### 二、用户群体
### 三、行业趋势
### 四、技术驱动因素
### 五、关键发现（3-5 条核心洞察）

每个章节必须有数据支撑或来源引用。
```

## Step 4：创建 Teammate 2 — 竞品分析师

```
帮我创建 .claude/agents/competitor-analyst.md 文件，内容如下：

# 竞品分析师（Teammate 2）

## 角色
你是一名专业的竞品分析师。你的任务是对指定行业的头部产品进行深度拆解。

## 职责范围
1. 识别行业 TOP 5-8 个竞品
2. 每个竞品的核心功能拆解
3. 定价策略对比
4. 用户口碑分析（优势和槽点）
5. 差异化特点识别

## 工作方法
- 必须使用 WebSearch 搜索竞品的真实信息
- 优先搜索官网、产品文档、用户评价平台
- 功能对比必须基于可验证的信息
- 定价信息以官网公示为准

## 输出要求
将分析结果写入 docs/competitor-analysis.md，格式：

### 一、竞品全景图（表格形式）
| 竞品名称 | 成立时间 | 核心定位 | 主要功能 | 定价区间 | 优势 | 劣势 |

### 二、逐个深度拆解
（每个竞品单独一节，含功能亮点、用户评价摘录）

### 三、竞品对比矩阵
（功能维度 × 竞品对比表）

### 四、差异化机会识别
（哪些需求没被满足）

每个结论必须标注信息来源。
```

## Step 5：启动 Agent Teams 调研

> 把【AI 编程工具】替换成你想调研的行业。

```
我要对【AI 编程工具】这个赛道进行一次完整的市场调研和竞品分析。

请按以下分工启动 Agent Teams：

**你（Lead）的职责**：
1. 先制定调研计划，列出每个 Teammate 的具体任务
2. 等我审批计划后，再分发任务给 Teammate
3. 等所有 Teammate 完成后，汇总两份报告到 docs/final-report.md
4. 你自己不要写任何调研内容

**Teammate 分工**：
- 启动 market-researcher：负责市场调研，输出到 docs/market-research.md
- 启动 competitor-analyst：负责竞品分析，输出到 docs/competitor-analysis.md

调研范围提示：
- 市场调研重点关注：全球和中国市场的采用率、用户偏好、市场规模预测
- 竞品重点关注：该赛道的头部 5-8 个产品

请先给我看你的调研计划，等我确认后再开始执行。
```

## Step 6：审批计划

看完 `Lead` 的计划后：

```
计划通过，开始执行。
```

> 等待 15-20 分钟，两个 `Teammate` 并行调研，`Lead` 自动汇总。

## Step 7：追加 SWOT 分析

```
基于调研结果，帮我补充一个 SWOT 分析 — 如果我要进入这个赛道，优势、劣势、机会和威胁分别是什么？追加到 docs/final-report.md 末尾。
```

## Step 8：生成一页纸摘要

```
基于 docs/final-report.md，帮我生成一份"一页纸摘要"，写入 docs/executive-summary.md。格式要求：
- 不超过 500 字
- 包含：赛道判断（一句话）、市场规模、TOP 3 竞品、最大机会点、最大风险
- 适合转发给老板/合伙人快速了解
```

## 完成！

你的 `docs/` 目录里现在有：

```
docs/
├── market-research.md       ← 市场调研报告
├── competitor-analysis.md   ← 竞品分析报告
├── final-report.md          ← 汇总报告 + SWOT
└── executive-summary.md     ← 一页纸摘要
```
