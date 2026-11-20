
# 直接复制MCP

> 来源：[https://xatc0v8uz5m.feishu.cn/docx/XfYfdRvLaoE0MBxuFJlcT4s4nef](https://xatc0v8uz5m.feishu.cn/docx/XfYfdRvLaoE0MBxuFJlcT4s4nef)

`SequentialThinking`引导模型通过分步骤、有逻辑地逐步推理来解决复杂问题的方法，旨在提升思考的条理性、准确性和可解释性。

```
{
  "mcpServers": {
    "sequential-thinking": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-sequential-thinking"
      ]
    }
  }
}

{
  "mcpServers": {
    "sequential-thinking": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-sequential-thinking"
      ],
      "env": {}
    },
    "firebase-mcp-server": {
      "command": "npx",
      "args": [
        "-y",
        "firebase-tools@latest",
        "mcp"
      ],
      "env": {}
    },
    "context7": {
      "serverUrl": "https://mcp.context7.com/mcp",
      "headers": {
        "CONTEXT7_API_KEY": "ctx7sk-418b1f7b-042a-4da2-b4c1-ac4cd4816b47"
      }
    },
    "chrome-devtools": {
      "command": "npx",
      "args": [
        "-y",
        "chrome-devtools-mcp@latest"
      ]
    }
  }
}

"mcp-server-firecrawl": {
  "command": "npx",
  "args": ["-y", "firecrawl-mcp"],
  "env": {
    "FIRECRAWL_API_KEY": "fc-36a7607d77234e3a977b010ed51c0f96"
  }
}
```
