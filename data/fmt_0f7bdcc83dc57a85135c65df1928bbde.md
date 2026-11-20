
# Agents SDK 核心功能与使用

```
MessageOutputItem(agent=Agent(name='天气查询Agent', instructions='你是一名助人为乐的助手，并且可以进行天气信息查询', handoff_description=None, handoffs=[], model=<agents.models.openai_chatcompletions.OpenAIChatCompletionsModel object at 0x0000015531F87F50>, model_settings=ModelSettings(temperature=None, top_p=None, frequency_penalty=None, presence_penalty=None, tool_choice=None, parallel_tool_calls=False, truncation=None, max_tokens=None), tools=[FunctionTool(name='get_weather', description='查询即时天气函数', params_json_schema={'properties': {'loc': {'description': "必要参数，字符串类型，用于表示查询天气的具体城市名称，    注意，中国的城市需要用对应城市的英文名称代替，例如如果需要查询北京市天气，则loc参数需要输入'Beijing';", 'title': 'Loc'}, 'required': ['loc'], 'title': 'get_weather_args', 'type': 'object', 'additionalProperties': False}, on_invoke_tool=<function function_tool.<locals>._create_function_tool.<locals>._on_invoke_tool at 0x000001553492CAE0>, strict_json_schema=True)], mcp_servers=[], input_guardrails=[], output_guardrails=[], output_type=None, hooks=None, tool_use_behavior='run_llm_again', reset_tool_choice=True), raw_item=ResponseOutputMessage(id='__fake_id__', content=[ResponseOutputText(annotations=[], text='今天北京的天气是多云，阴天。当前温度为20.94°C，体感温度约为19.32°C。湿度较低，为9%，风速为7.26米/秒，阵风可达11.1米/秒。能见度为10000米。\n\n如果需要更详细的天气信息或其他帮助，请告诉我！', type='output_text')], role='assistant', status='completed', type='message'), type='message_output_item')
```

也就是模型最终回复：

```
weather_result.new_items[2].raw_item
```

```
ResponseOutputMessage(id='__fake_id__', content=[ResponseOutputText(annotations=[], text='今天北京的天气是多云，阴天。当前温度为20.94°C，体感温度约为19.32°C。湿度较低，为9%，风速为7.26米/秒，阵风可达11.1米/秒。能见度为10000米。\n\n如果需要更详细的天气信息或其他帮助，请告诉我！', type='output_text')], role='assistant', status='completed', type='message')
```

也就是说，在调用`Agents SDK`时，我们只需要在`tools`参数位上输入对应的函数，就能自动执行`Function calling`流程。

![](img/2a061bd09cc51fc910fe80d38061a643.png)

## 3. Agents SDK Runner loop循环执行功能介绍

而之所以`Agents SDK`能够高效快速执行`Function calling`，得益于`Runner`的循环执行机制：

```
Runner.run?
```

```
Signature:
Runner.run(
    starting_agent: 'Agent[TContext]',
    input: 'str | list[TResponseInputItem]',
    *,
    context: 'TContext | None' = None,
    max_turns: 'int' = 10,
    hooks: 'RunHooks[TContext] | None' = None,
    run_config: 'RunConfig | None' = None,
) -> 'RunResult'
Docstring:
Run a workflow starting at the given agent. The agent will run in a loop until a final
output is generated. The loop runs like so:
1. The agent is invoked with the given input.
2. If there is a final output (i.e. the agent produces something of type
    `agent.output_type`, the loop terminates.
3. If there's a handoff, we run the loop again, with the new agent.
4. Else, we run tool calls (if any), and re-run the loop.

In two cases, the agent may raise an exception:
1. If the max_turns is exceeded, a MaxTurnsExceeded exception is raised.
2. If a guardrail tripwire is triggered, a GuardrailTripwireTriggered exception is raised.

Note that only the first agent's input guardrails are run.

Args:
    starting_agent: The starting agent to run.
    input: The initial input to the agent. You can pass a single string for a user message,
        or a list of input items.
    context: The context to run the agent with.
    max_turns: The maximum number of turns to run the agent for. A turn is defined as one
        AI invocation (including any tool calls that might occur).
    hooks: An object that receives callbacks on various lifecycle events.
    run_config: Global settings for the entire agent run.

Returns:
    A run result containing all the inputs, guardrail results and the output of the last
    agent. Agents may perform handoffs, so we don't know the specific type of the output.
File: c:\programdata\anaconda3\envs\jupyter_env\lib\site-packages\agents\run.py
Type: method
```

解释如下：

从指定的起始 `Agent` 开始运行一个工作流。`Agent` 会在一个循环中运行，直到生成最终输出为止。

循环流程如下：

1. 使用提供的输入调用 `Agent`。
2. 如果产生了“最终输出”（例如返回了 `agent.output_type` 类型的内容），循环终止。
3. 如果产生了 handoff（交接），则使用新的 `Agent` 重新进入循环。
4. 否则，会执行工具调用（`tool calls`，如有），然后重新进入循环。

在以下两种情况下，`Agent` 会抛出异常：

1. 如果超过了最大轮数 `max_turns`，则会抛出 `MaxTurnsExceeded` 异常。
2. 如果触发了 `Guardrail` 的“绊线”机制（`tripwire`），则会抛出 `GuardrailTripwireTriggered` 异常。

⚠️ 注意：只有`第一个 Agent`的输入会被检查 `Guardrails`（安全检查机制）。

📌 参数说明：

| 参数名 | 类型 | 说明 |
| --- | --- | --- |
| `starting_agent` | `Agent[TContext]` | 运行起点的 `Agent`（智能体） |
| `input` | `str` 或 `list[TResponseInputItem]` | `Agent` 的初始输入。你可以传入一个字符串（如用户消息），也可以传入一个输入项的列表。 |
| `context` | `TContext` 或 `None` | 要与 `Agent` 一起运行的上下文（比如用户资料、状态等） |
| `max_turns` | `int` | `Agent` 最多运行的轮数（默认是 10）。每次 AI 响应（包括工具调用）都算一轮。 |
| `hooks` | `RunHooks[TContext]` 或 `None` | 生命周期钩子对象。你可以通过它注册回调函数，监听 `Agent` 的各种事件（如每轮开始、每次 `tool` 调用等）。 |
| `run_config` | `RunConfig` 或 `None` | 对整个 `Agent` 运行过程的全局配置选项。 |

而理解该循环，也是用好`Agents SDK`的第一步。该循环可以用下图进行展示：

![](img/61d933e857e58ba2097f38aa10e3d6cd.png)

而为了避免陷入死循环，我们可以使用`max_turns`进行控制。例如我们创建一个错误的外部函数如下：

```
@function_tool
def test_write_file(content):
    """
    将指定内容写入本地文件。
    :param content: 必要参数，字符串类型，用于表示需要写入文档的具体内容。
    :return：是否成功写入
    """
    
    raise RuntimeError("由于格式问题，无法写入本地文档，请调整格式后重新尝试。")
```

```
test_agent = Agent(
    name="写入本地文件Agent",
    instructions="你是一名助人为乐的助手，并且将部分文档写入本地",
    tools=[test_write_file],
    model=deepseek_model
)
```

然后即可测试调用结果

```
test_result = await Runner.run(test_agent, input="请将《DeepSeek智能体开发实战》标题写入本地文档")
```

```
test_result.final_output
```

```
'抱歉，多次尝试后仍然无法成功写入文档。可能是由于系统限制或其他原因导致无法完成操作。如果您有其他需求或需要进一步帮助，请告诉我！'
```

而此时模型总共尝试了三次：

```
len(test_result.new_items)
```

```
9
```

```
test_result.new_items[0].raw_item
```

```
ResponseFunctionToolCall(arguments='{"content":"DeepSeek智能体开发实战"}', call_id='call_0_a2cca4f5-607d-4c18-94e6-5b7843ca897f', name='test_write_file', type='function_call', id='__fake_id__', status=None)
```

```
test_result.new_items[1].raw_item
```

```
{'call_id': 'call_0_a2cca4f5-607d-4c18-94e6-5b7843ca897f',
 'output': 'An error occurred while running the tool. Please try again. Error: 由于格式问题，无法写入本地文档，请调整格式后重新尝试。',
 'type': 'function_call_output'}
```

```
test_result.new_items[2].raw_item
```

```
ResponseOutputMessage(id='__fake_id__', content=[ResponseOutputText(annotations=[], text='看起来写入文档时遇到了格式问题，我尝试调整格式后重新写入。', type='output_text')], role='assistant', status='completed', type='message')
```

```
test_result.new_items[3].raw_item
```

```
ResponseFunctionToolCall(arguments='{"content":"《DeepSeek智能体开发实战》"}', call_id='call_0_16187d51-f39d-4383-8318-025c62257d3b', name='test_write_file', type='function_call', id='__fake_id__', status=None)
```

```
test_result.new_items[4].raw_item
```

```
{'call_id': 'call_0_16187d51-f39d-4383-8318-025c62257d3b',
 'output': 'An error occurred while running the tool. Please try again. Error: 由于格式问题，无法写入本地文档，请调整格式后重新尝试。',
 'type': 'function_call_output'}
```

> 此外，需要注意的是，`DeepSeek-V3-0324`模型有自带的避免`Function calling`出现死循环的功能，最多调用若干次无法返回正确结果后，将会停止继续调用外部函数。

## 4. Agents SDK接入更多自定义工具

在此前的公开课中，我们曾多次介绍过`Function calling`中创建自定义函数并进行调用。这里我们继续尝试创建`python_inter`函数来让`Agents`接入`Python`环境，以及创建`sql_inter`来谅解本地`MySQL`环境。

+ `python_inter`函数创建与使用
