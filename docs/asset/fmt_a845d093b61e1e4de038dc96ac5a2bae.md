
# AI 短剧 | AI分镜师 Agent，Claude Agent+Skill方法

> 来源：[https://feicaiclub.feishu.cn/docx/NdmWdXpOJoSecZxTWGJcL2E4nfh](https://feicaiclub.feishu.cn/docx/NdmWdXpOJoSecZxTWGJcL2E4nfh)

![](img/71c4a1da1228df73b357d0a838259afa.png)

<https://player.bilibili.com/player.html?bvid=1y8qMBsER8&share_source=copy_web&vd_source=1adb0321bdfa6d9eb5d1bf5665ffc381>

## 用 Claude Agent Skill 搭建 AI 分镜师 - 提升分镜出图有效率

做 AI 视频的应该都有体会，分镜是最烧钱的环节之一。分镜数量本来就大，生图又是抽卡——单张图要控制人物、场景、光影的一致性，多张图之间还要基于剧本拆解出连贯的镜头序列。这两件事都不容易，大量时间和成本都浪费在重复抽卡上。

所以我们用 Claude 的 `Agent` + `Skill` 功能搭建了一个 AI 分镜师，核心思路是把影视分镜的方法论（清晰、简洁、连贯、分层渐进）写进 `Skill`，让 AI 按照专业标准去拆解剧情、设计镜头、生成提示词。你只需要上传剧本并确定视觉风格，剩下的节拍拆解、九宫格提示词、四宫格提示词、动态提示词就自动完成。

整体架构是一个制片人加三个 `Subagent`：分镜师负责生成，导演负责审核，动画师负责动态提示词。流程按分层渐进的原则推进——先锁定 9 个关键剧情点，再确定视觉基准，然后展开连续镜头，最后生成动态提示词。每个阶段完成后导演都会介入审核，不通过就打回修改，直到符合方法论标准。

---

### 核心功能模块

`Agent` + `Skill` 解耦设计

整套系统的核心思路是把「谁来做」和「怎么做」分开。`Agent` 只负责调度——制片人管流程，分镜师管生成，导演管审核，动画师管动态提示词。而专业知识全部放在 `Skill` 里：分镜方法论、验收清单、提示词模板、Gemini 出图指南。这样设计的好处是灵活，比如你想换一套自己的分镜方法论，只需要替换 `storyboard-methodology-playbook.md`，不用动 `Agent` 配置。

#### 分层渐进的生成逻辑

流程设计遵循分镜的「分层渐进」原则：先用节拍拆解锁定 9 个关键剧情点，再用九宫格确定视觉基准（人物、场景、光色），然后用四宫格把需要展开的关键帧拆成连续镜头，最后生成动态提示词用于视频模型。每一层都基于上一层的产物，信息逐层细化，一致性逐层继承。

#### 导演审核作为质量阀门

每个阶段完成后，导演会按 `review-checklist.md` 逐项检查。九宫格要查一致性、覆盖度、清晰度；四宫格要查动作连贯、轴线稳定、剪接风险；动态提示词要查长度、聚焦度、物理合理性。不通过就打回修改，循环直到通过。这个机制保证了产出不是「生成完就算」，而是「符合标准才算」。

#### `Subagent` 保持上下文

`Claude Code` 的 `Subagent` 默认每次调用都是全新上下文。但分镜是连续工作，分镜师生成四宫格时需要记得之前九宫格的设计意图。所以我们用 `.agent-state.json` 记录每个 `Subagent` 的 `agentId`，后续调用用 `resume` 恢复，让 `Agent` 能延续之前的完整对话。

#### 文档驱动而非对话驱动

所有产物都写入 `outputs/` 目录的独立文件，而不是只存在对话里。这意味着你可以随时中断、随时恢复，只要文件还在，输入 `/status` 就能看到进度。生成的提示词文件可以直接复制到 AI 出图工具使用。

> 注：这套方案能快速生成结构化的分镜提示词，但不要期待一步到位的完美结果。生成的提示词是起点，不是终点——你仍然需要根据实际出图效果去调整、优化、迭代。另外，这套流程涉及剧情拆解、关键帧提取、连续镜头设计，对模型的理解能力要求比较高，建议使用 Claude Opus 或 Sonnet 级别的模型。模型能力和输出质量是正相关的，用更强的模型，生成的提示词质量也会更高。

### 在 Claude Code 中配置

请按照以下步骤配置分镜师系统，配置完成后，你的项目结构应该是这样：

```
project/
├── script/                              # 剧本源文件（用户上传）
│   └── ep01-xxx.md
├── outputs/                             # 生成产物（自动创建）
│   ├── beat-breakdown-ep01.md
│   ├── beat-board-prompt-ep01.md
│   ├── sequence-board-prompt-ep01.md
│   └── motion-prompt-ep01.md
├── .agent-state.json                    # Agent 状态记录（自动创建）
└── .claude/
    ├── CLAUDE.md                        # 制片人主Agent配置（必须创建）
    ├── agents/                          # Subagent 配置（必须创建）
    │   ├── storyboard-artist.md
    │   ├── director.md
    │   └── animator.md
    └── skills/                          # 技能包（必须创建）
        ├── film-storyboard-skill/       # 分镜师技能包
        │   ├── SKILL.md
        │   ├── storyboard-methodology-playbook.md
        │   ├── gemini-image-prompt-guide.md
        │   └── templates/
        │       ├── beat-breakdown-template.md
        │       ├── beat-board-template.md
        │       └── sequence-board-template.md
        ├── storyboard-review-skill/     # 导演技能包
        │   ├── SKILL.md
        │   └── review-checklist.md
        └── animator-skill/              # 动画师技能包
            ├── SKILL.md
            ├── motion-prompt-methodology.md
            └── templates/
                └── motion-prompt-template.md
```

1. 创建主 Agent 配置

1. 在 `.claude/` 目录下创建 `CLAUDE.md` 文件

1. 粘贴制片人角色定义、工作流程、指令集

1. 包含项目状态检测、`Subagent` 机制、四个阶段的流程定义

1. 创建 Subagent 配置

1. 在 `.claude/agents/` 目录下创建三个 Agent 文件

1. `storyboard-artist.md`：分镜师，负责生成节拍拆解和分镜提示词

1. `director.md`：导演，负责审核所有产出

1. `animator.md`：动画师，负责生成动态提示词

1. 创建分镜师技能包

1. 在 `.claude/skills/` 目录下创建 `film-storyboard-skill/` 文件夹

1. 创建 `SKILL.md` 定义技能包配置和调用规则

1. 创建 `storyboard-methodology-playbook.md` 包含完整的分镜方法论

1. 创建 `gemini-image-prompt-guide.md` 包含提示词写法指南

1. 在 `templates/` 下创建节拍拆解、九宫格、四宫格三个模板文件

1. 创建导演技能包

1. 在 `.claude/skills/` 目录下创建 `storyboard-review-skill/` 文件夹

1. 创建 `SKILL.md` 定义审核流程

1. 创建 `review-checklist.md` 包含各阶段的验收清单

1. 创建动画师技能包

1. 在 `.claude/skills/` 目录下创建 `animator-skill/` 文件夹

1. 创建 `SKILL.md` 定义技能包配置

1. 创建 `motion-prompt-methodology.md` 包含动态提示词方法论

1. 在 `templates/` 下创建动态提示词模板

#### 重要配置细节

+ `Subagent` 调度：`CLAUDE.md` 定义了调度规则，生成任务由分镜师或动画师执行，审核任务全部由导演执行

+ Resumable 机制：每个 `Subagent` 首次调用后记录 `agentId` 到 `.agent-state.json`，后续调用用 `resume` 恢复上下文

+ 分层渐进流程：严格按节拍拆解 → 九宫格 → 四宫格 → 动态提示词的顺序推进，每阶段基于上游产物

+ 导演审核循环：每个阶段完成后导演自动审核，`FAIL` 则打回修改，循环直到 `PASS`

+ 模板约束：`templates/` 中的格式是强制性的，所有输出必须按照模板结构

+ 方法论驱动：分镜师和导演共享 `storyboard-methodology-playbook.md`，确保生成和审核标准一致

+ 一致性继承：四宫格必须继承九宫格对应格的人物、场景、光色描述

+ 多集支持：通过文件名集数标识（如 `ep01`）区分不同集的产物

+ 文档驱动：所有产物写入 `outputs/` 目录，对话中断后可通过 `/status` 恢复进度

+ 项目状态检测：启动时自动扫描 `script/` 和 `outputs/`，检测每集进度并路由到对应阶段

### 核心设计理念

```
┌─────────────────────────────────────────────────────────────────────────┐
│  CLAUDE.md（制片人）                                                     │ ← 流程层：控制生产流程
│                                                                         │    定义工作流、指令集、Subagent 调度规则
├─────────────────────────────────────────────────────────────────────────┤
│  Subagents（子Agent）                                                    │ ← 执行层：分工协作
│  ├─ storyboard-artist          生成节拍拆解、九宫格、四宫格提示词        │
│  ├─ director                   审核所有产出，PASS/FAIL 把关              │
│  └─ animator                   生成动态提示词                            │
├─────────────────────────────────────────────────────────────────────────┤
│  Skills（技能包）                                                        │ ← 知识层：提供专业能力
│  ├─ film-storyboard-skill      分镜方法论、提示词写法、模板              │
│  ├─ storyboard-review-skill    验收清单、审核标准                        │
│  └─ animator-skill             动态提示词方法论、模板                    │
├─────────────────────────────────────────────────────────────────────────┤
│  outputs/                                                               │ ← 文档层：结构化存储
│  ├─ beat-breakdown-ep01.md     节拍拆解表                                │
│  ├─ beat-board-prompt-ep01.md  九宫格提示词                              │
│  ├─ sequence-board-prompt-ep01.md  四宫格提示词                          │
│  └─ motion-prompt-ep01.md      动态提示词                                │
└─────────────────────────────────────────────────────────────────────────┘
```

#### 工作流程

1. 上传剧本 — 将剧本/梗概放入 `script/` 目录

1. 配置项目 — 确定视觉风格、目标媒介、画幅比例

1. 节拍拆解 — `/breakdown` 分镜师从剧本中提取关键剧情点，选出 9 个锚点

1. 导演审核 — 检查完整性、清晰性、选点合理性，`FAIL` 则打回修改

1. 九宫格提示词 — `/beatboard` 基于锚点生成 Beat Board，确定视觉基准

1. 导演审核 — 检查清晰度、覆盖度、一致性，`FAIL` 则打回修改

1. 四宫格提示词 — `/sequence` 展开连续镜头序列，继承九宫格的人物/场景/光色

1. 导演审核 — 检查动作连贯、轴线稳定、剪接风险，`FAIL` 则打回修改

1. 动态提示词 — `/motion` 动画师生成用于视频模型的 Motion Prompt

1. 导演审核 — 检查简洁性、聚焦度、物理合理性，`FAIL` 则打回修改

1. AI 出图 — 用九宫格提示词生成关键帧，用四宫格提示词展开镜头序列

1. AI 生成视频 — 用动态提示词驱动图生视频模型

#### 关键特性

+ `Agent` + `Skill` 解耦 — `Agent` 负责调度，`Skill` 负责专业知识，可独立替换

+ `Subagent` 可恢复 — 通过 `.agent-state.json` 保持上下文连续，中断后可恢复

+ 导演审核循环 — 每阶段自动审核，不通过就打回修改，直到符合标准

+ 分层渐进流程 — 先锁定关键点，再展开细节，信息逐层细化

+ 模板约束 — 严格遵循 `templates/` 定义的格式，保证输出规范统一

+ 方法论驱动 — 分镜师和导演共享同一份方法论，生成和审核标准一致

+ 一致性继承 — 四宫格必须继承九宫格的人物、场景、光色描述

+ 多集支持 — 通过文件名集数标识区分不同集的产物

+ 文档驱动 — 所有产物写入 `outputs/`，对话中断后可通过 `/status` 恢复进度

+ 项目状态检测 — 启动时自动扫描目录，检测每集进度并路由到对应阶段

## 模型选择说明

这套方案依赖 `Claude Code` 的 `Agent` + `Skill` 功能来实现自动化分镜流程。但考虑到 Claude 账号的使用限制，你可以选择在 `Claude Code` 中配置其他大语言模型的 API，比如 Gemini 3 Pro、Kimi K2、GLM 4.7 Minimax M2等。这样可以使用 `Claude Code` 的完整项目管理和 `Skill` 调用能力，但实际分镜工作由你配置的模型完成，不会占用 Claude 的使用额度。

需要注意的是，不同模型在理解分镜方法论、执行复杂指令、保持长上下文一致性等方面的能力存在差异。从实测效果来看：

+ 理解能力：模型对 `storyboard-methodology-playbook.md` 中分镜法则的理解深度会影响节拍拆解的准确性，比如能否准确识别叙事拐点、判断镜头目的

+ 执行能力：模型遵循模板格式、应用叙事描述式提示词写法的严格程度会影响输出质量

+ 一致性维护：模型在多阶段生成过程中保持人物外观、场景光色、镜头连贯的能力会影响最终分镜的可用性

建议选择支持长上下文的模型。初次使用时可以先用一个短剧本测试分镜效果，确认该模型能否满足你的质量要求，再进行完整项目。如果发现生成内容偏离方法论标准较多，可以尝试更换模型或调整配置。

## 👀 先睹为快
