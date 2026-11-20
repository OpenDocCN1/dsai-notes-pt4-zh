
# MCP 服务器列表

+ [`DataEval/dingo`](https://github.com/DataEval/dingo) 🎖️ 🐍 🏠 🍎 🪟 🐧 - Dingo 的 MCP 服务端。Dingo是一款全面的数据质量评估工具。支持与 Dingo 基于规则和 LLM 的评估功能进行交互以及列出可用规则和提示词。

## 📂 文件系统

提供对本地文件系统的直接访问，并具有可配置的权限。使 AI 模型能够读取、写入和管理指定目录中的文件。

+ [`@modelcontextprotocol/server-filesystem`](https://github.com/modelcontextprotocol/servers/tree/main/src/filesystem) 📇 🏠 - 直接访问本地文件系统。

+ [`@modelcontextprotocol/server-google-drive`](https://github.com/modelcontextprotocol/servers/tree/main/src/gdrive) 📇 ☁️ - Google Drive 集成，用于列出、阅读和搜索文件

+ `8b-is/smart-tree` 🦀 🏠 🍎 🪟 🐧 - AI 原生目录可视化，具有语义分析、AI 消费的超压缩格式和 10 倍令牌减少。支持具有智能文件分类的量子语义模式。

+ `hmk/box-mcp-server` 📇 ☁️ - Box 集成，支持文件列表、阅读和搜索功能

+ `mark3labs/mcp-filesystem-server` 🏎️ 🏠 - 用于本地文件系统访问的 Golang 实现。

+ `mamertofabian/mcp-everything-search` 🐍 🏠 🪟 - 使用 Everything SDK 实现的快速 Windows 文件搜索

+ `cyberchitta/llm-context.py` 🐍 🏠 - 通过 MCP 或剪贴板与 LLM 共享代码上下文

+ [`filesystem@quarkiverse/quarkus-mcp-servers`](https://github.com/quarkiverse/quarkus-mcp-servers/tree/main/filesystem) ☕ 🏠 - 一个基于Java和Quarkus实现的文件系统，支持浏览和编辑文件。提供jar包或原生镜像两种形式。

+ `Xuanwo/mcp-server-opendal` 🐍 🏠 ☁️ - 使用 Apache OpenDAL™ 访问任何存储

+ `exoticknight/mcp-file-merger` 📇 🏠 - 文件合并工具，适配AI Chat长度限制

## 💰 金融与金融科技

金融数据访问和加密货币市场信息。支持查询实时市场数据、加密货币价格和财务分析。

+ [`heurist-network/heurist-mesh-mcp-server`](https://github.com/heurist-network/heurist-mesh-mcp-server) 🎖️ ⛅️ 🏠 🐍 - 通过Heurist Mesh网络访问专业化的web3 AI代理，用于区块链分析、智能合约安全审计、代币指标评估及链上交互。提供全面的DeFi分析工具、NFT估值及跨多链交易监控功能

+ [`@base/base-mcp`](https://github.com/base/base-mcp) 🎖️ 📇 ☁️ - 集成Base网络的链上工具，支持与Base网络及Coinbase API交互，实现钱包管理、资金转账、智能合约和DeFi操作

+ `QuantGeekDev/coincap-mcp` 📇 ☁️ - 使用 CoinCap 的公共 API 集成实时加密货币市场数据，无需 API 密钥即可访问加密货币价格和市场信息

+ `anjor/coinmarket-mcp-server` 🐍 ☁️ - Coinmarket API 集成以获取加密货币列表和报价

+ `berlinbra/alpha-vantage-mcp` 🐍 ☁️ - Alpha Vantage API 集成，用于获取股票和加密货币信息

+ `ferdousbhai/tasty-agent` 🐍 ☁️ - Tastyworks API 集成，用于管理 Tastytrade 平台的交易活动

+ `ferdousbhai/investor-agent` 🐍 ☁️ - 整合雅虎财经以获取股市数据，包括期权推荐

+ `mcpdotdirect/evm-mcp-server` 📇 ☁️ - 全面支持30多种EVM网络的区块链服务，涵盖原生代币、ERC20、NFT、智能合约、交易及ENS解析。

+ `bankless/onchain-mcp` 📇 ☁️ - Bankless链上API，用于与智能合约交互、查询交易及代币信息

+ `kukapay/cryptopanic-mcp-server` 🐍 ☁️ - 为AI代理提供由CryptoPanic驱动的最新加密货币新闻。

+ `kukapay/whale-tracker-mcp` 🐍 ☁️ - 一个用于追踪加密货币大额交易的MCP服务器。

+ `kukapay/crypto-feargreed-mcp` 🐍 ☁️ - 提供实时和历史加密恐惧与贪婪指数数据。

+ `kukapay/dune-analytics-mcp` 🐍 ☁️ - 一个将Dune Analytics数据桥接到AI代理的mcp服务器。

+ `kukapay/pancakeswap-poolspy-mcp` 🐍 ☁️ - 一个追踪Pancake Swap上新创建资金池的MCP服务器。

+ `kukapay/uniswap-poolspy-mcp` 🐍 ☁️ - 一个MCP服务器，用于追踪Uniswap在多个区块链上新创建的流动性池。

+ `kukapay/uniswap-trader-mcp` 🐍 ☁️ - 一个MCP服务器，用于AI代理在多个区块链上的Uniswap去中心化交易所自动执行代币交换。

+ `kukapay/token-minter-mcp` 🐍 ☁️ - 一个MCP服务器，为AI代理提供工具以跨多个区块链铸造ERC-20代币。

+ `kukapay/thegraph-mcp` 🐍 ☁️ - 一个MCP服务器，通过The Graph提供的索引区块链数据为AI代理提供支持。

+ `longportapp/openapi` - 🐍 ☁️ - LongPort OpenAPI 提供港美股等市场的股票实时行情数据，通过 MCP 提供 AI 接入分析、交易能力。

+ `pwh-pwh/coin-mcp-server` 🐍 ☁️ - 使用 Bitget 公共 API 去获取加密货币最新价格

+ `HuggingAGI/mcp-baostock-server` 🐍 ☁️ - 基于 baostock 的 MCP 服务器,提供对中国股票市场数据的访问和分析功能。

+ `Wuye-AI/mcp-server-wuye-ai` 🎖️ 📇 ☁️ - 接入 CRIC物业AI 平台的 MCP 服务器。CRIC物业AI 是克而瑞专为物业行业打造的智能 AI 助理。

+ `zlinzzzz/finData-mcp-server` 🐍 ☁️ - 一个获取专业级金融数据的MCP服务器，支持Tushare等多种数据供应商。

+ `JamesANZ/prediction-market-mcp` 📇 ☁️ - 一个MCP服务器，提供来自Polymarket、PredictIt和Kalshi等多个平台的实时预测市场数据。使AI助手能够通过统一接口查询当前赔率、价格和市场信息。

+ `JamesANZ/bitcoin-mcp` 📇 🏠 - 一个MCP服务器，使AI模型能够查询比特币区块链。

## 🎮 游戏

游戏相关数据和服务集成

+ `Coding-Solo/godot-mcp` 📇 🏠 - 一个用于与Godot游戏引擎交互的MCP服务器，提供编辑、运行、调试和管理Godot项目中场景的工具。

+ `rishijatia/fantasy-pl-mcp` 🐍 ☁️ - 用于实时 Fantasy Premier League 数据和分析工具的 MCP 服务器。

+ `CoderGamester/mcp-unity` 📇 #️⃣ 🏠 - Unity3d 游戏引擎集成 MCP 服务器

+ `opgginc/opgg-mcp` 📇 ☁️ - 访问英雄联盟、云顶之弈、无界英雄等热门游戏的实时游戏数据，提供英雄分析、电竞赛程、元组合和角色统计。

## 🧠 知识与记忆

使用知识图谱结构的持久内存存储。使 AI 模型能够跨会话维护和查询结构化信息。

+ [`jinzcdev/markmap-mcp-server`](https://github.com/markmap/markmap) 📇 🏠 - 基于 markmap 构建的 MCP 服务器，可将 Markdown 转换为交互式的 思维导图。支持多格式导出（PNG/JPG/SVG）、浏览器在线预览、一键复制 Markdown 和动态可视化功能。

+ `[@modelcontextprotocol/server-memory]` 📇 🏠 - 基于知识图谱的长期记忆系统用于维护上下文

+ `/CheMiguel23/MemoryMesh` 📇 🏠 - 增强基于图形的记忆，重点关注 AI 角色扮演和故事生成

+ `/topoteretes/cognee` 📇 🏠 - AI应用程序和Agent的内存管理器使用各种图存储和向量存储，并允许从 30 多个数据源提取数据

+ `[@hannesrudolph/mcp-ragdocs]` 🐍 🏠 - MCP 服务器实现提供了通过矢量搜索检索和处理文档的工具，使 AI 助手能够利用相关文档上下文来增强其响应能力

+ `[@kaliaboi/mcp-zotero]` 📇 ☁️ - 为 LLM 提供的连接器，用于操作 Zotero Cloud 上的文献集合和资源

+ [`mcp-summarizer`](https://github.com/0xshellming/mcp-summarizer) 📕 ☁️ - AI摘要生成MCP服务器，支持多种内容类型：纯文本、网页、PDF文档、EPUB电子书、HTML内容

+ [`apecloud/ApeRAG`](https://github.com/apecloud/ApeRAG) 🐍 ☁️ 🏠 - 生产级RAG平台，结合Graph RAG、向量搜索和全文搜索。构建知识图谱和上下文工程的最佳选择

+ `graphlit-mcp-server` 📇 ☁️ - 将来自Slack、Discord、网站、Google Drive、Linear或GitHub的任何内容摄取到Graphlit项目中，然后在诸如Cursor、Windsurf或Cline等MCP客户端中搜索并检索相关知识。

+ `[@mem0ai/mem0-mcp]` 🐍 🏠 - 用于 Mem0 的模型上下文协议服务器，帮助管理编码偏好和模式，提供工具用于存储、检索和语义处理代码实现、最佳实践和技术文档，适用于 Cursor 和 Windsurf 等 IDE

+ `[@ragieai/mcp-server]` 📇 ☁️ - 从您的 Ragie (RAG) 知识库中检索上下文，该知识库连接到 Google Drive、Notion、JIRA 等多种集成。

+ `[@upstash/context7]` 📇 ☁️ - 最新的LLM和AI代码编辑器的代码文档。

+ `JamesANZ/memory-mcp` 📇 🏠 - 一个MCP服务器，使用MongoDB存储和检索来自多个LLM的记忆。提供保存、检索、添加和清除带有时间戳和LLM识别的对话记忆的工具。

+ `JamesANZ/cross-llm-mcp` 📇 🏠 - 一个MCP服务器，实现跨LLM通信和记忆共享，使不同的AI模型能够在对话间协作和共享上下文。

## 🗺️ 位置服务

地理和基于位置的服务集成。支持访问地图数据、方向和位置信息。

+ [`@modelcontextprotocol/server-google-maps`](https://github.com/modelcontextprotocol/servers-archived/tree/main/src/google-maps) 📇 ☁️ - Google 地图集成，提供位置服务、路线规划和地点详细信息

+ `SecretiveShell/MCP-timeserver` 🐍 🏠 - 访问任意时区的时间并获取当前本地时间

+ `webcoderz/MCP-Geo` 🐍 🏠 - 支持 nominatim、ArcGIS、Bing 的地理编码 MCP 服务器

+ `[@briandconnelly/mcp-server-ipinfo]` 🐍 ☁️ - 使用 IPInfo API 获取 IP 地址的地理位置和网络信息

+ [`QGIS MCP`](https://github.com/jjsantos01/qgis_mcp) - 通过MCP将QGIS桌面端与Claude AI连接。该集成支持提示辅助的项目创建、图层加载、代码执行等功能。

+ `kukapay/nearby-search-mcp` 🐍 ☁️ - 一个基于IP定位检测的附近地点搜索MCP服务器。

## 🎯 营销

用于创建和编辑营销内容、处理网页元数据、产品定位和编辑指南的工具。

+ [`Open Strategy Partners Marketing Tools`](https://github.com/open-strategy-partners/osp_marketing_tools) 🐍 🏠 - Open Strategy Partners 提供的营销工具套件，包含写作风格指南、编辑规范和产品营销价值图谱创建工具

## 📊 监测

访问和分析应用程序监控数据。使 AI 模型能够审查错误报告和性能指标。

+ `[@modelcontextprotocol/server-sentry]` 🐍 ☁️ - Sentry.io 集成用于错误跟踪和性能监控

+ `[@MindscapeHQ/server-raygun]` 📇 ☁️ - Raygun API V3 集成用于崩溃报告和真实用户监控

+ `metoro-io/metoro-mcp-server` 🎖️ 🏎️ ☁️ - 查询并与 Metoro 监控的 kubernetes 环境交互

+ `grafana/mcp-grafana` 🎖️ 🐍 🏠 ☁️ - 在 Grafana 实例中搜索仪表盘、调查事件并查询数据源

+ `tumf/grafana-loki-mcp` 🐍 🏠 - 一个 MCP 服务器，允许通过 Grafana API 查询 Loki 日志。

+ `pydantic/logfire-mcp` 🎖️ 🐍 ☁️ - 通过Logfire提供对OpenTelemetry追踪和指标的访问

+ `seekrays/mcp-monitor` 🏎️ 🏠 - 一款通过模型上下文协议（MCP）暴露系统指标的监控工具。该工具允许大型语言模型通过兼容MCP的接口实时获取系统信息（支持CPU、内存、磁盘、网络、主机、进程）。

+ `hyperb1iss/lucidity-mcp` 🐍 🏠 - 通过基于提示的智能分析，从代码复杂度到安全漏洞等10个关键维度，提升AI生成代码的质量

+ [`inventer-dev/mcp-internet-speed-test`](https://github.com/inventer-dev/mcp-internet-speed-test) 🐍 ☁️ - 互联网速度测试，包括下载/上传速度、延迟、抖动分析和地理映射的CDN服务器检测等网络性能指标

+ [`VictoriaMetrics-Community/mcp-victoriametrics`](https://github.com/VictoriaMetrics-Community/mcp-victoriametrics) [`文档`](https://docs.victoriametrics.com/) 🏆 🏆 🏠 - 与 VictoriaMetrics API 和文档 完整集成，监控你的 VictoriaMetrics 实例及排查问题。

## 🔎 搜索

+ `scrapeless-ai/scrapeless-mcp-server` 🐍 ☁️ - Scrapeless模型上下文协议服务作为MCP服务器连接器，连接到Google SERP API，使得在MCP生态系统内无需离开即可进行网页搜索。

+ [`@modelcontextprotocol/server-brave-search`](https://github.com/modelcontextprotocol/servers/tree/main/src/brave-search) 📇 ☁️ - 使用 Brave 的搜索 API 实现网页搜索功能

+ `DappierAI/dappier-mcp` 🐍 ☁️ - Dappier 的 MCP 服务器可让 AI 代理快速、免费地进行实时网页搜索，并访问来自可靠媒体品牌的新闻、金融市场、体育、娱乐、天气等优质数据。

+ [`Dumpling-AI/mcp-server-dumplingai`](https://www.dumplingai.com/) 🎖️ 📇 ☁️ - 通过 Dumpling AI 提供的数据访问、网页抓取与文档转换 API

+ `[@angheljf/nyt]` 📇 ☁️ - 使用 NYTimes API 搜索文章

+ `[@modelcontextprotocol/server-fetch]` 🐍 🏠 ☁️ - 高效获取和处理网页内容，供 AI 使用

+ `ac3xx/mcp-servers-kagi` 📇 ☁️ - Kagi 搜索 API 集成

+ `exa-labs/exa-mcp-server` 🎖️ 📇 ☁️ – 模型上下文协议 (MCP) 服务器让 Claude 等 AI 助手可以使用 Exa AI Search API 进行网络搜索。此设置允许 AI 模型以安全且可控的方式获取实时网络信息。

+ `fatwang2/search1api-mcp` 📇 ☁️ - 通过 search1api 搜索（需要付费 API 密钥）
