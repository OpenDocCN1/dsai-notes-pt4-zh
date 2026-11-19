
# AI Harness 工程：Agent 能跑起来的那一层到底是什么？

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/Bv7mdqmKaoP7I0xsqXlcJ64HnMh](https://qqs7y1hozd1.feishu.cn/docx/Bv7mdqmKaoP7I0xsqXlcJ64HnMh)

关于构建 AI Agent，业界通常谈的是三种架构路径：SDK、Frameworks、Scaffolding。这三种方式各自站在灵活性与结构性的不同位置，也各有适用场景。

**但 2026年**悄悄冒出了第四种模式，而且直接架在这三种之上——它叫 Harness。

## Harness 到底是什么？

先把一件事说清楚：Harness 不是 Agent 本身。

它是管理 Agent 如何运行的那套软件系统，负责处理完整的生命周期——工具调用、内存、重试、人工审批、上下文工程、子 Agent……让模型可以专心做推理，其他的一概不用操心。

Philipp Schmid() 用了一个很贴切的计算机类比来解释这件事：

![](img/63b0340cb0ebdb4d9a108a4b1ca6dea1.png)

模型是原始的处理能力，上下文窗口是有限的工作内存，而 Harness 就是操作系统——管上下文、管初始化序列、管标准工具驱动。Agent 则是跑在这一切之上的应用程序。

这个比喻非常准，一下子就把关系理清楚了。

---

## 它跟 SDK、Framework、Scaffolding 是什么关系？

SDK、Scaffolding、`Framework` 回答的是同一个问题：怎么把 Agent 造出来？

Harness 回答的是一个完全不同的问题：Agent 造出来之后，怎么让它安全、稳定地跑？

> 这两件事不是替代关系——你完全可以用 `Framework` 来构建一个 Harness，它们处于不同的层次。

![](img/54cf8a54e865a7aacad28baa87a6db42.png)

**四种方式的对比关系如下图：**

![](img/5b6faea716eab8ed005e0392f9e48b63.png)

## Harness 有哪些核心组件？

parallel.ai 的团队梳理出了六个核心组件，这也跟 OpenAI 和 Anthropic 官方发布的内容高度一致：

![](img/ffc5c0ad0a1e341a02bb8c0ddf8e7e1a.png)

**工具集成层**，通过定义好的协议把模型接入外部 API、数据库、代码执行环境

**内存与状态管理**，多层内存体系——工作上下文、会话状态、长期记忆——在单个上下文窗口之外做持久化。Anthropic 的做法是用进度文件和 `git` 历史来桥接不同的会话

**上下文工程与提示管理**，不是一堆写死的提示模板，而是根据当前任务状态动态决定每次模型调用时塞进去什么信息——主动选择，而不是全部照单全收。

**规划与任务分解**，引导模型一步一步走结构化的任务序列，而不是试图一口气搞定所有事情。

**验证与防护**，格式验证、安全过滤、自我纠错循环。当 Agent 卡住的时候，Harness 把它当成一个信号——说明有什么东西缺了，而不是直接报错崩掉。

**模块化与可扩展性**，各个组件可以独立开关、独立替换，插拔式设计，改一个不影响其他的。

## 真实生产中的 Harness 长什么样？

**Claude Code**() 就是一个典型的 Harness。

它读取整个代码库，管理文件系统访问，调度子 Agent，处理工具编排，跨会话维护内存，还内置了各种防护。开发者只需要专注于任务本身，其他的 Harness 全部兜着。

OpenAI Codex 也是。

他们的团队用这套 Harness 工程方式，搭出了一个超过 100 万行的代码库，全程没有一行手动输入的代码。Harness 是主要接口，当 Agent 遇到问题，反馈会直接流回代码库，推动上下文工程和架构约束的持续迭代。

OpenAI 的 CUA 示例应用（计算机使用场景下的 Harness）中，`Runner` 管理的是"截图 → 操作 → 验证 → 再循环"这个完整闭环。模型负责决定做什么，Harness 负责安全地把它执行出来。

## Framework 层正在被 Harness 吞掉

有一个很值得关注的趋势：传统 Framework 处理的那些事情，正在被模型本身吸收。

Agent 定义、消息路由、任务生命周期、依赖管理、生成工作进程——以前开发者用 Framework 来搞定这些，但现在大约 80% 的这类功能，模型已经原生支持了。

剩下那 20%——持久性、确定性重放、成本控制、可观察性、错误恢复——正好是 Harness 负责的部分。

![](img/b05a857f63232abc6525cb63c0a77070.png)

Framework 层不只是在消失，它在分裂：智能进入模型，基础设施进入 Harness。

Harness 与 Framework 的核心区别也很清晰：Framework 告诉开发者怎么构建应用，Harness 告诉 Agent 怎么安全运行。用 Framework 时，开发者写编排逻辑；用 Harness 时，模型自己制定计划，Harness 保持它不翻车。

![](img/d6b9b62b60bc050179e78a3e11e6724b.png)

## 现在构建 AI Agent，问题变了

以前大家问的是：用哪个 Framework？

现在更关键的问题是：**Harness 应该长什么样？**

Harness 决定了 Agent 是成功还是失败。好的 Harness 能管住人工审批、文件系统访问、工具编排、子 Agent、提示和完整的生命周期，干预最少，但能挡住灾难性失败。

落地建议也很实在：从简单的地方开始，先把原子工具做扎实，让模型来制定计划，再逐步加上防护、重试和验证机制。这就是 Harness 工程的基本思路。

## 最后说一种特殊形态

值得单独提一下的是 Markdown/Prompt Harness，比如 Anthropic 的 `CLAUDE.md` 技能文件，它把编排指令直接嵌进系统提示或结构化的 Markdown 文件里。

这种方式下，LLM 本身就成了循环控制器——它读取 Harness 规则，然后照着执行。当模型足够强、能做到自我引导，而且你需要快速迭代、不想每次都改代码的时候，这是个相当好用的选择。
