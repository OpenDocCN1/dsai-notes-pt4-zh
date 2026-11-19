
手动执行股票市场预测和分析可能是一项繁琐且耗时的任务。相反，想象一下能够简单地问：“现在 MSFT 的 RSI 是多少？”

MCP服务器可以立即获取最新的股票数据、计算 RSI 并返回结果——这使得做出明智的交易决策变得更加容易，而无需在多个应用程序和网站之间切换。

在本节中，我们将使用 Alpha Vantage API提取实时股票数据并将其集成到 MCP 服务器中。这种集成使我们能够使用定制的 AI 工具分析股票。

## 安装依赖项

对于单独的项目，请创建一个新的 `uv` 项目，并使用以下命令添加依赖项。

```
# Create a new directory for our project
uv init finance
cd finance

# Create virtual environment and activate it
uv venv
.venv\Scripts\activate

# Install dependencies
uv add mcp[cli] requests pandas tabulate
```

对于现有项目（`mcp_demo`），请使用 `PIP` 命令安装 MCP 和 `httpx` Python 软件包。

```
pip install requests pandas tabulate
```

## 8.3 使用 Alpha Vantage API 获取股票市场数据

Alpha Vantage可提供实时和历史金融市场数据，它提供了一系列 API 来访问有关股票、货币、加密货币等的信息。

要开始使用 Alpha Vantage，您需要在他们的官方网站（`https://www.alphavantage.co/support/#api-key`）上注册并获得免费的 API 密钥。免费套餐每天最多允许 25 个 API 请求。获得 API 密钥后，您可以使用 `TIME_SERIES_INTRADAY` 端点检索当日股票价格数据。此 API 返回实时更新的时间序列数据，其中包含关键指标（如开盘价、最高价、最低价、收盘价和成交量）。

### 需要指定如下参数：

+ 股票代码；
+ 数据点之间的间隔；
+ API Key；

### API调用示例如下：

```
https://www.alphavantage.co/query?function=TIME_SERIES_INTRADAY&symbol=MSFT&interval=5min&apikey=YOUR_API_KEY
```

这会调用返回 Microsoft 股票的最新 5 分钟间隔数据，然后就可以分析这些数据并在应用程序或分析工作流程中使用这些数据。

![](img/5fb86afbd2a35d8c88bce456a2ac02ea.png)

## 8.4 使用MCP Tools进行股票分析

我们使用MCP工具来进行股票分析。此时，MCP服务器将创建移动平均线、相对强弱指数和交易建议工具。

### Tool 1：移动平均线

移动平均线用于股票分析，以平滑价格数据并识别趋势。

+ 短期平均值（5-10 天）反应迅速，凸显了近期的市场变化。
+ 长期平均值（50-200 天）变化缓慢，并揭示出更广泛、持续的趋势。

用于计算移动平均线的实现工具如下所示：

```
from mcp import tool  # 需确保正确导入 mcp 相关装饰器，根据实际库结构调整
import pandas as pd
from datetime import datetime
# 以下两个需根据实际实现补充，这里模拟定义
class AlphaVantageAPI:
    @staticmethod
    def get_intraday_data(symbol, interval, outputsize):
        # 实际应实现调用 Alpha Vantage API 获取数据逻辑，这里返回模拟空 DataFrame
        return pd.DataFrame()
class MarketData:
    def __init__(self, symbol, interval, data, last_updated):
        self.symbol = symbol
        self.interval = interval
        self.data = data
        self.last_updated = last_updated

market_data_cache = {}  # 初始化缓存字典

@tool()  # 假设 mcp.tool 装饰器可这样导入使用，若实际有不同命名空间需调整
def calculate_moving_averages(symbol: str, short_period: int = 20, long_period: int = 50) -> dict:
    """
    Calculate short and long moving averages for a symbol
    
    Args:    
        symbol: The ticker symbol to analyze        
        short_period: Short moving average period in minutes        
        long_period: Long moving average period in minutes            
    Returns:    
        Dictionary with moving average data and analysis    
    """
    cache_key = f"{symbol}_1min"
    
    if cache_key not in market_data_cache:
        df = AlphaVantageAPI.get_intraday_data(symbol, "1min", outputsize="full")
        market_data_cache[cache_key] = MarketData(
            symbol=symbol,
            interval="1min",
            data=df,
            last_updated=datetime.now()
        )
        
    data = market_data_cache[cache_key].data
    
    # 处理数据为空情况，避免后续计算报错，这里简单返回空结果，实际可优化
    if data.empty:
        return {"error": "No data available for the given symbol"}
    
    # Calculate moving averages
    data[f'SMA{short_period}'] = data['close'].rolling(window=short_period).mean()
    data[f'SMA{long_period}'] = data['close'].rolling(window=long_period).mean()
    
    # Get latest values
    latest = data.iloc[-1]
    current_price = latest['close']
    short_ma = latest[f'SMA{short_period}']
    long_ma = latest[f'SMA{long_period}']
    
    # Determine signal
    if short_ma > long_ma:
        signal = "BULLISH (Short MA above Long MA)"
    elif short_ma < long_ma:
        signal = "BEARISH (Short MA below Long MA)"
    else:
        signal = "NEUTRAL (MAs are equal)"
        
    # Check for crossover in the last 5 periods
    last_5 = data.iloc[-5:]
    crossover = False
    crossover_type = ""
    
    for i in range(1, len(last_5)):
        prev = last_5.iloc[i-1]
        curr = last_5.iloc[i]
        
        # Golden Cross (short crosses above long)
        if prev[f'SMA{short_period}'] <= prev[f'SMA{long_period}'] and curr[f'SMA{short_period}'] > curr[f'SMA{long_period}']:
            crossover = True
            crossover_type = "GOLDEN CROSS (Bullish)"
            break
            
        # Death Cross (short crosses below long)
        if prev[f'SMA{short_period}'] >= prev[f'SMA{long_period}'] and curr[f'SMA{short_period}'] < curr[f'SMA{long_period}']:
            crossover = True
            crossover_type = "DEATH CROSS (Bearish)"
            break
            
    return {
        "symbol": symbol,
        "current_price": current_price,
        f"SMA{short_period}": short_ma,
        f"SMA{long_period}": long_ma,
        "signal": signal,
        "crossover_detected": crossover,
        "crossover_type": crossover_type if crossover else "None",
        "analysis": f"""Moving Average Analysis for {symbol}:
Current Price: ${current_price:.2f}
{short_period}-period SMA: ${short_ma:.2f}
{long_period}-period SMA: ${long_ma:.2f}
Signal: {signal}

Recent Crossover: {"Yes - " + crossover_type if crossover else "No"}

Recommendation: {
    "STRONG BUY" if crossover and crossover_type == "GOLDEN CROSS (Bullish)" else
    "BUY" if signal == "BULLISH (Short MA above Long MA)" else
    "STRONG SELL" if crossover and crossover_type == "DEATH CROSS (Bearish)" else
    "SELL" if signal == "BEARISH (Short MA below Long MA)" else
    "HOLD"
}"""
    }
```

### Tool 2：相对强弱指数（RSI）

相对强弱指数（RSI）是一种动量指标，有助于识别资产的超买（RSI > 70）或超卖（RSI < 30）情况。在典型的 14 天周期内计算，它使用平均收益与损失的比率来评估价格变动的速度和变化，有助于做出更好的交易决策。

用于计算 RSI 的实现工具。
