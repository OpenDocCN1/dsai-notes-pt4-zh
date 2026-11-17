
# 消息修剪与删除

## 修剪消息

✂️ 修剪消息：从 5 条 → 4 条

```json
{'messages': [
  HumanMessage(content='你好，我叫陈明', additional_kwargs={}, response_metadata={}, id='94e69f1f-556f-47ce-807e-447e1c869dd2'),
  HumanMessage(content='帮我写一句每日格言', additional_kwargs={}, response_metadata={}, id='be8dfc15-4488-4d8d-87b8-86f30a03062e'),
  AIMessage(content='以下是一句每日格言供您参考：\n\n"每一天都是新的开始，每一次努力都是未来的铺垫。"\n\n这句格言鼓励我们以积极的心态面对每一天，认识到每个清晨都带来新的机会，而我们当下的努力和坚持都在为更美好的未来打下基础。无论昨天如何，今天都可以重新出发，向着目标稳步前行。\n\n希望这句格言能给您带来启发和力量！如果您想要特定主题的格言，比如关于坚持、勇气、学习等方面的，我很乐意为您提供更多选择。', additional_kwargs={'refusal': None}, response_metadata={'token_usage': {'completion_tokens': 106, 'prompt_tokens': 246, 'total_tokens': 352, 'completion_tokens_details': None, 'prompt_tokens_details': {'audio_tokens': None, 'cached_tokens': 192}, 'prompt_cache_hit_tokens': 192, 'prompt_cache_miss_tokens': 54}, 'model_provider': 'deepseek', 'model_name': 'deepseek-chat', 'system_fingerprint': 'fp_ffc7281d48_prod0820_fp8_kvcache', 'id': '63f6c65b-09b8-4883-bdc9-3513200b3de9', 'finish_reason': 'stop', 'logprobs': None}, id='lc_run--a89813ba-4e3f-4cea-8990-a2a627189188-0', usage_metadata={'input_tokens': 246, 'output_tokens': 106, 'total_tokens': 352, 'input_token_details': {'cache_read': 192}, 'output_token_details': {}}),
  HumanMessage(content='请介绍下你自己。', additional_kwargs={}, response_metadata={}, id='3245ed3b-8ece-403f-b155-42bdc843de73'),
  AIMessage(content='你好陈明！我是DeepSeek，由深度求索公司创造的AI助手。很高兴认识你！\n\n让我简单介绍一下自己：\n\n**我的基本信息：**\n- 我是一个纯文本AI模型，专注于理解和生成自然语言\n- 知识截止到2024年7月\n- 支持128K的上下文长度，能够处理较长的对话\n\n**我能为你做什么：**\n- 回答各种问题，提供信息和解释\n- 协助写作、翻译、总结等文字工作\n- 进行逻辑推理和问题分析\n- 上传并处理文件（图像、txt、pdf、ppt、word、excel等），读取其中的文字信息\n- 通过联网搜索获取最新信息（需要你手动开启搜索功能）\n\n**我的特点：**\n- 完全免费使用，没有收费计划\n- 热情细腻的交流风格\n- 尽力提供准确、有帮助的回答\n\n你可以通过官方应用商店下载DeepSeek App来更方便地使用我。\n\n有什么我可以帮助你的吗？无论是学习、工作还是生活中的问题，我都很乐意协助你！', additional_kwargs={'refusal': None}, response_metadata={'token_usage': {'completion_tokens': 219, 'prompt_tokens': 310, 'total_tokens': 529, 'completion_tokens_details': None, 'prompt_tokens_details': {'audio_tokens': None, 'cached_tokens': 192}, 'prompt_cache_hit_tokens': 192, 'prompt_cache_miss_tokens': 118}, 'model_provider': 'deepseek', 'model_name': 'deepseek-chat', 'system_fingerprint': 'fp_ffc7281d48_prod0820_fp8_kvcache', 'id': '7c3432a8-c516-4a3e-897d-317d9c4bd4ae', 'finish_reason': 'stop', 'logprobs': None}, id='lc_run--029fc4ad-9dd2-4570-98bf-19fa781987be-0', usage_metadata={'input_tokens': 310, 'output_tokens': 219, 'total_tokens': 529, 'input_token_details': {'cache_read': 192}, 'output_token_details': {}}),
  HumanMessage(content='你还记得我叫什么吗？', additional_kwargs={}, response_metadata={}, id='c61cb6ff-de5c-47af-8c98-651cf3f8cc39'),
  AIMessage(content='当然记得！你刚才告诉我你叫**陈明**。很高兴再次与你交流！😊\n\n有什么我可以帮助你的吗，陈明？', additional_kwargs={'refusal': None}, response_metadata={'token_usage': {'completion_tokens': 30, 'prompt_tokens': 423, 'total_tokens': 453, 'completion_tokens_details': None, 'prompt_tokens_details': {'audio_tokens': None, 'cached_tokens': 128}, 'prompt_cache_hit_tokens': 128, 'prompt_cache_miss_tokens': 295}, 'model_provider': 'deepseek', 'model_name': 'deepseek-chat', 'system_fingerprint': 'fp_ffc7281d48_prod0820_fp8_kvcache', 'id': 'bb9048ed-9989-4207-8e5a-1db0568fd8e6', 'finish_reason': 'stop', 'logprobs': None}, id='lc_run--6e18e71d-f96a-475d-859e-01b9d3861a79-0', usage_metadata={'input_tokens': 423, 'output_tokens': 30, 'total_tokens': 453, 'input_token_details': {'cache_read': 128}, 'output_token_details': {}})
]}
```

```python
agent.invoke(
    {"messages": "你还记得我叫什么吗？"},
    config
)
```

✂️ 修剪消息：从 6 条 → 4 条

```json
{'messages': [
  HumanMessage(content='你好，我叫陈明', additional_kwargs={}, response_metadata={}, id='94e69f1f-556f-47ce-807e-447e1c869dd2'),
  HumanMessage(content='请介绍下你自己。', additional_kwargs={}, response_metadata={}, id='3245ed3b-8ece-403f-b155-42bdc843de73'),
  AIMessage(content='你好陈明！我是DeepSeek，由深度求索公司创造的AI助手。很高兴认识你！\n\n让我简单介绍一下自己：\n\n**我的基本信息：**\n- 我是一个纯文本AI模型，专注于理解和生成自然语言\n- 知识截止到2024年7月\n- 支持128K的上下文长度，能够处理较长的对话\n\n**我能为你做什么：**\n- 回答各种问题，提供信息和解释\n- 协助写作、翻译、总结等文字工作\n- 进行逻辑推理和问题分析\n- 上传并处理文件（图像、txt、pdf、ppt、word、excel等），读取其中的文字信息\n- 通过联网搜索获取最新信息（需要你手动开启搜索功能）\n\n**我的特点：**\n- 完全免费使用，没有收费计划\n- 热情细腻的交流风格\n- 尽力提供准确、有帮助的回答\n\n你可以通过官方应用商店下载DeepSeek App来更方便地使用我。\n\n有什么我可以帮助你的吗？无论是学习、工作还是生活中的问题，我都很乐意协助你！', additional_kwargs={'refusal': None}, response_metadata={'token_usage': {'completion_tokens': 219, 'prompt_tokens': 310, 'total_tokens': 529, 'completion_tokens_details': None, 'prompt_tokens_details': {'audio_tokens': None, 'cached_tokens': 192}, 'prompt_cache_hit_tokens': 192, 'prompt_cache_miss_tokens': 118}, 'model_provider': 'deepseek', 'model_name': 'deepseek-chat', 'system_fingerprint': 'fp_ffc7281d48_prod0820_fp8_kvcache', 'id': '7c3432a8-c516-4a3e-897d-317d9c4bd4ae', 'finish_reason': 'stop', 'logprobs': None}, id='lc_run--029fc4ad-9dd2-4570-98bf-19fa781987be-0', usage_metadata={'input_tokens': 310, 'output_tokens': 219, 'total_tokens': 529, 'input_token_details': {'cache_read': 192}, 'output_token_details': {}}),
  HumanMessage(content='你还记得我叫什么吗？', additional_kwargs={}, response_metadata={}, id='c61cb6ff-de5c-47af-8c98-651cf3f8cc39'),
  AIMessage(content='当然记得！你刚才告诉我你叫**陈明**。很高兴再次与你交流！😊\n\n有什么我可以帮助你的吗，陈明？', additional_kwargs={'refusal': None}, response_metadata={'token_usage': {'completion_tokens': 30, 'prompt_tokens': 423, 'total_tokens': 453, 'completion_tokens_details': None, 'prompt_tokens_details': {'audio_tokens': None, 'cached_tokens': 128}, 'prompt_cache_hit_tokens': 128, 'prompt_cache_miss_tokens': 295}, 'model_provider': 'deepseek', 'model_name': 'deepseek-chat', 'system_fingerprint': 'fp_ffc7281d48_prod0820_fp8_kvcache', 'id': 'bb9048ed-9989-4207-8e5a-1db0568fd8e6', 'finish_reason': 'stop', 'logprobs': None}, id='lc_run--6e18e71d-f96a-475d-859e-01b9d3861a79-0', usage_metadata={'input_tokens': 423, 'output_tokens': 30, 'total_tokens': 453, 'input_token_details': {'cache_read': 128}, 'output_token_details': {}})
]}
```

## 删除消息（Deleting）

删除（Deleting）是一种更主动的上下文管理方式。

它允许开发者通过 `RemoveMessage` 机制，精确指定要删除哪些消息，例如清除最早的 2 条消息、只删除 tool 调用类消息、或直接重置整个会话。

这种方法通常配合 `@after_model` 钩子使用，也就是说在模型回复后清理历史消息，保证下次调用时上下文简洁干净。

删除策略通常用于长周期运行的 Agent 或工作流系统中，既能防止上下文爆炸，又能在关键节点（如新任务开始）重置状态，实现“会话阶段化”管理。

```python
from langchain_deepseek import ChatDeepSeek
from langchain.agents import create_agent, AgentState
from langchain.agents.middleware import after_model
from langchain.messages import RemoveMessage
from langgraph.checkpoint.memory import InMemorySaver
from langgraph.runtime import Runtime
from langchain_core.runnables import RunnableConfig

# 初始化模型
model = ChatDeepSeek(model="deepseek-chat")

@after_model
def delete_old_messages(state: AgentState, runtime: Runtime) -> dict | None:
    """模型调用后，删除最早的两条消息"""
    messages = state["messages"]
    if len(messages) > 4:
        removed = [m.id for m in messages[:2]]
        print(f"🧹 删除前两条消息，ID: {removed}")
        return {"messages": [RemoveMessage(id=m.id) for m in messages[:2]]}
    return None

agent = create_agent(
    model,
    tools=[],
    middleware=[delete_old_messages],
    checkpointer=InMemorySaver(),
)
```

```python
config: RunnableConfig = {"configurable": {"thread_id": "session-1"}}

agent.invoke({"messages": "你好，我叫小明"}, config)
agent.invoke({"messages": "请记住我的名字"}, config)
agent.invoke({"messages": "写一首关于春天的诗"}, config)
```
