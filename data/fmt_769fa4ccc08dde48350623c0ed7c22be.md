
# 单机多模型的显存优化和自动释放最佳实践

> 来源：[https://gotoaiworld.feishu.cn/docx/X9EHdGhEIoYi8ixXeZlc85XYnph](https://gotoaiworld.feishu.cn/docx/X9EHdGhEIoYi8ixXeZlc85XYnph)

针对单机部署多模态模型（文本+图片+视频生成）并实现显存自动优化、闲置/关闭时自动释放显存的需求，以下是开箱即用的开源工具/项目，无需从零编写监控和管理逻辑，可直接适配你的场景：

## 一、核心推荐：轻量级单机多模型管理工具

这类工具专为单机多模型推理设计，内置显存优化、进程管理、自动释放逻辑，适配多模态场景：

### 1. Modal（开源版：Modal Labs）

- 项目地址：`https://github.com/modal-labs/modal`
- 核心能力：
  - 🔥 单机/云端通用，支持多模型进程隔离部署，自动管理GPU显存；
  - 内置显存优化：针对`LLM`（集成`vLLM`）、图片生成（`SD`+`XFormers`）、视频生成模型的显存优化；
  - 自动释放：模型闲置超时（可配置）自动卸载，释放显存；请求到来时自动加载；
  - 极简API：几行代码部署多模态模型，无需手动管理进程/显存。
- 适配场景：单机快速部署多模态模型，自动显存管理，无需写监控脚本。
- 示例（部署`LLM`+`SD`多模型）：

```
import modal

stub = modal.Stub("multimodal-models")
# 配置GPU资源（显存限制）
gpu_config = modal.GPU("A10G", memory=24000)  # 限制24GB显存

# 1. 部署LLM（vLLM优化）
@stub.function(gpu=gpu_config, idle_timeout=300)  # 闲置5分钟释放
@modal.web_endpoint(method="POST")
def llm_infer(text: str):
    from vllm import LLM, SamplingParams
    llm = LLM(model="vicuna-7b-v1.5", gpu_memory_utilization=0.4)
    outputs = llm.generate(text, SamplingParams(max_tokens=200))
    return {"result": outputs[0].outputs[0].text}

# 2. 部署SD图片生成（显存优化）
@stub.function(gpu=gpu_config, idle_timeout=300)
@modal.web_endpoint(method="POST")
def sd_infer(prompt: str):
    import torch
    from diffusers import StableDiffusionPipeline
    pipe = StableDiffusionPipeline.from_pretrained(
        "runwayml/stable-diffusion-v1-5",
        torch_dtype=torch.float16,
        use_xformers=True
    ).to("cuda")
    pipe.enable_attention_slicing()
    with torch.no_grad():
        image = pipe(prompt).images[0]
    return {"image_base64": image_to_base64(image)}

# 启动服务（自动管理显存，闲置释放）
if __name__ == "__main__":
    stub.serve()
```

### 2. Ray Serve（轻量级分布式/单机多模型管理）

- 项目地址：`https://github.com/ray-project/ray`
- 核心能力：
  - 单机/集群通用，支持多模型进程隔离，显存精细化调度（如指定「0.5张GPU」「8GB显存」）；
  - 内置显存优化：可集成`vLLM`（`LLM`）、`Diffusers`（`SD`）、视频生成模型的优化逻辑；
  - 自动释放：配置`idle_timeout`，模型闲置超时自动卸载释放显存；
  - 统一API：多模型通过统一网关暴露，请求路由自动触发模型加载/卸载。
- 适配场景：单机多模态模型，需灵活的显存调度+自动释放，支持自定义优化逻辑。
- 示例（部署多模态模型）：

```
import ray
from ray import serve
import torch

# 初始化Ray（单机模式）
ray.init(num_gpus=1)
serve.start()

# 1. LLM服务（vLLM，闲置5分钟释放）
@serve.deployment(num_gpus=0.4, idle_timeout=300)
class LLMDeployment:
    def __init__(self):
        from vllm import LLM
        self.llm = LLM(model="vicuna-7b-v1.5", gpu_memory_utilization=0.4)

    async def __call__(self, request):
        text = (await request.json())["text"]
        outputs = self.llm.generate(text)
        return {"result": outputs[0].outputs[0].text}

# 2. SD图片生成服务（闲置5分钟释放）
@serve.deployment(num_gpus=0.3, idle_timeout=300)
class SDDeployment:
    def __init__(self):
        self.pipe = StableDiffusionPipeline.from_pretrained(
            "runwayml/stable-diffusion-v1-5",
            torch_dtype=torch.float16,
            use_xformers=True
        ).to("cuda")

    async def __call__(self, request):
        prompt = (await request.json())["prompt"]
        with torch.no_grad():
            image = self.pipe(prompt).images[0]
        return {"image_base64": image_to_base64(image)}

# 部署服务（自动管理显存）
LLMDeployment.deploy()
SDDeployment.deploy()
```

### 3. BentoML（专为机器学习模型部署设计）

- 项目地址：`https://github.com/bentoml/BentoML`
- 核心能力：
  - 单机多模型打包/部署，进程隔离，支持显存限制和自动释放；
  - 内置显存优化：可集成`vLLM`、`SD`、视频生成模型的优化插件；
  - 自动扩展：无请求时缩容至0（释放显存），请求到来自动启动；
  - 可视化管理：`WebUI`监控模型显存占用、请求量，手动/自动释放显存。
- 适配场景：需可视化管理多模态模型，支持一键打包/部署/释放。
- 关键特性：
  - 用`bentoctl`工具打包模型时，可配置`resources.gpu.memory`限制显存；
  - 配置`autoscaling.min_replicas=0`，无请求时释放所有显存。

## 二、多模态模型专用部署工具（集成显存优化）

这类工具聚焦多模态模型，内置显存优化和自动释放逻辑，开箱即用：

### 1. InvokeAI（SD+LLM多模态部署）

- 项目地址：`https://github.com/invoke-ai/InvokeAI`
- 核心能力：
  - 专为图片生成（`SD`）+ 文本生成（`LLM`）设计的单机部署工具；
  - 内置`XFormers`、注意力分片等显存优化，自动限制显存占用；
  - 支持模型一键加载/卸载，卸载时自动释放显存；
  - `WebUI`界面，可视化管理多模型，支持闲置自动释放。
- 适配场景：以图片生成为核心，搭配`LLM`的多模态场景。

### 2. MLFlow + MLServer（通用多模型管理）

- MLFlow地址：`https://github.com/mlflow/mlflow`
- MLServer地址：`https://github.com/SeldonIO/MLServer`
- 核心能力：
  - `MLFlow`：管理多模型生命周期（加载/卸载/版本）；
  - `MLServer`：提供统一推理API，支持显存限制、闲置自动释放；
  - 适配所有模态模型（`LLM`/图片/视频），集成`vLLM`/`SD`的显存优化。
- 适配场景：企业级单机多模型管理，需标准化模型生命周期+显存管理。

## 三、显存监控+自动释放辅助工具

若你已有模型部署框架，可搭配以下工具实现显存自动释放：

### 1. gpu-manager（单机GPU显存管理工具）

- 项目地址：`https://github.com/zhreshold/gpu-manager`
- 核心能力：
  - 实时监控GPU显存/负载，支持进程级显存限制；
  - 配置「进程闲置超时自动杀死」，释放显存；
  - 轻量级，无依赖，仅需一行命令启动监控。
- 使用示例：

```
# 启动监控：闲置5分钟，显存占用>2GB的进程自动杀死
gpu-manager --idle-timeout 300 --min-memory 2048
```

### 2. nvitop（可视化GPU监控+进程管理）

- 项目地址：`https://github.com/XuehaiPan/nvitop`
- 核心能力：
  - 类似`htop`的GPU监控工具，可视化显存/进程占用；
  - 支持交互式杀死进程（释放显存），或配置脚本自动杀死闲置进程；
  - 可集成到自定义监控逻辑中，替代`py3nvml`。

## 四、选型总结（按场景匹配）

| 场景 | 推荐工具 | 核心优势 |
| --- | --- | --- |
| 极简部署多模态模型（无代码/少代码） | `Modal` | 一键部署，自动显存优化+闲置释放 |
| 需灵活自定义显存调度/多模型逻辑 | `Ray Serve` | 显存精细化调度，支持自定义优化逻辑 |
| 图片生成+`LLM`为主的多模态 | `InvokeAI` | 专为`SD`+`LLM`设计，`WebUI`可视化管理 |
| 已有部署框架，仅需显存监控+自动释放 | `gpu-manager` + `nvitop` | 轻量级，无侵入，适配任意模型部署方式 |
| 企业级标准化模型管理 | `BentoML` / `MLFlow`+`MLServer` | 模型版本管理+显存自动释放，适配多模态 |

## 五、落地建议

1. 快速验证：优先选`Modal`或`Ray Serve`，5分钟内可部署`LLM`+`SD`+视频生成模型，自动管理显存；
1. 图片生成为主：选`InvokeAI`，内置`SD`的显存优化，开箱即用；
1. 已有模型服务：无需重构，直接部署`gpu-manager`，配置闲置超时释放显存；
1. 企业级需求：选`BentoML`，支持模型打包、版本管理、显存监控全流程。

这些工具均为开源项目，无需商业授权，且已适配单机多模型的显存优化和自动释放逻辑，可直接替代「手动编写监控脚本+进程管理」的方案，大幅降低开发和维护成本。
