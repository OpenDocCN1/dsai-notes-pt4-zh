# 第一章：Hermes Agent概述

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/ZxgGdnnwEoamiAxawlUc4P1Unc1](https://qqs7y1hozd1.feishu.cn/docx/ZxgGdnnwEoamiAxawlUc4P1Unc1)

Hermes Agent 具备丰富的功能，远远超越了基本的聊天功能。从持久记忆和文件感知上下文到浏览器自动化和语音对话，这些功能共同作用，使 Hermes 成为一个强大的自主助手

<https://player.bilibili.com/player.html?bvid=1jRQxBAERc&share_source=copy_web>

## 核心

+   [**工具与工具集**](https://hermes-agent.nousresearch.com/docs/user-guide/features/tools) — 工具是扩展代理能力的功能。它们被组织成逻辑工具集，可按平台启用或禁用，涵盖网络搜索、终端执行、文件编辑、内存、委托等。

+   [**技能系统**](https://hermes-agent.nousresearch.com/docs/user-guide/features/skills) — 按需知识文档，代理可在需要时加载。技能遵循[渐进式披露模式，以尽量减少令](https://agentskills.io/specification)牌使用，并与agentskills.io开放标准兼容。

+   [**持久内存**](https://hermes-agent.nousresearch.com/docs/user-guide/features/memory) — 有界的、经过整理的内存，可在会话之间持久存在。Hermes 会记住你的偏好、项目、环境以及通过和学到的东西。MEMORY.mdUSER.md

+   [**上下文文件**](https://hermes-agent.nousresearch.com/docs/user-guide/features/context-files) — Hermes会自动发现并加载项目上下文文件（.hermes.md、AGENTS.md、CLAUDE.md、SOUL.md、cursorrules），这些文件决定了它在您项目中的行为方式。.hermes.md、AGENTS.md、CLAUDE.md、SOUL.md、cursorrules

+   [**上下文引用**](https://hermes-agent.nousresearch.com/docs/user-guide/features/context-references) — 类型后面跟一个引用，可直接将文件、文件夹、git差异和URL注入到您的消息中。Hermes会内联展开引用并自动追加内容。@

+   [**检查点**](https://hermes-agent.nousresearch.com/docs/user-guide/checkpoints-and-rollback) — Hermes在进行文件更改之前会自动对工作目录进行快照，为你提供一个安全网，以便在出现问题时可以回滚。/回滚

## 自动化

+   [**计划任务（Cron）**](https://hermes-agent.nousresearch.com/docs/user-guide/features/cron) — 使用自然语言或cron表达式安排任务自动运行。作业可以附加技能，将结果发送到任何平台，并支持暂停/恢复/编辑操作。

+   [**子代理委派**](https://hermes-agent.nousresearch.com/docs/user-guide/features/delegation) — 该工具会生成具有独立上下文、受限工具集和自己的终端会话的子代理实例。最多可运行3个并发子代理以处理并行工作流。委派任务

+   [**代码执行**](https://hermes-agent.nousresearch.com/docs/user-guide/features/code-execution) — 该工具允许代理编写Python脚本，以编程方式调用Hermes工具，通过沙盒化的RPC执行将多步骤工作流合并为单个大语言模型回合。execute_code

+   [**事件钩子**](https://hermes-agent.nousresearch.com/docs/user-guide/features/hooks) — 在关键生命周期点运行自定义代码。网关钩子处理日志记录、警报和 Webhook；插件钩子处理工具拦截、指标和护栏。

+   [**批量处理**](https://hermes-agent.nousresearch.com/docs/user-guide/features/batch-processing) — 并行运行 Hermes 代理，处理成百上千个提示，生成结构化的 ShareGPT 格式轨迹数据，用于训练数据生成或评估。

## 媒体与网络

+   [**语音模式**](https://hermes-agent.nousresearch.com/docs/user-guide/features/voice-mode) — 在CLI和消息平台上实现全面语音交互。使用麦克风与代理对话，听取语音回复，并在Discord语音频道中进行实时语音对话。

+   [**浏览器自动化**](https://hermes-agent.nousresearch.com/docs/user-guide/features/browser) — 支持多后端的全浏览器自动化：Browserbase云、Browser Use云、通过CDP的本地Chrome或本地Chromium。浏览网站、填写表单并提取信息。

+   [**视觉与图像粘贴**](https://hermes-agent.nousresearch.com/docs/user-guide/features/vision) — 多模态视觉支持。将剪贴板中的图像粘贴到 CLI 中，并要求智能体使用任何具备视觉能力的模型对其进行分析、描述或处理。

+   [**图像生成**](https://hermes-agent.nousresearch.com/docs/user-guide/features/image-generation) — 使用 FAL.ai 的 FLUX 2 Pro 模型，通过文本提示生成图像，并通过 Clarity Upscaler 自动进行 2 倍超分辨率处理。

+   [**语音与TTS**](https://hermes-agent.nousresearch.com/docs/user-guide/features/tts) — 跨所有消息平台的文本转语音输出和语音消息转录，有五个供应商选项：Edge TTS（免费）、ElevenLabs、OpenAI TTS、MiniMax和NeuTTS。

## 集成

+   [**MCP集成**](https://hermes-agent.nousresearch.com/docs/user-guide/features/mcp) — 通过标准输入输出或HTTP传输连接到任何MCP服务器。无需编写原生Hermes工具即可访问GitHub上的外部工具、数据库、文件系统和内部API。包括按服务器进行工具过滤和采样支持。

+   [**供应商路由**](https://hermes-agent.nousresearch.com/docs/user-guide/features/provider-routing) — 对哪些AI供应商处理您的请求进行细粒度控制。通过排序、白名单、黑名单和优先级排序，针对成本、速度或质量进行优化。

+   [**备用提供商**](https://hermes-agent.nousresearch.com/docs/user-guide/features/fallback-providers) — 当您的主模型遇到错误时，自动故障转移到备用大语言模型（LLM）提供商，包括针对视觉和压缩等辅助任务的独立备用。

+   [**凭证池**](https://hermes-agent.nousresearch.com/docs/user-guide/features/credential-pools) — 跨同一提供商的多个密钥分发 API 调用。在达到速率限制或发生故障时自动轮换。

+   [**内存提供程序**](https://hermes-agent.nousresearch.com/docs/user-guide/features/memory-providers) — 插入外部内存后端（Honcho、OpenViking、Mem0、Hindsight、Holographic、RetainDB、ByteRover），以实现跨会话用户建模和超越内置内存系统的个性化。

+   [**API服务器**](https://hermes-agent.nousresearch.com/docs/user-guide/features/api-server) — 将Hermes作为兼容OpenAI的HTTP端点公开。连接任何支持OpenAI格式的前端 — Open WebUI、LobeChat、LibreChat等等。

+   [**IDE集成（ACP）**](https://hermes-agent.nousresearch.com/docs/user-guide/features/acp) — 在与ACP兼容的编辑器（如VS Code、Zed和JetBrains）中使用Hermes。聊天、工具活动、文件差异和终端命令都在编辑器内呈现。

+   [**强化学习训练**](https://hermes-agent.nousresearch.com/docs/user-guide/features/rl-training) — 从智能体会话中生成轨迹数据，用于强化学习和模型微调。

## 定制

+   Personality & SOUL.md — 完全可定制的代理个性。是主要身份文件 — 系统提示中的第一项 — 你可以在每个会话中切换内置或自定义预设。SOUL.md/personality

+   [**皮肤与主题**](https://hermes-agent.nousresearch.com/docs/user-guide/features/skins) — 自定义CLI的视觉呈现：横幅颜色、加载动画的外观和动词、响应框标签、品牌文本以及工具活动前缀。

+   [**插件**](https://hermes-agent.nousresearch.com/docs/user-guide/features/plugins) — 添加自定义工具、钩子和集成，而无需修改核心代码。有三种插件类型：通用插件（工具/钩子）、内存提供程序（跨会话知识）和上下文引擎（替代上下文管理）。通过统一的交互式 UI 进行管理。hermes 插件