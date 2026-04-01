# MCP Flomo笔记助手

> 来源：[https://v11enp9ok1h.feishu.cn/docx/YMJEd4kaSov9fixWUiPcXvUNnHe](https://v11enp9ok1h.feishu.cn/docx/YMJEd4kaSov9fixWUiPcXvUNnHe)

![](img/314addd8d4ab33a6f49f2bafab0da698.png)

一个基于 TypeScript 的 MCP 服务器，帮助您将笔记写入 Flomo。

---

# mcp-server-flomo MCP服务器

将笔记写入Flomo。

这是一个基于TypeScript的MCP服务器，可帮助您将笔记写入Flomo。

## 功能

### 工具

+   write_note - 将文本笔记写入Flomo

+   需要内容作为必需参数

## 开发

安装依赖项：

```
npm install
```

构建服务器：

```
npm run build
```

用于开发时自动重建：

```
npm run watch
```

## 安装

要在Claude桌面版中使用，请添加服务器配置：

在MacOS上：~/Library/Application Support/Claude/claude_desktop_config.json 在Windows上：%APPDATA%/Claude/claude_desktop_config.json

```
{
  &quot;mcpServers&quot;: {
    &quot;mcp-server-flomo&quot;: {
      &quot;command&quot;: &quot;npx&quot;,
      &quot;args&quot;: [&quot;-y&quot;, &quot;@chatmcp/mcp-server-flomo&quot;],
      &quot;env&quot;: {
        &quot;FLOMO_API_URL&quot;: &quot;https://flomoapp.com/iwh/xxx/xxx/&quot;
      }
    }
  }
}
```

[找到您](https://v.flomoapp.com/mine?source=incoming_webhook)的Flomo_API_URL 在此处

调试

由于MCP服务器通过标准输入输出进行[通信，调试可](https://github.com/modelcontextprotocol/inspector)能会具有挑战性。我们建议使用MCP检查器，它作为包脚本可用：

```
npm run inspector
```

检查器将提供一个URL，您可以在浏览器中访问调试工具。