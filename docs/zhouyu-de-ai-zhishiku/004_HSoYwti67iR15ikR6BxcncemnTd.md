
# 装AI机记录

> 来源：[https://xatc0v8uz5m.feishu.cn/docx/BTeFdmkxnovqHbxD4t3ctOtZnub](https://xatc0v8uz5m.feishu.cn/docx/BTeFdmkxnovqHbxD4t3ctOtZnub)

`https://github.com/affaan-m/everything-claude-code`

该项目 `everything-claude-code` 是一个针对 Anthropic 发布的命令行工具 `Claude Code` 的全套配置与扩展资源库。

其主要功能概括如下：

## 核心组件集

+ [`Agents (代理)`](https://github.com/affaan-m/everything-claude-code?tab=contributing-ov-file#example-agents)：预设了多种专业角色的 AI 助手（如代码审查员、TDD 引导员、安全扫描员），通过特定指令调用以处理复杂任务。

+ [`Skills (技能)`](https://github.com/affaan-m/everything-claude-code?tab=contributing-ov-file#example-skills)：知识模块，为 `Claude` 提供特定领域（如 `TypeScript` 标准、前端/后端模式）的上下文和最佳实践。

+ [`Hooks (钩子)`](https://github.com/affaan-m/everything-claude-code?tab=contributing-ov-file#hook-examples)：自动触发器。支持在工具运行前/后（如 自动格式化、阻止危险指令）执行脚本。

+ [`Commands (命令)`](https://github.com/affaan-m/everything-claude-code?tab=contributing-ov-file#example-commands)：扩展了 `/` 开头的斜杠命令，用于简化 `Git` 提交、`端到端测试` 等常用工作流。

## 自动化与工程化支持

+ `MCP Configs`：包含 [`模型上下文协议 (MCP)`](https://github.com/topics/mcp) 的配置，增强 AI 对外部工具和数据的访问能力。

+ 工作流优化：提供了针对 [`长篇开发指南`](https://github.com/affaan-m/everything-claude-code/blob/main/the-longform-guide.md) 和 [`短篇快速参考`](https://github.com/affaan-m/everything-claude-code/blob/main/the-shortform-guide.md) 的模板。

+ 跨平台支持：通过 [`Node.js`](https://github.com/affaan-m/everything-claude-code/commit/970f8bf88459bb5cb73f77c1fcd46807ac0f0011) 脚本实现跨平台兼容性。

## 使用场景

该项目旨在通过“战斗测试”过的配置，将 `Claude Code` 从一个简单的聊天接口转变为一个具备深度感知、自动校验和专业领域知识的全自动 AI 软件工程师终端。

`github.com/danielmiessler/PAI`
