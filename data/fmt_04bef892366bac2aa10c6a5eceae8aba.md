
# 使用示例

```
query = "DeepSeek v3 相关的新闻"
result = fetch_real_time_info(query)
print(result)
```

```
[{"title": "DeepSeek崛起AI经济模型或将开启全面重构 - 21财经", "link": "https://www.21jingji.com/article/20250115/a4cf46d7e05505af03f59aad1f78450d.html", "snippet": "在高端芯片禁运的情况下，DeepSeek V3靠着往年囤积的“阉割版”H卡，用区区五百万美元，在惊人的不到三百万H800 GPU小时里完成了预训练，获得了聊天机器人竞技场 ...", "date": "8 hours ago", "position": 1}, {"title": "DeepSeek推出App版本，使用V3大模型，网友：终于等到了 - 腾讯新闻", "link": "https://news.qq.com/rain/a/20250113A05E1Z00", "snippet": "该App的商店页面显示，这是DeepSeek官方推出的AI助手，可“免费体验与全球领先AI模型的互动交流”。其使用开源的DeepSeek-V3 大模型，多项性能指标对齐海外顶尖 ...", "date": "2 days ago", "position": 2}, {"title": "深度求索大模型：“花小钱办大事” - 科技日报", "link": "https://www.stdaily.com/web/gdxw/2025-01/14/content_286087.html", "snippet": "... DeepSeek-V3模型，同时公布长达53页的技术报告，介绍关键技术和训练细节。 和很多语焉不详的报告相比，这份报告真正做到了开源。其中最抓人眼球的部分是，V3 ...", "date": "18 hours ago", "position": 3}]
```

如果功能正常，该函数将根据用户的输入，返回实时的网页检索信息，这包括标题、链接、摘要等等有效的信息。而如果想要将普通的函数变成`ToolNode`可以应用的外部函数，只需要在函数定义时添加`@tool`装饰器。

## 定义可调用工具函数

```
from langchain_core.tools import tool

@tool
def fetch_real_time_info(query):
    """Get real-time Internet information"""
    url = "https://google.serper.dev/search"
    payload = json.dumps({
      "q": query,
      "num": 1,
    })
    headers = {
      'X-API-KEY': 'd9c2c002085468d95712cbc71d76c4302b4f7342',
      'Content-Type': 'application/json'
    }
    
    response = requests.post(url, headers=headers, data=payload)
    data = json.loads(response.text)  # 将返回的JSON字符串转换为字典
    if 'organic' in data:
        return json.dumps(data['organic'],  ensure_ascii=False)  # 返回'organic'部分的JSON字符串
    else:
        return json.dumps({"error": "No organic results found"},  ensure_ascii=False)  # 如果没有'organic'键，返回错误信息
```

```
print(f'''
name: {fetch_real_time_info.name}
description: {fetch_real_time_info.description}
arguments: {fetch_real_time_info.args}
''')
```

```
name: fetch_real_time_info
description: Get real-time Internet information
arguments: {'query': {'title': 'Query'}}
```

## ToolNode 实例化与单工具调用

接下来使用`ToolNode`对函数进行实例化，代码如下：

```
from langgraph.prebuilt import ToolNode

tools = [fetch_real_time_info]
tool_node = ToolNode(tools)
```

```
tool_node
```

```
tools(tags=None, recurse=True, func_accepts_config=True, func_accepts={'writer': False, 'store': True}, tools_by_name={'fetch_real_time_info': StructuredTool(name='fetch_real_time_info', description='Get real-time Internet information', args_schema=<class 'langchain_core.utils.pydantic.fetch_real_time_info'>, func=<function fetch_real_time_info at 0x0000023B318F45E0>)}, tool_to_state_args={'fetch_real_time_info': {}}, tool_to_store_arg={'fetch_real_time_info': None}, handle_tool_errors=True, messages_key='messages')
```

`ToolNode`使用消息列表对图状态进行操作。所以它要求消息列表中的最后一条消息是带有`tool_calls`参数的`AIMessage`，比如我们可以手动调用工具节点：

```
from langchain_core.messages import AIMessage

message_with_single_tool_call = AIMessage(
    content="",
    tool_calls=[
        {
            "name": "fetch_real_time_info",
            "args": {"query": "DeepSeek v3 最新的新闻"},
            "id": "tool_call_id",
            "type": "tool_call",
        }
    ],
)

tool_node.invoke({"messages": [message_with_single_tool_call]})
```

```
{'messages': [ToolMessage(content='[{"title": "DeepSeek崛起AI经济模型或将开启全面重构 - 证券时报", "link": "https://www.stcn.com/article/detail/1494815.html", "snippet": "在高端芯片禁运的情况下，DeepSeek V3靠着往年囤积的“阉割版”H卡，用区区五百万美元，在惊人的不到三百万GPU小时里完成了预训练，获得了聊天机器人竞技场（ ...", "date": "3 hours ago", "position": 1}]', name='fetch_real_time_info', tool_call_id='tool_call_id')]}
```

## 并行调用多个工具

同时，如果将多个工具调用同时传递给`AIMessage`的`tool_calls`参数，仍然可以使用`ToolNode`进行并行工具调用：

```
@tool
def get_weather(location):
    """Call to get the current weather."""
    if location.lower() in ["beijing"]:
        return "北京的温度是16度，天气晴朗。"
    elif location.lower() in ["shanghai"]:
        return "上海的温度是20度，部分多云。"
    else:
        return "不好意思，并未查询到具体的天气信息。"
```

```
tools = [fetch_real_time_info, get_weather]
tool_node = ToolNode(tools)
```

```
tool_node
```

```
tools(tags=None, recurse=True, func_accepts_config=True, func_accepts={'writer': False, 'store': True}, tools_by_name={'fetch_real_time_info': StructuredTool(name='fetch_real_time_info', description='Get real-time Internet information', args_schema=<class 'langchain_core.utils.pydantic.fetch_real_time_info'>, func=<function fetch_real_time_info at 0x0000023B318F45E0>), 'get_weather': StructuredTool(name='get_weather', description='Call to get the current weather.', args_schema=<class 'langchain_core.utils.pydantic.get_weather'>, func=<function get_weather at 0x0000023B36036A20>)}, tool_to_state_args={'fetch_real_time_info': {}, 'get_weather': {}}, tool_to_store_arg={'fetch_real_time_info': None, 'get_weather': None}, handle_tool_errors=True, messages_key='messages')
```

```
message_with_multiple_tool_calls = AIMessage(
    content="",
    tool_calls=[
        {
            "name": "fetch_real_time_info",
            "args": {"query": "DeepSeek v3 最新的新闻"},
            "id": "tool_call_id",
            "type": "tool_call",
        },
        {
            "name": "get_weather",
            "args": {"location": "beijing"},
            "id": "tool_call_id_2",
            "type": "tool_call",
        },
    ],
)

tool_node.invoke({"messages": [message_with_multiple_tool_calls]})
```

```
{'messages': [ToolMessage(content='[{"title": "DeepSeek崛起AI经济模型或将开启全面重构 - 证券时报", "link": "https://www.stcn.com/article/detail/1494815.html", "snippet": "在高端芯片禁运的情况下，DeepSeek V3靠着往年囤积的“阉割版”H卡，用区区五百万美元，在惊人的不到三百万GPU小时里完成了预训练，获得了聊天机器人竞技场（ ...", "date": "3 hours ago", "position": 1}]', name='fetch_real_time_info', tool_call_id='tool_call_id'),
  ToolMessage(content='北京的温度是16度，天气晴朗。', name='get_weather', tool_call_id='tool_call_id_2')]}
```

## Tool Calling Agent 原理

而Tool Calling Agent的本质原理是：让大模型根据用户的输入，自动的去判断应该使用哪个函数，并实际的执行，最后结合工具的响应结果 + 用户的原始问题作为完整的Prompt生成最终的问题。即如下图所示：

![](img/f410e735c8816992083c45afc8b1c0fc.png)

通过`ToolNode(tools)`可以根据参数来执行函数，并返回结果。而其前一步，根据自然语言生成执行具体某个函数必要参数的过程，则由大模型决定，所以一个完整的基于大模型的工具调用过程应该是，在实例化大模型的时候，就告诉大模型你都有哪些工具可以使用。这个过程可以通过`bind_tools`函数来实现。代码如下：

```
from langchain_openai import ChatOpenAI

model = ChatOpenAI(model="deepseek-chat",
                   api_key='sk-98f37d04e1374105b31cb994ae0c9c96',
                   base_url='https://api.deepseek.com')
```

```
tools = [fetch_real_time_info, get_weather]
model_with_tools = model.bind_tools(tools)
```

```
model_with_tools
```
