# 必应搜索中文

> 来源：[https://v11enp9ok1h.feishu.cn/docx/C3wJduBEVocakZxjwtCcfX7An3f](https://v11enp9ok1h.feishu.cn/docx/C3wJduBEVocakZxjwtCcfX7An3f)

![](img/086f2af81e1741d8427a66493e7a92fd.png)

一个基于 MCP (Model Context Protocol) 的中文必应搜索工具，可以直接通过 Claude 或其他支持 MCP 的 AI 来搜索必应并获取网页内容。

---

## 特点

+   支持中文搜索结果

+   无需 API 密钥，直接爬取必应搜索结果

+   提供网页内容获取功能

+   轻量级，易于安装和使用

+   专为中文用户优化

+   支持 Claude 等 AI 工具调用

## 安装

**全局安装**

```
npm install -g bing-cn-mcp
```

**或者直接通过 npx 运行**

```
npx bing-cn-mcp
```

## 使用方法

### 启动服务器

```
bing-cn-mcp
```

或者使用 npx：

```
npx bing-cn-mcp
```

### 在支持 MCP 的环境中使用

在支持 MCP 的环境（如 Cursor）中，配置 MCP 服务器来使用它：

1.  找到 MCP 配置文件（例如 .cursor/mcp.json）

1.  添加服务器配置：

```
{
  &quot;mcpServers&quot;: {
    &quot;bingcn&quot;: {
      &quot;command&quot;: &quot;npx&quot;,
      &quot;args&quot;: [
        &quot;bing-cn-mcp&quot;
      ]
    }
  }
}
```

Windows用户的配置

```
{
  &quot;mcpServers&quot;: {
    &quot;bingcnmcp&quot;: {
        &quot;command&quot;: &quot;cmd&quot;,
        &quot;args&quot;: [
          &quot;/c&quot;,
          &quot;npx&quot;,
          &quot;bing-cn-mcp&quot;
      ]
    }
  }
}
```

1.  现在你可以在 Claude 中使用 mcp__bing_search 和 mcp__fetch_webpage 工具了

## 支持的工具

### bing_search

搜索必应并获取结果列表。

**参数：**

+   query: 搜索关键词

+   num_results: 返回结果数量（默认为 5）

**fetch_webpage**

根据搜索结果 ID 获取对应网页的内容。

**参数：**

+   result_id: 从 bing_search 返回的结果 ID

## 自定义配置

你可以通过创建 .env 文件来自定义配置，例如：

```
# 用户代理设置
USER_AGENT=Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36
```