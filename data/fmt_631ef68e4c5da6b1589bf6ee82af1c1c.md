
```
│   │   ├── tool_registry.py       # 工具注册表

│   │   ├── validator.py           # 数据验证器

│   │   └── utils.py               # 工具函数

│   │

│   ├── services/              # 服务层

│   │   ├── llm_client.py     # LLM客户端

│   │   └── task_manager.py   # 任务管理器

│   │

│   ├── configs/               # 配置文件

│   │   └── tools_registry.json    # 工具定义

│   │

│   └── data/                  # 数据目录

│       └── processed/         # 生成的数据

│

├── frontend/                   # 前端代码

│   ├── src/

│   │   ├── App.tsx           # 主应用组件

│   │   ├── components/       # UI组件

│   │   │   ├── ControlPanel.tsx    # 控制面板

│   │   │   └── Dashboard.tsx       # 数据看板

│   │   └── styles/           # 样式文件

│   │

│   ├── package.json          # 依赖配置

│   └── vite.config.ts        # 构建配置

│

├── scripts/                   # 脚本工具

│   ├── start_backend.sh      # 启动后端

│   └── stop_backend.sh       # 停止后端

│

├── logs/                      # 日志目录

│   └── backend.log           # 后端日志

│

└── 文档/

    ├── ARCHITECTURE.md       # 本文档

    ├── BUGS_FIXED.md        # BUG修复记录

    ├── API_RATE_LIMIT_SOLUTION.md  # API限流解决方案

    └── FINAL_FIX_SUMMARY.md  # 最终修复总结
```

## 10. 快速开始指南

### 1 环境准备

```
# Python环境

python -m venv venv

source venv/bin/activate

pip install -r requirements.txt

# Node.js环境

cd frontend

npm install
```

### 2 配置API Key

```
# 编辑工具配置

vim backend/configs/tools_registry.json

# 或在前端界面填写

# API URL: https://api.deepseek.com/v1

# API Key: sk-your-api-key

# Model: deepseek-chat
```

### 3 启动服务

```
# 启动后端（Terminal 1）

./scripts/start_backend.sh

# 启动前端（Terminal 2）

cd frontend && npm run dev
```

### 4 生成数据

1. 打开浏览器：`http://localhost:3000`

1. 填写配置：

+ 样本数量：100

+ 并发度：3

+ 多轮对话比例：0.3

1. 点击"开始生成"

1. 等待完成后点击"下载数据集"

数据样例

```
{
  "system": "你是一个智能AI助手，可以通过调用工具来帮助用户完成各种任务。\n\n你需要根据用户的问题，选择合适的工具进行调用。工具调用格式如下：\n\n<function_call>\n{\n  \"name\": \"工具名称@版本\",\n  \"arguments\": {\n    \"参数名\": \"参数值\"\n  }\n}</function_call>\n\n在得到工具返回结果后，你需要用<final>标签给出最终回答：\n\n<final>\n这是最终的回答内容\n</final>\n\n请确保你的工具调用是准确的，参数是完整的。",
  "tools": "[{\"name\": \"get_stock_price@v1\", \"description\": \"获取股票实时价格\", \"parameters\": {\"type\": \"object\", \"properties\": {\"symbol\": {\"type\": \"string\", \"description\": \"股票代码，例如：AAPL, TSLA\"}}, \"required\": [\"symbol\"]}}, {\"name\": \"web_search@v1\", \"description\": \"在互联网上搜索信息\", \"parameters\": {\"type\": \"object\", \"properties\": {\"query\": {\"type\": \"string\", \"description\": \"搜索关键词\"}, \"max_results\": {\"type\": \"integer\", \"description\": \"最大结果数量\", \"default\": 10}}, \"required\": [\"query\"]}}, {\"name\": \"send_email@v1\", \"description\": \"发送电子邮件\", \"parameters\": {\"type\": \"object\", \"properties\": {\"to\": {\"type\": \"string\", \"description\": \"收件人邮箱地址\"}, \"subject\": {\"type\": \"string\", \"description\": \"邮件主题\"}, \"body\": {\"type\": \"string\", \"description\": \"邮件正文\"}}, \"required\": [\"to\", \"subject\", \"body\"]}}]",
  "messages": [
    {
      "role": "user",
      "content": "请使用获取股票实时价格工具"
    }
  ],
  "chosen": "我需要知道您想查询哪只股票的实时价格。请提供股票代码，比如 AAPL（苹果公司）或 TSLA（特斯拉公司）。\n\n<function_call>\n{\n  \"name\": \"get_stock_price@v1\",\n  \"arguments\": {\n    \"symbol\": \"请提供股票代码\"\n  }\n}</function_call>",
  "rejected": "<function_call>\n{\n  \"name\": \"get_stock_price@v1\",\n  \"arguments\": {\n    \"symbol\": \"\"\n  }\n}</function_call>"
}

```
{
  "system": "你是一个智能AI助手，可以通过调用工具来帮助用户完成各种任务。\n\n你需要根据用户的问题，选择合适的工具进行调用。工具调用格式如下：\n\n<function_call>\n{\n  \"name\": \"工具名称@版本\",\n  \"arguments\": {\n    \"参数名\": \"参数值\"\n  }\n}</function_call>\n\n在得到工具返回结果后，你需要用<final>标签给出最终回答：\n\n<final>\n这是最终的回答内容\n</final>\n\n请确保你的工具调用是准确的，参数是完整的。",
  "tools": "[{\"name\": \"calculate@v1\", \"description\": \"执行数学计算\", \"parameters\": {\"type\": \"object\", \"properties\": {\"expression\": {\"type\": \"string\", \"description\": \"数学表达式，例如：1+1, 25*4\"}}, \"required\": [\"expression\"]}}, {\"name\": \"convert_currency@v1\", \"description\": \"货币汇率转换\", \"parameters\": {\"type\": \"object\", \"properties\": {\"amount\": {\"type\": \"number\", \"description\": \"金额\"}, \"from_currency\": {\"type\": \"string\", \"description\": \"源货币代码，例如：USD, CNY\"}, \"to_currency\": {\"type\": \"string\", \"description\": \"目标货币代码\"}}, \"required\": [\"amount\", \"from_currency\", \"to_currency\"]}}, {\"name\": \"web_search@v1\", \"description\": \"在互联网上搜索信息\", \"parameters\": {\"type\": \"object\", \"properties\": {\"query\": {\"type\": \"string\", \"description\": \"搜索关键词\"}, \"max_results\": {\"type\": \"integer\", \"description\": \"最大结果数量\", \"default\": 10}}, \"required\": [\"query\"]}}]",
  "messages": [
    {
      "role": "user",
      "content": "200人民币等于多少港币，接着区块链有什么特点"
    },
    {
      "role": "assistant",
      "content": "<function_call>\n{\n  \"name\": \"convert_currency@v1\",\n  \"arguments\": {\n    \"amount\": 200,\n    \"from_currency\": \"CNY\",\n    \"to_currency\": \"HKD\"\n  }\n}</function_call>\n\n等待汇率转换结果返回后，我将继续调用搜索工具查询区块链特点。"
    },
    {
      "role": "user",
      "content": "<function_call>\n{\n  \"name\": \"convert_currency@v1\",\n  \"arguments\": {\n    \"amount\": 200,\n    \"from_currency\": \"CNY\", \n    \"to_currency\": \"HKD\"\n  }\n}</function_call>"
    },
    {
      "role": "assistant",
      "content": "我需要等待汇率转换工具返回结果，然后继续为您查询区块链的特点。请稍等片刻。\n\n<function_call>\n{\n  \"name\": \"web_search@v1\",\n  \"arguments\": {\n    \"query\": \"区块链技术的主要特点特征\"\n  }\n}</function_call>"
    },
    {
      "role": "user",
      "content": "<function_call>\n{\n  \"name\": \"web_search@v1\", \n  \"arguments\": {\n    \"query\": \"区块链技术的主要特点特征\",\n    \"max_results\": 5\n  }\n}</function_call>"
    }

  ],
