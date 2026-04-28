# 单机部署多模态模型，集成 FastAPI 网关解决方案

> 来源：[https://gotoaiworld.feishu.cn/docx/Qa3PdYijwohyX3xs9Rfc9wOQnvc](https://gotoaiworld.feishu.cn/docx/Qa3PdYijwohyX3xs9Rfc9wOQnvc)

针对单机部署文本（LLM）+ 图片（SD）+ 视频生成多模态模型，且需集成 FastAPI 做统一 API 网关的需求，核心方案是「轻量级多模型管理框架 + 分模态推理引擎 + FastAPI 统一网关」——既保证各模态模型的推理性能/显存优化，又通过 FastAPI 实现统一入口、自动资源管理。以下是可直接落地的框架选型、架构设计和实操方案：

### 一、核心框架选型（单机多模态最优组合）

| 模块 | 推荐框架/工具 | 核心作用 |
| --- | --- | --- |
| **文本生成（LLM）** | vLLM（推理引擎）+ FastAPI（接口封装） | 极致显存优化（PagedAttention），高并发推理，FastAPI 封装 API |
| **图片生成（SD）** | Diffusers（基础）+ XFormers/TensorRT（显存优化）+ FastAPI（接口封装） | 低显存占用，支持 FP16/INT8 量化，FastAPI 封装推理接口 |
| **视频生成** | ModelScope/MMagic（基础）+ TensorRT（优化）+ FastAPI（接口封装） | 适配主流视频生成模型（如 Text2Video），显存优化，FastAPI 封装 |
| **统一网关/管理** | Ray Serve / Modal / LiteLLM（多模型管理）+ FastAPI（统一入口） | 进程隔离、显存自动分配/释放、闲置超时卸载，FastAPI 统一路由/参数校验 |

### 二、最优落地架构（单机多模态+FastAPI）

```
用户请求 → FastAPI 统一网关（8000端口）
  ├─ 路由分发 → Ray Serve/Modal 管理的模型进程
  │  ├─ LLM 进程（vLLM 优化，显存隔离）
  │  ├─ 图片生成进程（SD+XFormers，显存隔离）
  │  └─ 视频生成进程（ModelScope，显存隔离）
  ├─ 资源管控 → 自动启停模型、闲置释放显存
  └─ 监控 → GPU 显存/负载监控、请求日志
```

#### 核心优势：

1.  **进程隔离**：每个模态模型独立进程，避免显存冲突，关闭即释放显存；

1.  **分模态优化**：文本用 vLLM、图片用 XFormers、视频用 TensorRT，各取所长；

1.  **FastAPI 原生集成**：统一网关基于 FastAPI 构建，支持参数校验、自动文档、高并发；

1.  **自动资源管理**：闲置超时自动卸载模型释放显存，请求到来自动加载。

### 三、实操方案（基于 Ray Serve + FastAPI，单机最简）

Ray Serve 是单机多模型管理的「轻量首选」——无需集群，原生支持 FastAPI 封装、显存精细化调度、自动启停，且适配所有模态模型。

#### 步骤1：环境安装

```
# 核心依赖（Ray Serve + FastAPI + 多模态推理）
pip install ray[serve] fastapi uvicorn pydantic torch
# 分模态依赖
pip install vllm diffusers xformers modelscope mmagic tensorrt  # 按需安装
```

#### 步骤2：多模态模型 + FastAPI 统一网关实现

以下代码一次性实现「文本+图片+视频」多模态推理，基于 Ray Serve 管理进程/显存，FastAPI 作为统一接口：

```
import ray
from ray import serve
from fastapi import FastAPI, Request
from pydantic import BaseModel
import torch
import base64
from io import BytesIO

# ===================== 1. 初始化 Ray + Serve（单机模式） =====================
ray.init(num_gpus=1)  # 单机1张GPU
app = FastAPI(title=&quot;单机多模态统一网关&quot;)  # FastAPI 应用
serve.start(http_options={&quot;host&quot;: &quot;0.0.0.0&quot;, &quot;port&quot;: 8000})  # Ray Serve 集成 FastAPI

# ===================== 2. 定义统一请求参数 =====================
class MultiModalRequest(BaseModel):
    model_type: str  # llm/sd/video
    prompt: str
    # 各模型可选参数
    max_tokens: int = 200  # LLM
    width: int = 512       # SD
    height: int = 512      # SD
    video_length: int = 8  # 视频生成

# ===================== 3. 封装各模态模型（Ray Serve 部署，显存隔离） =====================
# 3.1 文本生成（LLM + vLLM）
@serve.deployment(
    name=&quot;llm_service&quot;,
    num_gpus=0.4,  # 限制显存占比（1张GPU的40%）
    idle_timeout=300  # 闲置5分钟卸载，释放显存
)
@serve.ingress(app)  # 集成到 FastAPI 网关
class LLMDeployment:
    def __init__(self):
        # 加载 vLLM 模型（显存优化）
        from vllm import LLM, SamplingParams
        self.llm = LLM(model=&quot;lmsys/vicuna-7b-v1.5&quot;, gpu_memory_utilization=0.4)
        self.sampling_params = SamplingParams(temperature=0.7, max_tokens=200)

    async def llm_infer(self, prompt: str, max_tokens: int):
        self.sampling_params.max_tokens = max_tokens
        outputs = self.llm.generate(prompt, self.sampling_params)
        return {&quot;model_type&quot;: &quot;llm&quot;, &quot;result&quot;: outputs[0].outputs[0].text}

# 3.2 图片生成（SD + XFormers）
@serve.deployment(
    name=&quot;sd_service&quot;,
    num_gpus=0.3,  # 限制显存占比（1张GPU的30%）
    idle_timeout=300
)
class SDDeployment:
    def __init__(self):
        # 加载 SD 模型（显存优化）
        from diffusers import StableDiffusionPipeline
        self.pipe = StableDiffusionPipeline.from_pretrained(
            &quot;runwayml/stable-diffusion-v1-5&quot;,
            torch_dtype=torch.float16,
            use_xformers=True
        ).to(&quot;cuda&quot;)
        self.pipe.enable_attention_slicing()  # 进一步降低显存

    def img2base64(self, image):
        buffer = BytesIO()
        image.save(buffer, format=&quot;PNG&quot;)
        return base64.b64encode(buffer.getvalue()).decode()

    async def sd_infer(self, prompt: str, width: int, height: int):
        with torch.no_grad():
            image = self.pipe(prompt, width=width, height=height).images[0]
        return {&quot;model_type&quot;: &quot;sd&quot;, &quot;result&quot;: self.img2base64(image)}

# 3.3 视频生成（ModelScope Text2Video）
@serve.deployment(
    name=&quot;video_service&quot;,
    num_gpus=0.3,  # 限制显存占比（1张GPU的30%）
    idle_timeout=300
)
class VideoDeployment:
    def __init__(self):
        # 加载视频生成模型（显存优化）
        from modelscope.pipelines import pipeline
        torch.cuda.set_per_process_memory_fraction(0.3, device=0)
        self.pipe = pipeline(
            task=&quot;text-to-video-synthesis&quot;,
            model=&quot;damo/text-to-video-synthesis&quot;,
            device=&quot;cuda&quot;,
            torch_dtype=torch.float16
        )

    async def video_infer(self, prompt: str, video_length: int):
        with torch.no_grad():
            result = self.pipe(prompt)
        return {&quot;model_type&quot;: &quot;video&quot;, &quot;result&quot;: result[&quot;output_video&quot;]}

# ===================== 4. FastAPI 统一路由（核心网关接口） =====================
@app.post(&quot;/infer&quot;)
async def unified_infer(req: MultiModalRequest):
    &quot;&quot;&quot;FastAPI 统一推理接口：路由到对应模态模型&quot;&quot;&quot;
    # 1. 初始化模型服务（Ray Serve 自动管理，未启动则加载）
    llm_service = await serve.get_deployment(&quot;llm_service&quot;).get_handle()
    sd_service = await serve.get_deployment(&quot;sd_service&quot;).get_handle()
    video_service = await serve.get_deployment(&quot;video_service&quot;).get_handle()

    # 2. 路由到对应模型
    if req.model_type == &quot;llm&quot;:
        return await llm_service.llm_infer.remote(req.prompt, req.max_tokens)
    elif req.model_type == &quot;sd&quot;:
        return await sd_service.sd_infer.remote(req.prompt, req.width, req.height)
    elif req.model_type == &quot;video&quot;:
        return await video_service.video_infer.remote(req.prompt, req.video_length)
    else:
        return {&quot;error&quot;: f&quot;不支持的模型类型：{req.model_type}&quot;}

@app.get(&quot;/gpu/status&quot;)
async def gpu_status():
    &quot;&quot;&quot;FastAPI 监控接口：查看 GPU 显存占用&quot;&quot;&quot;
    import py3nvml
    py3nvml.nvmlInit()
    handle = py3nvml.nvmlDeviceGetHandleByIndex(0)
    mem = py3nvml.nvmlDeviceGetMemoryInfo(handle)
    return {
        &quot;total_gb&quot;: mem.total / 1024 / 1024 / 1024,
        &quot;used_gb&quot;: mem.used / 1024 / 1024 / 1024,
        &quot;free_gb&quot;: mem.free / 1024 / 1024 / 1024
    }

# ===================== 5. 部署服务 =====================
if __name__ == &quot;__main__&quot;:
    # 部署所有模型服务（集成到 FastAPI 网关）
    LLMDeployment.deploy()
    SDDeployment.deploy()
    VideoDeployment.deploy()
    # 保持服务运行（FastAPI 网关监听 8000 端口）
    import time
    while True:
        time.sleep(3600)
```

#### 步骤3：启动与测试

1.  **启动服务**：

```
python multi_modal_server.py
```

1.  **测试接口**：

+   访问 http://127.0.0.1:8000/docs（FastAPI 自动生成的 Swagger UI），可直接调试：

+   统一推理接口 /infer：传入 model_type（llm/sd/video）和对应参数，触发推理；

+   GPU 监控接口 /gpu/status：查看显存占用；

+   示例请求（文本生成）：

```
curl -X POST &quot;http://127.0.0.1:8000/infer&quot; -H &quot;Content-Type: application/json&quot; -d &#x27;{
  &quot;model_type&quot;: &quot;llm&quot;,
  &quot;prompt&quot;: &quot;写一段春天的文案&quot;,
  &quot;max_tokens&quot;: 100
}&#x27;
```

### 四、备选框架（按场景适配）

如果 Ray Serve 稍重，可选择以下轻量化方案，均原生集成 FastAPI：

#### 1. Modal（极简无配置，单机/云端通用）

+   **核心优势**：几行代码部署多模态模型，自动显存优化/释放，内置 FastAPI 接口；

+   **示例**：

```
import modal
stub = modal.Stub(&quot;multi-modal&quot;)
gpu = modal.GPU(&quot;A10G&quot;, memory=24000)  # 限制24GB显存

# LLM 服务（vLLM + FastAPI）
@stub.function(gpu=gpu, idle_timeout=300)
@modal.web_endpoint(method=&quot;POST&quot;)
def llm(prompt: str):
    from vllm import LLM
    llm = LLM(model=&quot;vicuna-7b-v1.5&quot;)
    return llm.generate(prompt)[0].outputs[0].text

# SD 服务（FastAPI 接口）
@stub.function(gpu=gpu, idle_timeout=300)
@modal.web_endpoint(method=&quot;POST&quot;)
def sd(prompt: str):
    from diffusers import StableDiffusionPipeline
    pipe = StableDiffusionPipeline.from_pretrained(&quot;sd-v1-5&quot;, torch_dtype=torch.float16).to(&quot;cuda&quot;)
    return base64.b64encode(pipe(prompt).images[0].tobytes()).decode()

if __name__ == &quot;__main__&quot;:
    stub.serve()  # 启动 FastAPI 网关
```

#### 2. LiteLLM + FastAPI（多模型统一网关）

+   **核心优势**：专为 LLM 设计，可扩展多模态，FastAPI 原生封装，支持 OpenAI 兼容接口；

+   **快速启动**：

```
# 启动 LiteLLM 网关（集成 vLLM + SD + FastAPI）
litellm --model vicuna-7b-v1.5 --backend vllm --port 8000 --gpu_memory_utilization 0.4
# 扩展图片/视频模型：通过 LiteLLM 插件集成 SD/视频模型，复用 FastAPI 网关
```

#### 3. BentoML（企业级模型管理+FastAPI）

+   **核心优势**：模型打包成独立 Bento，FastAPI 封装接口，显存隔离/自动释放，支持可视化管理；

+   **适用场景**：需模型版本管理、团队协作的企业级单机部署。

### 五、核心优化点（单机显存最大化利用）

1.  **显存隔离**：每个模型限制显存占比（如 LLM 0.4、SD 0.3、视频 0.3），避免显存溢出；

1.  **闲置释放**：配置 idle_timeout=300，5 分钟无请求自动卸载模型，释放显存；

1.  **量化优化**：

+   LLM：vLLM 启用 INT8 量化（--quantization awq）；

+   图片/视频：TensorRT 编译模型，降低 30%+ 显存占用；

1.  **进程隔离**：Ray Serve/Modal 自动将每个模型放入独立进程，关闭即释放显存。

### 六、选型总结

| 场景 | 推荐框架 | 核心优势 |
| --- | --- | --- |
| 单机快速落地、需灵活显存调度 | Ray Serve + FastAPI | 轻量级、显存精细化管理、FastAPI 原生集成 |
| 极简无配置、开箱即用 | Modal + FastAPI | 几行代码部署，自动显存优化/释放 |
| 多模型统一网关、OpenAI 兼容 | LiteLLM + FastAPI | 统一接口，易扩展多模态，企业级特性 |
| 企业级模型管理、版本控制 | BentoML + FastAPI | 模型打包、可视化管理、显存隔离 |

所有方案均满足「单机多模态+FastAPI 统一网关+显存自动优化/释放」核心需求**，优先推荐** Ray Serve + FastAPI（平衡灵活性和易用性），或 Modal（极简无配置）。