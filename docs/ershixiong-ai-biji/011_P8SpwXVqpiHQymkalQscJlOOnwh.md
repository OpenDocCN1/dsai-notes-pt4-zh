# MCP协议实现：为何客户端方案优于 Web 应用的三维分析

> 来源：[https://qianniucity.feishu.cn/docx/GjLJdcHTjovS7MxU3NPcJlcdncd](https://qianniucity.feishu.cn/docx/GjLJdcHTjovS7MxU3NPcJlcdncd)

> 来自用户的提问：Model Context Protocol (MCP)中，现在看到的 MCP Host普遍都是 用客户端去做的，没有看到用web去做，怎么理解呢

Model Context Protocol（MCP）中 Host 普遍采用客户端形式而非 Web 应用的设计，主要源于协议架构特性、安全机制和技术实现需求三方面的考量：

## 一、架构特性适配

1.  **本地资源深度整合**
 MCP 的核心场景涉及直接操作系统级资源（如文件系统、本地浏览器控制），客户端应用能更直接地与操作系统 API 交互。例如 Puppeteer MCP 服务器需要本地浏览器进程操控，而 Web 应用受浏览器沙盒限制难以实现此类深度集成。

1.  **混合通信模式支持**
 MCP 规范包含同步/异步通信、双向数据流等复杂交互模式。Spring AI 的 MCP 集成显示客户端支持 STDIO、HTTP SSE 等多种传输协议，这种底层通信机制在桌面客户端中更易实现稳定连接。

## 二、安全控制机制

## 三、技术生态现状

1.  **开发工具链倾向**
 MCP Java SDK 提供 Spring Boot 集成，主要面向服务端/桌面应用开发。现有 SDK（Python/TypeScript/Java）更适配本地应用场景，Web 集成需要额外封装层。

1.  **部署模式限制**
 当前 MCP 规范要求服务器本地运行以确保资源访问安全，这与 Web 应用的远程部署模式存在冲突。Raygun 案例显示，即便是 API 访问也通过本地 MCP 服务器代理实现。

1.  **协议演进阶段**
 据 Anthropic 路线图，远程 MCP 支持正在开发中。当协议扩展远程通信能力后，Web 应用作为 Host 的技术障碍可能被逐步消除，届时可能出现混合形态的 Host 实现。

值得注意的是，Web 方案并非完全不可行，但需要解决以下技术挑战：建立安全的跨进程通信桥接（如通过 WebSocket 代理本地服务）、重构权限控制体系、以及处理浏览器环境下的资源隔离问题。随着 MCP 生态发展（目前已有 200+社区服务器），未来可能出现 Web Host 的特定实现方案。

### Citations:

1.  [h](https://docs.spring.io/spring-ai/reference/api/mcp/mcp-overview.html)ttps://docs.spring.io/spring-ai/reference/api/mcp/mcp-overview.html

1.  [https://github.com/cyanheads/model-context-protoco](https://github.com/cyanheads/model-context-protocol-resources/blob/main/guides/mcp-client-development-guide.md)l-resources/blob/main/guides/mcp-client-development-guide.md

1.  [htt](https://wandb.ai/onlineinference/mcp/reports/The-Model-Context-Protocol-MCP-by-Anthropic-Origins-functionality-and-impact--VmlldzoxMTY5NDI4MQ)ps://wandb.ai/onlineinference/mcp/reports/The-Model-Context-Protocol-MCP-by-Anthropic-Origins-functionality-and-impact--VmlldzoxMTY5NDI4MQ

1.  [ht](https://www.siddharthbharath.com/ultimate-guide-to-model-context-protocol-part-1-what-is-mcp/)tps://www.siddharthbharath.com/ultimate-guide-to-model-context-protocol-part-1-what-is-mcp/

1.  [https://raygun.com/](https://raygun.com/blog/announcing-mcp/)blog/announcing-mcp/

1.  [htt](https://www.docker.com/blog/the-model-context-protocol-simplifying-building-ai-apps-with-anthropic-claude-desktop-and-docker/)ps://www.docker.com/blog/the-model-context-protocol-simplifying-building-ai-apps-with-anthropic-claude-desktop-and-docker/

1.  [h](https://www.anthropic.com/news/model-context-protocol)ttps://www.anthropic.com/news/model-context-protocol

1.  [h](https://k33g.hashnode.dev/understanding-the-model-context-protocol-mcp)ttps://k33g.hashnode.dev/understanding-the-model-context-protocol-mcp

1.  [https://blog.typingmind.com/what-you-need-t](https://blog.typingmind.com/what-you-need-to-know-about-model-context-protocol-mcp/)o-know-about-model-context-protocol-mcp/

1.  [h](https://spec.modelcontextprotocol.io/specification/draft/architecture/)ttps://spec.modelcontextprotocol.io/specification/draft/architecture/

1.  [https://www.youtube.com](https://www.youtube.com/watch?v=3Jsh4brTjE0)/watch?v=3Jsh4brTjE0

1.  [https://www.youtube.com](https://www.youtube.com/watch?v=RyZVqne9HWA)/watch?v=RyZVqne9HWA

1.  [h](https://www.devshorts.in/p/how-to-build-your-own-mcp-server)ttps://www.devshorts.in/p/how-to-build-your-own-mcp-server

1.  [h](https://blog.marcnuri.com/model-context-protocol-mcp-introduction)ttps://blog.marcnuri.com/model-context-protocol-mcp-introduction

1.  [https://www.youtube.com](https://www.youtube.com/watch?v=MC2BwMGFRx4)/watch?v=MC2BwMGFRx4

1.  [h](https://llmindset.co.uk/posts/2025/01/mcp-files-resources-part1/)ttps://llmindset.co.uk/posts/2025/01/mcp-files-resources-part1/

1.  [h](https://www.baeldung.com/spring-ai-model-context-protocol-mcp)ttps://www.baeldung.com/spring-ai-model-context-protocol-mcp

1.  [h](https://docs.anthropic.com/en/docs/agents-and-tools/mcp)ttps://docs.anthropic.com/en/docs/agents-and-tools/mcp

1.  [https://modelcontextprot](https://modelcontextprotocol.io/introduction)ocol.io/introduction

1.  [https://workos.com/blog/mo](https://workos.com/blog/model-context-protocol)del-context-protocol

1.  [https://www.youtube.com](https://www.youtube.com/watch?v=5ZWeCKY5WZE)/watch?v=5ZWeCKY5WZE

1.  [https://modelcontextprotocol.](https://modelcontextprotocol.io/quickstart/client)io/quickstart/client

1.  [https://github.com/modelcon](https://github.com/modelcontextprotocol/servers)textprotocol/servers

1.  [https://news.ycombinator.](https://news.ycombinator.com/item?id=43302297)com/item?id=43302297

1.  [https://quarkus.io/blog/intr](https://quarkus.io/blog/introducing-mcp-servers/)oducing-mcp-servers/

1.  [https://aiagentslist.com/blog/w](https://aiagentslist.com/blog/what-is-an-mcp-server)hat-is-an-mcp-server

1.  [https://www.latent](https://www.latent.space/p/why-mcp-won).space/p/why-mcp-won

1.  [ht](https://www.apideck.com/blog/unlocking-ai-potential-how-to-quickly-set-up-a-cursor-mcp-server)tps://www.apideck.com/blog/unlocking-ai-potential-how-to-quickly-set-up-a-cursor-mcp-server

1.  [https://stackoverflow.com/questions/715063/what-is-the-differen](https://stackoverflow.com/questions/715063/what-is-the-difference-between-a-web-application-and-a-client-server-application)ce-between-a-web-application-and-a-client-server-application

1.  [https://blog.cloudflare.com/mod](https://blog.cloudflare.com/model-context-protocol/)el-context-protocol/

1.  [h](https://blog.replit.com/everything-you-need-to-know-about-mcp)ttps://blog.replit.com/everything-you-need-to-know-about-mcp

1.  [htt](https://trainingsupport.microsoft.com/en-us/mcp/forum/all/module-hosting-a-web-application-with-azure-app/a425222b-d69b-44be-9a42-ab1f5839afd7)ps://trainingsupport.microsoft.com/en-us/mcp/forum/all/module-hosting-a-web-application-with-azure-app/a425222b-d69b-44be-9a42-ab1f5839afd7

1.  [ht](https://www.reddit.com/r/ClaudeAI/comments/1h55zxd/can_someone_explain_mcp_to_me_how_are_you_using/)tps://www.reddit.com/r/ClaudeAI/comments/1h55zxd/can_someone_explain_mcp_to_me_how_are_you_using/

1.  [https://github.com/](https://github.com/modelcontextprotocol)modelcontextprotocol

1.  [h](https://daily.dev/blog/what-is-mcp-model-context-protocol)ttps://daily.dev/blog/what-is-mcp-model-context-protocol