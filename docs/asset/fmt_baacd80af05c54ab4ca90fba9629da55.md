
## 1. `Modal` + `FastAPI`（多模型部署示例）

```
import modal
stub = modal.Stub("multi-modal")
gpu = modal.GPU("A10G", memory=24000)  # 限制24GB显存

# LLM 服务（vLLM + FastAPI）
@stub.function(gpu=gpu, idle_timeout=300)
@modal.web_endpoint(method="POST")
def llm(prompt: str):
    from vllm import LLM
    llm = LLM(model="vicuna-7b-v1.5")
    return llm.generate(prompt)[0].outputs[0].text

# SD 服务（FastAPI 接口）
@stub.function(gpu=gpu, idle_timeout=300)
@modal.web_endpoint(method="POST")
def sd(prompt: str):
    from diffusers import StableDiffusionPipeline
    pipe = StableDiffusionPipeline.from_pretrained("sd-v1-5", torch_dtype=torch.float16).to("cuda")
    return base64.b64encode(pipe(prompt).images[0].tobytes()).decode()

if __name__ == "__main__":
    stub.serve()  # 启动 FastAPI 网关
```

## 2. `LiteLLM` + `FastAPI`（多模型统一网关）

+   **核心优势**：专为 `LLM` 设计，可扩展多模态，`FastAPI` 原生封装，支持 `OpenAI` 兼容接口；

+   **快速启动**：

```
# 启动 LiteLLM 网关（集成 vLLM + SD + FastAPI）
litellm --model vicuna-7b-v1.5 --backend vllm --port 8000 --gpu_memory_utilization 0.4
# 扩展图片/视频模型：通过 LiteLLM 插件集成 SD/视频模型，复用 FastAPI 网关
```

## 3. `BentoML`（企业级模型管理+`FastAPI`）

+   **核心优势**：模型打包成独立 `Bento`，`FastAPI` 封装接口，显存隔离/自动释放，支持可视化管理；

+   **适用场景**：需模型版本管理、团队协作的企业级单机部署。

## 五、核心优化点（单机显存最大化利用）

1.  **显存隔离**：每个模型限制显存占比（如 `LLM` 0.4、`SD` 0.3、视频 0.3），避免显存溢出；

1.  **闲置释放**：配置 `idle_timeout=300`，5 分钟无请求自动卸载模型，释放显存；

1.  **量化优化**：

+   `LLM`：`vLLM` 启用 `INT8` 量化（`--quantization awq`）；

+   图片/视频：`TensorRT` 编译模型，降低 30%+ 显存占用；

1.  **进程隔离**：`Ray Serve`/`Modal` 自动将每个模型放入独立进程，关闭即释放显存。

## 六、选型总结

| 场景 | 推荐框架 | 核心优势 |

| --- | --- | --- |

| 单机快速落地、需灵活显存调度 | `Ray Serve` + `FastAPI` | 轻量级、显存精细化管理、`FastAPI` 原生集成 |

| 极简无配置、开箱即用 | `Modal` + `FastAPI` | 几行代码部署，自动显存优化/释放 |

| 多模型统一网关、`OpenAI` 兼容 | `LiteLLM` + `FastAPI` | 统一接口，易扩展多模态，企业级特性 |

| 企业级模型管理、版本控制 | `BentoML` + `FastAPI` | 模型打包、可视化管理、显存隔离 |

所有方案均满足「单机多模态+`FastAPI` 统一网关+显存自动优化/释放」核心需求，优先推荐 `Ray Serve` + `FastAPI`（平衡灵活性和易用性），或 `Modal`（极简无配置）。
