
> 在第一阶段，我们成功构建了一个“能对话、能查天气”的 AI 助手，但它的能力是被**硬编码（Hardcoded）**在代码里的。每次想要增加新功能（比如查股票、操作数据库），都需要修改 `tools.py` 并重启服务。

> 在第二阶段，我们将引入 MCP (Model Context Protocol)，对系统进行“基因重组”，将其升级为一个 Agent OS（智能体操作系统）。

## 核心交付标准 (Requirements)

1. **能力热插拔 (Hot-Swapping)**：用户在前端勾选某个工具，Agent 在毫秒级内即可获得该能力，无需重启后端服务。

2. 混合连接架构 (Hybrid Connectivity)：

   + 本地安全连接 (StdIO)：支持连接本地 Python 脚本或数据库，确保敏感数据不出域。

   + 远程云服务 (SSE)：支持连接互联网上的标准 MCP 服务（如高德地图、12306）。

3. 零代码配置 (No-Code Config)：普通用户可以通过前端界面填写 API Key 或文件路径来配置工具，无需接触 Python 代码。

4. 跨平台兼容 (Cross-Platform)：后端需自动处理 Windows/Mac/Linux 的路径差异，确保配置文件的可移植性。

## 1. 架构设计与数据层构建 (Data Layer)

> 设计理念：任何复杂的系统，都是数据先行。在编写逻辑代码之前，我们需要先定义好**“应用商店（Registry）”和“用户配置（Config）”**的数据结构。这两个 JSON 文件将是整个系统的基石。

### 1.1 核心概念回顾：MCP 的两种连接形态

在设计数据结构前，必须明确 MCP 的两种核心传输协议，因为配置文件的写法完全不同：

+ StdIO (Standard Input/Output)：

+ **原理**：Agent 直接启动一个子进程（Subprocess）。

+ **场景**：本地工具（如 `mcp-server-time`, `sqlite`）。

+ **配置核心**：需要 `command`（执行什么命令，如 `python`）和 `args`（参数）。

+ SSE (Server-Sent Events)：

+ **原理**：Agent 通过 HTTP 长连接与远程服务器通信。

+ **场景**：云端服务（如阿里云百炼 MCP）。

+ **配置核心**：需要 `url`（服务器地址）和 `headers`（鉴权信息）。

### 1.2 构建 MCP 工具知识库 (`mcp_registry.json`)

这是我们的“应用商店”数据源。它是一个只读的 JSON 列表，定义了系统预置了哪些工具，以及它们的默认配置模板。

**操作**：请在 `mini_chatgpt/backend/` 目录下创建 `mcp_registry.json`。

![](img/d2b6262d1934634a70dfc751848e37c6.png)

**代码内容：**

```json
[
  {
    "name": "time",
    "category": "Utility",
    "description": "提供精确的当前时间查询和时区转换功能。",
    "type": "stdio",
    "default_config": {
      "command": "python",
      "args": [
        "-m",
        "mcp_server_time"
      ]
    }
  },
  {
    "name": "sqlite",
    "category": "Database",
    "description": "本地 SQLite 数据库工具。支持查看表结构和执行 SQL 查询。",
    "type": "stdio",
    "default_config": {
      "command": "python",
      "args": [
        "-m",
        "mcp_server_sqlite",
        "--db-path",
        "./test.db"
      ]
    }
  },
  {
    "name": "amap-maps",
    "category": "Geo/Travel",
    "description": "高德地图官方 MCP。提供地点搜索、路径规划、天气查询等服务。",
    "type": "sse",
    "default_config": {
      "url": "https://dashscope.aliyuncs.com/api/v1/mcps/amap-maps/sse",
      "headers": {
        "Authorization": "Bearer 你的阿里百炼API-Key"
      }
    }
  }
]
```

josn文档功能解读：

+ `type`: 明确区分了 stdio 和 sse，这是后续 `MCPManager` 决定如何启动客户端的关键依据。

+ `default_config`: 这是一个模板。

+ 对于 `time`，模板里写的是 `python`，我们在后续代码中会将其动态替换为 `sys.executable`（当前环境的 Python），从而实现跨平台。

+ 对于 `amap-maps`，模板里留了 `"Bearer 你的API-Key"`，提示用户在安装时需要填入自己的密钥。

### 1.3 设计用户配置存储 (`mcp_config.json`)

这是用户的个性化存档。它记录了用户到底安装了哪些工具、是否激活、以及具体的配置参数（如真实的 API Key）。

操作：在 `mini_chatgpt/backend/` 目录下创建 `mcp_config.json`。(注意：在实际运行中，这个文件通常由程序自动生成，但为了测试，我们可以先手动创建一个初始版本)

**代码内容：**

```json
{
  "tools": {
    "time": {
      "type": "stdio",
      "description": "提供精确的当前时间查询和时区转换功能。",
      "active": true,
      "config": {
        "command": "python",
        "args": [
          "-m",
          "mcp_server_time"
        ]
      }
    }
  }
}
```

json文件解读：

+ `active`: 这是一个布尔值开关。

+ true: Agent 在启动时会加载这个工具。

+ false: 即使配置存在，Agent 也会忽略它。这实现了“软删除”或“临时禁用”的功能。

+ `config`: 这里存储的是最终执行时的参数。

+ 思考题：这里的 command 写的是 python，但在 Windows 和 Mac 上，Python 的绝对路径是不同的。如果我们把绝对路径写死在这里（例如 `C:\Python312\python.exe`），把文件发给别人就跑不起来了。

+ 伏笔：我们在接下来的 `MCPManager` 开发中，会编写一个“路径自适应”逻辑，在读取配置的瞬间，把这里的 `python` 动态替换为当前环境的绝对路径。这就是工业级开发的细节！

## 2. 核心业务逻辑开发：MCP 管理器 (Business Logic Layer)

> 开发目标：编写 `backend/mcp_manager.py`。我们将在这个文件中实现三大核心能力：

1. 全生命周期管理：工具的增删改查与状态切换。

2. 智能容错机制：处理用户乱填配置（嵌套 JSON）和环境差异（Windows/Mac 路径）的自动纠错。

3. AI 驱动推荐：集成 `DeepSeek` 模型，实现“说人话，找工具”。

### 2.1 依赖导入与 Pydantic 模型定义

首先，我们需要定义 AI 推荐返回的数据结构。在 `LangChain` 1.0 中，使用 Pydantic 定义结构化输出是标准范式。

代码片段 (`mcp_manager.py` 头部)：

```python
import json
import os
import sys
import asyncio
from typing import List, Dict, Tuple, Optional
from dotenv import load_dotenv

from pydantic import BaseModel, Field
from langchain_deepseek import ChatDeepSeek
from langchain_core.prompts import ChatPromptTemplate
from langchain_mcp_adapters.client import MultiServerMCPClient

load_dotenv(override=True)

REGISTRY_FILE = "mcp_registry.json"
CONFIG_FILE = "mcp_config.json"

class ToolRecommendation(BaseModel):
    """单个工具推荐"""
    name: str = Field(description="工具名称，必须与知识库一致")
    reason: str = Field(description="推荐理由")

class RecommendationList(BaseModel):
    """推荐列表容器"""
    recommendations: List[ToolRecommendation]
```

### 2.2 核心类构建与基础 CRUD

接下来是 `MCPManager` 的初始化和基础读写功能。这里的关键点在于列表读取时的格式化。

代码片段 (`MCPManager` 类基础)：

```python
class MCPManager:
    def __init__(self):
        # 初始化时加载配置
        self.config = self._load_config()
        self.registry = self._load_registry()
        # 初始化 DeepSeek (低温度，保证推荐稳定)
        self.llm = ChatDeepSeek(model="deepseek-chat", temperature=0.1)

    # ... (省略 _load_config / _save_config 的标准文件读写代码，与之前一致) ...

    def list_installed_tools(self) -> List[Dict]:
        """
        列出已安装工具。
        关键点：额外返回 config_json 字符串，供前端编辑器直接显示。
        """
        # 每次读取前强制刷新，防止多进程数据不一致
        self.config = self._load_config()
        
        results = []
        for name, data in self.config.get("tools", {}).items():
            raw_config = data.get("config", {})
            results.append({
                "name": name,
                "description": data.get("description", ""),
                "active": data.get("active", True),
                "type": data.get("type"),
                "config": raw_config,
                "config_json": json.dumps(raw_config, ensure_ascii=False, indent=2)
            })
        return results
```

### 2.3 健壮的连接测试 (`test_tool_connection`)

这是本节的“高光时刻”。我们要解决两个让无数开发者头疼的问题：

1. 用户乱填：直接把阿里百炼的文档复制粘贴进去（导致 JSON 嵌套）。

2. 环境差异：Windows 写的配置传给 Mac，路径报错。

代码片段 (`test_tool_connection`)：
