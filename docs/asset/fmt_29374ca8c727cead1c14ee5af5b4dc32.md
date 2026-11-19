
# AI 编码实践与项目规范指南

Generate a file named `AGENTS.md` that serves as a contributor guide for this repository. Your goal is to produce a clear, concise, and well-structured document with descriptive headings and actionable explanations for each section. Follow the outline below, but adapt as needed — add sections if relevant, and omit those that do not apply to this project.

## Document Requirements

- Title the document `Repository Guidelines`.
- Use Markdown headings (#, ##, etc.) for structure.
- Keep the document concise. 200-400 words is optimal.
- Keep explanations short, direct, and specific to this repository.
- Provide examples where helpful (commands, directory paths, naming patterns).
- Maintain a professional, instructional tone.

## Recommended Sections

### Project Structure & Module Organization

- Outline the project structure, including where the source code, tests, and assets are located.

### Build, Test, and Development Commands

- List key commands for building, testing, and running locally (e.g., `npm test`, `make build`).
- Briefly explain what each command does.

### Coding Style & Naming Conventions

- Specify indentation rules, language-specific style preferences, and naming patterns.
- Include any formatting or linting tools used.

### Testing Guidelines

- Identify testing frameworks and coverage requirements.
- State test naming conventions and how to run tests.

### Commit & Pull Request Guidelines

- Summarize commit message conventions found in the project’s Git history.
- Outline pull request requirements (descriptions, linked issues, screenshots, etc.).

(Optional) Add other sections if relevant, such as Security & Configuration Tips, Architecture Overview, or Agent-Specific Instructions.

## 渐进式开发

### 2.6.3 采用渐进式开发，而不是大需求一口气梭哈

根据实践经验，不推荐输出完方案后让 AI 一口气基于方案完成需求（非常小的需求除外），需求越大代码质量越烂。我的使用方式是，跟 AI 进行 结对编程，讨论具体的方案是什么，这个场景下的 最佳实践 是什么，拆解需求后，人工控制每一个块的代码生成。生成之后，可以咨询一下代码实现是否 优雅，是否 有重构空间，根据需要进行修改。

总结来说，有这些好处：

- 因为上下文窗口有限，任务粒度越小，AI 完成度越高
- 分步骤代码量便于做 Code Review

缺点就是，不够“自动”，比较费脑子。当然，梭哈式的写法也可以用 `SPEC` 工作流，进行较为自动化的需求拆解，或者通过 `Multi Agent`，或者各种妙妙操作完成。不过这是另一个课题，大家选择适合的方式即可。

个人感觉 `Vibe Coding` 方式不存在“完爆”，需要因场景而异；工具选择上也没有绝对的高下之分，我的朋友们就用什么的都有。最后还是应该以结果论英雄，对于个体来说，自己用着顺手就好。

![](img/b2280468e341c010de0a42ebfff4d434.png)

## 讲讲应用

上面说的都是一些准则类的经验，下面将结合具体场景，看一下如何把这些规则落地。以及帮助大家了解一下，除了纯粹的写代码，AI 还可以帮我们做什么。

### 3.1 快速熟悉项目 & 自然语言检索代码

讲解一下这个项目的每个 `module` 都是用来做什么的，并且给出包依赖关系图。我希望实现一个「什么什么」功能，需要修改的部分包括「这里」和「那里」，我的代码放在哪个包/目录下比较合适？仔细分析项目结构，并给出你的理由。我在找「某个某个」功能的实现，请帮我在仓库里搜寻，并给出它的核心具体代码位置和片段，并附上简洁的说明。

### 3.2 `PlantUML` / `Mermaid` 文本绘图生成

这里是打算让 AI 帮忙画一下某个功能的前后端交互流程图，先是把接口请求顺序在浏览器里进行了截图：

![](img/4bb19bdf11540c163278b38b7cacbc19.png)

然后把我的需求+我的判断（比如图里有一些接口实际上和这个需求并不相关）一起告诉 AI：

![](img/bdc60debe0518a105f14df135fb10887.png)

AI 在进行了一长串分析之后，也是给出了比较准确的流程图。

结果展示（做了模糊处理）：

![](img/1ec04be45c3a7ce0c7c0b5e82289ec85.png)

### 3.3 问题排查

这里是用真实遇到的一个问题举例子，这张图片是执行流程图的展示，当时遇到的问题是这个流程的产出结果出现了问题，大致定位到是图里的左侧节点有问题：

![](img/0184f734df05286a9d03b3c9c288a543.png)

由于对这个仓库并不熟悉，于是暂时交给 AI 帮忙翻代码排查：

![](img/51ea7c6223259acd9751cc8b2b95c8b6.png)

最后人工验证了一下，给出的问题分析和解决方案确实非常详细且准确。在对于项目不熟悉的情况下，这是一种快速进行检索和排查的有效手段。

### 3.4 补充网页信息到上下文

Web 是相对来说比较容易忽视，但是又非常好用的工具，可以通过 `@Web` 的进行添加，也可以直接粘链接进去。需要注意的是，需要进行权限认证的网页（如内网）无法直接被读取。

这里直接粘贴链接进去，让大模型帮忙总结文章内容，也可以基于它直接对任意网页进行问答。

![](img/609ef9fa4c8309735b69330a4c9982cc.png)

![](img/3e6bc919224cc675bfc40eb360c47b7a.png)

## 推荐的 Coding 方式

这里主要是一些实践性的探索，目前尚未进行推广，原因是还没有找到一种提升大、好维护、可移植的工具，团队同学的偏好也各不相同。当然，在这方面，团队内部已经有很多探索，目前在积极进行投入和尝试。

### 4.1 rule 的制定和优化

最基本应该包含你当前项目的技术栈使用，以及对应依赖版本；除此之外应该包含编码明确要求的规范。

![](img/9bcd2b9053effea174c24a39af8a26df.png)

注：使用一套统一的 rule，需要统一使用 `cursor`，可能不符合个人习惯。但是在使用其它 AI 编程工具时，维护一个项目规则文档，并在对话时手动添加至上下文，可以达到一样的效果。

### 4.2 文档

根目录下，存放以下内容

- `README.md`
  - 仓库门面，包含：简介、核心特性、快速开始、其它重要文件的说明和索引
- `CHANGELOG.md`
  - 需要发版的仓库可以考虑添加
  - 包含：
    - 新增功能
    - bug 修复
    - `breaking changes`（破坏性变更）
    - 注意事项等
- `ARCHITECTURE.md`
  - 架构复杂且需要多人协作的仓库可以考虑添加
  - 包含：
    - 整体架构
    - 模块划分
    - 核心逻辑流程图

在项目根目录中建立 `/docs` 目录

- 目的：让团队成员快速理解项目、高效协作
- 内容：兼顾实用性、准确性、易维护性
- 可以包含以下内容文件
  - 错误码体系
  - 错误码格式
  - 错误码分类
  - 详细场景
  - 异常处理指南
  - 可重试/不可重试
  - 异常抛出原则
  - 日志打印要求
  - 常见异常案例
  - 常用工具类

注：需根据项目内容修改，以上仅为个人推荐

### 4.3 注释和命名

1. 方法、参数等注释，尽量保证语义清晰、内容完整；
1. 可以考虑添加调用方的使用场景；
1. 适当添加行内注释，明确每个分支的场景和期望处理方式；
1. 命名需要清晰，无歧义；
1. AI 生成量 > 80%的文件，建议使用 `@author AI Assistant`，注明是 AI 生成，便于维护和统计。

### 4.4 安全合规问题

需要根据公司规定，合规使用。

### 4.5 推广方式

这里是在团队内部创建了一个代码仓库，作一下示例，主要维护这些内容（仍在优化中）：

- 范围确定
- 核心代码库梳理
- 区分给人看的文档（详细）和给 LLM 看（核心内容）的文档
- Docs 资产
  - 内容指定：`markdown` 文档内容 by project 定制
  - 生产：初始化文档，暂定为各应用 owner
- `Project Rule`
  - 内容指定：`rule` 内容 by project 定制，产出为 `markdown`；放在仓库中
  - 生产：初始化 `project rule` 的 `md` 文档，暂定为各应用 owner
- `User Rule`
  - 内容指定：产出通用部分，如团队规约，保证内容简洁；不放在仓库中
- 同步方式：（暂定）统一维护在一个新增仓库中，各应用中可以使用脚本进行一键同步到本地

## 参考文献

- 《`Cursor` 实战万字经验分享，与 AI 编码的深度思考》： https://www.cnblogs.com/echolun/p/18965624
- 《`Claude Code`（及 `Cursor`） 内部工作原理窥探》 ： https://www.superlinear.academy/c/share-your-projects/claude-code-cursor

![](img/a1990294bf5cbb00c4089765644dff3f.png)

## 团队介绍

本文作者库啵，来自淘天集团-用户&内容技术团队。本团队是双边电商平台的核心驱动力之一，通过精细化运营、数据智能和技术创新，实现用户增长与内容价值最大化。同时我们也在持续跟进前沿技术，探索如何让技术在真实业务场景中落地，形成可复用的增长方法论与工程实践。
