# 探索LangGraph：构建多专家协作模型

> 来源：[https://agijuejin.feishu.cn/docx/ZzYLdLiVGoXywBxNwk6cwlEgnib](https://agijuejin.feishu.cn/docx/ZzYLdLiVGoXywBxNwk6cwlEgnib)

> 作者：AI小智

> 原文： [https://mp.weixin.](https://mp.weixin.qq.com/s/6PWhOWUpT-ySJU7UjgVavg)qq.com/s/6PWhOWUp...

> 在之前的部分，我们探讨了如何利用单一提示和大型语言模型（LLM）来处理多样化的用户需求，没看过的同学可以点击链接 探索LangGraph：如何创建一个既智能又可控的航空客服AI 查阅。

探索LangGraph：如何创建一个既智能又可控的航空客服AI 利用单一提示的方法确实能覆盖很广的应用场景。但是，如果想要为特定的用户需求提供稳定且出色的体验，仅靠这种方法就显得有些力不从心了。

取而代之的是，我们可以通过 **识别用户的意图 **，并将其引导至相应的定制化流程或“技能”，来满足用户的具体需求。每个流程都可以专注于特定的领域，这样不仅可以实现各自领域的优化提升，还不会影响到整体助手的性能。

在本节中，我们将用户交互体验划分为多个子图，形成一个类似下面的结构：

![](img/7d1c331be154f8a00af1e2b5ae5cfb63.png)

在**上图中**，每个方框都代表一个具有特定功能的独立工作流程。主要助手负责接收用户的初步询问，然后根据询问内容将任务分配给相应的 专家 。

###  **状态管理 **

我**们需要**跟踪在任何特定时刻哪个子图正在控制交互过程。虽然我们可以通过消息列表上的一些计算来实现这一点，但更简单的方法是使用一个专门的 堆栈 来跟踪。

在下面的 State 中添加一个 dialog_state 列表。每当一个 node 运行并返回 dialog_state 的值时，就会调用 update_dialog_stack 函数来决定如何更新堆栈。

```
from typing import Annotated, Literal, Optional

from typing_extensions import TypedDict

from langgraph.graph.message import AnyMessage, add_messages


def update_dialog_stack(left: list[str], right: Optional[str]) -&gt; list[str]:
    &quot;&quot;&quot;推入或弹出状态。&quot;&quot;&quot;
    if right is None:
        return left
    if right == &quot;pop&quot;:
        return left[:-1]
    return left + [right]


class State(TypedDict):
    messages: Annotated[list[AnyMessage], add_messages]
    user_info: str
    dialog_state: Annotated[
        list[
            Literal[
                &quot;assistant&quot;,
                &quot;update_flight&quot;,
                &quot;book_car_rental&quot;,
                &quot;book_hotel&quot;,
                &quot;book_excursion&quot;,
            ]
        ],
        update_dialog_stack,
    ]
```

###  **助手 **

这次我们将为每个工作流程创建一个助手。这意味着：

1.  航班预订助手

1.  酒店预订助手

1.  汽车租赁助手

1.  旅行助手

1.  最后，一个“主要助手”来在这些助手之间进行切换

**如果你仔**细观察，你会发现这实际上是我们在多代理示例中提到的 监督者 设计模式的一个实例。

下面，定义每个助手的 Runnable 对象。每个 Runnable 都有一个提示、LLM以及针对该助手的工具集。每个专门的助手还可以调用 CompleteOrEscalate 工具，以指示控制权应该交回给主要助手。这可能发生在助手成功完成任务，或者用户改变主意或需要该特定工作流程范围之外的帮助时。

```
from langchain_anthropic import ChatAnthropic
from langchain_community.tools.tavily_search import TavilySearchResults
from langchain_core.prompts import ChatPromptTemplate
from langchain_core.pydantic_v1 import BaseModel, Field
from langchain_core.runnables import Runnable, RunnableConfig


class Assistant:
    def __init__(self, runnable: Runnable):
        self.runnable = runnable

    def __call__(self, state: State, config: RunnableConfig):
        while True:
            result = self.runnable.invoke(state)

            if not result.tool_calls and (
                not result.content
                or isinstance(result.content, list)
                and not result.content[0].get(&quot;text&quot;)
            ):
                messages = state[&quot;messages&quot;] + [(&quot;user&quot;, &quot;用真实的输出回应。&quot;)]
                state = {**state, &quot;messages&quot;: messages}
                messages = state[&quot;messages&quot;] + [(&quot;user&quot;, &quot;用真实的输出回应。&quot;)]
                state = {**state, &quot;messages&quot;: messages}
            else:
                break
        return {&quot;messages&quot;: result}


class CompleteOrEscalate(BaseModel):
    &quot;&quot;&quot;一个工具，标记当前任务为已完成和/或将对话控制权升级到主助手，
    主助手可以根据用户的需求重新路由对话。&quot;&quot;&quot;

    cancel: bool = True
    reason: str

    class Config:
        schema_extra = {
            &quot;example&quot;: {
                &quot;cancel&quot;: True,
                &quot;reason&quot;: &quot;用户改变了他们对当前任务的想法。&quot;,
            },
            &quot;example 2&quot;: {
                &quot;cancel&quot;: True,
                &quot;reason&quot;: &quot;我已经完全完成了任务。&quot;,
            },
            &quot;example 3&quot;: {
                &quot;cancel&quot;: False,
                &quot;reason&quot;: &quot;我需要搜索用户的电子邮件或日历以获取更多信息。&quot;,
            },
        }

```

####   **航班预订助手 **

创建一个专门的助手来处理航班更新和取消预订的任务。

```
# 航班预订助手

flight_booking_prompt = ChatPromptTemplate.from_messages(
    [
        (
            &quot;system&quot;,
            &quot;您是专门处理航班更新的助手。&quot;
            &quot;每当用户需要帮助更新他们的预订时，主要助手就会委派工作给您。&quot;
            &quot;与客户确认更新的航班详情，并告知他们任何额外费用。&quot;
            &quot;搜索时，要坚持不懈。如果第一次搜索没有结果，就扩大您的查询范围。&quot;
            &quot;如果您需要更多信息或客户改变了主意，将任务升级回主助手。&quot;
            &quot;记住，只有在相关工具成功使用后，预订才算完成。&quot;
            &quot;\n\n当前用户航班信息：\n&lt;Flights&gt;\n{user_info}\n&lt;/Flights&gt;&quot;
            &quot;\n当前时间：{time}。&quot;
            &quot;\n\n如果用户需要帮助，而且您的工具都不适合，那么&quot;
            &#x27;&quot;CompleteOrEscalate&quot; 对话到主机助手。不要浪费用户的时间。不要编造无效的工具或功能。&#x27;,
        ),
        (&quot;placeholder&quot;, &quot;{messages}&quot;),
    ]
).partial(time=datetime.now())

update_flight_safe_tools = [search_flights]
update_flight_sensitive_tools = [update_ticket_to_new_flight, cancel_ticket]
update_flight_tools = update_flight_safe_tools + update_flight_sensitive_tools
update_flight_runnable = flight_booking_prompt | llm.bind_tools(
    update_flight_tools + [CompleteOrEscalate]
)
```

####   **汽车租赁助手 **

接下来，创建一个 **汽车租赁助手 **，以满足所有租车需求。

```
# 汽车租赁助手
book_car_rental_prompt = ChatPromptTemplate.from_messages(
    [
        (
            &quot;system&quot;,
            &quot;您是专门处理汽车租赁预订的助手。&quot;
            &quot;每当用户需要帮助预订汽车租赁时，主要助手就会委派工作给您。&quot;
            &quot;根据用户的偏好搜索可用的汽车租赁，并与客户确认预订详情。&quot;
            &quot;搜索时，要坚持不懈。如果第一次搜索没有结果，就扩大您的查询范围。&quot;
            &quot;如果您需要更多信息或客户改变了主意，将任务升级回主助手。&quot;
            &quot;记住，只有在相关工具成功使用后，预订才算完成。&quot;
            &quot;\n当前时间：{time}。&quot;
            &quot;\n\n如果用户需要帮助，而且您的工具都不适合，那么 &quot;
            &#x27;&quot;CompleteOrEscalate&quot; 对话到主机助手。不要浪费用户的时间。不要编造无效的工具或功能。&#x27;
            &quot;\n\n一些您应该 CompleteOrEscalate 的示例：&quot;
            &quot; - &#x27;今年这个时候的天气怎么样？&#x27;&quot;
            &quot; - &#x27;有哪些航班可用？&#x27;&quot;
            &quot; - &#x27;没关系，我想我会单独预订&#x27;&quot;
            &quot; - &#x27;哦等等，我还没有预订我的航班，我会先做这件事&#x27;&quot;
            &quot; - &#x27;汽车租赁预订确认&#x27;&quot;,
        ),
        (&quot;placeholder&quot;, &quot;{messages}&quot;),
    ]
).partial(time=datetime.now())

book_car_rental_safe_tools = [search_car_rentals]
book_car_rental_sensitive_tools = [
    book_car_rental,
    update_car_rental,
    cancel_car_rental,
]
book_car_rental_tools = book_car_rental_safe_tools + book_car_rental_sensitive_tools
book_car_rental_runnable = book_car_rental_prompt | llm.bind_tools(
    book_car_rental_tools + [CompleteOrEscalate]
)
```

####   **酒店预订助手 **

然后定义 **酒店预订 **的工作流程。

```
# 酒店预订助手
book_hotel_prompt = ChatPromptTemplate.from_messages(
    [
        (
            &quot;system&quot;,
            &quot;您是专门处理酒店预订的助手。&quot;
            &quot;每当用户需要帮助预订酒店时，主要助手就会委派工作给您。&quot;
            &quot;根据用户的偏好搜索可用的酒店，并与客户确认预订详情。&quot;
            &quot;搜索时，要坚持不懈。如果第一次搜索没有结果，就扩大您的查询范围。&quot;
            &quot;如果您需要更多信息或客户改变了主意，将任务升级回主助手。&quot;
            &quot;记住，只有在相关工具成功使用后，预订才算完成。&quot;
            &quot;\n当前时间：{time}。&quot;
            &#x27;\n\n如果用户需要帮助，而且您的工具都不适合，那么 &quot;CompleteOrEscalate&quot; 对话到主机助手。&#x27;
            &quot;不要浪费用户的时间。不要编造无效的工具或功能。&quot;
            &quot;\n\n一些您应该 CompleteOrEscalate 的示例：&quot;
            &quot; - &#x27;今年这个时候的天气怎么样？&#x27;&quot;
            &quot; - &#x27;没关系，我想我会单独预订&#x27;&quot;
            &quot; - &#x27;我需要弄清楚我在那里的时候的交通&#x27;&quot;
            &quot; - &#x27;哦等等，我还没有预订我的航班，我会先做这件事&#x27;&quot;
            &quot; - &#x27;酒店预订确认&#x27;&quot;,
        ),
        (&quot;placeholder&quot;, &quot;{messages}&quot;),
    ]
).partial(time=datetime.now())

book_hotel_safe_tools = [search_hotels]
book_hotel_sensitive_tools = [book_hotel, update_hotel, cancel_hotel]
book_hotel_tools = book_hotel_safe_tools + book_hotel_sensitive_tools
book_hotel_runnable = book_hotel_prompt | llm.bind_tools(
    book_hotel_tools + [CompleteOrEscalate]
)
```

####   **旅行助手 **

之后，定义 **旅行助手 **。

```
# 旅行助手

book_excursion_prompt = ChatPromptTemplate.from_messages(
    [
        (
            &quot;system&quot;,
            &quot;您是专门处理旅行建议的助手。&quot;
            &quot;每当用户需要帮助预订推荐的旅行时，主要助手就会委派工作给您。&quot;
            &quot;根据用户的偏好搜索可用的旅行建议，并与客户确认预订详情。&quot;
            &quot;如果您需要更多信息或客户改变了主意，将任务升级回主助手。&quot;
            &quot;搜索时，要坚持不懈。如果第一次搜索没有结果，就扩大您的查询范围。&quot;
            &quot;记住，只有在相关工具成功使用后，预订才算完成。&quot;
            &quot;\n当前时间：{time}。&quot;
            &#x27;\n\n如果用户需要帮助，而且您的工具都不适合，那么 &quot;CompleteOrEscalate&quot; 对话到主机助手。不要浪费用户的时间。不要编造无效的工具或功能。&#x27;
            &quot;\n\n一些您应该 CompleteOrEscalate 的示例：&quot;
            &quot; - &#x27;没关系，我想我会单独预订&#x27;&quot;
            &quot; - &#x27;我需要在那里的时候弄清楚交通&#x27;&quot;
            &quot; - &#x27;哦等等，我还没有预订我的航班，我会先做这件事&#x27;&quot;
            &quot; - &#x27;旅行预订确认！&#x27;&quot;,
        ),
        (&quot;placeholder&quot;, &quot;{messages}&quot;),
    ]
).partial(time=datetime.now())

book_excursion_safe_tools = [search_trip_recommendations]
book_excursion_sensitive_tools = [book_excursion, update_excursion, cancel_excursion]
book_excursion_tools = book_excursion_safe_tools + book_excursion_sensitive_tools
book_excursion_runnable = book_excursion_prompt | llm.bind_tools(
    book_excursion_tools + [CompleteOrEscalate]
)

```

####   **主要助手 **

最后，创建 **主要助手 **。

```
# 主要助手
class ToFlightBookingAssistant(BaseModel):
    &quot;&quot;&quot;将工作转交给专门助手来处理航班更新和取消。&quot;&quot;&quot;

    request: str = Field(
        description=&quot;更新航班助手在继续操作之前需要澄清的任何必要的后续问题。&quot;
    )


class ToBookCarRental(BaseModel):
    &quot;&quot;&quot;将工作转交给专门助手来处理汽车租赁预订。&quot;&quot;&quot;

    location: str = Field(
        description=&quot;用户想要租车的地点。&quot;
    )
    start_date: str = Field(description=&quot;汽车租赁的开始日期。&quot;)
    end_date: str = Field(description=&quot;汽车租赁的结束日期。&quot;)
    request: str = Field(
        description=&quot;用户关于汽车租赁的任何额外信息或请求。&quot;
    )

    class Config:
        schema_extra = {
            &quot;example&quot;: {
                &quot;location&quot;: &quot;巴塞尔&quot;,
                &quot;start_date&quot;: &quot;2023-07-01&quot;,
                &quot;end_date&quot;: &quot;2023-07-05&quot;,
                &quot;request&quot;: &quot;我需要一辆自动变速箱的紧凑型汽车。&quot;,
            }
        }


class ToHotelBookingAssistant(BaseModel):
    &quot;&quot;&quot;将工作转交给专门助手来处理酒店预订。&quot;&quot;&quot;

    location: str = Field(
        description=&quot;用户想要预订酒店的地点。&quot;
    )
    checkin_date: str = Field(description=&quot;酒店的入住日期。&quot;)
    checkout_date: str = Field(description=&quot;酒店的退房日期。&quot;)
    request: str = Field(
        description=&quot;用户关于酒店预订的任何额外信息或请求。&quot;
    )

    class Config:
        schema_extra = {
            &quot;example&quot;: {
                &quot;location&quot;: &quot;苏黎世&quot;,
                &quot;checkin_date&quot;: &quot;2023-08-15&quot;,
                &quot;checkout_date&quot;: &quot;2023-08-20&quot;,
                &quot;request&quot;: &quot;我更喜欢靠近市中心的酒店，有风景的房间。&quot;,
            }
        }


class ToBookExcursion(BaseModel):
    &quot;&quot;&quot;将工作转交给专门助手来处理旅行推荐和其他旅行预订。&quot;&quot;&quot;

    location: str = Field(
        description=&quot;用户想要预订推荐旅行的地点。&quot;
    )
    request: str = Field(
        description=&quot;用户关于旅行建议的任何额外信息或请求。&quot;
    )

    class Config:
        schema_extra = {
            &quot;example&quot;: {
                &quot;location&quot;: &quot;卢塞恩&quot;,
                &quot;request&quot;: &quot;用户对户外活动和风景感兴趣。&quot;,
            }
        }


# 最高级助手执行一般问答，并将专业任务委派给其他助手。
# 任务委派是一种简单的语义路由/简单的意图检测
# llm = ChatAnthropic(model=&quot;claude-3-haiku-20240307&quot;)
llm = ChatAnthropic(model=&quot;claude-3-sonnet-20240229&quot;, temperature=1)

primary_assistant_prompt = ChatPromptTemplate.from_messages(
    [
        (
            &quot;system&quot;,
            &quot;您是瑞士航空的乐于助人的客服助手。&quot;
            &quot;您的主要角色是搜索航班信息和公司政策，以回答客户查询。&quot;
            &quot;如果客户请求更新或取消预订、租车、预订酒店或获取旅行建议，&quot;
            &quot;通过调用相应的工具，将任务委派给适当的专业助手。您自己无法进行这些类型的更改。&quot;
            &quot;只有专业助手才有权为用户执行此操作。&quot;
            &quot;用户不知道不同的专业助手，所以不要提及他们；只需通过功能调用来静静地委派。&quot;
            &quot;向客户提供详细信息，并在得出信息不可用的结论之前，始终再次检查数据库。&quot;
            &quot;当搜索时，要坚持不懈。如果第一次搜索没有结果，就扩大您的查询范围。&quot;
            &quot;\n\n当前用户航班信息：\n&lt;Flights&gt;\n{user_info}\n&lt;/Flights&gt;&quot;
            &quot;\n当前时间：{time}。&quot;,
        ),
        (&quot;placeholder&quot;, &quot;{messages}&quot;),
    ]
).partial(time=datetime.now())
primary_assistant_tools = [
    TavilySearchResults(max_results=1),
    search_flights,
    lookup_policy,
]
assistant_runnable = primary_assistant_prompt | llm.bind_tools(
    primary_assistant_tools
    + [
        ToFlightBookingAssistant,
        ToBookCarRental,
        ToHotelBookingAssistant,
        ToBookExcursion,
    ]
)
```

###  **创建助手 **

我们即将创建图。在前一节中，我们做出了设计决策，让所有节点之间共享 messages 状态。这在每个委派助手都可以看到整个用户旅程并拥有共享上下文方面非常强大。然而，这意味着较弱的LLMs很容易对它们特定的范围感到困惑。为了标记主助手和委派工作流程之一之间的“交接”（并完成路由器的工具调用），我们将向状态中添加一个 ToolMessage 。

###  **实用工具 **

创建一个函数，为每个工作流程制作一个“入口”节点，声明“当前助手是 assistant_name ”。

```
from typing import Callable

from langchain_core.messages import ToolMessage


def create_entry_node(assistant_name: str, new_dialog_state: str) -&gt; Callable:
    def entry_node(state: State) -&gt; dict:
        tool_call_id = state[&quot;messages&quot;][-1].tool_calls[0][&quot;id&quot;]
        return {
            &quot;messages&quot;: [
                ToolMessage(
                    content=f&quot;现在助手是 {assistant_name}。回想一下主机助手和用户之间的上述对话。&quot;
                    f&quot; 用户的意图尚未得到满足。使用提供的工具来帮助用户。记住，你是 {assistant_name}，&quot;
                    &quot; 预订、更新、其他操作或其他动作只有在您成功调用适当的工具后才完成。&quot;
                    &quot; 如果用户改变主意或需要其他任务的帮助，请调用 CompleteOrEscalate 函数，让主机助手接管控制权。&quot;
                    &quot; 不要提及你是谁 - 只作为助手的代理行事。&quot;,
                    tool_call_id=tool_call_id,
                )
            ],
            &quot;dialog_state&quot;: new_dialog_state,
        }

    return entry_node
```

###  **定义图 **

现在是我们开始构建图的时候了。和以前一样，我们将从一个节点开始，用用户的当前信息预填充状态。

```
from typing import Literal

from langgraph.checkpoint.sqlite import SqliteSaver
from langgraph.graph import END, StateGraph
from langgraph.prebuilt import tools_condition

builder = StateGraph(State)


def user_info(state: State):
    return {&quot;user_info&quot;: fetch_user_flight_information.invoke({})}


builder.add_node(&quot;fetch_user_info&quot;, user_info)
builder.set_entry_point(&quot;fetch_user_info&quot;)
```

现在，让我们开始构建我们定制的工作流程。每个小工作流程的结构都和我们在第3部分中展示的完整工作流程图非常相似，它们都包含5个节点：

1.  enter_* : 使用你之前定义的 create_entry_node 工具来创建一个ToolMessage，这个ToolMessage表明新的专业助手已经接管了工作。

1.  **助手 **: 这个由提示和大型语言模型（LLM）组成的模块会根据当前状态来决定是使用一个工具、向用户提问还是结束整个工作流程（返回到主助手）。

1.  *_safe_tools : 这些是助手可以在不需要用户确认的情况下使用的“只读”工具。

1.  *_sensitive_tools : 这些具有“写入”权限的工具需要用户的确认，并且在我们编译工作流程图时，它们会被设置一个 interrupt_before 。

1.  leave_skill : 通过 弹出 dialog_state 来表示 主助手 重新掌握了控制权。

由于这些工作流程的相似性，我们本可以定义一个工厂函数来生成它们。但因为这是一个教程，我们会逐一明确地定义它们。

首先，我们来创建一个 **航班预订助手 **，它专门负责管理用户更新和取消预订航班的流程。

```
# 航班预订助手
# Flight booking assistant
builder.add_node(
    &quot;enter_update_flight&quot;,
    create_entry_node(&quot;Flight Updates &amp; Booking Assistant&quot;, &quot;update_flight&quot;),
)
builder.add_node(&quot;update_flight&quot;, Assistant(update_flight_runnable))
builder.add_edge(&quot;enter_update_flight&quot;, &quot;update_flight&quot;)
builder.add_node(
    &quot;update_flight_sensitive_tools&quot;,
    create_tool_node_with_fallback(update_flight_sensitive_tools),
)
builder.add_node(
    &quot;update_flight_safe_tools&quot;,
    create_tool_node_with_fallback(update_flight_safe_tools),
)


def route_update_flight(
    state: State,
) -&gt; Literal[
    &quot;update_flight_sensitive_tools&quot;,
    &quot;update_flight_safe_tools&quot;,
    &quot;leave_skill&quot;,
    &quot;__end__&quot;,
]:
    route = tools_condition(state)
    if route == END:
        return END
    tool_calls = state[&quot;messages&quot;][-1].tool_calls
    did_cancel = any(tc[&quot;name&quot;] == CompleteOrEscalate.__name__ for tc in tool_calls)
    if did_cancel:
        return &quot;leave_skill&quot;
    safe_toolnames = [t.name for t in update_flight_safe_tools]
    if all(tc[&quot;name&quot;] in safe_toolnames for tc in tool_calls):
        return &quot;update_flight_safe_tools&quot;
    return &quot;update_flight_sensitive_tools&quot;


builder.add_edge(&quot;update_flight_sensitive_tools&quot;, &quot;update_flight&quot;)
builder.add_edge(&quot;update_flight_safe_tools&quot;, &quot;update_flight&quot;)
builder.add_conditional_edges(&quot;update_flight&quot;, route_update_flight)


# This node will be shared for exiting all specialized assistants
def pop_dialog_state(state: State) -&gt; dict:
    &quot;&quot;&quot;Pop the dialog stack and return to the main assistant.

    This lets the full graph explicitly track the dialog flow and delegate control
    to specific sub-graphs.
    &quot;&quot;&quot;
    messages = []
    if state[&quot;messages&quot;][-1].tool_calls:
        # Note: Doesn&#x27;t currently handle the edge case where the llm performs parallel tool calls
        messages.append(
            ToolMessage(
                content=&quot;Resuming dialog with the host assistant. Please reflect on the past conversation and assist the user as needed.&quot;,
                tool_call_id=state[&quot;messages&quot;][-1].tool_calls[0][&quot;id&quot;],
            )
        )
    return {
        &quot;dialog_state&quot;: &quot;pop&quot;,
        &quot;messages&quot;: messages,
    }


builder.add_node(&quot;leave_skill&quot;, pop_dialog_state)
builder.add_edge(&quot;leave_skill&quot;, &quot;primary_assistant&quot;)
```

接下来，创建一个 **租车助手 **的工作流程图，它将负责处理所有的租车需求。

```
# 租车助手

# Car rental assistant

builder.add_node(
    &quot;enter_book_car_rental&quot;,
    create_entry_node(&quot;Car Rental Assistant&quot;, &quot;book_car_rental&quot;),
)
builder.add_node(&quot;book_car_rental&quot;, Assistant(book_car_rental_runnable))
builder.add_edge(&quot;enter_book_car_rental&quot;, &quot;book_car_rental&quot;)
builder.add_node(
    &quot;book_car_rental_safe_tools&quot;,
    create_tool_node_with_fallback(book_car_rental_safe_tools),
)
builder.add_node(
    &quot;book_car_rental_sensitive_tools&quot;,
    create_tool_node_with_fallback(book_car_rental_sensitive_tools),
)


def route_book_car_rental(
    state: State,
) -&gt; Literal[
    &quot;book_car_rental_safe_tools&quot;,
    &quot;book_car_rental_sensitive_tools&quot;,
    &quot;leave_skill&quot;,
    &quot;__end__&quot;,
]:
    route = tools_condition(state)
    if route == END:
        return END
    tool_calls = state[&quot;messages&quot;][-1].tool_calls
    did_cancel = any(tc[&quot;name&quot;] == CompleteOrEscalate.__name__ for tc in tool_calls)
    if did_cancel:
        return &quot;leave_skill&quot;
    safe_toolnames = [t.name for t in book_car_rental_safe_tools]
    if all(tc[&quot;name&quot;] in safe_toolnames for tc in tool_calls):
        return &quot;book_car_rental_safe_tools&quot;
    return &quot;book_car_rental_sensitive_tools&quot;


builder.add_edge(&quot;book_car_rental_sensitive_tools&quot;, &quot;book_car_rental&quot;)
builder.add_edge(&quot;book_car_rental_safe_tools&quot;, &quot;book_car_rental&quot;)
builder.add_conditional_edges(&quot;book_car_rental&quot;, route_book_car_rental)
```

然后，创建一个 **酒店预订 **的工作流程。

```
# 酒店预订助手
# Hotel booking assistant
builder.add_node(
    &quot;enter_book_hotel&quot;, create_entry_node(&quot;Hotel Booking Assistant&quot;, &quot;book_hotel&quot;)
)
builder.add_node(&quot;book_hotel&quot;, Assistant(book_hotel_runnable))
builder.add_edge(&quot;enter_book_hotel&quot;, &quot;book_hotel&quot;)
builder.add_node(
    &quot;book_hotel_safe_tools&quot;,
    create_tool_node_with_fallback(book_hotel_safe_tools),
)
builder.add_node(
    &quot;book_hotel_sensitive_tools&quot;,
    create_tool_node_with_fallback(book_hotel_sensitive_tools),
)


def route_book_hotel(
    state: State,
) -&gt; Literal[
    &quot;leave_skill&quot;, &quot;book_hotel_safe_tools&quot;, &quot;book_hotel_sensitive_tools&quot;, &quot;__end__&quot;
]:
    route = tools_condition(state)
    if route == END:
        return END
    tool_calls = state[&quot;messages&quot;][-1].tool_calls
    did_cancel = any(tc[&quot;name&quot;] == CompleteOrEscalate.__name__ for tc in tool_calls)
    if did_cancel:
        return &quot;leave_skill&quot;
    tool_names = [t.name for t in book_hotel_safe_tools]
    if all(tc[&quot;name&quot;] in tool_names for tc in tool_calls):
        return &quot;book_hotel_safe_tools&quot;
    return &quot;book_hotel_sensitive_tools&quot;


builder.add_edge(&quot;book_hotel_sensitive_tools&quot;, &quot;book_hotel&quot;)
builder.add_edge(&quot;book_hotel_safe_tools&quot;, &quot;book_hotel&quot;)
builder.add_conditional_edges(&quot;book_hotel&quot;, route_book_hotel)
```

之后，定义一个 **旅行预订助手 **。

```
# 旅行预订助手
# Excursion assistant
builder.add_node(
    &quot;enter_book_excursion&quot;,
    create_entry_node(&quot;Trip Recommendation Assistant&quot;, &quot;book_excursion&quot;),
)
builder.add_node(&quot;book_excursion&quot;, Assistant(book_excursion_runnable))
builder.add_edge(&quot;enter_book_excursion&quot;, &quot;book_excursion&quot;)
builder.add_node(
    &quot;book_excursion_safe_tools&quot;,
    create_tool_node_with_fallback(book_excursion_safe_tools),
)
builder.add_node(
    &quot;book_excursion_sensitive_tools&quot;,
    create_tool_node_with_fallback(book_excursion_sensitive_tools),
)


def route_book_excursion(
    state: State,
) -&gt; Literal[
    &quot;book_excursion_safe_tools&quot;,
    &quot;book_excursion_sensitive_tools&quot;,
    &quot;leave_skill&quot;,
    &quot;__end__&quot;,
]:
    route = tools_condition(state)
    if route == END:
        return END
    tool_calls = state[&quot;messages&quot;][-1].tool_calls
    did_cancel = any(tc[&quot;name&quot;] == CompleteOrEscalate.__name__ for tc in tool_calls)
    if did_cancel:
        return &quot;leave_skill&quot;
    tool_names = [t.name for t in book_excursion_safe_tools]
    if all(tc[&quot;name&quot;] in tool_names for tc in tool_calls):
        return &quot;book_excursion_safe_tools&quot;
    return &quot;book_excursion_sensitive_tools&quot;


builder.add_edge(&quot;book_excursion_sensitive_tools&quot;, &quot;book_excursion&quot;)
builder.add_edge(&quot;book_excursion_safe_tools&quot;, &quot;book_excursion&quot;)
builder.add_conditional_edges(&quot;book_excursion&quot;, route_book_excursion)
```

最后，创建一个 **主助手 **。

```
# Primary assistant
builder.add_node(&quot;primary_assistant&quot;, Assistant(assistant_runnable))
builder.add_node(
    &quot;primary_assistant_tools&quot;, create_tool_node_with_fallback(primary_assistant_tools)
)


def route_primary_assistant(
    state: State,
) -&gt; Literal[
    &quot;primary_assistant_tools&quot;,
    &quot;enter_update_flight&quot;,
    &quot;enter_book_hotel&quot;,
    &quot;enter_book_excursion&quot;,
    &quot;__end__&quot;,
]:
    route = tools_condition(state)
    if route == END:
        return END
    tool_calls = state[&quot;messages&quot;][-1].tool_calls
    if tool_calls:
        if tool_calls[0][&quot;name&quot;] == ToFlightBookingAssistant.__name__:
            return &quot;enter_update_flight&quot;
        elif tool_calls[0][&quot;name&quot;] == ToBookCarRental.__name__:
            return &quot;enter_book_car_rental&quot;
        elif tool_calls[0][&quot;name&quot;] == ToHotelBookingAssistant.__name__:
            return &quot;enter_book_hotel&quot;
        elif tool_calls[0][&quot;name&quot;] == ToBookExcursion.__name__:
            return &quot;enter_book_excursion&quot;
        return &quot;primary_assistant_tools&quot;
    raise ValueError(&quot;Invalid route&quot;)


# The assistant can route to one of the delegated assistants,
# directly use a tool, or directly respond to the user
builder.add_conditional_edges(
    &quot;primary_assistant&quot;,
    route_primary_assistant,
    {
        &quot;enter_update_flight&quot;: &quot;enter_update_flight&quot;,
        &quot;enter_book_car_rental&quot;: &quot;enter_book_car_rental&quot;,
        &quot;enter_book_hotel&quot;: &quot;enter_book_hotel&quot;,
        &quot;enter_book_excursion&quot;: &quot;enter_book_excursion&quot;,
        &quot;primary_assistant_tools&quot;: &quot;primary_assistant_tools&quot;,
        END: END,
    },
)
builder.add_edge(&quot;primary_assistant_tools&quot;, &quot;primary_assistant&quot;)


# Each delegated workflow can directly respond to the user
# When the user responds, we want to return to the currently active workflow
def route_to_workflow(
    state: State,
) -&gt; Literal[
    &quot;primary_assistant&quot;,
    &quot;update_flight&quot;,
    &quot;book_car_rental&quot;,
    &quot;book_hotel&quot;,
    &quot;book_excursion&quot;,
]:
    &quot;&quot;&quot;If we are in a delegated state, route directly to the appropriate assistant.&quot;&quot;&quot;
    dialog_state = state.get(&quot;dialog_state&quot;)
    if not dialog_state:
        return &quot;primary_assistant&quot;
    return dialog_state[-1]


builder.add_conditional_edges(&quot;fetch_user_info&quot;, route_to_workflow)

# Compile graph
memory = SqliteSaver.from_conn_string(&quot;:memory:&quot;)
part_4_graph = builder.compile(
    checkpointer=memory,
    # Let the user approve or deny the use of sensitive tools
    interrupt_before=[
        &quot;update_flight_sensitive_tools&quot;,
        &quot;book_car_rental_sensitive_tools&quot;,
        &quot;book_hotel_sensitive_tools&quot;,
        &quot;book_excursion_sensitive_tools&quot;,
    ],
)
```

![](img/c5bfd4b829eceec213ebb1ed22a1f184.png)

这里是一个图片链接

###  **对话 **

那真是很多内容！让我们在下面的对话轮次列表上运行它。这次，我们将有更少的确认。

```
import shutil
import uuid

# Update with the backup file so we can restart from the original place in each section
shutil.copy(backup_file, db)
thread_id = str(uuid.uuid4())

config = {
    &quot;configurable&quot;: {
        # The passenger_id is used in our flight tools to
        # fetch the user&#x27;s flight information
        &quot;passenger_id&quot;: &quot;3442 587242&quot;,
        # Checkpoints are accessed by thread_id
        &quot;thread_id&quot;: thread_id,
    }
}

_printed = set()
# We can reuse the tutorial questions from part 1 to see how it does.
for question in tutorial_questions:
    events = part_4_graph.stream(
        {&quot;messages&quot;: (&quot;user&quot;, question)}, config, stream_mode=&quot;values&quot;
    )
    for event in events:
        _print_event(event, _printed)
    snapshot = part_4_graph.get_state(config)
    while snapshot.next:
        # We have an interrupt! The agent is trying to use a tool, and the user can approve or deny it
        # Note: This code is all outside of your graph. Typically, you would stream the output to a UI.
        # Then, you would have the frontend trigger a new run via an API call when the user has provided input.
        user_input = input(
            &quot;Do you approve of the above actions? Type &#x27;y&#x27; to continue;&quot;
            &quot; otherwise, explain your requested changed.\n\n&quot;
        )
        if user_input.strip() == &quot;y&quot;:
            # Just continue
            result = part_4_graph.invoke(
                None,
                config,
            )
        else:
            # Satisfy the tool invocation by
            # providing instructions on the requested changes / change of mind
            result = part_4_graph.invoke(
                {
                    &quot;messages&quot;: [
                        ToolMessage(
                            tool_call_id=event[&quot;messages&quot;][-1].tool_calls[0][&quot;id&quot;],
                            content=f&quot;API call denied by user. Reasoning: &#x27;{user_input}&#x27;. Continue assisting, accounting for the user&#x27;s input.&quot;,
                        )
                    ]
                },
                config,
            )
        snapshot = part_4_graph.get_state(config)
```

###  **结论 **

您现在开发了一个能够处理多种任务的客户支持机器人，它使用了专注的工作流程。更重要的是，您已经学会了如何使用LangGraph的核心功能来设计和根据产品需求重构应用程序。

上述示例并不是针对您的特定需求进行优化的 - 大型语言模型（LLMs）可能会出错，每个流程都可以通过更好的提示和实验来提高可靠性。一旦您创建了初始支持机器人，下一步就是开始添加评估，这样您就可以自信地改进您的系统。查看那些文档和我们的其他教程，了解更多！

今天的内容就到这里，如果老铁觉得还行，可以来一波三连，感谢！

**PS ： **A**I小智 **技 术交流群（技术交流、摸鱼、白嫖课程为主）又不定时开放了，感兴趣的朋友，可以在下方公号内回复： 666 ，即可进入。

**老规矩 **，道友们还记得么， **右下角的 “在看” 点一下 **， 如果感觉文章内容不错的话，记得分享朋友圈让更多的人知道！