# mcp_agent.config.yaml

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/SNCCdqUoIoZ68UxiW9Ych9KXn2g](https://qqs7y1hozd1.feishu.cn/docx/SNCCdqUoIoZ68UxiW9Ych9KXn2g)

```
execution_engine: asyncio
logger:
  transports: [console, file]
  level: debug
  progress_display: true
  path_settings:
    path_pattern: &quot;logs/mcp-agent-{unique_id}.jsonl&quot;
    unique_id: &quot;timestamp&quot; # Options: &quot;timestamp&quot; or &quot;session_id&quot;
    timestamp_format: &quot;%Y%m%d_%H%M%S&quot;

mcp:
  servers:
    playwright:
      command: &quot;npx&quot;
      args: [&quot;@playwright/mcp@latest&quot;]


openai:
  # Secrets (API keys, etc.) are stored in an mcp_agent.secrets.yaml file which can be gitignored
  default_model: &quot;gpt-4o-mini&quot;
```

```
openai:
  api_key: YOUR_OPENAI_API_KEY
```