
```python
#!/usr/bin/env python3
"""InternVL-U FastAPI server for A100 GPU."""

from internvlu.pipeline_internvlu import InternVLUPipeline
from PIL import Image
from fastapi import FastAPI, Request
from fastapi.responses import JSONResponse


app = FastAPI(title="InternVL-U API")
pipe = None
API_KEY = os.environ.get("API_KEY", "")

def get_pipe():
    global pipe
    if pipe is None:
        print("[INIT] Loading InternVL-U pipeline on GPU...")
        t0 = time.time()
        pipe = InternVLUPipeline.from_pretrained(
            '/root/models/OpenGVLab/InternVL-U', torch_dtype=torch.bfloat16
        )
        pipe = pipe.to("cuda:0")
        print(f"[INIT] Pipeline ready in {time.time()-t0:.1f}s, "
              f"GPU mem: {torch.cuda.memory_allocated(0)/1e9:.1f} GB")
    return pipe

@app.on_event("startup")
async def startup():
    get_pipe()

@app.middleware("http")
async def check_api_key(request: Request, call_next):
    if API_KEY and request.url.path != "/health":
        key = request.headers.get("X-API-Key", "")
        if key != API_KEY:
            return JSONResponse({"error": "Unauthorized"}, status_code=401)
    return await call_next(request)

@app.get("/health")
def health():
    return {"status": "ok", "device": "a100-gpu", "model": "InternVL-U-4B"}

@app.post("/v1/chat")
async def chat(request: Request):
    body = await request.json()
    messages = body.get("messages", [])
    if not messages:
        return JSONResponse({"error": "No messages"}, status_code=400)

    p = get_pipe()
    last = messages[-1]
    prompt = last.get("content", "")
    image = None
    if last.get("image"):
        image = Image.open(io.BytesIO(base64.b64decode(last["image"]))).convert("RGB")

    t0 = time.time()
    try:
        result = p(
            prompt=prompt, image=image, generation_mode="text",
            max_new_tokens=body.get("max_new_tokens", 2048),
            temperature=body.get("temperature", 0.7)
        )
        if hasattr(result, 'generate_output') and result.generate_output is not None:
            text = p.tokenizer.decode(result.generate_output[0], skip_special_tokens=True)
        elif isinstance(result, str):
            text = result
        else:
            text = str(result)
        print(f"[CHAT] {time.time()-t0:.1f}s, prompt={prompt[:50]}")
        return {"ok": True, "text": text}
    except Exception as e:
        traceback.print_exc()
        return JSONResponse({"error": str(e)}, status_code=500)

@app.post("/v1/generate")
async def generate(request: Request):
    body = await request.json()
    prompt = body.get("prompt", "")
    if not prompt:
        return JSONResponse({"error": "No prompt"}, status_code=400)

    p = get_pipe()
    width = min(body.get("width", 512), 1024)
    height = min(body.get("height", 512), 1024)

    t0 = time.time()
    try:
        result = p(
            prompt=prompt, generation_mode="image",
            height=height, width=width,
            num_inference_steps=30, all_cfg_scale=4.5
        )
        if hasattr(result, 'images') and result.images:
            buf = io.BytesIO()
            result.images[0].save(buf, format="PNG")
            img_b64 = base64.b64encode(buf.getvalue()).decode()
            print(f"[GEN] {time.time()-t0:.1f}s, {width}x{height}, prompt={prompt[:50]}")
            return {"ok": True, "image": img_b64}
        return JSONResponse({"error": "No image generated"}, status_code=500)
    except Exception as e:
        traceback.print_exc()
        return JSONResponse({"error": str(e)}, status_code=500)

@app.post("/v1/edit")
async def edit(request: Request):
    body = await request.json()
    prompt = body.get("prompt", "")
    image_b64 = body.get("image", "")
    if not prompt or not image_b64:
        return JSONResponse({"error": "prompt and image required"}, status_code=400)

    p = get_pipe()
    source = Image.open(io.BytesIO(base64.b64decode(image_b64))).convert("RGB")

    t0 = time.time()
    try:
        result = p(
            prompt=prompt, image=source, generation_mode="image",
            height=512, width=512,
            num_inference_steps=30, all_cfg_scale=4.5
        )
        if hasattr(result, 'images') and result.images:
            buf = io.BytesIO()
            result.images[0].save(buf, format="PNG")
            img_b64_out = base64.b64encode(buf.getvalue()).decode()
            print(f"[EDIT] {time.time()-t0:.1f}s, prompt={prompt[:50]}")
            return {"ok": True, "image": img_b64_out}
        return JSONResponse({"error": "No image generated"}, status_code=500)
    except Exception as e:
        traceback.print_exc()
        return JSONResponse({"error": str(e)}, status_code=500)

if __name__ == "__main__":
```

启动服务：

`nohup python3 /root/api_server.py > /root/api_server.log 2>&1 &`

验证：

`curl http://localhost:8000/health`
# {"status":"ok","device":"a100-gpu","model":"InternVL-U-4B"}

`curl -X POST http://localhost:8000/v1/chat \
  -H 'Content-Type: application/json' \
  -d '{"messages":[{"role":"user","content":"hello"}]}'`
# {"ok":true,"text":"Hello! How can I assist you today?..."}

## Step 6：配置公网访问（可选）

InternStudio 开发机没有公网 IP，需要通过 SSH 隧道暴露服务。

假设你有一台公网服务器 `YOUR_SERVER_IP`：

```
# 在公网服务器上执行，将 8100 端口转发到 A100 的 8000 端口
sshpass -p '<A100_PASSWORD>' \
  ssh -N -L 0.0.0.0:8100:localhost:8000 \
  -p <A100_SSH_PORT> root@<A100_SSH_HOST> \
  -o StrictHostKeyChecking=no \
  -o ServerAliveInterval=30 \
  -o ServerAliveCountMax=3 \
  -o ExitOnForwardFailure=yes
```

验证公网可达：

`curl http://YOUR_SERVER_IP:8100/health`
# {"status":"ok","device":"a100-gpu","model":"InternVL-U-4B"}

> 建议用 `nohup` 或 `tmux` 保持隧道常驻。

## 性能参考

以下数据基于 A100 80GB 实测：

### 与昇腾 NPU 对比

> A100 使用原生 `FlashAttention2`，性能更优。昇腾 NPU 部署教程请参考 在昇腾 NPU 上部署 InternVL-U。

## 常见问题

### Q: flash-attn 安装失败

`flash-attn` 需要 CUDA 编译，安装时间较长（约 10 分钟）。如果失败：

```
# 确保 CUDA toolkit 已安装
nvcc --version

# 指定 CUDA 架构编译
TORCH_CUDA_ARCH_LIST="8.0" pip install flash-attn --no-build-isolation
```

如果仍然失败，可以使用 SDPA Fallback Patch 方案，参考 昇腾 NPU 教程 中的 Step 3。

### Q: CUDA Out of Memory

InternVL-U 4B 仅需 8.6 GB 显存，A100 80GB 有充足余量。如果出现 OOM：

| 项目 | 要求 |
| --- | --- |
| 硬件 | NVIDIA A100（80 GB），推荐 InternStudio GPU 开发机 |
| 驱动 | CUDA 12.x + cuDNN 8.x |
| Python | 3.10+ |
| 磁盘 | >= 20 GB（模型 ~8.2 GB + 依赖） |

| 能力 | 参数 | 耗时 | 备注 |
| --- | --- | --- | --- |
| 文本对话 | 短文本 ~64 tokens | ~1.6 秒 | FlashAttention2 加速 |
| 文生图 | 512x512, 30 steps | ~3.5 秒 | 热启动后稳定 |
| 图片编辑 | 512x512, 30 steps | ~3.5 秒 | 与生图速度一致 |
| 图像理解 | 1024x1024 输入 | ~3 秒 | 包含图片编码 |

| 指标 | A100 (CUDA) | 昇腾 NPU (SDPA) |
| --- | --- | --- |
| 文生图 512x512 | ~3.5 秒 | ~5.5 秒 |
| 文本对话 | ~1.6 秒 | ~3-5 秒 |
| 显存占用 | 8.6 GB | 8.6 GB |
| Attention 实现 | FlashAttention2 原生 | SDPA Fallback |
