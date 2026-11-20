
```python
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
