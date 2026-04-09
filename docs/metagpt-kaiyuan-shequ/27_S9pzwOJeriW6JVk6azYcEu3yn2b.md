# 多智能体通讯建设

> 来源：[https://deepwisdom.feishu.cn/docx/SZwud9gv4o8TzHxxIrWc0ojBnNe](https://deepwisdom.feishu.cn/docx/SZwud9gv4o8TzHxxIrWc0ojBnNe)

智能体之间的消息交换是通过Message中提供标签的属性，以及Environment提供的publish_message能力来实现的。

+   智能体作为消息的发送者，只需提供消息的来源信息即可。消息的来源对应Message的sent_from、cause_by。

+   智能体作为消息的使用者，需要订阅相应的消息。消息的订阅标签对应Message的cause_by。

+   Environment对象负责将消息按各个智能体的订阅需求，广播给各个智能体。

```
class Message(BaseModel):
    id: str = Field(default=&quot;&quot;, validate_default=True)  # According to Section 2.2.3.1.1 of RFC 135
    content: str
    instruct_content: Optional[BaseModel] = Field(default=None, validate_default=True)
    role: str = &quot;user&quot;  # system / user / assistant
    cause_by: str = Field(default=&quot;&quot;, validate_default=True)
    sent_from: str = Field(default=&quot;&quot;, validate_default=True)
    send_to: set[str] = Field(default={MESSAGE_ROUTE_TO_ALL}, validate_default=True)
```

在规划智能体之间的消息转发流程时，首先要确定智能体的功能边界，这跟设计一个函数的套路一样：

1.  智能体输入什么。智能体的输入决定了智能体对象的rc.watch的值。

1.  智能体输出什么。智能体的输出决定了智能体输出Message的参数。

1.  智能体要完成什么工作。智能体要完成什么工作决定了智能体有多少action，action之间按什么状态流转。

我们以一个复杂的交互示例为例，来介绍整个实现的过程。 假设我们要实现如下的流程：

```
Agent A takes requirements, splits into 10 subtasks.
Agent B is assigned to do 10 sub tasks.
Agent C is assigned to compile 10 subtasks.
Agent D reviews the compilation, provides feedback to Agent B.

Steps 2-4 need to happen 3-4 times. How do I architect my system such that this happens the right way? I can do the dirty code to tape it together but wanted to know the right way.
```

分析这个场景，我们可以得出如下结论：

1.  Agent A负责将需求拆分成10个subtasks。

1.  对于每一个subtask，Agent B，C，D按如下流程处理：

```
Message(subtask) -&gt; AgentB.run -&gt; AgentC.run -&gt; AgentD.run -&gt; AgentB.run -&gt; AgentC.run -&gt; AgentD.run -&gt; ...
```

也就是：

1.  Agent B的输入是Agent A的一个subtask，或者是Agent D的执行结果；

1.  Agent C的输入是Agent B的输出；

1.  Agent D的输入是Agent C的输出；

因此，Agent A的action需要将用户需求拆分成10个subtasks，发给Agent B：

1.  第一步：让Agent A能接收用户发过来的需求：

```
class AgentA(Role):
    def __init__(self, **kwargs) -&gt; None:
        super().__init__(**kwargs)
        # Initialize actions specific to the Architect role
        self.set_actions([AgentAAction]) # 由于智能体只有1种action，所以不用改写_think函数。

        # 订阅消息
        self._watch({UserRequirement}) # UserRequirement是Message缺省的cause_by的值
```

1.  第二步： AgentAAction中，将用户需求拆分成10份

```
class AgentAAction(Aciton):
    async def run(self, with_messages:List[Message]=None, **kwargs) -&gt; List[str]:
        subtasks: List[str] = split_10_subtask(with_messages[0].content)
        return subtasks
```

1.  第三步：Agent A在_act中run AgentAAction，然后将结果发给Agent B：

```
class AgentA(Role):
    async def _act(self) -&gt; Message:
        subtasks = await self.rc.todo.run(self.rc.history)
        for i in subtasks:
           self.rc.env.publish_message(Message(content=i, cause_by=AgentAAction)) # 发送10条这种消息，Agent B订阅了这种类型的消息
        return Message(content=&quot;dummy message&quot;, send_to=MESSAGE_ROUTE_TO_NONE) # 消息已发，所以return一个空消息就行
```

Agent B、Agent C和Agent D的消息收发逻辑相同：

1.  Agent B订阅cause_by为AgentAAction和AgentDAction的消息；

1.  Agent C订阅cause_by为AgentBAction的消息；

1.  Agent D订阅cause_by为AgentCAction的消息；

```
class AgentBAction(Action):
        async def run(self, with_messages:List[Message]=None, **kwargs) -&gt; Message:
            ... # 将结果填到Message里

class AgentB(Role):
    def __init__(self, **kwargs) -&gt; None:
        super().__init__(**kwargs)
        # Initialize actions specific to the Architect role
        self.set_actions([AgentBAction]) # 由于智能体只有1种action，所以不用改写_think函数。

        # 订阅消息
        self._watch({AgentAAction, AgentDAction})
```

```
class AgentCAction(Action):
        async def run(self, with_messages:List[Message]=None, **kwargs) -&gt; Message:
            ... # 将结果填到Message里

class AgentC(Role):
    def __init__(self, **kwargs) -&gt; None:
        super().__init__(**kwargs)
        # Initialize actions specific to the Architect role
        self.set_actions([AgentCAction]) # 由于智能体只有1种action，所以不用改写_think函数。

        # 订阅消息
        self._watch({AgentBAction})
```

```
class AgentDAction(Action):
        async def run(self, with_messages:List[Message]=None, **kwargs) -&gt; Message:
            ... # 将结果填到Message里

class AgentD(Role):
    def __init__(self, **kwargs) -&gt; None:
        super().__init__(**kwargs)
        # Initialize actions specific to the Architect role
        self.set_actions([AgentDAction]) # 由于智能体只有1种action，所以不用改写_think函数。

        # 订阅消息
        self._watch({AgentCAction})
```

现在，所有智能体都定义完毕了，接下来只需将它们放到同一个Environment对象中，然后将用户消息发给Agent A，让它们联动起来：

```
    context = Context() # Load config2.yaml
    env = Environment(context=context)
    env.add_roles([AgentA(), AgentB(), AgentC(), AgentD()])
    env.publish_message(Message(content=&#x27;New user requirements&#x27;, send_to=AgentA)) # 将用户的消息发送个Agent A，让Agent A开始工作。
    while not env.is_idle: # env.is_idle要等到所有Agent都没有任何新消息要处理后才会为True
        await env.run()
```