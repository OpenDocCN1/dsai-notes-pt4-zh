
# 多智能体研究系统代码实现

## 环境配置与基础客户端

```
from openai import AsyncOpenAI
from agents import OpenAIChatCompletionsModel,Agent,Runner,set_default_openai_client, set_tracing_disabled
from agents.model_settings import ModelSettings
from pydantic import BaseModel
from agents import Agent, WebSearchTool
from agents.model_settings import ModelSettings
import os
from dotenv import load_dotenv
load_dotenv(override=True)

external_client = AsyncOpenAI(
    base_url = os.getenv("BASE_URL"),
    api_key = os.getenv("API_KEY"), 
)
set_default_openai_client(external_client)
set_tracing_disabled(True)
agent = Agent(name="Assistant", instructions="你是一名助人为乐的助手。")
result = await Runner.run(agent, "请写一首关于编程中递归的俳句。") 
print(result.final_output)
```

这里需要在`base_url`中填写国内反向代理地址，需要在`api_key`中填写`OpenAI`的`API-KEY`。实际运行结果如下：

![](img/2f23e111f73dcc61525569af9e55cb87.png)

## 2.3 planner_agent代码详解与功能介绍

接下来创建`planner_agent`，具体代码如下：

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

其中`Planner Agent` 的职责是接收一个研究主题，生成一份“搜索计划（`WebSearchPlan`）”，告诉系统应该搜索哪些子问题/关键词以及搜索这些关键词的理由。具体代码解释如下：

```
from pydantic import BaseModel
```

导入 `Pydantic`，用于定义结构化数据模型。它是整个 Agents SDK 中用于类型校验 + 数据结构定义的核心库。

```
from agents import Agent
```

导入 SDK 的核心类 `Agent`，我们后面会实例化一个具体的 `Agent` 实体 `planner_agent`。

```
PROMPT = (
    "You are a helpful research assistant. Given a query, come up with a set of web searches "
    "to perform to best answer the query. Output between 20 and 30 terms to query for."
)
```

定义这个 `Agent` 的提示词（`Prompt`），告诉 `LLM`：

+   你是一个研究助手；

+   收到一个主题后，请生成 20 到 30 条搜索建议；

+   每条建议应当包括 `搜索关键词 + 搜索原因`。

接下来是两个重要的数据结构定义：

```
class WebSearchItem(BaseModel):
    reason: str
    "Your reasoning for why this search is important to the query."

    query: str
    "The search term to use for the web search."
```

这是一个“单个搜索建议”的结构，包含两个字段：

| 字段 | 说明 |
| --- | --- |
| `reason` | 你为什么要搜索这个关键词？（用于解释搜索动机） |
| `query` | 你要搜索的关键词本身 |

```
class WebSearchPlan(BaseModel):
    searches: list[WebSearchItem]
    """A list of web searches to perform to best answer the query."""
```

这是“搜索计划”的整体结构，包含一个列表 `searches`，每项是上面的 `WebSearchItem`。

其中模型输出结构化的关键在这：

```
output_type=WebSearchPlan,
```

它告诉 `planner_agent` “你必须输出一份结构化的 `WebSearchPlan` 对象，里面包含多个 `WebSearchItem`。”这样可以约束模型输出格式，也便于下一步自动解析内容。

![](img/4558bb7a68e1a45d28ed192747989655.png)

输出了14个搜索关键词和对应的搜索原因。

## 2.4 search_agent功能实现

接下来是负责实际执行搜索工作的`search_agent`，代码如下：

```
from agents import Agent, WebSearchTool
from agents.model_settings import ModelSettings

INSTRUCTIONS = (
    "You are a research assistant. Given a search term, you search the web for that term and "
    "produce a concise summary of the results. The summary must 2-3 paragraphs and less than 300 "
    "words. Capture the main points. Write succinctly, no need to have complete sentences or good "
    "grammar. This will be consumed by someone synthesizing a report, so its vital you capture the "
    "essence and ignore any fluff. Do not include any additional commentary other than the summary "
    "itself."
)

search_agent = Agent(
    name="Search agent",
    instructions=INSTRUCTIONS,
    tools=[WebSearchTool()],
    model_settings=ModelSettings(tool_choice="required"),
)
```

`search_agent` 的作用是接收一个搜索关键词，调用 `Web` 搜索工具，然后根据搜索结果生成一份简洁的摘要（2-3 段，<300 词），不带评论，只保留信息本身。也就是说，这是整个系统中真正“去网上查资料”的角色。具体代码解释如下：

```
from agents import Agent, WebSearchTool
from agents.model_settings import ModelSettings
```

其中：

+   `Agent`：导入 SDK 的智能体基类；

+   `WebSearchTool`：一个由 Agents SDK 内置的网页搜索工具，调用它可以模拟 “上网搜索” 的效果；

+   `ModelSettings`：可以配置一些模型使用时的行为，比如是否必须使用工具。

然后是重点提示词 `INSTRUCTIONS`

```
INSTRUCTIONS = (
    "You are a research assistant. Given a search term, you search the web for that term and"
    "produce a concise summary of the results. The summary must 2-3 paragraphs and less than 300"
    "words. Capture the main points. Write succinctly, no need to have complete sentences or good"
    "grammar. This will be consumed by someone synthesizing a report, so its vital you capture the"
    "essence and ignore any fluff. Do not include any additional commentary other than the summary"
    "itself."
)
```

这是模型的行为提示，翻译一下这段内容的精髓：

| 指令含义 | 说明 |
| --- | --- |
| 你是一个研究助手 | 模拟一个能查资料的人 |
| 给你关键词后上网搜索 | 关键词来自 `planner_agent` |
| 写出 2-3 段简洁总结 | 每次搜索结果必须压缩成 300 字以内的摘要 |
| 用要点式语言、可以语法不完整 | 不要求像论文，重点是信息密度高 |
| 不要添加自己的评论 | 不能主观判断，只提取信息 |
| 最终目的是为后续写报告的人准备素材 | 所以格式自由，内容集中就行 |

接下来创建创建 `Agent` 对象

```
search_agent = Agent(
    name="Search agent",
    instructions=INSTRUCTIONS,
    tools=[WebSearchTool()],
    model_settings=ModelSettings(tool_choice="required"),
)
```

其中`name="Search agent"`是`Agent` 名称，用于日志和上下文显示。而`instructions=INSTRUCTIONS`告诉大语言模型它要怎么完成任务。`tools=[WebSearchTool()]`设置它可以使用的工具。在这个 case 中，它只能用 `WebSearchTool`，这是 SDK 内置的网页搜索工具。而`model_settings=ModelSettings(tool_choice="required")`则表示：强制要求模型一定要调用工具（`WebSearchTool`）来完成任务。不允许模型“凭空回答”或“胡编搜索内容”，这有助于提升真实性。

总结流程如下：

```
关键词（来自 `planner_agent`） → `search_agent` →
     🔍 使用 `WebSearchTool` 搜索
     ✂️ 根据结果生成 2-3 段摘要
     📦 返回给 `writer_agent` 写整合报告
```

带入`planner_agent`的一个搜索条目后运行效果如下：

![](img/a8da49f9c52d00833775e3922c4bec02.png)

## 2.5 writer_agent功能实现

最后则是`writer_agent`的功能介绍，其核心代码如下：

```
from pydantic import BaseModel
from agents import Agent

PROMPT_TEMP = (
    "You are a senior researcher tasked with writing a cohesive report for a research query. "
    "You will be provided with the original query, and some initial research done by a research "
    "assistant.\n"
    "You should first come up with an outline for the report that describes the structure and "
    "flow of the report. Then, generate the report and return that as your final output.\n"
    "The final output should be in markdown format, and it should be lengthy and detailed. Aim "
    "for 10-20 pages of content, at least 1500 words."
    "最终结果请用中文输出。"
)


class ReportData(BaseModel):
    short_summary: str
    """A short 2-3 sentence summary of the findings."""

    markdown_report: str
    """The final report"""

    follow_up_questions: list[str]
    """Suggested topics to research further"""


writer_agent = Agent(
    name="WriterAgent",
    instructions=PROMPT_TEMP,
    model="o4-mini",
    output_type=ReportData,
)
```

`Writer Agent`这是 **整个研究系统的“输出终结者”**，负责把之前所有搜索到的信息，综合成一份完整、结构化、可阅读的长篇报告。该 `Agent` 的任务是：

+   收到研究主题和之前的所有搜索摘要；

+   先写一个大纲（`outline`）；

+   然后根据大纲写出一份 详细的 `Markdown` 格式报告；

+   同时生成一个简短总结和一些后续可以研究的问题。

具体代码解释如下：

```
# Agent used to synthesize a final report from the individual summaries.
from pydantic import BaseModel
from agents import Agent
```

+   和前两个 `Agent` 一样，我们导入了：

+   `BaseModel`：用于定义结构化输出；

+   `Agent`：`Agent` SDK 中的核心类。

然后是提示词定义（`Prompt`）
