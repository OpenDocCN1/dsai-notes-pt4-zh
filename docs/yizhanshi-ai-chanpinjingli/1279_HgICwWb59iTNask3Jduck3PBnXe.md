# MCPYahoo Finance股票数据工具

> 来源：[https://v11enp9ok1h.feishu.cn/docx/DRV6dUZtIoml3lxftpYcdMBUnug](https://v11enp9ok1h.feishu.cn/docx/DRV6dUZtIoml3lxftpYcdMBUnug)

![](img/8e38d27b786212095cb4863145733cd0.png)

一种模型上下文协议服务器，可通过自然语言查询与雅虎财经互动，以获取股票价格、公司信息和历史财务数据。

# MCP Yahoo Finance

---

一个用于与Yahoo Finance交互的Model Context Protocol (MCP) 服务器。该服务器提供了获取定价、公司信息等功能。

> 请注意，mcp-yahoo-finance 目前正处于早期开发阶段。随着我继续开发和改进服务器，其功能和可用工具可能会发生变化和扩展。

## 安装

如果你使用的是[uv](https://docs.astral.sh/uv/)，则无需手动安装mcp-ya[hoo](https://docs.astral.sh/uv/guides/tools/)-finance。我们将使用uvx直接运行mcp-yahoo-finance。

如果你只是想使用MCP服务器，我建议你采用这种方法。

使用pip

使用pip。

```
pip install mcp-yahoo-finance
```

### 使用Git

您也可以在将仓库克隆到您的机器后安装该软件包。

```
git clone git@github.com:maxscheijen/mcp-yahoo-finance.git
cd mcp-yahoo-finance
uv sync
```

## 配置

### Claude Desktop

在你的claude_desktop_config.json中添加以下内容：

```
{
    &quot;mcpServers&quot;: {
        &quot;yahoo-finance&quot;: {
            &quot;command&quot;: &quot;uvx&quot;,
            &quot;args&quot;: [&quot;mcp-yahoo-finance&quot;]
        }
    }
}
```

您还可以使用docker：

```
{
    &quot;mcpServers&quot;: {
        &quot;yahoo-finance&quot;: {
            &quot;command&quot;: &quot;docker&quot;,
            &quot;args&quot;: [&quot;run&quot;, &quot;-i&quot;, &quot;--rm&quot;, &quot;IMAGE&quot;]
        }
    }
}
```

### VSCode

在你的.vscode/mcp.json中添加以下内容：

```
{
    &quot;servers&quot;: {
        &quot;yahoo-finance&quot;: {
            &quot;command&quot;: &quot;uvx&quot;,
            &quot;args&quot;: [&quot;mcp-yahoo-finance&quot;]
        }
    }
}
```

## 示例问题

1.  "苹果公司的股价是多少？"

1.  "苹果和谷歌之间的股价差额是多少？"

1.  "从2024-01-01到2025-01-01，苹果公司的股价变化了多少？"

## 构建

Docker:

```
docker build -t [IMAGE] .
```

## 使用MCP Inspector测试

```
npx @modelcontextprotocol/inspector uv run mcp-yahoo-finance
```