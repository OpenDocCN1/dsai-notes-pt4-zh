
# 基于 MCP + FAISS 的 RAG 框架

## 框架概述

> `index_docs` 把新文档的向量添加到索引，并把文本保存在 `_docs` 列表里；

> `retrieve_docs` 用查询向量在索引里做最近邻搜索，返回对应的文本片段。

内存中用 FAISS 的 `IndexFlatL2(1536)` 存储向量索引，维度 `1536` 与 OpenAI 的嵌入模型 `text-embedding-3-small` 模型输出一致。

> 通过 `embed_text` 函数异步调用 OpenAI Embeddings API，批量生成文本向量并返回 `float32` `numpy` 数组。

客户端（`rag-client`）将通过 `StdioServerParameters` 启动服务器进程（`.venv/bin/python server.py`），并用 MCP 的 `stdio` 通道建立双向通信。

> 然后用 DeepSeek 提供的 OpenAI 兼容接口发起对话，设置 `tools=self.tools`, `tool_choice="auto"`，当模型选择调用工具时，客户端捕获 `tool_calls`，并转发给 MCP 会话执行相应的函数；

执行结果再注入回对话中，直到不再需要工具调用。

> 在客户端 `main()` 里，先批量索引一组医学文档，然后进入命令行交互，用户输入问题后触发 `query()`，会话结束后优雅地关闭 MCP 会话和进程。

## 动手搭建基于 MCP + FAISS 的 RAG 框架

先来展示系统运行之后的状态。

[首先，在 GitHub 上](https://github.com/huangjia2019/mcp-in-action)面 Clone 下来我的代码：👉 `mcp-in-action`

![](img/2cc053a3a46a1ca6f32a1677e123b45b.png)

然后通过 `cd 02-mcp-rag/server` 命令进入 `server` 目录。

![](img/856b5cd167232c11f68dd9f2c46d81d2.png)

> 此时需要你在 `.env` 文件中设置你的 OpenAI 和 DeepSeek 大模型 `key` 等大模型应用开发必不可少的环境变量。

（如果 `OpenAI` `Key` 获取有困难，不要紧，有 `DeepSeek` 也就可以了。）

![](img/4ae215dce6d365a6233337f2e7fec647.png)

> 再使用 `uv run rag-server.py` 命令启动 MCP 服务，第一次跑的时候，`UV` 工具就会自动在 `server` 目录下安装环境（而且 `UV` 装包速度非常快，远超 `pip`），并且将服务跑起来。

![](img/addbc27293286b54e4dbd6b2bd1609f0.png)

> `server` 已经跑起来。现在再开一个新终端，通过 `cd 02-mcp-rag/client` 命令进入 `client` 目录。

![](img/f948ddfa4507f5f9a3739cb8925da4ed.png)

此时，运行 `uv run rag-client.py …/server/rag-server.py`，就可以调用 MCP 服务，运行这个极简版的医疗 RAG 系统

![](img/d72286f1593e0e5da2568e943460c452.png)

当我们询问高血压相关问题，`Client` 把我们的内部资料文档传给 MCP 服务器，并调用服务器端的 MCP 工具来进行嵌入、检索，并把检索结果返回给 `Client` 生成最后的回答。

![](img/f3883b8e93ac52adc0806c05e59da355.png)

## 代码实现：服务端

下面我们就来一步步实现服务器端的向量检索功能。

### 导入相关的库

```python
from typing import List
import faiss
import numpy as np
from openai import OpenAI
from mcp.server.fastmcp import FastMCP
from dotenv import load_dotenv
print("load_dotenv")
load_dotenv()
```

### 重点导入模块

`from typing import Any` 引入了通用类型 `Any`，用于在异步请求函数中声明返回值可以是任意 JSON 结构。

> `from mcp.server.fastmcp import FastMCP` 导入 `FastMCP` 核心类，这是构建 MCP 服务器的入口，通过该类可注册工具、资源和提示模板等。

之后，我们通过下面的语句来通过 `mcp.server.fastmcp` 模块中的 `FastMCP` 来初始化一个 `mcp server`。

```python
# 初始化 MCP Server
mcp = FastMCP("rag")
```

这里有一个 `FastMCP` 新名词需要我们了解。`FastMCP` 是 MCP 协议在 Python SDK 中的高层抽象封装，使开发者可以通过简单的装饰器和管理器快速创建并部署工具、资源和提示服务。

> 它内部自动注册了底层 `MCPServer` 的核心消息处理器（如 `list_tools`、`call_tool`、`list_resources`、`read_resource`、`list_prompts`、`get_prompt` 和 `list_resource_templates`），将 JSON-RPC 协议细节映射到直观的 Python 方法上，让实现变得简单易用。

> 虽然这个示例中使用了 `FastMCP`，但为了更清晰，后续仍将主要通过 MCP 核心消息处理器，如 `list_tools`、`call_tool`、`list_resources`、`read_resource` 等函数来操作。

> `FastMCP` 支持标准 I/O、`SSE`（Server-Sent Events）和可流式 `HTTP` 等多种传输方式，并提供 `run()` 方法一键启动，也可挂载到现有的 `ASGI` 应用中。

> 这里的“ASGI 应用”指的就是任何遵循 `ASGI`（Asynchronous Server Gateway Interface）规范、能够被 `ASGI` 服务器（如 `Uvicorn`、`Daphne` 等）驱动的 Python 异步 Web 应用。它与传统的 `WSGI`（同步接口）类似，但支持异步 I/O、`WebSocket`、`SSE`、长轮询等场景。

在下面的代码中，将进行向量索引的初始化。

```python
# 向量索引（内存版 FAISS）
_index: faiss.IndexFlatL2 = faiss.IndexFlatL2(1536)
_docs: List[str] = []
```

> 使用 FAISS 的 `IndexFlatL2` 建立了一个内存版的 `L2` 距离索引，向量维度为 `1536`（需与后续生成的嵌入维度一致）。 `_docs` 列表用于保存原始文本，以便后续检索时通过索引位置映射回对应文档。

接下来定义嵌入生成的函数。

```python
# OpenAI API（用于生成嵌入）
openai = OpenAI()

async def embed_text(texts: List[str]) -> np.ndarray:
    resp = openai.embeddings.create(
        model="text-embedding-3-small",
        input=texts,
        encoding_format="float"
    )
    return np.array([d.embedding for d in resp.data], dtype='float32')
```

> 利用 `OpenAI` 客户端异步调用 `text-embedding-3-small` 模型，一次性对一个字符串列表生成嵌入向量。将返回结果中的每个 `d.embedding` 提取出来，组装成一个 `float32` 类型的 `numpy` 数组，供后续加索引或查询使用。

目前为止一切就绪了。可以开始定义 MCP 工具了。`FastMCP` 使用 `@mcp.tool()` 装饰同步或异步函数，即可将其注册为可被 `LLM` 调用的工具。工具函数还可通过添加 `Context` 类型注入请求上下文，以实现进度报告、日志记录和资源访问等功能。

```python
@mcp.tool()
async def index_docs(docs: List[str]) -> str:
    """将一批文档加入索引。
    Args:
        docs: 文本列表
    """
    global _index, _docs
    embeddings = await embed_text(docs)
    _index.add(embeddings.astype('float32'))
    _docs.extend(docs)
    return f"已索引 {len(docs)} 篇文档，总文档数：{len(_docs)}"

@mcp.tool()
async def retrieve_docs(query: str, top_k: int = 3) -> str:
    """检索最相关文档片段。
    Args:
        query: 用户查询
        top_k: 返回的文档数
    """
    q_emb = await embed_text([query])
    D, I = _index.search(q_emb.astype('float32'), top_k)
    results = [f"[{i}] {_docs[i]}" for i in I[0] if i < len(_docs)]
    return "\n\n".join(results) if results else "未检索到相关文档。"
```

此处，`index_docs` 和 `retrieve_docs` 这两个工具函数搭配使用，就实现了一个最简版的内存 RAG 系统。

这里分别看看这两个工具函数都做了什么。

> `index_docs(docs: List[str]) -> str` 是一个异步函数，用来批量将文档文本加入到内存中的 FAISS 向量索引里。

> 它首先调用 `embed_text(docs)` 生成一组与输入文档一一对应的嵌入向量（`float32`）。

> 然后通过 `_index.add(…)` 将这些向量插入到全局的 FAISS `IndexFlatL2` 索引中；

> 同时，把原始文本追加到 `_docs` 列表，以便后续从索引位置回溯到具体内容。

函数最终返回一条字符串，报告本次新增了多少篇文档以及当前总文档数。

`retrieve_docs(query: str, top_k: int = 3) -> str` 则是一个检索工具，同样异步执行。

+   它先对用户的查询 `query` 调用 `embed_text`，生成一个查询向量 `q_emb`，

+   再用 `_index.search(q_emb, top_k)` 在 FAISS 索引中做最近邻搜索，

+   得到返回的距离数组 `D` 和索引数组 `I`。

+   然后，函数根据 `I[0]` 中的每个下标，从全局 `_docs` 列表取回对应文本，拼成类似 `[2]` 文档内容的格式，

+   并以两个换行分隔聚合成一个字符串返回；

+   如果没有命中任何文档，则返回“未检索到相关文档”。

这个服务端逻辑就设计完成了，下面是该服务 Python 脚本的“入口点”判断和服务器启动逻辑：

```python
if __name__ == "__main__":
    mcp.run(transport="stdio")
    # mcp.run(transport="tcp", host="127.0.0.1", port=8000)
```

> 这里调用了 `mcp.run(transport="stdio")`，它会在当前进程中启动一个基于标准输入 / 输出（`stdio`）通道的 MCP 服务端。

此时，MCP 服务器会监听从 `stdin` 到 `stdout` 的消息流，用来接收客户端（如你的 `rag-client`）发过来的 JSON-RPC 请求并返回响应。这个启动方式适合将服务器作为子进程嵌入到其他程序里，通过管道进行通信。

## 代码实现：客户端

客户端在 `rag-client` 目录下运行，通过 MCP 的 `stdio` 通道与刚才的服务端进程通信，而对外则调用 `DeepSeek`（或 OpenAI 兼容）接口发起聊天请求。

首先也是导入相关的库和环境变量（主要是 `DeepSeek` 大模型的 `Key`）。

```python
import sys, asyncio, os, json
from mcp import ClientSession
from mcp.client.stdio import stdio_client, StdioServerParameters
from openai import OpenAI
from dotenv import load_dotenv

load_dotenv()
```

下面，我们定义 `RagClient` 类。
