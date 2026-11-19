
# llama-agents:构建生产多智能体AI系统的强大框架

> 来源：[https://agijuejin.feishu.cn/docx/Z8Vyd8M6poPWEUxdonIc2hNRnZd](https://agijuejin.feishu.cn/docx/Z8Vyd8M6poPWEUxdonIc2hNRnZd)

> 作者：lucas大叔

> 原文：[https://zhuanlan.zhihu.com/p/705744470](https://zhuanlan.zhihu.com/p/705744470)

近日，LlamaIndex宣布推出 alpha 版本的 `llama-agents` ，这是一个全新的开源框架，旨在简化构建、迭代和部署多智能体 AI 系统的过程，并将智能体转变为生产微服务。无论你是在开发复杂的问答系统、协作式 AI 助手还是分布式 AI 工作流，`llama-agents` 都能为你提供实现想法所需的工具和结构。

## llama-agents 的主要特点

1. **分布式面向服务架构**：LlamaIndex 中的每个智能体都可以是独立运行的微服务，由完全可定制的 LLM 驱动的控制平面进行协调，用于路由和分配任务。
2. 通过标准化 `API` 接口进行通信：使用中央控制平面编排器在智能体之间进行交互。使用消息队列在智能体之间传递消息。
3. **定义代理和显式编排流程**：开发人员可以灵活地直接定义智能体之间的交互序列，或者交给“代理编排器”来决定哪些智能体与任务相关。
4. **易于部署**：独立启动、扩展和监控每个智能体和控制平面。
5. **可扩展性和资源管理**：使用内置的可观察性工具来监控系统和每个智能体服务的质量和性能。

让我们深入了解如何开始使用 `llama-agents` 构建自己的多智能体系统。

## 开始使用 llama-agents

首先，使用 `pip` 安装框架：

```
pip install llama-agents
```

### 基本系统设置

这是一个使用 `llama-agents` 设置基本多智能体系统的简单示例。首先，引入依赖项并设置控制平面，其中包含由 LLM 驱动的编排器。

```
import dotenv
dotenv.load_dotenv() # our .env file defines OPENAI_API_KEY
from llama_agents import (
    AgentService,
    ControlPlaneServer,
    SimpleMessageQueue,
    AgentOrchestrator,
)
from llama_index.core.agent import FunctionCallingAgentWorker
from llama_index.core.tools import FunctionTool
from llama_index.llms.openai import OpenAI
import logging

# turn on logging so we can see the system working
logging.getLogger("llama_agents").setLevel(logging.INFO)

# Set up the message queue and control plane
message_queue = SimpleMessageQueue()
control_plane = ControlPlaneServer(
    message_queue=message_queue,
    orchestrator=AgentOrchestrator(llm=OpenAI()),
)
```

接下来，用 LlamaIndex 现有的抽象创建工具，将这些工具提供给智能体，并将该智能体转变为独立的微服务：

```
# create a tool
def get_the_secret_fact() -> str:
    """Returns the secret fact."""
    return "The secret fact is: A baby llama is called a 'Cria'."

tool = FunctionTool.from_defaults(fn=get_the_secret_fact)

# Define an agent
worker = FunctionCallingAgentWorker.from_tools([tool], llm=OpenAI())
agent = worker.as_agent()

# Create an agent service
agent_service = AgentService(
    agent=agent,
    message_queue=message_queue,
    description="General purpose assistant",
    service_name="assistant",
)
```

最后，启动服务和控制平面。请注意，这里使用辅助函数在系统中运行单个查询然后退出；接下来将展示如何将其部署到生产环境中。

```
# Set up the launcher for local testing
from llama_agents import LocalLauncher

launcher = LocalLauncher(
    [agent_service],
    control_plane,
    message_queue,
)

# Run a single query through the system
result = launcher.launch_single("What's the secret fact?")
print(result)
```

## 部署多智能体系统

在本地测试完系统后，可以将其部署为一组服务，供实际生产使用。你可以按照以下方法进行设置。这与前面的示例类似，但我们添加了第二个智能体服务，并使用了不同的启动器。让我们引入依赖项并再次设置控制平面：

```
import dotenv
dotenv.load_dotenv()
from llama_agents import (
    AgentService,
    AgentOrchestrator,
    ControlPlaneServer,
    SimpleMessageQueue,
)

from llama_index.core.agent import FunctionCallingAgentWorker
from llama_index.core.tools import FunctionTool
from llama_index.llms.openai import OpenAI
import logging

# change logging level to enable or disable more verbose logging
logging.getLogger("llama_agents").setLevel(logging.INFO)

# create our multi-agent framework components
message_queue = SimpleMessageQueue()
control_plane = ControlPlaneServer(
    message_queue=message_queue,
    orchestrator=AgentOrchestrator(llm=OpenAI()),
)
```

然后和之前一样，创建一个工具和一个智能体，但这次我们将添加第二个智能体：

```
# create a tool
def get_the_secret_fact() -> str:
    """Returns the secret fact."""
    return "The secret fact is: A baby llama is called a 'Cria'."

tool = FunctionTool.from_defaults(fn=get_the_secret_fact)

# create our agents
worker1 = FunctionCallingAgentWorker.from_tools([tool], llm=OpenAI())
worker2 = FunctionCallingAgentWorker.from_tools([], llm=OpenAI())
agent1 = worker1.as_agent()
agent2 = worker2.as_agent()
```

我们将这些智能体转变为服务：

```
agent_server_1 = AgentService(
    agent=agent1,
    message_queue=message_queue,
    description="Useful for getting the secret fact.",
    service_name="secret_fact_agent",
    host="localhost",
    port=8003
)
agent_server_2 = AgentService(
    agent=agent2,
    message_queue=message_queue,
    description="Useful for getting random dumb facts.",
    service_name="dumb_fact_agent",
    host="localhost",
    port=8004
)
```

最后，将每项服务作为独立智能体启动。在这里，我们通过一个脚本完成所有操作，但每项服务都可以是完全独立的服务，独立启动和扩展：

```
from llama_agents import ServerLauncher, CallableMessageConsumer

# Additional human consumer
def handle_result(message) -> None:
    print(f"Got result:", message.data)


# the final result is published to a "human" consumer
# so we define one to handle it!
human_consumer = CallableMessageConsumer(
    handler=handle_result, message_type="human"
)

# Define Launcher
launcher = ServerLauncher(
    [agent_server_1, agent_server_2],
    control_plane,
    message_queue,
    additional_consumers=[human_consumer]
)

launcher.launch_servers()
```

## 实时监控

多智能体系统最酷的调试功能之一是内置的智能体监视器。你可以像这样启动它：

```
llama-agents monitor --control-plane-url http://127.0.0.1:8000
```

启动后，你将获得一个直观的点击式终端应用程序。可以看到两个智能体都在运行，在底部可以注入一项任务，例如查询“What is the secret fact?”你将获得一个job ID，然后可以单击它查看结果：

![](img/083856c69b368e93b0b79d3ce8ed856d.png)

## 构建查询重写 RAG 系统

让我们看一个稍微复杂的例子：查询重写 RAG 系统。该系统将重写用户查询以改进检索，然后使用重写的查询在文档上做RAG。

此示例演示了如何创建一个更复杂的系统，该系统将查询重写与 [RAG 相结合，以提高问答能力](https://link.zhihu.com/?target=https%3A//github.com/run-llama/llama-agents/blob/main/examples/query_rewrite_rag.ipynb)。请参阅 [此 notebook](https://link.zhihu.com/?target=https%3A//github.com/run-llama/llama-agents/blob/main/examples/query_rewrite_rag.ipynb) 查看完整代码。



```
import dotenv
dotenv.load_dotenv() # our .env defines OPENAI_API_KEY
from llama_index.core import VectorStoreIndex, Document
from llama_index.core.agent import FnAgentWorker
from llama_index.core import PromptTemplate
from llama_index.core.query_pipeline import QueryPipeline
from llama_index.core.query_engine import RetrieverQueryEngine
from llama_agents import (
    AgentService,
    ControlPlaneServer,
    SimpleMessageQueue,
    PipelineOrchestrator,
    ServiceComponent,
)
from llama_agents.launchers import LocalLauncher
from llama_index.llms.openai import OpenAI
import logging

# change logging level to enable or disable more verbose logging
logging.getLogger("llama_agents").setLevel(logging.INFO)

# Load and index your document
docs = [Document(text="The rabbit is a small mammal with long ears and a fluffy tail. His name is Peter.")]
index = VectorStoreIndex.from_documents(docs)

# Define a query rewrite agent
HYDE_PROMPT_STR = (
    "Please rewrite the following query to include more detail:\n{query_str}\n"
)
HYDE_PROMPT_TMPL = PromptTemplate(HYDE_PROMPT_STR)

def run_hyde_fn(state):
    prompt_tmpl, llm, input_str = (
        state["prompt_tmpl"],
        state["llm"],
        state["__task__"].input,
    )
    qp = QueryPipeline(chain=[prompt_tmpl, llm])
    output = qp.run(query_str=input_str)
    state["__output__"] = str(output)
    return state, True

hyde_agent = FnAgentWorker(
    fn=run_hyde_fn,
    initial_state={"prompt_tmpl": HYDE_PROMPT_TMPL, "llm": OpenAI()}
).as_agent()

# Define a RAG agent
def run_rag_fn(state):
    retriever, llm, input_str = (
        state["retriever"],
        state["llm"],
        state["__task__"].input,
    )
    query_engine = RetrieverQueryEngine.from_args(retriever, llm=llm)
    response = query_engine.query(input_str)
    state["__output__"] = str(response)
    return state, True

rag_agent = FnAgentWorker(
    fn=run_rag_fn,
    initial_state={"retriever": index.as_retriever(), "llm": OpenAI()}
).as_agent()

# Set up the multi-agent system
message_queue = SimpleMessageQueue()

query_rewrite_service = AgentService(
    agent=hyde_agent,
    message_queue=message_queue,
    description="Query rewriting service",
    service_name="query_rewrite",
)

rag_service = AgentService(
    agent=rag_agent,
    message_queue=message_queue,
    description="RAG service",
    service_name="rag",
)

# Create the pipeline
pipeline = QueryPipeline(chain=[
    ServiceComponent.from_service_definition(query_rewrite_service),
    ServiceComponent.from_service_definition(rag_service),
])
orchestrator = PipelineOrchestrator(pipeline)

control_plane = ControlPlaneServer(
    message_queue=message_queue,
    orchestrator=orchestrator,
)

# Set up the launcher
launcher = LocalLauncher(
    [query_rewrite_service, rag_service],
    control_plane,
    message_queue,
)

# Run a query
result = launcher.launch_single("Tell me about rabbits")
print(result)
```

## 公开roadmap

这是一个 [Al](https://link.zhihu.com/?target=https%3A//github.com/run-llama/llama-agents/discussions/49)pha 版本，这意味着我们非常希望你能就功能提出反馈，以便更好地帮助你在生产中构建多智能体系统！我们创建了一个公开路线图，展示了计划的发展方向。我们正在积极寻求公众反馈，了解哪些对你有用，哪些对你无用。

## 深入学习

`llama-agents` 为构建复杂的多智能体 AI 系统[提供](https://link.zhihu.com/?target=https%3A//github.com/run-llama/llama-agents)了强大而灵活的框架。无论[你是](https://link.zhihu.com/?target=https%3A//github.com/run-llama/llama-agents/tree/main/examples)在为新想法设计原型还是将其扩展到生产，`llama-agents` 都能为你提供实现 AI 愿景所需的工具。你可以查看 repo 以了解更多信息，尤其是示例库。

很高兴看到大家用 `llama-agents` 构建应用。祝你编码愉快！

## 参考文档

[Introducing llama-agents: A Powerful Framework for Building Production Multi-Agent AI Systems — LlamaIndex, Data Framework for LLM Applications](https://link.zhihu.com/?target=https%3A//www.llamaindex.ai/blog/introducing-llama-agents-a-powerful-framework-for-building-production-multi-agent-ai-systems)

发布于 2024-06-27 15:07 ・IP 属地江苏
