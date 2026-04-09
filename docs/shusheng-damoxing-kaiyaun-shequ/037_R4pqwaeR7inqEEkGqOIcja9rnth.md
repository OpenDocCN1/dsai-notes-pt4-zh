# A100 部署指南

> 来源：[https://aicarrier.feishu.cn/docx/KV8bdj196oTKPZxJrzEcLiEqnbc](https://aicarrier.feishu.cn/docx/KV8bdj196oTKPZxJrzEcLiEqnbc)

本教程基于 InternStudio 开发机实战，手把手带你在 NVIDIA A100 GPU 上完成 InternVL-U 4B 模型的全能力部署，包括文本对话、文生图、图片编辑三大功能。

## 前置条件

## Step 1：环境准备

### 1.1 验证 GPU 可用

```
nvidia-smi
# 确认看到 A100 设备

python3 -c &quot;import torch; print(torch.cuda.is_available(), torch.cuda.get_device_name(0))&quot;

```

### 1.2 安装依赖

```
pip install fastapi uvicorn pillow modelscope transformers accelerate flash-attn
```

> flash-attn 是 InternVL-U 图像生成的加速组件，A100 上可直接安装。如果安装失败，可参考 昇腾 NPU 部署教程 中的 SDPA Fallback Patch 方案作为替代。

## Step 2：下载模型

HuggingFace 在国内网络不可用，使用 ModelScope 镜像下载：

```
python3 -c &quot;
from modelscope import snapshot_download
snapshot_download(&#x27;OpenGVLab/InternVL-U&#x27;, local_dir=&#x27;/root/models/OpenGVLab/InternVL-U&#x27;)
&quot;
```

模型约 8.2 GB，下载完成后目录结构：

```
/root/models/OpenGVLab/InternVL-U/
├── config.json
├── model-*.safetensors
├── tokenizer.json
└── ...
```

> 如果可以访问 HuggingFace，也可以使用：

> `bash

> pip install huggingface_hub

> huggingface-cli download OpenGVLab/InternVL-U --local-dir /root/models/OpenGVLab/InternVL-U

> `

## Step 3：验证模型加载

```

from internvlu.pipeline_internvlu import InternVLUPipeline

pipe = InternVLUPipeline.from_pretrained(
    &#x27;/root/models/OpenGVLab/InternVL-U&#x27;,
    torch_dtype=torch.bfloat16
)
pipe = pipe.to(&quot;cuda:0&quot;)

print(f&quot;GPU 显存占用: {torch.cuda.memory_allocated(0)/1e9:.1f} GB&quot;)

```

> 注意：需要将 InternVL-U 的 internvlu 包放在 Python Path 中。如果你用 git clone 下载了完整仓库，可以 cd InternVL-U-2 或者 sys.path.insert(0, '/root/InternVL-U-2')。

## Step 4：测试三大能力

### 4.1 文本对话

```
result = pipe(prompt=&quot;你好，用一句话介绍你自己&quot;, generation_mode=&quot;text&quot;)
text = pipe.tokenizer.decode(result.generate_output[0], skip_special_tokens=True)
print(text)

```

> 注意：pipeline 返回的是 token IDs tensor，需要用 pipe.tokenizer.decode() 解码为文本。

### 4.2 文生图

```
from PIL import Image

result = pipe(
    prompt=&quot;一只穿着宇航服的猫咪在太空中漫步，背景是星云&quot;,
    generation_mode=&quot;image&quot;,
    height=512, width=512,
    num_inference_steps=30,
    all_cfg_scale=4.5
)
result.images[0].save(&quot;cat_space.png&quot;)

```

### 4.3 图片编辑

```
source = Image.open(&quot;input.png&quot;).convert(&quot;RGB&quot;)

result = pipe(
    prompt=&quot;把背景换成海滩&quot;,
    image=source,
    generation_mode=&quot;image&quot;,
    height=512, width=512,
    num_inference_steps=30,
    all_cfg_scale=4.5
)

```

## Step 5：搭建 FastAPI 服务

创建 /root/api_server.py，将模型封装为 REST API：

```
#!/usr/bin/env python3
&quot;&quot;&quot;InternVL-U FastAPI server for A100 GPU.&quot;&quot;&quot;



from internvlu.pipeline_internvlu import InternVLUPipeline
from PIL import Image
from fastapi import FastAPI, Request
from fastapi.responses import JSONResponse


app = FastAPI(title=&quot;InternVL-U API&quot;)
pipe = None
API_KEY = os.environ.get(&quot;API_KEY&quot;, &quot;&quot;)

def get_pipe():
    global pipe
    if pipe is None:
        print(&quot;[INIT] Loading InternVL-U pipeline on GPU...&quot;)
        t0 = time.time()
        pipe = InternVLUPipeline.from_pretrained(
            &#x27;/root/models/OpenGVLab/InternVL-U&#x27;, torch_dtype=torch.bfloat16
        )
        pipe = pipe.to(&quot;cuda:0&quot;)
        print(f&quot;[INIT] Pipeline ready in {time.time()-t0:.1f}s, &quot;
              f&quot;GPU mem: {torch.cuda.memory_allocated(0)/1e9:.1f} GB&quot;)
    return pipe

@app.on_event(&quot;startup&quot;)
async def startup():
    get_pipe()

@app.middleware(&quot;http&quot;)
async def check_api_key(request: Request, call_next):
    if API_KEY and request.url.path != &quot;/health&quot;:
        key = request.headers.get(&quot;X-API-Key&quot;, &quot;&quot;)
        if key != API_KEY:
            return JSONResponse({&quot;error&quot;: &quot;Unauthorized&quot;}, status_code=401)
    return await call_next(request)

@app.get(&quot;/health&quot;)
def health():
    return {&quot;status&quot;: &quot;ok&quot;, &quot;device&quot;: &quot;a100-gpu&quot;, &quot;model&quot;: &quot;InternVL-U-4B&quot;}

@app.post(&quot;/v1/chat&quot;)
async def chat(request: Request):
    body = await request.json()
    messages = body.get(&quot;messages&quot;, [])
    if not messages:
        return JSONResponse({&quot;error&quot;: &quot;No messages&quot;}, status_code=400)

    p = get_pipe()
    last = messages[-1]
    prompt = last.get(&quot;content&quot;, &quot;&quot;)
    image = None
    if last.get(&quot;image&quot;):
        image = Image.open(io.BytesIO(base64.b64decode(last[&quot;image&quot;]))).convert(&quot;RGB&quot;)

    t0 = time.time()
    try:
        result = p(
            prompt=prompt, image=image, generation_mode=&quot;text&quot;,
            max_new_tokens=body.get(&quot;max_new_tokens&quot;, 2048),
            temperature=body.get(&quot;temperature&quot;, 0.7)
        )
        if hasattr(result, &#x27;generate_output&#x27;) and result.generate_output is not None:
            text = p.tokenizer.decode(result.generate_output[0], skip_special_tokens=True)
        elif isinstance(result, str):
            text = result
        else:
            text = str(result)
        print(f&quot;[CHAT] {time.time()-t0:.1f}s, prompt={prompt[:50]}&quot;)
        return {&quot;ok&quot;: True, &quot;text&quot;: text}
    except Exception as e:
        traceback.print_exc()
        return JSONResponse({&quot;error&quot;: str(e)}, status_code=500)

@app.post(&quot;/v1/generate&quot;)
async def generate(request: Request):
    body = await request.json()
    prompt = body.get(&quot;prompt&quot;, &quot;&quot;)
    if not prompt:
        return JSONResponse({&quot;error&quot;: &quot;No prompt&quot;}, status_code=400)

    p = get_pipe()
    width = min(body.get(&quot;width&quot;, 512), 1024)
    height = min(body.get(&quot;height&quot;, 512), 1024)

    t0 = time.time()
    try:
        result = p(
            prompt=prompt, generation_mode=&quot;image&quot;,
            height=height, width=width,
            num_inference_steps=30, all_cfg_scale=4.5
        )
        if hasattr(result, &#x27;images&#x27;) and result.images:
            buf = io.BytesIO()
            result.images[0].save(buf, format=&quot;PNG&quot;)
            img_b64 = base64.b64encode(buf.getvalue()).decode()
            print(f&quot;[GEN] {time.time()-t0:.1f}s, {width}x{height}, prompt={prompt[:50]}&quot;)
            return {&quot;ok&quot;: True, &quot;image&quot;: img_b64}
        return JSONResponse({&quot;error&quot;: &quot;No image generated&quot;}, status_code=500)
    except Exception as e:
        traceback.print_exc()
        return JSONResponse({&quot;error&quot;: str(e)}, status_code=500)

@app.post(&quot;/v1/edit&quot;)
async def edit(request: Request):
    body = await request.json()
    prompt = body.get(&quot;prompt&quot;, &quot;&quot;)
    image_b64 = body.get(&quot;image&quot;, &quot;&quot;)
    if not prompt or not image_b64:
        return JSONResponse({&quot;error&quot;: &quot;prompt and image required&quot;}, status_code=400)

    p = get_pipe()
    source = Image.open(io.BytesIO(base64.b64decode(image_b64))).convert(&quot;RGB&quot;)

    t0 = time.time()
    try:
        result = p(
            prompt=prompt, image=source, generation_mode=&quot;image&quot;,
            height=512, width=512,
            num_inference_steps=30, all_cfg_scale=4.5
        )
        if hasattr(result, &#x27;images&#x27;) and result.images:
            buf = io.BytesIO()
            result.images[0].save(buf, format=&quot;PNG&quot;)
            img_b64_out = base64.b64encode(buf.getvalue()).decode()
            print(f&quot;[EDIT] {time.time()-t0:.1f}s, prompt={prompt[:50]}&quot;)
            return {&quot;ok&quot;: True, &quot;image&quot;: img_b64_out}
        return JSONResponse({&quot;error&quot;: &quot;No image generated&quot;}, status_code=500)
    except Exception as e:
        traceback.print_exc()
        return JSONResponse({&quot;error&quot;: str(e)}, status_code=500)

if __name__ == &quot;__main__&quot;:

```

启动服务：

```
nohup python3 /root/api_server.py &gt; /root/api_server.log 2&gt;&amp;1 &amp;
```

验证：

```
curl http://localhost:8000/health
# {&quot;status&quot;:&quot;ok&quot;,&quot;device&quot;:&quot;a100-gpu&quot;,&quot;model&quot;:&quot;InternVL-U-4B&quot;}

curl -X POST http://localhost:8000/v1/chat \
  -H &#x27;Content-Type: application/json&#x27; \
  -d &#x27;{&quot;messages&quot;:[{&quot;role&quot;:&quot;user&quot;,&quot;content&quot;:&quot;hello&quot;}]}&#x27;
# {&quot;ok&quot;:true,&quot;text&quot;:&quot;Hello! How can I assist you today?...&quot;}
```

## Step 6：配置公网访问（可选）

InternStudio 开发机没有公网 IP，需要通过 SSH 隧道暴露服务。

假设你有一台公网服务器 YOUR_SERVER_IP：

```
# 在公网服务器上执行，将 8100 端口转发到 A100 的 8000 端口
sshpass -p &#x27;&lt;A100_PASSWORD&gt;&#x27; \
  ssh -N -L 0.0.0.0:8100:localhost:8000 \
  -p &lt;A100_SSH_PORT&gt; root@&lt;A100_SSH_HOST&gt; \
  -o StrictHostKeyChecking=no \
  -o ServerAliveInterval=30 \
  -o ServerAliveCountMax=3 \
  -o ExitOnForwardFailure=yes
```

验证公网可达：

```
curl http://YOUR_SERVER_IP:8100/health
# {&quot;status&quot;:&quot;ok&quot;,&quot;device&quot;:&quot;a100-gpu&quot;,&quot;model&quot;:&quot;InternVL-U-4B&quot;}
```

> 建议用 nohup 或 tmux 保持隧道常驻。

## 性能参考

以下数据基于 A100 80GB 实测：

### 与昇腾 NPU 对比

> A100 使用原生 FlashAttention2，性能更优。昇腾 NPU 部署教程请参考 在昇腾 NPU 上部署 InternVL-U。

## 常见问题

### Q: flash-attn 安装失败

flash-attn 需要 CUDA 编译，安装时间较长（约 10 分钟）。如果失败：

```
# 确保 CUDA toolkit 已安装
nvcc --version

# 指定 CUDA 架构编译
TORCH_CUDA_ARCH_LIST=&quot;8.0&quot; pip install flash-attn --no-build-isolation
```

如果仍然失败，可以使用 SDPA Fallback Patch 方案，参考 昇腾 NPU 教程 中的 Step 3。

### Q: CUDA Out of Memory

InternVL-U 4B 仅需 8.6 GB 显存，A100 80GB 有充足余量。如果出现 OOM：

| **项目** | **要求** |
| --- | --- |
|  |  |
| 硬件 | NVIDIA A100（80 GB），推荐 InternStudio GPU 开发机 |
| 驱动 | CUDA 12.x + cuDNN 8.x |
| Python | 3.10+ |
| 磁盘 | &gt;= 20 GB（模型 ~8.2 GB + 依赖） |

|  |  |
| --- | --- |
|  |  |

| **能力** | **参数** | **耗时** | **备注** |
| --- | --- | --- | --- |
|  |  |  |  |
| 文本对话 | 短文本 ~64 tokens | ~1.6 秒 | FlashAttention2 加速 |
| 文生图 | 512x512, 30 steps | ~3.5 秒 | 热启动后稳定 |
| 图片编辑 | 512x512, 30 steps | ~3.5 秒 | 与生图速度一致 |
| 图像理解 | 1024x1024 输入 | ~3 秒 | 包含图片编码 |

| **指标** | **A100 (CUDA)** | **昇腾 NPU (SDPA)** |
| --- | --- | --- |
|  |  |  |
| 文生图 512x512 | ~3.5 秒 | ~5.5 秒 |
| 文本对话 | ~1.6 秒 | ~3-5 秒 |
| 显存占用 | 8.6 GB | 8.6 GB |
| Attention 实现 | FlashAttention2 原生 | SDPA Fallback |