
### 8.4.3 性能优化

**五大技巧：**

1. **并行处理**：使用 `GNU parallel` 或 `xargs`
2. **缓存结果**：缓存昂贵操作（带 `TTL`）
3. **流式处理**：避免加载大文件到内存
4. **提前退出**：找到答案立即返回
5. **性能监控**：测量执行时间和资源使用

### 8.4.4 测试与调试

**测试策略：**

- **单元测试**：测试独立功能
- **集成测试**：测试与 OpenClaw 的集成
- **性能测试**：基准测试和压力测试

**调试技巧：**

```bash
# 启用调试模式
export SKILL_DEBUG=true
set -x  # Bash trace

# 保存中间结果
echo "$step1_output" > "/tmp/debug/step1.txt"

# 使用 Mock 数据
if [ "$MOCK_MODE" = "true" ]; then
  echo '{"status": "success", "data": "mock"}'
fi
```

---

## 8.5 Skill 发布与分享

### 8.5.1 发布到 ClawHub

**准备清单：**

- ✅ 完整的 `README.md`
- ✅ 测试通过
- ✅ 明确的依赖说明
- ✅ 示例和文档
- ✅ 开源许可证（推荐 MIT）

**发布步骤：**

```bash
# 1. 创建 Git 仓库
git init
git add .
git commit -m "Initial release"

# 2. 推送到 GitHub
git remote add origin https://github.com/username/skill-name.git
git push -u origin main

# 3. 提交到 ClawHub
# 访问 https://github.com/openclaw/clawhub
# 提交 Pull Request 添加你的 Skill
```

### 8.5.2 版本管理

**语义化版本：**

- `1.0.0`：主版本。次版本。修订版本
- 主版本：不兼容的 API 变更
- 次版本：向后兼容的功能新增
- 修订版本：向后兼容的问题修正

**Changelog 示例：**

```markdown
## [1.1.0] - 2026-03-15
### Added
- 新增批量处理功能
- 支持自定义输出格式

### Fixed
- 修复超时错误处理
- 改进错误消息
```

### 8.5.3 文档最佳实践

`README.md` 必备内容：

1. 一句话描述
2. 功能特性列表
3. 安装要求（环境变量/二进制/外部服务）
4. 安装步骤
5. 使用示例
6. 故障排查
7. 贡献指南
8. 许可证

**文档写作原则：**

- 从用户视角出发
- 提供可复制的命令
- 包含实际输出示例
- 说明常见问题解决方案

---

## 8.6 实战案例：GitHub PR 审查器

### 8.6.1 需求分析

**功能需求：**

- 自动获取 PR 信息
- 分析代码变更
- 检查代码质量
- 生成审查报告
- 提交审查评论

**技术选型：**

- GitHub API（获取 PR 数据）
- `git diff`（分析变更）
- 静态分析工具（代码质量）
- LLM（生成审查意见）

### 8.6.2 核心实现

`SKILL.md` 结构：

````markdown
---
name: github-pr-reviewer
description: 自动审查 GitHub Pull Request 并提供详细反馈
metadata:
  openclaw:
    requires:
      bins: [curl, jq, git]
      env: [GITHUB_TOKEN]
    primaryEnv: GITHUB_TOKEN
---

# GitHub PR Reviewer

## Step 1: 获取 PR 信息

```bash
curl -H "Authorization: token $GITHUB_TOKEN" \
     "https://api.github.com/repos/{owner}/{repo}/pulls/{pr_number}"
```

## Step 2: 分析代码变更

```bash
# 获取 diff
curl -H "Accept: application/vnd.github.v3.diff" \
     "https://api.github.com/repos/{owner}/{repo}/pulls/{pr_number}"

# 统计变更
added_lines=$(echo "$diff" | grep "^+" | wc -l)
removed_lines=$(echo "$diff" | grep "^-" | wc -l)
```

## Step 3: 代码质量检查

- 检查代码风格
- 识别潜在问题
- 评估测试覆盖率
- 检查文档完整性

## Step 4: 生成审查报告

```markdown
# PR Review: {title}

## 📊 变更统计

- 文件数：{files_changed}
- 新增：+{added_lines} 行
- 删除：-{removed_lines} 行

## ✅ 优点

- {positive_point_1}
- {positive_point_2}

## ⚠️ 建议

- {suggestion_1}
- {suggestion_2}

## 🐛 潜在问题

- {issue_1}
- {issue_2}

## 总体评价

{overall_assessment}
```

## Step 5: 提交审查评论

```bash
curl -X POST \
     -H "Authorization: token $GITHUB_TOKEN" \
     -d '{"body": "'"$review_comment"'", "event": "COMMENT"}' \
     "https://api.github.com/repos/{owner}/{repo}/pulls/{pr_number}/reviews"
```
````

### 8.6.3 使用示例

```bash
# 设置环境变量
export GITHUB_TOKEN="ghp_your_token_here"

# 在 OpenClaw 中使用
你: "请审查 https://github.com/owner/repo/pull/123"

OpenClaw: [调用 github-pr-reviewer skill]
[获取 PR 信息]
[分析代码变更]
[生成审查报告]

📋 PR Review: Add user authentication feature

## 📊 变更统计
- 文件数：8
- 新增：+245 行
- 删除：-12 行

## ✅ 优点
- 代码结构清晰，模块化良好
- 添加了完整的单元测试
- 文档更新及时

## ⚠️ 建议
- 建议添加输入验证
- 考虑添加速率限制
- 错误消息可以更友好

## 🐛 潜在问题
- 第 45 行可能存在 SQL 注入风险
- 密码存储应使用更强的哈希算法

## 总体评价
这是一个高质量的 PR，核心功能实现完善。建议修复安全问题后合并。
```

### 8.6.4 优化与扩展

**性能优化：**

- 缓存 API 响应
- 并行处理多个文件
- 增量分析（只检查变更部分）

**功能扩展：**

- 支持多种编程语言
- 集成更多静态分析工具
- 自动修复简单问题
- 生成改进建议的代码示例
- 跟踪审查历史

---

## 本章总结

### 核心要点

1. **Skill 本质**：用自然语言编写的 AI 指令手册，不是代码
2. **开发流程**：环境搭建 → 编写 `SKILL.md` → 测试 → 发布
3. **规范重要性**：清晰的命名、结构化的指令、完善的错误处理
4. **模板复用**：数据处理、API 调用、文件操作、网络爬虫
5. **高级技巧**：状态管理、错误处理、性能优化、测试调试
6. **发布分享**：ClawHub、版本管理、文档编写

### 最佳实践

- ✅ 从简单 Skill 开始，逐步增加复杂度
- ✅ 提供清晰的步骤和具体的示例
- ✅ 明确约束 AI 行为（NEVER/ALWAYS/MUST）
- ✅ 完善的错误处理和用户友好的错误消息
- ✅ 充分测试（单元测试 + 集成测试）
- ✅ 编写详细的文档和使用示例
- ✅ 遵循社区规范和最佳实践

### 进阶方向

- 学习社区优秀 Skill 的设计模式
- 贡献到 ClawHub 生态
- 开发垂直领域的专业 Skill
- 探索 Skill 与 Tools 的深度集成
- 参与 OpenClaw 社区讨论和改进

掌握 Skill 开发，就掌握了扩展 OpenClaw 能力的钥匙。从简单的天气查询到复杂的 PR 审查，Skill 让 AI Agent 能够完成各种专业任务。开始创建你的第一个 Skill，加入 OpenClaw 开发者社区吧！

---

**相关章节：**

- 第 3 章：项目背景与技术架构
- 第 6 章：踩坑经验大全
- 第 9 章：多 Agent 协作模式

**参考资源：**

- [OpenClaw 官方文档](https://docs.openclaw.ai/tools/skills)
- ClawHub Skill 仓库
- Awesome OpenClaw Skills

# 第 9 章 多 Agent 协作

在现代 AI 应用中，单一 Agent 往往难以应对复杂的业务场景。多 Agent 系统通过让多个专业化的 Agent 协同工作，能够处理更复杂的任务、提供更高质量的服务。本章将深入探讨如何在 OpenClaw 中设计和实现多 Agent 协作系统。

## 9.1 多 Agent 架构设计

### 9.1.1 什么是多 Agent 系统

多 Agent 系统（Multi-Agent System, MAS）是由多个自主 Agent 组成的协作网络，每个 Agent 都有自己的专业领域、工具集和决策逻辑。这种架构类似于企业中的团队协作：不同的专家各司其职，通过协调配合完成复杂项目。

**核心特征：**

多 Agent 系统的本质是将复杂问题分解为可管理的子任务，然后由专门的 Agent 处理。每个 Agent 拥有独立的上下文窗口、记忆系统和工具访问权限，这使得系统能够进行并行推理和专业化处理。研究表明，在 [Anthropic 的多 Agent 研究系统](https://blog.langchain.com/choosing-the-right-multi-agent-architecture/)中，使用 Claude Opus 4 作为主 Agent、Claude Sonnet 4 作为子 Agent 的架构，比单一 Claude Opus 4 的表现提升了 90.2%。

在 OpenClaw 中，多 Agent 系统通过 Gateway 进程实现完全隔离。每个 Agent 拥有独立的[工作空间（`agentDir`）](https://docs.openclaw.ai/concepts/multi-agent)、身份配置、会话管理和工具策略，这确保了不同 Agent 之间不会产生认证冲突或会话混淆。

**适用场景：**

并非所有任务都需要[多 Agent 架构](https://lumadock.com/tutorials/openclaw-multi-agent-setup)。当任务需要真正的隔离、不同的工具策略或永远不应混合的独立记忆时，多 Agent 才开始显示其价值。对于简单的单步任务，单一 Agent 通常就足够了。

### 9.1.2 协作模式分类

根据 Google 和业界最佳实践，多 Agent 系统主要有以下几种基础协作模式：

1. **顺序流水线模式（Sequential Pipeline）**

   Agent 按照预定义的顺序依次处理任务，每个 Agent 的输出成为下一个 [Agent](https://www.infoq.com/news/2026/01/multi-agent-design-patterns/) 的输入。这是最简单也是最可预测的模式，适合具有明确步骤的工作流。

   在 OpenClaw 中，可以通过 Lobster 工作流引擎实现确定性的顺序流水线。例如，一个代码开发[流水线](https://dev.to/ggondim/how-i-built-a-deterministic-multi-agent-dev-pipeline-inside-openclaw-and-contributed-a-missing-4ool)可以包括：程序员 Agent 编写代码 → 审查员 Agent 审查 → 测试员 Agent 测试 → 通知结果。

2. **协调者/分发者模式（Coordinator/Dispatcher）**

   一个[中心 Agent](https://www.infoq.com/news/2026/01/multi-agent-design-patterns/) [作为决策者，接收](https://www.speakeasy.com/mcp/using-mcp/ai-agents/architecture-patterns)请求并将其分发给专业化的 Agent 处理。这种模式也称为“监督者模式”（Supervisor Pattern），是最常用的多 Agent 架构之一。

3. **并行扇出/聚合模式（Parallel Fan-out/Gather）**

   多个 [Agent](https://www.infoq.com/news/2026/01/multi-agent-design-patterns/) 同时并行工作，每个负责不同的子任务，最后将结果汇总。这种模式充分利用了并行计算的优势，显著提升处理速度。

4. **层级代理模式（Hierarchical Agent）**

   建立[多层次的 Agent](https://www.confluent.io/blog/event-driven-multi-agent-systems/)结构，高层 Agent 负责战略决策和任务分解，低层 Agent 执行具体操作。这种模式适合处理复杂的企业级应用。

5. **黑板模式（Blackboard Pattern）**

   所有 [Agent](https://www.confluent.io/blog/event-driven-multi-agent-systems/) 共享一个中央知识库（黑板），Agent 可以读取和更新黑板上的信息。这种模式适合需要动态协作和知识共享的场景。

6. **循环模式（Loop Pattern）**

   [Agent 在循环](https://www.infoq.com/news/2026/01/multi-agent-design-patterns/)中反复执行任务，直到满足特定条件。常用于需要迭代优化的场景，如代码审查循环：编写 → 审查 → 修改 → 再审查，直到通过。

### 9.1.3 架构设计原则

设计多 Agent 系统时，应遵循以下核心原则：

**原则一：模块化与专业化**

每个 Agent 应该有明确的职责边界和专业领域。避免创建“万能 [Agent](https://docs.openclaw.ai/concepts/multi-agent)”，而是让每个 Agent 专注于做好一件事。在 OpenClaw 中，这意味着为每个 Agent 配置独立的工作空间文件（如 `AGENTS.md` 和 `SOUL.md`）来定义其个性和能力。

**原则二：隔离性**

确保 [Agent 之间](https://docs.openclaw.ai/concepts/multi-agent)的会话、认证和状态完全隔离。在 OpenClaw 中，绝不要在多个 Agent 之间重用 `agentDir`，这会导致认证和会话冲突。如果需要共享凭证，应该复制 `auth-profiles.json` 到其他 Agent 的目录中。

**原则三：通信效率**

设计[清晰的通信协议](https://smythos.com/developers/agent-development/agent-communication-and-message-passing/)和消息格式。避免过早地过度结构化消息格式，这会导致系统无法适应现实世界的变化。消息应该既能被机器解析，又保持足够的灵活性。

**原则四：可观测性**

建立完善的监控和日志系统，能够追踪 Agent 之间的通信、任务执行状态和性能指标。这对于调试和优化系统至关重要。
