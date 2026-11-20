
# 你根本不会用 AI 写代码

> 来源：[https://feicaiclub.feishu.cn/docx/ItlTd4tImoQj74xanlvcwT1cnMe](https://feicaiclub.feishu.cn/docx/ItlTd4tImoQj74xanlvcwT1cnMe)

![](img/c61b8de9f2961bdecec9b6e9f755c17e.png)

如果你跟大多数人一样，你以为自己会用 AI Coding 工具。

你打开 Cursor，或者 Claude Code，或者 Codex，或者刚出的 Antigravity，然后你开始打字。你把需求扔给它，它吐出代码，你复制粘贴，偶尔改改，然后你觉得自己是个"用 AI 提效的人"。

我不是想打击你。但如果你只是在和 AI 聊天，你根本没在用它。

你只是在用一个很贵的补全工具。

真正的 AI Coding 不是对话，是编排。不是你问它答，是你搭建一套系统，让不同的"AI 工人"各司其职，自动完成复杂任务。

这套系统的核心，就是两个你可能听过但从没真正理解的概念：Sub-agent 和 Skill。

四大 AI Coding 工具——Claude Code、OpenAI Codex、Google Antigravity、Cursor——都在押注这两个东西。但它们的实现方式完全不同，适合的人也完全不同。

这篇文章会告诉你：这四个工具在 sub-agent 和 skill 上到底有什么区别，以及你应该选哪个。

这不是产品评测。这是一份让你真正理解"AI 编排"的指南。

---

## I — 先搞清楚这两个东西到底是什么

在对比之前，你必须先理解这两个概念。不是那种"哦我知道"的理解，是那种能让你自己设计方案的理解。

### Skill：可复用的工作流能力包

你可以把 Skill 想象成一个"技能卡"。

它的本质是一个文件夹，里面装着：一份说明书（通常叫 `SKILL.md`），可能还有一些脚本、参考资料、资源文件。

当 AI 需要做某件特定的事情时——比如"用特定格式写测试"或者"按公司规范生成 API 文档"——它会去翻这个技能卡，按照里面的指令执行。

关键机制叫渐进式加载（progressive disclosure）：AI 启动时只读技能的名字和简介，只有真的需要用的时候，才会把完整的说明塞进上下文。这样不会撑爆 token 窗口。

Skill 解决的问题是：让 AI 的能力可复用、可共享、可标准化。

你写了一个"生成单元测试"的 skill，团队里所有人都能用。你不用每次都在 prompt 里重复写"请按照 `Jest` 格式，覆盖边界情况，mock 外部依赖……"。

### Sub-agent：独立上下文的专职工人

Sub-agent 是另一回事。

你可以把它想象成"分身"。主 agent 是你的总管，sub-agent 是它派出去的工人。每个工人有自己的脑子（独立上下文），有自己的权限（比如这个只能读、那个可以写），干完活之后把结果交回来。

关键特征是隔离：

+ 上下文隔离：sub-agent 的对话不会污染主对话

+ 权限隔离：你可以限制某个 sub-agent 只能读代码不能改

+ 工具隔离：不同 sub-agent 可以用不同的工具集

Sub-agent 解决的问题是：让复杂任务可以分解、并行、安全地执行。

比如你让 AI 重构一个大项目，它可以派一个 sub-agent 去"只读分析"代码结构，派另一个去"实际修改"文件，两边互不干扰。分析的那个不会误删你的代码，修改的那个不用等分析完。

### 两者的关系

一句话：Skill 给当前对话加能力，Sub-agent 把任务丢到另一个脑子里跑。

Skill 是"你会什么"，Sub-agent 是"谁来干"。

它们可以组合使用：一个 sub-agent 可以被配置成"只用某几个 skill"。这才是真正的 AI 编排。

现在你理解了概念，我们来看四个工具是怎么实现的——以及它们的实现差异意味着什么。

---

## II — Claude Code：把 Sub-agent 当一等公民

Claude Code（很多人误写成 CloudCode）是 Anthropic 的命令行编程工具。

在 sub-agent 这件事上，它做得最"产品化"。

### Sub-agent：真正的独立代理

Claude Code 的 sub-agent 不是概念，是产品功能。你可以用 `/agents` 管理它们，可以自定义创建，可以配置独立的系统提示词、工具权限、甚至模型。

它内置了几个专用 sub-agent：

+ `Explore`：只读模式，专门用来快速搜索和理解代码库，禁止写操作

+ `Plan`：计划模式下的研究员，同样只读

+ `General-purpose`：干脏活的，可以改代码

这种设计有一个很明确的好处：硬隔离。

`Explore` 模式下，AI 根本没有写文件的能力。不是"被告知不要写"，是"没有那个工具"。这在做代码审查、安全分析时极其有用——你不用担心 AI 分析的时候顺手改了什么。

还有一个限制值得注意：sub-agent 不能再生成 sub-agent（防套娃）。这是安全设计，但也意味着嵌套复杂任务时你得自己规划层级。

### Skill：强自动触发

Claude Code 的 skill 是"模型调用型"的——AI 根据你的请求自动判断要不要用某个 skill。

你不需要（也很难）手动点名调用一个 skill。你就正常说需求，AI 会自己翻技能卡。

这有好有坏。好处是流畅，坏处是不够可控。如果你想强制用某个 skill，你得通过 slash commands 迂回，而不是直接说"`$xxx` skill"。

另一个有意思的设计是 `context: fork`——skill 可以配置成在"fork 出来的上下文"里运行，相当于 skill 也享受了 sub-agent 的隔离特性。

### 安全提示

Anthropic 明确警告：skill 可能携带脚本和指令，你要审计来源。恶意 skill 可以引导 AI 泄露数据或执行危险操作。

这不是吓你。开源社区里已经有人研究 skill 级别的 prompt 注入了。

---

## III — OpenAI Codex：工程师的多代理编排

OpenAI Codex 需要分两层理解：产品层和 SDK 层。

### 产品层：并行任务沙箱

在 Codex 产品里，你不太会看到"sub-agent"这个词。它更强调的是"并行处理很多任务"——每个任务跑在独立的云端沙箱环境里，完成后给你可验证的证据（日志、测试输出）。

这种设计的核心价值是隔离和验证：任务之间互不干扰，结果可追溯。但它不是"sub-agent"那种"有自己的人格和权限配置"的概念。

### SDK 层：显式多代理编排

如果你用 Agents SDK，那就完全是另一个世界了。

你可以定义多个角色代理，设置 `handoff`（交接）——当一个 agent 做完它的部分，把任务交给下一个。整个过程有 trace 记录：每次 prompt、工具调用、交接都被记下来，方便审计和复盘。

这是 Codex 最强的地方：可审计性。

在企业场景里，你必须能解释"AI 为什么做了这个决定"。Codex 的 trace 机制让你可以回放整个多代理协作过程，看到每一步的输入输出。

### Skill：显式调用最完整

Codex 的 skill 体系是四个工具里"工程化"程度最高的。

两种触发方式：

1. 显式调用：在 prompt 里用 `/skills` 或者 `$skill_name` 直接点名

1. 隐式匹配：任务描述匹配 skill 的 description 时自动启用

最关键的区别是：默认只注入 skill 的名字和描述，指令正文只有显式调用时才会注入。

这意味着什么？可预测性极强。你知道 AI 在用哪个 skill，你知道它看到了什么指令。没有"AI 自己偷偷加载了一堆东西"的黑箱感。

技能目录也分得很细：repo 级、用户级、系统级，有明确的覆盖优先级。团队协作时，你可以在 repo 里放通用 skill，个人有自己的增强，系统级兜底。

---

## IV — Google Antigravity：平台化的自动分工

Google 的 Antigravity 是今年刚出的"Agentic IDE"，走的是另一条路。

### Sub-agent：系统内置分工

Antigravity 有 sub-agent，但不是你能自由配置的那种。

它的设计更像"系统自动帮你分工"：

+ Web research 的任务自动交给 `Browser Subagent`

+ 命令行操作自动交给 `Terminal Subagent`

+ 主模型专注高层推理，具体执行由专用模块负责

官方叫它 "Manager surface 可以 `spawn`/`orchestrate` 多 agents"。听起来很强，但这里有个关键限制：用户自定义能力不如 Claude Code 和 Codex 明确。

在 Google AI 开发者论坛里，有人明确问"能不能自建 sub-agent，用不同模型、不同 skill"。目前的答案是：公开材料里没看到这种支持。

如果你要的是"开箱即用的自动分工"，Antigravity 不错。如果你要的是"精细控制每个 sub-agent 的权限和行为"，可能还得等。

### Skill：解决上下文膨胀

Antigravity 的 skill 设计理念很清晰：解决 Context Saturation（上下文饱和）和 Tool Bloat（工具膨胀）。

它用"菜单元数据 + 按需加载"的方式，把工作流做成可插拔模块。你需要的时候加载，不需要的时候不占空间。

但相比 Codex 那种"显式/隐式双模式"，Antigravity 更偏隐式和平台化。你没有那么多手动控制的选项。

### 安全性争议

值得一提的是，Antigravity 的安全性已经被报道过一些问题——比如终端自动执行、提示注入风险。这会直接影响 sub-agent 的默认权限策略。

如果你的场景对安全敏感，这是要考虑的。

---

## V — Cursor：方向对，但还在早测

Cursor 是四个里面最特殊的。

它确实有 subagents 和 skills 的概念，文档页面也写了。但打开一看：仅 Nightly 渠道可用，功能可能不完整且会变化。

社区论坛里有人问"怎么在 skills 里用 subagent"，回复是"还在早期测试"。

这不是批评。Cursor 的产品方向明显在跟进 Claude Code 和 Codex 的能力——它甚至被传可以"从 Claude Code 和 Codex 导入 skills/sub-agents"。

但现阶段，如果你要的是稳定可预期的 sub-agent/skill 体系，Cursor 不是首选。它更适合那些愿意踩坑、愿意跟着 Nightly 迭代的人。

Cursor 的优势是：作为 IDE，它的集成体验可能最流畅。等 subagents 和 skills 稳定后，它在"开发者日常使用"这个场景可能会很强。

但那是以后的事。

---

## VI — 怎么选

说了这么多，给你一个决策框架。

### 如果你最在意"sub-agent 能不能强隔离、能不能精细控制权限"

选 Claude Code。

它的 sub-agent 是真正的一等公民。内置的 `Explore` 只读模式是硬隔离，不是靠 prompt 约束。你可以自定义 sub-agent 的权限、工具、甚至模型。

适合场景：代码审查、安全分析、需要"AI 只能看不能动"的任务。

### 如果你最在意"多代理协作过程可审计、可追溯"

选 OpenAI Codex（SDK 层）。

它的 trace 机制是四个里面最强的。每次 prompt、工具调用、`handoff` 都有记录，你可以回放整个协作过程。

适合场景：企业级使用、需要合规审计、团队协作需要复盘。

### 如果你最在意"skill 可以显式点名调用、可预测性强"

还是 Codex。

它是唯一一个把"显式 `$skill` 调用"和"隐式自动匹配"都做得很完整的。默认不注入正文、只有显式调用才加载指令的设计，让你完全知道 AI 在用什么。

适合场景：需要精确控制 AI 行为、不想有"黑箱感"的人。

### 如果你想"开箱即用，不想配置太多，让系统自动分工"

看看 Antigravity。

它的 sub-agent 是系统内置的自动分工，你不用自己编排。Skill 也是按需加载，不用操心上下文膨胀。

适合场景：想要"托管式"体验、不想深度配置的人。但要注意安全问题。

### 如果你是 Cursor 重度用户，愿意等

继续用 Cursor，关注 Nightly 更新。它的方向是对的，只是还没稳定。等 subagents 和 skills 成熟后，作为 IDE 的集成体验可能会最好。

---

## VII — 最后说几句

Sub-agent 和 skill 不是什么高深的概念。它们就是"让 AI 有分工"和"让 AI 有技能库"。

但大多数人不用这些东西，因为他们还在用 AI 聊天。

真正会用 AI Coding 的人，是在设计系统。他们在想：这个任务应该拆成几个 sub-agent？每个 agent 应该有什么权限？哪些能力应该抽成 skill 复用？

这才是 AI 编程的正确打开方式。

你可以继续和 AI 聊天。但如果你想真正提效 10 倍，你得开始编排。

四个工具各有侧重。Claude Code 最产品化，Codex 最工程化，Antigravity 最自动化，Cursor 最有潜力但还在路上。

选一个，开始搭你的第一个 sub-agent 和 skill 方案。

别只是聊天了。
