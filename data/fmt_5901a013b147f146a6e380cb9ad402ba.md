
# 基于MCP架构的金融数据分析智能系统

> 来源：[https://v11enp9ok1h.feishu.cn/docx/DriydXc7sooP4rxTbvyclNMpnhc](https://v11enp9ok1h.feishu.cn/docx/DriydXc7sooP4rxTbvyclNMpnhc)

## Cursor + MCP 金融分析 Agent

构建一个 `MCP` 驱动的 AI Agent，直接从 `Cursor` 或 `Claude Desktop` 获取、分析股票市场趋势并生成洞察。

![](img/63fed83fc23dea43ee26fa7577ae6595.png)

## 技术栈

+   `CrewAI` for multi-agent orchestration

`CrewAI` 用于多代理编排

+   `Ollama` to locally serve DeepSeek-R1 LLM

`Ollama` 本地部署 DeepSeek-R1 大语言模型

+   `Cursor` as the MCP host

`Cursor` 作为 `MCP` 主机

## 工作流程

1.  用户提交查询。

1.  `MCP` 代理启动金融分析师团队。

1.  团队进行研究并创建可执行脚本。

1.  代理运行脚本生成分析图表。

## 设置 LLM

我们将使用 DeepSeek-R1 作为大语言模型，通过 `Ollama` 在本地部署。

![](img/c1869ea83ef89bb798c7ee5a60777593.png)

```
from crewai import LLM

llm = LLM(
    model="ollama/deepseek-r1:7b",
    base_url="http://localhost:11434"
)
```

## 查询解析器代理

该代理接受自然语言查询，并使用 `Pydantic` 提取结构化输出。这确保了后续处理的输入干净且结构化！

![](img/0120b555bd7cd65f67bf05a9b1851875.png)

```
from pydantic import BaseModel, Field
from crewai import Agent, Task

class QueryAnalysisOutput(BaseModel):
    """查询分析任务的结构化输出。"""
    symbols: list[str] = Field(..., description="股票代码列表")
    timeframe: str = Field(..., description="时间周期")
    action: str = Field(..., description="要执行的操作")

query_parser_agent = Agent(
    role="股票数据分析师",
    goal="从{query}中提取股票详情并获取所需数据。",
    backstory="专注于股票市场数据检索的金融分析师。",
    llm=llm
)

query_parsing_task = Task(
    description="分析用户查询并提取股票详情。",
    expected_output="包含'symbols'、'timeframe'、'action'键的字典。",
    output_pydantic=QueryAnalysisOutput,
    agent=query_parser_agent,
)
```

## 代码编写 Agent

该 Agent 使用 `Pandas`、`Matplotlib` 和 `Yahoo Finance` 库编写 `Python` 代码以可视化股票数据。

![](img/538e01a865d4e38545ae5fb82492c629.png)

```
from crewai import Agent, Task

code_writer_agent = Agent(
    role="高级Python开发者",
    goal="编写Python代码以可视化股票数据。",
    backstory="""专注于股票市场数据可视化和编写生产级Python代码的高级Python开发者。""",
    llm=llm
)

code_writer_task = Task(
    description="编写生产级Python代码以可视化股票数据。",
    expected_output="用于股票可视化的可执行Python脚本。",
    agent=code_writer_agent,
)
```

## 代码执行

![](img/70c18f984086b05aa4fc4d6781866fa1.png)

```
from crewai import Agent, Task
from crewai_tools import CodeInterpreterTool

code_execution_agent = Agent(
    role="高级代码执行专家",
    goal="审查并执行代码编写代理编写的Python代码。",
    backstory="擅长执行Python代码。",
    tools=[CodeInterpreterTool()],
    allow_code_execution=True,
    allow_delegation=True,
    llm=llm
)

code_execution_task = Task(
    description="审查并执行用于股票数据可视化的Python代码。",
    expected_output="干净且可执行的Python脚本文件（.py）。",
    agent=code_execution_agent,
)
```

## 设置团队并启动

![](img/0bcefdbc6b98cc385e339747fe8ff302.png)

```
from crewai import Crew, Process

crew = Crew(
    agents=[query_parser_agent, code_writer_agent, code_execution_agent],
    tasks=[query_parsing_task, code_writer_task, code_execution_task],
    process=Process.sequential
)

# 使用示例查询运行团队
result = crew.kickoff(
    inputs={"query": "绘制特斯拉与英伟达的年初至今股票涨幅"}
)
```

## 创建 MCP 服务器

现在，我们将金融分析师封装在 `MCP` 工具中，并添加两个工具以增强用户体验。

+   `save_code` -> 将生成的代码保存到本地目录

+   `run_code_and_show_plot` -> 执行代码并生成图表

![](img/fc3abb258be337b7ef2a142ff8a3c5ce.png)

```
from mcp.server.fastmcp import FastMCP
from finance_crew import run_financial_analysis

mcp = FastMCP("financial-analyst")

@mcp.tool()
def analyze_stock(query: str) -> str:
    """基于查询分析股票市场数据并生成可执行的Python代码用于分析和可视化"""
    result = run_financial_analysis(query)
    return result

@mcp.tool()
def save_code(code: str) -> str:
    """将给定代码保存到文件stock_analysis.py"""
    with open('stock_analysis.py', 'w') as f:
        f.write(code)
    return "代码已保存到stock_analysis.py"

@mcp.tool()
def run_code_and_show_plot() -> str:
    """执行代码并生成图表"""
    with open('stock_analysis.py', 'r') as f:
        exec(f.read())
    return "代码执行完毕并生成图表"

if __name__ == "__main__":
    mcp.run(transport="stdio")
```
