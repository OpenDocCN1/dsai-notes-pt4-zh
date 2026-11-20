
```
! pip show langchain
```

```
Name: langchain
Version: 1.1.3
Summary: Building applications with LLMs through composability
Home-page: https://docs.langchain.com/
Author: 
Author-email: 
License: MIT
Location: /root/anaconda3/envs/langchainskill/lib/python3.12/site-packages
Requires: langchain-core, langgraph, pydantic
Required-by: 
```

```
# 基础库导入
import os
import sys
from pathlib import Path
from typing import List, Callable, Any, Optional
from typing_extensions import TypedDict

# 加载环境变量
from dotenv import load_dotenv
load_dotenv(override=True)

# LangChain 1.1 核心导入
from langchain.agents import create_agent
from langchain.agents.middleware import (
    AgentMiddleware,
    ModelRequest,
    ModelResponse,
)
from langchain_core.tools import BaseTool, tool
from langchain_core.messages import HumanMessage, AIMessage, BaseMessage

print("核心库导入成功")
```

```
核心库导入成功
```

上面的代码导入了我们需要的所有核心组件。特别注意 `langchain.agents.middleware` 模块，这是 LangChain 1.1 新增的关键模块，包含了：

- `AgentMiddleware`：中间件基类，我们需要继承它；
- `ModelRequest`：封装了模型调用请求的所有信息（消息、工具、状态等）；
- `ModelResponse`：模型调用的响应；

## 3.2 配置 DeepSeek-v3.2 模型

在本教程中，我们使用 DeepSeek 的 deepseek-reasoner 模型作为底层 LLM。这个模型的特点是支持「推理过程」输出，可以让我们看到模型的思考过程。

> 提示：你也可以替换为 OpenAI、Anthropic 或其他兼容 LangChain 的模型。核心的 Middleware 机制是通用的。

这里我们使用的是最新发布的 DeepSeek-v3.2 推理模型。但需要说明的是：因为 DeepSeek-v3.2 模型刚刚发布，其推理模式在 `langChian 1.1` 的新版本中存在兼容性问题，在进行工具调用过程中会报错如下：

![](img/d568326a42323186389000fe137ea97f.png)

因此，我们团队也是完全重写了 DeepSeek-v3.2-reasoner 的模型适配器，大家扫码即可领取项目文件：

![](img/c95caef893f565c9cdba61dc8d618538.png)

![](img/2e7b4e4ec75acb819ce4f9a00fe66703.png)

```
# 添加项目路径（用于导入自定义模型）
PROJECT_ROOT = Path.cwd()
sys.path.insert(0, str(PROJECT_ROOT))

# 导入自定义的 DeepSeek 模型适配器
from skill_system.models import DeepSeekReasonerChatModel

# 检查 API Key
api_key = os.getenv("DEEPSEEK_API_KEY")
if not api_key:
    print("未设置 DEEPSEEK_API_KEY")
    print("   请在 .env 文件中添加: DEEPSEEK_API_KEY=your-key")
else:
    print(f"API Key 已配置 (前8位: {api_key[:8]}...)")

# 创建模型实例
model = DeepSeekReasonerChatModel(
    api_key=api_key,
    model_name="deepseek-reasoner",
    temperature=0.7
)

print(f"DeepSeek 模型已创建")
```

```
API Key 已配置 (前8位: sk-705c5...)
DeepSeek 模型已创建
```

`DeepSeekReasonerChatModel` 是我们自定义的模型适配器，它继承自 `LangChain` 的 `BaseChatModel`，并添加了对 DeepSeek 推理模型的支持。关键特性是它会保留模型返回的 `reasoning_content`（推理过程），这对于调试和理解模型行为非常有帮助。

## 3.3 配置 LangChain 1.1 Middleware

中间件（Middleware）是一个非常常见的设计模式。它允许你在请求处理的过程中插入自定义逻辑，而不需要修改核心代码。

`LangChain 1.1` 引入的 Agent Middleware 遵循同样的理念：

```
请求 ──▶ [中间件1] ──▶ [中间件2] ──▶ ... ──▶ [核心处理] ──▶ 响应
        │              │
        ▼              ▼
    可以修改请求    可以修改请求
```

在 Agent 的上下文中，Middleware 可以：

1. 拦截模型调用请求：在模型被调用之前获取请求信息
1. 修改请求参数：比如修改工具列表、系统提示等
1. 传递给下一个处理器：调用 `handler(request)` 继续执行
1. 处理响应：可以在返回之前修改响应（可选）

这正是实现动态工具过滤的关键！我们可以在 Middleware 中检查当前状态，然后只把相关的工具传递给模型。

在 `LangChain 1.1` 中 `AgentMiddleware` 基类的结构如下：

```
class AgentMiddleware:
    """Agent 中间件基类"""
    
    def wrap_model_call(
        self,
        request: ModelRequest,
        handler: Callable[[ModelRequest], ModelResponse]
    ) -> ModelResponse:
        """同步版本 - 包装模型调用"""
        return handler(request)  # 默认直接传递
    
    async def awrap_model_call(
        self,
        request: ModelRequest,
        handler: Callable[[ModelRequest], ModelResponse]
    ) -> ModelResponse:
        """异步版本 - 包装模型调用"""
        return await handler(request)  # 默认直接传递
```

`wrap_model_call` 参数说明

| 参数名 | 类型 | 描述 | 说明 |
| --- | --- | --- | --- |
| `request` | `ModelRequest` | 模型调用请求 | 包含 `messages`, `tools`, `state` 等 |
| `handler` | `Callable` | 下一个处理器 | 调用它来继续执行链 |
| 返回值 | `ModelResponse` | 模型响应 | `handler` 执行后的结果 |

`ModelRequest` 是 Middleware 中最重要的对象，它封装了模型调用的所有信息：

```
class ModelRequest:
    messages: List[BaseMessage]  # 消息历史
    tools: List[BaseTool]        # 可用工具列表
    state: Dict[str, Any]        # 当前状态（关键！）
    
    def override(self, **kwargs) -> ModelRequest:
        """创建一个修改了指定参数的新 Request"""
        # 返回新的 ModelRequest，保持不可变性
```

`request.override()` 是实现动态过滤的核心方法。它允许我们创建一个新的请求对象，其中某些参数被修改了，而其他参数保持不变。例如：

```
# 只修改工具列表，其他参数保持不变
filtered_request = request.override(tools=filtered_tools)
```

## 3.4 实现一个简单的日志 Middleware

在实现复杂的工具过滤之前，我们先写一个简单的日志 Middleware，帮助大家理解整个流程：

```
class LoggingMiddleware(AgentMiddleware):
    """
    日志中间件 - 记录每次模型调用的信息
    
    这是一个最简单的 Middleware 示例，用于理解基本工作流程
    """
    
    def __init__(self, name: str = "Logger"):
        super().__init__()
        self.name = name
        self.call_count = 0
    
    def wrap_model_call(
        self,
        request: ModelRequest,
        handler: Callable[[ModelRequest], ModelResponse]
    ) -> ModelResponse:
        """
        拦截模型调用，打印日志信息
        """
        self.call_count += 1
        
        # 1. 调用前：记录请求信息
        print(f"\n{'='*60}")
        print(f"[{self.name}] 第 {self.call_count} 次模型调用")
        print(f"{'='*60}")
        
        # 打印工具信息
        if hasattr(request, 'tools') and request.tools:
            tool_names = [t.name for t in request.tools]
            print(f"可用工具 ({len(tool_names)}个): {tool_names}")
        
        # 打印状态信息
        if hasattr(request, 'state') and request.state:
            print(f"当前状态: {request.state}")
        
        # 2. 调用下一个处理器（这里是实际的模型调用）
        response = handler(request)
        
        # 3. 调用后：可以处理响应（这里只是打印）
        print(f"模型调用完成")
        print(f"{'='*60}\n")
        
        return response

print("LoggingMiddleware 类已定义")
```

```
LoggingMiddleware 类已定义
```

上面的 `LoggingMiddleware` 展示了 Middleware 的基本结构：

1. 继承 `AgentMiddleware`：这是必须的
1. 重写 `wrap_model_call`：这是核心方法
1. 调用 `handler(request)`：必须调用，否则模型不会被执行
1. 返回响应：必须返回 `handler` 的结果

> 注意：如果你不调用 `handler(request)`，整个 Agent 链条就会中断！

## 3.5 定义工具与状态 Schema

在 `LangChain 1.1` 中，State Schema 用于定义 Agent 运行时需要追踪的状态信息。它使用 Python 的 `TypedDict` 来定义，提供类型安全和清晰的结构。

对于 Claude Skills，我们需要追踪一个关键状态：`skills_loaded` - 当前已加载的技能列表。这个状态会被 Middleware 读取，用于决定哪些工具应该暴露给模型。

```
# # 使用 MessagesState 而不是 TypedDict
from langgraph.graph import MessagesState
from typing import Annotated, List

# 第一种模式：替换模式
# # 定义 reducer 函数
# def skill_list_reducer(current: List[str], new: List[str]) -> List[str]:
#     """替换模式：用新列表替换旧列表"""
#     return new

# # 使用 MessagesState 作为基类
# class SkillState(MessagesState):
#     """
#     Skill 状态 Schema
    
#     使用 MessagesState 作为基类，它已经包含了 messages 字段
#     我们只需要添加 skills_loaded 字段
#     """
#     skills_loaded: Annotated[List[str], skill_list_reducer] = []


# 第二种模式：累计模式
# 修改 reducer 函数为累积模式
def skill_list_accumulator(current: List[str], new: List[str]) -> List[str]:
    """
    累积模式：合并已加载的 Skills
    保持所有已加载的技能，而不是替换
    """
    if not current:
        return new
    # 合并并去重，保持顺序
    combined = current + [s for s in new if s not in current]
    return combined

# 使用累积模式的 reducer
class SkillState(MessagesState):
    """
    Skill 状态 Schema
    """
    skills_loaded: Annotated[List[str], skill_list_accumulator] = []  # 改为累积模式
```

`SkillState` 定义了两个关键字段：

1. `messages`：消息历史，这是 `LangChain` Agent 的标准字段，用于存储对话历史；
1. `skills_loaded`：已加载的技能列表，这是我们自定义的字段，用于追踪当前激活的技能；

> 重要：`TypedDict` 在运行时本质上就是一个普通的 dict，所以在 Middleware 中我们使用 `request.state.get("skills_loaded", [])` 来访问它。

## 3.6 定义外部工具

现在，我们需要定义一组示例工具来实际展示动态过滤流程。我们会创建三类工具：

1. `Loader 工具`（始终可见）：用于加载技能；
1. 数据分析工具：只有加载了 `data_analysis` 技能后才可见；
