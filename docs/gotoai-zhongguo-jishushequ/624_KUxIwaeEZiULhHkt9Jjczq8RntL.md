# 使用 vLLM、Nginx、网关、分布式等方式实现大规模多模型 LLM 服务部署

> 来源：[https://gotoaiworld.feishu.cn/docx/SJUXdGxI8oU747xHwCbcwLOIn6c](https://gotoaiworld.feishu.cn/docx/SJUXdGxI8oU747xHwCbcwLOIn6c)

## 推理引擎基石——vLLM 的高并发机制与挑战

[最近在海光](https://zhida.zhihu.com/search?content_id=255828158&content_type=Article&match_order=1&q=Nginx&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NjY2Mjc5MjcsInEiOiJOZ2lueCIsInpoaWRhX3NvdXJjZSI6ImVudGl0eSIsImNvbnRlbnRfaWQiOjI1NTgyODE1OCwiY29udGVudF90eXBlIjoiQXJ0aWNsZSIsIm1hdGNoX29yZGVyIjoxLCJ6ZF90b2tlbiI6bnVsbH0.bRV3OXQDFRK8J6MN1IQOI7yOrM2QMSpUDuZp490K_DM&zhida_source=entity)机器上部署了服务，为了充分利用机器资源（64G*8)，使用Nginx来进行单一端点上高效部署多个 LLM 模型。

![](img/9a3f8082cda947c527ee8eef5ded4ac5.png)

在单一端点上高效部署多个 LLM 模型对于可扩展的 AI 推理至关重要。虽然 vLLM 本身不支持在单一端口[上进行多模型服务，](https://link.zhihu.com/?target=https%3A//docs.vllm.ai/en/stable/deployment/nginx.html)但我们可以通过启动多个 vLLM 实例并使用 Nginx 作为负载均衡器来实现这一目标。本指南参考了 vLLM 官方文档。

![](img/8b8b9f5b3a6669e461c0c49bf877ec66.png)

> 前期主要对Nginx方式做了介绍，也对开源的方案做了统一的学习整理，前期着重介绍了Nginx的方式，也对开源文章做了借鉴，这里有重复的地方，欢迎批评指正。

---

> 这里更进一步，从 Nginx 实现 vLL[M 多模型负载](https://zhida.zhihu.com/search?content_id=255828158&content_type=Article&match_order=1&q=LiteLLM&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NjY2Mjc5MjcsInEiOiJMaXRlTExNIiwiemhpZGFfc291cmNlIjoiZW50aXR5IiwiY29udGVudF9pZCI6MjU1ODI4MTU4LCJjb250ZW50X3R5cGUiOiJBcnRpY2xlIiwibWF0Y2hfb3JkZXIiOjEsInpkX3Rva2VuIjpudWxsfQ.cztykE6_7FGsL96HHLq8ik952Diz0a-K659oEyvMkd4&zhida_source=entity)[均衡、智能 AI ](https://zhida.zhihu.com/search?content_id=255828158&content_type=Article&match_order=1&q=Ray+Serve&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NjY2Mjc5MjcsInEiOiJSYXkgU2VydmUiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNTU4MjgxNTgsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.jl03Jl4CNOlgDQJzHT_K4ekyo0SXJbhuGGBhgWCULH8&zhida_source=entity)网关——LiteLLM 等架构与实战、分布式编排的终极形态——Ray Serve、聚合运营等方面做一个介绍，确保请求得到最优分配，实现LLM 服务的多服务部署。

在深入探讨如何“调度”模型之前，深入理解我们正在调度的核心对象——LLM 推理引擎是至关重要的。vLLM 作为目前业界最主流的开源 LLM 推理框架之一，其设计哲学和运行机制直接决定了上层服务架构的选型与配置。

### 显存管理：PagedAttention

传统的 Transformer 推理框架（如早期的 HuggingFace Transformers）在处理长文本时，采用连续的内存空间来存储 Key-Value (KV) Cache。由于请求的输出长度不可预知，框架往往需要预分配最大可能的显存空间，这导致了严重的显存碎片和浪费，类似于操作系统早期的内存管理问题。

vLLM 的核心创新在于引入了操作系统中“虚拟内存”和“分页”的概念，即 PagedAttention 1。这一机制允许 KV Cache 被切分为多个固定大小的块（Block），并存储在不连续的物理显存空间中。通过维护一张逻辑块到物理块的映射表，vLLM 实现了对显存的精细化管理。这种机制带来的直接好处是显存利用率接近 100%，从而允许在同一张 GPU 上处理更大的 Batch Size，显著提升了吞吐量。

### 批处理

除了显存管理，vLLM 的另一大特性是连续批处理（Continuous Batching），也被称为动态批处理 1。在传统的批处理中，整个 Batch 的推理速度取决于其中最长的那个序列，短序列生成完毕后必须等待长序列结束，造成计算资源的闲置。

vLLM 采用了迭代级的调度策略。在每一次迭代（生成一个 Token）后，调度器会检查是否有序列已经生成了结束符（EOS）。如果有，该序列会被立即移除，释放出的显存空间可以立刻用于插入等待队列中的新请求。这意味着，当我们在网关层面进行负载均衡时，我们可以采用更激进的策略向 vLLM 发送请求，因为它内部具备强大的动态调度能力，能够“吃掉”并并行处理大量的并发请求，只要显存尚未耗尽。

### 单机部署的局限性

尽管 vLLM 极大地挖掘了单卡的性能潜力，但单张 A100 或 H100 GPU 的显存和算力终究存在物理上限。

首先，并发瓶颈依然存在。当并发请求量超过显存所能容纳的 KV Cache 上限时，vLLM 即使有 PagedAttention 也会被迫进行 Swap 操作（将 KV Cache 换出到 CPU 内存）甚至直接拒绝请求。此时，单纯依靠排队会导致首字延迟（Time to First Token, TTFT）急剧增加。

其次，容灾需求不可忽视。单机故障（如显存 ECC 错误、驱动崩溃或物理机宕机）会导致服务完全不可用。在企业级应用中，高可用性（HA）是基本要求。

最后，多模型混合部署需求日益普遍。业务场景往往不再单一，可能同时需要 Llama-3-70B 处理复杂的逻辑推理，Qwen-72B 处理中文语境，以及专门微调过的 CodeLlama 处理代码生成任务。在一台机器上频繁加载和卸载不同的大模型权重是不切实际的，因为模型权重的加载通常需要数秒甚至数分钟。

因此，构建一个能够统一管理多个 vLLM 实例、在不同模型间智能路由流量、并能处理故障转移的“网关”或“代理层”，成为了 LLM 服务化部署的必然选择。

---

## 使用 Nginx 实现 vLLM 多模型负载均衡

对于许多拥有传统 Web 运维背景的工程师而言，Nginx 是最亲切、最成熟且性能卓越的工具。作为反向代理和负载均衡器，Nginx 经受了互联网数十年的流量考验。如果业务需求仅仅是“将流量分发给几台 GPU 服务器”，且对鉴权和计费没有复杂要求，Nginx 依然是构建 LLM 集群的坚实基石。

### 架构设计与网络拓扑

假设我们拥有两台 GPU 服务器（或在同一台服务器上运行的两个 Docker 容器），它们分别运行着 vLLM 实例，服务端口分别为 8001 和 8002。我们的目标是利用 Nginx 的 80 端口对外提供一个统一的 API 入口，使得客户端无需关心后端具体的服务器地址，且能享受到负载均衡带来的性能提升。

组件清单与角色分配：

+   Backend Node A: 运行 vLLM (模型：Llama-3-8B)，监听端口 8001。

+   Backend Node B: 运行 vLLM (模型：Llama-3-8B)，监听端口 8002。

+   Load Balancer Node: 运行 Nginx，监听端口 80，负责流量分发。

在 Docker 网络环境中，我们可以创建一个专用的桥接网络（如 vllm_nginx），将所有容器连接到该网络中，通过容器名进行 DNS 解析和通信 3。

### 实战配置：构建支持流式输出的 Nginx 负载均衡器

在使用 Nginx 代理 LLM 服务时，最常遇到的坑是流式输出（Server-Sent Events, SSE）的卡顿问题。LLM 的推理是一个逐个 Token 生成的过程，客户端通常希望每生成一个字就立即显示（打字机效果）。然而，Nginx 默认开启了 Proxy Buffering，它会尝试缓存后端响应的一部分数据，直到缓冲区满才发送给客户端。这会导致用户在浏览器端看到光标长时间不动，然后突然蹦出一大段文字，极大地破坏了 LLM 的交互体验。

以下是一份经过针对性优化的 nginx.conf 配置文件详解，它解决了流式传输、长连接复用以及负载均衡策略等关键问题 3：

Nginx

```
worker_proces[ses auto;  ](https://zhida.zhihu.com/search?content_id=255828158&amp;content_type=Article&amp;match_order=1&amp;q=OpenAI+%E5%85%BC%E5%AE%B9%E6%8E%A5%E5%8F%A3&amp;zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NjY2Mjc5MjcsInEiOiJPcGVuQUkg5YW85a655o6l5Y-jIiwiemhpZGFfc291cmNlIjoiZW50aXR5IiwiY29udGVudF9pZCI6MjU1ODI4MTU4LCJjb250ZW50X3R5cGUiOiJBcnRpY2xlIiwibWF0Y2hfb3JkZXIiOjEsInpkX3Rva2VuIjpudWxsfQ.4EGkDWdbGc3eVglE56Cc7qDpODLvaydU3HK5fpzX8QQ&amp;zhida_source=entity)# 根据 CPU 核数自动设置 Worker 进程数，利用多核优势

events {
    worker_connections 4096;  # 允许每个 Worker 处理的最大连接数
}

http {
    include       mime.types;
    default_type  application/octet-stream;
    
    # 定义上游服务器组 (Upstream)
    upstream vllm_backends {
        # 负载均衡策略选择：最少连接数 (Least Connections)
        # LLM 推理请求的处理时长差异巨大（有的生成 10 个字，有的生成 1000 个字）。
        # 使用 least_conn 策略可以将新请求分发给当前负载最轻的服务器，
        # 避免将请求发送给正忙于处理长任务的节点，比默认的轮询 (Round-Robin) 更合理。
        least_conn;
        
        # 定义后端服务器列表
        # max_fails=3 fail_timeout=30s 表示如果 30 秒内失败 3 次，则暂时剔除该节点
        server 192.168.1.101:8001 max_fails=3 fail_timeout=30s;
        server 192.168.1.102:8002 max_fails=3 fail_timeout=30s;
        
        # 保持长连接池，减少 TCP 握手开销
        # 在 LLM 这种高并发场景下，频繁建立 TCP 连接会消耗大量系统资源
        keepalive 32;
    }

    server {
        listen 80;
        server_name api.example.com;

        # 增加 Header 缓冲区大小，防止某些携带大量 Metadata 的 Token 过长导致 Header 溢出
        client_header_buffer_size 4k;
        large_client_header_buffers 4 16k;

        # 针对 OpenAI 兼容接口的路由配置
        location /v1/chat/completions {
            proxy_pass http://vllm_backends;

            # --- 关键配置区：流式输出优化 ---
            # 关闭代理缓冲，确保 SSE 流式输出流畅，实现“打字机”效果
            proxy_buffering off;
            proxy_cache off;
            
            # --- 关键配置区：长连接优化 ---
            # 设置 HTTP 版本为 1.1 以支持 Keep-Alive
            proxy_http_version 1.1;
            # 清除 Connection Header，防止 Nginx 默认将其设为 close
            proxy_set_header Connection &quot;&quot;;
            
            # --- 透传必要的 Header 信息 ---
            # 传递 Host 头，某些后端服务可能依赖此信息进行路由
            proxy_set_header Host $host;
            # 传递真实客户端 IP，这对 vLLM 的日志记录和基于 IP 的限流至关重要
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            
            # --- 超时设置 ---
            # LLM 推理可能非常缓慢，特别是生成长文本时。
            # 默认的 60s 超时往往不够，需要适当延长，防止 Nginx 主动断开连接。
            proxy_read_timeout 600s;
            proxy_connect_timeout 600s;
            proxy_send_timeout 600s;
        }
        
        # 健康检查端点（可选，依赖 Nginx Plus 或第三方模块实现主动检查）
        # 开源版 Nginx 主要依赖被动检查（即请求失败后标记）
    }
}
```

### Nginx 方案的深度优劣势分析

优势：

+   极致的性能与低开销：Nginx 基于 C 语言编写，采用事件驱动架构。在转发 LLM 请求时，其自身的延迟开销（Overhead）通常在微秒级别（< 1ms）6。相比于 Python 编写的代理，Nginx 几乎不占用 CPU 资源，能最大限度地保留系统资源给 GPU 推理使用。

+   工业级稳定性：作为全球使用最广泛的 Web 服务器，Nginx 的稳定性毋庸置疑，长时间运行几乎不会出现内存泄漏或崩溃，非常适合作为最外层的流量入口。

+   零学习成本与现有架构兼容：对于大多数已经拥有 Web 基础设施的团队来说，Nginx 是标准配置，无需引入新的技术栈即可快速上手。

局限性：

+   “哑巴”路由（无 Token 感知）：Nginx 在 OSI 模型中主要工作在 L4/L7 层，它不理解 LLM 的协议内容。它不知道一个请求的 Prompt 包含多少个 Token。在 LLM 场景下，一个包含 10 个 Token 的请求和一个包含 10,000 个 Token 的请求，在 Nginx 眼里都是一个普通的 HTTP 连接。这可能导致 Nginx 将一个极重的长文本推理任务分发给了一个虽然连接数少、但显存已经接近极限的节点，从而引发 OOM 或巨大的延迟。

+   缺乏语义级重试逻辑：虽然 Nginx 支持基于 TCP 连接失败或 HTTP 5xx 错误的重试，但它无法处理业务逻辑错误。例如，如果 vLLM 返回了一个 HTTP 200 但内容是“Model too busy”或乱码，Nginx 无法识别并触发重试。

+   多模型管理复杂：如果企业内部有 10 个不同的模型（Llama, Qwen, Mistral 等）分布在不同的服务器上，在 Nginx 中配置这些路由规则需要编写大量的 location 块或复杂的 rewrite 规则，且难以动态更新。

---

## 智能 AI 网关——LiteLLM 等架构与实战

随着 LLM 应用场景的深入，我们发现 Nginx 的“通用性”在 AI 垂直领域变成了“短板”。我们需要一个能听懂“OpenAI 协议”，能计算 Token 消耗，能在 Azure、AWS Bedrock 和自建 vLLM 之间自动切换的“智能大脑”。

这就是 LiteLLM 登场的时刻。LiteLLM 是目前 GitHub 上最活跃、功能最全面的开源 AI 网关（Proxy）之一。它通过统一的 OpenAI 兼容接口，屏蔽了后端 100+ 种不同 LLM 提供商的 API 差异，并提供了专门针对 LLM 场景的负载均衡和治理能力 7。

### 为什么选择 LiteLLM？

+   统一接口标准化：无论后端是自建的 vLLM、Ollama，还是公有云的 Azure OpenAI、Anthropic Claude、Google Vertex AI，LiteLLM 都在前端暴露统一的 openai.ChatCompletion.create 接口。这意味着业务代码无需修改即可随意切换底层模型。

+   Token 感知的智能路由：LiteLLM 能够解析请求体，计算 Token 数量，并支持基于速率（RPM/TPM）和延迟的动态路由 9。

+   模型别名系统 (Model Alias)：这是一个极具实用价值的功能。你可以让用户请求 "gpt-4"，但在后台通过 LiteLLM 将其路由到 "vllm-llama-3-70b" 或 "azure-gpt-4"，实现对用户的透明迁移和无感切换 10。

+   成本监控与预算控制：LiteLLM [内置了 T](https://zhida.zhihu.com/search?content_id=255828158&content_type=Article&match_order=1&q=Redis&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NjY2Mjc5MjcsInEiOiJSZWRpcyIsInpoaWRhX3NvdXJjZSI6ImVudGl0eSIsImNvbnRlbnRfaWQiOjI1NTgyODE1OCwiY29udGVudF90eXBlIjoiQXJ0aWNsZSIsIm1hdGNoX29yZGVyIjoxLCJ6ZF90b2tlbiI6bnVsbH0.kyN_mQhIdihU6Q9iVaP5T2uY5G3zWTDxEWxh9Tz2cc8&zhida_source=entity)oken 计数逻辑，可以对接 Redis 数据库，精确记录每个 API Key 或用户的 Token 消耗，并支持设置预算上限 11。

### 部署架构：LiteLLM Proxy

LiteLLM 通常以 Proxy Server 的形式部署，作为应用层网关。最简便的部署方式是使用 Docker。我们需要准备一个核心配置文件 config.yaml，该文件定义了所有的模型后端、路由规则和系统设置。

启动命令示例 13:

Bash

```
docker run \
    -v $(pwd)/config.yaml:/app/config.yaml \
    -p 4000:4000 \
    ghcr.io/berriai/litellm:main-latest \
    --config /app/config.yaml --detailed_debug
```

### 配置高可用与负载均衡

假设我们有三个 vLLM 实例运行 Llama-3-8B 模型，我们需要配置 LiteLLM 来对它们进行负载均衡，并且添加重试策略以应对不稳定的网络或推理错误。

config.yaml 核心配置详解 7：

YAML

```
model_list:
  # --- 定义模型组 ---
  # 这里的 model_name 都是 &quot;llama-3-8b&quot;。
  # 当客户端请求模型 &quot;llama-3-8b&quot; 时，LiteLLM 会自动在以下三个配置块中进行负载均衡。
  
  - model_name: llama-3-8b
    litellm_params:
      model: openai/llama-3-8b  # 这里的 openai/ 前缀告诉 LiteLLM 使用 OpenAI 协议适配器
      api_base: http://192.168.1.101:8000/v1
      api_key: vllm-key-1
      rpm: 1000 # Rate Limit: 声明该节点的每分钟请求数限制，用于路由权重参考

  - model_name: llama-3-8b
    litellm_params:
      model: openai/llama-3-8b
      api_base: http://192.168.1.102:8000/v1
      api_key: vllm-key-2
      rpm: 1000

  - model_name: llama-3-8b
    litellm_params:
      model: openai/llama-3-8b
      api_base: http://192.168.1.103:8000/v1
      api_key: vllm-key-3
      rpm: 1000

router_settings:
  # --- 路由策略配置 ---
  # simple-shuffle (默认): 随机路由，性能最好，开销最小，适合同构节点。
  # least-busy: 路由给当前并发请求数最少的节点，适合长尾请求场景。
  # latency-based-routing: 动态优先路由给响应最快的节点，适合异构节点或跨地域部署 。
  routing_strategy: simple-shuffle
  
  # Redis 配置：LiteLLM 使用 Redis 来存储速率限制状态、缓存和路由表。
  # 在多实例部署 LiteLLM 时，Redis 是必须的，用于共享状态。
  redis_host: redis-server
  redis_port: 6379
  
  # --- 可靠性配置 ---
  # 全局重试次数：LiteLLM 会在节点失败时自动尝试其他节点
  num_retries: 3
  # 全局超时设置：防止请求无限期挂起
  timeout: 60
  
  # --- 精细化重试策略 ---
  # 并非所有错误都值得重试（如 400 Bad Request），但网络抖动或限流必须重试。
  retry_policy:
    TimeoutErrorRetries: 3       # 超时重试 3 次
    RateLimitErrorRetries: 5     # 遇到限流 (429) 激进重试 5 次
    InternalServerErrorRetries: 2 # 500 错误重试 2 次
```

### 高级特性：混合云架构与 Fallback（降级）机制

在企业级生产环境中，最令人担忧的情况是所有自建的 vLLM 实例都挂了，或者由于突发流量导致显存全部耗尽。LiteLLM 提供了一个极其强大的 Fallback 机制，允许我们配置“兜底”方案。

例如，我们可以配置：当自建的 llama-3-8b 不可用时，自动无缝切换到商业 API（如 OpenAI 的 GPT-3.5 或 Azure），确保业务不中断，即使成本暂时上升。

YAML

```
litellm_settings:
  fallbacks:
    # 定义降级路径：如果 &quot;llama-3-8b&quot; 请求失败（重试耗尽），自动降级到 &quot;gpt-3.5-turbo&quot;
    - &quot;llama-3-8b&quot;: [&quot;gpt-3.5-turbo&quot;]
    
  # 上下文窗口降级：这是一个非常智能的功能。
  # 如果用户输入的 Prompt 超过了 Llama-3 的 8k 窗口，vLLM 会报错。
  # 此时 LiteLLM 可以自动捕捉该错误，并将请求转发给支持 128k 窗口的 &quot;gpt-4-turbo&quot; [15]。
  context_window_fallbacks:
    - &quot;llama-3-8b&quot;: [&quot;gpt-4-turbo&quot;]
```

这一配置使得 LiteLLM 超越了传统的负载均衡器，具备了业务语义层面的容灾能力，实现了“混合云”架构的弹性：平时使用廉价的自建算力，高峰期或故障期自动溢出到公有云。

### Token 级监控与预算控制

Nginx 只能看到网络流量的字节数，而 LiteLLM 能“看懂” Token。通过集成 Redis，LiteLLM 可以记录每个 API Key 的实时 Token 消耗，并据此进行计费和限流 11。

设置用户预算示例：

管理员可以创建一个 Master Key，并使用它生成受限的子 Key，分发给不同的部门或项目组：

Bash

```
curl -X POST &#x27;http://0.0.0.0:4000/key/generate&#x27; \
-H &#x27;Authorization: Bearer &lt;MASTER_KEY&gt;&#x27; \
-d &#x27;{&quot;max_budget&quot;: 10.0, &quot;duration&quot;: &quot;30d&quot;, &quot;models&quot;: [&quot;llama-3-8b&quot;]}&#x27;
```

此命令生成的一个 Key，其预算上限为 10 美元（根据配置的模型单价计算），有效期 30 天，且只能访问 Llama-3 模型。这对于公司内部的多部门成本分摊（Chargeback）至关重要。

---

## 分布式编排的终极形态——Ray Serve

如果说 LiteLLM 是一个功能强大的“智能路由器”，那么 Ray Serve 就是一个全能的“指挥中心”和“调度系统”。LiteLLM 和 Nginx 都属于控制面（Control Plane），它们假设后端的 vLLM 进程已经启动并准备就绪。而 Ray Serve 深入到了编排层（Orchestration Plane），它能够管理 Python 对象的全生命周期，实现真正的自动扩缩容（Autoscaling）和复杂的流水线编排。

### 为什么需要它？

在没有 Ray Serve 的时候，如果流量突然暴涨，运维人员需要手动去 K8s 里增加 Pod 数量，或者手动启动新的 Docker 容器。这个过程反应慢，且难以根据 GPU 的实际负载（如显存占用、队列深度）进行精确控制。

Ray Serve 与 vLLM 是天作之合。vLLM 官方就提供了对 Ray 的原生支持 1。Ray Serve 允许我们将 vLLM 封装为一个 Actor（一个有状态的 Python 进程），并根据实时流量指标动态地在集群中创建或销毁这些 Actor。

### Ray Cluster架构

在 Ray 的架构中，计算资源被池化管理。

+   Head Node: 集群的大脑，运行 Ray Controller，负责全局的资源调度、状态管理和自动扩缩容决策。

+   Worker Nodes: 实际的计算节点，提供 CPU 和 GPU 资源。

+   Proxy: 类似于 Nginx，作为集群的入口，接收外部 HTTP 请求并路由到具体的 Actor。

+   Replica (副本): 实际运行 vLLM 引擎的 Python 进程实例。

### 编写 Ray Serve 部署脚本

不同于 LiteLLM 的 YAML 配置，Ray Serve 采用 Python 代码定义服务（Infrastructure as Code），这带来了无限的灵活性。我们可以通过 Python 脚本精确控制每一个 vLLM 副本的初始化参数、GPU 分配策略以及自动扩缩容规则。

vLLM 适配 Ray Serve 的代码示例 16：

```
from ray import servefrom vllm.engine.arg_utils import AsyncEngineArgsfrom vllm.engine.async_llm_engine import AsyncLLMEnginefrom starlette.requests import Requestfrom starlette.responses import StreamingResponseimport json# @serve.deployment 装饰器定义了一个服务部署@serve.deployment(# 自动扩缩容配置autoscaling_config={&quot;min_replicas&quot;: 1,  # 最小副本数，设为 0 可实现 Scale-to-Zero（无流量时不占资源）
        &quot;max_replicas&quot;: 10, # 最大副本数，防止资源耗尽
        &quot;target_ongoing_requests&quot;: 5, # 关键指标：每个副本的目标并发请求数
        # 当单个副本的积压请求超过 5 个时，Ray Serve 会认为该节点过载，并触发扩容},
    # 资源需求：每个副本独占 1 张 GPU
    ray_actor_options={&quot;num_gpus&quot;: 1}
)class VLLMDeployment:def __init__(self, model: str):# 在 Actor 启动时初始化 vLLM 引擎# 这一步会加载模型权重到 GPU 显存engine_args = AsyncEngineArgs(model=model)self.engine = AsyncLLMEngine.from_engine_args(engine_args)async def __call__(self, request: Request):# 解析标准 HTTP 请求json_request = await request.json()prompt = json_request[&quot;messages&quot;][-1][&quot;content&quot;]
        
        # 调用 vLLM 引擎生成# request_id 用于追踪，sampling_params 定义温度等参数results_generator = self.engine.generate(prompt, request_id=&quot;...&quot;, sampling_params=...)
        
        # 将 vLLM 的生成器转换为流式响应async def stream_results():async for request_output in results_generator:yield json.dumps({&quot;text&quot;: request_output.outputs.text}) + &quot;\n&quot;
        
        return StreamingResponse(stream_results())# 将部署绑定到具体的模型deployment = VLLMDeployment.bind(model=&quot;meta-llama/Llama-3-8B-Instruct&quot;)
```

### 自动扩缩容 (Autoscaling) 的黑魔法

Ray Serve 的自动扩缩容机制比 Kubernetes HPA (Horizontal Pod Autoscaler) 更加灵敏和贴合 AI 场景。Kubernetes HPA 通常基于 CPU 或内存利用率，这对于 GPU 推理任务来说往往滞后且不准确。

Ray Serve 关注的是应用层指标，特别是 target_ongoing_requests 16。

+   扩容 (Upscaling)：当网关检测到平均每个副本的排队请求数超过目标值（例如 5 个）时，说明现有计算能力不足，Ray 会立即向集群申请资源，启动新的 vLLM 副本。这通常能在数秒到数分钟内完成（取决于模型加载速度）。

+   缩容 (Downscaling)：当流量下降，请求量低于目标值时，Ray 会逐步关闭闲置的副本，释放宝贵的 GPU 资源给其他任务使用。

+   Scale-to-Zero: 如果将 min_replicas 设为 0，当长时间没有请求时，所有的 GPU 资源都会被释放。当新请求到来时，Ray 会先暂存请求，启动副本，然后再处理。虽然这会带来较长的首字延迟（冷启动），但对于低频使用的内部工具或测试环境，能节省巨大的成本。

此外，Ray Serve 天然支持多卡张量并行 (Tensor Parallelism)。如果我们要部署 Llama-3-70B 这样的大模型，单张卡放不下，需要 4 张 A100。在 Ray 中，只需要简单修改装饰器 @serve.deployment(ray_actor_options={"num_gpus": 4})，Ray 就会自动寻找一台拥有 4 张空闲 GPU 的节点，并建立分布式通信环境（NCCL），大大简化了分布式推理的部署难度。

---

## 聚合与运营——OneAPI (NewAPI) 的商业化视角

在构建“二道贩子”平台（API Reseller）或者企业内部统一 AI 门户时，OneAPI（及其社区分支 NewAPI）是另一个备受关注的主流选择 8。

### 技术中间件 vs 运营平台

+   LiteLLM: 侧重于技术中间件。它追求的是极致的稳定性、高并发处理能力、协议转换的准确性。它通常作为代码的一部分或微服务网关存在，适合技术团队集成。

+   OneAPI: 侧重于运营管理。它自带完善的 Web UI 界面，提供了渠道管理、用户分组、额度充值、兑换码生成、邀请返利、详细的账单报表等功能。它的后端通常采用 Go 语言编写，并发性能优异，且内存占用极低。

### OneAPI 的多服务部署逻辑

OneAPI 将后端的 vLLM 实例抽象为“渠道 (Channel)”。

1.  渠道添加：在 OneAPI 后台，管理员可以添加一个类型为 “OpenAI” 的渠道，并将 Base URL 指向 vLLM 的地址（如 http://192.168.1.101:8000）。

1.  加权负载均衡：OneAPI 支持添加多个拥有相同模型的渠道，并为每个渠道设置权重 (Weight)。例如，你可以给一台 A100 服务器的渠道设置权重 10，给一台 T4 服务器的渠道设置权重 1。OneAPI 会根据权重进行加权轮询（Weighted Round Robin），确保强者多劳 22。

1.  高可用与自动禁用：OneAPI 具备自动健康检查机制。如果某个 vLLM 实例连续报错或超时，OneAPI 会将其标记为“已禁用”，并将流量自动转移到其他健康渠道。一段时间后，它会尝试半开状态探测恢复。

对于需要快速搭建一套带 Billing（计费）系统的对外服务，或者需要对内部大量用户进行精细化配额管理的团队，OneAPI + vLLM 是落地速度最快的组合。

---

## 方案对比与选型指南

面对 Nginx、LiteLLM、Ray Serve、OneAPI 四大流派，架构师该如何抉择？我们将从性能、功能复杂度、运维成本三个维度进行深度对比。

### 6性能开销

关于引入网关是否会影响 LLM 性能，是很多开发者关心的问题。

+   Nginx: 几乎没有开销，在 10,000 QPS 下依然保持微秒级延迟 6。

+   LiteLLM: 由于是 Python 编写，且需要解析 HTTP Body 进行 Token 计算，会引入数毫秒的延迟。在极高并发（>1000 QPS）下，如果不开启多进程 Worker 模式，Python 的 GIL 锁可能会成为瓶颈 6。

+   Ray Serve: 同样是 Python，但其采用了 Actor 模型和 Zero-Copy 技术，在大规模集群下的扩展性优于单机 LiteLLM 代理。

结论：考虑到 LLM 推理本身通常需要 2-5 秒（甚至更长），网关层增加 10-20ms 的延迟在 99% 的场景下是完全可以忽略不计的。相比之下，系统的稳定性、可观测性和容错能力远比这几毫秒的延迟重要。

在 2025 年的 AI 基础设施建设中，我们不再推荐“裸奔”使用 vLLM，也不建议仅使用 Nginx 这种“不懂模型”的组件，对于前期Nginx 的方式可以进行升级迭代了，创作不易欢迎点赞收藏。

**推荐的终极架构：三层汉堡包模式**

1.  最底层（计算层）：vLLM 运行在 GPU 容器中，专注推理，暴露 OpenAI 兼容接口。

1.  中间层（编排层）：Ray Serve 管理 vLLM 容器的生命周期。如果你的集群需要动态扩缩容，这一层必不可少。如果机器固定且规模较小，可以使用 Docker Compose 代替。

1.  最上层（网关层）：LiteLLM（或 OneAPI）作为统一入口。

+   对外：提供唯一的 API Endpoint 和 Key，屏蔽内部拓扑结构。

+   对内：智能路由到后端的 Ray Serve 集群或 vLLM 实例。

+   治理：处理日志记录、审计、Token 限制、Fallback 策略和缓存。

多模型服务的部署不仅仅是一个技术堆栈的选择问题，更是对计算成本、服务稳定性和用户体验的平衡艺术。从 Nginx 的粗犷转发，到 LiteLLM 的精细化治理，再到 Ray Serve 的弹性伸缩，工具的演进折射出 AI 工程化日益成熟的趋势。

对于初创团队，LiteLLM + vLLM 是最快的起步方式；对于需要精细化运营的平台，OneAPI 是不二之选；而对于拥有大规模算力池的企业级应用，构建基于 Ray Serve 的调度平台则是通向 AI 原生的必经之路。希望在构建 AI 基础设施时，为大家提供有力的决策支持。