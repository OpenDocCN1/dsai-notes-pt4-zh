# 项目3-内容选题生成器

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/J8bYdJojOonFNNxqzUgc1YNunzf](https://kq4b3vgg5b.feishu.cn/docx/J8bYdJojOonFNNxqzUgc1YNunzf)

# 项目 3：Agent Teams 内容选题生成器

> 热度 × 需求双维度交叉筛选，15 分钟产出 5 个高潜内容选题。

> 需要：Claude Code Pro/Max 版 + Agent Teams 已开启

---

## Step 1：创建项目目录

在终端执行：

```
mkdir -p content-topics/.claude/agents docs &amp;&amp; cd content-topics
```

---

## Step 2：创建项目规则

粘贴到 Claude Code：

```
帮我创建 CLAUDE.md 文件，内容如下：

# 内容选题调研项目

## 项目目标
为指定领域的内容创作者，找到本周最值得做的 5 个内容选题。

## 工作规则
1. 选题必须基于真实的热点数据和用户需求，不能凭空编造
2. 每个选题必须有&quot;为什么现在做&quot;的理由
3. 优先推荐有差异化空间的选题（不推荐已经被做烂的话题）
4. 考虑国内平台生态（B站、公众号、小红书、知乎）

## 输出目录
- docs/trends.md — 热点趋势报告（Teammate 1）
- docs/demands.md — 用户需求报告（Teammate 2）
- docs/topics.md — 最终选题推荐（Lead 汇总）

## Agent Teams 分工原则
- Lead 负责交叉比对两份报告，找出&quot;既有热度又有需求&quot;的交集
- Teammate 各自独立调研，确保视角多样性
```

---

## Step 3：创建 Teammate 1 — 热点猎人

```
帮我创建 .claude/agents/trend-hunter.md 文件，内容如下：

# 热点猎人（Teammate 1）

## 角色
你是一名敏锐的热点追踪专家。你的任务是找到指定领域最近 1-2 周的热点话题和趋势信号。

## 职责范围
1. 最近 1-2 周该领域的热门话题/事件
2. 社交媒体上的讨论热度
3. 行业大佬/KOL 最近在聊什么
4. 新产品发布、融资事件、政策变化等信号
5. 海外趋势（可能即将传导到国内的话题）

## 工作方法
- 使用 WebSearch 搜索最新的行业动态
- 搜索关键词组合：&quot;{领域} 最新&quot;&quot;2026 {领域} 趋势&quot;&quot;{领域} 热点&quot;
- 关注 Twitter/X、Reddit、Hacker News 上的讨论
- 关注 B 站、知乎、微信公众号的热门内容

## 输出要求
写入 docs/trends.md，格式：

### 一、本周热点事件（按热度排序）
| 事件 | 热度来源 | 讨论量级 | 时效性 |

### 二、趋势信号（正在升温的话题）
### 三、KOL 关注点（大佬们最近在聊什么）
### 四、海外趋势前瞻

每条标注信息来源和时间。
```

---

## Step 4：创建 Teammate 2 — 需求侦探

```
帮我创建 .claude/agents/demand-detective.md 文件，内容如下：

# 需求侦探（Teammate 2）

## 角色
你是一名用户需求分析专家。你的任务是找到指定领域的受众真正想看什么内容。

## 职责范围
1. 用户高频提问（搜索引擎、社区、评论区的常见问题）
2. 内容缺口（用户想看但现有内容供给不足的话题）
3. 痛点排序（哪些问题最让用户头疼）
4. 已有热门内容分析（播放量/阅读量 top 的内容都在讲什么）
5. 评论区需求挖掘（&quot;求教程&quot;&quot;求详解&quot;类话题）

## 工作方法
- 使用 WebSearch 搜索用户讨论、社区提问、评论区反馈
- 搜索 &quot;{领域} 教程 求&quot;&quot;如何 {领域}&quot;&quot;{领域} 入门 困难&quot;
- 关注 B 站评论区、知乎回答、CSDN 社区的用户反馈
- 找到&quot;高需求 + 低供给&quot;的选题空白

## 输出要求
写入 docs/demands.md，格式：

### 一、高频用户问题 TOP 10
| 问题 | 出现频率 | 来源平台 | 现有内容供给 |

### 二、内容缺口分析（想看但没人做的）
### 三、热门内容复盘（高播放量内容的共性）
### 四、评论区需求摘录（原始用户声音）

每条标注来源。
```

---

## Step 5：启动选题调研

> 把【AI 编程 / Vibe Coding】替换成你的内容领域。

```
我是一个做【AI 编程 / Vibe Coding】方向的内容创作者，主要在 B 站发视频。帮我找到本周最值得做的内容选题。

请启动 Agent Teams：

**你（Lead）的职责**：
1. 分发任务给两个 Teammate
2. 等两份报告都完成后，交叉比对热点和需求
3. 筛选出&quot;既有热度又有用户需求&quot;的交集话题
4. 产出 5 个推荐选题到 docs/topics.md
5. 每个选题附带：标题建议、核心角度、预估热度、差异化切入点

**Teammate 分工**：
- 启动 trend-hunter：调研最近 1-2 周的热点趋势，输出到 docs/trends.md
- 启动 demand-detective：调研用户真实需求和内容缺口，输出到 docs/demands.md

请先给我看计划。
```

---

## Step 6：审批计划

```
计划通过，开始执行。
```

> 等待 8-12 分钟，两个 Teammate 并行调研，Lead 自动交叉筛选。

---

## Step 7：（可选）深挖某个选题

> 把 {N} 替换成你感兴趣的选题编号。

```
第 {N} 个选题我很感兴趣，帮我进一步展开：
1. 这个选题的详细大纲（适合 B 站 15-20 分钟视频）
2. 开头 hook 怎么写（前 10 秒留住观众）
3. 差异化要点（怎么做得跟别人不一样）
追加到 docs/topics.md 对应选题下方。
```

---

## Step 8：（可选）生成内容日历

```
基于这 5 个选题，帮我排一个两周的内容日历，写入 docs/content-calendar.md。考虑：
- 话题时效性（热点话题优先发）
- 难易搭配（不要连续发高难度内容）
- 周中 vs 周末的发布策略
```

---

## 完成！

```
docs/
├── trends.md            ← 热点趋势报告
├── demands.md           ← 用户需求报告
├── topics.md            ← 5 个推荐选题
└── content-calendar.md  ← 两周内容日历
```

> 每周跑一次，持续获得新鲜选题。