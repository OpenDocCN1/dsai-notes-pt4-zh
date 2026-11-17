
# LangChain 1.0 与 LangGraph 概述

## 搜索结果示例

```
[
  {
    &quot;tool&quot;: &quot;duckduckgo_search&quot;,
    &quot;args&quot;: {
      &quot;query&quot;: &quot;LangChain 1.0 and LangGraph relationship&quot;
    },
    &quot;result&quot;: &quot;LangChain is mostly for a single agent, single purpose. LangGraph is mostly for multi agent (graph relationship ). It&#x27;s not really one or the other, you&#x27;re gonna ... Here&#x27;s the gist. LangChain is still great for quick experiments and stitching tools together. LangGraph feels like the natural step up for multi-agent systems. LangChain 有可能這陣子就會上 1.0 版囉，官方的目標是在十月下旬正式發佈穩定版本的 LangChain 1.0 。 LangGraph 也是會一起上 1.0 版哦！ 我會再密切關注，官方已經說了可能 ... # ** LangChain 1.0 引入統一代理設計，提升代理開發一致性** 人工智慧代理開發框架 LangChain 釋出 1.0 Alpha測試版，採用全新代理架構，底層整合 LangGraph 執行與管理狀態 ... 2025年9月2日 — LangGraph v1.0.0a2 is out ! We&#x27;ve released v1 alpha versions for langgraph, langchain, langchain-core, and some key integrations packages like ...&quot;
  },
  {
    &quot;tool&quot;: &quot;duckduckgo_search&quot;,
    &quot;args&quot;: {
      &quot;query&quot;: &quot;LangChain 1.0 LangGraph 最新信息&quot;
    },
    &quot;result&quot;: &quot;Sep 20, 2025 · LangChain 主体分为 6 个模块，分别是对（大语言）模型输入输出的管理、外部数据接入、链的概念、（上下文记忆）存储管理、智能代理以及回调系统，通过文档的组织结构，你可以清 … 这算得上是一个非常惊艳的功能。 主要优点是，你用自然语言提出问题，LangChain把这个问题丢给LLMs，LLMs返回SQL脚本，然后LangChain会自动执行查询语句得到结果后，再丢给LLMs生成一 … LangGraph 作为 LangChain 的专业化演进分支，官方投入持续加大（当前已到 v1.0.0-alpha）。 困难点 很多入门资料直接从一个“LLM Graph”大例子切入，容易让初学者一上来就被复杂度淹没。 为降低 … 1、LangChain 是什么 LangChain 是一个框架，用于开发由 LLM 驱动的应用程序。可以简单认为是 LLM 领域的 Spring，以及开源版的 ChatGPT 插件系统。核心的 2 个功能为： 1）可以将 LLM 模型与外 … 若对接海外服务和工具生态，n8n 拥有更广泛的集成能力，适合自动化场景；不过在国内生态支持方面相对较弱。 对于有开发经验、希望从底层进行定制或二次开发的用户来说，LlamaIndex 和 LangChain …&quot;
  }
]
```

## create_agent 简化 Agent 构建

通过上面的示例，我们可以看到 `create_agent` 大大简化了 `Agent` 的构建过程。`Agent` 会自动进行推理循环，调用合适的工具，直到完成任务，开发者无需编写复杂的状态管理逻辑。

## Middleware 机制

此外，版本还引入了强大的 `middleware` 机制，可以在模型调用的不同阶段插入自定义逻辑

+   `before_model`：模型调用前的预处理
+   `modify_model_request`：动态修改模型请求参数
+   `after_model`：模型调用后的后处理

这为实现细粒度控制提供了可能，比如动态调整提示词、实现自定义缓存、添加安全过滤等。

## 核心改动与市场地位

`LangChain` 的核心改动其实是更加兼容了`LangGraph`的设计理念，以及进一步降低各个模型和服务的兼容问题，做到更好的大一统服务环境处理，该举动也必将在`Agent`开发市场中占有越来越高的地位。
