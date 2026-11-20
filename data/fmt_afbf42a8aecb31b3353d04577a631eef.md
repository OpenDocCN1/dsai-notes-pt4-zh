
# MCP 传输协议改进提案解读：从 HTTP+SSE 到"可流式 HTTP"

> 来源：[https://qianniucity.feishu.cn/docx/Ao7CdtsNMoLl1txwTsLcV4OJniX](https://qianniucity.feishu.cn/docx/Ao7CdtsNMoLl1txwTsLcV4OJniX)

![](img/41a7ddf1df44a737300b2c60f217db2c.png)

[output_sub.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/FDhDbrik8o3wkdxwUv2c5Z0FnYf)

![](img/ce25d8638bb93e7c5f37e4f37fb0df4e.png)

[MCP’s biggest bug is fixed.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/SXhxbgDzzoX2xvxNfbpcxvvZnKb)

> 地址：https://github.com/modelcontextprotocol/specification/pull/206

![](img/56ac708fab2535d89248c947cfb8b51c.png)

这个 Pull Request (#206) 提出了一个名为"可流式 HTTP"(Streamable HTTP)的新传输协议，用于替代 MCP (Model Context Protocol) 当前使用的 HTTP+SSE 传输方式。这是一个重要的技术改进，旨在解决现有传输方式的一些关键限制，同时保留其优势。

## 主要变更点

与当前的 HTTP+SSE 传输相比，新提案做出了以下改变：

1. 移除了 `/sse` 端点
2. 所有客户端→服务器的消息都通过 `/message`（或类似）端点传输
3. 所有客户端→服务器的请求可以被服务器升级为 SSE，用于发送通知/请求
4. 服务器可以选择建立会话 ID 来维持状态
5. 客户端可以通过向 `/message` 发送空 `GET` 请求来初始化 SSE 流

这种方法可以向后兼容实现，并允许服务器在需要时完全无状态运行。

## 解决的问题

当前的 HTTP+SSE 传输存在以下限制：

+ 不支持可恢复性
+ 要求服务器维护高可用性的长连接
+ 服务器消息只能通过 SSE 传递

## 新方案的优势

1. 支持无状态服务器 - 不再需要高可用性的长连接
2. 纯 HTTP 实现 - MCP 可以在普通 HTTP 服务器上实现，不一定需要 SSE
3. 基础设施兼容性 - 因为"只是 HTTP"，确保与中间件和基础设施兼容
4. 向后兼容 - 这是对当前传输方式的渐进式演进
5. 灵活的升级路径 - 服务器可以在需要时选择使用 SSE 进行流式响应

## 使用场景示例

### 无状态服务器

提案支持完全无状态的服务器实现，无需支持长连接：

1. 始终确认初始化（但无需保留任何状态）
2. 对任何传入的 `ToolListRequest` 用单个 JSON-RPC 响应
3. 处理 `CallToolRequest` 时执行工具，等待完成，然后发送单个 `CallToolResponse` 作为 HTTP 响应体

### 带流式处理的无状态服务器

即使是完全无状态且不支持长连接的服务器，在这个设计中仍然可以利用流式处理：

1. 当收到 `CallToolRequest` 时，服务器指示响应将是 SSE
2. 服务器开始执行工具
3. 工具执行过程中，服务器通过 SSE 发送任意数量的 `ProgressNotification`
4. 工具执行完成后，服务器通过 SSE 发送 `CallToolResponse`
5. 服务器关闭 SSE 流

### 有状态服务器

有状态服务器的实现与现在非常相似，主要区别是服务器需要生成会话 ID，客户端需要在每个请求中传回该 ID。服务器可以使用会话 ID 进行粘性路由或在消息总线上路由消息。

## 为什么不使用 WebSocket

团队详细讨论了将 WebSocket 作为主要远程传输方式的可能性，但最终决定不采用，原因包括：

1. 对于"RPC 式"使用场景，WebSocket 会带来不必要的运营和网络开销
2. 在浏览器中，无法为 WebSocket 附加头信息（如 `Authorization`），且第三方库无法在浏览器中从头实现 WebSocket
3. 只有 `GET` 请求可以透明升级为 WebSocket，这意味着在 `POST` 端点上需要两步升级过程，增加复杂性和延迟

团队也避免将 WebSocket 作为规范中的额外选项，以限制 MCP 官方指定的传输方式数量，避免客户端和服务器之间的组合兼容性问题。

## 待办事项

+ 将会话 ID 责任转移到服务器
+ 定义可接受的会话 ID 空间
+ 确保中间件/WAF 可以内省会话 ID
+ 使取消操作明确化
+ 要求集中式 SSE `GET` 用于服务器→客户端请求和通知
+ 将可恢复性转换为每个流的概念
+ 设计主动"结束会话"的方式
+ "如果客户端有认证令牌，应在每个 MCP 请求中包含它"

## 后续工作

+ 标准化对 JSON-RPC 批处理的支持
+ 支持流式请求体
+ 在规范中加入关于超时的建议，可能还会制定约定，如"发出进度通知应重置默认超时"

这个提案是在广泛社区讨论和反馈的基础上形成的，表明 MCP 正在积极发展以满足更广泛的使用场景需求。
