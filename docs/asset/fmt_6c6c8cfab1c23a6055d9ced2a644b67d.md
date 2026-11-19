
虽然LLM Agent的概念还在快速发展中，但根据其设计理念和能力侧重，我们可以大致将其分为几类：

## 任务导向型Agent（Task-Oriented Agents）

- **特点：** 专注于完成特定领域的、定义明确的任务，例如客户服务、代码生成、数据分析等。

- **工作方式：** 通常有预设的流程和可调用的特定工具集。LLM主要负责理解用户意图、填充任务槽位、生成回应或调用合适的工具。

- **例子：** 专门用于预订餐厅的聊天机器人、辅助编程的代码助手（如`GitHub Copilot`在某些高级功能上体现了`Agent`特性）。

## 规划与推理型Agent（Planning & Reasoning Agents）

- **特点：** 强调自主分解复杂任务、制定多步计划，并根据环境反馈进行调整的能力。它们通常需要更强的推理能力。

- **工作方式：** 常采用特定的思维框架，如`ReAct` (Reason+Act)，让模型先进行“思考”（Reasoning）分析当前情况和所需行动，然后执行“行动”（Action）调用工具，再根据工具返回结果进行下一轮思考。`Chain-of-Thought` (CoT) 等提示工程技术也是其推理的基础。

- **例子：** 需要整合网络搜索、计算器、数据库查询等多种工具来回答复杂问题的研究型Agent，或者能够自主完成“写一篇关于XX主题的报告，并配上相关数据图表”这类任务的Agent。

## 多Agent系统（Multi-Agent Systems）

- **特点：** 由多个具有不同角色或能力的`Agent`协同工作，共同完成一个更宏大的目标。

- **工作方式：** `Agent`之间可以进行通信、协作、辩论甚至竞争。例如，一个`Agent`负责规划，一个负责执行，一个负责审查。

- **例子：** 模拟软件开发团队（产品经理`Agent`、程序员`Agent`、测试员`Agent`）来自动生成和测试代码；模拟一个公司组织结构来完成商业策划。`AutoGen`、`ChatDev`等框架支持这类系统的构建。

## 探索与学习型Agent（Exploration & Learning Agents）

- **特点：** 这类`Agent`不仅执行任务，还能在与环境的交互中主动学习新知识、新技能或优化自身策略，类似于强化学习中的`Agent`概念。

- **工作方式：** 可能包含更复杂的记忆和反思机制，能够根据成功或失败的经验调整未来的规划和行动。

- **例子：** 能在未知软件环境中自主探索学习如何操作的`Agent`，或者在玩游戏时不断提升策略的`Agent`。

### 7.3.3 动手构造一个 Tiny-Agent

我们来基于 `openai` 库和其 `tool_calls` 功能，动手构造一个 `Tiny-Agent`，这个 `Agent` 是一个简单的任务导向型 `Agent`，它能够根据用户的输入，回答一些简单的问题。

最终的实现效果如图7.8所示：

![](img/f3482c01e0611f29622c942f6d22cfca.png)

#### Step 1 : 初始化客户端和模型

首先，我们需要一个能够调用大模型的客户端。这里我们使用 `openai` 库，并配置其指向一个兼容 `OpenAI` API 的服务终端，例如 [SiliconFlow](https://cloud.siliconflow.cn/i/ybUFvmqK)。同时，指定要使用的模型，如 `Qwen/Qwen2.5-32B-Instruct`。

```
from openai import OpenAI

# 初始化 OpenAI 客户端
client = OpenAI(
    api_key="YOUR_API_KEY",  # 替换为你的 API Key
    base_url="https://api.siliconflow.cn/v1", # 使用 SiliconFlow 的 API 地址
)

# 指定模型名称
model_name = "Qwen/Qwen2.5-32B-Instruct"
```

> **注意:** 你需要将 `YOUR_API_KEY` 替换为你从 [SiliconFlow](https://cloud.siliconflow.cn/i/ybUFvmqK) 或其他服务商获取的有效 API Key。

#### Step 2: 定义工具函数

我们在 `src/tools.py` 文件中定义 `Agent` 可以使用的工具函数。每个函数都需要有清晰的文档字符串（`docstring`），描述其功能和参数，因为这将用于自动生成工具的 JSON Schema。

```
# src/tools.py
from datetime import datetime

# 获取当前日期和时间
def get_current_datetime() -> str:
    """
    获取当前日期和时间。
    :return: 当前日期和时间的字符串表示。
    """
    current_datetime = datetime.now()
    formatted_datetime = current_datetime.strftime("%Y-%m-%d %H:%M:%S")
    return formatted_datetime

def count_letter_in_string(a: str, b: str):
    """
    统计字符串中某个字母的出现次数。
    :param a: 要搜索的字符串。
    :param b: 要统计的字母。
    :return: 字母在字符串中出现的次数。
    """
    return str(a.count(b))

def search_wikipedia(query: str) -> str:
    """
    在维基百科中搜索指定查询的前三个页面摘要。
    :param query: 要搜索的查询字符串。
    :return: 包含前三个页面摘要的字符串。
    """
    page_titles = wikipedia.search(query)
    summaries = []
    for page_title in page_titles[: 3]:  # 取前三个页面标题
        try:
            # 使用 wikipedia 模块的 page 函数，获取指定标题的维基百科页面对象。
            wiki_page = wikipedia.page(title=page_title, auto_suggest=False)
            # 获取页面摘要
            summaries.append(f"页面: {page_title}\n摘要: {wiki_page.summary}")
        except (
                wikipedia.exceptions.PageError,
                wikipedia.exceptions.DisambiguationError,
        ):
            pass
    if not summaries:
        return "维基百科没有搜索到合适的结果"
    return "\n\n".join(summaries)
# ... (可能还有其他工具函数)
```

为了让 `OpenAI` API 理解这些工具，我们需要将它们转换成特定的 JSON Schema 格式。这可以通过 `src/utils.py` 中的 `function_to_json` 辅助函数完成。

```
# src/utils.py (部分)
import inspect

def function_to_json(func) -> dict:
    # ... (函数实现细节)
    # 返回符合 OpenAI tool schema 的字典
    return {
        "type": "function",
        "function": {
            "name": func.__name__,
            "description": inspect.getdoc(func),
            "parameters": {
                "type": "object",
                "properties": parameters,
                "required": required,
            },
        },
    }
```

#### Step 3: 构造 Agent 类

我们在 `src/core.py` 文件中定义 `Agent` 类。这个类负责管理对话历史、调用 `OpenAI` API、处理工具调用请求以及执行工具函数。

```
# src/core.py (部分)
from openai import OpenAI
import json
from typing import List, Dict, Any
from utils import function_to_json
# 导入定义好的工具函数
from tools import get_current_datetime, add, compare, count_letter_in_string

SYSTEM_PROMPT = """
你是一个叫不要葱姜蒜的人工智能助手。你的输出应该与用户的语言保持一致。
当用户的问题需要调用工具时，你可以从提供的工具列表中调用适当的工具函数。
"""

class Agent:
    def __init__(self, client: OpenAI, model: str = "Qwen/Qwen2.5-32B-Instruct", tools: List=[], verbose : bool = True):
        self.client = client
        self.tools = tools
        self.model = model
        self.messages = [
            {"role": "system", "content": SYSREM_PROMPT},
        ]
        self.verbose = verbose

    def get_tool_schema(self) -> List[Dict[str, Any]]:
        # 获取所有工具的 JSON 模式
        return [function_to_json(tool) for tool in self.tools]

    def handle_tool_call(self, tool_call):
        # 处理工具调用
        function_name = tool_call.function.name
        function_args = tool_call.function.arguments
        function_id = tool_call.id

        function_call_content = eval(f"{function_name}(**{function_args})")

        return {
            "role": "tool",
            "content": function_call_content,
            "tool_call_id": function_id,
        }

    def get_completion(self, prompt) -> str:

        self.messages.append({"role": "user", "content": prompt})

        # 获取模型的完成响应
        response = self.client.chat.completions.create(
            model=self.model,
            messages=self.messages,
            tools=self.get_tool_schema(),
            stream=False,
        )
        
        # 检查模型是否调用了工具        
        if response.choices[0].message.tool_calls:
            self.messages.append({"role": "assistant", "content": response.choices[0].message.content})
            # 处理工具调用
            tool_list = []
            for tool_call in response.choices[0].message.tool_calls:
                # 处理工具调用并将结果添加到消息列表中
                self.messages.append(self.handle_tool_call(tool_call))
                tool_list.append([tool_call.function.name, tool_call.function.arguments])
            if self.verbose:
                print("调用工具：", response.choices[0].message.content, tool_list)
            # 再次获取模型的完成响应，这次包含工具调用的结果
            response = self.client.chat.completions.create(
                model=self.model,
                messages=self.messages,
                tools=self.get_tool_schema(),
                stream=False,
            )

        # 将模型的完成响应添加到消息列表中
        self.messages.append({"role": "assistant", "content": response.choices[0].message.content})
        return response.choices[0].message.content
```

`Agent` 的工作流程如下：

1. 接收用户输入。
2. 调用大模型（如 `Qwen`），并告知其可用的工具及其 Schema。
3. 如果模型决定调用工具，`Agent` 会解析请求，执行相应的 Python 函数。
4. `Agent` 将工具的执行结果返回给模型。
5. 模型根据工具结果生成最终回复。
6. `Agent` 将最终回复返回给用户。

如图7.9所示，`Agent` 调用工具流程：

![](img/baf8a77e378046fa8bfb9703630cf85e.png)

#### Step 4: 运行 Agent

现在我们可以实例化并运行 `Agent`。在 `demo.py` 的 `if __name__ == "__main__":` 部分提供了一个简单的命令行交互示例。
