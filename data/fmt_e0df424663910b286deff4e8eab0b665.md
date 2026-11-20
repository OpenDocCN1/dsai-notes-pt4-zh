
# MCP 驱动的智能 RAG 技术文档

> 来源：[https://v11enp9ok1h.feishu.cn/docx/EQ2Udf0tgoeuJjx1BWZcDyvMn8c](https://v11enp9ok1h.feishu.cn/docx/EQ2Udf0tgoeuJjx1BWZcDyvMn8c)

## Agent 智能检索增强工作流

了解如何创建一个 MCP 驱动的智能 RAG 系统，它可以搜索向量数据库，并在需要时回退到网络搜索。

![](img/cbc137978acd7953970e277cd5f9b06d.png)

### 技术栈

+ Bright Data：用于大规模网页数据抓取

+ Qdrant：作为向量数据库

+ Cursor：作为 MCP 客户端

### 工作流程

+ 开发者通过 MCP 客户端（Cursor）输入查询

+ 客户端联系 MCP 服务器以选择相关工具

+ 工具输出返回给客户端以生成响应

## 第 1 步：启动 MCP 服务器

![](img/434ce0ac6f8f7d93479b3dddc2f184ef.png)

首先，我们定义一个带有主机 URL 和端口的 MCP 服务器

```
from mcp.server.fastmcp import FastMCP

# 创建MCP服务器
mcp = FastMCP("MCP-RAG-app",
              host="127.0.0.1",
              port=8080,
              timeout=30)
```

## 第 2 步：向量数据库 MCP 工具

通过 MCP 服务器公开的工具需要满足两个要求：

+ 必须使用 `tool` 装饰器进行装饰

+ 必须有清晰的文档字符串

下面是一个用于查询向量数据库的 MCP 工具，它存储了与机器学习相关的常见问题。

![](img/df84a0ac0104d82e5b03f75979cf6b9f.png)

```
from rag.app import Retriever, QdrantDB, EmbedData

@mcp.tool()  # 添加工具装饰器
def machine_learning_faq_retrieval_tool(query: str) -> str:
    """从机器学习常见问题收集中检索最相关的文档。当用户询问有关机器学习的问题时使用此工具。
    
    输入：
        query: str -> 用于检索最相关文档的用户查询
        
    输出：
        response: str -> 从向量数据库检索到的最相关文档
    """
    # 检查文本类型
    if not isinstance(query, str):
        raise ValueError("查询必须是字符串类型")
        
    retriever = Retriever(QdrantDB("ml_faq_collection"), EmbedData())
    return retriever.search(query)
```

## 第 3 步：网络搜索 MCP 工具

如果查询与机器学习无关，我们会使用 Bright Data 的 SERP API 进行网络搜索，从多个来源大规模抓取数据以获取相关上下文。

![](img/e7acc1da5d30759469e9d43dd3ff9782.png)

```
from rag.app import Retriever, QdrantDB, EmbedData

@mcp.tool()  # 添加工具装饰器
def bright_data_web_search_tool(query: str) -> list[str]:
    """使用Bright Data搜索给定主题的信息。
    
    输入：
        query: str -> 用于搜索信息的用户查询
        
    输出：
        context: list[str] -> 最相关的网络搜索结果列表
    """
    host = 'bd.superproxy.io'
    port = 33335
    
    username = os.getenv("BRIGHT_DATA_USERNAME")  # 来自.env文件
    password = os.getenv("BRIGHT_DATA_PASSWORD")
    
    proxy_url = f"http://{username}:{password}@{host}:{port}"
    
    formatted_query = "+".join(query.split(" "))
    url = f"https://www.google.com/search?q={formatted_query}&brd.json=1&num=50"
    response = requests.get(url, verify=False)
    
    return response.json()['organic']
```

## 第 4 步：将 MCP 服务器与 Cursor 集成

前往设置 → MCP → 添加新的全局 MCP 服务器。在 JSON 文件中，添加如下所示的内容。

![](img/e4a2a4a361c33304bf73c70cf695a97e.png)

```
{
  "mcpServers": {
    "mcp-rag-app": {
      "command": "python",
      "args": ["./path/to/server.py"],
      "host": "127.0.0.1",
      "port": 8080,
      "timeout": 36000
    }
  }
}
```

您的本地 MCP 服务器已启动并连接到 Cursor。它有两个 MCP 工具：

+ Bright Data 网络搜索工具，用于大规模抓取数据

+ 向量数据库搜索工具，用于查询相关文档

接下来，我们与 MCP 服务器进行交互：

+ 当我们提出与机器学习相关的查询时，它会调用向量数据库工具

+ 但当我们提出一般查询时，它会调用 Bright Data 网络搜索工具，从各种来源大规模收集网络数据
