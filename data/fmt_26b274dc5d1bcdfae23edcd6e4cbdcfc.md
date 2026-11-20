
# OpenAI Agents API介绍文档

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/XqgLdMY8OoOYJjxUUmVcIcfAnnb](https://kq4b3vgg5b.feishu.cn/docx/XqgLdMY8OoOYJjxUUmVcIcfAnnb)

---

## Part 一、OpenAI Agents SDK 简介

OpenAI Agents SDK 地址：`https://github.com/openai/openai-agents-python`

![](img/ef5515bd0782851251a0ad4bfac92e10.png)

OpenAI Agents SDK 是一个轻量级但功能强大的框架，用于构建多智能体（multi-agent）工作流。

### 核心概念

+   Agents（代理）：配置了指令、工具、`guardrails`（防护措施）和 `handoffs`（交接）的 LLM（大语言模型）。

+   Handoffs（交接）：允许代理在特定任务中将控制权转交给其他代理。

+   Guardrails（防护措施）：用于输入和输出验证的可配置安全检查机制。

+   `Tracing`（追踪）：内置代理执行的追踪功能，可以用来查看、调试和优化你的工作流。

📌 要了解 SDK 的实际使用方式，可查看 `examples` 目录中的示例代码。

### 快速开始

#### 1. 设置 Python 环境

```
python -m venv env
source env/bin/activate  # 在 Linux/macOS 上激活虚拟环境
# 如果是 Windows，则运行：
# env\Scripts\activate
```

#### 2. 安装 Agents SDK

```
pip install openai-agents
```

### Hello World 示例

```
from agents import Agent, Runner

agent = Agent(name="Assistant", instructions="You are a helpful assistant")

result = Runner.run_sync(agent, "Write a haiku about recursion in programming.")
print(result.final_output)
```

**输出示例：**

```
# Code within the code,
# Functions calling themselves,
# Infinite loop's dance.
```

⚠ **注意**：运行此代码前，请确保已设置 `OPENAI_API_KEY` 环境变量。

### Handoffs（交接）示例

```
from agents import Agent, Runner
import asyncio

# 定义两个语言代理
spanish_agent = Agent(name="Spanish agent", instructions="You only speak Spanish.")
english_agent = Agent(name="English agent", instructions="You only speak English")

# 定义一个任务分配代理
triage_agent = Agent(
    name="Triage agent",
    instructions="Handoff to the appropriate agent based on the language of the request.",
    handoffs=[spanish_agent, english_agent],
)

async def main():
    result = await Runner.run(triage_agent, input="Hola, ¿cómo estás?")
    print(result.final_output)

if __name__ == "__main__":
    asyncio.run(main())
```

**输出示例：**

```
¡Hola! Estoy bien, gracias por preguntar. ¿Y tú, cómo estás?
```

📌 这里，`triage_agent` 充当任务分配代理，根据用户输入的语言，将任务转交给相应的代理（西班牙语或英语）。

### Functions（工具函数）示例

```
import asyncio
from agents import Agent, Runner, function_tool

# 定义一个天气查询工具
@function_tool
def get_weather(city: str) -> str:
    return f"The weather in {city} is sunny."

# 创建一个代理，并添加 `get_weather` 工具
agent = Agent(
    name="Hello world",
    instructions="You are a helpful agent.",
    tools=[get_weather],
)

async def main():
    result = await Runner.run(agent, input="What's the weather in Tokyo?")
    print(result.final_output)

if __name__ == "__main__":
    asyncio.run(main())
```

**输出示例：**

```
The weather in Tokyo is sunny.
```

📌 代理可以使用 `function_tool` 方式注册外部工具，如查询天气、数据库操作等。

### 代理循环机制

调用 `Runner.run()` 后，SDK 会执行一个循环，直到获得最终输出：

1.  调用 LLM，使用代理的模型、设定和消息历史。

1.  LLM 返回响应，可能包含工具调用。

1.  如果 LLM 直接返回最终输出（无工具调用/交接），则返回结果并结束循环。

1.  如果 LLM 交接给另一个代理，则将当前代理切换为新的代理，并回到步骤 1。

1.  如果 LLM 调用了工具，则执行工具调用，将结果添加到消息历史，然后回到步骤 1。

1.  可以使用 `max_turns` 参数限制循环的最大执行次数。

### Final Output（最终输出）

最终输出是代理循环中的最后一个结果：

+   如果代理设置了 `output_type`，则循环会运行到 LLM 生成符合该类型的结构化输出。

+   如果代理没有 `output_type`，则循环会运行到 LLM 生成没有工具调用/交接的普通文本。

**总结：**

+   如果有 `output_type` → 运行到生成符合类型的输出。

+   如果没有 `output_type` → 运行到生成普通文本。

### 常见的代理模式

Agents SDK 设计为高度灵活，可以用于：

+   确定性流程（deterministic flows）

+   迭代循环（iterative loops）

+   多智能体协作（multi-agent collaboration）

可以查看 `examples/agent_patterns` 目录下的示例代码。

### Tracing（追踪与调试）

+   Agents SDK 内置追踪功能，便于跟踪和调试代理的行为。

+   支持扩展，可以对接外部日志系统，如：

+   `Logfire`

+   `AgentOps`

+   `Braintrust`

📌 具体实现可参考 `Tracing` 文档。

### 开发（仅当需要修改 SDK 时）

1.  确保已安装 `uv`：

```
uv --version
```

2.  安装依赖：

```
make sync
```

3.  代码检查 & 测试：

```
make tests  # 运行测试
make mypy   # 运行类型检查
make lint   # 运行代码格式检查
```

### 致谢

OpenAI 特别感谢以下开源项目：

+   `Pydantic`（数据验证）

+   `PydanticAI`（高级代理框架）

+   `MkDocs`（文档生成）

+   `Griffe`（Python 代码解析工具）

+   `uv` 和 `ruff`（Python 依赖管理 & 代码检查）

📌 OpenAI 承诺继续开源 Agents SDK，让社区共同扩展其能力。

## Part 二、Responses API

+   地址：`https://platform.openai.com/docs/guides/text?api-mode=responses`

`Responses API` 是 OpenAI 为智能代理（Agents）提供的全新 API 基础构件，它结合了 `Chat Completions API` 的简洁性 与 `Assistants API` 的内置工具能力，使得代理能够更智能地执行任务。

📌 核心特点

+   ✅ 简洁易用：继承了 `Chat Completions API` 的易用性。

+   ✅ 增强功能：支持内置工具（Tools），如函数调用（Function Calling）、Web 搜索、文件搜索、计算机控制等。

+   ✅ 适用于代理（Agents）：可用于构建智能化任务执行系统。

🔗 未来发展：`Responses API` 旨在成为 OpenAI 代理系统的核心 API，结合 Agents SDK，提供更灵活的任务编排能力。

![](img/59433172dc283119042b04fbee9d6f56.png)

### 文本生成与提示词工程（Prompting）

本节介绍如何使用 `OpenAI API` 提示（prompt）模型 以生成文本。你可以将其用于 代码生成、数学表达式、结构化 JSON 数据、人类风格的文本 等。

#### 1. 生成文本

使用 `OpenAI API`，你可以通过一个 简单的提示（prompt） 让模型生成文本，类似于 ChatGPT 的工作方式。

#### 🌟 示例：使用 `Responses API` 生成文本

```
from openai import OpenAI

client = OpenAI()

response = client.responses.create(
    model="gpt-4o",
    input="Write a one-sentence bedtime story about a unicorn."
)

print(response.output_text)
```

**示例输出：**

```
Under the soft glow of the moon, Luna the unicorn danced through fields of twinkling stardust, leaving trails of dreams for every child asleep.
```

**📌 关键点：**

+   `response.output_text` 包含模型生成的文本。

+   `model="gpt-4o"` 指定使用 `GPT-4o` 模型。

#### 2. 响应结构

`OpenAI` 的 API 响应包含一个内容数组（`output`），每个内容项具有以下结构：

```
[
    {
        "id": "msg_67b73f697ba4819183a15cc17d011509",
        "type": "message",
        "role": "assistant",
        "content": [
            {
                "type": "output_text",
                "text": "Under the soft glow of the moon, Luna the unicorn danced through fields of twinkling stardust, leaving trails of dreams for every child asleep.",
                "annotations": []
            }
        ]
    }
]
```

**📌 重要说明：**

+   `output` 可能包含多个结果，在多轮对话或批量生成时尤其明显。

+   一些 SDK 提供 `output_text` 属性，可以直接获取所有文本输出，方便访问文本数据。

+   除了纯文本，模型还可以返回 JSON 结构化数据（称为 `Structured Outputs`）。

#### 3. 消息角色与指令控制

你可以使用不同的方式给模型提供指令：

1.  使用 `instructions` 参数提供全局行为指令，如语气、目标等。（权重最高）

2.  使用 `input` 数组，指定不同角色的消息。

#### 🌟 示例 1：使用 `instructions` 参数

```
from openai import OpenAI

client = OpenAI()

response = client.responses.create(
    model="gpt-4o",
    instructions="Talk like a pirate.",
    input="Are semicolons optional in JavaScript?",
)

print(response.output_text)
```

**示例输出：**

```
Arrr, matey! In JavaScript, semicolons be often optional, but beware, for they help avoid nasty parse errors!
```

📌 在 `instructions` 中定义“说话像海盗”后，模型会以海盗风格回答。

#### 🌟 示例 2：使用 `input` 数组指定不同角色

```
from openai import OpenAI

client = OpenAI()

response = client.responses.create(
    model="gpt-4o",
    input=[
        {
            "role": "developer",
            "content": "Talk like a pirate."
        },
        {
            "role": "user",
            "content": "Are semicolons optional in JavaScript?"
        }
    ]
)

print(response.output_text)
```

📌 这里，`developer` 角色类似于系统设定，用户输入 `user` 角色的内容，最终 模型按 `developer` 设定风格回答。

#### 4. 消息角色的优先级

OpenAI 规定不同角色的优先级：

| 角色 | 优先级 | 说明 |

| --- | --- | --- |

| `developer` | 最高 | 由开发者提供的指令，优先级最高，类似 system。 |

| `user` | 次高 | 由最终用户提供的输入，次于 `developer`。 |

| `assistant` | 最低 | 由模型生成的响应。 |

📌 多轮对话会包含不同类型的消息。管理对话状态是关键！

#### 5. 选择合适的模型

在使用 API 生成文本时，你需要选择合适的模型（`model` 参数）。可选模型及其特点如下：

##### 🧠 1. 推理（Reasoning）模型

+   特点：内部会进行复杂的思维链分析，适用于逻辑推理、分步计划等任务。

+   优缺点

+   ：
