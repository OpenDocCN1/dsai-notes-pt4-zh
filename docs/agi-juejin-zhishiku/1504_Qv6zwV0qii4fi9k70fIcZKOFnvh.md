
# llama-agents：开源AI Agents框架、代理即服务、与Crew AI和AutoGen比有何不同？

> 来源：https://agijuejin.feishu.cn/docx/QYWnd5RnyoQgvixdebQcydWOnHf

> 作者：Aitrainee | AI进修生

> 原文：https://mp.weixin.qq.com/s/NOJd-KyIicj0dj1LLu_bUQ

> 🌟`Ollama-agents`是一个异步优先框架，用于构建、迭代和生产多智能体系统，包括多智能体通信、分布式工具执行、人在环等！

`https://github.com/run-llama/llama-agents`

Hi，这里是Aitrainee，欢迎阅读本期新文章。

Llama Agents是什么呢？它是由 `Llama Index` 发布的多代理框架。

> `Llama Index` 可以将你的企业数据转化为语言模型应用程序，处理日志记录、索引、查询和评估。

Llama Agents有什么不同呢？ 它是“AI代理即服务”，意味着每个代理会作为一个独立的服务运行在不同的URL上。

想象一下，有两个代理：代理1有访问机密信息的权限，代理2可以生成有趣的事实。目前，这些代理存在于瞬间然后消失，但如果你想持续使用这些代理，就需要将它们托管在服务器上，并在需要时访问它们。这就是Llama Agents框架简化的过程。

例如，如果你运行这个框架，代理1会在本地主机8000端口运行，而代理2会在8002端口运行，你可以通过仪表盘监控所有代理。你可以定义一个任务，比如获取机密信息，这个任务会被分配给相应的代理。

## 与其他代理框架如Crew AI和AutoGen进行比较

`Llama Agents`是一个异步优先框架，适用于构建、启动和生产化多代理系统。异步的意思是你可以在不等待其他代理完成任务的情况下继续执行任务，从而加快处理速度。比如，如果多个代理同时执行相同的任务，你可以同时处理，从而节省时间。

当用户提出问题时，它会进入控制平面，在其他框架中被称为管理语言模型（LLM）或主管语言模型，它负责将任务分配给其他代理。

在`Llama Agents`中，控制平面会决定接下来发生什么。虽然是`AutoGen`和`Crew AI`也有类似的设置。但是 `Llama Agents` 还拥有服务元数据信息和消息队列系统，以处理多重查询并发送给相应的代理。 这种消息队列系统在运行任何服务时是必要的，尤其是在负载均衡时。

这些代理服务的最终目标是托管在云环境中，如Google Cloud、AWS或Azure。 当用户通过API发出请求时，它可以处理大量请求，并将相同的任务多次发送给某个特定的代理，比如数学专家代理，而不会打扰其他代理。 你可以通过创建移动或桌面应用程序以相同方式访问API。

如有兴趣后续的内容， 接下来，我将逐步演示如何在代码中实现这一点，包括不同的设置，最终设置一个用户界面来监控这些代理， 请查看这个视频：`https://www.youtube.com/watch?v=nEQCpSd5mx8&t=155s`

下面提供官方的文档介绍、相关资源、部署教程等，进一步支撑你的行动，以提升本文的帮助力。

## 🦙 llama-agents 🤖

`llama-agents` 是一个以异步优先的框架，用于构建、迭代和生产化多智能体系统，包括多智能体通信、分布式工具执行、人机协作等功能！

在 `llama-agents` 中，每个智能体被视为一个 服务 ，不断处理传入的任务。每个智能体从一个 消息队列 中拉取和发布消息。

在 `llama-agents` 系统的顶层是 控制平面 。控制平面跟踪正在进行的任务、网络中的服务，并使用 协调器 决定哪个服务应处理任务的下一步。

下面是整体系统布局示意图。

![](img/b17a235ccf7812ed22242fe628cd6707.png)

## 安装

`llama-agents` 可以通过 `pip` 安装，主要依赖于 `llama-index-core` ：

```
pip install llama-agents
```

如果你还没有安装 `llama-index`，想要跟随这些示例，还需要

```
pip install llama-index-agent-openai
```

## 入门

最快的入门方法是使用现有的智能体（或多个智能体）并包装成启动器。

下面的示例展示了一个包含两个来自 `llama-index` 的智能体的简单示例：

首先，设置一些智能体和 `llama-agents` 系统的初始组件：

```
from llama_agents import (
    AgentService,
    AgentOrchestrator,
    ControlPlaneServer,
    SimpleMessageQueue,
)

from llama_index.core.agent import ReActAgent
from llama_index.core.tools import FunctionTool
from llama_index.llms.openai import OpenAI

# 创建一个智能体
def get_the_secret_fact() -> str:
    """返回一个秘密事实。"""
    return "The secret fact is: A baby llama is called a 'Cria'."

tool = FunctionTool.from_defaults(fn=get_the_secret_fact)

agent1 = ReActAgent.from_tools([tool], llm=OpenAI())
agent2 = ReActAgent.from_tools([], llm=OpenAI())

# 创建我们的多智能体框架组件
message_queue = SimpleMessageQueue(port=8000)
control_plane = ControlPlaneServer(
    message_queue=message_queue,
    orchestrator=AgentOrchestrator(llm=OpenAI(model="gpt-4-turbo")),
    port=8001,
)
agent_server_1 = AgentService(
    agent=agent1,
    message_queue=message_queue,
    description="Useful for getting the secret fact.",
    service_name="secret_fact_agent",
    port=8002,
)
agent_server_2 = AgentService(
    agent=agent2,
    message_queue=message_queue,
    description="Useful for getting random dumb facts.",
    service_name="dumb_fact_agent",
    port=8003,
)
```

### 本地 / 笔记本流程

接下来，当在笔记本中工作或进行更快的迭代时，我们可以在单次运行设置中启动 `llama-agents` 系统，其中一个消息在网络中传播并返回。

```
from llama_agents import LocalLauncher

# 启动它
launcher = LocalLauncher(
    [agent_server_1, agent_server_2],
    control_plane,
    message_queue,
)
result = launcher.launch_single("What is the secret fact?")

print(f"Result: {result}")
```

和任何智能体系统一样，考虑所使用的LLM的可靠性非常重要。一般来说，支持函数调用的API（如OpenAI、Anthropic、Mistral等）是最可靠的。

### 服务器流程

一旦你对你的系统感到满意，我们可以将所有服务作为独立进程启动，以提高吞吐量和可扩展性。

默认情况下，所有任务结果都会发布到特定的`human`队列，因此我们还定义了一个消费者来处理传入的结果。（未来，这个最终队列将是可配置的！）

要测试这一点，可以在脚本中使用服务器启动器：

```
from llama_agents import ServerLauncher, CallableMessageConsumer

# 额外的人类消费者
def handle_result(message) -> None:
    print(f"Got result:", message.data)

human_consumer = CallableMessageConsumer(
    handler=handle_result, message_type="human"
)

# 定义启动器
launcher = ServerLauncher(
    [agent_server_1, agent_server_2],
    control_plane,
    message_queue,
    additional_consumers=[human_consumer],
)

# 启动它！
launcher.launch_servers()
```

现在，由于一切都是服务器，你需要API请求与其交互。最简单的方法是使用我们的客户端和控制平面URL：

```
from llama_agents import LlamaAgentsClient, AsyncLlamaAgentsClient

client = LlamaAgentsClient("<control plane URL>")  # i.e. http://127.0.0.1:8001
task_id = client.create_task("What is the secret fact?")
# <等待几秒钟>
# 返回TaskResult或None（如果未完成）
result = client.get_task_result(task_id)
```

除了使用客户端或原始 `curl` 请求外，你还可以使用内置的CLI工具监控和与服务交互。

在另一个终端中，你可以运行：

```
llama-agents monitor --control-plane-url http://127.0.0.1:8000
```

![](img/d4aa5c836e469d3074177481dfc06421.png)

## 示例

你可以在我们的`examples`文件夹中找到许多示例：

- 智能体RAG + 工具服务
- 智能体协调器与本地启动器
- 智能体协调器与服务器启动器
- 智能体协调器与人机协作
- 智能体协调器与工具服务
- 流水线协调器与本地启动器
- 流水线协调器与人机协作
- 流水线协调器与智能体服务器作为工具
- 流水线协调器与查询重写RAG

## `llama-agents` 系统组件

在 `llama-agents` 中，有几个关键组件构成了整个系统：

- 消息队列 -- 消息队列作为所有服务和 控制平面 的队列。它具有将消息发布到命名队列的方法，并将消息委托给消费者。
- 控制平面 -- 控制平面是 `llama-agents` 系统的中央网关。它跟踪当前任务以及注册到系统的服务。它还包含 协调器 。
- 协调器 -- 该模块处理传入的任务并决定将其发送到哪个服务，以及如何处理服务的结果。协调器可以是智能体式的（使用LLM做决策），显式的（定义查询管道流），两者的混合，或者完全自定义的。
- 服务 -- 服务是实际工作的地方。服务接受一些传入任务和上下文，处理它，并发布结果。
- 工具服务 是一种特殊的服务，用于卸载智能体工具的计算。智能体可以装备一个调用工具服务的元工具。

## `llama-agents` 中的低级API

到目前为止，你已经看到了如何定义组件和如何启动它们。然而，在大多数生产使用场景中，你需要手动启动服务，并定义你自己的消费者！

因此，这里有一个关于如何做的快速指南！

### 启动

首先，你将需要启动所有内容。可以在一个脚本中完成，也可以使用多个脚本按服务启动，或在不同机器上，甚至在Docker镜像中。

在这个示例中，我们假设从一个脚本中启动。

```
import asyncio

# 启动消息队列
queue_task = asyncio.create_task(message_queue.launch_server())

# 等待消息队列准备好
await asyncio.sleep(1)

# 启动控制平面
control_plane_task = asyncio.create_task(self.control_plane.launch_server())

# 等待控制平面准备好
await asyncio.sleep(1)

# 注册控制平面为消费者
await self.message_queue.client.register_consumer(
    self.control_plane.as_consumer(remote=True)
)

# 注册服务
control_plane_url = (
    f"http://{self.control_plane.host}:{self.control_plane.port}"
)
service_tasks = []
for service in self.services:
    # 首先启动服务
    service_tasks.append(asyncio.create_task(service.launch_server()))

# 注册服务到消息队列
await service.register_to_message_queue()

# 注册服务到控制平面
await service.register_to_control_plane(control_plane_url)
```

完成后，你可能希望为任务结果定义一个消费者。

默认情况下，任务的结果会发布到`human`消息队列。

```
from llama_agents import (
    CallableMessageConsumer,
    RemoteMessageConsumer,
    QueueMessage,
)

def handle_result(message: QueueMessage) -> None:
    print(message.data)

human_consumer = CallableMessageConsumer(
    handler=handle_result, message_type="human"
)

message_queue.register_consumer(human_consumer)

# 或者，你可以将消息发送到任何URL
# human_consumer = RemoteMessageConsumer(url="some destination url")
# message_queue.register_consumer(human_consumer)
```

或者，如果你不想定义消费者，可以只使用`monitor`观察系统结果：

```
llama-agents monitor --control-plane-url http://127.0.0.1:8000
```

视频教程

`https://www.youtube.com/watch?v=nEQCpSd5mx8&t=155s`

`https://www.youtube.com/watch?v=2vDeBSb60xo`

知音难求，自我修炼亦艰

抓住前沿技术的机遇，与我们一起成为创新的超级个体

把握AIGC时代的个人力量

![](img/5389a6f773d9d3d6b5cfd3219ca4c5bb.png)
