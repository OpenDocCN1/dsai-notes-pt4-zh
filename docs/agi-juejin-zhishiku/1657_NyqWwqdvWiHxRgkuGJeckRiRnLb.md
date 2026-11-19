
# LangGraph Studio 助力构建 Agentic RAG

> 来源：[https://agijuejin.feishu.cn/docx/H8Z2d4CnVo63bgx7eUTcaEKHnPd](https://agijuejin.feishu.cn/docx/H8Z2d4CnVo63bgx7eUTcaEKHnPd)

## 👀快看👀

> 作者：[沧海九粟](https://space.bilibili.com/28357052)
> 原文链接：[🦜🕸️ LangGraph Studio 助力构建 Agentic RAG](https://www.bilibili.com/video/BV1v1421b75U/?spm_id_from=333.999.0.0&vd_source=5a54577a6bc7d876f6e5df88640c8e9f)
> 示例项目：`https://github.com/vbarda/pandas-rag-langgraph`
> LangGraph SDK：`https://langchain-ai.github.io/langgraph/cloud/reference/sdk/python_sdk_ref/`

## ⚡️摘要⚡️

> 主要介绍了如何利用 LangGraph Studio 构建一个结合了 Self RAG 和 Corrective RAG 的 Agentic RAG 系统，并通过 Line Graph Cloud 进行部署、调试和监控。

## 🤖分集🤖

### 🦜🕸️ LangGraph Studio 助力构建 Agentic RAG

介绍了基于 Land Graph 构建的 Agentic RAG 系统，结合 Self RAG 和 Corrective RAG 算法，可在 LangGraph Studio 中通过图形界面观察运行状态和流程，能通过 SDK、API 调用管理，在 Line Graph Cloud 部署监控，还通过代码示例展示实现步骤，涵盖重试次数设置等。

![](img/bdc8ce9223f98ae13a390e4ea2febc53.png)

[🦜🕸️ LangGraph Studio 助力构建 Agentic RAG.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/YA0NbunyCoYWMfx6CK1cnNMgnhb)

## ✏️课代表✏️

+ Agentic RAG 系统的重要性：结合 Self RAG 和 Corrective RAG 能够提高答案的准确性和可靠性，自我反省机制确保答案的质量，而网上搜索功能则为答案提供了额外的信息来源。

+ LangGraph Studio 的便利性：提供了一个图形化的界面，使得开发者可以直观地构建、调试和监控 Agent，同时支持时间旅行，可以回溯到任何状态重新执行流程。

+ SDK 和 API 的高效调用：通过 LangGraph SDK 和 API，可以更方便地与 Agent 交互，实现功能调用和状态管理，提高开发和调试的效率。

+ Line Graph Cloud 的生产环境优势：支持在线监控 Agent 的使用情况，帮助开发者了解 Agent 在实际环境中的表现，并及时进行优化和调整。

+ 代码实现的关键步骤：代码示例展示了如何设置重试次数、加载文档、构建 `retriever`、调用 LLM 模型、实现 web search 功能以及如何构建图的边和节点，这些都是构建 Agentic RAG 系统的关键环节。
