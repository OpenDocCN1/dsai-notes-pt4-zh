
# NLWeb 提供的 REST API 端点及其参数和返回格式。

> 来源：[https://qianniucity.feishu.cn/docx/N983dmImPoKS5Ax55FQc2EHHnjh](https://qianniucity.feishu.cn/docx/N983dmImPoKS5Ax55FQc2EHHnjh)

## 主要内容总结

### API 端点

- `/ask` 和 `/mcp`，两者参数和功能基本一致。
- `/mcp` 端点还支持 MCP 客户端的核心方法（如 `list_tools`、`list_prompts`、`call_tool`、`get_prompt`）。

### 请求参数

- 必需参数：`query`（自然语言查询）。
- 可选参数包括：
  - `site`：指定数据子集（如多站点支持）。
  - `prev`：逗号分隔的历史查询列表，用于上下文。
  - `decontextualized_query`：已去上下文化的查询（如有则跳过服务器端处理）。
  - `streaming`：是否开启流式返回，默认开启。
  - `query_id`：查询唯一标识（可自动生成）。
  - `mode`：返回模式（`list`、`summarize`、`generate`）。

### 返回结果

- 返回 JSON 对象，包含 `query_id`、结果属性和结果数组。
- 每个结果包括：`url`、`name`、`site`、`score`、`description`（由 LLM 生成）、`schema_object`（原始数据的 JSON 编码）。

### 无服务端会话状态

- 所有上下文需随请求传递（如历史查询），服务器不保存会话状态。

整体上，NLWeb 的 REST API 设计灵活，支持多种检索和摘要模式，便于集成和扩展。
