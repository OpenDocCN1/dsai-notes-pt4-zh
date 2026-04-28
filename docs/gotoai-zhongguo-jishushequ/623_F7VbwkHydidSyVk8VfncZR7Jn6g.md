# 企业级算力集群和调度平台Blog

> 来源：[https://gotoaiworld.feishu.cn/docx/Fg91dmTmEoGFsoxcclXcrvWonbh](https://gotoaiworld.feishu.cn/docx/Fg91dmTmEoGFsoxcclXcrvWonbh)

**推荐的终极架构：三层汉堡包模式**

1.  最底层（计算层）：vLLM 运行在 GPU 容器中，专注推理，暴露 OpenAI 兼容接口。

1.  中间层（编排层）：Ray Serve 管理 vLLM 容器的生命周期。如果你的集群需要动态扩缩容，这一层必不可少。如果机器固定且规模较小，可以使用 Docker Compose 代替。

1.  最上层（网关层）：LiteLLM（或 OneAPI）作为统一入口。

+   对外：提供唯一的 API Endpoint 和 Key，屏蔽内部拓扑结构。

+   对内：智能路由到后端的 Ray Serve 集群或 vLLM 实例。

+   治理：处理日志记录、审计、Token 限制、Fallback 策略和缓存。