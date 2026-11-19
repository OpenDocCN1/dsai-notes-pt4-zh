
# TradingAgents：多智能体LLM金融Agent

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/AiaodXIcioiS7Txr0HMcduI6nu3](https://qqs7y1hozd1.feishu.cn/docx/AiaodXIcioiS7Txr0HMcduI6nu3)

## TradingAgents 框架

`TradingAgents` 是一个模拟真实交易公司运作模式的多智能体交易框架。`TradingAgents` 和传统的量化交易或单一 AI 模型的不同在于—“多智能体（Multi-Agent）”的 LLM 金融交易框架。

![](img/9da6fe97564c3f17666a764b5e6e71b0.png)

可以把 `TradingAgents` 想象成一家顶级的投资公司，只不过里面的“员工”全都是由 AI 智能体扮演！每个智能体都拥有特定的专业知识和角色，能够进行动态讨论和协同决策。

### 不同视角的分析师团队

这支团队类似公司的“千里眼”，洞察市场，这些分析师智能体被赋予了特定的观察和推理能力：

> 基本面分析师：评估公司财务和业绩指标，识别内在价值和潜在风险信号

> 情绪分析师：运用情绪评分算法分析社交媒体和公众情绪，研判短期市场情绪

> 新闻分析师：监测全球新闻和宏观经济指标，解读事件对市场状况的影响

> 技术分析师：运用MACD、RSI等技术指标识别交易模式并预测价格走势

![](img/7b6caaf0619741c90dc38e5ee03b9bd0.png)

### 专门辩论的研究员

让不同观点的看涨研究员和看跌研究员进行结构化辩论：

![](img/100400898ec3c707d27458aeb05a8dae.png)

+   看涨研究员：积极寻找市场中的利好因素，例如行业发展机遇、公司创新成果等，构建支持投资的逻辑，并提供论据支持。

+   看跌研究员：捕捉潜在的风险信号和不利因素，例如宏观经济下行压力、公司竞争劣势等，并提出反驳意见和风险警示。

### 操盘手交易员

![](img/799545357d6e7241b48a651023d45d75.png)

交易员智能体会综合考虑：

> 市场深度和流动性： 决定交易规模，避免对市场造成不必要的冲击。

> 风险偏好： 根据预设的风险敞口，调整交易激进程度。

> 时间窗口： 精准把握交易时机，力求实现回报最大化。

一旦市场出现符合预设交易条件的信号，交易员智能体就会迅速下达买入或卖出指令，并根据市场的实时变化灵活调整投资组合。

### 风险管理团队

在金融市场，风险控制永远是重中之重！

研究团队构建了专门的风险管理智能体团队，实时监控持仓情况和市场波动，通过设置止损订单等手段，严格控制投资风险，利用价值风险（VaR）、条件风险值（CVaR）等量化风险指标，确保所有交易活动都在预设的风险参数范围内进行。

![](img/6b6f36384ca3e952d49f1e4845a6ae04.png)

例如，当某只股票的价格出现显著下跌，并接近预设的止损价位时，风险管理团队会及时提醒交易员智能体采取相应的应对措施，以最大限度地减少潜在损失。

## 核心技术解密

`TradingAgents` 的论文中详细阐述了几个关键的技术亮点：

### `LangGraph` 驱动的工作流

`TradingAgents` 采用了先进的“工厂模式（Factory Pattern）”来创建智能体，结合 `LangGraph` 构建了动态工作流，让不同的 LLM 智能体之间进行有状态的对话和决策路径切换，形成智能体之间的“高速公路”。

这种高度模块化和可配置的设计，使得系统能够：

+   根据不同的金融场景，轻松配置各种 LLM 模型，例如 GPT-4、o1-preview 等，甚至可以集成本地部署的开源 LLM。

+   支持实时市场数据或历史缓存数据作为分析来源。

+   允许用户自由设定辩论轮次，完美适应各种复杂多变的金融交易场景。

![](img/d21337a9006b5a496fc945ff8afe9a37.png)

### 多模态数据融合：让 AI 看到“全景图”

`TradingAgents` 具备强大的多模态数据融合能力。它能同时融合：

+   结构化数据：如财务报表、交易数据、宏观经济指标等。

+   非结构化文本数据：如新闻报道、社交媒体信息、公司公告等。

从这些复杂且多样化的数据中精准提取关键信息，`TradingAgents` 能够记那些跨模态的关联分析，提升市场预测的准确性。

![](img/747c86e0e26227a76b7c60c2ae64168d.png)

### 动态决策与持续学习进化

通过回测奖励机制可以根据市场的实时变化快速调整策略，实现持续学习进化。

`TradingAgents` 构建了持续学习的决策链条：

+   实时反馈：根据实际的市场表现，接收到实时反馈。

+   奖励信号：成功的交易或有效的风险控制会生成正向奖励信号。

+   模型优化：这些奖励信号被用于持续优化 LLM 的提示、智能体间的交互规则，以及决策参数，形成一个闭环学习的良性循环。

这就像一个经验丰富的交易员，在每一次交易中不断复盘、学习、成长！

研究团队喂给了 `TradingAgents` 以下相关内容，包括：

+   实时股价 & 60+ 技术指标（MACD、RSI 等）

+   彭博/雅虎财经等专业新闻

+   Reddit/推特社交情绪分析

+   公司财报、高管交易数据

+   宏观经济和政府政策更新

然后将 `TradingAgents` 的表现与五大传统策略（比如“买入持有”、MACD 均线、KDJ+RSI 组合等）进行了对比。`TradingAgents` 平均碾压基线策略 6-24 个百分点。

![](img/9edb04f2e352aa9656a6a82e76d6548d.png)

在模拟盘中高收益下最大亏损 (回撤) 仅 0.91%-2.11%，接近风控最强的保守策略； 收益风险比（夏普比率）高达 5.6-8.21（传统策略普遍 <3.5，超过 3 已是顶级水平）。

## 部署方式

### 安装

克隆 `TradingAgents` 仓库：

使用您偏好的环境管理工具创建虚拟环境：

```
conda create -n tradingagents python=3.13
conda activate tradingagents
```

安装依赖项：

```
pip install -r requirements.txt
```

### 所需API

所有智能体都需要 OpenAI API，基本面数据和新闻数据（默认配置）需要 Alpha Vantage API。

```
export OPENAI_API_KEY=$YOUR_OPENAI_API_KEYexport ALPHA_VANTAGE_API_KEY=$YOUR_ALPHA_VANTAGE_API_KEY
```

或者，您可以在项目根目录中创建一个包含 API 密钥的 `.env` 文件（请参考 `.env.example`）：

```
cp .env.example .env# Edit .env with your actual API keys
```

### 命令行使用

可直接运行CLI：

```
python -m cli.main
```

界面将显示可选参数：股票代码、日期、大语言模型、研究深度等。

运行时会实时显示加载结果，可追踪代理执行进度。

![](img/42b6791b5483882ac603cede2ec8f46b.png)

## `TradingAgents`包

### 实现细节

采用`LangGraph`构建`TradingAgents`以保证灵活性和模块化。实验中使用o1-preview和gpt-4o分别作为深度思考与快速思考的大语言模型。但测试时建议使用o4-mini和gpt-4.1-mini以节省成本，因本框架会发起大量API调用。

### Python调用

在代码中导入`tradingagents`模块并初始化`TradingAgentsGraph()`对象。`.propagate()`函数将返回决策结果。可运行`main.py`，以下是快速示例：

```
from tradingagents.graph.trading_graph import TradingAgentsGraph
from tradingagents.default_config import DEFAULT_CONFIG

ta = TradingAgentsGraph(debug=True, config=DEFAULT_CONFIG.copy())

# forward propagate
_, decision = ta.propagate("NVDA", "2024-05-10")
print(decision)

```

您也可以调整默认配置，设置您偏好的大语言模型（LLMs）、辩论轮次等参数。

```
from tradingagents.graph.trading_graph import TradingAgentsGraph
from tradingagents.default_config import DEFAULT_CONFIG

# Create a custom config
config = DEFAULT_CONFIG.copy()
config["deep_think_llm"] = "gpt-4.1-nano"  # Use a different model
config["quick_think_llm"] = "gpt-4.1-nano"  # Use a different model
config["max_debate_rounds"] = 1  # Increase debate rounds

# Configure data vendors (default uses yfinance and Alpha Vantage)
config["data_vendors"] = {
    "core_stock_apis": "yfinance",           # Options: yfinance, alpha_vantage, local
    "technical_indicators": "yfinance",      # Options: yfinance, alpha_vantage, local
    "fundamental_data": "alpha_vantage",     # Options: openai, alpha_vantage, local
    "news_data": "alpha_vantage",            # Options: openai, alpha_vantage, google, local
}

# Initialize with custom config
ta = TradingAgentsGraph(debug=True, config=config)

# forward propagate
_, decision = ta.propagate("NVDA", "2024-05-10")
print(decision)

```

**【重要提醒：`TradingAgents` 框架仅供研究用途哦。它的交易表现可能会因为模型、参数、数据等多种因素而异，而且它不构成财务、投资或交易建议！咱们玩归玩，闹归闹，投资有风险，入市需谨慎！】**
