
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
