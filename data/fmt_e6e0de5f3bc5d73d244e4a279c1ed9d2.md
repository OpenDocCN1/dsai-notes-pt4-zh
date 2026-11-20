
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
