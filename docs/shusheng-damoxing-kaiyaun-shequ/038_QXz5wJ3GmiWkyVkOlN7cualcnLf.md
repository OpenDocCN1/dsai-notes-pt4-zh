# 华为昇腾 Atlas 800T A2 部署指南

> 来源：[https://aicarrier.feishu.cn/docx/AQ9gdw9XnoMVtYxyA9CcD3oznjo](https://aicarrier.feishu.cn/docx/AQ9gdw9XnoMVtYxyA9CcD3oznjo)

本教程基于 InternStudio 开发机实战，手把手带你在华为昇腾 NPU 上完成 InternVL-U 4B 模型的全能力部署，包括文本对话、文生图、图片编辑三大功能。

## 前置条件

## Step 1：环境准备

### 1.1 初始化 Ascend 驱动环境

每次开启终端时需执行：

```
export LD_LIBRARY_PATH=/usr/local/Ascend/driver/lib64:\
/usr/local/Ascend/driver/lib64/common:\
/usr/local/Ascend/driver/lib64/driver:\
$LD_LIBRARY_PATH

source /usr/local/Ascend/ascend-toolkit/set_env.sh
```

> 建议将以上内容写入 ~/.bashrc，避免每次手动执行。

### 1.2 验证 NPU 可用

```
python3 -c &quot;import torch, torch_npu; print(torch_npu.npu.is_available())&quot;
# 期望输出: True
```

### 1.3 安装 InternVL-U 及依赖

InternVL-U 需要从源码安装：

```
# 克隆 InternVL-U 仓库
cd /root
git clone https://github.com/OpenGVLab/InternVL-U.git
cd InternVL-U

# 安装依赖（注意：不要安装 flash_attn，昇腾不支持）
pip install -e . --no-deps
pip install fastapi uvicorn pillow modelscope transformers accelerate
pip install diffusers sentencepiece tiktoken einops timm
```

> 重要：如果 pip install -e . 尝试安装 flash_attn 并报错，使用 --no-deps 跳过，然后手动安装其他依赖。

### 1.4 确认安装成功

```
python3 -c &quot;import internvlu; print(&#x27;internvlu 安装成功&#x27;)&quot;
```

如果报 ModuleNotFoundError，检查是否在 InternVL-U 目录下执行了 pip install -e .。

## Step 2：下载模型

HuggingFace 在国内网络不可用，使用 ModelScope 镜像下载：

```
# 方式一：ModelScope 下载
python3 -c &quot;
from modelscope import snapshot_download
snapshot_download(&#x27;OpenGVLab/InternVL-U&#x27;, local_dir=&#x27;/root/models/OpenGVLab/InternVL-U&#x27;)
&quot;

# 方式二：如果 ModelScope 也很慢，使用 HF Mirror
HF_ENDPOINT=https://hf-mirror.com python3 -c &quot;
from huggingface_hub import snapshot_download
snapshot_download(&#x27;OpenGVLab/InternVL-U&#x27;, local_dir=&#x27;/root/models/OpenGVLab/InternVL-U&#x27;)

```

模型约 8.2 GB，下载完成后目录结构：

```
/root/models/OpenGVLab/InternVL-U/
├── config.json
├── model-*.safetensors
├── tokenizer.json
└── ...
```

## Step 3：编写 SDPA Fallback Patch

核心问题：InternVL-U 的图像生成（Diffusion）路径硬编码依赖 flash_attn 库，该库仅支持 CUDA，无法在昇腾 NPU 上安装。

解决方案：编写 Monkey Patch，用 PyTorch 原生 F.scaled_dot_product_attention 替代 flash_attn 的所有函数。

创建 /root/flash_attn_sdpa_patch.py：

```
&quot;&quot;&quot;
SDPA fallback patch for InternVL-U on Ascend NPU (no flash_attn).
Monkey-patches flash_attn functions with F.scaled_dot_product_attention equivalents.
&quot;&quot;&quot;





def flash_attn_func(q, k, v, dropout_p=0.0, softmax_scale=None, causal=False, **kwargs):
    &quot;&quot;&quot;替代 flash_attn_func。
    Input/Output: (batch, seqlen, nheads, head_dim)
    &quot;&quot;&quot;
    batch, seqlen, nheads, head_dim = q.shape
    q = q.transpose(1, 2)  # -&gt; (batch, nheads, seqlen, head_dim)
    k = k.transpose(1, 2)
    v = v.transpose(1, 2)

    if softmax_scale is not None:
        q = q * (softmax_scale * math.sqrt(head_dim))

    out = F.scaled_dot_product_attention(q, k, v, dropout_p=0.0, is_causal=causal)
    return out.transpose(1, 2).contiguous()


def flash_attn_varlen_func(q, k, v, cu_seqlens_q, cu_seqlens_k,
                           max_seqlen_q, max_seqlen_k,
                           dropout_p=0.0, softmax_scale=None, causal=False, **kwargs):
    &quot;&quot;&quot;替代 flash_attn_varlen_func。
    Input/Output: (total_tokens, nheads, head_dim) packed variable-length
    &quot;&quot;&quot;
    nheads = q.shape[1]
    head_dim = q.shape[2]
    batch_size = cu_seqlens_q.shape[0] - 1

    # 将不等长序列 padding 到统一长度
    q_padded = q.new_zeros(batch_size, max_seqlen_q, nheads, head_dim)
    k_padded = k.new_zeros(batch_size, max_seqlen_k, nheads, head_dim)
    v_padded = v.new_zeros(batch_size, max_seqlen_k, nheads, head_dim)
    attn_mask = q.new_zeros(batch_size, max_seqlen_k, dtype=torch.bool)

    for i in range(batch_size):
        sq = (cu_seqlens_q[i + 1] - cu_seqlens_q[i]).item()
        sk = (cu_seqlens_k[i + 1] - cu_seqlens_k[i]).item()
        q_padded[i, :sq] = q[cu_seqlens_q[i]:cu_seqlens_q[i + 1]]
        k_padded[i, :sk] = k[cu_seqlens_k[i]:cu_seqlens_k[i + 1]]
        v_padded[i, :sk] = v[cu_seqlens_k[i]:cu_seqlens_k[i + 1]]
        attn_mask[i, sk:] = True

    q_padded = q_padded.transpose(1, 2)
    k_padded = k_padded.transpose(1, 2)
    v_padded = v_padded.transpose(1, 2)

    if softmax_scale is not None:
        q_padded = q_padded * (softmax_scale * math.sqrt(head_dim))

    expanded_mask = attn_mask.unsqueeze(1).unsqueeze(2)
    attn_bias = torch.zeros_like(expanded_mask, dtype=q_padded.dtype)
    attn_bias.masked_fill_(expanded_mask, float(&quot;-inf&quot;))

    out = F.scaled_dot_product_attention(
        q_padded, k_padded, v_padded,
        attn_mask=attn_bias if attn_mask.any() else None,
        dropout_p=0.0,
        is_causal=causal if not attn_mask.any() else False,
    )
    out = out.transpose(1, 2)

    # 重新 pack 为不等长格式
    output = q.new_zeros(q.shape[0], nheads, head_dim)
    for i in range(batch_size):
        sq = (cu_seqlens_q[i + 1] - cu_seqlens_q[i]).item()
        output[cu_seqlens_q[i]:cu_seqlens_q[i + 1]] = out[i, :sq]
    return output


def unpad_input(hidden_states, attention_mask):
    &quot;&quot;&quot;替代 flash_attn.bert_padding.unpad_input&quot;&quot;&quot;
    seqlens = attention_mask.sum(dim=-1, dtype=torch.int32)
    indices = torch.nonzero(attention_mask.flatten(), as_tuple=False).flatten()
    max_seqlen = seqlens.max().item()
    cu_seqlens = F.pad(torch.cumsum(seqlens, dim=0, dtype=torch.int32), (1, 0))
    flat = hidden_states.reshape(-1, *hidden_states.shape[2:])
    return flat[indices], indices, cu_seqlens, max_seqlen


def pad_input(hidden_states, indices, batch_size, seqlen):
    &quot;&quot;&quot;替代 flash_attn.bert_padding.pad_input&quot;&quot;&quot;
    dim = hidden_states.shape[1:]
    output = hidden_states.new_zeros(batch_size * seqlen, *dim)
    output[indices] = hidden_states
    return output.reshape(batch_size, seqlen, *dim)


def index_first_axis(input, indices):
    return input[indices]


def apply_patch():
    &quot;&quot;&quot;应用 SDPA Patch，必须在加载模型之前调用。&quot;&quot;&quot;
    import internvlu.diffusion.internvlu_transformer as mod

    mod.flash_attn_func = flash_attn_func
    mod.flash_attn_varlen_func = flash_attn_varlen_func
    mod.index_first_axis = index_first_axis
    mod.pad_input = pad_input
    mod.unpad_input = unpad_input
    mod._flash_supports_window_size = True

    import transformers.utils
    transformers.utils.is_flash_attn_2_available = lambda: True
    transformers.utils.is_flash_attn_greater_or_equal_2_10 = lambda: True
    mod.is_flash_attn_2_available = lambda: True
    mod.is_flash_attn_greater_or_equal_2_10 = lambda: True


```

### Patch 原理简述

## Step 4：验证模型加载

```

os.environ[&quot;PYTORCH_NPU_ALLOC_CONF&quot;] = &quot;expandable_segments:True&quot;


# 必须在 import InternVLU 之前应用 Patch
sys.path.insert(0, &#x27;/root&#x27;)

flash_attn_sdpa_patch.apply_patch()

from internvlu.pipeline_internvlu import InternVLUPipeline

pipe = InternVLUPipeline.from_pretrained(
    &#x27;/root/models/OpenGVLab/InternVL-U&#x27;,
    torch_dtype=torch.bfloat16
)
pipe = pipe.to(&quot;npu:0&quot;)

print(f&quot;NPU 显存占用: {torch_npu.npu.memory_allocated(0)/1e9:.1f} GB&quot;)

```

## Step 5：测试三大能力

### 5.1 文本对话

```
result = pipe(prompt=&quot;你好，用一句话介绍你自己&quot;, generation_mode=&quot;text&quot;)
text = pipe.tokenizer.decode(result.generate_output[0], skip_special_tokens=True)
print(text)

```

> 注意：pipeline 返回的是 token IDs tensor，需要用 pipe.tokenizer.decode() 解码为文本。

### 5.2 文生图

```
from PIL import Image

result = pipe(
    prompt=&quot;一只穿着宇航服的猫咪在太空中漫步，背景是星云&quot;,
    generation_mode=&quot;image&quot;,
    height=512, width=512,
    num_inference_steps=20,
    all_cfg_scale=4.5
)
result.images[0].save(&quot;cat_space.png&quot;)

```

### 5.3 图片编辑

```
source = Image.open(&quot;input.png&quot;).convert(&quot;RGB&quot;)

result = pipe(
    prompt=&quot;把背景换成海滩&quot;,
    image=source,
    generation_mode=&quot;image&quot;,
    height=512, width=512,
    num_inference_steps=20,
    all_cfg_scale=4.5
)

```

## Step 6：搭建 FastAPI 服务

创建 /root/api_server.py，将模型封装为 REST API：

```
#!/usr/bin/env python3
&quot;&quot;&quot;InternVL-U FastAPI server for Ascend NPU.&quot;&quot;&quot;

os.environ[&quot;PYTORCH_NPU_ALLOC_CONF&quot;] = &quot;expandable_segments:True&quot;



sys.path.insert(0, &#x27;/root&#x27;)

flash_attn_sdpa_patch.apply_patch()

from internvlu.pipeline_internvlu import InternVLUPipeline
from PIL import Image
from fastapi import FastAPI, Request
from fastapi.responses import JSONResponse


app = FastAPI(title=&quot;InternVL-U Ascend API&quot;)
pipe = None

def get_pipe():
    global pipe
    if pipe is None:
        pipe = InternVLUPipeline.from_pretrained(
            &#x27;/root/models/OpenGVLab/InternVL-U&#x27;, torch_dtype=torch.bfloat16
        )
        pipe = pipe.to(&quot;npu:0&quot;)
    return pipe

@app.on_event(&quot;startup&quot;)
async def startup():
    get_pipe()

@app.get(&quot;/health&quot;)
def health():
    return {&quot;status&quot;: &quot;ok&quot;, &quot;device&quot;: &quot;ascend-npu&quot;}

@app.post(&quot;/v1/chat&quot;)
async def chat(request: Request):
    body = await request.json()
    messages = body.get(&quot;messages&quot;, [])
    last = messages[-1]
    p = get_pipe()
    image = None
    if last.get(&quot;image&quot;):
        image = Image.open(io.BytesIO(base64.b64decode(last[&quot;image&quot;]))).convert(&quot;RGB&quot;)

    result = p(
        prompt=last[&quot;content&quot;], image=image, generation_mode=&quot;text&quot;,
        max_new_tokens=body.get(&quot;max_new_tokens&quot;, 2048),
        temperature=body.get(&quot;temperature&quot;, 0.7)
    )
    text = p.tokenizer.decode(result.generate_output[0], skip_special_tokens=True)
    return {&quot;ok&quot;: True, &quot;text&quot;: text}

@app.post(&quot;/v1/generate&quot;)
async def generate(request: Request):
    body = await request.json()
    p = get_pipe()
    result = p(
        prompt=body[&quot;prompt&quot;], generation_mode=&quot;image&quot;,
        height=min(body.get(&quot;height&quot;, 512), 1024),
        width=min(body.get(&quot;width&quot;, 512), 1024),
        num_inference_steps=20, all_cfg_scale=4.5
    )
    buf = io.BytesIO()
    result.images[0].save(buf, format=&quot;PNG&quot;)
    return {&quot;ok&quot;: True, &quot;image&quot;: base64.b64encode(buf.getvalue()).decode()}

@app.post(&quot;/v1/edit&quot;)
async def edit(request: Request):
    body = await request.json()
    p = get_pipe()
    source = Image.open(io.BytesIO(base64.b64decode(body[&quot;image&quot;]))).convert(&quot;RGB&quot;)
    result = p(
        prompt=body[&quot;prompt&quot;], image=source, generation_mode=&quot;image&quot;,
        height=512, width=512, num_inference_steps=20, all_cfg_scale=4.5
    )
    buf = io.BytesIO()
    result.images[0].save(buf, format=&quot;PNG&quot;)
    return {&quot;ok&quot;: True, &quot;image&quot;: base64.b64encode(buf.getvalue()).decode()}

if __name__ == &quot;__main__&quot;:

```

启动服务：

```
export LD_LIBRARY_PATH=/usr/local/Ascend/driver/lib64:$LD_LIBRARY_PATH
source /usr/local/Ascend/ascend-toolkit/set_env.sh
nohup python3 /root/api_server.py &gt; /root/api_server.log 2&gt;&amp;1 &amp;
```

验证：

```
curl http://localhost:8000/health
# {&quot;status&quot;:&quot;ok&quot;,&quot;device&quot;:&quot;ascend-npu&quot;}

curl -X POST http://localhost:8000/v1/chat \
  -H &#x27;Content-Type: application/json&#x27; \
  -d &#x27;{&quot;messages&quot;:[{&quot;role&quot;:&quot;user&quot;,&quot;content&quot;:&quot;hello&quot;}]}&#x27;
# {&quot;ok&quot;:true,&quot;text&quot;:&quot;Hello! How can I assist you today?...&quot;}
```

## Step 7：配置公网访问（可选）

InternStudio 开发机没有公网 IP，需要通过 SSH 隧道暴露服务。

### 方案：通过公网服务器 SSH 隧道

假设你有一台公网服务器 YOUR_SERVER_IP：

```
# 在公网服务器上执行，将 8101 端口转发到 Ascend 的 8000 端口
ssh -N -L 0.0.0.0:8101:localhost:8000 \
  -p &lt;ASCEND_SSH_PORT&gt; root@&lt;ASCEND_SSH_HOST&gt; \
  -o ServerAliveInterval=30 \
  -o ServerAliveCountMax=3 \
  -o ExitOnForwardFailure=yes
```

验证公网可达：

```
curl http://YOUR_SERVER_IP:8101/health
# {&quot;status&quot;:&quot;ok&quot;,&quot;device&quot;:&quot;ascend-npu&quot;}
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

不影响 bfloat16 推理。忽略即可。

### Q: `ModuleNotFoundError: No module named 'internvlu'`

说明 InternVL-U 包没有正确安装。请确认：

1. 已 git clone 了 InternVL-U 仓库

2. 在仓库目录下执行了 pip install -e .

3. 或者将仓库路径加入 PYTHONPATH：export PYTHONPATH=/root/InternVL-U:$PYTHONPATH

### Q: `pip install -e .` 报错 `flash_attn` 安装失败

flash_attn 只支持 CUDA，在昇腾上无法编译。解决方法：

```
# 方式一：跳过依赖安装
pip install -e . --no-deps

# 方式二：先注释掉 setup.py/pyproject.toml 中的 flash_attn 依赖再安装
```

我们的 SDPA Patch 会完全替代 flash_attn 的功能，不需要安装它。

### Q: `apply_patch()` 报错 `No module named 'internvlu.diffusion.internvlu_transformer'`

InternVL-U 的包结构可能更新了。请检查实际的模块路径：

```
# 查找实际的 transformer 模块
find /root/InternVL-U -name &quot;*transformer*&quot; -path &quot;*/diffusion/*&quot;
```

然后修改 flash_attn_sdpa_patch.py 中第 180 行的 import 路径。

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
result = pipe(prompt=&quot;...&quot;, generation_mode=&quot;image&quot;,
    height=256, width=256,  # 先用小尺寸测试

```

### Q: 能部署更大的模型吗？

InternVL-U 4B 仅占用 8.6 GB 显存，NPU 有 64 GB HBM，剩余 55+ GB 足够部署更大模型或多实例并发。

## 总结

通过 SDPA Fallback Patch，我们实现了 InternVL-U 在昇腾 NPU 上的全能力运行。这个方案的核心价值在于：

1. 零修改模型代码：Monkey Patch 方式，不需要改动 InternVL-U 源码

2. 通用性强：相同方案可推广到任何依赖 flash_attn 的模型

3. 功能完整：对话、文生图、图片编辑三大能力全部可用

4. 性能可接受：比 A100 慢约 50%，但远优于 CPU 推理

| **项目** | **要求** |
| --- | --- |
|  |  |
| 硬件 | 华为昇腾 Atlas 800T A2（64 GB HBM），推荐 InternStudio Ascend 开发机 |
| 驱动 | CANN 25.x + torch_npu 2.6.0+ |
| Python | 3.10+ |
| 磁盘 | ≥ 20 GB（模型 ~8.2 GB + 依赖） |

| **flash_attn 函数** | **SDPA 替代策略** |
| --- | --- |
|  |  |
| flash_attn_func | 转置 QKV 维度后调用 F.scaled_dot_product_attention |
| flash_attn_varlen_func | 将不等长序列 padding → SDPA 计算 → 重新 unpack |
| pad_input / unpad_input | 纯 PyTorch 实现的 padding 工具函数 |
| is_flash_attn_2_available | Patch 为始终返回 True，绕过可用性检查 |

| **能力** | **参数** | **耗时** | **备注** |
| --- | --- | --- | --- |
|  |  |  |  |
| 文本对话 | 短文本 ~64 tokens | 3~5 秒 | 首次推理含 warmup |
| 文生图 | 512x512, 20 steps | ~5.5 秒 | 热启动后稳定 |
| 图片编辑 | 512x512, 20 steps | ~5.5 秒 | 与生图速度一致 |
| 图像理解 | 1024x1024 输入 | ~11 秒 | 包含图片编码 |

| **指标** | **A100 (CUDA)** | Ascend NPU (SDPA) |
| --- | --- | --- |
|  |  |  |
| 文生图 512x512 | ~3.5 秒 | ~5.5 秒 |
| 显存占用 | 8.6 GB | 8.6 GB |
| Attention 实现 | FlashAttention2 原生 | SDPA Fallback |