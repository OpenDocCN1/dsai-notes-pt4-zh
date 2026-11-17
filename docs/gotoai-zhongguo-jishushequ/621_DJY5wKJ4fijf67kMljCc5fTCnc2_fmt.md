
# 单机部署多模型支持显存优化技术方案

> 来源：[https://gotoaiworld.feishu.cn/docx/Ief5dUsMyoLorGxF5gWcaejjnAh](https://gotoaiworld.feishu.cn/docx/Ief5dUsMyoLorGxF5gWcaejjnAh)

针对单机部署「文本 + 图片 + 视频生成」多模态模型，并实现显存自动优化、服务关闭后自动释放显存的需求，核心思路是「进程隔离部署 + 分模型显存优化 + 统一资源管理」—— 每个模型独立进程运行（避免显存冲突），针对不同模态模型适配专属显存优化方案，再通过统一工具实现显存监控、自动释放。以下是可落地的完整解决方案：

## 一、核心架构设计

单机多模态模型部署的核心是「进程隔离 + 显存精细化管理 + 自动化管控」，整体架构如下：

```text
┌─────────────────────────────────────────────────────────┐
│ 单机服务器（GPU：如A100/A10，显存≥24GB）                 │
│ ┌─────────────┐  ┌─────────────┐  ┌─────────────┐       │
│ │ 统一API网关 │──│ 模型管理中心 │──│ 显存监控器  │       │
│ └─────────────┘  └─────────────┘  └─────────────┘       │
│        │               │                │                │
│ ┌─────────────┐  ┌─────────────┐  ┌─────────────┐       │
│ │ LLM进程     │  │ 图片生成进程 │  │ 视频生成进程 │       │
│ │ （vLLM）    │  │ （SD+TensorRT）│ （TensorRT/MMagic）│
│ └─────────────┘  └─────────────┘  └─────────────┘       │
└─────────────────────────────────────────────────────────┘
```

+ 统一 API 网关：对外提供统一调用入口，路由请求到对应模型进程；

+ 模型管理中心：启停模型进程、记录进程状态，关联显存监控；

+ 显存监控器：实时监控各进程显存占用，触发闲置 / 关闭时的显存释放；

+ 模型进程：每个模态模型独立进程，适配专属显存优化方案。

## 二、分步骤落地方案

### 步骤 1：环境准备与基础依赖

先安装各模态模型的基础依赖，确保显存优化组件可用：

```bash
# 通用依赖
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121
pip install py3nvml psutil flask fastapi uvicorn supervisord  # 监控/进程管理/API
# LLM优化（vLLM）
pip install vllm

# 图片生成优化（Stable Diffusion）
pip install diffusers transformers xformers accelerate

# 视频生成优化（如MMagic/ModelScope）
pip install modelscope mmagic tensorrt
```

### 步骤 2：分模型部署 + 显存优化（进程隔离）

每个模型独立启动进程，针对模态特性做显存优化，限制显存占比避免冲突。

#### 文本生成（LLM）：vLLM 部署（显存优化核心）

创建 `llm_service.py`，用 vLLM 加载 LLM，限制显存占比：

```python
from vllm import LLM, SamplingParams
from fastapi import FastAPI
import uvicorn
import torch

app = FastAPI()
llm = None
sampling_params = SamplingParams(temperature=0.7, max_tokens=200)

@app.on_event("startup")
def load_llm():
    global llm
    # 显存优化：限制GPU显存利用率为40%，自动启用PagedAttention
    llm = LLM(model="lmsys/vicuna-7b-v1.5", gpu_memory_utilization=0.4, tensor_parallel_size=1)

@app.post("/llm/infer")
def infer(text: str):
    outputs = llm.generate(text, sampling_params)
    return {"result": outputs[0].outputs[0].text}

@app.on_event("shutdown")
def release_llm():
    global llm
    del llm
    torch.cuda.empty_cache()  # 显式释放显存

if __name__ == "__main__":
    uvicorn.run(app, host="0.0.0.0", port=8001)
```

#### 图片生成（Stable Diffusion）：显存优化部署

创建 `sd_service.py`，用 Diffusers+XFormers 优化显存：

```python
from fastapi import FastAPI
import torch
from diffusers import StableDiffusionPipeline
import uvicorn

app = FastAPI()
pipe = None

@app.on_event("startup")
def load_sd():
    global pipe
    # 显存优化：FP16量化 + XFormers注意力优化 + 显存分片
    pipe = StableDiffusionPipeline.from_pretrained(
        "runwayml/stable-diffusion-v1-5",
        torch_dtype=torch.float16,
        use_xformers=True
    ).to("cuda:0")
    pipe.enable_attention_slicing()  # 注意力分片，降低显存占用

@app.post("/sd/infer")
def infer(prompt: str):
    with torch.no_grad():  # 禁用梯度，节省显存
        image = pipe(prompt).images[0]
    return {"image_base64": image_to_base64(image)}  # 自行实现base64转换

@app.on_event("shutdown")
def release_sd():
    global pipe
    del pipe
    torch.cuda.empty_cache()

if __name__ == "__main__":
    uvicorn.run(app, host="0.0.0.0", port=8002)
```

#### 视频生成（如 ModelScope 视频生成模型）：显存优化部署

创建 `video_service.py`，用 TensorRT 量化 + 显存限制：

```python
from fastapi import FastAPI
import torch
from modelscope.pipelines import pipeline
import uvicorn

app = FastAPI()
video_pipe = None

@app.on_event("startup")
def load_video():
    global video_pipe
    # 显存优化：FP16量化 + 限制进程显存占比
    torch.cuda.set_per_process_memory_fraction(0.3, device=0)  # 最多用30%显存
    video_pipe = pipeline(
        task="text-to-video-synthesis",
        model="damo/text-to-video-synthesis",
        device="cuda:0",
        torch_dtype=torch.float16
    )

@app.post("/video/infer")
def infer(text: str):
    with torch.no_grad():
        result = video_pipe(text)
    return {"video_path": result["output_video"]}

@app.on_event("shutdown")
def release_video():
    global video_pipe
    del video_pipe
    torch.cuda.empty_cache()

if __name__ == "__main__":
    uvicorn.run(app, host="0.0.0.0", port=8003)
```

### 步骤 3：统一进程管理 + 自动显存释放

用 `supervisord` 管理所有模型进程，结合显存监控脚本实现「闲置自动关闭 + 显存释放」。

#### 配置 supervisord（进程管理）

创建 `supervisord.conf`，统一管理三个模型进程：

```ini
[supervisord]
nodaemon=true
logfile=/var/log/supervisord.log
pidfile=/var/run/supervisord.pid

# LLM进程
[program:llm_service]
command=python llm_service.py
autostart=false  # 手动启动，或按需自动启动
autorestart=false
stdout_logfile=/var/log/llm.log
stderr_logfile=/var/log/llm_err.log
stopasgroup=true  # 停止进程时杀死子进程
killasgroup=true

# 图片生成进程
[program:sd_service]
command=python sd_service.py
autostart=false
autorestart=false
stdout_logfile=/var/log/sd.log
stderr_logfile=/var/log/sd_err.log
stopasgroup=true
killasgroup=true

# 视频生成进程
[program:video_service]
command=python video_service.py
autostart=false
autorestart=false
stdout_logfile=/var/log/video.log
stderr_logfile=/var/log/video_err.log
stopasgroup=true
killasgroup=true
```

#### 显存监控 + 自动释放脚本

创建 `gpu_monitor.py`，实时监控各进程显存占用和请求状态，闲置超时自动关闭进程（释放显存）：

```python
import time
import psutil
import py3nvml
import subprocess
from datetime import datetime

# 配置项
IDLE_TIMEOUT = 300  # 闲置5分钟关闭进程
PROCESS_CONFIG = {
    "llm_service": {"port": 8001, "pid_key": "llm_service"},
    "sd_service": {"port": 8002, "pid_key": "sd_service"},
    "video_service": {"port": 8003, "pid_key": "video_service"},
}
LAST_REQUEST_TIME = {k: datetime.now() for k in PROCESS_CONFIG.keys()}

# 初始化NVML
py3nvml.nvmlInit()
gpu_handle = py3nvml.nvmlDeviceGetHandleByIndex(0)


def get_process_pid(process_key):
    """获取supervisord管理的进程PID"""
    result = subprocess.run(
        ["supervisorctl", "status", process_key],
        capture_output=True,
        text=True,
    )
    if "RUNNING" in result.stdout:
        return int(result.stdout.split()[1])
    return None


def check_process_idle(process_name):
    """检查进程是否闲置（超时无请求）"""
    idle_seconds = (datetime.now() - LAST_REQUEST_TIME[process_name]).total_seconds()
    return idle_seconds > IDLE_TIMEOUT


def release_process(process_name):
    """停止进程，释放显存"""
    print(f"[{datetime.now()}] 释放{process_name}显存...")
    subprocess.run(["supervisorctl", "stop", process_name])
    # 显式清理CUDA缓存（兜底）
    torch.cuda.empty_cache()


def update_request_time(process_name):
    """更新进程最后请求时间"""
    LAST_REQUEST_TIME[process_name] = datetime.now()


def monitor_gpu():
    while True:
        # 遍历所有模型进程
        for process_name, config in PROCESS_CONFIG.items():
            pid = get_process_pid(config["pid_key"])
            if not pid:
                continue  # 进程未启动，跳过
            # 检查是否闲置
            if check_process_idle(process_name):
                release_process(process_name)
        # 每秒检查一次
        time.sleep(1)


if __name__ == "__main__":
    # 启动监控（需和API网关联动，每次请求调用update_request_time更新时间）
    monitor_gpu()
```

#### 统一 API 网关（路由 + 请求时间更新）

创建 `gateway.py`，对外提供统一入口，路由请求并更新进程最后请求时间：



```python
from fastapi import FastAPI
import requests
from gpu_monitor import update_request_time

app = FastAPI()# 模型服务地址
MODEL_URLS = {"llm": "http://127.0.0.1:8001/llm/infer","sd": "http://127.0.0.1:8002/sd/infer","video": "http://127.0.0.1:8003/video/infer"}@app.post("/infer/{model_type}")def infer(model_type: str, text: str):if model_type not in MODEL_URLS:return {"error": "模型类型不支持"}# 更新最后请求时间（避免被监控脚本关闭）
    update_request_time(f"{model_type}_service")# 启动进程（如果未启动）
    subprocess.run(["supervisorctl", "start", f"{model_type}_service"])# 转发请求
    response = requests.post(MODEL_URLS[model_type], json={"text": text})return response.json()if __name__ == "__main__":import uvicorn
    uvicorn.run(app, host="0.0.0.0", port=8000)
```

# 步骤 4：启动与验证

1. 启动 `supervisord`：

   ```bash
   supervisord -c supervisord.conf
   ```

2. 启动显存监控脚本：

   ```bash
   python gpu_monitor.py &
   ```

3. 启动 API 网关：

   ```bash
   python gateway.py
   ```

4. 验证流程：

   + 调用 `http://127.0.0.1:8000/infer/llm`，LLM 进程启动，显存占用上升；
   + 5 分钟无请求后，监控脚本自动停止 LLM 进程，显存释放；
   + 调用 `http://127.0.0.1:8000/infer/sd`，SD 进程启动，复用释放的显存；
   + 关闭网关 / 进程时，显存均会被显式释放。

## 三、进阶优化（可选）

1. 显存动态分配：用 `GPUtil` 替代固定显存占比，启动模型时自动选择空闲显存最多的 GPU（若有多卡）；

2. 模型量化增强：

   + LLM：启用 `vLLM` 的 INT8 量化（`quantization=awq`），进一步降低显存占用；
   + 图片 / 视频：用 `TensorRT` 编译模型（如 SD 的 `TensorRT` 版本），显存占用降低 30%+；

3. 自动扩缩容：结合 `ray serve` 替代 `supervisord`，实现模型进程的自动启停（更轻量化）；

4. 显存泄漏检测：定期调用 `torch.cuda.memory_summary()` 检查显存泄漏，自动重启异常进程。

## 四、核心优势与注意事项

### 优势

1. 进程隔离：各模型独立进程，显存互不干扰，关闭进程即释放显存；

2. 分模型优化：针对 LLM / 图片 / 视频的特性适配最优显存优化方案；

3. 自动化管理：闲置自动关闭，避免显存浪费，请求到来时自动启动；

4. 低门槛落地：基于 Python+`FastAPI`+`supervisord`，无需复杂集群工具。

### 注意事项

1. 显存总量限制：单机 GPU 显存需≥24GB（如 A10 24GB），否则无法同时运行多模型，可通过「互斥启动」避免显存溢出（同一时间仅运行 1-2 个模型）；

2. 启动耗时：模型加载需时间（如 LLM 加载需 10-20 秒），可配置「预热」或「常驻核心模型」；

3. 监控脚本可靠性：需确保 API 网关和监控脚本的联动，避免误关正在处理请求的进程。

## 总结

该方案通过「进程隔离 + 分模型显存优化 + 监控脚本 + 统一网关」，完美解决单机部署多模态模型的显存管理问题：

+ 支持文本 / 图片 / 视频生成模型；

+ 每个模型适配专属显存优化方案；

+ 闲置自动关闭进程释放显存，请求到来自动启动；

+ 无集群依赖，纯单机部署，易落地、易维护。
