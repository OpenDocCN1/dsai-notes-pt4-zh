
# LLM API Gateway 网关

> 来源：`https://gotoaiworld.feishu.cn/docx/EyrldHxUdotZA7xXbfncjt7lnOf`

以下是 6 个主流的 LLM 大模型 API 网关开源解决方案，均支持统一接口、密钥管理、流量管控与监控，适配单机 / 云原生部署，可直接落地多模态模型的统一网关与显存优化管理。

## 主流开源 LLM API 网关方案对比（核心能力一览）

## 核心方案详解（落地优先级排序）

### LiteLLM（最适合单机多模型快速落地）

- 项目地址：`https://github.com/BerriAI/liteLLM`

- 核心能力：

- 统一 OpenAI 接口封装 100 + 模型，支持 vLLM / SD / 视频生成等多模态；

- 代理模式支持进程级显存限制、空闲超时自动释放；

- 故障转移、负载均衡、缓存 / 限流，适配单机显存优化场景。

- 快速部署（Docker）：

  ```bash
  docker run -p 4000:4000 berriai/liteLLM:latest --model vicuna-7b-v1.5 --gpu-memory-utilization 0.4
  ```

- 优势：低代码接入，无缝集成 vLLM 等显存优化库，单机部署无依赖。

### One API（企业级模型分发首选）

- 项目地址：`https://github.com/songquanpeng/one-api`

- 核心能力：

- 统一 OpenAI 接口管理多模型，支持国内文心一言 / ChatGLM 等；

- 密钥管理、额度控制、请求统计，支持模型下线自动释放资源；

- 单二进制 / Docker 部署，WebUI 可视化管理，适合团队协作。

- 适配场景：企业内多模型统一接入、二次分发，兼顾显存与权限管控。

### APIPark（云原生 AI 网关，兼容多模态）

- 项目地址：`https://github.com/eolink/apipark`

- 核心能力：

- 云原生架构，支持 Docker / K8s 一键部署，适配单机 / 集群；

- 多模型统一接口、流量监控、限流熔断，适配文本 / 图片 / 视频模型；

- 资源隔离与显存限制，关闭服务自动释放显存，契合你的需求。

- 适配场景：企业 AI 中台、多模型容灾，需标准化 API 管理与监控。

### Ray Serve（灵活显存调度 + 自动扩缩容）

- 项目地址：`https://github.com/ray-project/ray`

- 核心能力：

- 单机 / 集群通用，支持多模型进程隔离与显存精细化调度；

- 空闲超时缩容至 0 释放显存，请求到来自动扩容；

- 无缝集成 vLLM / XFormers 等优化库，适配多模态显存优化。

- 关键配置（空闲释放）：

  ```python
  @serve.deployment(num_gpus=0.4, idle_timeout=300)  # 空闲5分钟释放
  class LLMServer: ...
  ```

### BentoML（模型版本管理 + 显存优化）

- 项目地址：`https://github.com/bentoml/BentoML`

- 核心能力：

- 多模型打包成 Bento，进程隔离部署，限制单模型显存占比；

- 支持自动扩缩容，无请求时缩容释放显存；

- WebUI 监控显存占用，手动 / 自动释放，适配企业级管理。

## 选型建议（按你的场景匹配）

1. 单机快速落地多模态 + 自动显存释放：优先选 LiteLLM，低代码集成 vLLM / SD，支持空闲释放，无需复杂配置。

2. 企业内模型分发 + 密钥 / 额度管理：选 One API，WebUI 可视化，适配国内模型，适合团队协作。

3. 需云原生扩展 + 多模型容灾：选 APIPark，Docker / K8s 一键部署，支持流量管控与资源隔离。

4. 需灵活显存调度 + 自动扩缩容：选 Ray Serve，适配单机多模型显存动态分配与空闲释放。

## 落地关键配置（显存优化 + 自动释放）

无论选择哪个方案，需重点配置：

1. 进程级显存限制：通过参数限制单模型显存占比（如 vLLM 的 `gpu_memory_utilization=0.4`）；

2. 空闲超时释放：设置 `idle_timeout=300`（5 分钟），无请求自动释放显存；

3. 进程隔离：每个模型独立进程，避免显存冲突，关闭即释放。

## 总结

以上方案均为开源免费，可直接替代手动编写监控 / 进程管理脚本。优先推荐 LiteLLM 用于单机多模态快速落地，配合 Ray Serve 实现更灵活的显存调度，或用 One API 做企业级模型分发与权限管控。
