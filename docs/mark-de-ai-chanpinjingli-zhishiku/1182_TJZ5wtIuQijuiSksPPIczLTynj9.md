# Code

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/GqmvdZ68woMAdAxO48Dcxv8Hnof](https://qqs7y1hozd1.feishu.cn/docx/GqmvdZ68woMAdAxO48Dcxv8Hnof)

```
# import necessary python libraries
from agno.agent import Agent
from agno.models.xai import xAI
from agno.tools.yfinance import YFinanceTools
from agno.tools.duckduckgo import DuckDuckGoTools
from agno.playground import Playground, serve_playground_app

# create the AI finance agent
agent = Agent(
    name=&quot;xAI Finance Agent&quot;,
    model = xAI(id=&quot;grok-beta&quot;),
    tools=[DuckDuckGoTools(), YFinanceTools(stock_price=True, analyst_recommendations=True, stock_fundamentals=True)],
    instructions = [&quot;Always use tables to display financial/numerical data. For text data use bullet points and small paragrpahs.&quot;],
    show_tool_calls = True,
    markdown = True,
    )

# UI for finance agent
app = Playground(agents=[agent]).get_app()

if __name__ == &quot;__main__&quot;:
    serve_playground_app(&quot;xai_finance_agent:app&quot;, reload=True)
```