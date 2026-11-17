
# Google ADK Agent 与 Session 快速入门

## Agent 初始化签名

```
Init signature:
Agent(
    *,
    name: str,
    description: str = '',
    parent_agent: Optional[google.adk.agents.base_agent.BaseAgent] = None,
    sub_agents: list[google.adk.agents.base_agent.BaseAgent] = <factory>,
    before_agent_callback: Optional[Callable[[google.adk.agents.callback_context.CallbackContext], Optional[google.genai.types.Content]]] = None,
    after_agent_callback: Optional[Callable[[google.adk.agents.callback_context.CallbackContext], Optional[google.genai.types.Content]]] = None,
    model: Union[str, google.adk.models.base_llm.BaseLlm] = '',
    instruction: Union[str, Callable[[google.adk.agents.readonly_context.ReadonlyContext], str]] = '',
    global_instruction: Union[str, Callable[[google.adk.agents.readonly_context.ReadonlyContext], str]] = '',
    tools: list[typing.Union[typing.Callable, google.adk.tools.base_tool.BaseTool]] = <factory>,
    generate_content_config: Optional[google.genai.types.GenerateContentConfig] = None,
    disallow_transfer_to_parent: bool = False,
    disallow_transfer_to_peers: bool = False,
    include_contents: Literal['default', 'none'] = 'default',
    input_schema: Optional[type[pydantic.main.BaseModel]] = None,
    output_schema: Optional[type[pydantic.main.BaseModel]] = None,
    output_key: Optional[str] = None,
    planner: Optional[google.adk.planners.base_planner.BasePlanner] = None,
    code_executor: Optional[google.adk.code_executors.base_code_executor.BaseCodeExecutor] = None,
    examples: Union[list[google.adk.examples.example.Example], google.adk.examples.base_example_provider.BaseExampleProvider, NoneType] = None,
    before_model_callback: Optional[Callable[[google.adk.agents.callback_context.CallbackContext, google.adk.models.llm_request.LlmRequest], Optional[google.adk.models.llm_response.LlmResponse]]] = None,
    after_model_callback: Optional[Callable[[google.adk.agents.callback_context.CallbackContext, google.adk.models.llm_response.LlmResponse], Optional[google.adk.models.llm_response.LlmResponse]]] = None,
    before_tool_callback: Optional[Callable[[google.adk.tools.base_tool.BaseTool, dict[str, Any], google.adk.tools.tool_context.ToolContext], Optional[dict]]] = None,
    after_tool_callback: Optional[Callable[[google.adk.tools.base_tool.BaseTool, dict[str, Any], google.adk.tools.tool_context.ToolContext, dict], Optional[dict]]] = None,
) -> None
Docstring:      LLM-based Agent.
Init docstring:
Create a new model by parsing and validating input data from keyword arguments.

Raises [`ValidationError`][pydantic_core.ValidationError] if the input data cannot be
validated to form a valid model.

`self` is explicitly positional-only to allow `self` as a field name.
File:           c:\programdata\anaconda3\envs\jupyter_env\lib\site-packages\google\adk\agents\llm_agent.py
Type:           ModelMetaclass
Subclasses:     
```

其核心参数解释如下：

| 参数名 | 类型 | 说明 | 中文解释 |
| --- | --- | --- | --- |
| `name` | `str` | `Agent`的名称，必须唯一 | 代理的名字（必填） |
| `description` | `str` | `Agent`的简短描述，通常在多`Agent`系统中用于路由/选择 | 代理的简短介绍 |
| `parent_agent` | `BaseAgent` or `None` | 父`Agent`（如果这个`Agent`是子代理的话） | 父代理，默认无 |
| `sub_agents` | `list[BaseAgent]` | 这个`Agent`下面挂载的子`Agent`们 | 子代理列表 |
| `before_agent_callback` | `Callable` or `None` | 在`Agent`处理输入前，进行预处理的回调 | 处理前的自定义函数 |
| `after_agent_callback` | `Callable` or `None` | 在`Agent`完成任务后，进行后处理的回调 | 处理后的自定义函数 |
| `model` | `str` or `BaseLlm` | 关联的模型，可以是字符串名或模型实例（如`LiteLlm`） | 使用的模型（必填） |
| `instruction` | `str` or `Callable` | 给`Agent`的详细指令，可以是固定文本，也可以是函数动态生成 | 指令说明（必填或推荐填写） |
| `global_instruction` | `str` or `Callable` | 在整个`Agent`树范围共享的指令（全局指令） | 全局指令（少用） |
| `tools` | `list[Callable or BaseTool]` | 这个`Agent`可以调用的外部工具列表 | 工具列表 |
| `generate_content_config` | `GenerateContentConfig` or `None` | 控制模型生成内容时的一些配置，比如最大`Token`数、温度等 | 生成设置（选填） |
| `disallow_transfer_to_parent` | `bool` | 禁止把任务转给父`Agent`（防止死循环） | 禁止转移到父代理 |
| `disallow_transfer_to_peers` | `bool` | 禁止把任务转给同级`Agent` | 禁止转移到同级代理 |
| `include_contents` | `'default'` or `'none'` | 是否在提示中包含历史对话内容 | 是否包含上下文 |
| `input_schema` | `pydantic.BaseModel` or `None` | 限定输入必须符合某种结构（`Pydantic`模型） | 输入校验模型 |
| `output_schema` | `pydantic.BaseModel` or `None` | 限定输出必须符合某种结构（`Pydantic`模型） | 输出校验模型 |
| `output_key` | `str` or `None` | 指定模型输出结果中的哪个字段作为最终输出 | 输出字段名 |
| `planner` | `BasePlanner` or `None` | 自定义计划器（比如动态规划步骤） | 规划器（进阶功能） |
| `code_executor` | `BaseCodeExecutor` or `None` | 允许`Agent`执行代码（如`Python`沙盒环境） | 代码执行器（进阶功能） |
| `examples` | `list[Example]` or `BaseExampleProvider` or `None` | 示例对话或行为示范，帮助模型更好完成任务 | 示例数据 |
| `before_model_callback` | `Callable` or `None` | 在发送给模型推理前，修改请求内容 | 模型请求前回调 |
| `after_model_callback` | `Callable` or `None` | 在收到模型响应后，修改或处理响应 | 模型响应后回调 |
| `before_tool_callback` | `Callable` or `None` | 在调用工具前修改参数或做拦截 | 工具调用前处理 |
| `after_tool_callback` | `Callable` or `None` | 在工具返回结果后做处理 | 工具调用后处理 |

其中四个核心参数：

+   `model` 是必须指定的（不指定`Agent`就不会工作）

+   `instruction` 和 `description` 是推荐填写的（帮助大模型更好理解自己要干什么）

+   `tools` 是选填的（如果需要调用外部`API`、`数据库`等，就注册工具）

+   `sub_agents` 是选填的（需要多`Agent`协作时再用）

一个极简的`Agent`对象创建方法如下：

```
agent = Agent(
    name="simple_agent",
    model=model,
    instruction="你是一个乐于助人的中文助手。",
    description="回答用户的问题。"
)
```

## Step 3. 创建一个 Session

紧接着创建一个`Session`对象，用于保存`Agent`运行过程中的多轮对话内容。ADK提供了一个`InMemorySessionService`类，其核心作用如下：

+   保存每轮用户输入（user messages）

+   保存每轮模型输出（assistant messages）

+   保存工具调用过程中的中间数据（如果有工具的话）

+   提供历史对话上下文，让模型能基于完整对话继续推理，而不是每次都从空白开始

```
InMemorySessionService
```

```
Init signature: InMemorySessionService()
Docstring:      An in-memory implementation of the session service.
File:           c:\programdata\anaconda3\envs\jupyter_env\lib\site-packages\google\adk\sessions\in_memory_session_service.py
Type:           ABCMeta
Subclasses:     
```

在前期测试时，我们可以创建一个`InMemorySessionService()`实例：

```
session_service = InMemorySessionService()
```

这个对话管理示例实际会在内存中进行历史会话信息管理。而具体的会话信息会保存在`session_service.sessions`属性里，默认为空：

```
session_service.sessions
```

```
{}
```

在正式开启对话之前，我们需要创建一个初始session对象，为了便于区分不同的会话，该对象需要设置APP名称、用户ID和对话ID，方便后续进行查找和调用：

```
APP_NAME = "test_app"
USER_ID = "user_1"
SESSION_ID = "session_001"
```

```
session = session_service.create_session(
    app_name=APP_NAME,
    user_id=USER_ID,
    session_id=SESSION_ID
)
print(f"Session created: App='{APP_NAME}', User='{USER_ID}', Session='{SESSION_ID}'")
```

```
Session created: App='test_app', User='user_1', Session='session_001'
```

需要注意的是，执行完上面的代码后，我们就获得了一个包含APP名称、用户ID和对话ID的session对象：

```
session
```

```
Session(id='session_001', app_name='test_app', user_id='user_1', state={}, events=[], last_update_time=1745764835.901281)
```

```
session.app_name
```

```
'test_app'
```

```
session.user_id
```

```
'user_1'
```

```
session.id
```

```
'session_001'
```

目前session对象是空的，而在后续执行过程中，伴随着上下文生成器存储了更多的会话信息，我们也可以将更多的上下文信息保存在session中。

不过这里需要注意的是，在创建session对象的同时，对话生成器`session_service`的`.sessions`也发生了变化，其内容与session基本一致：

```
session_service.sessions
```

```
{'test_app': {'user_1': {'session_001': Session(id='session_001', app_name='test_app', user_id='user_1', state={}, events=[], last_update_time=1745764835.901281)}}}
```

而需要注意的是，在后续对话中，我们实际上是带入`session_service`进行对话，并读取`session_service.sessions`里面的历史对话信息作为上下文。这里可以将单独创建的`session`对象视作一个中间对象或者一个备份。

## Step 4. 创建智能体执行器（Runner）
