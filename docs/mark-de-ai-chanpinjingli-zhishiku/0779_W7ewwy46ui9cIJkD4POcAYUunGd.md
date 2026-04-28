# 第一节：Introduction（课程介绍）

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/JvSxdZ9VYoEfnfxtrATczD4RnEc](https://qqs7y1hozd1.feishu.cn/docx/JvSxdZ9VYoEfnfxtrATczD4RnEc)

【吴恩达】2026年公认最好的【Agent Skills】教程！大模型入门到进阶，一套全解决！

### Agent Skills with Anthropic—附课件代码

<https://player.bilibili.com/player.html?bvid=1qv6eBZErD&share_source=copy_web1>

## 1、课程概述

本课程是 DeepLearning.AI 与 Anthropic 合作打造的 Agent Skills专项课程。

在本课程中，我们将探讨 Skills如何赋能 Claude 及其他 AI 智能体执行复杂任务。技能通过向智能体提供专门的指令和知识来扩展其能力。

+   理解Skills的工作原理

+   掌握创建Skills

+   构建适用于不同场景的Skills（包括编程、研究、数据分析等）

---

## 2、什么是“Skills”？

### 什么是 Agent Skills？

Agent Skills 是一种扩展智能体能力的模块化指令集合。通过技能，Claude 和其他 AI 智能体可以获得执行特定任务的新能力。

Skills 是文件夹形式的指令集合，用于扩展智能体的能力，赋予其专门的知识来执行任务。

### Skills 的核心特点

1.  开放标准：Skills 现在是一个开放标准，采用标准化格式，可与任何兼容的智能体产品配合使用

1.  一次构建，多处部署：你可以构建一次技能，然后在多个智能体产品中部署使用

1.  渐进式披露：技能的名称和描述始终存在于智能体的上下文窗口中，但只有当用户请求与技能描述匹配时，才会加载其余指令

---

## 3、使用 Skills 所需的工具

智能体需要以下基本工具集来使用技能：

+   文件系统访问：读取和写入文件

+   Bash 工具：执行代码

这些工具使智能体能够执行技能所需的任何命令。

---

## 4、Skills 的组合使用

智能体可以将 Skills 与 MCP 和 子智能体 结合，创建强大的智能工作流：

---

## 5、课程内容概览

本课程将涵盖以下内容：

1.  Claude AI 入门

+   创建一个营销活动技能

+   结合预建的 Excel 和 PowerPoint 技能

1.  内容创建和数据分析工作流

+   创建两个技能

+   使用 Claude API 进行测试

1.  Claude Code 代码审查

+   使用技能进行代码审查和测试

1.  研究智能体构建

+   使用 Claude Agent SDK 构建研究智能体

+   使用技能整合研究结果

---

## 6、何时使用 Skills？

当你有一个反复要求智能体实现的工作流时，与其每次都解释相同的工作流，不如将其打包成一个技能，让智能体自动知道该做什么。