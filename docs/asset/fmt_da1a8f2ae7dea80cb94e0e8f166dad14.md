
# OpenSpec，轻量级Spec Coding，1-N的神！

> 来源：`https://xatc0v8uz5m.feishu.cn/docx/Fnr7dFeuQocvN3xNjZAc1Bbrnod`

> 🔗 原文链接：`https://mp.weixin.qq.com/s/CpO8DdxRKpjJP0quYj81XQ`

> ⏰ 剪存时间：2025-10-31 00:28:20 (UTC+8)

> ✂️ 本文档由 [飞书剪存](https://www.feishu.cn/hc/zh-CN/articles/606278856233?from=in_ccm_clip_doc) 一键生成

原创 我姚学AI 我姚学AI 2025年10月25日 16:35

> 命令大家好，我是姚路行，一个爱搞AI的大厂程序员，也是一个90后奶爸

关注下方

![](img/953c8e746267da6c70fccdce6aafcd65.png)

公众号，回复【claude】免费领取价值799的 Claude Code 超全学习资料

之前的文章中，我给大家带来了一个 Github 上最火的 Spec Coding 开源项目，Spec-Kit 的全网最保姆级的使用教程！保姆级 Spec Kit 教程来了！Spec Coding 如此简单！

很多同学用过之后都说非常不错，但也有一些不同的声音，提到了 Spec-Kit 0-1 表现不错，但在已有项目的迭代中就表现的不那么让人满意了。

今天给大家分享另外一个非常强、轻量且更适合 1-N 的 Spec Coding 工具—— OpenSpec！

![](img/cf744335fc60f35a52c0fb9b7d5baa8f.png)

💡

`https://github.com/Fission-AI/OpenSpec`

![](img/aab2eefb7f232b0d734c99abfff20d32.png)

## OpenSpec深入理解

### OpenSpec是什么？

OpenSpec 是一个 规范驱动开发（Spec-Driven Development）工具，它通过结构化的工作流让 AI 编码助手能够：

- 深度理解项目的技术栈、架构和开发规范；
- 自动追踪每个功能变更的完整生命周期；
- 保持代码与文档的实时同步；
- 以可追溯方式管理项目演进历史。

### OpenSpec的核心优势

- 轻量级：简单的工作流程，最少的配置；
- 适合迭代开发：特别擅长修改现有功能（1→N），而不仅是新项目（0→1）；
- 变更追踪：proposals、tasks、specs 三位一体，归档后自动合并到规范；
- 工具兼容性强：支持 Claude Code、Cursor、Windsurf、Codex 等 10+ 主流 AI 工具。

### OpenSpec的核心理念：双文件夹模型

OpenSpec 采用 “两大部分 + 四个步骤” 的工作流：

#### 两大核心部分

OpenSpec 的独特之处在于将 “当前的项目规范” 与 “提案更新” 分离：

```
openspec/
├── project.md      # 项目描述、技术栈、开发约定
├── specs/              # 当前项目的规范状态（source of truth）
│   └── auth/
│       └── spec.md     # 认证模块规范
│
└── changes/            # 变更提案与演进记录
    ├── add-2fa/        # 正在进行的变更
    │   ├── proposal.md # Why（为什么）、What（做什么）、Impact（影响）
    │   ├── tasks.md    # 任务清单
    │   ├── design.md   # 技术实现方案（可选）
    │   └── specs/      # 规范变更增量（delta）
    │       └── auth/
│           └── spec.md  # 标记 ADDED/MODIFIED/REMOVED
    │
    └── archive/        # 已完成的变更历史
        └── add-profile-filters/
```

这种 “双文件夹模型” 让状态和差异保持分离，特别适合跨模块的复杂变更，以及老项目迭代。

#### 四个标准步骤

```
┌────────────────────┐
│  1. 草拟提案        │  分享意图给 AI
│  (Draft Proposal)  │
└────────┬───────────┘
         │
         ▼
┌────────────────────┐
│  2. 审查对齐        │◄──── 反馈循环 ──────┐
│  (Review & Align)  │                    │
└────────┬───────────┘                    │
         │ 批准计划                        │
         ▼                                │
┌────────────────────┐                    │
│  3. 实施任务        │────────────────────┘
│  (Implement)       │  AI 编写代码
└────────┬───────────┘
         │ 部署变更
         ▼
┌────────────────────┐
│  4. 归档更新        │  合并到 specs/
│  (Archive)         │
└────────────────────┘
```

## OpenSpec安装与初始化

### OpenSpec安装

言归正传，首先是如何安装 OpenSpec，运行以下命令即可：

```
npm install -g @fission-ai/openspec@latestst
```

然后打开控制台，输入 `openspec --version`，如果可以正常显示版本号，则安装成功了。

### OpenSpec初始化

OpenSpec 和 Spec-Kit 无异，都是支持两种方式。

#### 方法一：在已有项目下初始化

你可以手动创建好一个新的项目，然后到这个项目目录下，运行以下命令进行初始化：

```
openspec init
```

#### 方法二：自动创建项目并初始化

你也可以直接运行以下命令，通过 `PROJECT_NAME` 参数，OpenSpec 会自动创建名为 `PROJECT_NAME` 的项目，并自动完成初始化。

```
openspec init <PROJECT_NAME>
```

#### 选择 AI 编程工具

然后需要选择 AI 编程工具，这步的话你平时用的是哪种，选择对应的就好，我选择 Claude Code。

![](img/e897056ccadec3cd10bd68b29b225b6f.png)

接着就是等待执行完成即可，OpenSpec 也给出后续操作的建议。

![](img/5fbf743d50e8c06842aba8c7ab48eda0.png)

流程完成之后，你会看到项目里多了一些文档，不过看起来还是比 Spec-Kit 轻量了不少的。

![](img/77b20f6c007bfe9d3c135e49f77b9c02.png)

初始化完成之后就是如何使用了，为了和之前 Spec-Kit 的案例做一下效果对比，我们也以写一个功能复杂的电商系统为例。

## OpenSpec实战

### 前置步骤：项目理解并补充开发指南

关键步骤：填充 `project.md`，主要包括项目简介和目标、技术栈等等作为后续 AI 工作的 “指南”。

新项目的话，这步相对没那么重要，因为没内容，所以总结出来的都差不多，但在老项目使用 OpenSpec，这步也是开发过程中非常重要的一步。

这步没有命令，自然语言即可，根据官方建议的提示词初始化，在 Claude Code 中输入：

```
Please read openspec/project.md and help me fill it out with details about my project, tech stack, and conventions
```

如果你觉得英文不容易看，想让生成的指南，以及后面的 OpenSpec 过程中返回中文，可以在 Claude Code 加上这段：

```
后续所有对话都以中文回复给我，定为本项目的指南

Please read openspec/project.md and help me fill it out with details about my project, tech stack, and conventions
```

OpenSpec 会自动理解项目并开发指南，这些原则和指南会指导所有后续的开发工作。

![](img/dacfff31f1241ba5a84eed6e3b1a7cc9.png)

可以看到 `openspec/project.md` 更新了，如果有不满意的地方也可以手动修改。

![](img/e32a503d0d0042d1bd971b3420588f09.png)

我在老项目里执行了一下，发现 OpenSpec 对已有功能理解总结的非常好，可能这也是 OpenSpec 在 1-N 表现更好的一大原因了！

### 第一步：起草功能变更提案

关键步骤：通过执行 `/openspec:proposal` 命令描述我们的提案。

![](img/1b611a368759eaea2634c4dac3743ae3.png)

`/openspec:proposal` 这个命令融合了提案起草、提案检验与完善、技术方案设计、任务拆分等步骤。

他会针对性的问你一些提案相关的问题，按照你的需求选择并完成提交。

![](img/7e3fe644a2192597f8bcfa5c0be9e69c.png)

接着就是拆分出如何生成这次提案相关文档的任务并执行了。

![](img/4b6da6b4baac8d64dc4af5ba428349c2.png)

一顿运行之后，`changes` 目录下生成了全新的提案目录，并且 `specs` 目录下有非常多的子需求目录。

#### 提案文档

里面包括了 Why：为什么要改；What：做什么改动；Impact：影响范围

![](img/18770a077a16942752f796338ed1c181.png)

#### 技术方案文档

![](img/0abca773a9ee8bd4fe94ca6a7da55a2a.png)

#### 任务拆分文档

![](img/27ec013b098f2b98d2b417fad9ebaa6e.png)

### 第二步：实现提案

关键步骤：通过执行 `/openspec:apply` 命令指定某一提案，并完成实现。

![](img/22b52906cef329628a67e310255d4685.png)

在正式开始实现提案前，OpenSpec 有个非常有意思的交互，因为他分析整个任务有点庞大，他想知道你真正的实现意图：

是想实现一个最小验证版本还是想实现完整版本，是提供一个实现指南还是说指导我自己实现。

这个实现意图的询问真的牛逼！

![](img/e81f46da01ba3156718c6c2f6ef9174d.png)

为了快速实现验证，我选择了 A，接着就是等待执行了，执行了哪些任务 OpenSpec 会自动打勾。

![](img/6d9e85a4b507899bb932422ae94d611e.png)

等待所有步骤完成。

![](img/b07df1ac77adda83f69adbecbeb1bf66.png)

非常快速地完成了第一版迷你验证版，让我们来看看效果！功能没啥问题

[🎦 点击观看视频](https://mpvideo.qpic.cn/0bc3seaekaaa6uacovx45bufbeodiwiqaria.f10002.mp4?dis_k=4b6ca0a96966694c78870712e8468bc1&dis_t=1761841647&play_scene=10120&auth_info=J+PmwNYJYGFv1NWYswILNGdWfGhuSENjO09wZDBUYTEWeEhfbwM9Dg17I28NIxwObQ==&auth_key=664f194a1a658d2a5cb3496b93b69086&vid=wxv_4224141495630266372&format_id=10002&support_redirect=0&mmversion=false)

如果版本验证没问题，符合你的预期，想继续实现其他功能的话，再重新执行 `/openspec:apply` 命令，选择想要执行的任务。

![](img/9eb9b7e0e65c8e5e3ecfbd84089cd3c9.png)

也可以每次选择一个小的任务执行，然后快速验证，直到完成整个提案。

### 第三步：归档提案

关键步骤：通过执行 `/openspec:archive` 命令完成功能归档。

![](img/e36f208d2907a8741c9d7976e5c0800b.png)

OpenSpec 会自动：

1. 将 `changes/add-ecommerce-system` 移动到 `changes/archive/`；
2. 变更合并到 `specs/` 中的规范文档。

![](img/c8601b9a0eb881e1d717420bf1d71268.png)

至此，一个完整的功能变更流程结束，所有文档都已同步更新！

### 后置步骤：重复执行

如果说还有最后一步，那就是重复执行第一步到第三步，直到完成到你满意为止！

### 老项目迭代

这里我也用 OpenSpec 试了一个老项目功能迭代，流程和上述一样的，对比 Spec-Kit，OpenSpec 的生成效果表现果然更好！

## OpenSpec vs Spec-Kit

这个可能是最多人关心的问题了，OpenSpec 和 Spec-Kit 有什么区别？该怎么做出选择？接下来分方向做个对比！

### 使用流程对比

OpenSpec 使用起来更加简洁高效！

使用 Spec-Kit 的流程（7 步）：

```
specify init .
    ↓
/speckit.constitution   # 定义编码规范、测试覆盖率、UI 风格
    ↓
/speckit.specify        # 编写需求（用户可以新增、编辑、删除待办事项）
    ↓
/speckit.clarify        # 查缺补漏
    ↓
/speckit.plan           # 制定技术实现方案
    ↓
/speckit.tasks          # 拆分任务
    ↓
/speckit.analyze        # 整体审核
    ↓
/speckit.implement      # 执行实现
```

使用 OpenSpec 的流程（4 步）：

```
openspec init .
    ↓
自然语言提示词描述即可      # 项目理解与生成指南
    ↓
/openspec:proposal      # 添加待办事项列表功能
    ↓
/openspec:apply         # 实施变更
    ↓
/openspec:archive       # 完成后归档
```

### 各维度详细对比

| 维度 | spec-kit | OpenSpec |
| --- | --- | --- |
| 流程复杂度 | 7 个阶段（宪章→需求→澄清→方案→任务→审核→实施） | 4 个阶段（项目理解→提案→实施→归档） |
| 适用场景 | 0→1 新项目搭建 | 1→N 现有项目迭代 |
| 变更管理 | 分散在多个需求文件夹，难以追踪 | 每个功能一个文件夹，集中管理 |
| 文档同步 | 需要手动维护 | 归档时自动合并到 `specs/` |
| 规范校验 | 无强制校验 | 强制使用 `MUST`/`SHALL` 等关键词 |
| 工具兼容 | 支持 Claude Code、Cursor 等 10+ 工具 | 支持 Claude Code、Cursor 等 10+ 工具 |
| 学习曲线 | 陡峭，阶段多 | 平缓，流程简洁 |
| 官方支持 | GitHub 官方维护 | Fission AI 维护，社区活跃（5k+ Star） |
| 用户反馈 | 需要耐心和时间投入 | 简化流程，几乎不需要用户干预 |

### 选型建议

| 场景 | 推荐工具 | 理由 |
| --- | --- | --- |
| 全新项目，需要快速搭建架构 | Spec-Kit | 提供丰富的模板和最佳实践 |
| 现有项目，需要持续迭代 | OpenSpec | 轻量级，变更追踪完善 |
| 多人协作，需要追溯变更历史 | OpenSpec | 每个功能独立文件夹，清晰可追溯 |
| 涉及多个规范模块的复杂变更 | OpenSpec | 双文件夹模型，状态和差异分离 |
| 单次性的原型验证 | Spec-Kit | 快速生成初始规范 |

### 对比总结与最佳实践

总结一句话：

- Spec-Kit 是 “建房子”（0→1）
- OpenSpec 是 “住房子 + 持续装修”（1→N）

最佳实践建议：用 Spec-Kit 新起项目，用 OpenSpec 迭代项目。Spec Coding 只是一个范式而已，总结一个适合自己的 Spec Coding 实践才是最佳实践。

## 写在最后

### 总结

OpenSpec 为 AI 辅助开发带来了一套轻量级、结构化、可追溯、自动化的范式。它不是取代你的开发工作，而是让 AI 成为真正理解你项目的 “老搭档”。
