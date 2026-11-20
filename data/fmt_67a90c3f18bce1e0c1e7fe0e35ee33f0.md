
# 最强Coding Agent: Claude Code权威实践指南

> 来源：[https://huchegames.feishu.cn/docx/RfWPdZvXpoBX5NxRRcJc42YonIu](https://huchegames.feishu.cn/docx/RfWPdZvXpoBX5NxRRcJc42YonIu)

> 🔗 原文链接：[https://zhuanlan.zhihu.com/p/1920263182062163086](https://zhuanlan.zhihu.com/p/1920263182062163086)

> ⏰ [木牛流码](https://www.zhihu.com/people/brisyramshere) 西安交通大学 生物医学工程硕士 2025-06-23 06:35

## 引言

> 近期`Claude Code`大受网上开发者的追捧，是公认的最强`Agentic Coding`工具。本文基于Anthropic官方工程博客发布的《Claude Code Best Practices》，旨在为开发者提供一套经过验证的`Claude Code`使用模式与技巧。`Claude Code`是一款专为辅助编程设计的命令行工具，它以其低层级、无固定模式的设计理念，为开发者提供了一个灵活、可定制且安全的强大助手。然而，这种灵活性也为初次接触"代理式编程"（Agentic Coding）的工程师带来了学习曲线。

本文的核心价值在于，系统性地梳理了Anthropic内部团队及外部开发者在不同代码库、语言和环境中总结出的高效工作流与优化策略。无论你是希望提升日常编码效率，还是探索自动化基础架构的复杂应用，本文都将为你提供坚实的起点，帮助你充分释放`Claude Code`的潜力。

## 1. 定制你的个性化开发环境

`Claude Code`通过自动抓取上下文来辅助编程，这一过程会消耗时间和`Token`。通过精细化配置环境，你可以显著优化其性能和准确性。

### a. 创建 `CLAUDE.md` 上下文文件

`CLAUDE.md`是一个特殊文件，`Claude Code`在启动对话时会自动加载其内容作为上下文。这使其成为记录项目关键信息的理想场所，内容可包括：

+   **常用命令**：例如`npm run build`、`npm run typecheck`。

+   **核心文件与函数**：项目中的关键模块或工具函数。

+   **代码风格指南**：如使用`ES`模块、解构导入等。

+   **测试说明**：如何运行单元测试或端到端测试。

+   **仓库规范**：分支命名规则、合并策略等。

+   **开发环境设置**：例如`pyenv`的使用、特定编译器的要求。

+   **项目特有的行为**：任何需要注意的警告或非预期行为。

`CLAUDE.md`没有固定格式，但建议保持简洁易读。你可以将它放置在多个位置，`Claude`会智能地加载它们：

+   **项目根目录**：最常见的用法，可将其命名为`CLAUDE.md`并提交至`Git`，与团队共享。若为个人配置，可命名为`CLAUDE.local.md`并加入`.gitignore`。

+   **父目录**：适用于`Monorepo`结构，`Claude`会自动加载所有父目录中的`CLAUDE.md`文件。

+   **子目录**：当你处理子目录中的文件时，`Claude`会按需加载对应子目录的`CLAUDE.md`。

+   **用户主目录**：`~/.claude/CLAUDE.md`中的配置将应用于所有`Claude`会话。

你可以通过运行`/init`命令，让`Claude`自动为你生成一个基础的`CLAUDE.md`文件。

### b. 调优 `CLAUDE.md` 文件

`CLAUDE.md`文件是`Claude`提示词的一部分，因此应像优化提示词一样持续迭代。一个常见的误区是添加大量内容后便不再维护。你应该花时间试验，找出能让模型最有效遵循指令的表述方式。

你可以在编码时随时按下`#`键，让`Claude`将你的指令（如常用命令、文件路径）自动添加到相关的`CLAUDE.md`文件中。为了进一步提升指令遵循度，可以尝试使用`Anthropic`的`Prompt Improver`工具，或在指令中加入"`IMPORTANT`"、`"YOU MUST"`等关键词来强调重点。

![](img/886ba191c93788513df1226ec6daee03.png)

`Claude Code`工具许可名单配置界面

### c. 管理 `Claude` 的工具许可名单

为确保安全，`Claude Code`默认会对所有可能修改系统的操作（如文件写入、执行`shell`命令）请求用户许可。你可以通过自定义**许可名单（allowlist）**来提升效率，允许其自动执行你信任的安全操作。

管理许可名单有四种方式：

+   **会话中授权**：当`Claude`请求权限时，选择"`Always allow`"。

+   **使用`/permissions`命令**：在会话中添加或移除工具权限，例如`Edit`允许文件编辑，`Bash(git commit:*)`允许`Git`提交。

+   **手动编辑配置文件**：修改项目下的`.claude/settings.json`或全局的`~/.claude.json`。

+   **使用命令行标志**：通过`-allowedTools`为特定会话指定权限。

### d. 安装 `GitHub CLI`

如果你的工作流涉及`GitHub`，强烈建议安装`gh`命令行工具。`Claude`能够利用它来创建`issue`、开启`PR`、读取评论等。若未安装，`Claude`仍可回退使用`GitHub API`或`MCP`服务器。

## 2. 为 Claude 提供更多工具

`Claude`可以利用你的`Shell`环境和更复杂的外部工具，从而扩展其能力边界。

### a. 结合使用 `Bash` 工具

`Claude Code`会继承你的`Bash`环境，使其能够访问所有已安装的工具。对于自定义的脚本或不常见的工具，你需要通过以下方式告知`Claude`如何使用：

+   提供工具名称和用法示例。

+   让`Claude`运行`-help`命令来学习工具的文档。

+   将常用工具的说明记录在`CLAUDE.md`中。

### b. 通过 `MCP` 扩展能力

`Claude Code`同时是`MCP`（`Model-Context Protocol`）的服务器和客户端，可以连接到任意数量的`MCP`服务器以获取其工具。这使得`Claude`能够与`Puppeteer`（浏览器自动化）、`Sentry`（错误监控）等外部服务交互。你可以通过项目配置、全局配置或`.mcp.json`文件来管理这些连接。

### c. 创建自定义斜杠命令

**对于重复性工作流**（如调试、日志分析），你可以在`.claude/commands`目录下创建`Markdown`文件作为自定义斜杠命令。这些文件可以包含提示词模板，并通过`$ARGUMENTS`关键字接收参数。

例如，创建一个名为`fix-github-issue.md`的文件：

```
Please analyze and fix the GitHub issue: $ARGUMENTS.

Follow these steps:
1. Use `gh issue view` to get the issue details.
2. Understand the problem described in the issue.
3. Search the codebase for relevant files.
4. Implement the necessary changes to fix the issue.
5. Write and run tests to verify the fix.
6. Ensure code passes linting and type checking.
7. Create a descriptive commit message.
8. Push and create a PR.

Remember to use the GitHub CLI (`gh`) for all GitHub-related tasks.
```

保存后，你就可以在`Claude Code`中使用`/project:fix-github-issue 1234`命令，让`Claude`自动修复指定的`GitHub issue`。

## 3. 探索常见的高效工作流

`Claude Code`的灵活性允许你自由设计工作流。以下是社区中沉淀下来的一些高效模式。

### a. 探索、规划、编码、提交

这是一个适用于多种复杂任务的通用工作流，它强调在编码前进行充分的思考和规划。

+   **探索**：要求`Claude`阅读相关文件、图片或`URL`，但明确指示它暂时不要编写任何代码。

+   **规划**：让`Claude`制定解决问题的计划。使用"`think`"、`"think hard"`或`"ultrathink"`等关键词，可以给予`Claude`更多的计算时间来评估不同方案。

+   **编码**：在确认计划后，让`Claude`开始实施。

+   **提交**：最后，让`Claude`提交代码、创建`PR`，并更新相关文档。

### b. 测试驱动开发（`TDD`）

`TDD`与代理式编程相结合，威力倍增。`Claude`在有明确目标（如通过测试用例）时表现最佳。

+   **编写测试**：让`Claude`根据预期输入输出编写测试用例。

+   **确认失败**：运行测试，确保它们因功能未实现而失败。

+   **提交测试**：将测试用例提交到版本控制。

+   **实现功能**：指示`Claude`编写能通过所有测试的代码，并在此过程中不断迭代。

+   **提交代码**：在所有测试通过后，提交最终实现。

![](img/c81d70bbcba5a35361f6cfdc6639d842.png)

以上两种模式的流程图

### c. 视觉驱动开发

与`TDD`类似，你可以为`Claude`提供视觉目标，尤其适用于`UI`开发。

1.  **提供截图工具**：通过`Puppeteer MCP`服务器或`iOS`模拟器`MCP`服务器，让`Claude`能够截取浏览器或应用的界面。

1.  **提供视觉稿**：通过粘贴、拖拽或文件路径的方式，将设计稿图片提供给`Claude`。

1.  **迭代实现**：要求`Claude`编写代码、截图、比对视觉稿，并循环迭代直至结果匹配。

1.  **提交**：满意后提交代码。

![](img/e30fd407531add2bb49194637cbdaacc.png)

`Safe YOLO`模式在容器中运行的示意图

### d. 安全的"`YOLO`"模式

对于修复`lint`错误或生成样板代码等低风险任务，你可以使用`--dangerously-skip-permissions`标志让`Claude`在无人监督的情况下连续工作。

**警告**：此模式存在数据丢失、系统损坏甚至数据泄露的风险。为将风险降至最低，请务必在没有网络访问的隔离容器（如`Docker Dev Container`）中使用此模式。

### e. 代码库问答与学习

在接触新项目时，`Claude Code`是一个出色的学习工具。你可以像与资深工程师结对编程一样向它提问：

+   这个项目是如何处理日志的？

+   如何创建一个新的`API`端点？

+   `foo.rs`文件第134行的`async move { ... }`是什么作用？

`Claude`会自动搜索代码库来回答你的问题，这已成为`Anthropic`内部新员工`onboarding`的核心流程，极大缩短了上手时间。

![](img/8c37f4042eaf016c73eff42a5a61af86.png)

使用`Claude`进行`Git`操作的示例

### f. 使用`Claude`操作`Git`和`GitHub`

许多`Anthropic`工程师超过90%的`Git`和`GitHub`操作都交由`Claude`完成，包括：

+   **搜索`Git`历史**：回答"这个功能是谁负责的？"或"这个`API`为何这样设计？"等问题。

+   **编写提交信息**：`Claude`会根据代码变更和近期历史自动生成高质量的`commit message`。

+   **处理复杂`Git`操作**：如文件回滚、解决`rebase`冲突等。

+   **创建`PR`**、**修复`CI`失败**、根据`Code Review`意见修改代码等。

## 4. 全面优化你的工作流

以下技巧适用于所有工作流，能进一步提升你与`Claude`协作的效率。

### a. 指令要具体明确

指令越具体，`Claude`的首次成功率就越高。明确的指令能减少后期反复修正的成本。

![](img/7b90cdbc7e122681f5eb1c1568755dfa.png)

向`Claude`提供图片作为上下文

### b. 善用图片、文件和`URL`

`Claude`的多模态能力非常强大。你可以通过以下方式提供上下文：

+   **粘贴截图**：直接将截图粘贴到输入框。

+   **拖拽图片**：将图片文件拖入终端。

+   **提供文件路径**：使用`Tab`自动补全快速引用仓库中的任何文件或目录。

+   **提供`URL`**：粘贴`URL`让`Claude`读取网页内容，如文档、博客等。

![](img/b11563ff0592b1a615475f631f339931.png)

通过`Tab`补全快速引用文件

![](img/f80038ac4fcaddd9549a6004a65b03f9.png)

向`Claude`提供`URL`作为上下文

### c. 尽早且频繁地修正方向

与其让`Claude`完全自主工作，不如成为一个积极的协作者。及时的引导和修正能产出更好的结果。

+   **打断**：按`Escape`键可以随时中断`Claude`的当前操作，然后给出新的指令。

+   **回溯**：连按两次`Escape`键可以回到历史记录，编辑之前的提示词，从某个节点开始探索新的方向。

+   **撤销**：要求`Claude`撤销刚才的修改。

### d. 使用`/clear`保持上下文清晰

在长会话中，上下文窗口可能会被不相关的信息填满，影响性能。在切换任务时，频繁使用`/clear`命令可以重置上下文，让`Claude`更专注于当前任务。

### e. 使用清单和草稿处理复杂任务

对于多步骤的复杂任务（如代码迁移、修复大量`lint`错误），可以让`Claude`使用一个`Markdown`文件作为清单（`checklist`）和草稿板（`scratchpad`）。

例如，修复`lint`错误时，可以指示`Claude`：

1.  运行`lint`命令，将所有错误输出到`lint-errors.md`文件中，并格式化为清单。

1.  逐一处理清单中的每个问题，修复、验证，然后勾选掉该项。

## 5. 使用无头模式实现自动化

`Claude Code`的**无头模式（`headless mode`）**专为非交互式环境设计，如`CI/CD`、`Git`钩子、自动化脚本等。使用`-p`标志并附带一个提示词即可启用。

### a. 用于`Issue`分类

你可以设置`GitHub Actions`，在创建新`issue`时触发无头模式的`Claude`，让它分析`issue`内容并自动打上合适的标签。

### b. 作为代码审查工具

传统`linter`无法检测主观问题。你可以让`Claude`在`CI`流程中进行代码审查，找出拼写错误、过时的注释、误导性的变量名等问题。

## 6. 进阶：多`Claude`协同工作流

通过并行运行多个`Claude`实例，可以解锁更强大的工作模式。

### a. 一个编码，一个验证

这是一个简单而有效的模式：

1.  让一个`Claude`实例编写代码。

1.  在另一个终端中启动第二个`Claude`实例（或在当前终端使用`/clear`）。

1.  让第二个`Claude`审查第一个实例的工作。

1.  再启动一个`Claude`（或再次`/clear`），让它结合代码和审查反馈进行修改。

这种上下文分离的模式，类似于人类工程师的结对编程或代码审查，通常能产生比单个`Claude`从头到尾处理所有事情更好的结果。

### b. 使用`Git Worktrees`并行处理任务

**`Git Worktrees`**允许你从同一个仓库中检出多个分支到不同的目录，每个目录都有独立的工作区但共享同一个`.git`历史。这使得并行处理多个不相关的任务变得异常高效。

![](img/5f72e23e1545a1e0329c1df8e5766727.png)

使用`Git Worktrees`并行处理任务
