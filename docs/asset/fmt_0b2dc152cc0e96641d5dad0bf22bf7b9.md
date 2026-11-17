
## 5. `create_agent` 记忆管理

在实际进行Agent开发时，还有一项工作至关重要，那就是需要管理智能体的记忆。

```
from langgraph.checkpoint.memory import InMemorySaver
```

```
checkpointer = InMemorySaver()
```

```
tools = [get_weather]
```

```
agent = create_agent(model=model, 
                     tools=tools,
                     checkpointer=checkpointer)
```

```
config = {
    "configurable": {
        "thread_id": "1"  
    }
}
```

```
response = agent.invoke(
    {"messages": [{"role": "user", "content": "你好，我叫陈明，好久不见！"}]},
    config
)
```

```
response['messages']
```

```
[HumanMessage(content='你好，我叫陈明，好久不见！', additional_kwargs={}, response_metadata={}, id='0bcbd429-e2c5-4078-ae2e-342cd3ded21d'),
 AIMessage(content='你好陈明！很高兴见到你！虽然我们可能是第一次对话，但还是很开心能和你聊天。有什么我可以帮助你的吗？比如查询天气信息，或者其他问题都可以问我！', additional_kwargs={'refusal': None}, response_metadata={'token_usage': {'completion_tokens': 37, 'prompt_tokens': 251, 'total_tokens': 288, 'completion_tokens_details': None, 'prompt_tokens_details': {'audio_tokens': None, 'cached_tokens': 0}, 'prompt_cache_hit_tokens': 0, 'prompt_cache_miss_tokens': 251}, 'model_provider': 'deepseek', 'model_name': 'deepseek-chat', 'system_fingerprint': 'fp_ffc7281d48_prod0820_fp8_kvcache', 'id': '620f39da-3567-4b1e-8c05-add3105d60e2', 'finish_reason': 'stop', 'logprobs': None}, id='lc_run--dde8ba3d-58c6-494e-baf4-cf0023ac06f9-0', usage_metadata={'input_tokens': 251, 'output_tokens': 37, 'total_tokens': 288, 'input_token_details': {'cache_read': 0}, 'output_token_details': {}})]
```

```
response['messages'][-1].content
```

```
'你好陈明！很高兴见到你！虽然我们可能是第一次对话，但还是很开心能和你聊天。有什么我可以帮助你的吗？比如查询天气信息，或者其他问题都可以问我！'
```

此时记忆就自动保存在当前Agent和线程中：

```
latest = agent.get_state(config)
```

```
latest
```

```
StateSnapshot(values={'messages': [HumanMessage(content='你好，我叫陈明，好久不见！', additional_kwargs={}, response_metadata={}, id='0bcbd429-e2c5-4078-ae2e-342cd3ded21d'), AIMessage(content='你好陈明！很高兴见到你！虽然我们可能是第一次对话，但还是很开心能和你聊天。有什么我可以帮助你的吗？比如查询天气信息，或者其他问题都可以问我！', additional_kwargs={'refusal': None}, response_metadata={'token_usage': {'completion_tokens': 37, 'prompt_tokens': 251, 'total_tokens': 288, 'completion_tokens_details': None, 'prompt_tokens_details': {'audio_tokens': None, 'cached_tokens': 0}, 'prompt_cache_hit_tokens': 0, 'prompt_cache_miss_tokens': 251}, 'model_provider': 'deepseek', 'model_name': 'deepseek-chat', 'system_fingerprint': 'fp_ffc7281d48_prod0820_fp8_kvcache', 'id': '620f39da-3567-4b1e-8c05-add3105d60e2', 'finish_reason': 'stop', 'logprobs': None}, id='lc_run--dde8ba3d-58c6-494e-baf4-cf0023ac06f9-0', usage_metadata={'input_tokens': 251, 'output_tokens': 37, 'total_tokens': 288, 'input_token_details': {'cache_read': 0}, 'output_token_details': {}}]}, next=(), config={'configurable': {'thread_id': '1', 'checkpoint_ns': '', 'checkpoint_id': '1f0b3e18-edee-6355-8001-e32b35d3d174'}}, metadata={'source': 'loop', 'step': 1, 'parents': {}}, created_at='2025-10-28T09:36:19.447074+00:00', parent_config={'configurable': {'thread_id': '1', 'checkpoint_ns': '', 'checkpoint_id': '1f0b3e18-d94f-633c-8000-b4ca61eb2816'}}, tasks=(), interrupts=())
```

当我们再次进行对话时，直接带入线程ID，即可带入此前对话记忆：

```
response = agent.invoke(
    {"messages": [{"role": "user", "content": "你好，请问你还记得我叫什么名字么？"}]},
    config
)
```

```
response['messages']
```

```
[HumanMessage(content='你好，我叫陈明，好久不见！', additional_kwargs={}, response_metadata={}, id='0bcbd429-e2c5-4078-ae2e-342cd3ded21d'),
 AIMessage(content='你好陈明！很高兴见到你！虽然我们可能是第一次对话，但还是很开心能和你聊天。有什么我可以帮助你的吗？比如查询天气信息，或者其他问题都可以问我！', additional_kwargs={'refusal': None}, response_metadata={'token_usage': {'completion_tokens': 37, 'prompt_tokens': 251, 'total_tokens': 288, 'completion_tokens_details': None, 'prompt_tokens_details': {'audio_tokens': None, 'cached_tokens': 0}, 'prompt_cache_hit_tokens': 0, 'prompt_cache_miss_tokens': 251}, 'model_provider': 'deepseek', 'model_name': 'deepseek-chat', 'system_fingerprint': 'fp_ffc7281d48_prod0820_fp8_kvcache', 'id': '620f39da-3567-4b1e-8c05-add3105d60e2', 'finish_reason': 'stop', 'logprobs': None}, id='lc_run--dde8ba3d-58c6-494e-baf4-cf0023ac06f9-0', usage_metadata={'input_tokens': 251, 'output_tokens': 37, 'total_tokens': 288, 'input_token_details': {'cache_read': 0}, 'output_token_details': {}}),
 HumanMessage(content='你好，请问你还记得我叫什么名字么？', additional_kwargs={}, response_metadata={}, id='450bbe75-6e48-46ac-b379-e2dd92c1279c'),
 AIMessage(content='当然记得！你刚才告诉我你叫陈明。很高兴再次和你打招呼！有什么我可以为你做的吗？', additional_kwargs={'refusal': None}, response_metadata={'token_usage': {'completion_tokens': 22, 'prompt_tokens': 302, 'total_tokens': 324, 'completion_tokens_details': None, 'prompt_tokens_details': {'audio_tokens': None, 'cached_tokens': 256}, 'prompt_cache_hit_tokens': 256, 'prompt_cache_miss_tokens': 46}, 'model_provider': 'deepseek', 'model_name': 'deepseek-chat', 'system_fingerprint': 'fp_ffc7281d48_prod0820_fp8_kvcache', 'id': 'e9663dd9-9ee5-4aa4-abfc-202ae90fa14d', 'finish_reason': 'stop', 'logprobs': None}, id='lc_run--6d6ec240-7af9-42b7-87ae-725b963b4a2e-0', usage_metadata={'input_tokens': 302, 'output_tokens': 22, 'total_tokens': 324, 'input_token_details': {'cache_read': 256}, 'output_token_details': {}})]
```

```
response['messages'][-1].content
```

```
'当然记得！你刚才告诉我你叫陈明。很高兴再次和你打招呼！有什么我可以为你做的吗？'
```

```
latest = agent.get_state(config)
latest
```
