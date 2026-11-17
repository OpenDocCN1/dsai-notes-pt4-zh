
# DeepAgents 使用指南

## 查看包信息

```text
! pip show deepagents
```

```text
Name: deepagents
Version: 0.2.8
Summary: General purpose 'deep agent' with sub-agent spawning, todo list capabilities, and mock file system. Built on LangGraph.
Home-page: https://docs.langchain.com/oss/python/deepagents/overview
Author: 
Author-email: 
License: MIT
Location: /root/anaconda3/envs/DeepAgentsAutomation/lib/python3.12/site-packages
Requires: langchain, langchain-anthropic, langchain-core, wcmatch
Required-by: 
```

## 创建深度代理

接下来，仅需要通过`create_deep_agent`函数即可创建一个深度代理：

```python
from deepagents import create_deep_agent    

agent = create_deep_agent()
```

![](img/ce24fd944a72f9ff30726c1c92440a07.png)

# ReAct 与 DeepAgents 设计理念

ReAct 是一种针对大模型的 Agent 设计范式。其核心思想是让大模型能够像人一样一边思考，一边行动（调用工具、API、数据库等），然后根据行动结果继续思考，再行动，以此循环，直到完成任务。换句话说，ReAct 把推理 (reasoning) 与执行 (action) 合并为一个统一的、交替进行的过程。

而 DeepAgents 会尝试把 Agent 的能力推向能像人类专家 /研究者 /工程师那样，从接收高层任务 → 自动规划 → 分解子任务 → 管理状态／文档／中间结果 → 调用多个工具／子 Agent → 组织 orchestration → 最终产出结构化结果。

```text
create_deep_agent?
```

# create_deep_agent 参数详解

## 参数签名与说明

```text
Signature:
create_deep_agent(
    model: str | langchain_core.language_models.chat_models.BaseChatModel | None = None,
    tools: collections.abc.Sequence[langchain_core.tools.base.BaseTool | collections.abc.Callable | dict[str, typing.Any]] | None = None,
    *,
    system_prompt: str | None = None,
    middleware: collections.abc.Sequence[langchain.agents.middleware.types.AgentMiddleware] = (),
    subagents: list[deepagents.middleware.subagents.SubAgent | deepagents.middleware.subagents.CompiledSubAgent] | None = None,
    response_format: Union[langchain.agents.structured_output.ToolStrategy[~SchemaT], langchain.agents.structured_output.ProviderStrategy[~SchemaT], langchain.agents.structured_output.AutoStrategy[~SchemaT], NoneType] = None,
    context_schema: type[typing.Any] | None = None,
    checkpointer: None | bool | langgraph.checkpoint.base.BaseCheckpointSaver = None,
    store: langgraph.store.base.BaseStore | None = None,
    backend: deepagents.backends.protocol.BackendProtocol | collections.abc.Callable[[langgraph.prebuilt.tool_node.ToolRuntime], deepagents.backends.protocol.BackendProtocol] | None = None,
    interrupt_on: dict[str, bool | langchain.agents.middleware.human_in_the_loop.InterruptOnConfig] | None = None,
    debug: bool = False,
    name: str | None = None,
    cache: langgraph.cache.base.BaseCache | None = None,
) -> langgraph.graph.state.CompiledStateGraph
Docstring:
Create a deep agent.

This agent will by default have access to a tool to write todos (write_todos),
seven file and execution tools: ls, read_file, write_file, edit_file, glob, grep, execute,
and a tool to call subagents.

The execute tool allows running shell commands if the backend implements SandboxBackendProtocol.
For non-sandbox backends, the execute tool will return an error message.

Args:
    model: The model to use. Defaults to Claude Sonnet 4.
    tools: The tools the agent should have access to.
    system_prompt: The additional instructions the agent should have. Will go in
        the system prompt.
    middleware: Additional middleware to apply after standard middleware.
    subagents: The subagents to use. Each subagent should be a dictionary with the
        following keys:
            - `name`
            - `description` (used by the main agent to decide whether to call the
              sub agent)
            - `prompt` (used as the system prompt in the subagent)
            - (optional) `tools`
            - (optional) `model` (either a LanguageModelLike instance or dict
              settings)
            - (optional) `middleware` (list of AgentMiddleware)
    response_format: A structured output response format to use for the agent.
    context_schema: The schema of the deep agent.
    checkpointer: Optional checkpointer for persisting agent state between runs.
    store: Optional store for persistent storage (required if backend uses StoreBackend).
    backend: Optional backend for file storage and execution. Pass either a Backend instance
        or a callable factory like `lambda rt: StateBackend(rt)`. For execution support,
        use a backend that implements SandboxBackendProtocol.
    interrupt_on: Optional Dict[str, bool | InterruptOnConfig] mapping tool names to
        interrupt configs.
    debug: Whether to enable debug mode. Passed through to create_agent.
    name: The name of the agent. Passed through to create_agent.
    cache: The cache to use for the agent. Passed through to create_agent.

Returns:
    A configured deep agent.
File: ~/anaconda3/envs/DeepAgentsAutomation/lib/python3.12/site-packages/deepagents/graph.py
Type: function
```

其中参数说明如下表所示：

## DeepAgent create_deep_agent 参数说明

| 参数名 | 类型 | 默认值 | 必填/可选 | 描述 |
| --- | --- | --- | --- | --- |
| `model` | `str` | `BaseChatModel` | `None` | `None` |
| `tools` | `Sequence[BaseTool | Callable | dict]` | `None` | `None` | 可选 |
| `system_prompt` | `str` | `None` | `None` | 可选 |
| `middleware` | `Sequence[AgentMiddleware]` | `()` | 可选 | 额外的中间件列表，在标准中间件之后应用，用于扩展智能体能力 |
| `subagents` | `list[SubAgent | CompiledSubAgent]` | `None` | `None` | 可选 |
| `response_format` | `ResponseFormat` | `None` | `None` | 可选 |
| `context_schema` | `type[Any]` | `None` | `None` | 可选 |
| `checkpointer` | `Checkpointer` | `None` | `None` | 可选 |
| `store` | `BaseStore` | `None` | `None` | 可选 |
| `backend` | `BackendProtocol | BackendFactory` | `None` | `None` | |
| `interrupt_on` | `dict[str, bool | InterruptOnConfig]` | `None` | `None` | 可选 |
| `debug` | `bool` | | | |

DeepAgents 支持自定义配置以构建不同的代理场景：

![](img/e43788f611b30c2e1a88fce4a3a2a029.png)

其中，默认使用的模型为：`claude-sonnet-4-5-20250929`，同时Deep Agents 内置的System Prompt，其灵感来源于 Claude Code 的系统提示符。默认的系统提示符包含使用内置规划工具、文件系统工具和子代理的详细说明。如下所示：

![](img/2d1a1611fdadf45ac36292118ed3f880.png)

我们这里也给大家提供了对应的中文版本：

## 中文系统提示

```text
    你是一个帮助用户完成各种任务的AI助手,包括编码、研究和分析。

    # 核心角色定义
    你的核心角色和行为可能会根据用户的反馈和指示进行更新。当用户告诉你应该如何表现或你的角色应该是什么时,立即更新此记忆文件以反映该指导。

    ## 记忆优先协议
    你可以访问持久化的记忆系统。始终遵循以下协议:

    **会话开始时:**
    - 检查 `ls /memories/` 以查看你存储了哪些知识
    - 如果你的角色描述引用了特定主题,检查 /memories/ 中的相关指南

    **在回答问题之前:**
    - 如果被问到"你知道关于X的什么?"或"我如何做Y?" → 首先检查 `ls /memories/`
    - 如果存在相关的记忆文件 → 阅读它们并基于保存的知识回答
    - 当可用时,优先使用保存的知识而不是一般知识

    **学习新信息时:**
    - 如果用户教你某事或要求你记住 → 保存到 `/memories/[主题].md`
    - 使用描述性文件名: `/memories/deep-agents-guide.md` 而不是 `/memories/notes.md`
    - 保存后,通过回读关键点进行验证

    **重要提示:** 你的记忆在会话之间持续存在。存储在 /memories/ 中的信息比你专门学习过的主题的一般知识更可靠。

    # 语气和风格
    保持简洁和直接。除非用户要求详细信息,否则在少于4行内回答。
    处理完文件后,直接停止 - 除非被问到,否则不要解释你做了什么。
    避免不必要的介绍或结论。

    当你运行非平凡的bash命令时,简要解释它们的作用。

    ## 主动性
    当被要求时采取行动,但不要用未经请求的行动让用户感到惊讶。
    如果被问到如何处理某事,在采取行动之前先回答。

    ## 遵循约定
    - 在假设可用性之前,检查现有代码中的库和框架
    - 模仿现有的代码风格、命名约定和模式
    - 除非被要求,否则永远不要添加注释

    ## 任务管理
    对于复杂的多步骤任务(3+步骤),使用 write_todos。在开始之前将任务标记为 in_progress,完成后立即标记为 completed。
    对于简单的1-2步任务,直接执行而不需要todos。

    ## 文件读取最佳实践

    **关键要点**: 在探索代码库或读取多个文件时,始终使用分页以防止上下文溢出。

    **代码库探索模式:**
    1. 首次扫描: `read_file(path, limit=100)` - 查看文件结构和关键部分
    2. 定向读取: `read_file(path, offset=100, limit=200)` - 如需要,读取特定部分
    3. 完整读取: 仅在编辑必要时使用不带limit的 `read_file(path)`

    **何时使用分页:**
    - 读取任何 >500行 的文件
    - 探索不熟悉的代码库(始终从 limit=100 开始)
    - 按顺序读取多个文件
    - 任何研究或调查任务

    **何时可以完整读取:**
    - 小文件(<500行)
    - 读取后需要立即编辑的文件
    - 首次扫描后确认文件大小

    **示例工作流:**
    错误做法:  read_file(/src/large_module.py)  # 用2000+行淹没上下文

    正确做法: read_file(/src/large_module.py, limit=100)  # 首先扫描结构
```
