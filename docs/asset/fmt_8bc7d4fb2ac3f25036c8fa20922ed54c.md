
# Claude Harness Engineering 操作手册

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/BrngdkSmPoUV9mxkgWvcldPwn7c](https://qqs7y1hozd1.feishu.cn/docx/BrngdkSmPoUV9mxkgWvcldPwn7c)

## 这份文档不是传统 SOP

这份文档有严格的适用边界。它不是一个固定流程，而是一份带时间戳的、个人视角的阶段性认识。原因有三：

1. 最佳实践每月都在变——以当前模型和工具的更新速率，上个月的最优做法这个月大概率已经不是最优
1. 模型能力在持续增强——具体操作步骤写下来就在过时，今天需要的 workaround 明天可能被原生支持
1. 不同人的使用方式差异很大——没有统一的”正确用法”，一个人的最优路径对另一个人可能是过度工程

因此，这份文档只覆盖 [Claude Code](https://zhida.zhihu.com/search?content_id=271730411&content_type=Article&match_order=1&q=Claude+Code&zhida_source=entity) 最基本的使用方法，聚焦在更长时间内都适用的主要原则，而不是具体操作步骤。里面的模板和示例是参考，不是规定。当你的判断和文档冲突时，信你自己的判断。

[蜂群模式](https://zhida.zhihu.com/search?content_id=271730411&content_type=Article&match_order=1&q=%E8%9C%82%E7%BE%A4%E6%A8%A1%E5%BC%8F&zhida_source=entity)、[多智能体协作](https://zhida.zhihu.com/search?content_id=271730411&content_type=Article&match_order=1&q=%E5%A4%9A%E6%99%BA%E8%83%BD%E4%BD%93%E5%8D%8F%E4%BD%9C&zhida_source=entity)、[Skills 体系](https://zhida.zhihu.com/search?content_id=271730411&content_type=Article&match_order=1&q=Skills+%E4%BD%93%E7%B3%BB&zhida_source=entity)等高级用法不在本文讨论范围内。

## 核心原则

本 SOP 建立在 Harness Engineering 的四大支柱之上，并针对 Claude 的特性做了适配：

## 规则

本手册的每条规则按违反后的代价分为三个层级：

### 2.1 主对话只做需求澄清 🔴

主对话负责：澄清需求、定义范围、识别风险、讨论优先级、产出 task brief、做复盘。

不在主对话里做大规模代码修改、跑测试、粘贴海量日志。

> 简单任务豁免：改 typo、加一行配置、单文件小修——如果 5 分钟内能完成且不需要 plan，可以直接在当前对话做，不必新开 session。判断标准：改动是否足够小到不会污染当前上下文。

### 2.2 工程任务新开 session 🔴

一个工程 session 只做一个 coherent unit of work（一个 bug / 一个 feature / 一次重构 / 一次 review）。

### 2.3 约束必须外化 🔴

口头提醒不可靠。约束要落地为：

+ `CLAUDE.md` / `.claude/rules/*.md` / `docs/ARCHITECTURE.md`
+ hooks / skills / subagents
+ lint / typecheck / tests
+ task brief 里的 Constraints 和 Non-goals

凡是你重复提醒两次以上的事情，就应该升级成机制。

### 2.4 主动管理 context 寿命 🔴

不要把 `compact` 当正常工作流。当出现以下信号时，停止当前 session，写 handoff，重开：

+ Claude 反复读同一批文件、忘掉已讲过的约束
+ 回答越来越长但推进越来越少
+ 把小任务越做越大、开始合法化 unrelated refactor
+ 循环修复——改了 A 破了 B，修了 B 又破了 A，来回打转
+ 你已经感觉需要 `/compact` 才能继续

> 关于 context 阈值：没有精确的百分比硬线。不同任务对 context 质量的敏感度不同——简单 bug fix 可以容忍更长 session，复杂架构变更则应更早切断。核心判断依据是行为信号（上面列的那些），而不是数字。

### 2.5 验证必须自动化且成本趋零 🔴

不要害怕 AI 犯错，要害怕犯错之后没有反馈。

核心思路：你不需要自己写测试。你需要确保验证环节存在并且能自动运行——让 Claude 在实现的同时写测试或使用已有测试，改完自动跑，失败自动修。整个循环不需要人工介入。

落地方式：

+ 最低限度：让 Claude 实现功能的同时写测试，实现后立刻跑 tests + typecheck，失败了 Claude 直接看报错并修复
+ 进阶：用 hooks 自动过滤测试输出，只把失败项喂回；用 reviewer subagent 自动审查 diff
+ 理想态：沙盒运行 → 提取 stderr → 静默反馈 → 自动修复，形成无人值守的验证闭环

判断标准：如果你需要手动复制粘贴报错给 Claude，说明验证闭环还没建好。

## Harness 标准工作流

### 阶段 A：主对话 → 产出 task brief 🔴

```
请不要写代码，只做需求整理。
把下面需求整理成 tasks/TASK-<date>-<name>.md，包含：
- Goal / Scope / Constraints / Non-goals
- Acceptance criteria / Verification / Risks / Unknowns
- Relevant files/directories
```

结束条件：task brief 写完、影响范围清楚、验收标准明确、主要风险识别完毕。

### 阶段 B：新 session → 先 plan 🔴

```
Read: CLAUDE.md, docs/ARCHITECTURE.md, tasks/TASK-xxx.md
Do not implement yet.
Return: affected files, risks, minimal plan, verification steps
Constraints: minimal diff, no unrelated refactor, do not change public API
```

### 阶段 C：批准后 → 实现 🔴

🟡 大项目建议重开 session 再动手。原因：

1. Plan 阶段产生的对话（方案比较、追问、废弃思路）对实现没用，但会一直占着 context
1. 实现需要的 context 和 plan 不同——plan 要广（多文件、多方案），实现要深（聚焦改动、跑测试、看报错）
1. 新 session 只读最终 plan 文件，起点更干净，Claude 不容易漂移

如果 plan 一轮就定了、对话很短，直接继续实现即可，不必强制重开。

```
Implement the approved plan.
Constraints: minimal diff, no unrelated refactor, preserve public API
Verify: <具体命令>
Return only: files changed, commands run, results, remaining risks
```

### 阶段 D：监测衰退 → 及时 handoff 🔴

出现 §2.4 中列出的衰退信号时立刻停止，要求 Claude 写 handoff：

```
Write handoff file with:
- current status / files changed / commands run
- passing checks / failing checks
- unresolved issues / exact next step / risks
- what not to revisit
```

### 阶段 E：收尾 → review + verify 🟡

这一步在实现 session 里直接做，不要重开。原因：当前 session 已经有完整的改动 context（改了哪些文件、跑了什么测试、哪些通过），重开反而要重新读一遍 diff，浪费 token 且容易遗漏。

```
Before finishing, self-review:
- scope creep / unintended file changes / missing tests
- architecture violations / whether implementation matches task brief
Return: must-fix / should-fix / safe-to-defer
```

## 约束落地对照表

不要只靠嘴说——落地为可执行的机制。

## 省 Token 要点 🟢

1. `CLAUDE.md` 控制在 200 行以内，项目级规则才放这里；目录级拆到 `.claude/rules/`，专项流程拆到 skills
1. 问得具体：动作 + 目标对象 + 位置 + 约束 + 验证 + 输出格式
1. 高体积内容隔离：超长日志、大量测试输出、大范围搜索 → 交给 subagent
1. 简单任务降低思考预算：`/effort` 或 Sonnet
1. 能 one-shot 就不长聊：`claude -p "query"` 单次查询后退出
1. 无关任务之间 `/clear`：顺手继续问另一个问题通常更贵
1. MCP 按需挂载：能用 CLI 工具解决就别上 MCP

## Session 生命周期

继续当前 session 的条件（全部满足）：

+ 同一个任务、约束没变、范围没扩大、Claude 没漂移、context 健康

必须重开的条件（任一满足）：

+ 换了任务 / 目标变了 / 新增约束 / 明显漂移 / 需要大段重新解释背景

重开前一定写 handoff——把状态写成文件，不留在对话里。

## 反模式速查

## Checklist

开工前

+ ☐ 主对话只做需求澄清，产出 task brief
+ ☐ 关键约束已文件化
+ ☐ 验收标准和验证命令明确

工程 session

+ ☐ 先读 `CLAUDE.md` + 相关 docs + task brief
+ ☐ 先 plan，不急着写
+ ☐ 限制 scope / non-goals / 返回格式

实现中

+ ☐ 按 plan 执行，不做计划外的事
+ ☐ 出现漂移立刻停
+ ☐ 不贴海量原始输出

收尾

+ ☐ self-review / reviewer agent
+ ☐ 写 handoff 文件
+ ☐ 记录 remaining risk
+ ☐ 关闭 session，下个 session 从文件继续

## 附录：理论依据

以下是本手册各条规则的理论依据，引用自 Anthropic 官方文档。

Session 不是长期记忆体：每个 Claude Code session 从全新 context window 开始，跨 session 的持久信息靠 `CLAUDE.md` 和 auto memory。

`CLAUDE.md` 是 context，不是强制配置：越具体、简洁、结构化，遵循效果越稳定。

Context 会腐坏（context rot）：随 token 增长，accuracy 和 recall 下降。问题不是”还能不能塞进去”，而是”质量是否已经在下降”。

复杂任务应先 plan 再 implement：Plan Mode 让 Claude 先只读分析、收集需求、提出方案。

约束应机械化：hooks 可在生命周期事件自动运行，`PreToolUse` 可在工具执行前拦截。

高体积操作应隔离：subagents 运行在独立 context，冗长输出留在子上下文，只带摘要回来。

Skills 按需加载：把专项流程从 `CLAUDE.md` 挪到 skills 更省基础上下文。
