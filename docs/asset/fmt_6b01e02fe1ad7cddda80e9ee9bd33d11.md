
```
Type:            Agent
String form:     name='mini-Mate' model='deepseek-chat' instructions='You are a helpful agent.' functions=[] tool_choice=None parallel_tool_calls=True
File:            ~/miniconda3/lib/python3.12/site-packages/swarm/types.py
Docstring:       <no docstring>
Class docstring:
Usage docs: https://docs.pydantic.dev/2.10/concepts/models/

A base class for creating Pydantic models.

Attributes:
    __class_vars__: The names of the class variables defined on the model.
    __private_attributes__: Metadata about the private attributes of the model.
    __signature__: The synthesized `__init__` [`Signature`][inspect.Signature] of the model.

    __pydantic_complete__: Whether model building is completed, or if there are still undefined fields.
    __pydantic_core_schema__: The core schema of the model.
    __pydantic_custom_init__: Whether the model has a custom `__init__` function.
    __pydantic_decorators__: Metadata containing the decorators defined on the model.
        This replaces `Model.__validators__` and `Model.__root_validators__` from Pydantic V1.
    __pydantic_generic_metadata__: Metadata for generic models; contains data used for a similar purpose to
        __args__, __origin__, __parameters__ in typing-module generics. May eventually be replaced by these.
    __pydantic_parent_namespace__: Parent namespace of the model, used for automatic rebuilding of models.
    __pydantic_post_init__: The name of the post-init method for the model, if defined.
    __pydantic_root_model__: Whether the model is a [`RootModel`][pydantic.root_model.RootModel].
    __pydantic_serializer__: The `pydantic-core` `SchemaSerializer` used to dump instances of the model.
    __pydantic_validator__: The `pydantic-core` `SchemaValidator` used to validate instances of the model.

    __pydantic_fields__: A dictionary of field names and their corresponding [`FieldInfo`][pydantic.fields.FieldInfo] objects.
    __pydantic_computed_fields__: A dictionary of computed field names and their corresponding [`ComputedFieldInfo`][pydantic.fields.ComputedFieldInfo] objects.

    __pydantic_extra__: A dictionary containing extra values, if [`extra`][pydantic.config.ConfigDict.extra]
        is set to `'allow'`.
    __pydantic_fields_set__: The names of fields explicitly set during instantiation.
    __pydantic_private__: Values of private attributes set on the model instance.
Init docstring: 
Create a new model by parsing and validating input data from keyword arguments.

Raises [`ValidationError`][pydantic_core.ValidationError] if the input data cannot be
validated to form a valid model.

`self` is explicitly positional-only to allow `self` as a field name.
```

## 3. Swarm 流式传输实现方法

```
stream = swarm_client.run(
    agent=agent,
    messages=[{"role": "user", "content": "你好！"}],
    stream=True
)
for chunk in stream:
   print(chunk)
```

```
{'delim': 'start'}
{'content': '', 'function_call': None, 'refusal': None, 'role': 'assistant', 'tool_calls': None, 'sender': 'mini-Mate'}
{'content': '你好', 'function_call': None, 'refusal': None, 'role': None, 'tool_calls': None}
{'content': '！', 'function_call': None, 'refusal': None, 'role': None, 'tool_calls': None}
{'content': '有什么', 'function_call': None, 'refusal': None, 'role': None, 'tool_calls': None}
{'content': '我可以', 'function_call': None, 'refusal': None, 'role': None, 'tool_calls': None}
{'content': '帮助', 'function_call': None, 'refusal': None, 'role': None, 'tool_calls': None}
{'content': '你的', 'function_call': None, 'refusal': None, 'role': None, 'tool_calls': None}
{'content': '吗', 'function_call': None, 'refusal': None, 'role': None, 'tool_calls': None}
{'content': '？', 'function_call': None, 'refusal': None, 'role': None, 'tool_calls': None}
{'content': '', 'function_call': None, 'refusal': None, 'role': None, 'tool_calls': None}
{'delim': 'end'}
{'response': Response(messages=[{'content': '你好！有什么我可以帮助你的吗？', 'sender': 'mini-Mate', 'role': 'assistant', 'function_call': None, 'tool_calls': None}], agent=Agent(name='mini-Mate', model='deepseek-chat', instructions='You are a helpful agent.', functions=[], tool_choice=None, parallel_tool_calls=True), context_variables={})}
```

Swarm 添加了两个新的事件类型：

1. `{"delim":"start"}` 和 `{"delim":"end"}`：用于标识每次智能体处理单个消息（响应或函数调用）时的开始和结束。这有助于识别智能体之间的切换。

2. `{"response": Response}`：将在流结束时返回一个包含完整响应的 `Response` 对象，便于开发者使用。

```
def process_and_print_streaming_response(response):
    content = ""
    last_sender = ""

    # 处理响应中的每一个片段
    for chunk in response:
        if "sender" in chunk:
            last_sender = chunk["sender"]  # 保存消息发送者的名字

        if "content" in chunk and chunk["content"] is not None:
            # 如果当前内容为空并且有消息发送者，输出发送者名字
            if not content and last_sender:
                print(f"\033[94m{last_sender}:\033[0m", end=" ", flush=True)
                last_sender = ""
            # 输出消息内容
            print(chunk["content"], end="", flush=True)
            content += chunk["content"]

        if "tool_calls" in chunk and chunk["tool_calls"] is not None:
            # 处理工具调用
            for tool_call in chunk["tool_calls"]:
                f = tool_call["function"]
                name = f["name"]
                if not name:
                    continue
                # 输出工具调用的函数名
                print(f"\033[94m{last_sender}: \033[95m{name}\033[0m()")

        if "delim" in chunk and chunk["delim"] == "end" and content:
            # 处理消息结束的情况，换行表示结束
            print()  # End of response message
            content = ""

        if "response" in chunk:
            # 返回最终的完整响应
            return chunk["response"]
```

+ 响应片段处理：Swarm 的流式响应是逐步发送的，每次返回一小部分数据，这个函数会逐步处理这些片段并输出到控制台。

+ 工具调用处理：Swarm 支持智能体调用工具（如外部 API），如果响应中有工具调用信息，会将调用的函数名输出。

+ 消息结束：当检测到 `delim == "end"` 时，表明一条消息结束，函数会换行。

+ 返回完整响应：当处理完所有片段后，函数返回 `response` 对象。

```
stream = swarm_client.run(
    agent=agent,
    messages=[{"role": "user", "content": "你好，请介绍下你自己！"}],
    stream=True
)

response = process_and_print_streaming_response(stream)
```

```
mini-Mate: 您好！我是由中国的深度求索（DeepSeek）公司开发的智能助手DeepSeek-V3。如您有任何任何问题，我会尽我所能为您提供帮助。
```

## 4. Message 消息队列创建与多轮对话实现方法
