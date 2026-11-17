
# AI Agent Service Toolkit:为使用LangGraph框架构建智能体提供全流程工具集

> 来源：[https://agijuejin.feishu.cn/docx/L2FldrvUgo2i4GxPunscUpATnAh](https://agijuejin.feishu.cn/docx/L2FldrvUgo2i4GxPunscUpATnAh)

> 作者：lucas大叔

> 原文：[https://zhuanlan.zhihu.com/p/714243259](https://zhuanlan.zhihu.com/p/714243259)

AI Agent Service Toolkit 为基于 LangGraph 的智能体服务架构提供了完整的工具集，包括 LangGraph 智能体，为智能体提供服务的 FastAPI 服务，与服务交互的客户端，以及使用客户端提供聊天界面的 Steamlit 应用。用户使用该工程提供的模板可以轻松地创建和运行基于 LangGraph 框架的智能体。

## 关键特性

用户可以前往 [https://agent-service-toolkit.streamlit.app/](https://agent-service-toolkit.streamlit.app/) 体验，该应用有如下特性：

1. LangGraph 智能体：使用 LangGraph 框架构建可定制的智能体。
2. FastAPI 服务：为智能体提供流式和非流式端点。
3. 高级流式传输：一种支持基于 token 和基于消息的流式传输的新方法。
4. Streamlit 接口：提供用户友好的聊天界面，用于与智能体进行交互。
5. 异步设计：利用 `async/wait` 来高效处理并发请求。
6. （开发中）内容审核：实现 LlamaGuard 用于内容审核。
7. 反馈机制：与 LangSmith 集成的基于打星评分的反馈系统。
8. Docker 支持：包括 `Dockerfile` 和 `docker compose` 文件，便于开发和部署。

![](img/74edf12be797543422c5bb37993de4b3.png)

![](img/426a7446b206aacb878a56f2f3355781.png)

## 架构图

![](img/3ed0f2d8ca67d9335232506dd86ac490.png)

## 关键文件

项目的结构如下：

+ `agent/agent.py` ：定义 LangGraph 智能体
+ `schema/__init__.py` ：定义服务 schema
+ `service/service.py` ：为智能体服务的 FastAPI 服务
+ `client/__init__.py` ：与智能体服务交互的客户端
+ `streamlit_app.py` ：提供聊天界面的 Streamlit 应用

## 配置与使用

1. 克隆仓库

```
git clone https://github.com/JoshuaC215/agent-service-toolkit.git
cd agent-service-toolkit
```

2. 配置环境变量：在根目录中创建 `.env` 文件并添加以下内容：

```
OPENAI_API_KEY=your_openai_api_key
GROQ_API_KEY=your_groq_api_key

# Optional, to enable simple header-based auth on the service
AUTH_SECRET=any_string_you_choose

# Optional, to enable LangSmith tracing
LANGCHAIN_TRACING_V2=true
LANGCHAIN_ENDPOINT=https://api.smith.langchain.com
LANGCHAIN_API_KEY=your_langchain_api_key
LANGCHAIN_PROJECT=your_project
```

3. 现在可以用 Docker 或仅使用 Python 在本地运行智能体服务和 Streamlit 应用。建议使用 Docker 设置简化环境设置并在更改代码时立即重新加载服务。

### Docker 配置

该项目包括便于开发和部署的 Docker 设置。`compose.yaml` 文件定义了两个服务：`agent_service` 和 `streamlit_app` 。每个服务的 `Dockerfile` 都在它们各自的目录中。

对于本地开发，建议使用 `docker compose watch` 。此功能通过在源代码中检测到更改时自动更新容器来获得更流畅的开发体验。

1. 确保系统安装了 Docker 和 Docker Compose （>=2.23.0）。

2. 以 watch 模式构建并启动服务：

```
docker compose watch
```

3. 当更改代码时，服务将自动更新：

+ 相关 `python` 文件和目录的更改将触发相关服务的更新。

+ 如果对 `requirements.txt` 文件进行更改，则需要运行 `docker compose up --build` 重新构建服务。

4. 在浏览器中导航到 http://localhost:8501 访问 Streamlit 应用。

5. 智能体服务 API 将在 http://localhost:80 提供。你也可以在 http://localhost:80/redoc 访问 OpenAPI 文档。

6. 使用 `docker compose down` 停止服务。

这种设置使你能够实时开发和测试更改，而无需手动重新启动服务。

### 无 Docker 的本地开发

你还可以在没有 Docker 的情况下在本地运行智能体服务和 Streamlit 应用，只使用 Python 虚拟环境。

1. 创建虚拟环境、安装依赖

```
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

2. 运行 FastAPI 服务

```
python run_service.py
```

3. 在单独的终端里运行 Streamlit 应用

```
streamlit run streamlit_app.py
```

4. 打开浏览器并导航到 Streamlit 提供的 URL （通常是 http://localhost:8501 ）。

### 使用 LangGraph Studio 开发

你可以安装 LangGraph Studio，如上所述将 `.env` 文件添加到根目录，然后启动 LangGraph Studio 指向 `agent/` 目录。根据需要自定义 `agent/langgraph.json` 。

### 测试

目前测试需要在没有 Docker 设置的情况下使用本地开发运行。为了测试智能体服务：

1. 确保你处于项目根目录并且已激活虚拟环境 `venv`。

2. 安装测试依赖

```
pip install -r test-requirements.txt
```

3. 用 `pytest` 执行测试

```
pytest
```

## 定制化

为你自己的场景自定义智能体：

1. 修改 `agent/research_assistant.py` 文件以更改智能体的行为和工具。或者从头开始构建一个新智能体。

2. 在 `streamlit_app.py` 中调整 Streamlit 接口以匹配智能体的能力。

## 在 AgentClient 上构建其他应用

仓库包括通用的 `client.AgentClient` ，可用于与智能体服务交互。该客户端设计得很灵活，可用于在智能体之上构建其他应用。它支持同步和异步调用，以及流式和非流式请求。

有关如何使用 `AgentClient` 的完整示例，请参阅 `run_client.py` 文件。一个简单的例子如下：

```
from client import AgentClient
client = AgentClient()

response = client.invoke("Tell me a brief joke?")
response.pretty_print()
# ================================== Ai Message ==================================
#
# A man walked into a library and asked the librarian, "Do you have any books on Pavlov's dogs and Schrödinger's cat?"
# The librarian replied, "It rings a bell, but I'm not sure if it's here or not."
```

## 参考文档

[https://github.com/JoshuaC215/agent-service-toolkit](https://github.com/JoshuaC215/agent-service-toolkit)
