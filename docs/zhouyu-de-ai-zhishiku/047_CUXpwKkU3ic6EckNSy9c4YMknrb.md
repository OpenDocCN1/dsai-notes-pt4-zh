# 直接复制MCP

> 来源：[https://xatc0v8uz5m.feishu.cn/docx/XfYfdRvLaoE0MBxuFJlcT4s4nef](https://xatc0v8uz5m.feishu.cn/docx/XfYfdRvLaoE0MBxuFJlcT4s4nef)

SequentialThinking:引导模型通过分步骤、有逻辑地逐步推理来解决复杂问题的方法,旨在提升思考的条理性

准确性和可解释性。

```
{
  &quot;mcpServers&quot;: {
    &quot;sequential-thinking&quot;: {
      &quot;command&quot;: &quot;npx&quot;,
      &quot;args&quot;: [
        &quot;-y&quot;,
        &quot;@modelcontextprotocol/server-sequential-thinking&quot;
      ]
    }
  }
}

{
  &quot;mcpServers&quot;: {
    &quot;sequential-thinking&quot;: {
      &quot;command&quot;: &quot;npx&quot;,
      &quot;args&quot;: [
        &quot;-y&quot;,
        &quot;@modelcontextprotocol/server-sequential-thinking&quot;
      ],
      &quot;env&quot;: {}
    },
    &quot;firebase-mcp-server&quot;: {
      &quot;command&quot;: &quot;npx&quot;,
      &quot;args&quot;: [
        &quot;-y&quot;,
        &quot;firebase-tools@latest&quot;,
        &quot;mcp&quot;
      ],
      &quot;env&quot;: {}
    },
    &quot;context7&quot;: {
      &quot;serverUrl&quot;: &quot;https://mcp.context7.com/mcp&quot;,
      &quot;headers&quot;: {
        &quot;CONTEXT7_API_KEY&quot;: &quot;ctx7sk-418b1f7b-042a-4da2-b4c1-ac4cd4816b47&quot;
      }
    },
    &quot;chrome-devtools&quot;: {
      &quot;command&quot;: &quot;npx&quot;,
      &quot;args&quot;: [
        &quot;-y&quot;,
        &quot;chrome-devtools-mcp@latest&quot;
      ]
    }
  }
}

    &quot;mcp-server-firecrawl&quot;: {
      &quot;command&quot;: &quot;npx&quot;,
      &quot;args&quot;: [&quot;-y&quot;, &quot;firecrawl-mcp&quot;],
      &quot;env&quot;: {
        &quot;FIRECRAWL_API_KEY&quot;: &quot;fc-36a7607d77234e3a977b010ed51c0f96&quot;
      }
    }

```