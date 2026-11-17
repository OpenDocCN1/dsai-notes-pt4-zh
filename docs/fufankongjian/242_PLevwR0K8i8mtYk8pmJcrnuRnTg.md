# part2-04-vLLM高性能方案

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/Wgghdxljkog1kwxKygWcUXbRnkc](https://kq4b3vgg5b.feishu.cn/docx/Wgghdxljkog1kwxKygWcUXbRnkc)

# Part 4：大模型部署 — vLLM 高性能方案

> 从"私家车"升级到"客运专线"。本章用 vLLM 部署同款 Qwen3.5-27B 的全精度版本，解锁 19 路并发和更高推理质量。

## 引言

Ollama 就像你的私家车——日常通勤舒适方便，一个人开刚刚好。但如果你要开一个出租车公司，需要同时服务几十个乘客呢？

这时你需要 **vLLM**——**它就是大**模型推理领域的"客运专线"，专为高吞吐、多用户并发设计。同一个 Qwen3.5-27B 模型，vLLM 可以跑出比 HuggingFace Transformers 快 24 倍 的速度。

本章我们将从零搭建 vLLM 环境，部署全精度 Qwen3.5-27B，并与 OpenClaw 对接。掌握 Ollama + vLLM 两种方案，你的技能树才完整。

> **深入理解**：很多课程只教 Ollama 或只教 vLLM，但这远远不够。真实的生产环境中，Ollama 和 vLLM 扮演着完全不同的角色——就像瑞士军刀和专业工具箱，各有用武之地。这一章会让你理解它们的本质差异，而不只是"多学了一种安装方法"。

**本章学习目标**：

+   理解 vLLM 与 Ollama 的定位差异

+   从零创建 Python 虚拟环境并安装 vLLM

+   使用 vLLM 部署 Qwen3.5-27B 全精度模型

+   将 vLLM 与 OpenClaw 对接

+   对比 Ollama 和 vLLM 的实际表现

---

## 4.1 vLLM 是什么？为什么需要它？

### 一句话理解

**vLLM = 大模型推理的"**F1 赛车引擎"。它的核心技术叫 PagedAttention（分页注意力），能像操作系统管理内存一样高效管理 GPU 显存，从而支持更多并发请求。

**类比理解**：

| 维度 | Ollama | vLLM |
| --- | --- | --- |
| 类比 | 私家车 | 公交车/客运专线 |
| 优势 | 一个人开方便、灵活 | 同时拉很多乘客、效率高 |
| 劣势 | 只能坐几个人 | 启动慢、需要规划路线 |
| 适用场景 | 日常通勤 | 公共交通 |

### Ollama vs vLLM 详细对比

| 维度 | Ollama | vLLM | 选择建议 |
| --- | --- | --- | --- |
| **定位** | 个人开发/快速调试 | 生产部署/多用户服务 | 个人用 Ollama，团队用 vLLM |
| **安装难度** | ⭐ 一行命令 | ⭐⭐⭐ 需要 Python 环境 | Ollama 更简单 |
| **推理性能** | 良好 | 卓越（最高比 HF 快 24x） | vLLM 完胜 |
| **并发能力** | 1-2 请求 | **连续批处理，19+ 并发** | vLLM 完胜 |
| **模型格式** | GGUF（量化格式） | HuggingFace 原始权重 | 格式不同，各有优势 |
| **推理精度** | Q4 量化（4-bit） | BF16 全精度（16-bit） | vLLM 更精确 |
| **显存需求** | ~20GB（量化后） | ~58GB（全精度） | Ollama 更省 |
| **API 格式** | 原生 + OpenAI 兼容 | **OpenAI 完全兼容** | vLLM 对接更通用 |
| **启动速度** | 即时 | 1-3 分钟（加载权重） | Ollama 更快 |
| **社区生态** | GitHub 百万星标 | GitHub 6 万+ 星标 | 都很活跃 |

**怎么看这张表****：最后一列"选择建议"是关键—**—两者不是替代关系，而是互补关系。就像你家里有瑞士军刀（Ollama）也有专业工具箱（vLLM），用哪个取决于场景。

**核心结论****：Ollama 是"开发**标配"，vLLM 是"生产必备"——掌握两个方案才是完整技能。

### 关键区别：模型格式不同

|  | Ollama | vLLM |
| --- | --- | --- |
| **格式** | GGUF（量化压缩） | SafeTensors（全精度） |
| **27B 模型大小** | 17 GB | **56 GB** |
| **精度** | 4-bit 量化 | **16-bit 全精度** |
| **推理质量** | 优秀 | **更优（无量化损失）** |
| **来源** | Ollama 模型库 | HuggingFace / ModelScope |

> **重要提醒****：这意味**着 Ollama 下载的模型和 vLLM 不能共用——需要重新下载一份全精度的权重到 vLLM。别担心，数据盘有 1.1TB，两份模型共存完全没问题。

---

## 4.2 显存规划

启动 vLLM 之前，先算一下显存是否够用：

| 进程 | 显存占用 | 说明 |
| --- | --- | --- |
| TTS 服务 | ~4.6 GB | 已在运行的语音合成服务 |
| vLLM（Qwen3.5-27B BF16） | ~58 GB | 全精度模型 + KV 缓存 |
| **合计** | **~63 GB** |  |
| A800 总显存 | 80 GB |  |
| **剩余** | **~17 GB** | 够用 |

> ⚠️ **注意**：如果你之前用 Ollama 加载了 27B 模型（占 ~41GB），需要先卸载它释放显存：

```
ollama stop qwen3.5:27b
```

> 否则两个模型同时加载会超出 80GB，导致 OOM（Out of Memory）。

---

## 4.3 创建 Python 虚拟环境

### 为什么要创建虚拟环境？

系统自带的 Python 已经安装了很多包（比如 TTS 服务的依赖）。直接在系统环境里装 vLLM，很可能产生版本冲突——新包覆盖旧包，原来能跑的服务突然挂了。

**虚拟环境**就像一个"隔离沙盒"——在沙盒里随便装什么，都不会影响外面的世界。

| 方案 | 后果 |
| --- | --- |
| ❌ 直接装在系统环境 | vLLM 依赖的 PyTorch 版本可能覆盖 TTS 用的版本，TTS 服务崩溃 |
| ✅ 创建虚拟环境 | vLLM 和 TTS 各自有独立的依赖，互不影响 |

### 为什么装在数据盘？

**vLLM 及其**依赖（PyTorch、CUDA 库等）加起来约 15-20GB。如果装在系统盘，30GB 的空间直接告急。

### 创建虚拟环境

```
# 确保 conda 可用
export PATH=/root/miniconda3/bin:$PATH

# 在数据盘上创建 vLLM 专用虚拟环境
# --prefix 指定环境安装到数据盘
conda create --prefix /root/autodl-tmp/envs/vllm python=3.12 -y
```

| 参数 | 含义 | 说明 |
| --- | --- | --- |
| conda create | 创建虚拟环境 | conda 的环境管理命令 |
| --prefix /root/autodl-tmp/envs/vllm | 安装到数据盘 | 避免占用系统盘空间 |
| python=3.12 | Python 版本 | vLLM 推荐 3.12 |
| -y | 自动确认 | 跳过交互提示 |

**输入创建命令**：

**创建过程中**：

**创建完成**：

### 激活虚拟环境

```
# 激活 vLLM 虚拟环境
conda activate /root/autodl-tmp/envs/vllm
```

激活后，注意命令行前缀变成了 (vllm)：

> **特别注意**：从现在起，所有 pip install 和 python 命令都在 vllm 虚拟环境中执行。如果你发现命令行前缀没有 (vllm)，说明环境没激活，需要重新执行 conda activate 命令。

---

## 4.4 安装 vLLM

### 使用 uv 加速安装

vLLM 的依赖多达 177 个包，**传统** pip 可能要 10 分钟。先装 uv——pip 的"涡轮增压版"，安装速度快 10-100 倍。

```
# 先安装 uv（pip 的高性能替代品）
pip install uv

# 用 uv 安装 vLLM（自动检测 CUDA 版本）
uv pip install -U vllm --torch-backend=auto
```

| 命令 | 含义 | 说明 |
| --- | --- | --- |
| pip install uv | 安装 uv 工具 | Rust 实现的包管理器 |
| uv pip install -U vllm | 安装 vLLM | -U 升级到最新版 |
| --torch-backend=auto | 自动检测 CUDA | 选择匹配的 PyTorch |

**输入安装命令**：

**安装过程中**——uv 并行下载 177 个包：

**安装完成**：

### 验证安装

```
# 验证 vLLM 版本
python -c &quot;import vllm; print(vllm.__version__)&quot;
# 预期输出：0.17.1
```

> ⚠️ **可能遇到的问题**：

| 问题 | 原因 | 解决方案 |
| --- | --- | --- |
| No space left on device | 虚拟环境装到了系统盘 | 确保用 --prefix /root/autodl-tmp/envs/vllm |
| CUDA 版本不兼容 | vLLM 需要 CUDA ≥ 12.0 | 我们的 CUDA 13.0 完全兼容 |
| PyTorch 版本冲突 | 系统环境干扰 | 确认已激活 vllm 虚拟环境 |

---

## 4.5 下载 Qwen3.5-27B 模型权重

### 格式区别——为什么要重新下载

|  | Ollama 版（已有） | vLLM 版（需下载） |
| --- | --- | --- |
| **格式** | GGUF (Q4_K_M) | SafeTensors (BF16) |
| **大小** | 17 GB | **56 GB** |
| **精度** | 4-bit 量化 | **16-bit 全精度** |
| **位置** | /root/autodl-tmp/ollama-models/ | /root/autodl-tmp/models/Qwen3.5-27B/ |

### 使用 ModelScope 国内直连下载

**HuggingFac**e 在国外，直接下载很慢。ModelScope（魔搭）是阿里巴巴运营的国内模型平台，速度快得多。

```
# 安装 ModelScope 下载工具
pip install modelscope

# 下载 Qwen3.5-27B 全精度权重到数据盘
modelscope download --model Qwen/Qwen3.5-27B \
  --local_dir /root/autodl-tmp/models/Qwen3.5-27B
```

| 参数 | 含义 |
| --- | --- |
| --model Qwen/Qwen3.5-27B | 模型标识（Qwen 团队发布的 27B 版本） |
| --local_dir ... | 下载到数据盘指定目录 |

**输入下载命令**：

**下载过程中**——11 个 safetensors 文件，每个约 5GB，总计约 55GB：

**下载完成**：

> **下载时间参考****：AutoDL **国内网络下载 ModelScope，通常 15-30 分钟。如果需要从 HuggingFace 下载（需代理），命令为：

```
export http_proxy=http://127.0.0.1:10081
export https_proxy=http://127.0.0.1:10081
huggingface-cli download Qwen/Qwen3.5-27B \
  --local-dir /root/autodl-tmp/models/Qwen3.5-27B
```

---

## 4.6 启动 vLLM 推理服务

### 启动命令——每个参数详解

这是本章最重要的一条命令：

```
# 启动 vLLM 推理服务
# 每个参数都有明确的用途，不要省略
vllm serve /root/autodl-tmp/models/Qwen3.5-27B \
  --port 8000 \
  --tensor-parallel-size 1 \
  --max-model-len 32768 \
  --reasoning-parser qwen3 \
  --enable-auto-tool-choice \
  --tool-call-parser qwen3_coder
```

**每个参数的含义和选择理由**：

| 参数 | 值 | 含义 | 为什么这么设 |
| --- | --- | --- | --- |
| 第一个参数 | 模型路径 | 模型权重目录 | 指向数据盘上的权重 |
| --port | 8000 | API 端口 | 8000 是常用的 API 端口 |
| --tensor-parallel-size | 1 | 张量并行数 | 单 GPU，设为 1 |
| --max-model-len | 32768 | 最大上下文 | 32K 是安全值，太大会 OOM |
| --reasoning-parser | qwen3 | 思考模式解析器 | Qwen3 专用，支持&quot;先想后答&quot; |
| --enable-auto-tool-choice | — | 启用工具调用 | Agent 场景必须 |
| --tool-call-parser | qwen3_coder | 工具调用解析格式 | Qwen3 专用格式 |

**输入启动命令**：


**模型加载中**（1-3 分钟）：


**服务就绪**——看到 Uvicorn running on http://0.0.0.0:8000：

![](img/02aca77d64dd4a7c0ec91b16c440dda7.png)

### 关键信息解读

| 信息 | 含义 | 说明 |
| --- | --- | --- |
| dtype: bfloat16 | BF16 全精度推理 | 比 Ollama 的 Q4 精度高 4 倍 |
| Memory usage: 54.80 GB | 模型占用显存 | 全精度模型本身的大小 |
| # GPU blocks: 4912 | KV 缓存可用块数 | 越多，能处理的并发越多 |
| Maximum concurrency: 19 requests | **最多 19 路并发** | **这就是 vLLM 的威力** |

### 查看显存占用

```
# 在新终端中查看 GPU 显存
nvidia-smi
```

| 进程 | 显存 | 说明 |
| --- | --- | --- |
| vLLM | ~69 GB | 模型 51.1GB + KV 缓存 |
| TTS | ~4.6 GB | 共存正常 |
| **合计** | ~74 GB | 80GB 还有余量 |

> ⚠️ **如果出现 OOM**：减小 --max-model-len，比如改为 16384 或 8192。上下文越长，KV 缓存占的显存越多。

---

## 4.7 vLLM API 测试

vLLM 提供 OpenAI 完全兼容的 API——任何能调用 OpenAI API 的工具，都能直接调用 vLLM。

### curl 测试

```
# 通过 vLLM API 调用模型（OpenAI 兼容格式）
curl http://127.0.0.1:8000/v1/chat/completions \
  -H &quot;Content-Type: application/json&quot; \
  -d &#x27;{
    &quot;model&quot;: &quot;Qwen/Qwen3.5-27B&quot;,
    &quot;messages&quot;: [{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;你好&quot;}],
    &quot;max_tokens&quot;: 512
  }&#x27;
```

### API 格式对比：Ollama vs vLLM

|  | Ollama | vLLM |
| --- | --- | --- |
| **地址** | http://127.0.0.1:11434/api/chat | http://127.0.0.1:8000/v1/chat/completions |
| **格式** | Ollama 原生 | **OpenAI 兼容** |
| **model 字段** | qwen3.5:27b | Qwen/Qwen3.5-27B |
| **端口** | 11434 | 8000 |

**输入 API 测试命令**：

**收到响应**：

---

## 4.8 OpenClaw 对接 vLLM

### 配置文件

在 ~/.openclaw/openclaw.json 中添加 vLLM provider：

```
{
  gateway: { mode: &quot;local&quot; },
  models: {
    providers: {
      &quot;local-vllm&quot;: {
        baseUrl: &quot;http://127.0.0.1:8000/v1&quot;,   // vLLM API 地址
        apiKey: &quot;token-placeholder&quot;,             // 不需要认证，随便填
        api: &quot;openai-completions&quot;,               // ★ vLLM 用 OpenAI 兼容模式
        models: [{
          id: &quot;Qwen/Qwen3.5-27B&quot;,
          name: &quot;Qwen3.5 27B vLLM&quot;,
          reasoning: true,                       // 启用思考模式
          input: [&quot;text&quot;],
          contextWindow: 32768,                  // 与 --max-model-len 一致
          maxTokens: 8192
        }]
      }
    }
  },
  agents: {
    defaults: {
      model: { primary: &quot;local-vllm/Qwen/Qwen3.5-27B&quot; }
    }
  }
}
```

### 配置要点对比

| 配置项 | Ollama 配置（Part 3） | vLLM 配置（本章） | 原因 |
| --- | --- | --- | --- |
| api | &quot;ollama&quot; | &quot;openai-completions&quot; | vLLM 提供 OpenAI 兼容 API |
| baseUrl 端口 | 11434 | **8000** | 不同服务不同端口 |
| reasoning | false | **true** | vLLM 支持思考模式 |
| contextWindow | 262144 | **32768** | 必须与 --max-model-len 一致 |

**怎么看这张表**：左列是 Part 3 的 Ollama 配置，中列是本章的 vLLM 配置，右列解释为什么不同。最关键的区别是 api 字段——Ollama 用原生模式，vLLM 用 OpenAI 兼容模式。

### 切换默认模型

把 agents.defaults.model.primary 改为 "local-vllm/Qwen/Qwen3.5-27B"，重启 Gateway 即可。

---

## 4.9 Ollama vs vLLM 性能对比

### 对比实验

相同的问题，分别用 Ollama 和 vLLM 回答：

> "用 Python 实现一个简单的 Web 服务器，带路由和 JSON 响应"

### 对比结果

| 维度 | Ollama（Q4_K_M） | vLLM（BF16） | 差异说明 |
| --- | --- | --- | --- |
| **模型大小** | 17 GB | 56 GB | vLLM 是全精度，文件更大 |
| **显存占用** | ~41 GB | ~58 GB | 全精度模型 + KV 缓存 |
| **推理精度** | 4-bit 量化 | **16-bit 全精度** | vLLM 更精确 |
| **生成速度** | ~39 tokens/s | **~55 tokens/s** | vLLM 快约 40% |
| **最大并发** | 1-2 请求 | **19 请求** | vLLM 是 Ollama 的 10 倍 |
| **回答质量** | 优秀 | **更优** | 全精度无量化损失 |
| **启动速度** | 即时 | 1-3 分钟 | Ollama 加载更快 |
| **安装复杂度** | 一行命令 | 需要 Python 环境 | Ollama 更简单 |
| **适用场景** | 开发调试 | **生产环境** | 各有所长 |

**怎么看这张表**：**没有绝对的赢家**——Ollama 在"简单""快速""省资源"方面领先，vLLM 在"性能""并发""精度"方面碾压。选择取决于你的场景。

### 选型总结

| 场景 | 推荐方案 | 理由 |
| --- | --- | --- |
| 个人日常开发 | **Ollama** | 简单快速，20GB 显存就够 |
| 团队内部工具 | **vLLM** | 19 路并发，多人同时用 |
| 快速原型验证 | **Ollama** | 秒级启动，无需等待 |
| 生产上线 | **vLLM** | 全精度 + 高并发 + 稳定性 |
| 飞书群机器人 | **vLLM** | 多人 @ 时不会排队 |

> **实用建议**：简单记忆——开发调试用 Ollama，生产上线用 vLLM。如果你只是个人玩玩，Ollama 足矣。如果要给团队或客户提供服务，vLLM 是更好的选择。

---

## 4.10 Dashboard 实测：vLLM 后端

配置完成后，在浏览器中验证。

### 启动 Gateway 并查看 Dashboard

注意顶部模型名称已变为 Default (qwen3.5-27b-vllm)：

和 Part 3 对比：

| 章节 | 顶部显示 | 模型后端 |
| --- | --- | --- |
| Part 3 | Default (qwen3.5:27b) | Ollama 量化 |
| **Part 4** | Default (qwen3.5-27b-vllm) | **vLLM 全精度** |

同一个 Agent，同一个 Dashboard，只是换了"大脑"。

### 发送测试消息

| 关注点 | 详情 |
| --- | --- |
| 模型标识 | 底部显示 qwen3.5-27b-vllm |
| 响应速度 | ~55 tokens/s，比 Ollama 快约 40% |
| 回复质量 | 全精度 BF16，更自然流畅 |

> **技术洞察**：注意感受一下 vLLM 和 Ollama 回复的微妙差别——全精度版本的用词往往更精准、表达更自然。虽然差异不大，但在复杂推理和长文本生成场景下会更明显。

---

## 本章小结

| 步骤 | 完成情况 | 关键结果 |
| --- | --- | --- |
| 虚拟环境创建 | ✅ | 数据盘 /root/autodl-tmp/envs/vllm |
| vLLM 安装 | ✅ | v0.17.1（177 个包） |
| 模型权重下载 | ✅ | 56GB 全精度 BF16 |
| vLLM 服务启动 | ✅ | 端口 8000，**支持 19 并发** |
| API 测试 | ✅ | OpenAI 兼容格式 |
| OpenClaw 对接 | ✅ | openai-completions 模式 |
| Dashboard 实测 | ✅ | vLLM 后端对话成功 |
| 性能对比 | ✅ | vLLM 速度快 40%，并发高 10 倍 |

现在你的 Agent 有了两颗大脑——Ollama 适合日常开发，vLLM 适合生产环境。但有时候你可能不想自己维护模型，或者想用最顶尖的闭源模型。下一章，我们来对接云端 API——DeepSeek、Kimi、OpenAI，三分钟搞定。

> **实用建议**：如果你暂时不需要 vLLM，可以 Ctrl+C 停掉 vLLM 服务释放显存，把 openclaw.json 的默认模型改回 Ollama。两种方案可以随时切换，不冲突。