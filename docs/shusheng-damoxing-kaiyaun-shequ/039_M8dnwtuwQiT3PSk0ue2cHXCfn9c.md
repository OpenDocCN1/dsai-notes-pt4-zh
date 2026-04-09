# 沐曦 MetaX C500 部署指南

> 来源：[https://aicarrier.feishu.cn/docx/DoRIdBrPRopkkSxrhU5cWYVVncg](https://aicarrier.feishu.cn/docx/DoRIdBrPRopkkSxrhU5cWYVVncg)

本教程手把手带你在沐曦 MetaX C500 GPU 上完成 InternVL-U 4B 模型的全能力部署，包括文本对话、文生图、图片编辑三大功能。

得益于沐曦 MACA 框架预装的 FlashAttention2 实现，MetaX C500 无需任何额外 Patch 即可直接运行完整模型——这是相比昇腾 NPU 部署的一大优势。

## 关于沐曦 MetaX

沐曦集成电路（上海）有限公司是一家专注于高性能 GPU 芯片设计的国产半导体企业。MetaX C500 是其面向数据中心推出的 AI 推理/训练 GPU，搭配自研 MACA（MetaX Architecture for Computing Acceleration）计算框架，提供与 CUDA 高度兼容的编程接口。

MACA 框架核心特性：

本教程手把手带你在沐曦 MetaX C500 GPU 上完成 InternVL-U 4B 模型的全能力部署，包括文本对话、文生图、图片编辑三大功能。

得益于沐曦 MACA 框架预装的 FlashAttention2 实现，MetaX C500 无需任何额外 Patch 即可直接运行完整模型——这是相比昇腾 NPU 部署的一大优势。

## 关于沐曦 MetaX

沐曦集成电路（上海）有限公司是一家专注于高性能 GPU 芯片设计的国产半导体企业。MetaX C500 是其面向数据中心推出的 AI 推理/训练 GPU，搭配自研 MACA（MetaX Architecture for Computing Acceleration）计算框架，提供与 CUDA 高度兼容的编程接口。

MACA 框架核心特性：

## 获取 MetaX C500 算力

如果你还没有 MetaX C500 GPU 资源，可以通过 Gitee AI 算力市场 按需租用沐曦 C500 算力实例，支持小时级起租。

创建实例时选择镜像：PyTorch / 2.8.0 / Python 3.12 / MACA 3.3.0.4，该镜像已预装 MACA 框架、PyTorch 和 FlashAttention2，开箱即用。

> 联系书生社区小助手可获取算力优惠券，降低体验成本。

## 前置条件

## Step 1：环境准备

### 1.1 设置 MACA 环境变量

MetaX C500 的 MACA 框架安装在 /opt/maca，使用前需设置环境变量：

```
export MACA_PATH=/opt/maca
```

> 建议将以上内容写入 ~/.bashrc，避免每次手动执行：

> `bash

> echo 'export MACA_PATH=/opt/maca' >> ~/.bashrc

> source ~/.bashrc

> `

### 1.2 验证 GPU 状态

使用 mx-smi（沐曦版 nvidia-smi）查看 GPU 状态：

```
mx-smi
```

期望输出类似：

```
=================== MetaX System Management Interface Log ===================
Timestamp                                         : Wed Mar 18 06:44:43 2026

Attached GPUs                                     : 1
+---------------------------------------------------------------------------------+
| MX-SMI 2.2.9                       Kernel Mode Driver Version: 3.0.11           |
| MACA Version: 3.3.0.15             BIOS Version: 1.27.5.0                       |
|------------------+-----------------+---------------------+----------------------|
| Board       Name | GPU   Persist-M | Bus-id              | GPU-Util      sGPU-M |
| Pwr:Usage/Cap    | Temp       Perf | Memory-Usage        | GPU-Util             |
|==================+=================+=====================+======================|
| 0     MetaX C500 | 0           Off | 0000:10:00.0        | 0%          Disabled |
| 50W / 350W       | 35C          P9 | 0/65536 MiB         | Available            |
+------------------+-----------------+---------------------+----------------------+
```

确认可以看到 MetaX C500 设备信息，显存 65536 MiB（64 GB）。

### 1.3 验证 PyTorch 可用

MetaX C500 环境通常预装了 MACA 适配的 PyTorch。使用 Conda 环境中的 Python：

```
/opt/conda/bin/python3 -c &quot;

print(f&#x27;PyTorch 版本: {torch.__version__}&#x27;)
print(f&#x27;CUDA 可用: {torch.cuda.is_available()}&#x27;)
print(f&#x27;GPU 设备数: {torch.cuda.device_count()}&#x27;)
print(f&#x27;GPU 名称: {torch.cuda.get_device_name(0)}&#x27;)
&quot;
```

期望输出：

```
PyTorch 版本: 2.8.0+metax3.3.0.2
CUDA 可用: True
GPU 设备数: 1
GPU 名称: MetaX C500
```

> 关键点：MACA 提供 CUDA 兼容 API，因此代码中使用 torch.cuda 接口即可，无需导入任何特殊库（不像昇腾需要 torch_npu）。这意味着几乎所有基于 CUDA 的 PyTorch 代码可以在 MetaX C500 上零修改运行。

### 1.4 检查预装的关键库

```
/opt/conda/bin/pip list | grep -iE &quot;flash_attn|torch|transformers|xformers&quot;
```

期望看到类似：

```
flash_attn         2.6.3+metax3.3.0.2torch2.8
torch              2.8.0+metax3.3.0.2
transformers       4.x.x
xformers           0.0.22+metax3.3.0.2torch2.8
```

> 注意 flash_attn 已预装（MACA 版本），这正是 MetaX C500 不需要 SDPA Patch 的原因。

### 1.5 创建独立 Conda 环境（推荐）

为了避免污染系统预装的 Conda 环境，建议创建一个独立的虚拟环境：

```
# 基于系统 Python 创建独立环境，继承 MACA 预装的 PyTorch / flash_attn
/opt/conda/bin/conda create -n internvlu --clone base -y
/opt/conda/bin/conda activate internvlu

# 后续所有 pip 命令使用该环境
which python3
# /opt/conda/envs/internvlu/bin/python3
```

> 如果 conda create --clone 耗时较长或磁盘空间不足，也可以直接使用系统 base 环境（/opt/conda/bin/python3），但需注意避免依赖冲突。

### 1.6 安装额外依赖

```
# 如果使用了独立环境
pip install fastapi uvicorn pillow modelscope accelerate

# 如果使用系统 base 环境
/opt/conda/bin/pip install fastapi uvicorn pillow modelscope accelerate
```

## Step 2：下载模型和代码

### 2.1 下载 InternVL-U 模型权重

HuggingFace 在国内网络不可用，使用 ModelScope 镜像下载：

```
/opt/conda/bin/python3 -c &quot;
from modelscope import snapshot_download
snapshot_download(&#x27;OpenGVLab/InternVL-U&#x27;, local_dir=&#x27;/data/models/OpenGVLab/InternVL-U&#x27;)
&quot;
```

模型约 8.2 GB，下载完成后目录结构：

```
/data/models/OpenGVLab/InternVL-U/
├── configuration.json
├── generation_decoder/        # Diffusion 解码器权重
│   ├── config.json
│   └── diffusion_pytorch_model.safetensors
├── model_index.json
├── processor/                 # 图像处理器配置
├── scheduler/                 # 扩散调度器配置
├── vae/                       # VAE 编解码器权重
│   ├── config.json
│   └── diffusion_pytorch_model.safetensors
└── vlm/                       # 视觉语言模型权重
    ├── config.json
    ├── model-00001-of-00002.safetensors
    ├── model-00002-of-00002.safetensors
    └── tokenizer.json
```

### 2.2 下载 InternVL-U 推理代码

InternVL-U 的 Pipeline 代码需要单独克隆：

```
cd /data
git clone https://github.com/OpenGVLab/InternVL-U.git InternVL-U-code
```

> 如果 GitHub 访问受限，可使用镜像：

> `bash

> git clone https://mirror.ghproxy.com/https://github.com/OpenGVLab/InternVL-U.git InternVL-U-code

> `

代码目录结构：

```
/data/InternVL-U-code/
├── internvlu/
│   ├── __init__.py
│   ├── pipeline_internvlu.py      # 主 Pipeline（推理入口）
│   └── diffusion/
│       └── internvlu_transformer.py  # Diffusion Transformer（使用 flash_attn）
├── requirements.txt
└── README.md
```

### 2.3 安装推理代码依赖

```
cd /data/InternVL-U-code
/opt/conda/bin/pip install -r requirements.txt
```

## Step 3：理解 MACA 与 FlashAttention2 的关系

在开始验证模型之前，有必要理解为什么 MetaX C500 不需要 昇腾教程中那样的 SDPA Fallback Patch。

### 三大平台的 FlashAttention2 支持情况

InternVL-U 的 Diffusion 路径（internvlu_transformer.py）硬编码调用了以下 flash_attn 函数：

在昇腾 NPU 上，这些函数全部需要用 PyTorch SDPA 手动替换。而 MetaX C500 的 MACA 框架已经提供了完整的 flash_attn 实现（版本 2.6.3+metax3.3.0.2torch2.8），所有函数调用可以直接走通。

验证 flash_attn 可用：

```
/opt/conda/bin/python3 -c &quot;

print(f&#x27;flash_attn 版本: {flash_attn.__version__}&#x27;)
from flash_attn import flash_attn_func, flash_attn_varlen_func
print(&#x27;flash_attn_func 可用: OK&#x27;)
print(&#x27;flash_attn_varlen_func 可用: OK&#x27;)
&quot;
```

## Step 4：验证模型加载

```
os.environ[&quot;MACA_PATH&quot;] = &quot;/opt/maca&quot;
sys.path.insert(0, &quot;/data/InternVL-U-code&quot;)


from internvlu.pipeline_internvlu import InternVLUPipeline

print(&quot;正在加载 InternVL-U 模型...&quot;)
pipe = InternVLUPipeline.from_pretrained(
    &#x27;/data/models/OpenGVLab/InternVL-U&#x27;,
    torch_dtype=torch.bfloat16
)
pipe = pipe.to(&quot;cuda:0&quot;)

mem_gb = torch.cuda.memory_allocated(0) / 1e9
total_gb = torch.cuda.get_device_properties(0).total_memory / 1e9
print(f&quot;GPU 显存占用: {mem_gb:.1f} GB / {total_gb:.0f} GB&quot;)
print(f&quot;剩余显存: {total_gb - mem_gb:.0f} GB&quot;)

```

期望输出：

```
正在加载 InternVL-U 模型...
GPU 显存占用: 8.7 GB / 64 GB
剩余显存: 55 GB
模型加载成功!
```

> 启动时可能看到的警告（均可忽略）：

> - flash_attn is installed but failed to import: cannot import name '_wrapped_flash_attn_backward' — MACA 版 flash_attn 的已知无害警告，前向推理正常

> - FutureWarning: torch.cuda.amp.custom_fwd — PyTorch API 迁移警告

> - torchvision.datapoints Beta — torchvision 内部警告

## Step 5：测试三大能力

### 5.1 文本对话

```
result = pipe(prompt=&quot;你好，用一句话介绍你自己&quot;, generation_mode=&quot;text&quot;)
text = pipe.tokenizer.decode(result.generate_output[0], skip_special_tokens=True)
print(text)

```

> 注意：pipeline 返回的是 token IDs tensor，需要用 pipe.tokenizer.decode() 解码为文本。

测试多轮对话能力：

```
# 带上下文的多轮对话
result = pipe(
    prompt=&quot;请用 Python 写一个快速排序算法&quot;,
    generation_mode=&quot;text&quot;,
    max_new_tokens=1024,
    temperature=0.7
)
text = pipe.tokenizer.decode(result.generate_output[0], skip_special_tokens=True)

```

### 5.2 图像理解

InternVL-U 支持上传图片并进行理解分析：

```
from PIL import Image

# 加载一张图片
img = Image.open(&quot;test_image.jpg&quot;).convert(&quot;RGB&quot;)

result = pipe(
    prompt=&quot;详细描述这张图片的内容&quot;,
    image=img,
    generation_mode=&quot;text&quot;,
    max_new_tokens=512
)
text = pipe.tokenizer.decode(result.generate_output[0], skip_special_tokens=True)

```

### 5.3 文生图

```
t0 = time.time()
result = pipe(
    prompt=&quot;一只穿着宇航服的猫咪在太空中漫步，背景是星云和银河&quot;,
    generation_mode=&quot;image&quot;,
    height=512, width=512,
    num_inference_steps=20,
    all_cfg_scale=4.5
)
result.images[0].save(&quot;cat_space.png&quot;)
print(f&quot;生成耗时: {time.time()-t0:.1f} 秒&quot;)

```

尝试不同分辨率：

```
# 高分辨率生成（更慢但更精细）
result = pipe(
    prompt=&quot;水墨画风格的山水风景，云雾缭绕&quot;,
    generation_mode=&quot;image&quot;,
    height=1024, width=1024,
    num_inference_steps=20,
    all_cfg_scale=4.5
)

```

> Prompt 技巧：描述越具体效果越好。推荐结构：主体 + 动作/姿态 + 风格 + 光影 + 背景。支持的画风包括：写实、油画、水彩、水墨、赛博朋克、像素风、扁平插画等。

### 5.4 图片编辑

```
source = Image.open(&quot;input.png&quot;).convert(&quot;RGB&quot;)

# 局部编辑
result = pipe(
    prompt=&quot;把背景换成海滩&quot;,
    image=source,
    generation_mode=&quot;image&quot;,
    height=512, width=512,
    num_inference_steps=20,
    all_cfg_scale=4.5
)
result.images[0].save(&quot;edited_beach.png&quot;)

# 风格迁移
result = pipe(
    prompt=&quot;转为油画风格&quot;,
    image=source,
    generation_mode=&quot;image&quot;,
    height=512, width=512,
    num_inference_steps=20,
    all_cfg_scale=4.5
)

```

> 编辑类型参考：

> - 局部编辑："给人物加上墨镜"、"把天空换成星空"

> - 全局风格："转为黑白照片"、"添加复古滤镜"、"变成水彩画风格"

> - 场景变换："把背景换成雪山"、"放在太空宇宙中"

## Step 6：搭建 FastAPI 服务

创建 /data/api_server.py，将模型封装为 REST API，供前端 Playground 或其他服务调用：

> 安全提示：如果服务暴露在公网，必须通过 API_KEY 环境变量设置鉴权密钥，否则任何人都可以调用你的 GPU 资源。同时建议在反向代理（如 Nginx）层面配置速率限制。

```
#!/usr/bin/env python3
&quot;&quot;&quot;InternVL-U FastAPI server for MetaX C500.&quot;&quot;&quot;

os.environ[&quot;MACA_PATH&quot;] = &quot;/opt/maca&quot;
sys.path.insert(0, &quot;/data/InternVL-U-code&quot;)


from internvlu.pipeline_internvlu import InternVLUPipeline
from PIL import Image
from fastapi import FastAPI, Request
from fastapi.responses import JSONResponse


app = FastAPI(title=&quot;InternVL-U API (MetaX C500)&quot;)
pipe = None

# API Key 鉴权：如未设置，自动生成随机密钥并打印到日志
API_KEY = os.environ.get(&quot;API_KEY&quot;, &quot;&quot;)
if not API_KEY:
    API_KEY = secrets.token_urlsafe(32)
    print(f&quot;[WARN] API_KEY not set, auto-generated: {API_KEY}&quot;)
    print(&quot;[WARN] Set API_KEY env var for production use!&quot;)

# 输入限制常量
MAX_BASE64_SIZE = 10 * 1024 * 1024  # 10 MB base64 上限
MAX_PROMPT_LENGTH = 4096             # prompt 最大字符数
ALLOWED_IMAGE_FORMATS = {&quot;JPEG&quot;, &quot;PNG&quot;, &quot;WEBP&quot;, &quot;BMP&quot;, &quot;GIF&quot;}

def validate_base64_image(b64_str: str) -&gt; Image.Image:
    &quot;&quot;&quot;验证并解码 base64 图片，检查大小和格式。&quot;&quot;&quot;
    if len(b64_str) &gt; MAX_BASE64_SIZE:
        raise ValueError(f&quot;Image too large (max {MAX_BASE64_SIZE // 1024 // 1024} MB)&quot;)
    img = Image.open(io.BytesIO(base64.b64decode(b64_str))).convert(&quot;RGB&quot;)
    if img.format and img.format.upper() not in ALLOWED_IMAGE_FORMATS:
        raise ValueError(f&quot;Unsupported image format: {img.format}&quot;)
    # 限制图片尺寸，防止 OOM
    if img.width &gt; 4096 or img.height &gt; 4096:
        raise ValueError(f&quot;Image too large: {img.width}x{img.height} (max 4096x4096)&quot;)
    return img

def get_pipe():
    &quot;&quot;&quot;懒加载模型，首次调用时初始化。&quot;&quot;&quot;
    global pipe
    if pipe is None:
        print(&quot;[INIT] Loading InternVL-U on MetaX C500...&quot;)
        t0 = time.time()
        pipe = InternVLUPipeline.from_pretrained(
            &quot;/data/models/OpenGVLab/InternVL-U&quot;, torch_dtype=torch.bfloat16
        )
        pipe = pipe.to(&quot;cuda:0&quot;)
        mem = torch.cuda.memory_allocated(0) / 1e9
        print(f&quot;[INIT] Ready in {time.time()-t0:.1f}s, GPU mem: {mem:.1f} GB&quot;)
    return pipe

@app.on_event(&quot;startup&quot;)
async def startup():
    &quot;&quot;&quot;服务启动时预加载模型，避免首次请求超时。&quot;&quot;&quot;
    get_pipe()


```

### 启动服务

```
export MACA_PATH=/opt/maca
export API_KEY=&quot;your-secret-api-key&quot;  # 设置 API 鉴权密钥
nohup /opt/conda/bin/python3 /data/api_server.py &gt; /data/api_server.log 2&gt;&amp;1 &amp;
```

查看启动日志：

```
tail -f /data/api_server.log
# 等待看到 &quot;Uvicorn running on http://0.0.0.0:8000&quot; 即启动成功
```

### 验证所有接口

健康检查：

```
curl http://localhost:8000/health
# {&quot;status&quot;:&quot;ok&quot;,&quot;device&quot;:&quot;metax-c500&quot;,&quot;model&quot;:&quot;InternVL-U-4B&quot;,&quot;gpu_mem_gb&quot;:8.7}
```

文本对话：

```
curl -X POST http://localhost:8000/v1/chat \
  -H &#x27;Content-Type: application/json&#x27; \
  -H &#x27;X-API-Key: your-secret-api-key&#x27; \
  -d &#x27;{&quot;messages&quot;:[{&quot;role&quot;:&quot;user&quot;,&quot;content&quot;:&quot;用一句话解释什么是大语言模型&quot;}]}&#x27;
# {&quot;text&quot;:&quot;大语言模型是...&quot;,&quot;latency&quot;:1.07,&quot;device&quot;:&quot;metax-c500&quot;}
```

文生图：

```
curl -X POST http://localhost:8000/v1/generate \
  -H &#x27;Content-Type: application/json&#x27; \
  -H &#x27;X-API-Key: your-secret-api-key&#x27; \
  -d &#x27;{&quot;prompt&quot;:&quot;一只可爱的柴犬坐在樱花树下&quot;,&quot;height&quot;:512,&quot;width&quot;:512}&#x27; \
  -o response.json
# 返回 JSON 包含 base64 编码的图片
```

图片编辑（需提供 base64 编码的源图）：

```
# 将图片转为 base64
IMAGE_B64=$(base64 -w0 input.png)

curl -X POST http://localhost:8000/v1/edit \
  -H &#x27;Content-Type: application/json&#x27; \
  -H &#x27;X-API-Key: your-secret-api-key&#x27; \
  -d &quot;{\&quot;prompt\&quot;:\&quot;把背景换成星空\&quot;,\&quot;image\&quot;:\&quot;$IMAGE_B64\&quot;}&quot; \
  -o edit_response.json
```

## Step 7：配置公网访问（可选）

如果你的 MetaX C500 服务器没有公网 IP，需要通过 SSH 隧道暴露服务。

### 方案：通过公网服务器 SSH 隧道

假设你有一台公网服务器 YOUR_SERVER_IP：

```
# 在公网服务器上执行，将 8102 端口转发到 MetaX 的 8000 端口
ssh -N -L 0.0.0.0:8102:localhost:8000 \
  -p &lt;METAX_SSH_PORT&gt; root@&lt;METAX_SSH_HOST&gt; \
  -o StrictHostKeyChecking=no \
  -o UserKnownHostsFile=/dev/null \
  -o ServerAliveInterval=30 \
  -o ServerAliveCountMax=3 \
  -o ExitOnForwardFailure=yes
```

使用 sshpass 实现自动重连：

```
# 安装 sshpass
apt install -y sshpass

# 带密码的自动重连隧道
while true; do
    echo &quot;[$(date)] Starting SSH tunnel...&quot;
    sshpass -p &#x27;YOUR_PASSWORD&#x27; ssh -N \
        -L 0.0.0.0:8102:localhost:8000 \
        -p &lt;METAX_SSH_PORT&gt; root@&lt;METAX_SSH_HOST&gt; \
        -o StrictHostKeyChecking=no \
        -o UserKnownHostsFile=/dev/null \
        -o ServerAliveInterval=30 \
        -o ServerAliveCountMax=3 \
        -o ExitOnForwardFailure=yes \
        2&gt;/dev/null
    echo &quot;[$(date)] Tunnel disconnected, reconnecting in 5s...&quot;
    sleep 5
done
```

> 建议将隧道脚本保存为 .sh 文件并用 nohup 后台运行。

验证公网可达：

```
curl http://YOUR_SERVER_IP:8102/health
# {&quot;status&quot;:&quot;ok&quot;,&quot;device&quot;:&quot;metax-c500&quot;,&quot;model&quot;:&quot;InternVL-U-4B&quot;,&quot;gpu_mem_gb&quot;:8.7}
```

### 安全组配置

如果你的公网服务器在阿里云/腾讯云等云平台上，还需要在安全组中开放 8102 端口的入站规则：

## 性能参考

以下数据基于 MetaX C500（64 GB VRAM，MACA 3.3.0）实测：

### 三平台性能对比

> MetaX C500 的文生图性能接近 A100，文本对话速度也非常快。得益于 MACA 框架对 FlashAttention2 的原生支持，Attention 计算效率高于 SDPA Fallback 方案。

## 常见问题

### Q: 启动时出现 `_wrapped_flash_attn_backward` 相关警告

```
flash_attn is installed but failed to import: cannot import name &#x27;_wrapped_flash_attn_backward&#x27;

```

这是 MACA 版 flash_attn 的已知无害警告。尽管提示 "Falling back"，实际前向推理的 FlashAttention2 仍正常工作，不影响任何功能和性能。忽略即可。

### Q: `mx-smi` 显示 GPU 利用率很高

模型加载到 GPU 后常驻显存（约 8.7 GB / 64 GB），mx-smi 显示较高的 Memory-Usage 是正常现象。GPU-Util 在推理时会波动，空闲时接近 0%。

### Q: 什么是 MACA？

MACA（MetaX Architecture for Computing Acceleration）是沐曦自研的 GPU 计算框架，类似 NVIDIA 的 CUDA。它的核心特性：

### Q: 能部署更大的模型吗？

InternVL-U 4B 仅占用 8.7 GB 显存，C500 有 64 GB VRAM，剩余 55+ GB 足够部署更大模型或多实例并发。例如可以同时加载多个 InternVL-U 实例用于负载均衡。

### Q: 与昇腾 NPU 教程相比，为什么这里不需要 SDPA Patch？

昇腾 NPU 无法安装 CUDA 原生的 flash_attn 库（因为 flash_attn 底层依赖 CUDA kernel），因此需要用 PyTorch 原生 F.scaled_dot_product_attention 手动替代全部 flash_attn 函数。

而 MetaX C500 的 MACA 框架已预装了兼容版本的 flash_attn（版本号 2.6.3+metax3.3.0.2torch2.8），其底层使用 MACA kernel 实现，对上层 Python API 完全兼容。模型可以直接调用，无需任何 Monkey Patch。

### Q: 如何监控 GPU 状态？

```
# 实时监控（每秒刷新）
watch -n 1 mx-smi

# 查看 GPU 进程
mx-smi  # 底部会列出占用 GPU 的进程和显存
```

### Q: 服务启动后如何查看日志？

```
# 查看实时日志
tail -f /data/api_server.log

# 查看最近的请求记录
grep &quot;POST\|GET&quot; /data/api_server.log | tail -20
```

## 总结

MetaX C500 凭借 MACA 框架的 CUDA 兼容能力，实现了 InternVL-U 的开箱即用部署。这个方案的核心优势在于：

1. 零额外适配：MACA 预装 FlashAttention2，无需编写任何 Patch，部署流程比昇腾 NPU 更简单

2. 标准 CUDA API：使用 torch.cuda 接口，代码与 NVIDIA A100 完全一致，迁移成本为零

3. 功能完整：对话、图像理解、文生图、图片编辑四大能力全部可用

4. 性能优异：文生图速度接近 A100（~4-5s vs ~3.5s），文本推理速度快，显著优于 SDPA Fallback 方案

5. 国产替代：作为国产 GPU 方案，MetaX C500 展示了在 AI 推理场景下替代 NVIDIA GPU 的可行性

| **项目** | **要求** |
| --- | --- |
|  |  |
| 硬件 | 沐曦 MetaX C500（64 GB VRAM，350W TDP） |
| 驱动 | MACA 3.3.0.4+（沐曦 GPU 计算框架） |
| PyTorch | 2.8.0（MACA 适配版，镜像已预装） |
| Python | 3.12（推荐使用镜像预装的 Conda 环境） |
| 操作系统 | Ubuntu 22.04 / 24.04 |
| 磁盘 | &gt;= 30 GB（模型 ~8.2 GB + 代码 + 依赖） |
| 内存 | &gt;= 32 GB（模型加载时需要） |

| **平台** | **flash_attn 库** | **是否需要 Patch** |
| --- | --- | --- |
|  |  |  |
| NVIDIA A100 | 原生 CUDA 版本，pip 安装 | 不需要 |
| 华为昇腾 NPU | 不支持（CUDA-only），无法安装 | 需要 SDPA Patch |
| 沐曦 MetaX C500 | MACA 适配版本，环境预装 | 不需要 |

| **能力** | **参数** | **耗时** | **备注** |
| --- | --- | --- | --- |
|  |  |  |  |
| 文本对话 | 短文本 ~64 tokens | 1~2 秒 | 首次推理含 warmup |
| 图像理解 | 1024x1024 输入 | ~8 秒 | 包含图片编码 |
| 文生图 | 512x512, 20 steps | ~4-5 秒 | 热启动后稳定 |
| 文生图 | 1024x1024, 20 steps | ~15 秒 | 大尺寸更精细但更慢 |
| 图片编辑 | 512x512, 20 steps | ~4-5 秒 | 与生图速度一致 |
| 模型加载 | 首次启动 | ~30 秒 | 含权重加载和 GPU 初始化 |
| 显存占用 | bfloat16 | 8.7 GB | 64 GB 显存余量充足 |

| **指标** | **A100 (CUDA)** | **昇腾 NPU (SDPA)** | MetaX C500 (MACA) |
| --- | --- | --- | --- |
| 文生图 512x512 | ~3.5 秒 | ~5.5 秒 | ~4-5 秒 |
| 文本对话 | ~1 秒 | ~3-5 秒 | ~1-2 秒 |
| 显存占用 | 8.6 GB | 8.6 GB | 8.7 GB |
| 总显存 | 80 GB | 64 GB | 64 GB |
| Attention 实现 | FlashAttention2 原生 | SDPA Fallback | FlashAttention2 (MACA) |
| 是否需要 Patch | 否 | 是（SDPA Patch） | 否 |
| 设备接口 | cuda:0 | npu:0 | cuda:0 |
| GPU 工具 | nvidia-smi | npu-smi | mx-smi |