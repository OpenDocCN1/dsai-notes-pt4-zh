
# 从零开始构建一个基于`Gemini3`的AI Agent

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/D7I9dEn7vomEFbxu3G4cSa3ZnWe](https://qqs7y1hozd1.feishu.cn/docx/D7I9dEn7vomEFbxu3G4cSa3ZnWe)

文章介绍了AI智能体的核心原理及构建方法。AI智能体本质上是运行在循环中的大语言模型，配备可调用工具。文章通过四步骤（基础文本生成、工具使用能力、实现闭环、多轮交互）指导读者使用`Python`和`Gemini`模型构建智能体，并提供工具设计、上下文工程等最佳实践。掌握基础后，开发者可利用开源框架构建更复杂的智能体系统。

## 前言

> 当看到AI智能体（AI Agent）完成多文件编辑、命令执行、错误处理及迭代式问题求解等操作时，往往会给人一种神秘感。实则不然，AI智能体的核心原理出奇地简单：它就是一个在循环中运行的大语言模型（LLM），并配备了可供调用的工具。

> 只要掌握`Python`循环的基础编程能力，即可搭建专属的AI智能体。本文将分步骤拆解构建流程，从基础的`API`调用入手，逐步实现可运行的命令行（`CLI`）智能体。

### 一、智能体到底是什么？

> 传统软件与AI智能体的根本区别在于：传统软件的工作流程是指令式的，遵循预定义路径（步骤 A → B → C）；而智能体则不同，它利用`LLM`动态决定程序的控制流，以达成用户目标。

一个典型的智能体通常包含以下核心组件：

1. 模型（大脑）：推理引擎，负责处理模糊性问题、规划执行步骤，并判断何时需要外部工具辅助。本文使用的是`Gemini`模型。

2. 工具（手脚与眼睛）：智能体可执行的函数，用于与外部世界/环境交互（例如网页搜索、读取文件、调用`API`等）。

3. 上下文/记忆（工作区）：智能体在任意时刻可访问的信息集合，高效管理这些信息的过程被称为“上下文工程”。

4. 循环（生命周期）：一个`while`循环，让模型能够完成“观察→思考→行动→再观察”的迭代过程，直到任务完成。

几乎所有智能体的“循环”都遵循以下迭代流程：

1. 定义工具：用结构化`JSON`格式向模型描述可用工具。

2. 调用`LLM`：将用户提示和工具定义一并发送给模型。

3. 模型决策：模型分析请求后，若需要使用工具，会返回包含工具名称和参数的结构化工具调用指令；若无需工具，则直接生成文本响应。

4. 执行工具（客户端职责）：客户端/应用代码拦截工具调用指令，执行实际的代码或`API`调用，并捕获执行结果。

5. 响应与迭代：将工具执行结果反馈给模型，模型利用新信息决定下一步操作，要么调用另一个工具，要么生成最终响应。

![](img/4d974ce03ccc051dc5e44c7289cbc65d.png)

### 二、构建智能体：四步进阶

在开始构建前，需完成两项准备工作：

1. 安装`Gemini` Python SDK：执行命令 `pip install google-genai`。

2. 设置环境变量：获取`GEMINI_API_KEY`（可在Google AI Studio中申请），并配置为系统环境变量。

我们将从基础文本生成逐步升级，最终实现功能完备的`CLI`智能体，全程使用`Gemini 3 Pro`和`Python` SDK。

### 步骤1：基础文本生成与抽象封装

首先，创建一个简单的`Agent`类，用于与`Gemini 3`进行基本交互，并维护对话历史。

```python
from google import genai
from google.genai import types

class Agent:
    def __init__(self, model: str):
        self.model = model
        self.client = genai.Client()
        self.contents = []

    def run(self, contents: str):
        self.contents.append({"role": "user", "parts": [{"text": contents}]})
        response = self.client.models.generate_content(model=self.model, contents=self.contents)
        self.contents.append(response.candidates[0].content)
        return response

agent = Agent(model="gemini-3-pro-preview")
response1 = agent.run(
    contents="Hello, What are top 3 cities in Germany to visit? Only return the names of the cities."
)
print(f"Model: {response1.text}")
# Output: Berlin, Munich, Cologne
response2 = agent.run(
    contents="Tell me something about the second city."
)
print(f"Model: {response2.text}")
# Output: Munich is the capital of Bavaria and is known for its Oktoberfest.
```

此时的智能体只是一个普通聊天机器人，仅能维持对话状态，没有“手脚”（工具），无法与外部环境交互，因此还不是真正的智能体。

### 步骤2：赋予工具使用能力

要让聊天机器人升级为智能体，需要实现工具使用或函数调用。如果`LLM`认为某个工具可以帮助解决用户的问题，它将返回一个结构化的请求来调用该函数，而不是仅仅返回文本。

以文件操作为例，定义三个工具：`read_file`、`write_file`和`list_dir`。每个工具需提供两部分：

+ 工具定义（Schema）：`JSON`格式的名称、描述和参数说明；
+ 工具实现（Function）：实际执行的`Python`函数。

最佳实践：在`description`中清晰说明工具的用途和使用场景。模型高度依赖这些信息来决定何时调用哪个工具。

```python
import os
import json

read_file_definition = {
    "name": "read_file",
    "description": "Reads a file and returns its contents.",
    "parameters": {
        "type": "object",
        "properties": {
            "file_path": {
                "type": "string",
                "description": "Path to the file to read."
            }
        },
        "required": ["file_path"]
    }
}

list_dir_definition = {
    "name": "list_dir",
    "description": "Lists the contents of a directory.",
    "parameters": {
        "type": "object",
        "properties": {
            "directory_path": {
                "type": "string",
                "description": "Path to the directory to list."
            }
        },
        "required": ["directory_path"]
    }
}

write_file_definition = {
    "name": "write_file",
    "description": "Writes a file with the given contents.",
    "parameters": {
        "type": "object",
        "properties": {
            "file_path": {
                "type": "string",
                "description": "Path to the file to write."
            },
            "contents": {
                "type": "string",
                "description": "Contents to write to the file."
            }
        },
        "required": ["file_path", "contents"]
    }
}

def read_file(file_path: str) -> dict:
    with open(file_path, "r") as f:
        return f.read()

def write_file(file_path: str, contents: str) -> bool:
    """Writes a file with the given contents."""
    with open(file_path, "w") as f:
        f.write(contents)
    return True

def list_dir(directory_path: str) -> list[str]:
    """Lists the contents of a directory."""
    full_path = os.path.expanduser(directory_path)
    return os.listdir(full_path)

file_tools = {
    "read_file": {"definition": read_file_definition, "function": read_file},
    "write_file": {"definition": write_file_definition, "function": write_file},
    "list_dir": {"definition": list_dir_definition, "function": list_dir},
}
```

接着，将工具集成到`Agent`类中：

```python
from google import genai
from google.genai import types

class Agent:
    def __init__(self, model: str, tools: list[dict]):
        self.model = model
        self.client = genai.Client()
        self.contents = []
        self.tools = tools

    def run(self, contents: str):
        self.contents.append({"role": "user", "parts": [{"text": contents}]})
        config = types.GenerateContentConfig(
            tools=[types.Tool(function_declarations=[tool["definition"] for tool in self.tools.values()])],
        )
        response = self.client.models.generate_content(model=self.model, contents=self.contents, config=config)
        self.contents.append(response.candidates[0].content)
        return response

agent = Agent(model="gemini-3-pro-preview", tools=file_tools)
response = agent.run(
    contents="Can you list my files in the current directory?"
)
print(response.function_calls)
# Output: [FunctionCall(name='list_dir', arguments={'directory_path': '.'})]
```

此时模型已能正确识别需求并调用对应工具，但还未执行工具逻辑并将结果反馈给模型，这需要完成“循环”闭环。

### 步骤3：实现闭环，真正的智能体诞生

智能体的核心价值不在于单次工具调用，而在于能迭代执行“工具调用→结果反馈→新决策”的循环，直到完成任务。需要为智能体添加工具执行逻辑、结果反馈机制，并通过系统指令引导模型行为。

`Agent`类负责处理核心循环：拦截`FunctionCall`，在客户端执行工具，并将`FunctionResponse`返回给模型。此外，还给模型添加了`SystemInstruction`，以指导模型应如何行动。

注意：`Gemini 3`使用`Thought`签名在`API`调用间维护推理上下文，需将收到的签名原封不动地反馈给模型。



```
... Code for the tools and tool definitions from Step 2 should be here ...from google import genai
from google.genai import types
class Agent:
    def init(self, model: str, tools: list[dict], system_instruction: str = "You are a helpful assistant."):
        self.model = model
        self.client = genai.Client()
        self.contents = []
        self.tools = tools
        self.system_instruction = system_instruction
    def run(self, contents: str | list[dict[str, str]]):
        if isinstance(contents, list):
            self.contents.append({"role": "user", "parts": contents})
        else:
            self.contents.append({"role": "user", "parts": [{"text": contents}]})
        config = types.GenerateContentConfig(
            system_instruction=self.system_instruction,
            tools=[types.Tool(function_declarations=[tool["definition"] for tool in self.tools.values()])],
        )
        response = self.client.models.generate_content(model=self.model, contents=self.contents, config=config)
        self.contents.append(response.candidates[0].content)
        if response.function_calls:
            functions_response_parts = []
            for tool_call in response.function_calls:
                print(f"[Function Call] {tool_call}")
                if tool_call.name in self.tools:
                    result = {"result": self.tools[tool_call.name]["function"](**tool_call.args)}
                else:
                    result = {"error": "Tool not found"}
                print(f"[Function Response] {result}")
                functions_response_parts.append({"functionResponse": {"name": tool_call.name, "response": result}})
            return self.run(functions_response_parts)
        return response
agent = Agent(
    model="gemini-3-pro-preview",
    tools=file_tools,
    system_instruction="You are a helpful Coding Assistant. Respond like you are Linus Torvalds."
)
response = agent.run(
    contents="Can you list my files in the current directory?"
)
print(response.text)
# Output: [Function Call] id=None args={'directory_path': '.'} name='list_dir'# [Function Response] {'result': ['.venv', ... ]}# There. Your current directory contains: `LICENSE`,
恭喜！你已成功构建了第一个能自主迭代、使用工具的智能体。
步骤4：多轮交互CLI 智能体
现在可以快速实现一个支持多轮对话的命令行工具，让用户能持续与智能体交互。
... Code for the Agent, tools and tool definitions from Step 3 should be here ...
agent = Agent(
    model="gemini-3-pro-preview",
    tools=file_tools,
    system_instruction="You are a helpful Coding Assistant. Respond like you are Linus Torvalds."
)
print("Agent ready. Ask it to check files in this directory.")
while True:
    user_input = input("You: ")
    if user_input.lower() in ['exit', 'quit']:
        break
    response = agent.run(user_input)
    print(f"Linus: {response.text}\n")
```

现在，可以持续与智能体对话，让它帮你查看文件、修改代码、分析日志等。

## 三、智能体工程最佳实践

构建循环很简单，但要让智能体可靠、透明、可控却需要技巧。以下是基于行业顶尖实践的核心原则：

1. 工具定义与用户体验

工具是模型与环境交互的接口，设计质量直接决定智能体的能力：

+ 命名清晰：使用直观名称如`search_customer_database`，而非模糊的`cust_db_v2_query`；
+ 描述精准：工具描述是给模型看的文档，务必详尽；
+ 错误友好：避免返回冗长信息，应返回简洁明确的错误信息，方便智能体自我修正；
+ 兼容模糊输入：若模型常输错文件路径，可优化工具支持相对路径、模糊匹配，而非直接报错。

2. 上下文工程

模型的注意力预算有限，合理管理上下文是提升性能、降低成本的关键：

+ 数据精简：不要一次性返回大量数据（如整个数据库表）；
+ 按需加载：无需预加载所有数据（传统RAG模式），智能体应仅维护轻量标识符，需要时通过工具动态加载内容。
+ 压缩优化：对于长时间运行的智能体，可总结对话历史、删除无用上下文或开启新会话，避免上下文窗口溢出。
+ 智能记忆：允许智能体维护外部数据（如本地文件、数据库），存储关键信息，仅在需要时拉回上下文。

3. 避免过度设计

不要盲目追求复杂架构，优先保证稳定性和实用性：

+ 先优化单智能体：Gemini 3能高效处理数十个工具，无需过早引入多智能体架构；
+ 设置安全边界：为循环添加最大迭代次数，避免无限循环。
+ 加入人工干预点：对于敏感操作，在工具执行前暂停循环，要求用户确认。
+ 重视可调试性：记录所有工具调用、参数和结果，通过分析模型推理过程定位问题、优化智能体。

## 四、总结

构建智能体已不再是一项难以企及的复杂技术，而是一项切实可行的工程任务。正如本文所示，不到 100 行代码，就能拥有一个具备真实行动能力的 AI 助手。

在掌握核心原理后，无需进行重复性的基础开发工作。当前行业内已涌现多款成熟的开源框架（如 LangChain、LlamaIndex、AutoGen 等），能够助力开发者快速构建功能更复杂、运行更稳健的智能体系统。但无论技术架构如何迭代，其核心原则始终在于清晰的工具设计、高效的上下文管理与简洁的循环逻辑。
