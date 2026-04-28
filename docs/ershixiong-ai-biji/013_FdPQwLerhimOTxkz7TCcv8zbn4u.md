# 请问下 您的 Cline 中的 cache 是用的 MCP server 吗？

> 来源：[https://qianniucity.feishu.cn/docx/BMGCdFJlCo3SBTxvC88cxWyGnHg](https://qianniucity.feishu.cn/docx/BMGCdFJlCo3SBTxvC88cxWyGnHg)

> 来自用户的提问：请问下 您的 Cline 中的 cache 是用的 MCP server 吗？

使用 MCP Server 来缓存与 AI 助手对话的上下文是一个有趣的想法。MCP Server 通过提供一个标准化**的协议，允许 AI 模型与外部数据源和工具进行**交互，从而增强 AI 在代码仓库管理、Issue 追踪等方面的能力。然而，MCP Server 主要用于提供实时上下文信息和操作能力，而不是传统意义上的缓存机制。

## **MCP Server 的优势**

1.  **实时上下文获取**：MCP Server 可以让 AI 助手实时获取和操作代码仓库、Issue 等相关数据，提高了 AI 在协作开发中的实用性。

1.  **灵活配置**：支持自定义 API 端点和不同部署场景，适合企业私有化部署。

## **与传统缓存的区别**

1.  **缓存机制**：传统缓存（如 Context Caching）主要用于存储频繁访问的数据，以减少重复计算和提高效率。而 MCP Server 则是提供实时数据交互和操作能力。

1.  **应用场景**：MCP Server 更适合需要实时获取和操作外部资源的场景，而传统缓存则适合于频繁重复访问相同数据的场景。

## 推荐方案：知识转移文档

一个简单但高效的方法，解决Ai 编程过程时，经常出现的上下文过载问题

<https://player.bilibili.com/player.html?bvid=1e3XKY5EPg&vd_source=54e20856a9f5276a04b00b90fbc51717>