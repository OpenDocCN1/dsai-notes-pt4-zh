
# vLLM+FastAPI

> 来源：[https://gotoaiworld.feishu.cn/docx/SIx6dNjNmo3vduxzTF3cAIw4nJb](https://gotoaiworld.feishu.cn/docx/SIx6dNjNmo3vduxzTF3cAIw4nJb)

专门整合 `vLLM`（LLM 高性能推理）和 `FastAPI`（API 服务封装）的开源工具，无需手动拼接代码，开箱即用支持 LLM 推理优化、`FastAPI` 接口封装、显存自动管理、多模型统一网关等核心能力。以下是最贴合需求的 4 款工具，按「易用性 + 功能完整性」排序：

## 一、核心推荐：vLLM 官方 FastAPI 封装（最原生）

### 项目背景

`vLLM` 官方提供了基于 `FastAPI` 的 API 服务入口，是「`vLLM` + `FastAPI`」最原生的实现，支持 `OpenAI` 兼容接口、显存优化、多模型部署，无额外依赖。

**项目地址**：`https://github.com/vllm-project/vllm`

**核心能力**：✅ 原生集成 `FastAPI`，一键启动 LLM 推理 API；✅ 内置 `PagedAttention` 显存优化、`Continuous Batching` 高并发；✅ 支持多模型部署、`OpenAI` 兼容接口（无缝替换 `OpenAI` API）；✅ 显存自动管理，进程关闭即释放显存。

**快速使用（一行命令启动）**：

```bash
python -m vllm.entrypoints.openai.api_server --model lmsys/vicuna-7b-v1.5 --port 8000 \
  --gpu-memory-utilization 0.8  # 显存利用率限制
```

**接口调用（兼容 OpenAI）**：

```bash
curl http://localhost:8000/v1/completions -H "Content-Type: application/json" -d '{
    "model": "lmsys/vicuna-7b-v1.5",
    "prompt": "Hello!",
    "max_tokens": 100
  }'
```

**优势**：官方维护、无兼容问题，支持 `vLLM` 所有优化特性（量化、张量并行等）。

## 二、LiteLLM（多模型统一网关 + vLLM + FastAPI）

### 项目定位

`LiteLLM` 是专为 LLM 设计的统一 API 网关，核心封装了「`FastAPI` + `vLLM` + 多模型适配」，支持 100+ LLM 模型，一键实现多模型统一接口、显存管理。

**项目地址**：`https://github.com/BerriAI/liteLLM`

**核心能力**：✅ 内置 `FastAPI` 网关，无缝集成 `vLLM` 作为推理后端；✅ 统一 `OpenAI` 接口调用所有 LLM（`vLLM` 优化的模型 / 本地模型 / 云端模型）；✅ 支持显存限制、空闲超时释放、多模型进程隔离；✅ 适配多模态（可扩展 SD / 视频模型）。

**快速使用（`vLLM` + `FastAPI` 网关）**：

1. 安装：`pip install litellm`

2. 启动网关（自动集成 `vLLM`）：

```bash
litellm --model vicuna-7b-v1.5 --backend vllm --port 8000 --gpu_memory_utilization 0.8
```

3. 调用接口（OpenAI 兼容）：

```python
import openai
openai.api_base = "http://localhost:8000/v1"
openai.api_key = "dummy"
response = openai.Completion.create(model="vicuna-7b-v1.5", prompt="Hello!", max_tokens=100)
```

**优势**：多模型统一网关，支持 `vLLM` 显存优化，可扩展多模态，企业级特性（缓存、限流、监控）。

## 三、LLMDeploy（一站式 LLM 部署工具，vLLM + FastAPI）

### 项目定位

由商汤 / InternLM 开源的 LLM 部署工具，核心基于「`vLLM` + `FastAPI`」构建，支持一键部署、多模型、显存优化、`WebUI`。

**项目地址**：`https://github.com/InternLM/LLMDeploy`

**核心能力**：✅ 封装 `FastAPI` 接口，默认用 `vLLM` 作为推理引擎；✅ 支持多模型部署、显存自动分配 / 释放；✅ 内置 `WebUI`、`OpenAI` 兼容接口、批量推理；✅ 支持量化（INT4/INT8）进一步降低显存占用。

**快速使用**：

1. 安装：`pip install llmdeploy`

2. 启动 `vLLM` + `FastAPI` 服务：

```bash
llmdeploy serve --model vicuna-7b-v1.5 --port 8000 --gpu-memory 8G
```

**优势**：专为中文 LLM 优化，开箱即用，支持多模型并行部署，显存管理更贴合国内模型。

## 四、FastChat（LMSYS 开源，vLLM + FastAPI + 多模型）

### 项目定位

`LMSYS` 开源的 LLM 部署框架，核心集成「`vLLM` + `FastAPI`」，支持多 LLM 模型、分布式部署、显存自动管理。

**项目地址**：`https://github.com/lm-sys/FastChat`

**核心能力**：✅ 基于 `FastAPI` 构建 API 网关，默认集成 `vLLM` 作为高性能推理后端；✅ 支持百余种 LLM 模型，多模型共享 GPU 显存；✅ 自动显存优化、空闲模型卸载释放显存；✅ 内置控制器 / 工作器架构，支持多模型负载均衡。

**快速使用**：

1. 安装：`pip install "fschat[vllm]"`

2. 启动控制器（管理多模型）：

```bash
python -m fastchat.serve.controller
```

3. 启动 `vLLM` 工作器（`FastAPI` 接口）：

```bash
python -m fastchat.serve.vllm_worker --model-path vicuna-7b-v1.5 --gpu-memory-utilization 0.8
```

4. 启动 `FastAPI` 网关：

```bash
python -m fastchat.serve.openai_api_server --port 8000
```

**优势**：多模型调度能力强，支持分布式部署，企业级稳定性高。

## 五、选型对比（按你的需求匹配）

## 六、落地建议

1. **快速验证**：优先选「`vLLM` 官方 API」，一行命令启动，无需配置，原生支持 `FastAPI` + `vLLM`；

2. **多模型 / 多模态扩展**：选「`LiteLLM`」，可统一管理 `vLLM`（文本）+ `SD`（图片）+ 视频模型，扩展成本低；

3. **中文 LLM 部署**：选「`LLMDeploy`」，对国产模型（如 `InternLM`、`Qwen`、`Baichuan`）适配更好；

4. **企业级多模型调度**：选「`FastChat`」，支持分布式部署、显存共享，稳定性更高。

## 七、核心特性总结

这些工具均已封装好：

+ ✅ `vLLM` 的显存优化（`PagedAttention`、`Continuous Batching`）；

+ ✅ `FastAPI` 的 API 接口封装、参数校验、自动文档；

+ ✅ 显存自动管理（闲置释放、进程隔离、量化优化）；

+ ✅ `OpenAI` 兼容接口（无缝替换云端 API）。
