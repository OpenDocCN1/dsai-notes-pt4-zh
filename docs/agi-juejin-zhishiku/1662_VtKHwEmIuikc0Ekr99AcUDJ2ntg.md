# 探索LangGraph：如何创建一个既智能又可控的航空客服AI

> 来源：[https://agijuejin.feishu.cn/docx/AYSCde2yoo0LsqxzttFcxvVCnJQ](https://agijuejin.feishu.cn/docx/AYSCde2yoo0LsqxzttFcxvVCnJQ)

> 作者：AI小智

> 原文： [https://mp.weixin.](https://mp.weixin.qq.com/s/TZe7os5ISo6sH4WOyMXLnw)qq.com/s/TZe7os5I...

> 上节课，我们将利用 **LangGraph **的 interrupt_before 功能，在执行任何工具之前，暂停流程并把控制权交还给用户。没看过的同学可以点击链接 LangGraph实战：可控的AI航空客服助手 查阅。在本章节中，我们通过将工具分为只读（安全）和修改数据（敏感）两类，来优化我们的中断机制。我们仅对敏感工具实施中断，使得机器人能够自主处理一些简单的查询。

这种设计既保持了用户控制权，又确保了对话流程的顺畅。但随着工具数量的增加，单一的图结构可能会变得过于复杂。我们将在下一节中解决这个问题。

第三部分的图将类似于下面的示意图：

![](img/8f0d77c07c107e9cf458336e2f68e907.png)

第三部分示意图

###  **状态定义 **

**首先，**定义图的状态。我们的状态和LLM调用与第二部分 相同 。

```
from typing import Annotated

from langchain_anthropic import ChatAnthropic
from langchain_community.tools.tavily_search import TavilySearchResults
from langchain_core.prompts import ChatPromptTemplate
from langchain_core.runnables import Runnable, RunnableConfig
from typing_extensions import TypedDict

from langgraph.graph.message import AnyMessage, add_messages


class State(TypedDict):
    messages: Annotated[list[AnyMessage], add_messages]
    user_info: str


class Assistant:
    def __init__(self, runnable: Runnable):
        self.runnable = runnable

    def __call__(self, state: State, config: RunnableConfig):
        while True:
            result = self.runnable.invoke(state)
            # 如果LLM碰巧返回了一个空响应，我们将重新提示它
            # 以获得一个实际的响应。
            if not result.tool_calls and (
                not result.content
                or isinstance(result.content, list)
                and not result.content[0].get(&quot;text&quot;)
            ):
                messages = state[&quot;messages&quot;] + [(&quot;user&quot;, &quot;请给出真实的输出。&quot;)]
                state = {**state, &quot;messages&quot;: messages}
                messages = state[&quot;messages&quot;] + [(&quot;user&quot;, &quot;请给出真实的输出。&quot;)]
                state = {**state, &quot;messages&quot;: messages}
            else:
                break
        return {&quot;messages&quot;: result}


# Haiku更快更便宜，但准确性较低
# llm = ChatAnthropic(model=&quot;claude-3-haiku-20240307&quot;)
llm = ChatAnthropic(model=&quot;claude-3-sonnet-20240229&quot;, temperature=1)
# 你可以更新LLMs，尽管你可能需要更新提示
# from langchain_openai import ChatOpenAI

# llm = ChatOpenAI(model=&quot;gpt-4-turbo-preview&quot;)

assistant提示 = ChatPromptTemplate.from_messages(
    [
        (
            &quot;system&quot;,
            &quot;你是一位乐于助人的瑞士航空客户支持助手。&quot;
            &quot; 使用提供的工具搜索航班、公司政策和其他信息以协助用户的查询。&quot;
            &quot; 当搜索时，要坚持不懈。如果第一次搜索没有结果，扩大你的查询范围。&quot;
            &quot; 如果搜索空手而归，请在放弃之前扩大你的搜索。&quot;
            &quot;\n\n当前用户：\n&lt;User&gt;\n{user_info}\n&lt;/User&gt;&quot;
            &quot;\n当前时间：{time}。&quot;,
        ),
        (&quot;placeholder&quot;, &quot;{messages}&quot;),
    ]
).partial(time=datetime.now())


# &quot;阅读&quot;仅工具（例如检索器）不需要用户确认即可使用
part_3_safe_tools = [
    TavilySearchResults(max_results=1),
    fetch_user_flight_information,
    search_flights,
    lookup_policy,
    search_car_rentals,
    search_hotels,
    search_trip_recommendations,
]

# 这些工具都改变了用户的预订。
# 用户有权控制做出什么决定
part_3_sensitive_tools = [
    update_ticket_to_new_flight,
    cancel_ticket,
    book_car_rental,
    update_car_rental,
    cancel_car_rental,
    book_hotel,
    update_hotel,
    cancel_hotel,
    book_excursion,
    update_excursion,
    cancel_excursion,
]
sensitive_tool_names = {t.name for t in part_3_sensitive_tools}
# 我们的LLM不需要知道它必须路由到哪个节点。在它的&quot;思维&quot;中，它只是在调用函数。
part_3_assistant_runnable = assistant_prompt | llm.bind_tools(
    part_3_safe_tools + part_3_sensitive_tools
)
```

###  **图的定义 **

**现在，**创建图。我们的图与第二部分几乎 相同 ，只是我们将工具分为了两个不同的节点。我们只在实际更改用户预订的工具之前进行中断。

```
from typing import Literal

from langgraph.checkpoint.sqlite import SqliteSaver
from langgraph.graph import END, StateGraph
from langgraph.prebuilt import tools_condition

builder = StateGraph(State)


def user_info(state: State):
    return {&quot;user_info&quot;: fetch_user_flight_information.invoke({})}


# 新增：fetch_user_info节点首先运行，这意味着我们的助手可以在
# 不需要采取行动的情况下看到用户的航班信息
builder.add_node(&quot;fetch_user_info&quot;, user_info)
builder.set_entry_point(&quot;fetch_user_info&quot;)
builder.add_node(&quot;assistant&quot;, Assistant(part_3_assistant_runnable))
builder.add_node(&quot;safe_tools&quot;, create_tool_node_with_fallback(part_3_safe_tools))
builder.add_node(
    &quot;sensitive_tools&quot;, create_tool_node_with_fallback(part_3_sensitive_tools)
)
# 定义逻辑
builder.add_edge(&quot;fetch_user_info&quot;, &quot;assistant&quot;)


def route_tools(state: State) -&gt; Literal[&quot;safe_tools&quot;, &quot;sensitive_tools&quot;, &quot;__end__&quot;]:
    next_node = tools_condition(state)
    # 如果没有调用工具，返回用户
    if next_node == END:
        return END
    ai_message = state[&quot;messages&quot;][-1]
    # 这假设是单个工具调用。要处理并行工具调用，你将想要
    # 使用ANY条件
    first_tool_call = ai_message.tool_calls[0]
    if first_tool_call[&quot;name&quot;] in sensitive_tool_names:
        return &quot;sensitive_tools&quot;
    return &quot;safe_tools&quot;


builder.add_conditional_edges(
    &quot;assistant&quot;,
    route_tools,
)
builder.add_edge(&quot;safe_tools&quot;, &quot;assistant&quot;)
builder.add_edge(&quot;sensitive_tools&quot;, &quot;assistant&quot;)

memory = SqliteSaver.from_conn_string(&quot;:memory:&quot;)
part_3_graph = builder.compile(
    checkpointer=memory,
    # 新增：图将在执行&quot;tools&quot;节点之前始终停止。
    # 用户可以在助手继续之前批准或拒绝（甚至更改请求）
    interrupt_before=[&quot;sensitive_tools&quot;],
)
```

![](img/3124135f2675233966535d7fabbb60d8.png)

###  **示例对话 **

接下来，让我们尝试新修订的聊天机器人！我们将在以下对话列表上运行它。这次，我们将减少确认的次数。

```
import shutil
import uuid

# 使用备份文件更新，以便我们可以从每个部分的原始位置重新启动
shutil.copy(backup_file, db)
thread_id = str(uuid.uuid4())

config = {
    &quot;configurable&quot;: {
        # passenger_id在我们的航班工具中使用
        # 以获取用户的航班信息
        &quot;passenger_id&quot;: &quot;3442 587242&quot;,
        # 通过thread_id访问检查点
        &quot;thread_id&quot;: thread_id,
    }
}

tutorial_questions = [
    &quot;嗨，我的航班是什么时候？&quot;,
    &quot;我可以更新我的航班到更早的时间吗？我想今天晚些时候离开。&quot;,
    &quot;那就更新到下周的某个时间&quot;,
    &quot;下一个可用的选项很好&quot;,
    &quot;关于住宿和交通呢？&quot;,
    &quot;是的，我想要一个负担得起的酒店，用于我为期一周的住宿（7天）。我还想要租一辆车。&quot;,
    &quot;好的，可以为你推荐的酒店预订吗？听起来不错。&quot;,
    &quot;是的，继续预订任何中等费用且有可用性的酒店。&quot;,
    &quot;现在对于汽车，我的选择是什么？&quot;,
    &quot;太棒了，我们只需要最便宜的选项。继续预订7天&quot;,
    &quot;好的，现在你对旅行有什么建议？&quot;,
    &quot;在我在那里的时候，它们可用吗？&quot;,
    &quot;有趣 - 我喜欢博物馆，有什么选择？&quot;,
    &quot;好的，选一个并在我到达的第二天为我预订。&quot;,
]


_printed = set()
# 我们可以重用第一部分的教程问题，看看它的表现如何。
for question in tutorial_questions:
    events = part_3_graph.stream(
        {&quot;messages&quot;: (&quot;user&quot;, question)}, config, stream_mode=&quot;values&quot;
    )
    for event in events:
        _print_event(event, _printed)
    snapshot = part_3_graph.get_state(config)
    while snapshot.next:
        # 我们有中断！代理试图使用工具，用户可以批准或拒绝它
        # 注意：此代码都在图之外。通常，你会将输出流到UI。
        # 然后，当用户提供输入时，你会通过API调用触发新的运行。
        user_input = input(
            &quot;你是否批准上述操作？输入&#x27;y&#x27;继续；&quot;
            &quot; 否则，请说明你请求的更改。\n\n&quot;
        )
        if user_input.strip() == &quot;y&quot;:
            # 只是继续
            result = part_3_graph.invoke(
                None,
                config,
            )
        else:
            # 通过提供有关请求更改/改变主意的说明
            # 满足工具调用
            result = part_3_graph.invoke(
                {
                    &quot;messages&quot;: [
                        ToolMessage(
                            tool_call_id=event[&quot;messages&quot;][-1].tool_calls[0][&quot;id&quot;],
                            content=f&quot;API调用被用户拒绝。理由：&#x27;{user_input}&#x27;。继续协助，考虑用户的输入。&quot;,
                        )
                    ]
                },
                config,
            )
        snapshot = part_3_graph.get_state(config)
```

###  **第三部分回顾 **

现在，我们的聊天机器人工作得很好，你可以通过LangSmith跟踪来检查它的最新运行情况。这个设计可能已经满足了你的需求。代码是封闭的，并且它的行为符合预期。

![](img/47381d689d714642204f9124f50e0548.png)

然而，这个设计的一个潜在问题是，它对单个提示施加了很大压力。如果我们想要添加更多工具，或者每个工具变得更加复杂，那么机器人使用工具的效率和整体行为可能会受到影响。

在下一节中，我们将展示如何通过根据用户的意图将用户引导至专业代理或子图，来更精确地控制不同的用户体验。

今天的内容就到这里，如果老铁觉得还行，可以来一波三连，感谢！

**PS ： **A**I小智 **技 术交流群（技术交流、摸鱼、白嫖课程为主）又不定时开放了，感兴趣的朋友，可以在下方公号内回复： 666 ，即可进入。

**老规矩 **，道友们还记得么， **右下角的 “在看” 点一下 **， 如果感觉文章内容不错的话，记得分享朋友圈让更多的人知道！