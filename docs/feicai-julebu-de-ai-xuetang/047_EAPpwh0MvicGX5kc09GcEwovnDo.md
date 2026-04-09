# AI 剧本 | Claude Code 搭建短剧编剧团队

> 来源：[https://feicaiclub.feishu.cn/docx/SFUMdGswQouterxI3mjc8hG8nkg](https://feicaiclub.feishu.cn/docx/SFUMdGswQouterxI3mjc8hG8nkg)

![](img/6115a3890bc18dbc248e106b3dd424b1.png)

<https://player.bilibili.com/player.html?bvid=1PKWEzvE7v&share_source=copy_web&vd_source=1adb0321bdfa6d9eb5d1bf5665ffc381>

# Claude Code 搭建短剧编剧团队 - 多Agent协作创作方案

这套方案主要解决一个问题：用AI写短剧时，生成的内容往往不符合短剧的商业逻辑和创作规律。单纯一次性输出很难把握短剧特有的节奏感、爽点分布和付费转化设计。

所以我们通过 Claude Code 搭建了一个编剧团队。主编剧负责创作，Script Aligner 审核是否符合短剧标准，Script Recorder 记录创作过程。通过 Output Style 把原本用于编程的 Claude Code 改造成懂短剧节奏的编剧，同时保留它的文件管理和自动化能力。

工作流程基于 ReAct 框架：主编剧创作初稿，Aligner 审核是否符合预设的短剧创作标准，不达标就提出修改建议，主编剧据此优化，循环直到通过。整个过程的决策和修改都会被 Recorder 实时记录，形成完整的创作记忆。

这个方案让 AI 创作从一次性生成变成了多轮优化，能稳定产出符合短剧风格的剧本初稿。不过要注意，最终还需要人工润色，而且 Token 消耗需要提前考虑。

---

# **核心功能模块**

**/输出风格替换**

通过 Output Style 机制完全替换 Claude Code 的系统提示词，从编程助手转换为短剧编剧。系统保留文件读写、脚本执行等核心能力，但输出风格完全改变：对话控制在15字内、场景描述极简、节奏紧凑密集。风格文件采用三段式结构（Description、Instruction、Behavior），可在项目级或全局级生效，支持随时切换。

**/分阶段创作流程**

创作严格遵循"大纲→人物→集目录→剧本正文"的流程，每个阶段对应独立文档。主编剧完成初稿后，必须经过 Script Aligner 审核才能写入文件。这种阶段性管理确保前后一致性，避免人物设定与剧情脱节、集数规划与大纲不符等常见问题。

**/循环优化机制**

基于 ReAct 框架实现多轮优化。Script Aligner 会根据预设的短剧创作法则（如爽点分布、付费转化、节奏控制）审核内容，不符合标准就返回具体修改建议。主编剧据此调整后再次提交，循环直到通过。每轮审核都有明确的 PASS/FAIL 状态，确保质量达标。

**/创作进度追踪**

Script Recorder 实时记录创作全过程，维护 script.progress.md 作为项目记忆。包括已完成集数、创作决策、修改历史、伏笔管理等关键信息。系统会自动计算创作进度百分比，支持断点续写。即使对话被清空，也能通过 /status 命令快速恢复上下文。

**/文档驱动架构**

所有创作内容分文件存储：outline.md（大纲）、character.md（人物）、episode_index.md（集目录）、EP-XX.md（各集剧本）。这种结构化存储解决了上下文限制问题，支持20万Token后的自动压缩场景。文档成为真实的创作资产，而非临时对话记录。

> 注：这套系统的核心在于将 Claude Code 的工程能力改造为创作能力，通过多Agent协作实现质量把控。Script Aligner 基于内置的短剧创作法则进行审核，Script Recorder 通过语义识别自动记录关键信息。整个流程通过 CLAUDE.md 配置文件定义，支持 /outline、/character、/catalog、/write 等命令调用，特别适合需要批量产出、风格一致的短剧剧本创作。

---

# **配置短剧编剧团队系统**

请按照以下步骤配置短剧编剧团队，让你的 Claude Code 成为专业的剧本创作系统。配置完成后，你的项目结构应该是这样：

```
project/
├── CLAUDE.md                    # 项目规则和创作流程（必须创建）
├── script.progress.md           # 创作进度记录（自动生成）
├── outline.md                   # 剧情大纲（创作后生成）
├── character.md                 # 人物小传（创作后生成）
├── episode_index.md            # 集目录（创作后生成）
├── .claude/
│   ├── output-styles/
│   │   └── short-drama-cn.md   # 短剧创作风格（配置创建）
│   └── agents/
│       ├── script-aligner.md   # 审核员Agent（配置创建）
│       └── script-recorder.md  # 记录员Agent（配置创建）
└── episodes/
    ├── EP-01.md                # 各集剧本（创作后生成）
    └── ...
```

## 配置步骤：

1.  **设置输出风格**

+   输入 /output 指令，选择 "Create custom style"

+   选择 Project Level（仅在当前项目生效）

+   将 short-drama-cn 输出风格内容粘贴进去

+   使用 /output-style short-drama-cn 激活风格

1.  **创建项目规则**

+   在根目录创建 CLAUDE.md 文件

+   粘贴完整的项目规则内容，包含创作流程、短剧法则、自动触发规则

+   确保包含所有创作模板（outline、character、episode_index、episodes）

1.  创建 Script Aligner

+   输入 /agents 指令，选择 "Create New Agent"

+   层级选择 Project，名称设为 script-aligner

+   粘贴 Script Aligner 的完整系统提示词

+   Description 设置为："剧本质量检查员，审核内容是否符合短剧创作标准"

+   工具选择 All Tools，模型选择 Sonnet

1.  创建 Script Recorder

+   再次输入 /agents，创建第二个Agent

+   名称设为 script-recorder

+   粘贴 Script Recorder 的系统提示词

+   Description 设置为："创作进度记录员，维护项目记忆和创作历史"

+   同样选择 All Tools 和 Sonnet 模型

## 重要配置细节：

+   **Output Style** 必须在激活后才生效，通过 /output-style 可以随时切换回默认或其他风格

+   **CLAUDE.md** 是整个系统的核心，定义了创作流程、审核标准、自动化规则，必须完整配置

+   **Agent 描述** 要与 CLAUDE.md 中的触发规则对应，确保自动调用的准确性

+   **短剧法则** 已内置在 CLAUDE.md 中，包括节奏控制、爽点分布、付费转化等标准

+   **文档模板** 必须保持格式一致，系统会严格按照模板结构创作和审核

+   **自动触发** 主要通过语义识别实现，创作后自动审核，通过后自动记录

+   **Token 预算** 每轮审核都会消耗Token，90集剧本预计需要较大预算

---

# 🎯 核心设计理念

传统的 AI 剧本创作中，模型会一次性生成完整内容，缺乏对短剧特有规律的理解。生成的剧本往往节奏拖沓、爽点稀疏，不符合短剧的商业逻辑。这套系统改变了创作模式：

+   **多轮优化机制** - 不再是一次性输出，而是通过 ReAct 框架循环优化，直到符合短剧标准

+   **硬性规则约束** - 在 CLAUDE.md 中内置短剧创作法则（如第20集付费点、每集必有冲突）作为审核标准

+   **分工协作体系** - 主编剧专注创作，Aligner 专注审核，Recorder 专注记录，各司其职

+   **文档驱动架构** - 所有内容分文件存储，解决上下文限制，支持断点续写

## 🏗️ 系统架构

```
┌─────────────────────┐
│  Output Style       │ ← 风格层：定义短剧创作风格
├─────────────────────┤
│  CLAUDE.md          │ ← 规则层：创作流程和审核标准
├─────────────────────┤
│  Main Agent         │ ← 创作层：负责剧本创作
│  ├─Script Aligner   │ ← 审核层：质量把控
│  └─Script Recorder  │ ← 记录层：进度追踪
├─────────────────────┤
│  outline.md         │ ← 大纲文档
│  character.md       │ ← 人物文档
│  episode_index.md   │ ← 目录文档
│  episodes/*.md      │ ← 剧本文档
│  script.progress.md │ ← 进度记录
└─────────────────────┘
```

## 工作流程：

1.  **需求收集** - 主Agent询问剧本方向、结局类型、核心爽点等创作需求

1.  **初稿创作** - 根据需求和模板生成初版内容（大纲/人物/剧本）

1.  **审核循环** - Script Aligner 根据短剧法则审核，返回 PASS/FAIL 状态和修改建议

1.  **优化调整** - 主Agent根据反馈修改，再次提交审核，循环直到通过

1.  **文档写入** - 审核通过后，将内容写入对应的结构化文档

1.  **进度记录** - Script Recorder 自动记录创作决策、修改历史、完成状态

## 关键特性：

+   **阶段性创作** - 严格按照"大纲→人物→目录→正文"流程，确保前后一致

+   **质量门禁** - 必须通过 Aligner 审核才能写入文档，确保符合短剧标准

+   **伏笔管理** - Recorder 追踪伏笔埋设与回收，维护剧情连贯性

+   **进度可视** - 实时计算创作进度百分比，支持随时查看状态

+   **断点续写** - 基于文档驱动，即使清空对话也能继续创作

+   **模板规范** - 所有文档遵循统一模板，便于管理和复用

订阅年度会员解锁

剩余教程内容

Output Style

CLAUDE.md 项目规则

Aligner/Recorder System Prompt

...

# **💰 订阅社群年度会员**

> **💰 加入废才俱乐部**

> 加入社群，一次性解锁所有AI实战攻略。

> 包括现有的全部内容——提示词框架、Agent搭建教程、Skill开发攻略等，以及未来每一期视频更新的配套资料。不再零散获取，系统地学习和应用。

> ⚠️ 购买提醒：请仔细浏览页面内容，谨慎购买，不退不换。如未看到支付按钮，请使用桌面端网页浏览器访问。

> **光看文字不够？实操遇到卡点？**

> 加入社群后，你还可以进入专属交流群，获得废才老师的亲自指导。在这里，你不仅能与数百位AI实践者交流经验，更能随时提问，解决实操中遇到的问题。

> 我们期待你成为社群的第 n+1 位成员！

> [社群微信群入口](https://feicaiclub.feishu.cn/wiki/JBNXwZ5QZidRtuk1HCCcvgZmnNg?from=from_copylink)（戳这里，跳转去社群微信群页面扫码入群）