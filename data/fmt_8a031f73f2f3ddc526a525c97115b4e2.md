
# 多智能体通讯建设

> 来源：[https://deepwisdom.feishu.cn/docx/SZwud9gv4o8TzHxxIrWc0ojBnNe](https://deepwisdom.feishu.cn/docx/SZwud9gv4o8TzHxxIrWc0ojBnNe)

## 消息交换机制

智能体之间的消息交换是通过 `Message` 中提供标签的属性，以及 `Environment` 提供的 `publish_message` 能力来实现的。

+ 智能体作为消息的发送者，只需提供消息的来源信息即可。消息的来源对应 `Message` 的 `sent_from`、`cause_by`。
+ 智能体作为消息的使用者，需要订阅相应的消息。消息的订阅标签对应 `Message` 的 `cause_by`。
+ `Environment` 对象负责将消息按各个智能体的订阅需求，广播给各个智能体。

```
class Message(BaseModel):
    id: str = Field(default="", validate_default=True)  # According to Section 2.2.3.1.1 of RFC 135
    content: str
    instruct_content: Optional[BaseModel] = Field(default=None, validate_default=True)
    role: str = "user"  # system / user / assistant
    cause_by: str = Field(default="", validate_default=True)
    sent_from: str = Field(default="", validate_default=True)
    send_to: set[str] = Field(default={MESSAGE_ROUTE_TO_ALL}, validate_default=True)
```

## 智能体功能边界设计

在规划智能体之间的消息转发流程时，首先要确定智能体的功能边界，这跟设计一个函数的套路一样：

1. 智能体输入什么。智能体的输入决定了智能体对象的 `rc.watch` 的值。
2. 智能体输出什么。智能体的输出决定了智能体输出 `Message` 的参数。
3. 智能体要完成什么工作。智能体要完成什么工作决定了智能体有多少 `action`，`action` 之间按什么状态流转。

## 复杂交互场景示例

我们以一个复杂的交互示例为例，来介绍整个实现的过程。假设我们要实现如下的流程：

```
Agent A takes requirements, splits into 10 subtasks.
Agent B is assigned to do 10 sub tasks.
Agent C is assigned to compile 10 subtasks.
Agent D reviews the compilation, provides feedback to Agent B.

Steps 2-4 need to happen 3-4 times. How do I architect my system such that this happens the right way? I can do the dirty code to tape it together but wanted to know the right way.
```

分析这个场景，我们可以得出如下结论：

1. `Agent A` 负责将需求拆分成 10 个 `subtasks`。
2. 对于每一个 `subtask`，`Agent B`、`Agent C`、`Agent D` 按如下流程处理：

```
Message(subtask) -> AgentB.run -> AgentC.run -> AgentD.run -> AgentB.run -> AgentC.run -> AgentD.run -> ...
```

也就是：

1. `Agent B` 的输入是 `Agent A` 的一个 `subtask`，或者是 `Agent D` 的执行结果；
2. `Agent C` 的输入是 `Agent B` 的输出；
3. `Agent D` 的输入是 `Agent C` 的输出；

因此，`Agent A` 的 `action` 需要将用户需求拆分成 10 个 `subtasks`，发给 `Agent B`：

### 第一步：让 `Agent A` 能接收用户发过来的需求

```
class AgentA(Role):
    def __init__(self, **kwargs) -> None:
        super().__init__(**kwargs)
        # Initialize actions specific to the Architect role
        self.set_actions([AgentAAction]) # 由于智能体只有1种action，所以不用改写_think函数。

        # 订阅消息
        self._watch({UserRequirement}) # UserRequirement是Message缺省的cause_by的值
```

### 第二步：`AgentAAction` 中，将用户需求拆分成 10 份

```
class AgentAAction(Aciton):
    async def run(self, with_messages:List[Message]=None, **kwargs) -> List[str]:
        subtasks: List[str] = split_10_subtask(with_messages[0].content)
        return subtasks
```

### 第三步：`Agent A` 在 `_act` 中 `run` `AgentAAction`，然后将结果发给 `Agent B`

```
class AgentA(Role):
    async def _act(self) -> Message:
        subtasks = await self.rc.todo.run(self.rc.history)
        for i in subtasks:
           self.rc.env.publish_message(Message(content=i, cause_by=AgentAAction)) # 发送10条这种消息，Agent B订阅了这种类型的消息
        return Message(content="dummy message", send_to=MESSAGE_ROUTE_TO_NONE) # 消息已发，所以return一个空消息就行
```

`Agent B`、`Agent C` 和 `Agent D` 的消息收发逻辑相同：

1. `Agent B` 订阅 `cause_by` 为 `AgentAAction` 和 `AgentDAction` 的消息；
2. `Agent C` 订阅 `cause_by` 为 `AgentBAction` 的消息；
3. `Agent D` 订阅 `cause_by` 为 `AgentCAction` 的消息；

```
class AgentBAction(Action):
        async def run(self, with_messages:List[Message]=None, **kwargs) -> Message:
            ... # 将结果填到Message里

class AgentB(Role):
    def __init__(self, **kwargs) -> None:
        super().__init__(**kwargs)
        # Initialize actions specific to the Architect role
        self.set_actions([AgentBAction]) # 由于智能体只有1种action，所以不用改写_think函数。

        # 订阅消息
        self._watch({AgentAAction, AgentDAction})
```

```
class AgentCAction(Action):
        async def run(self, with_messages:List[Message]=None, **kwargs) -> Message:
            ... # 将结果填到Message里

class AgentC(Role):
    def __init__(self, **kwargs) -> None:
        super().__init__(**kwargs)
        # Initialize actions specific to the Architect role
        self.set_actions([AgentCAction]) # 由于智能体只有1种action，所以不用改写_think函数。

        # 订阅消息
        self._watch({AgentBAction})
```

```
class AgentDAction(Action):
        async def run(self, with_messages:List[Message]=None, **kwargs) -> Message:
            ... # 将结果填到Message里

class AgentD(Role):
    def __init__(self, **kwargs) -> None:
        super().__init__(**kwargs)
        # Initialize actions specific to the Architect role
        self.set_actions([AgentDAction]) # 由于智能体只有1种action，所以不用改写_think函数。

        # 订阅消息
        self._watch({AgentCAction})
```

## 环境组装与运行

现在，所有智能体都定义完毕了，接下来只需将它们放到同一个 `Environment` 对象中，然后将用户消息发给 `Agent A`，让它们联动起来：

```
    context = Context() # Load config2.yaml
    env = Environment(context=context)
    env.add_roles([AgentA(), AgentB(), AgentC(), AgentD()])
    env.publish_message(Message(content='New user requirements', send_to=AgentA)) # 将用户的消息发送个Agent A，让Agent A开始工作。
    while not env.is_idle: # env.is_idle要等到所有Agent都没有任何新消息要处理后才会为True
        await env.run()
```
