
## 模型调用测试与 Agents SDK 功能测试

### 测试调用 GPT-4.1 模型

```
response = client.chat.completions.create(
    model="gpt-4.1",
    messages=[
        {"role": "user", "content": "你好，好久不见!"}
    ]
)
```

```
# 输出生成的响应内容
print(response.choices[0].message.content)
```

你好！很高兴再次见到你！😊 有什么我可以帮忙的吗？

### 测试调用 o4-mini 模型

```
response = client.chat.completions.create(
    model="o4-mini",
    messages=[
        {"role": "user", "content": "你好，好久不见!"}
    ]
)
```

```
# 输出生成的响应内容
print(response.choices[0].message.content)
```

你好！确实好久不见了，最近过得怎么样？有什么新鲜事想和我分享吗？

```
response
```

```
ChatCompletion(id='chatcmpl-BNDW49onkvg8CvaBoqgxu3i0RKFQI', choices=[Choice(finish_reason='stop', index=0, logprobs=None, message=ChatCompletionMessage(content='你好！确实好久不见了，最近过得怎么样？有什么新鲜事想和我分享吗？', refusal=None, role='assistant', annotations=[], audio=None, function_call=None, tool_calls=None))], created=1744873200, model='o4-mini-2025-04-16', object='chat.completion', service_tier='default', system_fingerprint=None, usage=CompletionUsage(completion_tokens=236, prompt_tokens=12, total_tokens=248, completion_tokens_details=CompletionTokensDetails(accepted_prediction_tokens=0, audio_tokens=0, reasoning_tokens=192, rejected_prediction_tokens=0), prompt_tokens_details=PromptTokensDetails(audio_tokens=0, cached_tokens=0)))
```

![](img/57b96c8c0e22a16781aea65733d54c8e.png)

![](img/465b1164f1651aaa49cce2da4f0cafb1.png)

### Agents SDK 功能测试

OpenAI 于 2025 年 3 月发布了全新的 Agents SDK，这是一个轻量级且功能强大的 Python 开源框架，旨在简化多智能体（Multi-Agent）系统的构建与管理。该 SDK 使开发者能够高效地设计、协调和调试复杂的 AI 工作流，广泛适用于客服、自动化办公、编程助手等场景。

Agents SDK 的设计理念是提供足够的功能以满足实际需求，同时保持简洁易用，降低开发门槛。其核心组成包括

+   `Agent`（智能体）配置了指令、工具、输入输出验证（Guardrails）和任务委托（Handoffs）的语言模型

+   `Handoffs`（任务委托）允许智能体将任务委托给其他专门的智能体，适用于多智能体协作

+   `Guardrails`（安全验证）对输入和输出进行验证，确保交互符合预定标准

+   `Tracing`（执行追踪）内置的执行追踪功能，帮助开发者可视化、调试和优化智能体工作流

这些功能使得 SDK 能够支持多种工作流模式，包括确定性流程、迭代循环等，适应不同的应用需求。

```
from openai import AsyncOpenAI
from agents import OpenAIChatCompletionsModel, Agent, Runner, set_default_openai_client, set_tracing_disabled
from agents.model_settings import ModelSettings
from pydantic import BaseModel
from agents import Agent, WebSearchTool
from agents.model_settings import ModelSettings
```

```
external_client = AsyncOpenAI(
    base_url = BASE_URL,
    api_key = API_KEY, 
)
```

```
set_default_openai_client(external_client)
set_tracing_disabled(True)
```

```
agent = Agent(name="Assistant", instructions="你是一名助人为乐的助手。")
```

```
result = await Runner.run(agent, "请写一首关于编程中递归的俳句。") 
```

> 俳（pái）句。

```
print(result.final_output)
```

函数自调用，  
层层嵌套无尽，  
答案渐明。

![](img/b11d5b2ba760d0db52b6a50510848f40.png)

![](img/25a0014731dae89b7fe722b4b78acb98.png)

## 三、从零到一快速搭建 Mini DeepResearch

OpenAI 于 2025 年初推出了 ChatGPT 的全新功能——Deep Research（深度研究），这是一项革命性的 AI 代理能力，旨在帮助用户高效完成复杂的研究任务。通过自动化的信息搜集、分析和报告生成，Deep Research 能在短时间内完成原本需要数小时甚至数天的工作，极大地提升了知识工作的效率和质量。

Deep Research 的核心优势包括：

+   多步骤推理与信息整合 利用最新的 OpenAI o3 模型，Deep Research 能够从多个来源收集信息，进行深入分析，并整合成结构化的报告

+   自动化研究流程 用户只需提出研究主题，AI 会自动规划研究步骤，执行信息搜集与分析，最终生成详尽的报告

+   引用来源明确 生成的报告中包含清晰的引用来源，便于用户进行验证和进一步阅读

+   支持文件上传 用户可以上传相关文档或数据表格，AI 将结合这些信息进行更精准的分析

+   适用多种领域 无论是市场调研、政策分析、科学研究，还是个人决策支持，Deep Research 都能提供有力的支持

![](img/eef6baf6d4a25cc242973d1815eddc6f.png)

### Mini DeepResearch 视频演示

```
from IPython.display import Video
```

```
Video("https://ml2022.oss-cn-hangzhou.aliyuncs.com/%E6%BC%94%E7%A4%BA%E8%A7%86%E9%A2%91.mp4", width=800, height=400)
```

Your browser does not support the video element.

### 项目规划

目标：构建一个“研究型助手”，可以：

1. 接收一个研究主题（如“AI 对教育的影响”）
2. 自动规划应该搜索的子问题/关键词
3. 每个关键词在网上搜索信息、摘要
4. 把多个摘要汇总成一份完整的研究报告

也就是说，这是一个「具备规划能力 + 工具调用 + 总结归纳」能力的系统。

### 项目架构

![](img/9d48e9cae8be2e2b99e1bc75a7f84378.png)

三个核心 Agent 简介

| Agent 名 | 职责 |
| --- | --- |
| `planner_agent` | 生成研究关键词和搜索策略 |
| `search_agent` | 负责执行网络搜索 + 总结内容（使用工具） |
| `writer_agent` | 汇总所有搜索结果，编写报告 |

### Planner Agent 执行流程

```
PROMPT = (
    "You are a helpful research assistant. Given a query, come up with a set of web searches "
    "to perform to best answer the query. Output between 10 and 20 terms to query for."
)


class WebSearchItem(BaseModel):
    reason: str
    "Your reasoning for why this search is important to the query."

    query: str
    "The search term to use for the web search."


class WebSearchPlan(BaseModel):
    searches: list[WebSearchItem]
    """A list of web searches to perform to best answer the query."""


planner_agent = Agent(
    name="PlannerAgent",
    instructions=PROMPT,
    model="o4-mini",
    output_type=WebSearchPlan,
)
```

```
planner_agent_res = await Runner.run(planner_agent, "我想深入了解下MCP（Model Context Protocol）这项技术。") 
```

```
planner_agent_res.final_output
```

```
WebSearchPlan(searches=[WebSearchItem(reason='查找MCP（Model Context Protocol）的基础概念与定义', query='MCP Model Context Protocol overview'), WebSearchItem(reason='获取MCP官方文档或规范手册', query='Model Context Protocol MCP documentation'), WebSearchItem(reason='下载MCP协议规范的PDF或白皮书', query='MCP protocol spec PDF'), WebSearchItem(reason='了解MCP的系统架构和设计原理', query='MCP Model Context Protocol architecture'), WebSearchItem(reason='探索MCP在不同场景下的应用案例', query='MCP Model Context Protocol use cases'), WebSearchItem(reason='查找如何实现MCP协议的技术细节', query='Implementing MCP Model Context Protocol'), WebSearchItem(reason='寻找MCP的API调用示例和代码片段', query='MCP API examples'), WebSearchItem(reason='评估MCP协议的性能表现', query='MCP Model Context Protocol performance benchmarks'), WebSearchItem(reason='研究MCP协议的安全性与最佳实践', query='MCP security best practices'), WebSearchItem(reason='对比MCP与其他上下文管理协议的异同', query='MCP vs other context protocols'), WebSearchItem(reason='寻找MCP的入门教程和快速上手指南', query='MCP tutorial'), WebSearchItem(reason='查找学术论文中关于MCP的研究', query='MCP Model Context Protocol research paper'), WebSearchItem(reason='搜索MCP的开源实现库', query='MCP open source library'), WebSearchItem(reason='阅读中文相关文章以获取本地化解读', query='MCP 模型 上下文 协议 文章'), WebSearchItem(reason='获取MCP中文白皮书或技术报告', query='MCP 模型 上下文 协议 白皮书')])
```
