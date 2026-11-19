
# 多智能体协同办公

> 来源：[https://v11enp9ok1h.feishu.cn/docx/HBixdpHzkojd9UxnGD2cvadanmg](https://v11enp9ok1h.feishu.cn/docx/HBixdpHzkojd9UxnGD2cvadanmg)

ChatGPT 具有深度研究功能。它可以帮助你获取任何主题的详细见解。了解如何构建一个 100% 本地的替代方案。

![](img/498fded5f448f33b7e2bc566670dc382.png)

## 技术栈

+ Linkup 平台：用于深度网络研究

+ CrewAI：用于多智能体编排

+ Ollama：本地部署 DeepSeek 模型

+ Cursor：作为 MCP 主机

## 工作流程

+ 用户提交查询

+ 网络搜索代理通过 Linkup 执行深度网络搜索

+ 研究分析师验证并去重结果

+ 技术作家撰写带有引用的连贯响应

### 设置 LLM

我们将使用 Ollama 在本地部署 DeepSeek-R1。

![](img/06c6d25331b651b405f50d91cf1a5088.png)

```
import os
from crewai import LLM

def get_llm_client():
    """初始化并返回LLM客户端"""
    return LLM(
        model="ollama/deepseek-r1:7b",
        base_url="http://localhost:11434"
    )
```

### 定义网络搜索工具

我们将使用 Linkup 平台强大的搜索能力（可与 Perplexity 和 OpenAI 相媲美）为我们的网络搜索代理提供支持。这通过定义代理可以使用的自定义工具来实现。

![](img/fcb6a16543209e42322aba80112fc225.png)

```
import os
from typing import Type
from pydantic import BaseModel, Field
from linkup import LinkupClient
from crewai.tools import BaseTool

class LinkupSearchInput(BaseModel):
    """Linkup搜索的输入模式"""
    query: str = Field(description="要执行的搜索查询")
    depth: str = Field(default="standard", description="搜索深度：'standard'或'deep'")
    output_type: str = Field(
        default="searchResults",
        description="输出类型：'searchResults'或'sourcedAnswer'"
    )

class LinkupSearchTool(BaseTool):
    name: str = "Linkup Search"
    description: str = "使用Linkup从网络检索信息并返回结果"
    args_schema: Type[BaseModel] = LinkupSearchInput

    def _run(self, query: str, depth: str = "standard", output_type: str = "searchResults") -> str:
        """执行Linkup搜索并返回结果"""
        # 从环境变量初始化Linkup客户端
        linkup_client = LinkupClient(api_key=os.getenv("LINKUP_API_KEY"))
        
        # 执行搜索
        search_response = linkup_client.search(query=query,
                                               depth=depth,
                                               output_type=output_type)
        
        return str(search_response)
```

### 定义网络搜索Agent

网络搜索代理根据用户查询从互联网收集最新信息。此代理使用我们之前定义的 Linkup 工具。

![](img/7d6e2af12911ffda2b6b841e4a80d122.png)

```
from crewai import Agent, Task

linkup_search_tool = LinkupSearchTool()
client = get_llm_client()

web_searcher = Agent(
    role="Web Searcher",
    goal="Retrieve relevant info with citations (source URLs)",
    backstory="Expert searcher; forwards results to Research Analyst only.",
    verbose=True,
    allow_delegation=True,
    tools=[linkup_search_tool],
)

search_task = Task(
    description=f"Search for comprehensive information about: {{query}}.",
    agent=web_searcher,
    expected_output="Detailed raw search results including sources (urls).",
    tools=[linkup_search_tool]
)
```

### 定义研究分析师Agent

该代理将原始网络搜索结果转换为结构化见解，并附带来源 URL。它还可以将任务委派回网络搜索代理进行验证和事实核查。

![](img/1b1231b225f46e1a77d889b4447a03a8.png)

```
from crewai import Agent, Task

# 定义研究分析师
research_analyst = Agent(
    role="Research Analyst",
    goal="Turn raw info into structured insights with URLs.",
    backstory="Expert analyst; can delegate fact-checks to Web Searcher; hands final output to Technical Writer.",
    verbose=True,
    allow_delegation=True,
    llm=client,
)

analysis_task = Task(
    description="Analyze search results, extract insights, and verify facts.",
    agent=research_analyst,
    expected_output="Structured insights with verified facts and source URLs.",
    context=[search_task],
)
```

### 定义技术作家代理

该代理接收来自分析师代理的已分析和验证结果，并为最终用户起草带有引用的连贯响应。

![](img/f27fb685e955049f01c3a2b63d899bb4.png)

```
from crewai import Agent, Task

technical_writer = Agent(
    role="Technical Writer",
    goal="Create clear markdown responses with citations and source URLs.",
    backstory="Expert in simplifying complex information.",
    verbose=True,
    allow_delegation=False,
)

writing_task = Task(
    description="Write a clear, organized response based on research.",
    agent=technical_writer,
    expected_output="Comprehensive answer with citations and source URLs.",
    context=[analysis_task],
)
```

### 设置团队

最后，一旦我们定义了所有代理和工具，我们就可以设置并启动我们的深度研究团队。

![](img/86a0888382e5054e34cf7e522bb554db.png)

```
from crewai import Crew, Process

crew = Crew(
    agents=[web_searcher, research_analyst, technical_writer],
    tasks=[search_task, analysis_task, writing_task],
    verbose=True,
    process=Process.sequential
)

result = crew.kickoff(
    inputs={
        "query": "how Small Language Models (SLMs) are Reshaping AI?"
    }
)
```

### 创建 MCP 服务器

现在，我们将把深度研究团队封装在 MCP 工具中。只需几行代码，我们的 MCP 服务器就可以准备就绪。

![](img/c2326a6d2b4a8e1574a98d507ef7329d.png)

```
from mcp.server.fastmcp import FastMCP
from agents import run_research

# 创建FastMCP实例
mcp = FastMCP("crew_research")

@mcp.tool()
def crew_research(query: str) -> str:
    """
    为给定的用户查询运行基于CrewAI的深度研究系统。
    
    参数:
        query (str): 研究查询或问题。
        
    返回:
        str: 来自CrewAI流水线的研究响应。
    """
    return run_research(query)

# 运行服务器
if __name__ == "__main__":
    mcp.run(transport="stdio")
```

### 将 MCP 服务器与 Cursor 集成

前往：文件 → 首选项 → Cursor 设置 → MCP → 添加新的全局 MCP 服务器

在 JSON 文件中，添加如下所示的内容

![](img/74462c168e85d8b9b1ec9b1c4a14e5f3.png)

### 完成

![](img/88cce8feb4651e16676872d750564246.png)
