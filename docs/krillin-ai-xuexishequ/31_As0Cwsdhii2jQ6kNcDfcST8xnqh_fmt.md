
# Introduction 简介

> 来源：[https://kcn9ut4m7icc.feishu.cn/docx/F0j2dEYM6oQEIGxIBylcrvn6nSe](https://kcn9ut4m7icc.feishu.cn/docx/F0j2dEYM6oQEIGxIBylcrvn6nSe)

Learn how to use Cursor’s core features: Tab completion, Chat for code queries, and Agent for assistance

了解如何使用 Cursor 的核心功能：Tab 补全、代码查询聊天和助手功能

## Overview 概览

[Cursor](https://docs.cursor.com/get-started/installation) is a powerful AI-first code editor that enhances your development workflow. After installation, you’ll have access to these core features that work together seamlessly to make you more productive:

Cursor是一款强大的以 AI 为先导的代码编辑器，可增强您的开发工作流程。安装后，您将能够访问这些核心功能，它们协同工作，无缝地提高您的生产力：

+   AI-powered code completion that understands your codebase and provides context-aware suggestions

AI 驱动的代码补全，理解您的代码库并提供上下文感知建议

+   Conversation interface for exploring, understanding, and modifying code with Ask, Edit, and Agent modes

对话界面，用于通过“询问”、“编辑”和“代理”模式探索、理解和修改代码

+   Intelligent tools for handling complex development tasks

智能工具，用于处理复杂开发任务

## Getting Started 入门

Start exploring Cursor’s AI-powered features:

开始探索 Cursor 的 AI 功能：

+   Tab: Press `Tab` for intelligent code completions

Tab：按 `Tab` 进行智能代码补全

+   CMD-K: Use `Cmd/Ctrl + K` for inline code edits

CMD-K：使用 `Cmd/Ctrl + K` 进行内联代码编辑

+   Composer: Use `⌘I` to open the unified AI interface with Ask, Edit, and Agent modes

作曲家：使用 `⌘I` 打开具有询问、编辑和代理模式的统一 AI 界面

## Settings 设置

Cursor is designed to be flexible and customizable. You can configure it in two ways:

Cursor设计得灵活且可定制。您可以通过两种方式来配置它：

### Cursor Settings  Cursor设置

+   Access via gear icon, `Cmd/Ctrl + Shift + J`, or Command Palette > Cursor Settings

通过齿轮图标访问，`Cmd/Ctrl + Shift + J` ，或命令面板 > Cursor Settings

+   Configure AI features and Cursor-specific preferences

配置 AI 功能和 Cursor 特定首选项

### Editor Settings  编辑设置

+   Access via Command Palette (`Cmd/Ctrl + Shift + P`) > `"Preferences: Open Settings (UI)"`

通过命令面板访问（`Cmd/Ctrl + Shift + P` ）> `"Preferences: Open Settings (UI)"`

+   Adjust editor behavior and appearance

调整编辑器行为和外观

Let’s explore each feature in detail:

让我们详细探索每个功能：

### Tab  标签

Tab completion in Cursor is powered by advanced AI models that understand your code context. As you type, you’ll receive intelligent suggestions that:

Cursor 中的 `Tab` 补全功能由理解代码上下文的高级 AI 模型提供支持。在您输入时，您将收到智能建议，这些建议包括：

+   Complete your current line of code

完成您当前的一行代码

+   Suggest entire function implementations

建议完整的函数实现

+   Help with common patterns and boilerplate

帮助常见模式和模板

+   Adapt to your coding style over time

适应您的编码风格

[Learn more about Tab features](https://docs.cursor.com/tab/overview) or see how it compares to GitHub Copilot.

了解 Tab 功能或查看它与 GitHub Copilot 的对比。

### Composer  作曲家

Cursor provides a unified AI interface with three modes that seamlessly work together:

Cursor提供了一种统一的 AI 界面，具有三种无缝协作的模式：

### Ask Mode  询问模式

+   Ask questions about specific code sections

询问特定代码部分的问题

+   Get explanations of complex functions

获取复杂函数的解释

+   Find code patterns and examples

查找代码模式和示例

+   Discover and understand your codebase

发现并理解您的代码库

### Edit Mode  编辑模式

+   Make single-turn edits to your code

对代码进行单次编辑

+   Apply targeted changes with precision

应用精确的针对性更改

+   Review and apply changes with confidence

审阅并自信地应用更改

+   Work with files individually

与文件单独工作

### Agent Mode (Default)  代理模式（默认）

+   Make codebase-wide changes and refactoring

使代码库范围内的更改和重构

+   Implement new features from requirements

实现需求中的新功能

+   Debug complex issues across multiple files

调试跨多个文件的复杂问题

+   Generate tests and documentation

生成测试和文档

+   Maintain consistency across your entire project

保持整个项目的一致性

Switch between modes during conversations to best suit your current task. Learn more about the unified AI interface or explore specific capabilities in [Agent mode](https://docs.cursor.com/agent).

在对话中切换模式以最适合当前任务。了解更多关于统一人工智能界面或探索在代理模式下的特定功能。

### Context  上下文

Context is the foundation that powers all of Cursor’s AI features. Here’s how it works:

上下文是驱动 Cursor 所有 AI 功能的基础。以下是它是如何工作的：

+   When you open a [codebase](https://docs.cursor.com/context/codebase-indexing), we automatically index your code to make it available as context

当你打开一个代码库时，我们会自动索引你的代码，使其作为上下文可用

+   Use `[@-symbols](https://docs.cursor.com/context/@-symbols/basic)` to precisely control what context you provide:

使用 `[@-symbols](https://docs.cursor.com/context/@-symbols/basic)` 精确控制您提供的上下文：

+   `[@files](https://docs.cursor.com/context/@-symbols/@-files)` and `[@folders](https://docs.cursor.com/context/@-symbols/@-folders)` for specific paths

`@文件`和`@文件夹`用于特定路径

+   `[@web](https://docs.cursor.com/context/@-symbols/@-web)` for external documentation

`@web` 用于外部文档

+   `[@git](https://docs.cursor.com/context/@-symbols/@-git)` for version control context

`@git` 用于版本控制上下文

+   Configure [rules for AI](https://docs.cursor.com/context/rules-for-ai) to customize behavior

配置 `rules for AI` 以自定义行为

+   Set up [MCP](https://docs.cursor.com/context/model-context-protocol) for external context providers

设置 `MCP` 以供外部上下文提供者使用

## Models  模型

You can see [all the models we support](https://docs.cursor.com/settings/models) and their pricing on the models page. [Configure your API keys](https://docs.cursor.com/settings/api-keys) and [preferences](https://docs.cursor.com/settings/preferences) in Settings.

您可以在模型页面查看我们支持的所有模型及其定价。在设置中配置您的 API 密钥和偏好设置。

## Usage  使用

It’s highly recommended to [read about usage](https://docs.cursor.com/account/usage) and [plans](https://docs.cursor.com/account/plans) to understand how Cursor pricing works. Check out our [pricing page](https://docs.cursor.com/account/pricing) for more details about plans and features.

强烈建议您了解使用方法和计划，以便理解 Cursor 的定价方式。请查看我们的定价页面，获取更多关于计划和功能的详细信息。
