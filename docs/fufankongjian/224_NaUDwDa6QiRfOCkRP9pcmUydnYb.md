# LangGraph 基础入门与 Agent 开发实战

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/ZUTvdjFYdoX0tcxndOLcVlnHnwb](https://kq4b3vgg5b.feishu.cn/docx/ZUTvdjFYdoX0tcxndOLcVlnHnwb)

## 课程说明：

+   体验课内容节选⾃《2025大模型Agent智能体开发实战》完整版付费课程

体验课时间有限，若想深度学习⼤模型技术，欢迎⼤家报名由我主讲的《2025大模型Agent智能体开发实战》：

![](img/ce1b249a1e5e25a3532db78588c4be94.png)

![](img/b4d61f92df5cdcc8881f413c52eade6d.png)

此外，公开课全部课件，以及项目代码、数据等学习资料，扫码⬆️联系助教即可领取～

---

# 《2025大模型智能体Agent开发实战》体验课

## LangChain AI+ DeepSeek v3 企业 Agent 开发实战

## Part 2. LangGraph 基础入门与 Agent 开发实战

# 1. LangGraph 接入 DeepSeek v3 模型

LangGraph 中的 StateGraph类，这个类允许我们创建图，其节点通过读取和写入共享状态进行通信。 StateGraph 类由开发者定义的 State 对象进行参数化，该对象表示图中的节点将通过其进行通信的共享数据结构。

```
from typing import Annotated
from langgraph.graph import StateGraph
from typing_extensions import TypedDict
from langgraph.graph.message import add_messages

class State(TypedDict):
    messages: Annotated[list, add_messages]

graph_builder = StateGraph(State)
```

定义模型实例，这里使用DeepSeek v3 模型。

```
from langchain_openai import ChatOpenAI

model = ChatOpenAI(model=&quot;deepseek-chat&quot;,
                   api_key=&#x27;sk-98f37d04e1374105b31cb994ae0c9c96&#x27;,
                   base_url=&#x27;https://api.deepseek.com&#x27;)
```

定义大模型对话节点。

```
def chatbot(state: State):
    # print(state)
    return {&quot;messages&quot;: [model.invoke(state[&quot;messages&quot;])]}
```

添加节点并编译图。同样，我们先看普通边。如果直接想从节点A到节点B，可以直接使用add_edge方法。注意：LangGraph有两个特殊的节点：START和END。START表示将用户输入发送到图的节点。使用该节点的主要目的是确定应该首先调用哪些节点。END节点是代表终端节点的特殊节点。当想要指示哪些边完成后没有任何操作时，将使用该节点。因此，一个完整的图就可以使用如下代码进行定义：

```
from langgraph.graph import START, END

# 添加自定义节点
graph_builder.add_node(&quot;chatbot&quot;, chatbot)

# 构建边
graph_builder.add_edge(START, &quot;chatbot&quot;)
graph_builder.add_edge(&quot;chatbot&quot;, END)

# 编译图
graph = graph_builder.compile()
```

LangGraph还提供了多种内置的图形可视化方法，能够将任**何Graph以图形的形式展示出来，帮助我们更好地理**解节点之间的关系和流程的动态变化。可视化最大的好处是：直接从代码中生成图形化的表示，可以检查图的执行逻辑是否符合构建的预期。LangGraph提供的三种图形可视化方法如下：

+   **Mermaid.Ink**：一[个开源服务，可](https://github.com/mermaid-js/mermaid)以根据 Mermaid 代码生成图表的 URL。它通过 API 提供多种输出格式，包括 PNG、JPEG、SVG 和 PDF，并可以自定义尺寸、主题和背景颜色等选项。开源仓库👉：mermaid

+   Mermaid + Pyppeteer：使用 Mermaid 结合 Pyppeteer 的主要区别在于如何将 Mermaid 图表转换成图像或其他格式。Mermaid 本身是一个轻量级的工具，用于通过文本描述生成图表的图形表示。而 Pyppeteer 是一个 Python 库，它提供了一个接口来控制 Chrome，自动打开包含 Mermaid 图表的网页，然后通过浏览器自动截图功能捕获这些图表，生成图像文件。

+   **Graphviz**：Graphviz 是一个图形可视化软件，主要用于自动图形布局。它非常适合于复杂图形的生成，如有向图和无向图，而且它支持多种格式的图像输出，如 PNG、SVG、PDF 等，有更精细的布局控制。

> https://langchain-ai.github.io/langgraph/how-tos/visualization/

![](img/09ec4b7e1fc056fef307cdec2d8b0579.png)

如果是Linux操作系统，建议使用Graphviz工具。而Windows系统建议使用Mermaid + Pyppeteer方法，因为在Windwos中Graphviz并不能直接通过 pip install的形式安装，编译安装的方法较为复杂。这里我们就使用Mermaid + Pyppeteer来进行图的可视化操作。首先，在当前的虚拟环境中安装依赖包，执行如下代码：

```
# ! pip install pyppeteer ipython
```

生成图结构的可视化非常直接，只需一行代码即可完成。具体代码如下：

```
from IPython.display import Image, display

display(Image(graph.get_graph(xray=True).draw_mermaid_png()))
```

![](img/1ca9a1c1191d1b471725c07ffeade8a2.png)

当通过 builder.compile() 方法编译图后，编译后的 graph 对象提供了 invoke 方法，该方法用于启动图的执行。我们可以通过 invoke 方法传递一个初始状态，这个状态将作为图执行的起始输入。代码如下：

```
input_message = {&quot;messages&quot;: [&quot;你好，请你介绍一下你自己&quot;]}

result = graph.invoke(input_message)
print(result)
```

```
{&#x27;messages&#x27;: [HumanMessage(content=&#x27;你好，请你介绍一下你自己&#x27;, additional_kwargs={}, response_metadata={}, id=&#x27;7ce1accb-1de7-432a-b77e-6617a45eb24f&#x27;), AIMessage(content=&#x27;你好！我是一个由OpenAI开发的人工智能助手，旨在帮助用户解答问题、提供信息、协助学习和完成各种任务。我可以处理多种语言，理解自然语言，并根据你的需求提供相应的帮助。\n\n我的功能包括但不限于：\n1. **信息查询**：提供关于科学、历史、文化、技术等领域的知识。\n2. **学习辅助**：帮助解答作业问题、解释复杂概念或提供学习建议。\n3. **语言翻译**：支持多种语言之间的翻译。\n4. **写作支持**：协助撰写文章、邮件、报告等。\n5. **日常建议**：提供生活、健康、旅行等方面的建议。\n6. **编程帮助**：解答编程问题、调试代码或提供算法思路。\n\n我没有个人情感或意识，但会尽力以友好和专业的方式与你互动。如果你有任何问题或需要帮助，随时告诉我！ 😊&#x27;, additional_kwargs={&#x27;refusal&#x27;: None}, response_metadata={&#x27;token_usage&#x27;: {&#x27;completion_tokens&#x27;: 179, &#x27;prompt_tokens&#x27;: 8, &#x27;total_tokens&#x27;: 187, &#x27;completion_tokens_details&#x27;: None, &#x27;prompt_tokens_details&#x27;: None, &#x27;prompt_cache_hit_tokens&#x27;: 0, &#x27;prompt_cache_miss_tokens&#x27;: 8}, &#x27;model_name&#x27;: &#x27;deepseek-chat&#x27;, &#x27;system_fingerprint&#x27;: &#x27;fp_3a5770e1b4&#x27;, &#x27;finish_reason&#x27;: &#x27;stop&#x27;, &#x27;logprobs&#x27;: None}, id=&#x27;run-dadb251b-b1a5-4651-a8f2-785d21114bed-0&#x27;, usage_metadata={&#x27;input_tokens&#x27;: 8, &#x27;output_tokens&#x27;: 179, &#x27;total_tokens&#x27;: 187, &#x27;input_token_details&#x27;: {}, &#x27;output_token_details&#x27;: {}})]}
```

```
print(result[&quot;messages&quot;][-1].content)
```

```
你好！我是一个由OpenAI开发的人工智能助手，旨在帮助用户解答问题、提供信息、协助学习和完成各种任务。我可以处理多种语言，理解自然语言，并根据你的需求提供相应的帮助。

我的功能包括但不限于：
1. **信息查询**：提供关于科学、历史、文化、技术等领域的知识。
2. **学习辅助**：帮助解答作业问题、解释复杂概念或提供学习建议。
3. **语言翻译**：支持多种语言之间的翻译。
4. **写作支持**：协助撰写文章、邮件、报告等。
5. **日常建议**：提供生活、健康、旅行等方面的建议。
6. **编程帮助**：解答编程问题、调试代码或提供算法思路。

我没有个人情感或意识，但会尽力以友好和专业的方式与你互动。如果你有任何问题或需要帮助，随时告诉我！ 😊
```

通过一个程序构建可交互式的聊天机器人。完整代码如下：

```
def stream_graph_updates(user_input: str):  
    for event in graph.stream({&quot;messages&quot;: [(&quot;user&quot;, user_input)]}):
        for value in event.values():
            print(&quot;模型回复:&quot;, value[&quot;messages&quot;][-1].content)


while True:
    try:
        user_input = input(&quot;用户提问: &quot;)
        if user_input.lower() in [&quot;退出&quot;]:
            print(&quot;下次再见！&quot;)
            break

        stream_graph_updates(user_input)
    except:
        break
```

```
用户提问:  退出


下次再见！
```

掌握State的定义模式和消息传递是LangGraph中最关键，也是构建应用最核心的部分，所有的高阶功能，如工具调用、上下文记忆，人机交互等依赖State的管理和使用，所以大家务必理解并掌握上述相关内容。

# 2. 工具调用代理

Tool Calling Agent（工具调用代理）是LangGraph**支持的一种AI ****Agent代理架构。这个**代理架构是在Router Agent的基础上，大模型可以自主选择并使用多种工具来完成某个条件分支中的任务。 当我们希望代理与外部系统交互时，工具就非常有用。外部系统（例如API）通常需要特定的输入模式，而不是自然语言。例如，当我们绑定 API 作为工具时，我们赋予大模型对所需输入模式的感知，大模型就能根据用户的自然语言输入选择调用工具，并将返回符合该工具架构的输出。

![](img/c5472a84b07a6fc74cf334a6e21e3b31.png)

在LangGraph框架中，可以直接使用预构建ToolNode进行工具调用，其内部实现原理和我们之前介绍的手动实现的Function Calling流程思路基本一致，即：

> LangGraph ToolNode 源码：https://langchain-ai.github.io/langgraph/reference/prebuilt/#langgraph.prebuilt.tool_node.ToolNode

```
tools_by_name = {tool.name: tool for tool in tools}
def tool_node(state: dict):
    result = []
    for tool_call in state[&quot;messages&quot;][-1].tool_calls:
        tool = tools_by_name[tool_call[&quot;name&quot;]]
        observation = tool.invoke(tool_call[&quot;args&quot;])
        result.append(ToolMessage(content=observation, tool_call_id=tool_call[&quot;id&quot;]))
    return {&quot;messages&quot;: result}
```

## 2.1 使用ToolNode接入工具

经过ToolNode工具后，其返回的是一个LangChain Runnable对象，会将图形状态（带有消息列表）作为输入并输出状态更新以及工具调用的结果，通过这种设计去适配LangGraph中其他的功能组件。比如我们LangGraph预构建的更高级AI Agent架构 - ReAct，两者搭配起来可以开箱即用，同时通过ToolNode构建的工具对象也能与任何StateGraph一起使用。由此，对于ToolNode的使用，有三个必要的点需要满足，即：

1.  **状态必须包含消息列表。**

1.  最后一条消息必须是AIMessage。

1.  AIMessage必须填充tool_calls。

我们尝试进行一下实践。首先，既然是工具调用代理，我们就准备一下需要用的外部工具/函数。这里我们使用Serper API去构建实时联网检索的功能。

```
# ! pip install requests
```

```
import requests
import json

def fetch_real_time_info(query):
    &quot;&quot;&quot;Get real-time Internet information&quot;&quot;&quot;
    
    url = &quot;https://google.serper.dev/search&quot;
    
    payload = json.dumps({
      &quot;q&quot;: query,
      &quot;num&quot;: 3,
    })
    
    headers = {
      &#x27;X-API-KEY&#x27;: &#x27;d9c2c002085468d95712cbc71d76c4302b4f7342&#x27;,
      &#x27;Content-Type&#x27;: &#x27;application/json&#x27;
    }
    
    response = requests.post(url, headers=headers, data=payload)
    
    data = json.loads(response.text)  # 将返回的JSON字符串转换为字典
    if &#x27;organic&#x27; in data:
        return json.dumps(data[&#x27;organic&#x27;],  ensure_ascii=False)  # 返回&#x27;organic&#x27;部分的JSON字符串
    else:
        return json.dumps({&quot;error&quot;: &quot;No organic results found&quot;},  ensure_ascii=False)  # 如果没有&#x27;organic&#x27;键，返回错误信息
```

定义好实时联网检索函数后，我们可以先进行一下连通性测试：

```
# 使用示例
query = &quot;DeepSeek v3 相关的新闻&quot;
result = fetch_real_time_info(query)
print(result)
```

```
[{&quot;title&quot;: &quot;DeepSeek崛起AI经济模型或将开启全面重构 - 21财经&quot;, &quot;link&quot;: &quot;https://www.21jingji.com/article/20250115/a4cf46d7e05505af03f59aad1f78450d.html&quot;, &quot;snippet&quot;: &quot;在高端芯片禁运的情况下，DeepSeek V3靠着往年囤积的“阉割版”H卡，用区区五百万美元，在惊人的不到三百万H800 GPU小时里完成了预训练，获得了聊天机器人竞技场 ...&quot;, &quot;date&quot;: &quot;8 hours ago&quot;, &quot;position&quot;: 1}, {&quot;title&quot;: &quot;DeepSeek推出App版本，使用V3大模型，网友：终于等到了 - 腾讯新闻&quot;, &quot;link&quot;: &quot;https://news.qq.com/rain/a/20250113A05E1Z00&quot;, &quot;snippet&quot;: &quot;该App的商店页面显示，这是DeepSeek官方推出的AI助手，可“免费体验与全球领先AI模型的互动交流”。其使用开源的DeepSeek-V3 大模型，多项性能指标对齐海外顶尖 ...&quot;, &quot;date&quot;: &quot;2 days ago&quot;, &quot;position&quot;: 2}, {&quot;title&quot;: &quot;深度求索大模型：“花小钱办大事” - 科技日报&quot;, &quot;link&quot;: &quot;https://www.stdaily.com/web/gdxw/2025-01/14/content_286087.html&quot;, &quot;snippet&quot;: &quot;... DeepSeek-V3模型，同时公布长达53页的技术报告，介绍关键技术和训练细节。 和很多语焉不详的报告相比，这份报告真正做到了开源。其中最抓人眼球的部分是，V3 ...&quot;, &quot;date&quot;: &quot;18 hours ago&quot;, &quot;position&quot;: 3}]
```

如果功能正常，该函数将根据用户的输入，返回实时的网页检索信息，这包括标题、链接、摘要等等有效的信息。而如果想要将普通的函数变成ToolNode可以应用的外部函数，只需要在函数定义时添加@tool装饰器。

```
from langchain_core.tools import tool

@tool
def fetch_real_time_info(query):
    &quot;&quot;&quot;Get real-time Internet information&quot;&quot;&quot;
    url = &quot;https://google.serper.dev/search&quot;
    payload = json.dumps({
      &quot;q&quot;: query,
      &quot;num&quot;: 1,
    })
    headers = {
      &#x27;X-API-KEY&#x27;: &#x27;d9c2c002085468d95712cbc71d76c4302b4f7342&#x27;,
      &#x27;Content-Type&#x27;: &#x27;application/json&#x27;
    }
    
    response = requests.post(url, headers=headers, data=payload)
    data = json.loads(response.text)  # 将返回的JSON字符串转换为字典
    if &#x27;organic&#x27; in data:
        return json.dumps(data[&#x27;organic&#x27;],  ensure_ascii=False)  # 返回&#x27;organic&#x27;部分的JSON字符串
    else:
        return json.dumps({&quot;error&quot;: &quot;No organic results found&quot;},  ensure_ascii=False)  # 如果没有&#x27;organic&#x27;键，返回错误信息
```

```
print(f&#x27;&#x27;&#x27;
name: {fetch_real_time_info.name}
description: {fetch_real_time_info.description}
arguments: {fetch_real_time_info.args}
&#x27;&#x27;&#x27;)
```

```
name: fetch_real_time_info
description: Get real-time Internet information
arguments: {&#x27;query&#x27;: {&#x27;title&#x27;: &#x27;Query&#x27;}}
```

接下来使用ToolNode对函数进行实例化，代码如下：

```
from langgraph.prebuilt import ToolNode

tools = [fetch_real_time_info]
tool_node = ToolNode(tools)
```

```
tool_node
```

```
tools(tags=None, recurse=True, func_accepts_config=True, func_accepts={&#x27;writer&#x27;: False, &#x27;store&#x27;: True}, tools_by_name={&#x27;fetch_real_time_info&#x27;: StructuredTool(name=&#x27;fetch_real_time_info&#x27;, description=&#x27;Get real-time Internet information&#x27;, args_schema=&lt;class &#x27;langchain_core.utils.pydantic.fetch_real_time_info&#x27;&gt;, func=&lt;function fetch_real_time_info at 0x0000023B318F45E0&gt;)}, tool_to_state_args={&#x27;fetch_real_time_info&#x27;: {}}, tool_to_store_arg={&#x27;fetch_real_time_info&#x27;: None}, handle_tool_errors=True, messages_key=&#x27;messages&#x27;)
```

ToolNode使用消息列表对图状态进行操作。所以它要求消息列表中的最后一条消息是带有tool_calls参数的AIMessage ，比如我们可以手动调用工具节点：

```
from langchain_core.messages import AIMessage

message_with_single_tool_call = AIMessage(
    content=&quot;&quot;,
    tool_calls=[
        {
            &quot;name&quot;: &quot;fetch_real_time_info&quot;,
            &quot;args&quot;: {&quot;query&quot;: &quot;DeepSeek v3 最新的新闻&quot;},
            &quot;id&quot;: &quot;tool_call_id&quot;,
            &quot;type&quot;: &quot;tool_call&quot;,
        }
    ],
)

tool_node.invoke({&quot;messages&quot;: [message_with_single_tool_call]})
```

```
{&#x27;messages&#x27;: [ToolMessage(content=&#x27;[{&quot;title&quot;: &quot;DeepSeek崛起AI经济模型或将开启全面重构 - 证券时报&quot;, &quot;link&quot;: &quot;https://www.stcn.com/article/detail/1494815.html&quot;, &quot;snippet&quot;: &quot;在高端芯片禁运的情况下，DeepSeek V3靠着往年囤积的“阉割版”H卡，用区区五百万美元，在惊人的不到三百万GPU小时里完成了预训练，获得了聊天机器人竞技场（ ...&quot;, &quot;date&quot;: &quot;3 hours ago&quot;, &quot;position&quot;: 1}]&#x27;, name=&#x27;fetch_real_time_info&#x27;, tool_call_id=&#x27;tool_call_id&#x27;)]}
```

同时，如果将多个工具调用同时传递给AIMessage的tool_calls参数，仍然可以使用ToolNode进行并行工具调用：

```
@tool
def get_weather(location):
    &quot;&quot;&quot;Call to get the current weather.&quot;&quot;&quot;
    if location.lower() in [&quot;beijing&quot;]:
        return &quot;北京的温度是16度，天气晴朗。&quot;
    elif location.lower() in [&quot;shanghai&quot;]:
        return &quot;上海的温度是20度，部分多云。&quot;
    else:
        return &quot;不好意思，并未查询到具体的天气信息。&quot;
```

```
tools = [fetch_real_time_info, get_weather]
tool_node = ToolNode(tools)
```

```
tool_node
```

```
tools(tags=None, recurse=True, func_accepts_config=True, func_accepts={&#x27;writer&#x27;: False, &#x27;store&#x27;: True}, tools_by_name={&#x27;fetch_real_time_info&#x27;: StructuredTool(name=&#x27;fetch_real_time_info&#x27;, description=&#x27;Get real-time Internet information&#x27;, args_schema=&lt;class &#x27;langchain_core.utils.pydantic.fetch_real_time_info&#x27;&gt;, func=&lt;function fetch_real_time_info at 0x0000023B318F45E0&gt;), &#x27;get_weather&#x27;: StructuredTool(name=&#x27;get_weather&#x27;, description=&#x27;Call to get the current weather.&#x27;, args_schema=&lt;class &#x27;langchain_core.utils.pydantic.get_weather&#x27;&gt;, func=&lt;function get_weather at 0x0000023B36036A20&gt;)}, tool_to_state_args={&#x27;fetch_real_time_info&#x27;: {}, &#x27;get_weather&#x27;: {}}, tool_to_store_arg={&#x27;fetch_real_time_info&#x27;: None, &#x27;get_weather&#x27;: None}, handle_tool_errors=True, messages_key=&#x27;messages&#x27;)
```

```
message_with_multiple_tool_calls = AIMessage(
    content=&quot;&quot;,
    tool_calls=[
        {
            &quot;name&quot;: &quot;fetch_real_time_info&quot;,
            &quot;args&quot;: {&quot;query&quot;: &quot;DeepSeek v3 最新的新闻&quot;},
            &quot;id&quot;: &quot;tool_call_id&quot;,
            &quot;type&quot;: &quot;tool_call&quot;,
        },
        {
            &quot;name&quot;: &quot;get_weather&quot;,
            &quot;args&quot;: {&quot;location&quot;: &quot;beijing&quot;},
            &quot;id&quot;: &quot;tool_call_id_2&quot;,
            &quot;type&quot;: &quot;tool_call&quot;,
        },
    ],
)

tool_node.invoke({&quot;messages&quot;: [message_with_multiple_tool_calls]})
```

```
{&#x27;messages&#x27;: [ToolMessage(content=&#x27;[{&quot;title&quot;: &quot;DeepSeek崛起AI经济模型或将开启全面重构 - 证券时报&quot;, &quot;link&quot;: &quot;https://www.stcn.com/article/detail/1494815.html&quot;, &quot;snippet&quot;: &quot;在高端芯片禁运的情况下，DeepSeek V3靠着往年囤积的“阉割版”H卡，用区区五百万美元，在惊人的不到三百万GPU小时里完成了预训练，获得了聊天机器人竞技场（ ...&quot;, &quot;date&quot;: &quot;3 hours ago&quot;, &quot;position&quot;: 1}]&#x27;, name=&#x27;fetch_real_time_info&#x27;, tool_call_id=&#x27;tool_call_id&#x27;),
  ToolMessage(content=&#x27;北京的温度是16度，天气晴朗。&#x27;, name=&#x27;get_weather&#x27;, tool_call_id=&#x27;tool_call_id_2&#x27;)]}
```

而Tool Calling Agent的本质原理是：让大模型根据用户的输入，自动的去判断应该使用哪个函数，并实际的执行，最后结合工具的响应结果 + 用户的原始问题作为完整的Prompt生成最终的问题。即如下图所示：

![](img/f410e735c8816992083c45afc8b1c0fc.png)

通过ToolNode(tools)可以根据参数来执行函数，并返回结果。而其前一步，根据自然语言生成执行具体某个函数必要参数的过程，则由大模型决定，所以一个完整的基于大模型的工具调用过程应该是，在实例化大模型的时候，就告诉大模型你都有哪些工具可以使用。这个过程可以通过bind_tools函数来实现。代码如下：

```
from langchain_openai import ChatOpenAI

model = ChatOpenAI(model=&quot;deepseek-chat&quot;,
                   api_key=&#x27;sk-98f37d04e1374105b31cb994ae0c9c96&#x27;,
                   base_url=&#x27;https://api.deepseek.com&#x27;)
```

```
tools = [fetch_real_time_info, get_weather]
model_with_tools = model.bind_tools(tools)
```

```
model_with_tools
```

```
RunnableBinding(bound=ChatOpenAI(client=&lt;openai.resources.chat.completions.Completions object at 0x0000023B3603D310&gt;, async_client=&lt;openai.resources.chat.completions.AsyncCompletions object at 0x0000023B36012F50&gt;, root_client=&lt;openai.OpenAI object at 0x0000023B3603FE50&gt;, root_async_client=&lt;openai.AsyncOpenAI object at 0x0000023B36013DD0&gt;, model_name=&#x27;deepseek-chat&#x27;, model_kwargs={}, openai_api_key=SecretStr(&#x27;**********&#x27;), openai_api_base=&#x27;https://api.deepseek.com&#x27;), kwargs={&#x27;tools&#x27;: [{&#x27;type&#x27;: &#x27;function&#x27;, &#x27;function&#x27;: {&#x27;name&#x27;: &#x27;fetch_real_time_info&#x27;, &#x27;description&#x27;: &#x27;Get real-time Internet information&#x27;, &#x27;parameters&#x27;: {&#x27;properties&#x27;: {&#x27;query&#x27;: {}}, &#x27;required&#x27;: [&#x27;query&#x27;], &#x27;type&#x27;: &#x27;object&#x27;}}}, {&#x27;type&#x27;: &#x27;function&#x27;, &#x27;function&#x27;: {&#x27;name&#x27;: &#x27;get_weather&#x27;, &#x27;description&#x27;: &#x27;Call to get the current weather.&#x27;, &#x27;parameters&#x27;: {&#x27;properties&#x27;: {&#x27;location&#x27;: {}}, &#x27;required&#x27;: [&#x27;location&#x27;], &#x27;type&#x27;: &#x27;object&#x27;}}}]}, config={}, config_factories=[])
```

```
model_with_tools.kwargs
```

```
{&#x27;tools&#x27;: [{&#x27;type&#x27;: &#x27;function&#x27;,
   &#x27;function&#x27;: {&#x27;name&#x27;: &#x27;fetch_real_time_info&#x27;,
    &#x27;description&#x27;: &#x27;Get real-time Internet information&#x27;,
    &#x27;parameters&#x27;: {&#x27;properties&#x27;: {&#x27;query&#x27;: {}},
     &#x27;required&#x27;: [&#x27;query&#x27;],
     &#x27;type&#x27;: &#x27;object&#x27;}}},
  {&#x27;type&#x27;: &#x27;function&#x27;,
   &#x27;function&#x27;: {&#x27;name&#x27;: &#x27;get_weather&#x27;,
    &#x27;description&#x27;: &#x27;Call to get the current weather.&#x27;,
    &#x27;parameters&#x27;: {&#x27;properties&#x27;: {&#x27;location&#x27;: {}},
     &#x27;required&#x27;: [&#x27;location&#x27;],
     &#x27;type&#x27;: &#x27;object&#x27;}}}]}
```

```
model_with_tools.invoke(&quot;DeepSeek v3 最新的新闻&quot;).tool_calls
```

```
[{&#x27;name&#x27;: &#x27;fetch_real_time_info&#x27;,
  &#x27;args&#x27;: {&#x27;query&#x27;: &#x27;DeepSeek v3 最新新闻&#x27;},
  &#x27;id&#x27;: &#x27;call_0_ddc20394-315e-452d-ad61-4268102e5ffe&#x27;,
  &#x27;type&#x27;: &#x27;tool_call&#x27;}]
```

```
model_with_tools.invoke(&quot;北京现在多少度呀？&quot;).tool_calls
```

```
[{&#x27;name&#x27;: &#x27;get_weather&#x27;,
  &#x27;args&#x27;: {&#x27;location&#x27;: &#x27;北京&#x27;},
  &#x27;id&#x27;: &#x27;call_0_35200dd5-423b-4844-a2a9-4de463b66da6&#x27;,
  &#x27;type&#x27;: &#x27;tool_call&#x27;}]
```

由此可见，借助大模型可以正确的填充tool_calls信息 ，因此我们可以将其直接传递给ToolNode，从而完成完整的Tool Calling Agent链路。代码如下：

```
tool_node.invoke({&quot;messages&quot;: [model_with_tools.invoke(&quot;DeepSeek v3&quot;)]})
```

```
{&#x27;messages&#x27;: [ToolMessage(content=&#x27;[{&quot;title&quot;: &quot;DeepSeek-V3&quot;, &quot;link&quot;: &quot;https://www.deepseek.com/&quot;, &quot;snippet&quot;: &quot;DeepSeek-V3 is now fully available, with leading performance and improved speed. Click to view details. Brand-new experience, redefining possibilities.&quot;, &quot;sitelinks&quot;: [{&quot;title&quot;: &quot;DeepSeek Platform&quot;, &quot;link&quot;: &quot;https://platform.deepseek.com/&quot;}, {&quot;title&quot;: &quot;DeepSeek Service Status&quot;, &quot;link&quot;: &quot;https://status.deepseek.com/&quot;}, {&quot;title&quot;: &quot;DeepSeek API Upgrade&quot;, &quot;link&quot;: &quot;https://api-docs.deepseek.com/news/news0725&quot;}], &quot;position&quot;: 1}]&#x27;, name=&#x27;fetch_real_time_info&#x27;, tool_call_id=&#x27;call_0_d9c37e37-1292-4597-9277-5a51017040bd&#x27;)]}
```

```
tool_node.invoke({&quot;messages&quot;: [model_with_tools.invoke(&quot;北京现在多少度呀&quot;)]})
```

```
{&#x27;messages&#x27;: [ToolMessage(content=&#x27;不好意思，并未查询到具体的天气信息。&#x27;, name=&#x27;get_weather&#x27;, tool_call_id=&#x27;call_0_a237fa85-0f43-4a67-b922-1a581850e40d&#x27;)]}
```

## 2.2 Tool Calling Agent的完整实现案例

接下来，我们来构建完整的Tool Calling Agent。这里我们对Router Agent实现的图做进一步的升级，即用户输入问题后，如果不需要外部工具的信息，则直接生成回复，否则，则进入一个工具库中，选择最合适的工具执行，并返回最终的响应。因此，我们首先来定义工具库：

+   Step 2. 构建Mysql后台数据

首先，我们需要构造一份商家后台的模拟数据。那么，如何存储这些数据呢？对于结构化数据，通常使用关系型数据库来进行存储。常见的选择包括 SQLite、MySQL 和 MongoDB 等。我们这里选择使用 MySQL 作为数据存储的方案，然后再借助pymysql库在本地Python环境中进行连接。关于在Windows安装Mysql服务的过程，我们就不在直播过程中进行讲解，大家可以添加专属助理领取非常详细的安装教程：

![](img/45b725c9053d888f5ec3a802db1126b1.png)

按照上述教程安装完成并启动Mysql服务后，我们可以使用一些可视化的工[具来进行直观的测试](https://www.mysql.com/products/workbench/)连接[。常用的像 👉 ](https://dbeaver.io/)w[orkbenc](https://www.navicat.com/en/)h、 DBeaver 、Navicat等，大家按个人喜好选择就行。安装的方法非常简单，点击链接选择对应自己操作系统的版本执行傻瓜式安装即可。我们这里通过Navicat进行演示。

首先启动 Navicat 客户端，进入主界面，创建新的连接： 在 Navicat 主界面左上方，点击 “连接” 按钮，选择 MySQL：

![](img/b82d6fb20c50004b71018879f6652cbb.png)

在弹出的连接设置窗口中，填写你的 MySQL 数据库连接信息，其中：

+   连接名称：给你的连接起个名字，方便识别（例如：智能客服系统）。

+   主机名/IP 地址：如果安装在本地计算机上，可以填写 localhost 或 127.0.0.1。

+   端口：默认情况下，MySQL 使用端口 3306，除非你有修改，保持默认即可。

+   用户名：输入你的 MySQL 用户名（例如：root，或者你设置的其他用户名）。

+   密码：输入对应的密码（如果是 root 用户，默认是你安装时设置的密码）。

填写完连接信息后，点击窗口下方的 “测试连接” 按钮。如果连接成功，Navicat 会显示 "连接成功" 的提示。如果连接失败，则需要上述配置是否填写正确。

![](img/8d7cba510e24ee363d98e62873660e6a.png)

最后，如果测试连接成功，点击 “确定” 按钮，Navicat 会保存连接并尝试连接到你的 MySQL 数据库，并显示在左侧的数据库列表，至此就可以选择并管理该数据库了。

![](img/fc6a4ec6aa997cf05829102974398f9e.png)

接下来，我们需要使用Python来连接本地的Mysql数据库，同时创建education_db库并插入生成的模拟数据，代码如下：

```
import pymysql

def create_and_populate_mysql():
    # 连接到MySQL服务器（未指定数据库）
    conn_mysql = pymysql.connect(
        host=&#x27;localhost&#x27;,         # 这里替换成你自己的 MySQL 主机地址
        user=&#x27;root&#x27;,              # 这里替换成你自己的用户名
        password=&#x27;snowball2019&#x27;,  # 这里替换成你自己的密码
        charset=&#x27;utf8mb4&#x27;         # 字符集
    )
    cursor_mysql = conn_mysql.cursor()

    # 创建数据库 education_db（如果不存在）
    cursor_mysql.execute(&quot;CREATE DATABASE IF NOT EXISTS education_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci&quot;)

    # 切换到 education_db 数据库
    cursor_mysql.execute(&#x27;USE education_db;&#x27;)

    # 创建课程信息表
    create_courses_table_query = &#x27;&#x27;&#x27;
    CREATE TABLE IF NOT EXISTS courses (
        course_id VARCHAR(20),                  -- 课程ID
        course_name VARCHAR(100) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci,  -- 课程名称
        course_category VARCHAR(50) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci,  -- 课程类别
        chapter_number INT,                      -- 章节编号
        chapter_name VARCHAR(100) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci,  -- 章节名称
        duration INT,                            -- 课程时长（小时）
        keywords VARCHAR(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci,   -- 关键词
        PRIMARY KEY (course_id, chapter_number)   -- 组合主键（课程ID + 章节编号）
    );
    &#x27;&#x27;&#x27;
    cursor_mysql.execute(create_courses_table_query)

    # 插入示例数据
    insert_data_query = &#x27;&#x27;&#x27;
    INSERT INTO courses (course_id, course_name, course_category, chapter_number, chapter_name, duration, keywords)
    VALUES
    (&#x27;ML001&#x27;, &#x27;机器学习基础&#x27;, &#x27;机器学习&#x27;, 1, &#x27;机器学习简介&#x27;, 4, &#x27;监督学习,无监督学习&#x27;),
    (&#x27;ML001&#x27;, &#x27;机器学习基础&#x27;, &#x27;机器学习&#x27;, 2, &#x27;线性回归&#x27;, 6, &#x27;回归分析,模型训练&#x27;),
    (&#x27;DL001&#x27;, &#x27;深度学习概论&#x27;, &#x27;深度学习&#x27;, 1, &#x27;神经网络基础&#x27;, 5, &#x27;神经网络,激活函数&#x27;),
    (&#x27;DL001&#x27;, &#x27;深度学习概论&#x27;, &#x27;深度学习&#x27;, 2, &#x27;卷积神经网络&#x27;, 6, &#x27;CNN,图像识别&#x27;),
    (&#x27;LM001&#x27;, &#x27;大模型智能体开发&#x27;, &#x27;大模型智能体开发&#x27;, 1, &#x27;智能体基础&#x27;, 5, &#x27;Agent,FunctionCalling&#x27;),
    (&#x27;LM001&#x27;, &#x27;大模型智能体开发&#x27;, &#x27;大模型智能体开发&#x27;, 2, &#x27;本地知识库问答&#x27;, 7, &#x27;RAG,VectorStore&#x27;),
    (&#x27;MLP001&#x27;, &#x27;大模型原理&#x27;, &#x27;大模型原理&#x27;, 1, &#x27;大模型概念&#x27;, 4, &#x27;大模型,计算资源&#x27;),
    (&#x27;MLP001&#x27;, &#x27;大模型原理&#x27;, &#x27;大模型原理&#x27;, 2, &#x27;大规模训练&#x27;, 6, &#x27;分布式训练,并行计算&#x27;),
    (&#x27;CI001&#x27;, &#x27;因果推断导论&#x27;, &#x27;因果推断&#x27;, 1, &#x27;因果关系与相关性&#x27;, 5, &#x27;因果推断,回归分析&#x27;),
    (&#x27;CI001&#x27;, &#x27;因果推断导论&#x27;, &#x27;因果推断&#x27;, 2, &#x27;随机化实验设计&#x27;, 5, &#x27;实验设计,干预实验&#x27;);
    &#x27;&#x27;&#x27;
    try:
        cursor_mysql.execute(insert_data_query)
        conn_mysql.commit()  # 提交事务
        print(&quot;数据插入成功！&quot;)
    except pymysql.MySQLError as e:
        print(f&quot;插入数据时出错：{e}&quot;)
        conn_mysql.rollback()  # 回滚事务

    # 提交更改并关闭连接
    conn_mysql.close()

# 调用函数创建数据库并插入数据
create_and_populate_mysql()
```

```
数据插入成功！
```

上述代码的执行过程是：首先，程序通过 pymysql 连接到本地的 MySQL 服务器，并检查是否存在名为 education_db 的数据库。如果该数据库不存在，程序会创建一个新的数据库 education_db。然后，切换到该数据库并检查是否已经创建了名为 courses 的表，如果没有，会自动创建此表。接着，将一组预设的产品数据插入到 courses 表中。插入完成后，提交这些更改，并关闭数据库连接，最后输出提示信息，表示数据库和数据已经成功创建并插入 MySQL 中。接下来我们执行这个函数以生成模拟数据：

执行完上述函数后，将在本地 MySQL 数据库中创建一个名为 courses 的表。

![](img/4f9fd09a1211799b124fbc1640bebdb8.png)

如果希望根据关键字来搜索数据，可以通过在查询中使用 LIKE 操作符来查找包含特定关键字的记录。我们可以对 SQL 查询进行修改，让它支持根据 keywords 字段进行搜索。

```
def query_by_keyword(keyword):
    # 连接 MySQL 数据库
    conn_mysql = pymysql.connect(
        host=&#x27;localhost&#x27;,
        user=&#x27;root&#x27;,
        password=&#x27;snowball2019&#x27;,
        database=&#x27;education_db&#x27;,
        charset=&#x27;utf8mb4&#x27;
    )
    cursor_mysql = conn_mysql.cursor()

    # 使用 SQL 查询按关键字查找课程。&#x27;%&#x27;符号允许部分匹配。
    cursor_mysql.execute(&quot;SELECT * FROM courses WHERE LOWER(keywords) LIKE %s&quot;, (&#x27;%&#x27; + keyword.lower() + &#x27;%&#x27;,))

    # 获取所有查询到的数据
    rows = cursor_mysql.fetchall()

    # 字段说明
    field_descriptions = {
        &#x27;course_id&#x27;: &#x27;课程唯一标识符&#x27;,
        &#x27;course_name&#x27;: &#x27;课程名称&#x27;,
        &#x27;course_category&#x27;: &#x27;课程所属类别&#x27;,
        &#x27;chapter_number&#x27;: &#x27;章节的编号&#x27;,
        &#x27;chapter_name&#x27;: &#x27;章节名称&#x27;,
        &#x27;duration&#x27;: &#x27;课程时长（小时）&#x27;,
        &#x27;keywords&#x27;: &#x27;与课程相关的关键词&#x27;
    }

    # 关闭连接
    conn_mysql.close()

    # 返回数据和字段说明
    return {
        &#x27;data&#x27;: rows,
        &#x27;field_descriptions&#x27;: field_descriptions
    }
```

```
query_by_keyword(&quot;Agent&quot;)
```

```
{&#x27;data&#x27;: ((&#x27;LM001&#x27;,
   &#x27;大模型智能体开发&#x27;,
   &#x27;大模型智能体开发&#x27;,
   1,
   &#x27;智能体基础&#x27;,
   5,
   &#x27;Agent,FunctionCalling&#x27;),),
 &#x27;field_descriptions&#x27;: {&#x27;course_id&#x27;: &#x27;课程唯一标识符&#x27;,
  &#x27;course_name&#x27;: &#x27;课程名称&#x27;,
  &#x27;course_category&#x27;: &#x27;课程所属类别&#x27;,
  &#x27;chapter_number&#x27;: &#x27;章节的编号&#x27;,
  &#x27;chapter_name&#x27;: &#x27;章节名称&#x27;,
  &#x27;duration&#x27;: &#x27;课程时长（小时）&#x27;,
  &#x27;keywords&#x27;: &#x27;与课程相关的关键词&#x27;}}
```

将其封装成大模型可以调用的外部工具。

```
@tool
def fetch_real_time_info(query):
    &quot;&quot;&quot;Get real-time Internet information&quot;&quot;&quot;
    url = &quot;https://google.serper.dev/search&quot;
    payload = json.dumps({
      &quot;q&quot;: query,
      &quot;num&quot;: 1,
    })
    headers = {
      &#x27;X-API-KEY&#x27;: &#x27;d9c2c002085468d95712cbc71d76c4302b4f7342&#x27;,
      &#x27;Content-Type&#x27;: &#x27;application/json&#x27;
    }
    
    response = requests.post(url, headers=headers, data=payload)
    data = json.loads(response.text)  # 将返回的JSON字符串转换为字典
    if &#x27;organic&#x27; in data:
        return json.dumps(data[&#x27;organic&#x27;],  ensure_ascii=False)  # 返回&#x27;organic&#x27;部分的JSON字符串
    else:
        return json.dumps({&quot;error&quot;: &quot;No organic results found&quot;},  ensure_ascii=False)  # 如果没有&#x27;organic&#x27;键，返回错误信息

@tool
def query_by_keyword(keyword):
    &quot;&quot;&quot;Search course details in local database according to keywords&quot;&quot;&quot;
    # 连接 MySQL 数据库
    conn_mysql = pymysql.connect(
        host=&#x27;localhost&#x27;,
        user=&#x27;root&#x27;,
        password=&#x27;snowball2019&#x27;,
        database=&#x27;education_db&#x27;,
        charset=&#x27;utf8mb4&#x27;
    )
    cursor_mysql = conn_mysql.cursor()

    # 使用 SQL 查询按关键字查找课程。&#x27;%&#x27;符号允许部分匹配。
    cursor_mysql.execute(&quot;SELECT * FROM courses WHERE LOWER(keywords) LIKE %s&quot;, (&#x27;%&#x27; + keyword.lower() + &#x27;%&#x27;,))

    # 获取所有查询到的数据
    rows = cursor_mysql.fetchall()

    # 字段说明
    field_descriptions = {
        &#x27;course_id&#x27;: &#x27;课程唯一标识符&#x27;,
        &#x27;course_name&#x27;: &#x27;课程名称&#x27;,
        &#x27;course_category&#x27;: &#x27;课程所属类别&#x27;,
        &#x27;chapter_number&#x27;: &#x27;章节的编号&#x27;,
        &#x27;chapter_name&#x27;: &#x27;章节名称&#x27;,
        &#x27;duration&#x27;: &#x27;课程时长（小时）&#x27;,
        &#x27;keywords&#x27;: &#x27;与课程相关的关键词&#x27;
    }

    # 关闭连接
    conn_mysql.close()

    # 返回数据和字段说明
    return {
        &#x27;data&#x27;: rows,
        &#x27;field_descriptions&#x27;: field_descriptions
    }
```

```
tools = [query_by_keyword, fetch_real_time_info, get_weather]
tool_node = ToolNode(tools)
```

```
tool_node
```

```
tools(tags=None, recurse=True, func_accepts_config=True, func_accepts={&#x27;writer&#x27;: False, &#x27;store&#x27;: True}, tools_by_name={&#x27;query_by_keyword&#x27;: StructuredTool(name=&#x27;query_by_keyword&#x27;, description=&#x27;Search course details in local database according to keywords&#x27;, args_schema=&lt;class &#x27;langchain_core.utils.pydantic.query_by_keyword&#x27;&gt;, func=&lt;function query_by_keyword at 0x0000023B37271D00&gt;), &#x27;fetch_real_time_info&#x27;: StructuredTool(name=&#x27;fetch_real_time_info&#x27;, description=&#x27;Get real-time Internet information&#x27;, args_schema=&lt;class &#x27;langchain_core.utils.pydantic.fetch_real_time_info&#x27;&gt;, func=&lt;function fetch_real_time_info at 0x0000023B37271BC0&gt;), &#x27;get_weather&#x27;: StructuredTool(name=&#x27;get_weather&#x27;, description=&#x27;Call to get the current weather.&#x27;, args_schema=&lt;class &#x27;langchain_core.utils.pydantic.get_weather&#x27;&gt;, func=&lt;function get_weather at 0x0000023B36036A20&gt;)}, tool_to_state_args={&#x27;query_by_keyword&#x27;: {}, &#x27;fetch_real_time_info&#x27;: {}, &#x27;get_weather&#x27;: {}}, tool_to_store_arg={&#x27;query_by_keyword&#x27;: None, &#x27;fetch_real_time_info&#x27;: None, &#x27;get_weather&#x27;: None}, handle_tool_errors=True, messages_key=&#x27;messages&#x27;)
```

```
from langchain_openai import ChatOpenAI

model = ChatOpenAI(model=&quot;deepseek-chat&quot;,
                   api_key=&#x27;sk-98f37d04e1374105b31cb994ae0c9c96&#x27;,
                   base_url=&#x27;https://api.deepseek.com&#x27;)

llm = model.bind_tools(tools)
```

```
from langgraph.prebuilt import create_react_agent

graph = create_react_agent(llm, tools=tools)
```

```
from IPython.display import Image, display

display(Image(graph.get_graph().draw_mermaid_png()))
```

![](img/8eebd571369babedf6c9aa27edc8ec3f.png)

理解了上面的create_react_agent方法内部的构建原理后，其实就能明白：当通过create_react_agent(llm, tools=tools)一行代码的执行，现在得到的已经是一个编译后、可执行的图了。我们可以通过mermaid方法来可视化经过create_react_agent方法构造出来的图结构，代码如下所示：

返回的是编译好的LangGraph可运行程序，可直接用于聊天交互。调用方式则和之前使用的方法一样，我们可以依次针对不同复杂程度的需求依次进行提问。首先是测试是否可以不使用工具，直接调用大模型生成响应。

```
# query=&quot;你好，请你介绍一下你自己&quot;
# input_message = {&quot;messages&quot;: [HumanMessage(content=query)]}

# 可以自动处理成 HumanMessage 的消息格式
finan_response = graph.invoke({&quot;messages&quot;:[&quot;你好，请你介绍一下你自己&quot;]})
finan_response
```

```
{&#x27;messages&#x27;: [HumanMessage(content=&#x27;你好，请你介绍一下你自己&#x27;, additional_kwargs={}, response_metadata={}, id=&#x27;4a4f31e2-1cf4-478d-9b84-088a88476d26&#x27;),
  AIMessage(content=&#x27;你好！我是一个人工智能助手，专门设计来帮助用户获取信息、解答问题、提供建议和执行各种任务。我可以帮助你查找资料、学习新知识、规划日程、解决问题等。无论你有什么问题或需要帮助，我都会尽力提供支持。请问有什么我可以帮你的吗？&#x27;, additional_kwargs={&#x27;refusal&#x27;: None}, response_metadata={&#x27;token_usage&#x27;: {&#x27;completion_tokens&#x27;: 57, &#x27;prompt_tokens&#x27;: 246, &#x27;total_tokens&#x27;: 303, &#x27;completion_tokens_details&#x27;: None, &#x27;prompt_tokens_details&#x27;: None, &#x27;prompt_cache_hit_tokens&#x27;: 192, &#x27;prompt_cache_miss_tokens&#x27;: 54}, &#x27;model_name&#x27;: &#x27;deepseek-chat&#x27;, &#x27;system_fingerprint&#x27;: &#x27;fp_3a5770e1b4&#x27;, &#x27;finish_reason&#x27;: &#x27;stop&#x27;, &#x27;logprobs&#x27;: None}, id=&#x27;run-a0dd0b3d-7414-4e79-b491-26be1e967b7b-0&#x27;, usage_metadata={&#x27;input_tokens&#x27;: 246, &#x27;output_tokens&#x27;: 57, &#x27;total_tokens&#x27;: 303, &#x27;input_token_details&#x27;: {}, &#x27;output_token_details&#x27;: {}})]}
```

```
finan_response[&quot;messages&quot;][-1].content
```

```
&#x27;你好！我是一个人工智能助手，专门设计来帮助用户获取信息、解答问题、提供建议和执行各种任务。我可以帮助你查找资料、学习新知识、规划日程、解决问题等。无论你有什么问题或需要帮助，我都会尽力提供支持。请问有什么我可以帮你的吗？&#x27;
```

加大输入问题的复杂度，接下来我们提问的问题希望它能够自动找到正确的工具函数，基于工具的执行结果作为既定的事实，引导生成最终的回复。

```
finan_response = graph.invoke({&quot;messages&quot;:[&quot;课程中有没有Agent相关的内容？&quot;]})

finan_response
```

```
{&#x27;messages&#x27;: [HumanMessage(content=&#x27;课程中有没有Agent相关的内容？&#x27;, additional_kwargs={}, response_metadata={}, id=&#x27;11cfbacf-359d-41f7-ab28-9d4824d0f53c&#x27;),
  AIMessage(content=&#x27;&#x27;, additional_kwargs={&#x27;tool_calls&#x27;: [{&#x27;id&#x27;: &#x27;call_0_0f7ef5dd-b42a-45fd-afd8-2e9164532ef5&#x27;, &#x27;function&#x27;: {&#x27;arguments&#x27;: &#x27;{&quot;keyword&quot;:&quot;Agent&quot;}&#x27;, &#x27;name&#x27;: &#x27;query_by_keyword&#x27;}, &#x27;type&#x27;: &#x27;function&#x27;, &#x27;index&#x27;: 0}], &#x27;refusal&#x27;: None}, response_metadata={&#x27;token_usage&#x27;: {&#x27;completion_tokens&#x27;: 20, &#x27;prompt_tokens&#x27;: 248, &#x27;total_tokens&#x27;: 268, &#x27;completion_tokens_details&#x27;: None, &#x27;prompt_tokens_details&#x27;: None, &#x27;prompt_cache_hit_tokens&#x27;: 192, &#x27;prompt_cache_miss_tokens&#x27;: 56}, &#x27;model_name&#x27;: &#x27;deepseek-chat&#x27;, &#x27;system_fingerprint&#x27;: &#x27;fp_3a5770e1b4&#x27;, &#x27;finish_reason&#x27;: &#x27;tool_calls&#x27;, &#x27;logprobs&#x27;: None}, id=&#x27;run-0ec27a86-b645-441e-95d8-f75d538741b2-0&#x27;, tool_calls=[{&#x27;name&#x27;: &#x27;query_by_keyword&#x27;, &#x27;args&#x27;: {&#x27;keyword&#x27;: &#x27;Agent&#x27;}, &#x27;id&#x27;: &#x27;call_0_0f7ef5dd-b42a-45fd-afd8-2e9164532ef5&#x27;, &#x27;type&#x27;: &#x27;tool_call&#x27;}], usage_metadata={&#x27;input_tokens&#x27;: 248, &#x27;output_tokens&#x27;: 20, &#x27;total_tokens&#x27;: 268, &#x27;input_token_details&#x27;: {}, &#x27;output_token_details&#x27;: {}}),
  ToolMessage(content=&#x27;{&quot;data&quot;: [[&quot;LM001&quot;, &quot;大模型智能体开发&quot;, &quot;大模型智能体开发&quot;, 1, &quot;智能体基础&quot;, 5, &quot;Agent,FunctionCalling&quot;]], &quot;field_descriptions&quot;: {&quot;course_id&quot;: &quot;课程唯一标识符&quot;, &quot;course_name&quot;: &quot;课程名称&quot;, &quot;course_category&quot;: &quot;课程所属类别&quot;, &quot;chapter_number&quot;: &quot;章节的编号&quot;, &quot;chapter_name&quot;: &quot;章节名称&quot;, &quot;duration&quot;: &quot;课程时长（小时）&quot;, &quot;keywords&quot;: &quot;与课程相关的关键词&quot;}}&#x27;, name=&#x27;query_by_keyword&#x27;, id=&#x27;e205b29f-0a80-48ee-868f-6aeb6b29483d&#x27;, tool_call_id=&#x27;call_0_0f7ef5dd-b42a-45fd-afd8-2e9164532ef5&#x27;),
  AIMessage(content=&#x27;&#x27;, additional_kwargs={&#x27;tool_calls&#x27;: [{&#x27;id&#x27;: &#x27;call_0_4c7cfd4a-0704-4d9c-a798-99e49550e58a&#x27;, &#x27;function&#x27;: {&#x27;arguments&#x27;: &#x27;{&quot;keyword&quot;: &quot;智能体&quot;}&#x27;, &#x27;name&#x27;: &#x27;query_by_keyword&#x27;}, &#x27;type&#x27;: &#x27;function&#x27;, &#x27;index&#x27;: 0}], &#x27;refusal&#x27;: None}, response_metadata={&#x27;token_usage&#x27;: {&#x27;completion_tokens&#x27;: 22, &#x27;prompt_tokens&#x27;: 385, &#x27;total_tokens&#x27;: 407, &#x27;completion_tokens_details&#x27;: None, &#x27;prompt_tokens_details&#x27;: None, &#x27;prompt_cache_hit_tokens&#x27;: 192, &#x27;prompt_cache_miss_tokens&#x27;: 193}, &#x27;model_name&#x27;: &#x27;deepseek-chat&#x27;, &#x27;system_fingerprint&#x27;: &#x27;fp_3a5770e1b4&#x27;, &#x27;finish_reason&#x27;: &#x27;tool_calls&#x27;, &#x27;logprobs&#x27;: None}, id=&#x27;run-76d4ffcf-7dcc-4efd-bce9-80e84cab5830-0&#x27;, tool_calls=[{&#x27;name&#x27;: &#x27;query_by_keyword&#x27;, &#x27;args&#x27;: {&#x27;keyword&#x27;: &#x27;智能体&#x27;}, &#x27;id&#x27;: &#x27;call_0_4c7cfd4a-0704-4d9c-a798-99e49550e58a&#x27;, &#x27;type&#x27;: &#x27;tool_call&#x27;}], usage_metadata={&#x27;input_tokens&#x27;: 385, &#x27;output_tokens&#x27;: 22, &#x27;total_tokens&#x27;: 407, &#x27;input_token_details&#x27;: {}, &#x27;output_token_details&#x27;: {}}),
  ToolMessage(content=&#x27;{&quot;data&quot;: [], &quot;field_descriptions&quot;: {&quot;course_id&quot;: &quot;课程唯一标识符&quot;, &quot;course_name&quot;: &quot;课程名称&quot;, &quot;course_category&quot;: &quot;课程所属类别&quot;, &quot;chapter_number&quot;: &quot;章节的编号&quot;, &quot;chapter_name&quot;: &quot;章节名称&quot;, &quot;duration&quot;: &quot;课程时长（小时）&quot;, &quot;keywords&quot;: &quot;与课程相关的关键词&quot;}}&#x27;, name=&#x27;query_by_keyword&#x27;, id=&#x27;97431846-5762-4ccb-9a4f-27be4245a4c2&#x27;, tool_call_id=&#x27;call_0_4c7cfd4a-0704-4d9c-a798-99e49550e58a&#x27;),
  AIMessage(content=&#x27;&#x27;, additional_kwargs={&#x27;tool_calls&#x27;: [{&#x27;id&#x27;: &#x27;call_0_0d4b96b6-2c12-4c76-8868-4b4a84bdc1c9&#x27;, &#x27;function&#x27;: {&#x27;arguments&#x27;: &#x27;{&quot;keyword&quot;: &quot;FunctionCalling&quot;}&#x27;, &#x27;name&#x27;: &#x27;query_by_keyword&#x27;}, &#x27;type&#x27;: &#x27;function&#x27;, &#x27;index&#x27;: 0}], &#x27;refusal&#x27;: None}, response_metadata={&#x27;token_usage&#x27;: {&#x27;completion_tokens&#x27;: 45, &#x27;prompt_tokens&#x27;: 494, &#x27;total_tokens&#x27;: 539, &#x27;completion_tokens_details&#x27;: None, &#x27;prompt_tokens_details&#x27;: None, &#x27;prompt_cache_hit_tokens&#x27;: 384, &#x27;prompt_cache_miss_tokens&#x27;: 110}, &#x27;model_name&#x27;: &#x27;deepseek-chat&#x27;, &#x27;system_fingerprint&#x27;: &#x27;fp_3a5770e1b4&#x27;, &#x27;finish_reason&#x27;: &#x27;tool_calls&#x27;, &#x27;logprobs&#x27;: None}, id=&#x27;run-500aa448-606f-4c01-98cb-4ad9f556f6dd-0&#x27;, tool_calls=[{&#x27;name&#x27;: &#x27;query_by_keyword&#x27;, &#x27;args&#x27;: {&#x27;keyword&#x27;: &#x27;FunctionCalling&#x27;}, &#x27;id&#x27;: &#x27;call_0_0d4b96b6-2c12-4c76-8868-4b4a84bdc1c9&#x27;, &#x27;type&#x27;: &#x27;tool_call&#x27;}], usage_metadata={&#x27;input_tokens&#x27;: 494, &#x27;output_tokens&#x27;: 45, &#x27;total_tokens&#x27;: 539, &#x27;input_token_details&#x27;: {}, &#x27;output_token_details&#x27;: {}}),
  ToolMessage(content=&#x27;{&quot;data&quot;: [[&quot;LM001&quot;, &quot;大模型智能体开发&quot;, &quot;大模型智能体开发&quot;, 1, &quot;智能体基础&quot;, 5, &quot;Agent,FunctionCalling&quot;]], &quot;field_descriptions&quot;: {&quot;course_id&quot;: &quot;课程唯一标识符&quot;, &quot;course_name&quot;: &quot;课程名称&quot;, &quot;course_category&quot;: &quot;课程所属类别&quot;, &quot;chapter_number&quot;: &quot;章节的编号&quot;, &quot;chapter_name&quot;: &quot;章节名称&quot;, &quot;duration&quot;: &quot;课程时长（小时）&quot;, &quot;keywords&quot;: &quot;与课程相关的关键词&quot;}}&#x27;, name=&#x27;query_by_keyword&#x27;, id=&#x27;9eeb7535-5570-4e72-9834-063dd6d72ebb&#x27;, tool_call_id=&#x27;call_0_0d4b96b6-2c12-4c76-8868-4b4a84bdc1c9&#x27;),
  AIMessage(content=&#x27;根据搜索结果，课程中有与Agent相关的内容。具体来说，课程“大模型智能体开发”中有一个章节名为“智能体基础”，该章节的时长为5小时，关键词包括“Agent”和“FunctionCalling”。这表明该章节涵盖了智能体开发和函数调用等相关内容。&#x27;, additional_kwargs={&#x27;refusal&#x27;: None}, response_metadata={&#x27;token_usage&#x27;: {&#x27;completion_tokens&#x27;: 61, &#x27;prompt_tokens&#x27;: 633, &#x27;total_tokens&#x27;: 694, &#x27;completion_tokens_details&#x27;: None, &#x27;prompt_tokens_details&#x27;: None, &#x27;prompt_cache_hit_tokens&#x27;: 448, &#x27;prompt_cache_miss_tokens&#x27;: 185}, &#x27;model_name&#x27;: &#x27;deepseek-chat&#x27;, &#x27;system_fingerprint&#x27;: &#x27;fp_3a5770e1b4&#x27;, &#x27;finish_reason&#x27;: &#x27;stop&#x27;, &#x27;logprobs&#x27;: None}, id=&#x27;run-f143d9cd-8696-4ef9-8344-5fcb61394a3a-0&#x27;, usage_metadata={&#x27;input_tokens&#x27;: 633, &#x27;output_tokens&#x27;: 61, &#x27;total_tokens&#x27;: 694, &#x27;input_token_details&#x27;: {}, &#x27;output_token_details&#x27;: {}})]}
```

```
finan_response[&quot;messages&quot;][-1].content
```

```
&#x27;根据搜索结果，课程中有与Agent相关的内容。具体来说，课程“大模型智能体开发”中有一个章节名为“智能体基础”，该章节的时长为5小时，关键词包括“Agent”和“FunctionCalling”。这表明该章节涵盖了智能体开发和函数调用等相关内容。&#x27;
```

```
finan_response = graph.invoke({&quot;messages&quot;:[&quot;你知道 OpenAI 最近的动态吗？请用中文回复我&quot;]})

finan_response
```

```
{&#x27;messages&#x27;: [HumanMessage(content=&#x27;你知道 OpenAI 最近的动态吗？请用中文回复我&#x27;, additional_kwargs={}, response_metadata={}, id=&#x27;5dbcb35a-1ae4-478d-b1ba-17cdd19bbeca&#x27;),
  AIMessage(content=&#x27;&#x27;, additional_kwargs={&#x27;tool_calls&#x27;: [{&#x27;id&#x27;: &#x27;call_0_f4ef169a-4a55-4793-ab63-2e0db5742405&#x27;, &#x27;function&#x27;: {&#x27;arguments&#x27;: &#x27;{&quot;query&quot;:&quot;OpenAI 最近动态&quot;}&#x27;, &#x27;name&#x27;: &#x27;fetch_real_time_info&#x27;}, &#x27;type&#x27;: &#x27;function&#x27;, &#x27;index&#x27;: 0}], &#x27;refusal&#x27;: None}, response_metadata={&#x27;token_usage&#x27;: {&#x27;completion_tokens&#x27;: 24, &#x27;prompt_tokens&#x27;: 253, &#x27;total_tokens&#x27;: 277, &#x27;completion_tokens_details&#x27;: None, &#x27;prompt_tokens_details&#x27;: None, &#x27;prompt_cache_hit_tokens&#x27;: 192, &#x27;prompt_cache_miss_tokens&#x27;: 61}, &#x27;model_name&#x27;: &#x27;deepseek-chat&#x27;, &#x27;system_fingerprint&#x27;: &#x27;fp_3a5770e1b4&#x27;, &#x27;finish_reason&#x27;: &#x27;tool_calls&#x27;, &#x27;logprobs&#x27;: None}, id=&#x27;run-7277402f-f673-480d-b6de-83a8ece4035f-0&#x27;, tool_calls=[{&#x27;name&#x27;: &#x27;fetch_real_time_info&#x27;, &#x27;args&#x27;: {&#x27;query&#x27;: &#x27;OpenAI 最近动态&#x27;}, &#x27;id&#x27;: &#x27;call_0_f4ef169a-4a55-4793-ab63-2e0db5742405&#x27;, &#x27;type&#x27;: &#x27;tool_call&#x27;}], usage_metadata={&#x27;input_tokens&#x27;: 253, &#x27;output_tokens&#x27;: 24, &#x27;total_tokens&#x27;: 277, &#x27;input_token_details&#x27;: {}, &#x27;output_token_details&#x27;: {}}),
  ToolMessage(content=&#x27;[{&quot;title&quot;: &quot;OpenAI最新资讯 - 快科技- 驱动之家&quot;, &quot;link&quot;: &quot;https://news.mydrivers.com/tag/openai.htm&quot;, &quot;snippet&quot;: &quot;微软组建新的AI团队瞄准端到端应用开发与部署 · 当地时间周一，美国科技巨头微软宣布，正在组建一个新的部门，专注于开发人工智能(AI)应用程序，并为第三方客户提供工具。&quot;, &quot;position&quot;: 1}]&#x27;, name=&#x27;fetch_real_time_info&#x27;, id=&#x27;0ea50574-a337-47df-a0b7-29a53e6b9132&#x27;, tool_call_id=&#x27;call_0_f4ef169a-4a55-4793-ab63-2e0db5742405&#x27;),
  AIMessage(content=&#x27;OpenAI 最近的动态包括微软组建了一个新的AI团队，专注于开发人工智能应用程序，并为第三方客户提供工具。这一举措显示了AI技术在应用开发和部署方面的重要性和潜力。你可以通过以下链接了解更多详情：[OpenAI最新资讯 - 快科技- 驱动之家](https://news.mydrivers.com/tag/openai.htm)。&#x27;, additional_kwargs={&#x27;refusal&#x27;: None}, response_metadata={&#x27;token_usage&#x27;: {&#x27;completion_tokens&#x27;: 71, &#x27;prompt_tokens&#x27;: 386, &#x27;total_tokens&#x27;: 457, &#x27;completion_tokens_details&#x27;: None, &#x27;prompt_tokens_details&#x27;: None, &#x27;prompt_cache_hit_tokens&#x27;: 384, &#x27;prompt_cache_miss_tokens&#x27;: 2}, &#x27;model_name&#x27;: &#x27;deepseek-chat&#x27;, &#x27;system_fingerprint&#x27;: &#x27;fp_3a5770e1b4&#x27;, &#x27;finish_reason&#x27;: &#x27;stop&#x27;, &#x27;logprobs&#x27;: None}, id=&#x27;run-5eda0277-2afe-45ec-a1fb-848c2007cacf-0&#x27;, usage_metadata={&#x27;input_tokens&#x27;: 386, &#x27;output_tokens&#x27;: 71, &#x27;total_tokens&#x27;: 457, &#x27;input_token_details&#x27;: {}, &#x27;output_token_details&#x27;: {}})]}
```

```
finan_response[&quot;messages&quot;][-1].content
```

```
&#x27;OpenAI 最近的动态包括微软组建了一个新的AI团队，专注于开发人工智能应用程序，并为第三方客户提供工具。这一举措显示了AI技术在应用开发和部署方面的重要性和潜力。你可以通过以下链接了解更多详情：[OpenAI最新资讯 - 快科技- 驱动之家](https://news.mydrivers.com/tag/openai.htm)。&#x27;
```

```
def stream_graph_updates(user_input: str):
    # 初始化一个变量来累积输出
    accumulated_output = []

    for event in graph.stream({&quot;messages&quot;: [(&quot;user&quot;, user_input)]}):
        for value in event.values():
            # 将模型回复的内容添加到累积输出中
            accumulated_output.append(value[&quot;messages&quot;][-1].content)

    # 返回累积的输出
    return accumulated_output

while True:
    try:
        user_input = input(&quot;用户提问: &quot;)
        if user_input.lower() in [&quot;退出&quot;]:
            print(&quot;下次再见！&quot;)
            break

        # 获取累积的输出
        updates = stream_graph_updates(user_input)

        # 打印最后一个输出
        if updates:
            print(&quot;模型回复:&quot;)
            print(updates[-1])
    except:
        break
```

```
用户提问:  你好


模型回复:
你好！请问有什么我可以帮助你的吗？


用户提问:  请问我们有图像识别的课程吗？


模型回复:
我们有关于图像识别的课程。以下是相关的课程信息：

- 课程唯一标识符: DL001
- 课程名称: 深度学习概论
- 课程所属类别: 深度学习
- 章节的编号: 2
- 章节名称: 卷积神经网络
- 课程时长（小时）: 6
- 与课程相关的关键词: CNN, 图像识别

这门课程涵盖了卷积神经网络（CNN）和图像识别的内容，适合对深度学习感兴趣的学习者。


用户提问:  openai 最近有什么新闻？
```

---

公开课全部课件，以及项目代码、数据等学习资料，扫码⬇️联系助教即可领取～

此外我们还创办了赋范大模型技术交流群，超活跃氛围，来遇见更多志同道合的小伙伴~

![](img/cecac36aef3e2490c0b53c40e442a950.png)

![](img/d9f736a165138d88c03081be644d396f.png)

+   体验课时间有限，若想深度学习大模型技术，欢迎大家报名由我主讲的《2025大模型Agent智能体开发实战》

**《2025大模型Agent智能体开发实战》为【100+小时】体**系大课，总共20大模块精讲精析，零基础直达大模型企业级应用！了解详细信息可扫码咨询