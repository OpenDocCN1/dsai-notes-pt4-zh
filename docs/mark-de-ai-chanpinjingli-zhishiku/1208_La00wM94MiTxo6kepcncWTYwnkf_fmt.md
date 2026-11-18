
# 手搓一个会调用`MCP`的智能体，（架构+代码实现）

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/V5vLd7bgSodySuxLMfMcMTxPnwh](https://qqs7y1hozd1.feishu.cn/docx/V5vLd7bgSodySuxLMfMcMTxPnwh)

## 前期准备

### 1.1 运行环境准备

+   `Python 3.8+`
+   一个可以本地运行的`mcp`工具
+   一个大模型`API KEY`

```
pip install -r requirements.txt
```

### 1.2 配置大模型和角色

编辑 `llm_config.yaml`，配置你的`API Key`、`模型`、`角色`等参数。

`llm_config.yaml` 配置文件详解：

```
# 大模型提供商配置
providers:
  # OpenAI 提供商配置
  openai:
    # API密钥 - 从OpenAI官网获取
    api_key: "sk-xxx"
    # API基础URL - OpenAI官方API地址
    api_base: "https://api.openai.com"
    # API类型 - 用于选择对应的适配器
    api_type: "openai"
    # 默认模型 - 当未指定模型时使用的默认模型
    default_model: "gpt-3.5-turbo"
    # 请求超时时间（秒）
    timeout: 60
    # 重试次数 - API调用失败时的重试次数
    retry_times: 3
    # 重试延迟（秒） - 每次重试之间的等待时间
    retry_delay: 1
    # 聊天配置模板
    chat:
      # 默认聊天模板
      default:
        # 请求模板 - 定义发送给API的请求格式
        request_template:
          # 模型名称 - 使用配置中的模型
          model: "{model_name}"
          # 消息列表 - 对话历史
          messages: "{messages}"
          # 温度参数 - 控制输出的随机性（0-1），值越高越随机
          temperature: 0.7
          # Top-p参数 - 核采样参数，控制词汇选择的多样性
          top_p: 0.9
          # Top-k参数 - 限制每次选择时考虑的词汇数量
          top_k: 50
          # 最大token数 - 限制回复的最大长度
          max_tokens: 2048
        # 响应路径 - 从API响应中提取内容的路径
        response_path: "choices.0.message.content"   
  
  # DeepSeek V3 提供商配置（通过SiliconFlow API）
  DeepSeekV3:
    # API密钥 - 从SiliconFlow获取的DeepSeek API密钥
    api_key: "sk-xxx"
    # API基础URL - SiliconFlow的API地址
    api_base: "https://api.siliconflow.cn"
    # API类型 - 使用siliconflow适配器
    api_type: "siliconflow"
    # 默认模型 - DeepSeek V3模型
    default_model: "deepseek-ai/DeepSeek-V3"
    # 请求超时时间（秒）- DeepSeek模型推理较慢，设置更长超时
    timeout: 120
    # 重试次数
    retry_times: 3
    # 重试延迟（秒）
    retry_delay: 1
    # 聊天配置模板
    chat:
      default:
        request_template:
          model: "{model_name}"
          messages: "{messages}"
          # DeepSeek模型推荐参数
          temperature: 0.7
          top_p: 0.7
          top_k: 50
          max_tokens: 2048
        response_path: "choices.0.message.content" 

# 角色配置（可选）
roles:
  # 助手角色
  assistant:
    # 系统提示词 - 定义AI助手的角色和行为
    system_prompt: "你是一个有用的AI助手，能够帮助用户解决各种问题。"
    # 角色特定参数
    temperature: 0.7
    max_tokens: 1024
  
  # 代码助手角色
  code_assistant:
    system_prompt: "你是一个专业的编程助手，擅长代码编写、调试和优化。"
    temperature: 0.3  # 代码生成需要更确定性
    max_tokens: 2048
```

### 1.3 ReAct智能体框架（Reasoning and Acting）

#### 1.3.1 工作流程

+   **思考（Thought）：**分析当前情况
+   **行动（Action）：**决定下一步操作
+   观察（Observation）：获取行动结果
+   **循环：**直到解决问题

#### 1.3.2 实现方式

```
[系统提示]
你是一个智能助手，可以思考问题并调用工具。请按照以下格式输出：
思考（Thought）：分析当前情况
行动（Action）：选择要执行的操作
观察（Observation）：记录操作结果

[示例]
问题（Question）:"今天天津天气如何？"

思考（Thought）：我需要调用`get_weather`工具来查天气。
行动（Action）：调用`get_weather("天津")`
观察（Observation）：当前天津天气为 12℃，多云。

思考（Thought）：已获取答案，可以回答了。
行动（Action）：返回天气信息
观察（Observation）：天津今日天气为 12℃，多云。

最终答案（Answer）：天津今日天气为 12℃，多云。
```

> 在实际应用中，主机端可以实现一个"文本提取器"，自动从行动（Action）中提取函数名和参数（`function name` & `arguments`），然后将其发送给本地的`mcp`工具或通过`function call`的方式进行实际执行。

这样，模型只需专注于**推理和决策**，具体的工具调用和结果获取由主机端自动完成，实现推理与行动的闭环。

#### 1.3.3 ReAct推理提示词

```
# 智能体配置
agent:
  # 系统提示词 - ReAct智能体的核心提示词
  system_prompt: |
    你是一个智能 Agent，具备思考、调用工具和观察结果的能力,请根据用户的问题，灵活选择直接回复还是调用工具。请严格按照以下格式输出：
    Thought: <你在思考什么？>  
    Action: 如果你需要调用工具，只输出如下JSON格式（不要加任何markdown代码块或```json）：

      {

        "method": "tools/call",

        "params": {

          "name": "工具名",

          "arguments": { "参数名": "参数值" }

        }

      }

    Observation:如果你需要调用工具，请放空,等待`Action`调用成功后返回结果再进行观察。

    Answer: 如果你需要调用工具，请放空，否则请按照如下输出<你的最终回答>，如果有工具调用结果，请严格按工具调用结果输出，不要编造默认值。

    注意：

    - `Action` 必须为标准 JSON 格式，`method` 固定为 `tools/call`，`name` 为工具名，`arguments` 为参数字典。

    - `Action` 步骤只输出纯JSON，不要加任何markdown代码块（如```json）。
    - 每次调用工具失败，应重新思考并调整策略；
  # 智能体参数 - 控制推理过程
  temperature: 0.1    # 低温度确保输出格式的一致性
  top_p: 0.9         # 保持词汇选择的多样性
  top_k: 20          # 限制词汇选择范围
  frequency_penalty: 0.5  # 减少重复内容
  max_tokens: 1024   # 限制单次回复长度
```

#### 1.3.4 ReAct推理循环流程

![](img/40b4f87afa3f2756bce2090efee152e3.png)

## agent系统架构与调用流程

主机`agent`是整个智能体系统的"大脑"和"指挥官"，负责统筹全局、协调资源、管理安全。

> 在`MCP`（Model Context Protocol）架构中，主机是连接用户、大模型和外部工具的核心枢纽。

`agent`架构概览如下：

### 2.1 系统架构概览

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   用户输入      │    │   配置文件      │    │   外部工具      │
│   (User Input)  │    │  (Config Files) │    │  (MCP Tools)    │
└─────────┬───────┘    └─────────┬───────┘    └─────────┬───────┘
          │                      │                      │
          ▼                      ▼                      ▼
┌─────────────────────────────────────────────────────────────────┐
│                    智能体系统 (Agent System)                     │
├─────────────────┬─────────────────┬─────────────────┬───────────┤
│   AgentManager  │   ReActAgent    │   LLMFactory    │  MCPClient│
│   (智能体管理)   │   (推理执行)     │   (模型工厂)     │  (工具管理)│
├─────────────────┼─────────────────┼─────────────────┼───────────┤
│   ContextStore  │   MemoryManager │   AdapterMgr    │  Logger   │
│   (上下文管理)   │   (记忆管理)     │   (适配器管理)   │  (日志)   │
└─────────────────┴─────────────────┴─────────────────┴───────────┘
          │                      │                      │
          ▼                      ▼                      ▼
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   大模型API     │    │   本地工具      │    │   结果输出      │
│  (LLM APIs)     │    │  (Local Tools)  │    │  (Output)       │
└─────────────────┘    └─────────────────┘    └─────────────────┘

```

### 2.2 核心调用流程

#### 2.2.1 基础调用流程

### 2.3 详细流程说明

#### 2.3.1 第一步：系统初始化

1.  **配置文件加载**

```
# 加载配置文件
config = YAMLConfig()
config.load("llm_config.yaml")

```

1.  **组件初始化**

+   `AgentManager`：管理智能体生命周期
+   `LLMFactory`：创建大模型适配器
+   `ContextStore`：管理对话上下文
+   `MemoryManager`：管理对话历史

#### 2.3.2 第二步：智能体创建

**创建`ReActAgent`实例**

```
agent = ReActAgent(
    provider="openai",
    role="assistant", 
    user_content="用户问题",
    config=config
)

```

**初始化上下文**

+   从`ContextStore`获取历史上下文
+   设置系统参数
+   初始化对话历史

#### 2.3.3 第三步：ReAct推理循环

**思考阶段（Thought）**

+   `LLM`分析当前问题
+   生成推理思路
+   决定下一步行动

**行动阶段（Action）**

+   解析工具调用需求
+   提取工具名称和参数
+   验证工具可用性

观察阶段（Observation）

+   执行工具调用
+   获取执行结果
+   更新`ContextStore`

**循环判断**

+   检查是否为最终答案
+   决定是否继续推理
+   管理循环次数限制

#### 2.3.4 第四步：结果输出

**答案格式化**

+   提取最终答案
+   格式化输出内容
+   清理中间过程信息

**上下文更新**

+   保存对话历史到`MemoryManager`
+   更新`ContextStore`
+   优化上下文长度

### 2.4 各组件职责

主要要实现以下五大组件：

#### 2.4.1 `AgentManager`（智能体管理器）

**职责：**统一管理智能体的创建、配置和生命周期

**功能：**

+   加载和验证配置文件
+   创建不同类型的智能体
+   管理智能体实例池
+   提供统一的调用接口

#### 2.4.2 `ReActAgent`（推理执行器）

**职责：**实现`ReAct`推理循环，协调`LLM`和工具调用

**功能：**

+   解析`LLM`输出中的`Thought`/`Action`/`Observation`
+   管理推理循环逻辑
+   处理工具调用请求
+   生成最终答案

#### 2.4.3 `LLMFactory`（模型工厂）

**职责：**创建和管理不同大模型的适配器

**功能：**

+   根据配置创建适配器实例
+   管理API连接和认证
+   处理模型调用错误
+   支持流式输出

#### 2.4.4 `ContextStore`（上下文存储）

**职责：**管理对话上下文和历史记录

**功能：**

+   维护对话上下文
+   智能裁剪消息
+   管理结构化步骤
+   提供上下文统计

#### 2.4.5 `MemoryManager`（记忆管理器）

**职责：**管理智能体的对话历史

**功能：**

+   存储对话历史
+   按`agent_id`组织数据
+   提供历史查询
+   清理历史记录

## 统一的API调用与Token计算

在业务中经常会使用多个大模型`API`，甚至自主搭建的模型，统一`API`调用方便业务层进行统一的调度，同时`token`计算在上下文中十分关键。

> 很多大模型调用工具失败就源于超过大模型上下文`token`限制,导致内容备截断，后续我们在上下文管理中也需要用到`token`计算。

### 3.1 系统架构图

**架构说明**：

+   **智能体层**：通过统一的接口调用大模型
+   **适配器层**：为不同`API`提供商提供统一接口
+   **Token计算层**：支持不同模型的`token`计算
+   **配置管理层**：通过`YAML`文件管理所有配置
+   **外部API**：各种大模型服务提供商

### 3.2 API调用流程图

![](img/e7f758694527455ce0b9d8ff2cad9cb0.png)

**流程说明：**

+   **用户请求：**智能体接收用户输入
+   **工厂创建：**根据配置创建对应的`LLM`适配器
+   **配置读取：**从`YAML`文件读取`API`密钥、模型等配置
+   **适配器调用：**通过统一接口调用具体`API`
+   **结果返回：**将`API`响应返回给用户

### 3.3 Token计算流程图



![](img/e14774382ab270ad30f71e241b7e4830.png)

## 流程说明

> **文本输入：**接收需要计算`token`的文本

> **提供商选择：**根据配置选择合适的`tokenizer`

## 计算方式

> **OpenAI：**使用`tiktoken`库精确计算

> SiliconFlow/DeepSeek：使用字符级计算

> **结果返回：**返回包含`token`数量和`token`列表的字典

### 3.4 基础接口定义

首先定义统一的接口规范：

```python
# llm/adapters/base_llm.py
from typing import List, Dict, Any, Optional, AsyncGenerator
from llm.logging.decorators import log_method

class BaseLLM:
    def __init__(self, config: Any = None, **kwargs):
        self.config = config or kwargs

    @log_method(module="llm", level="INFO")
    async def chat(self, messages: List[Dict[str, str]]) -> str:
        raise NotImplementedError("chat 方法需要子类实现")

    @log_method(module="llm", level="INFO")
    async def stream_chat(self, messages: List[Dict[str, str]]) -> AsyncGenerator[str, None]:
        raise NotImplementedError("stream_chat 方法需要子类实现")

    @log_method(module="llm", level="INFO")
    async def get_embedding(self, text: str) -> List[float]:
        raise NotImplementedError("get_embedding 方法需要子类实现")

    @log_method(module="llm", level="INFO")
    async def get_embeddings_batch(self, texts: List[str], batch_size: int = 32) -> List[List[float]]:
        raise NotImplementedError("get_embeddings_batch 方法需要子类实现")

    @log_method(module="llm", level="INFO")
    async def rerank(self, query: str, documents: List[str], top_n: Optional[int] = None) -> List[Dict[str, Any]]:
        raise NotImplementedError("rerank 方法需要子类实现")
```

### 代码解释

> `BaseLLM`：定义了大模型适配器的统一接口

> `chat`：同步聊天接口，返回完整的回复内容

> `stream_chat`：流式聊天接口，支持实时返回

> `get_embedding`和`get_embeddings_batch`：获取文本嵌入向量

> `rerank`：文档重排序功能

使用装饰器进行日志记录，便于调试和监控

### 3.5 SiliconFlow适配器实现



```python
# llm/adapters/siliconflow_llm.py
from typing import List, Dict, Any, Optional, AsyncGenerator
import json
import logging
import httpx
import asyncio
from .base_llm import BaseLLM
from llm.logging.decorators import log_method

logger = logging.getLogger(__name__)

class SiliconFlowChat(BaseLLM):
    def __init__(self, config: Any = None, **kwargs):
        self.api_key = kwargs.get('api_key')
        self.api_base = kwargs.get('api_base', 'https://api.siliconflow.cn')
        self.model = kwargs.get('model_name', 'Qwen/QwQ-32B')
        self.timeout = kwargs.get('timeout', 30)
        self.retry_times = kwargs.get('retry_times', 3)
        self.retry_delay = kwargs.get('retry_delay', 1)
        self.system_prompt = kwargs.get('system_prompt')
        super().__init__(config or kwargs)

    def _validate_config(self) -> None:
        if not self.api_key:
            raise ValueError("API key is required")
        if not self.model:
            raise ValueError("Model name is required")
        if not self.api_base:
            raise ValueError("API base URL is required")

    async def _call_api(self, *args, **kwargs) -> Any:
        headers = {
            'Authorization': f'Bearer {self.api_key}',
            'Content-Type': 'application/json'
        }
        is_stream = kwargs.get('data', {}).get('stream', False)
        try:
            async with httpx.AsyncClient(timeout=self.timeout) as client:
                response = await client.post(
                    f'{self.api_base}{kwargs.get("endpoint", "")}',
                    headers=headers,
                    json=kwargs.get('data')
                )
                response.raise_for_status()
                if is_stream:
                    return response
                return response.json()
        except httpx.HTTPError as e:
            logger.error(f"HTTP Error: {str(e)}")
            logger.error(f"Response: {e.response.text if hasattr(e, 'response') else 'No response'}")
            raise
        except Exception as e:
            logger.error(f"Error calling API: {str(e)}")
            raise

    @log_method(module="llm", level="INFO")
    async def chat(self, messages: List[Dict[str, str]]) -> str:
        if self.system_prompt:
            messages = [{"role": "system", "content": self.system_prompt}] + messages
        data = {
            'model': self.model,
            'messages': messages,
            'stream': False,
            'temperature': getattr(self, 'temperature', 0.7),
            'top_p': getattr(self, 'top_p', 0.7),
            'top_k': getattr(self, 'top_k', 50),
            'frequency_penalty': getattr(self, 'frequency_penalty', 0.5),
            'max_tokens': getattr(self, 'max_tokens', 512)
        }
        result = await self._call_api(endpoint='/v1/chat/completions', data=data)
        return result['choices'][0]['message']['content']

    @log_method(module="llm", level="INFO")
    async def stream_chat(self, messages: List[Dict[str, str]]) -> AsyncGenerator[str, None]:
        if self.system_prompt:
            messages = [{"role": "system", "content": self.system_prompt}] + messages
        data = {
            'model': self.model,
            'messages': messages,
            'stream': True,
            'temperature': getattr(self, 'temperature', 0.7),
            'top_p': getattr(self, 'top_p', 0.7),
            'top_k': getattr(self, 'top_k', 50),
            'frequency_penalty': getattr(self, 'frequency_penalty', 0.5),
            'max_tokens': getattr(self, 'max_tokens', 512)
        }
        for retry in range(self.retry_times):
            try:
                response = await self._call_api(endpoint='/v1/chat/completions', data=data)
                async for line in response.aiter_lines():
                    if line:
                        line = line.strip()
                        if line.startswith('data: '):
                            line = line[6:]
                            if line == '[DONE]':
                                break
                            try:
                                chunk = json.loads(line)
                                if chunk['choices'][0]['delta'].get('content'):
                                    yield chunk['choices'][0]['delta']['content']
                            except json.JSONDecodeError as e:
                                logger.warning(f"Failed to parse chunk: {e}")
                                continue
                break
            except Exception as e:
                if retry == self.retry_times - 1:
                    logger.error(f"Failed after {self.retry_times} retries: {e}")
                    raise
                logger.warning(f"Retry {retry + 1}/{self.retry_times} due to error: {e}")
                await asyncio.sleep(self.retry_delay)

    @log_method(module="llm", level="INFO")
    async def chat_with_context(self, messages: List[Dict[str, str]], context: Dict[str, Any]) -> str:
        if self.system_prompt:
            context_str = "\n".join([f"{k}: {v}" for k, v in context.items()])
            system_message = f"{self.system_prompt}\n\nContext:\n{context_str}"
            messages = [{"role": "system", "content": system_message}] + messages
        return await self.chat(messages)
```

**代码解释：**

> `SiliconFlowChat`：SiliconFlow API的完整实现

> `_validate_config`：验证配置参数的有效性

> `_call_api`：统一的API调用方法，支持错误处理和重试

> `chat`：同步聊天接口，支持系统提示和参数配置

> `stream_chat`：流式聊天接口，支持实时返回和重试机制

> `chat_with_context`：支持上下文的聊天接口

### 3.6 OpenAI适配器实现



```
# llm/adapters/openai_llm.py
from typing import List, Dict, Any, Optional, AsyncGenerator
import openai
from .base_llm import BaseLLM

class OpenAIChat(BaseLLM):
    def __init__(self, config: Any = None, **kwargs):
        self.api_key = kwargs.get('api_key')
        self.api_base = kwargs.get('api_base', 'https://api.openai.com/v1')
        self.model = kwargs.get('model_name', 'gpt-3.5-turbo')
        self.timeout = kwargs.get('timeout', 30)
        self.system_prompt = kwargs.get('system_prompt')
        super().__init__(config or kwargs)
        openai.api_key = self.api_key
        openai.api_base = self.api_base

    async def chat(self, messages: List[Dict[str, str]]) -> str:
        if self.system_prompt:
            messages = [{"role": "system", "content": self.system_prompt}] + messages
        response = await openai.ChatCompletion.acreate(
            model=self.model,
            messages=messages,
            timeout=self.timeout
        )
        return response['choices'][0]['message']['content']

    async def stream_chat(self, messages: List[Dict[str, str]]) -> AsyncGenerator[str, None]:
        # 可选实现
        yield ""

    async def get_embedding(self, text: str) -> List[float]:
        # 可选实现
        return []

    async def get_embeddings_batch(self, texts: List[str], batch_size: int = 32) -> List[List[float]]:
        # 可选实现
        return []

    async def rerank(self, query: str, documents: List[str], top_n: Optional[int] = None) -> List[Dict[str, Any]]:
        # 可选实现
        return []
```

**代码解释**：

+   OpenAIChat：OpenAI API的简洁实现

+   使用官方SDK进行API调用

+   支持自定义API基础URL和超时设置

+   自动添加系统提示到消息列表

### 3.7 Token计算实现

```
# llm/adapters/tokenizer.py
import importlib
from typing import List, Tuple, Dict, Any

class BaseTokenizer:
    def count_tokens(self, text: str) -> Tuple[int, List[str]]:
        raise NotImplementedError

class OpenAITokenizer(BaseTokenizer):
    def __init__(self, model_name: str = "gpt-3.5-turbo"):
        try:
            self.tiktoken = importlib.import_module("tiktoken")
        except ImportError:
            raise ImportError("请先安装tiktoken库: pip install tiktoken")
        self.model_name = model_name
        self.encoder = self.tiktoken.encoding_for_model(model_name)

    def count_tokens(self, text: str) -> Tuple[int, List[str]]:
        tokens = self.encoder.encode(text)
        token_strs = [self.encoder.decode([t]) for t in tokens]
        return len(tokens), token_strs

class SiliconFlowTokenizer(BaseTokenizer):
    def __init__(self, model_name: str = "text-embedding-v1"):
        self.model_name = model_name
        
    def count_tokens(self, text: str) -> Tuple[int, List[str]]:
        tokens = list(text)
        return len(tokens), tokens

def get_tokenizer(provider: str, model_name: str = None) -> BaseTokenizer:
    provider = provider.lower()
    # DeepSeek系列模型使用SiliconFlowTokenizer
    if provider in ["deepseekv3", "deepseekr1"] or provider.startswith("deepseek"):
        return SiliconFlowTokenizer(model_name or "text-embedding-v1")
    elif provider == "openai":
        return OpenAITokenizer(model_name or "gpt-3.5-turbo")
    elif provider == "siliconflow":
        return SiliconFlowTokenizer(model_name or "text-embedding-v1")
    else:
        raise ValueError(f"暂不支持的provider: {provider}")

def count_tokens(text: str, provider: str = "openai", model_name: str = None) -> Dict[str, Any]:
    """
    计算文本的token数量
    Args:
        text: str，输入文本
        provider: str，模型提供商
        model_name: str，可选，模型名
    Returns:
        dict，包含token数量和token列表
    """
    tokenizer = get_tokenizer(provider, model_name)
    token_count, tokens = tokenizer.count_tokens(text)
    return {
        "token_count": token_count,
        "tokens": tokens
    }
```

**代码解释：**

> `BaseTokenizer`：Token计算器的基类

> `OpenAITokenizer`：使用tiktoken库进行精确的token计算

> `SiliconFlowTokenizer`：简单的字符级token计算

> `get_tokenizer`：根据提供商选择合适的tokenizer

> `count_tokens`：统一的token计算接口

### 3.8 工厂模式创建适配器

```
# llm/adapters/llm_factory.py
from llm.config.config import YAMLConfig
from .openai_llm import OpenAIChat
from .siliconflow_llm import SiliconFlowChat

class DummyLLM:
    def __init__(self, **kwargs):
        self.config = kwargs
    async def chat(self, messages):
        return "[模拟回复]" + str(messages[-1]["content"])

class LLMFactory:
    @classmethod
    def create_from_yaml(cls, provider_name: str, role_name: str, yaml_path: str = "llm_config.yaml", user_content: str = None):
        config = YAMLConfig(yaml_path)
        provider = config.get_provider(provider_name)
        role = config.get_role(role_name)
        chat_template = config.get_chat_template(provider_name, role_name)
        api_type = provider["api_type"]
        params = {**chat_template, **role}
        params["model_name"] = provider["default_model"]
        params["api_key"] = provider["api_key"]
        params["api_base"] = provider["api_base"]
        params["system_prompt"] = role.get("system_prompt")
        # 选择对应的 LLM 实现
        if api_type == "openai":
            llm = OpenAIChat(**params)
        elif api_type == "siliconflow":
            llm = SiliconFlowChat(**params)
        else:
            llm = SiliconFlowChat(**params)
        return llm
```

**代码解释**：

+   LLMFactory：统一的LLM创建工厂

+   `create_from_yaml`：从YAML配置文件创建LLM实例

+   支持多种提供商和角色配置

+   自动合并聊天模板和角色配置

+   提供`DummyLLM`用于测试

### 3.9 使用示例

```
# examples/llm_usage_example.py
import asyncio
from llm.adapters.llm_factory import LLMFactory
from llm.adapters.tokenizer import count_tokens

async def main():
    # 从配置文件创建SiliconFlow LLM
    siliconflow_llm = LLMFactory.create_from_yaml(
        provider_name="siliconflow",
        role_name="assistant",
        yaml_path="llm_config.yaml"
    )
    
    # 从配置文件创建OpenAI LLM
    openai_llm = LLMFactory.create_from_yaml(
        provider_name="openai",
        role_name="assistant",
        yaml_path="llm_config.yaml"
    )
    
    # 统一的消息格式
    messages = [
        {"role": "user", "content": "你好，请介绍一下Python编程语言的特点。"}
    ]
    
    # 使用SiliconFlow
    print("=== SiliconFlow响应 ===")
    response1 = await siliconflow_llm.chat(messages)
    print(f"内容: {response1}")
    
    # 使用OpenAI
    print("\n=== OpenAI响应 ===")
    response2 = await openai_llm.chat(messages)
    print(f"内容: {response2}")
    
    # Token计算示例
    text = "这是一个测试文本，用于计算token数量。"
    token_info = count_tokens(text, provider="siliconflow")
    print(f"\nToken计算: {token_info['token_count']} tokens")
    
    # 流式聊天示例
    print("\n=== 流式聊天 ===")
    async for chunk in siliconflow_llm.stream_chat(messages):
        print(chunk, end="", flush=True)

if __name__ == "__main__":
    asyncio.run(main())
```

**代码解释：**

> 展示了如何使用工厂模式创建不同的LLM实例

> 演示了同步和流式聊天功能

> 包含token计算的使用示例

> 使用统一的配置管理

**通过这样的设计，实现了：**

> **统一接口：**所有大模型都使用相同的调用方式

> **易于扩展：**添加新的模型提供商只需实现`BaseLLM`接口

> **配置灵活：**通过YAML配置文件管理不同环境的设置

> **功能完整：**支持同步/流式聊天、token计算、嵌入向量等

> **错误处理：**完善的错误处理和重试机制

> **日志记录：**使用装饰器进行统一的日志记录

## 上下文管理实现

上下文管理是智能体系统的核心组件，负责维护对话历史、管理token使用、优化内存占用。一个好的上下文管理系统能够显著提升智能体的对话质量和响应效率。

### 4.1 ContextStore 核心实现

ContextStore是系统中负责管理对话上下文的核心组件，基于`agent_id`来组织和管理不同智能体的上下文数据。



## 4 上下文与内存管理

### 4.1 ContextStore 实现

```python
# llm/memory/context_store.py
from llm.memory.message_utils import trim_messages

class ContextStore:
    def __init__(self):
        self.contexts = {}

    def set_context(self, agent_id, context: dict):
        """设置智能体的上下文"""
        self.contexts[agent_id] = context

    def get_context(self, agent_id):
        """获取智能体的上下文"""
        return self.contexts.get(agent_id, {})

    def clear_context(self, agent_id):
        """清空智能体的上下文"""
        if agent_id in self.contexts:
            del self.contexts[agent_id]

    def trim_context_messages(self, agent_id, max_tokens=4096, provider="openai", model_name=None):
        """智能裁剪上下文消息"""
        ctx = self.get_context(agent_id)
        if "messages" in ctx:
            ctx["messages"] = trim_messages(ctx["messages"], max_tokens=max_tokens, provider=provider, model_name=model_name)

    # 结构化步骤管理
    def set_steps(self, agent_id, steps):
        """设置智能体的执行步骤"""
        ctx = self.get_context(agent_id)
        ctx["steps"] = steps
        self.set_context(agent_id, ctx)

    def get_steps(self, agent_id):
        """获取智能体的执行步骤"""
        ctx = self.get_context(agent_id)
        return ctx.get("steps", [])

    def replace_step(self, agent_id, step_index, new_step):
        """替换指定步骤"""
        ctx = self.get_context(agent_id)
        steps = ctx.get("steps", [])
        if 0 <= step_index < len(steps):
            steps[step_index] = new_step
            ctx["steps"] = steps
            self.set_context(agent_id, ctx)

    def insert_step(self, agent_id, step_index, new_step):
        """插入新步骤"""
        ctx = self.get_context(agent_id)
        steps = ctx.get("steps", [])
        if 0 <= step_index <= len(steps):
            steps.insert(step_index, new_step)
            ctx["steps"] = steps
            self.set_context(agent_id, ctx)
```

**核心功能说明：**

> **上下文存储：**使用字典结构按`agent_id`存储上下文

> **智能裁剪：**调用`trim_messages`函数进行token优化

> **步骤管理：**支持ReAct推理步骤的增删改查

> **内存管理：**提供上下文清理功能

### 4.2 MemoryManager 实现

`MemoryManager`负责管理智能体的对话历史，提供简单的历史记录功能。

```python
# llm/memory/memory_manager.py
class MemoryManager:
    def __init__(self):
        self.histories = {}

    def add_message(self, agent_id, message):
        """添加消息到历史记录"""
        if agent_id not in self.histories:
            self.histories[agent_id] = []
        self.histories[agent_id].append(message)

    def get_history(self, agent_id):
        """获取智能体的历史记录"""
        return self.histories.get(agent_id, [])

    def clear_history(self, agent_id):
        """清空智能体的历史记录"""
        if agent_id in self.histories:
            self.histories[agent_id] = []
```

**核心功能说明**：

+   **历史存储**：按`agent_id`组织对话历史

+   **消息管理**：支持消息的添加和查询

+   **内存清理**：提供历史记录清理功能

### 4.3 智能消息裁剪

`message_utils`模块提供了智能的消息裁剪和token计算功能。

```python
# llm/memory/message_utils.py
from llm.adapters.tokenizer import count_tokens
from typing import List, Dict, Any
import logging

logger = logging.getLogger(__name__)

def calculate_message_tokens(message: Dict[str, Any], provider: str = "openai", model_name: str = None) -> int:
    """计算单条消息的token数量"""
    content = message.get("content", "")
    if not content:
        return 0
        
    try:
        token_info = count_tokens(content, provider=provider, model_name=model_name)
        return token_info["token_count"]
    except Exception as e:
        logger.error(f"计算token数量时出错: {str(e)}")
        # 如果出错，返回一个安全的估计值
        return len(content) // 4  # 假设每个token平均4个字符

def trim_messages(messages: List[Dict[str, Any]], max_tokens: int = 4096, provider: str = "openai", model_name: str = None) -> List[Dict[str, Any]]:
    """智能裁剪多轮对话消息"""
    if not messages:
        return []

    # 计算每条消息的token数
    message_tokens = [(msg, calculate_message_tokens(msg, provider, model_name)) for msg in messages]
    total_tokens = sum(tokens for _, tokens in message_tokens)
    
    # 如果总token数在限制范围内，直接返回
    if total_tokens <= max_tokens:
        return messages

    # 动态窗口裁剪策略
    trimmed = []
    current_tokens = 0
    
    # 1. 始终保留系统提示词
    system_messages = [(msg, tokens) for msg, tokens in message_tokens if msg.get("role") == "system"]
    for msg, tokens in system_messages:
        trimmed.append(msg)
        current_tokens += tokens
    
    # 2. 保留最新的用户和助手消息对
    remaining_messages = [(msg, tokens) for msg, tokens in message_tokens if msg.get("role") != "system"]
    remaining_messages.reverse()  # 从最新的消息开始处理
    
    for msg, tokens in remaining_messages:
        if current_tokens + tokens > max_tokens:
            break
        trimmed.insert(len(system_messages), msg)  # 在系统消息后插入
        current_tokens += tokens
    
    # 记录裁剪信息
    removed_count = len(messages) - len(trimmed)
    if removed_count > 0:
        logger.info(f"上下文裁剪: 移除了 {removed_count} 条消息，从 {total_tokens} tokens 减少到 {current_tokens} tokens")
    
    return trimmed

def get_context_window(messages: List[Dict[str, Any]], max_tokens: int = 4096, provider: str = "openai", model_name: str = None) -> Dict[str, Any]:
    """获取当前上下文的统计信息"""
    total_tokens = sum(calculate_message_tokens(msg, provider, model_name) for msg in messages)
    system_tokens = sum(calculate_message_tokens(msg, provider, model_name) 
                       for msg in messages if msg.get("role") == "system")
    user_tokens = sum(calculate_message_tokens(msg, provider, model_name) 
                     for msg in messages if msg.get("role") == "user")
    assistant_tokens = sum(calculate_message_tokens(msg, provider, model_name) 
                          for msg in messages if msg.get("role") == "assistant")
    
    return {
        "total_tokens": total_tokens,
        "max_tokens": max_tokens,
        "system_tokens": system_tokens,
        "user_tokens": user_tokens,
        "assistant_tokens": assistant_tokens,
        "remaining_tokens": max_tokens - total_tokens,
        "message_count": len(messages),
        "system_messages": len([m for m in messages if m.get("role") == "system"]),
        "user_messages": len([m for m in messages if m.get("role") == "user"]),
        "assistant_messages": len([m for m in messages if m.get("role") == "assistant"])
    }
```

**核心功能说明**：

+   **Token计算**：支持多种模型的token计算

+   **智能裁剪**：优先保留系统消息和最新对话

+   **统计信息**：提供详细的上下文使用统计

+   **错误处理**：包含完善的异常处理机制

### 4.4 上下文管理流程

![](img/235abcc3ce30b572d7b9b933933cb1ff.png)

### 4.5 上下文数据结构使用范例



```python
# llm/memory/context.py
from dataclasses import dataclass, field
from typing import List, Dict, Any, Optional
from datetime import datetime
from enum import Enum

class MessageRole(Enum):
    """消息角色枚举"""
    SYSTEM = "system"
    USER = "user"
    ASSISTANT = "assistant"
    TOOL = "tool"

@dataclass
class ContextMessage:
    """上下文消息"""
    role: MessageRole
    content: str
    timestamp: datetime = field(default_factory=datetime.now)
    metadata: Dict[str, Any] = field(default_factory=dict)
    token_count: Optional[int] = None

@dataclass
class ConversationContext:
    """对话上下文"""
    session_id: str
    messages: List[ContextMessage] = field(default_factory=list)
    max_tokens: int = 4000  # 最大token限制
    current_tokens: int = 0
    created_at: datetime = field(default_factory=datetime.now)
    updated_at: datetime = field(default_factory=datetime.now)
    
    def add_message(self, message: ContextMessage):
        """添加消息到上下文"""
        self.messages.append(message)
        self.current_tokens += message.token_count or 0
        self.updated_at = datetime.now()
        
        # 如果超出token限制，进行裁剪
        if self.current_tokens > self.max_tokens:
            self._trim_context()
    
    def _trim_context(self):
        """裁剪上下文，保留最重要的消息"""
        # 保留系统消息和最新的消息
        system_messages = [msg for msg in self.messages if msg.role == MessageRole.SYSTEM]
        other_messages = [msg for msg in self.messages if msg.role != MessageRole.SYSTEM]
        
        # 从最新的消息开始，逐步添加直到接近token限制
        trimmed_messages = system_messages.copy()
        current_tokens = sum(msg.token_count or 0 for msg in system_messages)
        
        for msg in reversed(other_messages):
            if current_tokens + (msg.token_count or 0) <= self.max_tokens * 0.8:  # 保留20%缓冲
                trimmed_messages.insert(len(system_messages), msg)
                current_tokens += msg.token_count or 0
            else:
                break
        
        self.messages = trimmed_messages
        self.current_tokens = current_tokens
```

通过这样的上下文管理系统，实现了：

> **智能裁剪：**自动管理token使用，避免超出限制

> **类型安全：**使用枚举和类型注解，确保代码质量

> **持久化存储：**支持会话的保存和恢复

> **性能优化：**异步操作和智能缓存策略

> **易于监控：**提供详细的统计信息

## 智能体搭建详解

智能体是系统的核心执行单元，负责协调LLM推理、工具调用和上下文管理。本节将详细介绍智能体的搭建过程、架构设计和实现细节。

### 5.1 智能体架构设计

#### 5.1.1 整体架构图

![](img/d0c51b89650cd13a91dfe11eb4b15f1c.png)

#### 5.1.2 组件关系图

![](img/4fa61253fedfd96df76fd91157ae5284.png)

### 5.2 智能体创建流程

#### 5.2.1 创建流程图

![](img/5cf1af7b0591b04a14bee5b22b952975.png)

#### 5.2.2 代码实现详解

1. `AgentManager` - 智能体管理器

```python
# llm/agent/agent_manager.py
from llm.agent.react_agent import ReActAgent
from llm.config.config import YAMLConfig

class AgentManager:
    def __init__(self, provider_path="llm_config.yaml", roles_path="roles.yaml"):
        # 初始化配置管理器
        self.config = YAMLConfig(provider_path, roles_path)
        # 智能体实例池
        self.agents = {}

    def create_agent(self, provider, role, user_content, max_steps=5, agent_id=None):
        """
        创建智能体实例
        Args:
            provider: 大模型提供商 (openai, siliconflow, deepseek等)
            role: 角色名称 (assistant, coder, analyst等)
            user_content: 用户输入内容
            max_steps: 最大推理步数
            agent_id: 智能体唯一标识
        Returns:
            ReActAgent: 智能体实例
        """
        agent = ReActAgent(
            provider=provider,
            role=role,
            user_content=user_content,
            config=self.config,
            max_steps=max_steps
        )
        # 如果指定了agent_id，则保存到实例池
        if agent_id:
            self.agents[agent_id] = agent
        return agent

    def get_agent(self, agent_id):
        """获取已创建的智能体"""
        return self.agents.get(agent_id)

    def remove_agent(self, agent_id):
        """移除智能体实例"""
        if agent_id in self.agents:
            del self.agents[agent_id]
```

1. `ReActAgent` - 核心智能体

```python
# llm/agent/react_agent.py
class ReActAgent:
    def __init__(self, provider, role, user_content, config, max_steps=5, agent_id=None):
        """
        初始化ReAct智能体
        Args:
            provider: 大模型提供商
            role: 角色名称
            user_content: 用户输入
            config: 配置对象
            max_steps: 最大推理步数
            agent_id: 智能体ID
        """
        Logger.info(f"初始化ReActAgent: provider={provider}, role={role}")
        
        # 基础属性设置
        self.provider = provider
        self.role = role
        self.user_content = user_content
        self.config = config
        self.max_steps = max_steps
        self.agent_id = agent_id or f"agent-{id(self)}"
        
        # 创建LLM适配器
        self.llm = LLMFactory.create_from_yaml(
            provider_name=provider,
            role_name=role,
            yaml_path=config.provider_path,
            user_content=user_content
        )
        
        # 初始化消息队列
        system_prompt = config.get_role(role).get("system_prompt", "")
        self.messages = [
            {"role": "system", "content": system_prompt},
            {"role": "user", "content": user_content}
        ]
        
        # 初始化上下文和历史管理
        self.context_store = ContextStore()
        self.memory_manager = MemoryManager()
        self.context_store.set_context(self.agent_id, {"messages": list(self.messages)})
        self.memory_manager.clear_history(self.agent_id)
```

### 5.3 推理执行流程

#### 5.3.1 ReAct推理循环流程图

![](img/e0bff42e7635e79098296c436428cc41.png)

#### 5.3.2 核心推理方法



```
@log_method(module="agent", level="INFO")
async def run(self):
    """
    执行ReAct推理循环
    Returns:
        str: 最终答案或None
    """
    for step in range(self.max_steps):
        Logger.info(f"第{step+1}轮对话开始，当前消息: {self.messages}")
        
        try:
            # 1. 调用LLM进行推理
            reply = await self.llm.chat(self.messages)
            Logger.info(f"LLM回复: {reply}")
            
            # 2. 解析结构化步骤
            steps = parse_steps(reply)
            
            # 3. 检查是否有最终答案
            has_answer, answer = self._check_final_answer(steps)
            if has_answer:
                Logger.info(f"获得最终答案: {answer}")
                return answer
            
            # 4. 更新上下文
            self._update_context(reply, steps)
            
        except Exception as e:
            Logger.error(f"LLM调用异常: {e}")
            return None

        # 5. 执行工具调用
        for i, step_item in enumerate(steps):
            if step_item["type"].lower() in ["action", "行动"]:
                # 解析工具调用
                func, args = parse_action(step_item["content"])
                
                if func and func in TOOLS:
                    try:
                        # 执行工具调用
                        if isinstance(args, dict):
                            result = call_tool(func, **args)
                        elif isinstance(args, (list, tuple)):
                            result = call_tool(func, *args)
                        else:
                            result = call_tool(func, args)
                        
                        Logger.info(f"工具调用结果: {result}")
                        
                        # 处理工具结果
                        result_str = self._process_tool_result(result)
                        
                        # 更新上下文中的Observation
                        self._update_observation(i, steps, result_str)
                        
                    except Exception as e:
                        result = f"工具调用出错: {e}"
                        Logger.error(result)
                else:
                    result = "Thought: 我不太确定如何处理，请重试。"
                    Logger.warning(f"未识别的工具或参数: {func}")
                
                break  # 只处理第一个Action

    Logger.warning("未获得最终答案，请检查模型输出或工具注册。")
    return None
```

## 5.4 输出解析器

### 5.4.1 解析器架构

![](img/1a4ff1af0c10c5c678f32161013665c3.png)

### 5.4.2 核心解析方法

```
# llm/agent/parsers.py

def parse_action(text):
    """
    解析Action步骤，支持多种格式
    Args:
        text: 包含Action的文本
    Returns:
        tuple: (工具名称, 参数)
    """
    # 1. 优先解析JSON格式
    pattern = r'Action[：:][ \t]*(.*)'
    for line in text.splitlines():
        match = pattern.match(line)
        if match:
            action_content = match.group(1).strip()
            
            # 尝试JSON解析
            try:
                action_json = json.loads(action_content)
                if (isinstance(action_json, dict) 
                    and action_json.get("method") == "tools/call"
                    and "params" in action_json):
                    name = action_json["params"].get("name")
                    arguments = action_json["params"].get("arguments", {})
                    return name, arguments
            except Exception:
                pass
            
            # 2. 兼容函数调用格式
            pattern_func = r'([a-zA-Z_][a-zA-Z0-9_]*)\((.*)\)'
            match_func = re.search(pattern_func, action_content)
            if match_func:
                func_name = match_func.group(1)
                arg_str = match_func.group(2)
                try:
                    args = ast.literal_eval(f"[{arg_str}]")
                    return func_name, args
                except Exception as e:
                    return func_name, [f"参数解析失败：{e}"]
    
    return None, None

def parse_steps(text):
    """
    解析多行推理步骤
    Args:
        text: LLM输出的完整文本
    Returns:
        list: 结构化步骤列表
    """
    pattern = re.compile(
        r"^(?P<step>\d+)?\s*(?P<type>思考|Thought|行动|Action|观察|Observation|最终答案|Answer)[：:][ \t]*(?P<content>.*)",
        re.I
    )
    
    results = []
    lines = text.splitlines()
    i = 0
    step_counter = 1
    
    while i < len(lines):
        match = pattern.match(lines[i])
        if match:
            step = match.group("step")
            type_ = match.group("type")
            content_lines = [match.group("content").strip()]
            i += 1
            
            # 收集后续内容行
            while i < len(lines) and not pattern.match(lines[i]):
                content_lines.append(lines[i].rstrip())
                i += 1
            
            content = "\n".join(content_lines).strip()
            results.append({
                "step": int(step) if step else step_counter,
                "type": type_,
                "content": content
            })
            step_counter += 1
        else:
            i += 1
    
    return results
```

## 5.5 上下文管理集成

### 5.5.1 上下文更新流程

![](img/2179c9e94c60ed21badc5d35d1df6d8e.png)

### 5.5.2 上下文更新实现

```
def _update_context(self, reply: str, steps: list):
    """
    更新上下文，包括消息队列和步骤
    Args:
        reply: LLM的回复
        steps: 结构化步骤
    """
    # 1. 获取当前上下文
    context = self.context_store.get_context(self.agent_id)
    messages = context.get("messages", [])
    
    # 2. 添加新的助手回复
    messages.append({"role": "assistant", "content": reply})
    
    # 3. 更新步骤
    self.context_store.set_steps(self.agent_id, steps)
    
    # 4. 获取上下文统计信息
    context_info = get_context_window(messages, provider=self.provider)
    Logger.info(f"上下文统计: {context_info}")
    
    # 5. 智能裁剪（如果Token接近限制）
    if context_info["remaining_tokens"] < 1000:  # 安全阈值
        Logger.info("Token数接近限制，进行上下文裁剪")
        messages = trim_messages(messages, provider=self.provider)
        context_info = get_context_window(messages, provider=self.provider)
        Logger.info(f"裁剪后上下文统计: {context_info}")
    
    # 6. 更新上下文
    context["messages"] = messages
    self.context_store.set_context(self.agent_id, context)
    
    # 7. 更新记忆
    self.memory_manager.add_message(self.agent_id, reply)
    
    # 8. 更新当前消息队列
    self.messages = messages
```

## 5.6 使用示例

### 5.6.1 基础智能体创建

```
from llm.agent.agent_manager import AgentManager

# 1. 创建智能体管理器
agent_manager = AgentManager()

# 2. 创建智能体
agent = agent_manager.create_agent(
    provider="openai",
    role="assistant",
    user_content="请帮我分析一下今天的天气情况",
    max_steps=5
)

# 3. 执行推理
result = await agent.run()
print("智能体回答:", result)
```

### 5.6.2 多轮对话智能体

```
# 创建智能体
agent = agent_manager.create_agent(
    provider="openai",
    role="coder",
    user_content="请帮我写一个Python函数来计算斐波那契数列",
    agent_id="my_coder_agent"
)

# 第一轮对话
result1 = await agent.run()
print("第一轮回答:", result1)

# 继续对话（智能体会记住上下文）
agent.user_content = "能优化一下这个函数的性能吗？"
result2 = await agent.run()
print("第二轮回答:", result2)

# 获取智能体历史
history = agent.memory_manager.get_history("my_coder_agent")
print("对话历史:", history)
```

### 5.6.3 流式输出智能体

```
# 创建支持流式输出的智能体
agent = agent_manager.create_agent(
    provider="openai",
    role="assistant",
    user_content="请详细解释一下机器学习的基本概念"
)

# 流式执行
async for chunk in agent.run_stream():
    print(chunk, end="", flush=True)
```

# MCP工具系统详解



# MCP工具系统

MCP（Model Context Protocol）工具系统是智能体的重要扩展能力，允许智能体调用外部工具来执行特定任务。下面详细介绍MCP工具的初始化注册、架构设计和智能体调用流程。

## 6.1 MCP工具系统架构

### 6.1.1 整体架构图

![](img/d0f6e51290439116e78a1c59b254c66a.png)

### 6.1.2 工具类型分类

![](img/97ecb36bd5c6596e2095ff8985fced30.png)

## 6.2 MCP工具初始化注册

### 6.2.1 初始化流程图

```
flowchart TD
    A[系统启动] --> B[加载mcp_config.yaml]
    B --> C{配置文件存在?}
    C -->|否| D[跳过MCP初始化]
    C -->|是| E[解析MCP配置]
    E --> F[遍历服务器配置]
    F --> G{传输方式?}
    G -->|stdio| H[创建MCPClientAdapter]
    G -->|其他| I[跳过(暂不支持)]
    H --> J[建立进程通信]
    J --> K[获取工具列表]
    K --> L[动态注册工具函数]
    L --> M[更新TOOL_REGISTRY]
    M --> N[MCP工具注册完成]
    D --> O[系统就绪]
    N --> O
```

### 6.2.2 核心注册代码

1. 工具注册表 (`ToolRegistry`)

```
# llm/tools/tool_registry.py
import os
import yaml
from llm.logging.decorators import log_method
from .mcp_tools import register_mcp_tools

# 全局工具注册表
TOOL_REGISTRY = {}

# 加载MCP配置文件
MCP_CONFIG_PATH = os.path.join(os.path.dirname(os.path.dirname(__file__)), '..', 'mcp_config.yaml')
MCP_CONFIG_PATH = os.path.abspath(MCP_CONFIG_PATH)

def load_mcp_tools():
    """加载MCP工具"""
    if os.path.exists(MCP_CONFIG_PATH):
        with open(MCP_CONFIG_PATH, 'r', encoding='utf-8') as f:
            mcp_config = yaml.safe_load(f)
        
        servers = mcp_config.get('servers', {})
        for name, conn in servers.items():
            if conn.get('transport') == 'stdio':
                # 只注册本地stdio方式，后续可扩展其他方式
                tool_funcs = register_mcp_tools(conn)
                TOOL_REGISTRY.update(tool_funcs)
                print(f"已注册MCP工具: {list(tool_funcs.keys())}")

# 系统启动时自动加载MCP工具
load_mcp_tools()

def register_tool(name):
    """工具注册装饰器"""
    def decorator(func):
        TOOL_REGISTRY[name] = func
        return func
    return decorator

@log_method(module="tool", level="INFO")
def call_tool(name, *args, **kwargs):
    """调用工具"""
    if name not in TOOL_REGISTRY:
        raise ValueError(f"未注册的工具: {name}")
    return TOOL_REGISTRY[name](*args, **kwargs)

# 兼容旧代码，导出 TOOLS
TOOLS = TOOL_REGISTRY
```

2. MCP客户端适配器 (`MCPClientAdapter`)

```
# llm/tools/mcp_tools.py
import subprocess
import threading
import json
import uuid
import time
from typing import Dict, Any

class MCPClientAdapter:
    """MCP客户端适配器，负责与MCP服务器通信"""
    
    def __init__(self, command, args=None, cwd=None, env=None, encoding="utf-8"):
        """
        初始化MCP客户端
        Args:
            command: MCP服务器命令
            args: 命令行参数
            cwd: 工作目录
            env: 环境变量
            encoding: 编码格式
        """
        # 启动MCP服务器进程
        self.proc = subprocess.Popen(
            [command] + (args or []),
            stdin=subprocess.PIPE,
            stdout=subprocess.PIPE,
            stderr=subprocess.PIPE,
            cwd=cwd,
            env=env,
            text=True,
            encoding=encoding,
            bufsize=1
        )
        
        # 线程安全锁和待处理请求
        self.lock = threading.Lock()
        self.pending = {}
        
        # 启动监听线程
        threading.Thread(target=self._listen, daemon=True).start()

    def _listen(self):
        """监听MCP服务器响应"""
        for line in self.proc.stdout:
            try:
                resp = json.loads(line)
                req_id = str(resp.get("id"))
                if req_id in self.pending:
                    self.pending[req_id] = resp
            except Exception as e:
                print("[MCPClientAdapter] 解析响应失败:", e, line)

    def send_request(self, method, params=None, timeout=10):
        """
        发送请求到MCP服务器
        Args:
            method: 方法名
            params: 参数
            timeout: 超时时间
        Returns:
            dict: 服务器响应
        """
        req_id = str(uuid.uuid4())
        req = {
            "jsonrpc": "2.0",
            "id": req_id,
            "method": method,
            "params": params or {}
        }
        
        with self.lock:
            self.pending[req_id] = None
            self.proc.stdin.write(json.dumps(req) + "\n")
            self.proc.stdin.flush()
        
        # 等待响应
        start = time.time()
        while self.pending[req_id] is None:
            if time.time() - start > timeout:
                raise TimeoutError(f"MCPClient等待响应超时: {method}")
            time.sleep(0.05)
        
        resp = self.pending.pop(req_id)
        return resp

    def list_tools(self):
        """获取可用工具列表"""
        resp = self.send_request("tools/list")
        return resp["result"]["tools"]

    def call_tool(self, name, arguments):
        """调用指定工具"""
        resp = self.send_request("tools/call", {"name": name, "arguments": arguments})
        return resp["result"]["content"]
```

3. MCP工具注册函数

```
# llm/tools/mcp_tools.py

# 全局变量
MCP_TOOL_FUNCS = {}
MCP_CLIENT = None

def register_mcp_tools(config: Dict[str, Any]):
    """
    注册MCP工具
    Args:
        config: MCP服务器配置
    Returns:
        dict: 注册的工具函数字典
    """
    global MCP_CLIENT, MCP_TOOL_FUNCS
    
    # 解析配置
    command = config["command"]
    args = config.get("args", [])
    cwd = config.get("cwd")
    env = config.get("env")
    encoding = config.get("encoding", "utf-8")
    
    # 创建MCP客户端
    MCP_CLIENT = MCPClientAdapter(command, args, cwd, env, encoding)
    
    # 获取工具列表
    tools = MCP_CLIENT.list_tools()
    
    # 动态注册每个工具
    for tool in tools:
        name = tool["name"]
        input_schema = tool["inputSchema"]
        
        def make_tool_func(tool_name, input_schema):
            """创建工具函数"""
            def tool_func(**kwargs):
                # 参数校验可扩展
                return MCP_CLIENT.call_tool(tool_name, kwargs)
            
            tool_func.__name__ = f"{tool_name}"
            tool_func.__doc__ = f"MCP工具: {tool_name}\n参数: {input_schema}"
            return tool_func
        
        MCP_TOOL_FUNCS[f"{name}"] = make_tool_func(name, input_schema)
    
    return MCP_TOOL_FUNCS
```

## 6.3 内置工具系统

### 6.3.1 内置工具列表

```
# llm/tools/builtin_tools.py
from llm.tools.tool_registry import register_tool
import re
import json

@register_tool("add")
def add(a, b):
    """加法运算工具"""
    return str(float(a) + float(b))

@register_tool("mul")
def mul(a, b):
    """乘法运算工具"""
    return str(float(a) * float(b))

@register_tool("get_weather")
def get_weather(city):
    """获取天气信息工具"""
    return f"{city}今日天气为12℃，多云。"

@register_tool("check_answer")
def check_answer(answer):
    """检查答案工具"""
    return f"{answer} 是合格的题目。"

@register_tool("extract_json_from_response")
def extract_json_from_response_tool(response: str) -> str:
    """
    从大模型输出中提取JSON字符串
    Args:
        response: 大模型原始输出
    Returns:
        str: 提取到的JSON字符串
    """
    response = re.sub(r'^```\w*\n|```$', '', response.strip())

    start_idx = response.find('{')

    end_idx = response.rfind('}')

    if start_idx >= 0 and end_idx > start_idx:

        json_str = response[start_idx:end_idx + 1]

        try:

            json.loads(json_str)

            return json_str

        except Exception:
```



```

            return "";

    return "";

```

#### 6.3.2 插件工具扩展

```python
# llm/tools/plugin_tools.py

# 这里可以注册外部API或自定义插件工具

from llm.tools.tool_registry import register_tool

@register_tool("search")

def search(query):

    """搜索工具示例"""

    # 调用外部搜索API

    return f"搜索结果：{query}"

@register_tool("custom_api_call")

def custom_api_call(endpoint, data):

    """自定义API调用工具"""

    # 实现自定义API调用逻辑

    return f"API调用结果：{endpoint} - {data}"
```

### 6.4 智能体调用MCP工具流程

#### 6.4.1 工具调用流程图

![](`img/7b206c5be71e159984cd7b1fdafea3dc.png`)

#### 6.4.2 工具调用实现

1. 智能体中的工具调用处理

```python
# llm/agent/react_agent.py 中的相关代码

from llm.tools.tool_registry import call_tool, TOOLS

from llm.agent.parsers import parse_action

async def run(self):

    """执行ReAct推理循环"""

    for step in range(self.max_steps):

        try:

            # 1. 调用LLM进行推理

            reply = await self.llm.chat(self.messages)

            # 2. 解析结构化步骤

            steps = parse_steps(reply)

            # 3. 检查是否有最终答案

            has_answer, answer = self._check_final_answer(steps)

            if has_answer:

                return answer

            # 4. 更新上下文

            self._update_context(reply, steps)

        except Exception as e:

            Logger.error(f"LLM调用异常: {e}")

            return None

        # 5. 执行工具调用

        for i, step_item in enumerate(steps):

            if step_item["type"].lower() in ["action", "行动"]:

                # 解析工具调用

                func, args = parse_action(step_item["content"])

                if func and func in TOOLS:

                    try:

                        # 执行工具调用

                        if isinstance(args, dict):

                            result = call_tool(func, **args)

                        elif isinstance(args, (list, tuple)):

                            result = call_tool(func, *args)

                        else:

                            result = call_tool(func, args)

                        Logger.info(f"工具调用结果: {result}")

                        # 处理工具结果

                        result_str = self._process_tool_result(result)

                        # 更新上下文中的Observation

                        self._update_observation(i, steps, result_str)

                    except Exception as e:

                        result = f"工具调用出错: {e}"

                        Logger.error(result)

                else:

                    result = "Thought: 我不太确定如何处理，请重试。"

                    Logger.warning(f"未识别的工具或参数: {func}")

                break  # 只处理第一个Action
```

1. Action解析器

```python
# llm/agent/parsers.py 中的相关代码

def parse_action(text):

    """

    解析Action步骤，支持多种格式

    Args:

        text: 包含Action的文本

    Returns:

        tuple: (工具名称, 参数)

    """

    # 1. 优先解析JSON格式

    pattern = r'Action[：:][ \t]*(.*)'

    for line in text.splitlines():

        match = re.search(pattern, line)

        if match:

            action_content = match.group(1).strip()

            # 尝试JSON解析 (MCP标准格式)

            try:

                action_json = json.loads(action_content)

                if (isinstance(action_json, dict) 

                    and action_json.get("method") == "tools/call"

                    and "params" in action_json):

                    name = action_json["params"].get("name")

                    arguments = action_json["params"].get("arguments", {})

                    return name, arguments

            except Exception:

                pass

            # 2. 兼容函数调用格式

            pattern_func = r'([a-zA-Z_][a-zA-Z0-9_]*)\((.*)\)'

            match_func = re.search(pattern_func, action_content)

            if match_func:

                func_name = match_func.group(1)

                arg_str = match_func.group(2)

                try:

                    args = ast.literal_eval(f"[{arg_str}]")

                    return func_name, args

                except Exception as e:

                    return func_name, [f"参数解析失败：{e}"]

    return None, None
```

### 6.5 MCP配置文件示例

#### 6.5.1 配置文件结构

```yaml
# mcp_config.yaml

servers:

  filesystem:

    transport: stdio

    command: npx

    args: ["-y", "@modelcontextprotocol/server-filesystem", "/tmp/mcp-filesystem"]

    env:

      MCP_FILESYSTEM_ROOT: /tmp/mcp-filesystem

  sqlite:

    transport: stdio

    command: npx

    args: ["-y", "@modelcontextprotocol/server-sqlite", "/tmp/mcp-sqlite.db"]

    env:

      MCP_SQLITE_DB_PATH: /tmp/mcp-sqlite.db

  github:

    transport: stdio

    command: npx

    args: ["-y", "@modelcontextprotocol/server-github"]

    env:

      GITHUB_PERSONAL_ACCESS_TOKEN: your_token_here
```

#### 6.5.2 配置说明

- `transport`: 传输方式，目前支持`stdio`
- `command`: MCP服务器启动命令
- `args`: 命令行参数
- `env`: 环境变量配置
- `cwd`: 工作目录（可选）

### 6.6 使用示例

#### 6.6.1 基础工具调用

```python
from llm.agent.agent_manager import AgentManager

# 创建智能体

agent_manager = AgentManager()

agent = agent_manager.create_agent(

    provider="openai",

    role="assistant",

    user_content="请计算 15 + 27 的结果",

    max_steps=5

)

# 执行推理（智能体会自动调用add工具）

result = await agent.run()

print("计算结果:", result)
```

#### 6.6.2 MCP工具调用

```python
# 创建支持MCP工具的智能体

agent = agent_manager.create_agent(

    provider="openai",

    role="assistant",

    user_content="请帮我查看当前目录下的文件列表",

    max_steps=5

)

# 执行推理（智能体会调用MCP文件系统工具）

result = await agent.run()

print("文件列表:", result)
```

#### 6.6.3 自定义工具注册

```python
from llm.tools.tool_registry import register_tool

@register_tool("custom_calculator")

def custom_calculator(operation, a, b):

    """自定义计算器工具"""

    if operation == "add":

        return str(float(a) + float(b))

    elif operation == "subtract":

        return str(float(a) - float(b))

    elif operation == "multiply":

        return str(float(a) * float(b))

    elif operation == "divide":

        return str(float(a) / float(b))

    else:

        return "不支持的操作"

# 现在智能体可以使用这个自定义工具了
```
