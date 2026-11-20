
# 工具配置与模型绑定

```
tools(tags=None, recurse=True, func_accepts_config=True, func_accepts={'writer': False, 'store': True}, tools_by_name={'query_by_keyword': StructuredTool(name='query_by_keyword', description='Search course details in local database according to keywords', args_schema=<class 'langchain_core.utils.pydantic.query_by_keyword'>, func=<function query_by_keyword at 0x0000023B37271D00>), 'fetch_real_time_info': StructuredTool(name='fetch_real_time_info', description='Get real-time Internet information', args_schema=<class 'langchain_core.utils.pydantic.fetch_real_time_info'>, func=<function fetch_real_time_info at 0x0000023B37271BC0>), 'get_weather': StructuredTool(name='get_weather', description='Call to get the current weather.', args_schema=<class 'langchain_core.utils.pydantic.get_weather'>, func=<function get_weather at 0x0000023B36036A20>)}, tool_to_state_args={'query_by_keyword': {}, 'fetch_real_time_info': {}, 'get_weather': {}}, tool_to_store_arg={'query_by_keyword': None, 'fetch_real_time_info': None, 'get_weather': None}, handle_tool_errors=True, messages_key='messages')
```

```
from langchain_openai import ChatOpenAI

model = ChatOpenAI(model="deepseek-chat",
                   api_key='sk-98f37d04e1374105b31cb994ae0c9c96',
                   base_url='https://api.deepseek.com')

llm = model.bind_tools(tools)
```

# 构建 ReAct Agent

```
from langgraph.prebuilt import create_react_agent

graph = create_react_agent(llm, tools=tools)
```

# 可视化 Agent 图结构

```
from IPython.display import Image, display

display(Image(graph.get_graph().draw_mermaid_png()))
```

![](img/8eebd571369babedf6c9aa27edc8ec3f.png)

理解了上面的`create_react_agent`方法内部的构建原理后，其实就能明白：当通过`create_react_agent(llm, tools=tools)`一行代码的执行，现在得到的已经是一个编译后、可执行的图了。我们可以通过`mermaid`方法来可视化经过`create_react_agent`方法构造出来的图结构，代码如下所示：

返回的是编译好的`LangGraph`可运行程序，可直接用于聊天交互。调用方式则和之前使用的方法一样，我们可以依次针对不同复杂程度的需求依次进行提问。首先是测试是否可以不使用工具，直接调用大模型生成响应。

```
# query = "你好，请你介绍一下你自己"
# input_message = {"messages": [HumanMessage(content=query)]}

# 可以自动处理成 HumanMessage 的消息格式
finan_response = graph.invoke({"messages": ["你好，请你介绍一下你自己"]})
finan_response
```

```
{'messages': [HumanMessage(content='你好，请你介绍一下你自己', additional_kwargs={}, response_metadata={}, id='4a4f31e2-1cf4-478d-9b84-088a88476d26'),
  AIMessage(content='你好！我是一个人工智能助手，专门设计来帮助用户获取信息、解答问题、提供建议和执行各种任务。我可以帮助你查找资料、学习新知识、规划日程、解决问题等。无论你有什么问题或需要帮助，我都会尽力提供支持。请问有什么我可以帮你的吗？', additional_kwargs={'refusal': None}, response_metadata={'token_usage': {'completion_tokens': 57, 'prompt_tokens': 246, 'total_tokens': 303, 'completion_tokens_details': None, 'prompt_tokens_details': None, 'prompt_cache_hit_tokens': 192, 'prompt_cache_miss_tokens': 54}, 'model_name': 'deepseek-chat', 'system_fingerprint': 'fp_3a5770e1b4', 'finish_reason': 'stop', 'logprobs': None}, id='run-a0dd0b3d-7414-4e79-b491-26be1e967b7b-0', usage_metadata={'input_tokens': 246, 'output_tokens': 57, 'total_tokens': 303, 'input_token_details': {}, 'output_token_details': {}})]}
```

```
finan_response["messages"][-1].content
```

```
'你好！我是一个人工智能助手，专门设计来帮助用户获取信息、解答问题、提供建议和执行各种任务。我可以帮助你查找资料、学习新知识、规划日程、解决问题等。无论你有什么问题或需要帮助，我都会尽力提供支持。请问有什么我可以帮你的吗？'
```

# 复杂查询测试（自动调用工具）

加大输入问题的复杂度，接下来我们提问的问题希望它能够自动找到正确的工具函数，基于工具的执行结果作为既定的事实，引导生成最终的回复。

```
finan_response = graph.invoke({"messages": ["课程中有没有Agent相关的内容？"]})

finan_response
```
