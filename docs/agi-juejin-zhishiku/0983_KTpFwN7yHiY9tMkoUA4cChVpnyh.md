# LangGraph Agent：人在环路断点

> 来源：[https://agijuejin.feishu.cn/docx/UR9vdCmefoLEOwxd4zncms6Tn1b](https://agijuejin.feishu.cn/docx/UR9vdCmefoLEOwxd4zncms6Tn1b)

> 作者：lucas大叔

> 原文： [https://zhuanlan.z](https://zhuanlan.zhihu.com/p/708051359)hihu.com/p/708051...

人在环路（Human-in-the-loop，简称HIL）交互对于agent系统至关重要。断点是一种常见的 HIL 交互模式，允许图在特定步骤停止，并在继续执行之前寻求人类批准（例如，对于某些敏感操作）。

断点建立在 LangGraph checkpoint之上，后者在每个节点执行后保存图的状态。C[heck](https://www.zhihu.com/search?q=%E5%9B%BE%E6%89%A7%E8%A1%8C&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A708051359%7D)point保存在保留图状态的线程中，并且可以在图执行完成后访问。这允许 图执行 在特定点暂停，等待人类批准，然后从最后一个checkpoint恢复执行。

![](img/04d8007502e47f12df3b00ba21a983d7.png)

## 配置环境

首先安装必要的包

```
%%capture --no-stderr
%pip install --quiet -U langgraph langchain_anthropic
```

接下来配置Anthropic的API key

```
import getpass
import os


def _set_env(var: str):
    if not os.environ.get(var):
        os.environ[var] = getpass.getpass(f&quot;{var}: &quot;)


_set_env(&quot;ANTHROPIC_API_KEY&quot;)
```

## 简单应用

让我们看一下基本用法。 接下来我们做两件事：

+   指定断点使用interrupt_before指定步骤

+   设置checkpointer保存图的状态

```
from typing import TypedDict
from langgraph.graph import StateGraph, START, END
from langgraph.checkpoint.memory import MemorySaver
from IPython.display import Image, display

class State(TypedDict):
    input: str

def step_1(state):
    print(&quot;---Step 1---&quot;)
    pass

def step_2(state):
    print(&quot;---Step 2---&quot;)
    pass

def step_3(state):
    print(&quot;---Step 3---&quot;)
    pass

builder = StateGraph(State)
builder.add_node(&quot;step_1&quot;, step_1)
builder.add_node(&quot;step_2&quot;, step_2)
builder.add_node(&quot;step_3&quot;, step_3)
builder.add_edge(START, &quot;step_1&quot;)
builder.add_edge(&quot;step_1&quot;, &quot;step_2&quot;)
builder.add_edge(&quot;step_2&quot;, &quot;step_3&quot;)
builder.add_edge(&quot;step_3&quot;, END)

# Set up memory
memory = MemorySaver()

# Add 
graph = builder.compile(checkpointer=memory, interrupt_before=[&quot;step_3&quot;])

# View
display(Image(graph.get_graph().draw_mermaid_png()))
```

![](img/ae8dc49314e83ce6f29c335ec721d170.png)

为checkpointer创建一个线程ID。

如interrupt_before所定义的，我们运行到步骤3。

在用户输入/批准后，我们通过调用带有None的图来恢复执行。

```
# Input
initial_input = {&quot;input&quot;: &quot;hello world&quot;}

# Thread
thread = {&quot;configurable&quot;: {&quot;thread_id&quot;: &quot;1&quot;}}

# Run the graph until the first interruption
for event in graph.stream(initial_input, thread, stream_mode=&quot;values&quot;):
    print(event)

user_approval = input(&quot;Do you want to go to Step 3? (yes/no): &quot;)

if user_approval.lower() == &#x27;yes&#x27;:
    
    # If approved, continue the graph execution
    for event in graph.stream(None, thread, stream_mode=&quot;values&quot;):
        print(event)
else:
    print(&quot;Operation cancelled by user.&quot;)
```

> {'input': 'hello world'}

> ---Step 1---

> ---Step 2---

> ---Step 3---

## Agent

在agent的上下文中，断点对于手动批准某些agent操作很有用。 为了证明[这一点，我们将构建](https://www.zhihu.com/search?q=action%E8%8A%82%E7%82%B9&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A708051359%7D)一个相对简单的ReAct风格的agent来工具调用。 在调用 action节点 之前添加一个断点。

```
# Set up the tool
from langchain_anthropic import ChatAnthropic
from langchain_core.tools import tool
from langgraph.graph import MessagesState, START
from langgraph.prebuilt import ToolNode
from langgraph.graph import END, StateGraph
from langgraph.checkpoint.memory import MemorySaver


@tool
def search(query: str):
    &quot;&quot;&quot;Call to surf the web.&quot;&quot;&quot;
    # This is a placeholder for the actual implementation
    # Don&#x27;t let the LLM know this though  
    return [
        &quot;It&#x27;s sunny in San Francisco, but you better look out if you&#x27;re a Gemini  .&quot;
    ]


tools = [search]
tool_node = ToolNode(tools)

# Set up the model

model = ChatAnthropic(model=&quot;claude-3-5-sonnet-20240620&quot;)
model = model.bind_tools(tools)


# Define nodes and conditional edges


# Define the function that determines whether to continue or not
def should_continue(state):
    messages = state[&quot;messages&quot;]
    last_message = messages[-1]
    # If there is no function call, then we finish
    if not last_message.tool_calls:
        return &quot;end&quot;
    # Otherwise if there is, we continue
    else:
        return &quot;continue&quot;


# Define the function that calls the model
def call_model(state):
    messages = state[&quot;messages&quot;]
    response = model.invoke(messages)
    # We return a list, because this will get added to the existing list
    return {&quot;messages&quot;: [response]}


# Define a new graph
workflow = StateGraph(MessagesState)

# Define the two nodes we will cycle between
workflow.add_node(&quot;agent&quot;, call_model)
workflow.add_node(&quot;action&quot;, tool_node)

# Set the entrypoint as `agent`
# This means that this node is the first one called
workflow.add_edge(START, &quot;agent&quot;)

# We now add a conditional edge
workflow.add_conditional_edges(
    # First, we define the start node. We use `agent`.
    # This means these are the edges taken after the `agent` node is called.
    &quot;agent&quot;,
    # Next, we pass in the function that will determine which node is called next.
    should_continue,
    # Finally we pass in a mapping.
    # The keys are strings, and the values are other nodes.
    # END is a special node marking that the graph should finish.
    # What will happen is we will call `should_continue`, and then the output of that
    # will be matched against the keys in this mapping.
    # Based on which one it matches, that node will then be called.
    {
        # If `tools`, then we call the tool node.
        &quot;continue&quot;: &quot;action&quot;,
        # Otherwise we finish.
        &quot;end&quot;: END,
    },
)

# We now add a normal edge from `tools` to `agent`.
# This means that after `tools` is called, `agent` node is called next.
workflow.add_edge(&quot;action&quot;, &quot;agent&quot;)

# Set up memory
memory = MemorySaver()

# Finally, we compile it!
# This compiles it into a LangChain Runnable,
# meaning you can use it as you would any other runnable

# We add in `interrupt_before=[&quot;action&quot;]`
# This will add a breakpoint before the `action` node is called
app = workflow.compile(checkpointer=memory, interrupt_before=[&quot;action&quot;])

display(Image(app.get_graph().draw_mermaid_png()))
```

![](img/b5e0df7ac19adb4f446726f607b3ba77.png)

## 与Agent交互

Agent在调用工具之前停止，因为interrupt_before设置在action节点之前。

```
from langchain_core.messages import HumanMessage

thread = {&quot;configurable&quot;: {&quot;thread_id&quot;: &quot;3&quot;}}
inputs = [HumanMessage(content=&quot;search for the weather in sf now&quot;)]
for event in app.stream({&quot;messages&quot;: inputs}, thread, stream_mode=&quot;values&quot;):
    event[&quot;messages&quot;][-1].pretty_print()
```

> ================================ Human Message =================================

> 

> search for the weather in sf now

> ================================== Ai Message ==================================

> 

> [{'text': "Certainly! I'll search for the current weather in San Francisco for you. Let me use the search function to find this information.", 'type': 'text'}, {'text': None, 'type': 'tool_use', 'id': 'toolu_011ezBx5hKKjVJwqnECNPyyC', 'name': 'search', 'input': {'query': 'current weather in San Francisco'}}]

我们现在可以在没有输入继续的情况下再次调用agent，这将[按要求运](https://www.zhihu.com/search?q=%E4%B8%AD%E6%96%AD%E5%9B%BE&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A708051359%7D)行该工具。运行输入为None的 中断图 意味着就好像中断没有发生一样，可以继续执行。

```
for event in app.stream(None, thread, stream_mode=&quot;values&quot;):
    event[&quot;messages&quot;][-1].pretty_print()
```

> ================================= Tool Message ====================[==========](https://www.zhihu.com/search?q=astrology&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A708051359%7D)===

> Name: search

> 

> ["It's sunny in San Francisco, but you better look out if you're a Gemini \ud83d\ude08."]

> ================================== Ai Message ==================================

> 

> Based on the search results, I can provide you with information about the current weather in San Francisco:

> 

> The weather in San Francisco right now is sunny.

> 

> It's worth noting that the search result includes a playful reference to astrology , suggesting that Geminis should "look out." However, this is likely just a humorous addition and not related to the actual weather conditions.

> 

> Is there anything else you'd like to know about the weather in San Francisco or any other location?

## 参考文档

[https:// langchain-ai.github.io/ langgraph](https://link.zhihu.com/?target=https%3A//langchain-ai.github.io/langgraph/how-tos/human_in_the_loop/breakpoints/)/how-tos/human_in_the_loop/breakpoints/