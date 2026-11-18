
# InternVL-U 昇腾 NPU 部署指南

## 创建 API 服务器

创建 `/root/api_server.py`，将模型封装为 REST API：

```
#!/usr/bin/env python3
"""InternVL-U FastAPI server for Ascend NPU."""

os.environ["PYTORCH_NPU_ALLOC_CONF"] = "expandable_segments:True"

sys.path.insert(0, "/root")

flash_attn_sdpa_patch.apply_patch()

from internvlu.pipeline_internvlu import InternVLUPipeline
from PIL import Image
from fastapi import FastAPI, Request
from fastapi.responses import JSONResponse


app = FastAPI(title="InternVL-U Ascend API")
pipe = None

def get_pipe():
    global pipe
    if pipe is None:
        pipe = InternVLUPipeline.from_pretrained(
            "/root/models/OpenGVLab/InternVL-U", torch_dtype=torch.bfloat16
        )
        pipe = pipe.to("npu:0")
    return pipe

@app.on_event("startup")
async def startup():
    get_pipe()

@app.get("/health")
def health():
    return {"status": "ok", "device": "ascend-npu"}

@app.post("/v1/chat")
async def chat(request: Request):
    body = await request.json()
    messages = body.get("messages", [])
    last = messages[-1]
    p = get_pipe()
    image = None
    if last.get("image"):
        image = Image.open(io.BytesIO(base64.b64decode(last["image"]))).convert("RGB")

    result = p(
        prompt=last["content"], image=image, generation_mode="text",
        max_new_tokens=body.get("max_new_tokens", 2048),
        temperature=body.get("temperature", 0.7)
    )
    text = p.tokenizer.decode(result.generate_output[0], skip_special_tokens=True)
    return {"ok": True, "text": text}

@app.post("/v1/generate")
async def generate(request: Request):
    body = await request.json()
    p = get_pipe()
    result = p(
        prompt=body["prompt"], generation_mode="image",
        height=min(body.get("height", 512), 1024),
        width=min(body.get("width", 512), 1024),
        num_inference_steps=20, all_cfg_scale=4.5
    )
    buf = io.BytesIO()
    result.images[0].save(buf, format="PNG")
    return {"ok": True, "image": base64.b64encode(buf.getvalue()).decode()}

@app.post("/v1/edit")
async def edit(request: Request):
    body = await request.json()
    p = get_pipe()
    source = Image.open(io.BytesIO(base64.b64decode(body["image"]))).convert("RGB")
    result = p(
        prompt=body["prompt"], image=source, generation_mode="image",
        height=512, width=512, num_inference_steps=20, all_cfg_scale=4.5
    )
    buf = io.BytesIO()
    result.images[0].save(buf, format="PNG")
    return {"ok": True, "image": base64.b64encode(buf.getvalue()).decode()}

if __name__ == "__main__":

```

## 启动服务

```
export LD_LIBRARY_PATH=/usr/local/Ascend/driver/lib64:$LD_LIBRARY_PATH
source /usr/local/Ascend/ascend-toolkit/set_env.sh
nohup python3 /root/api_server.py > /root/api_server.log 2>&1 &
```

## 验证

```
curl http://localhost:8000/health
# {"status":"ok","device":"ascend-npu"}

curl -X POST http://localhost:8000/v1/chat \
  -H 'Content-Type: application/json' \
  -d '{"messages":[{"role":"user","content":"hello"}]}'
# {"ok":true,"text":"Hello! How can I assist you today?..."}
```

## Step 7：配置公网访问（可选）

InternStudio 开发机没有公网 IP，需要通过 SSH 隧道暴露服务。

### 方案：通过公网服务器 SSH 隧道

假设你有一台公网服务器 `YOUR_SERVER_IP`：

```
# 在公网服务器上执行，将 8101 端口转发到 Ascend 的 8000 端口
ssh -N -L 0.0.0.0:8101:localhost:8000 \
  -p <ASCEND_SSH_PORT> root@<ASCEND_SSH_HOST> \
  -o ServerAliveInterval=30 \
  -o ServerAliveCountMax=3 \
  -o ExitOnForwardFailure=yes
```

验证公网可达：

```
curl http://YOUR_SERVER_IP:8101/health
# {"status":"ok","device":"ascend-npu"}
```

## 性能参考

以下数据基于 Ascend NPU（64 GB HBM）实测：

### 与 A100 对比

## 常见问题

### Q: 启动时报 `FlashAttention2 is not installed`

这是正常现象。Patch 会在此警告之后生效，不影响功能。

### Q: 首次推理很慢

昇腾 NPU 首次推理需要编译算子 kernel（类似 CUDA JIT），后续调用会显著加快。

### Q: `bitsandbytes` 警告 `compiled without GPU support`

不影响 `bfloat16` 推理。忽略即可。

### Q: `ModuleNotFoundError: No module named 'internvlu'`

说明 `InternVL-U` 包没有正确安装。请确认：

1. 已 `git clone` 了 `InternVL-U` 仓库

2. 在仓库目录下执行了 `pip install -e .`

3. 或者将仓库路径加入 `PYTHONPATH`：`export PYTHONPATH=/root/InternVL-U:$PYTHONPATH`

### Q: `pip install -e .` 报错 `flash_attn` 安装失败

`flash_attn` 只支持 CUDA，在昇腾上无法编译。解决方法：

```
# 方式一：跳过依赖安装
pip install -e . --no-deps

# 方式二：先注释掉 `setup.py`/`pyproject.toml` 中的 `flash_attn` 依赖再安装
```

我们的 SDPA Patch 会完全替代 `flash_attn` 的功能，不需要安装它。

### Q: `apply_patch()` 报错 `No module named 'internvlu.diffusion.internvlu_transformer'`

`InternVL-U` 的包结构可能更新了。请检查实际的模块路径：

```
# 查找实际的 transformer 模块
find /root/InternVL-U -name "*transformer*" -path "*/diffusion/*"
```

然后修改 `flash_attn_sdpa_patch.py` 中第 180 行的 `import` 路径。

### Q: 模型下载很慢或失败

```
# 使用 HF Mirror 加速
export HF_ENDPOINT=https://hf-mirror.com

# 或用 git lfs 直接克隆
git lfs install
git clone https://hf-mirror.com/OpenGVLab/InternVL-U /root/models/OpenGVLab/InternVL-U
```

### Q: 生图时报 OOM（显存不足）

尝试减小图片尺寸或减少推理步数：

```
result = pipe(prompt="...", generation_mode="image",
    height=256, width=256,  # 先用小尺寸测试

```

### Q: 能部署更大的模型吗？

`InternVL-U 4B` 仅占用 8.6 GB 显存，NPU 有 64 GB HBM，剩余 55+ GB 足够部署更大模型或多实例并发。

## 总结

通过 SDPA Fallback Patch，我们实现了 `InternVL-U` 在昇腾 NPU 上的全能力运行。这个方案的核心价值在于：

1. 零修改模型代码：Monkey Patch 方式，不需要改动 `InternVL-U` 源码

2. 通用性强：相同方案可推广到任何依赖 `flash_attn` 的模型

3. 功能完整：对话、文生图、图片编辑三大能力全部可用

4. 性能可接受：比 A100 慢约 50%，但远优于 CPU 推理

| 项目 | 要求 |

| --- | --- |

| 硬件 | 华为昇腾 Atlas 800T A2（64 GB HBM），推荐 InternStudio Ascend 开发机 |

| 驱动 | CANN 25.x + `torch_npu` 2.6.0+ |

| Python | 3.10+ |

| 磁盘 | ≥ 20 GB（模型 ~8.2 GB + 依赖） |

| `flash_attn` 函数 | SDPA 替代策略 |

| --- | --- |

| `flash_attn_func` | 转置 `QKV` 维度后调用 `F.scaled_dot_product_attention` |

| `flash_attn_varlen_func` | 将不等长序列 padding → SDPA 计算 → 重新 unpack |

| `pad_input` / `unpad_input` | 纯 `PyTorch` 实现的 padding 工具函数 |

| `is_flash_attn_2_available` | Patch 为始终返回 `True`，绕过可用性检查 |

| 能力 | 参数 | 耗时 | 备注 |

| --- | --- | --- | --- |

| 文本对话 | 短文本 ~64 tokens | 3~5 秒 | 首次推理含 warmup |

| 文生图 | 512x512, 20 steps | ~5.5 秒 | 热启动后稳定 |

| 图片编辑 | 512x512, 20 steps | ~5.5 秒 | 与生图速度一致 |

| 图像理解 | 1024x1024 输入 | ~11 秒 | 包含图片编码 |

| 指标 | A100 (CUDA) | Ascend NPU (SDPA) |

| --- | --- | --- |

| 文生图 512x512 | ~3.5 秒 | ~5.5 秒 |

| 显存占用 | 8.6 GB | 8.6 GB |

| Attention 实现 | `FlashAttention2` 原生 | SDPA Fallback |
