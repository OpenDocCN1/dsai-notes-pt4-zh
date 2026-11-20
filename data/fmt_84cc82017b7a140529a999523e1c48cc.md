
# A Unified McP server

> 来源：[https://v11enp9ok1h.feishu.cn/docx/SBA8dbjmPo2Q9JxAhc2c18KznTf](https://v11enp9ok1h.feishu.cn/docx/SBA8dbjmPo2Q9JxAhc2c18KznTf)

本项目构建了一个 MCP 服务器，通过 MindsDB 和 Cursor IDE 支持的统一界面，使用自然语言查询和交互超过 200 个数据源。

![](img/b802110e3072886540e7c356c6565cbd.png)

## 技术栈

+ MindsDB 用于驱动我们的统一 MCP 服务器

+ Cursor 作为 MCP 主机

+ Docker 用于自托管服务器

## 工作流程

+ 用户提交查询

+ 代理连接到 MindsDB MCP 服务器以查找工具

+ 根据用户查询选择合适的工具并调用

+ 最后返回上下文相关的响应

## Docker 设置

MindsDB 提供可在 Docker 容器中运行的 Docker 镜像。通过在终端中运行以下命令，使用 Docker 镜像在本地安装 MindsDB。

![](img/24be97f2eb9d66c3bf100b4e74c72843.png)

```
docker run \              → 启动容器
--name mindsdb_container \ → 指定容器名称
-e MINDSDB_APIS=&#x27;http,mcp&#x27; \ → 指定API
-p 47334:47334 \          → HTTP端口
-p 47337:47337 \          → MCP端口
mindsdb/mindsdb           → Docker镜像
```

### 启动 MindsDB GUI

安装 Docker 镜像后，在浏览器中访问 `127.0.0.1:47334` 以打开 MindsDB 编辑器。通过此界面，您可以连接到 200 多个数据源并对其运行 SQL 查询。

### 集成数据源

让我们开始构建联邦查询引擎，将数据源连接到 MindsDB。我们使用 Slack、Gmail、GitHub 和 Hacker News 作为联邦数据源。

![](img/6adcc1cc542cbafc09426c13bb7ea291.png)

### 将 MCP 服务器与 Cursor 集成

构建联邦查询引擎后，让我们通过将数据源连接到 MindsDB 的 MCP 服务器来统一数据源。

前往：文件 → 偏好设置 → Cursor 设置 → MCP → 添加新的全局 MCP 服务器。在 JSON 文件中，添加以下内容：

![](img/af4c75f8a1573f88b6b5be390e6c2182.png)

完成！我们的 MindsDB MCP 服务器已启动并连接到 Cursor！

MCP 服务器提供两个工具：

+ `list_databases`：列出所有连接到 MindsDB 的数据源。

+ `query`：回答用户关于联邦数据的查询。

![](img/d55123f5b4dca0b26a3a48ab8fb0d5a3.png)

除了 Claude 和 Cursor，MindsDB MCP 服务器还与新的 OpenAI MCP 集成兼容。

![](img/d48af3f4ddfee55c4f642cc851a9b21f.png)
