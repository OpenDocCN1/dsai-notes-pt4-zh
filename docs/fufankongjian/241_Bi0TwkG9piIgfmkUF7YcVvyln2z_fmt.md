
# part2-03-Ollama大模型部署

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/IDC2dRpwnoQgiVxBbmBcMivCnMZ](https://kq4b3vgg5b.feishu.cn/docx/IDC2dRpwnoQgiVxBbmBcMivCnMZ)

## Part 3：大模型部署 — Ollama 方案

> 给空壳机器人装上大脑。本章用 Ollama 在 A800 上部署 270 亿参数的 Qwen3.5 大模型，从安装到 Dashboard 实战，一气呵成。

### 引言

OpenClaw 就像一个空壳机器人——四肢健全、感官灵敏，但没有大脑。它知道怎么接收消息、怎么调用工具，但不知道怎么"思考"。

这一章我们要做的，就是给它装上大脑。

而且不是随便一颗大脑——是 270 亿参数的 Qwen3.5，一个能思考、会推理、能使用工具的开源大模型。更妙的是，这颗大脑完全跑在你自己的服务器上，数据不出机房，完全私有化。

我们选择 Ollama 作为第一个部署方案，因为它真正做到了"一行命令跑大模型"——安装简单、社区活跃、与 OpenClaw 原生集成。先从最简单的方案入门，后面 Part 4 再上高性能的 vLLM。

> **深入理解**：Part 3 是全课程的核心转折点——从这一章开始，你手上的 Agent 就能"活"起来了。前面两章都是"准备"，这一章是"见证奇迹"。所以我建议你找一个不被打扰的时间段来学习，一口气做完，享受这个从"空白终端"到"能干活的 AI Agent"的完整过程。

### 本章学习目标

+ 在 AutoDL 上安装并启动 Ollama

+ 理解大模型量化的概念和选型逻辑

+ 下载并运行 Qwen3.5-27B 模型

+ 将 Ollama 与 OpenClaw 对接

+ 通过 Dashboard 让 Agent 在服务器上"干活"

---

### 3.1 Ollama 是什么？一句话理解

类比：如果把大模型比作各种口味的咖啡豆，那 Ollama 就是一台"全自动咖啡机"——你只需要告诉它想要什么口味（模型名称），它自动帮你下载、研磨（加载）、冲泡（推理），最后端上一杯热咖啡（回答）。

Ollama 的三大核心能力：

| 能力 | 说明 | 对应命令 | 类比 |

| --- | --- | --- | --- |

| 模型管理 | 下载、删除、查看模型 | `ollama pull`、`ollama list`、`ollama rm` | 咖啡豆仓库——管理你的豆子库存 |

| 推理服务 | 在 GPU 上运行模型 | `ollama serve`、`ollama run` | 咖啡机——把豆子变成咖啡 |

| API 接口 | 提供 HTTP API 让其他程序调用 | `http://127.0.0.1:11434` | 外卖窗口——让 OpenClaw 来"取咖啡" |

为什么选 Ollama 做第一个方案？

| 优势 | 详细说明 |

| --- | --- |

| 安装极简 | 一行脚本安装，不需要配 Python、不需要装 CUDA Toolkit |

| 社区活跃 | GitHub 百万星标，几乎支持所有主流开源模型 |

| 格式高效 | 使用 GGUF 量化格式，大幅降低显存需求 |

| 与 OpenClaw 原生集成 | OpenClaw 有专门的 api: `ollama` 模式，对接零适配 |

| 启动快 | 模型加载到 GPU 后，即时响应 |

---

### 3.2 安装前置依赖：zstd

#### 为什么需要 zstd？

在安装 Ollama 之前，需要先安装一个叫 `zstd` 的小工具。

`zstd`（Zstandard）是 Facebook 开发的高效压缩算法。Ollama 从 0.18 版本开始，用 `zstd` 来压缩它的安装包以减小下载体积。Ubuntu 22.04 默认没有安装 `zstd`。

> ⚠️ 这是全课程的第一个坑：如果跳过这步直接安装 Ollama，安装脚本会报错 `ERROR: This version requires zstd for extraction.`——很多教程没提到这个依赖，导致新手卡在第一步。

#### 安装 zstd

```
apt-get install -y zstd
```

| 参数 | 含义 |

| --- | --- |

| `apt-get install` | Ubuntu 的包管理命令，安装软件 |

| `-y` | 自动回答"是"，跳过确认提示 |

| `zstd` | 要安装的包名 |

输入安装命令：

安装过程中：

安装完成：

看到 `Setting up zstd` 就表示安装成功。整个过程几秒钟就完成。

---

### 3.3 安装 Ollama

#### 运行官方安装脚本

```
curl -fsSL https://ollama.com/install.sh | sh
```

| 参数 | 含义 | 详细说明 |

| --- | --- | --- |

| `curl` | 下载工具 | 从网络获取文件 |

| `-f` | fail silently | HTTP 错误时不显示 HTML 错误页 |

| `-s` | silent | 不显示进度条 |

| `-S` | show errors | 配合 `-s` 使用，有错误时仍然显示 |

| `-L` | follow redirects | 如果 URL 重定向，自动跟随 |

| `https://ollama.com/install.sh` | Ollama 官方安装脚本 | 会检测系统、下载二进制、配置服务 |

| `sh` | 管道执行 | 下载后直接用 shell 执行 |

输入安装命令：

安装进行中：

安装完成——注意看两个 WARNING：

#### 两个 WARNING 不要慌——逐一解读

安装输出中有两行黄色的 WARNING，很多人看到会紧张。但请放心，它们都不影响正常使用：

| WARNING 内容 | 原因 | 影响 | 解决方案 |

| --- | --- | --- | --- |

| `systemd is not running` | AutoDL 是 Docker 容器，没有 systemd | 不影响功能 | 手动启动 Ollama（3.5 节会讲） |

| `Unable to detect NVIDIA/AMD GPU` | 安装脚本用 `lspci` 检测，但容器没装 `pciutils` | 不影响功能 | 运行时通过 CUDA 库正确检测 GPU |

> 重要提醒：遇到 WARNING 和 ERROR 要区分对待——WARNING 是"提醒你注意"，ERROR 才是"真的出错了"。这两个 WARNING 完全可以忽略。

#### 验证安装

```
ollama --version
```

预期输出：`ollama version is 0.18.1`

看到版本号就表示安装成功。

---

### 3.4 配置模型存储目录（关键！）

#### 为什么这一步如此重要？

还记得 Part 2 说的"系统盘 vs 数据盘"吗？Ollama 默认把模型下载到 `~/.ollama/models/`——这个路径在系统盘上。

问题来了：系统盘只有约 30GB，我们要下的 Qwen3.5-27B 模型有 17GB。再加上其他文件，系统盘直接撑爆，整个服务器可能崩溃。

| 场景 | 模型存储位置 | 后果 |

| --- | --- | --- |

| 不配置 | `~/.ollama/models/`（系统盘） | 17GB 模型 + 系统文件 > 30GB，磁盘撑爆 |

| 配置后 | `/root/autodl-tmp/ollama-models/`（数据盘） | 数据盘 1.1TB，完全不用担心 |

> ⚠️ 这是全课程最常见的踩坑点：很多人安装完 Ollama 就兴冲冲地下载模型，结果系统盘满了、服务器变得极度卡顿，SSH 都连不上。到时候只能在 AutoDL 控制台强制重启。

#### 配置命令

```
export OLLAMA_MODELS=/root/autodl-tmp/ollama-models

mkdir -p /root/autodl-tmp/ollama-models
```

| 命令 | 含义 | 说明 |

| --- | --- | --- |

| `export OLLAMA_MODELS=...` | 设置环境变量 | 告诉 Ollama 把模型存到这个路径 |

| `mkdir -p ...` | 创建目录 | `-p` 表示父目录不存在时自动创建 |

输入配置命令：

验证配置：

```
echo $OLLAMA_MODELS
```

预期输出：`/root/autodl-tmp/ollama-models`

> 重要提醒：这个环境变量必须在每次启动 Ollama 之前设置好。好消息是，我们在 Part 2 已经把它写入了 `.bashrc`，所以每次登录都会自动生效。如果你跳过了 Part 2 的 2.8 节，请现在回去补上。

---

### 3.5 启动 Ollama 服务

#### 为什么要手动启动？

正常的 Linux 服务器使用 systemd 来管理服务——安装完 Ollama 后，它会自动注册为系统服务，开机自启。但 AutoDL 是 Docker 容器环境，没有 systemd。这就是安装时报 WARNING 的原因。我们需要手动启动。

#### 启动命令逐字解释

```
export OLLAMA_MODELS=/root/autodl-tmp/ollama-models

nohup ollama serve > /tmp/ollama.log 2>&1 &
```

| 部分 | 含义 | 为什么需要 |

| --- | --- | --- |

| `export OLLAMA_MODELS=...` | 设置模型存储路径 | 防止模型下到系统盘 |

| `nohup` | "No Hang Up" | 断开 SSH 后进程不被杀死 |

| `ollama serve` | 启动推理服务 | Ollama 的核心服务进程 |

| `> /tmp/ollama.log` | 标准输出重定向到日志文件 | 方便后续查看日志 |

| `2>&1` | 错误输出也重定向到同一文件 | 所有日志集中在一个文件 |

| `&` | 后台运行 | 不阻塞当前终端 |

输入启动命令：

等待两秒后，查看启动日志，确认关键信息：

```
tail -20 /tmp/ollama.log
```

确认服务就绪——用 `curl` 测试 API：

```
curl http://127.0.0.1:11434/api/version
```

#### 日志关键信息解读

| 关键信息 | 日志内容 | 含义 | 是否正常 |

| --- | --- | --- | --- |

| 监听端口 | `Listening on 127.0.0.1:11434` | API 服务已启动 | ✅ |

| GPU 检测 | `NVIDIA A800-SXM4-80GB` | 正确检测到 GPU | ✅ |

| 可用显存 | `available="74.7 GiB"` | ~75GB 可用 | ✅ |

| 存储路径 | `OLLAMA_MODELS:/root/autodl-tmp/ollama-models` | 模型存到数据盘 | ✅ |

全部确认通过，Ollama 服务已就绪！

---

### 3.6 认识 Qwen3.5 模型家族

在下载模型之前，先了解一下我们要用的模型——知道"为什么选它"比"怎么装它"更重要。

#### 为什么选 Qwen3.5？

Qwen3.5 是阿里通义实验室推出的最新一代开源大模型，三个核心优势：

| # | 优势 | 详细说明 | 对 Agent 的意义 |

| --- | --- | --- | --- |

| 1 | 中文能力一流 | 阿里团队专门优化中文理解和生成 | Agent 能准确理解中文指令 |

| 2 | 原生工具调用 | 内置 function calling 能力 | Agent 能调用工具"干活"（刚需） |

| 3 | 256K 超长上下文 | 能一次性处理约 20 万字 | 长对话不会"失忆" |

#### 模型选型表：选哪个版本？

Qwen3.5 有多个尺寸，从 0.8B 到 122B。我们需要根据 A800 80GB 的显存来选择：

| 模型 | 参数量 | 量化方式 | 下载大小 | 显存需求 | A800 80GB 适配 | 推荐指数 |

| --- | --- | --- | --- | --- | --- | --- |

| `qwen3.5:0.8b` | 0.8B | `Q4_K_M` | 1.0 GB | ~2 GB | 太小，能力不足 | ⭐ |

| `qwen3.5:9b` | 9B | `Q4_K_M` | 6.6 GB | ~10 GB | 适合对比测试 | ⭐⭐⭐ |

| `qwen3.5:27b` | 27B | `Q4_K_M` | 17 GB | ~20 GB | 课程首选 | ⭐⭐⭐⭐⭐ |

| `qwen3.5:27b-q8_0` | 27B | `Q8_0` | 30 GB | ~34 GB | 更高精度 | ⭐⭐⭐⭐ |

| `qwen3.5:27b-bf16` | 27B | 无（全精度） | 56 GB | ~60 GB | 满血版 | ⭐⭐⭐⭐ |

怎么看这张表：从上到下，模型越来越大、越来越强、也越"吃"显存。我们选第三行 `qwen3.5:27b`——在能力和显存之间取得最佳平衡：27B 参数带来优秀的推理能力，`Q4_K_M` 量化后只需 20GB 显存，A800 轻松驾驭。

#### 什么是量化？一分钟搞懂

你可能注意到了 `Q4_K_M`、`Q8_0`、`BF16` 这些术语。这涉及一个概念：量化（Quantization）。

通俗解释：

+ 原始模型中，每个参数用 `BF16`（Brain Float 16）格式存储，占 2 字节

+ 27B 参数 × 2 字节 = 54GB——这就是满血版的大小

+ 量化就是降低精度：把每个参数从 2 字节压缩到约 0.5 字节（4-bit）

+ 量化后：27B × 0.5 字节 ≈ 17GB——体积缩小到三分之一

类比：就像把图片从 RAW 格式（无损，几十 MB）压缩成 JPEG（有损，几百 KB）。文件小了很多，但肉眼几乎看不出区别。

| 量化等级 | 精度 | 大小比例 | 质量损失 | 适用场景 |

| --- | --- | --- | --- | --- |

| `BF16`（满血） | 16-bit | 100% | 无 | 追求最佳质量 |

| `Q8_0` | 8-bit | ~55% | 极小 | 质量优先 |

| `Q4_K_M` | 4-bit | ~31% | 小 | 性能/质量最佳平衡 |

| `Q2_K` | 2-bit | ~18% | 明显 | 极度受限环境 |

> 实用建议：日常使用选 `Q4_K_M` 就够了，回答质量和满血版几乎无法区分。如果你对精度有极致追求，Part 4 会教你用 vLLM 跑全精度 `BF16` 版本。

---

### 3.7 下载 Qwen3.5 模型

#### 下载主力模型（27B）

只需一行命令：

```
ollama pull qwen3.5:27b
```

| 参数 | 含义 | 说明 |

| --- | --- | --- |

| `ollama pull` | 下载模型 | 类似 Docker 的 `docker pull` |

| `qwen3.5:27b` | 模型名:标签 | `qwen3.5` 是系列名，`27b` 是 27B 参数的 `Q4_K_M` 量化版 |

输入下载命令：

下载过程中会显示实时进度条（总大小约 17GB，通常 5-10 分钟）：

下载完成后，自动验证文件完整性：

关键输出解读：

| 输出 | 含义 |

| --- | --- |

| `pulling f0b1e06263b1: 100%` | 17GB 模型权重文件下载完成 |

| `verifying sha256 digest` | 正在校验文件完整性（确保下载没有损坏） |

| `success` | 下载并验证成功 |

#### 查看已下载模型

```
ollama list

du -sh /root/autodl-tmp/ollama-models/
```

| 模型 | 大小 | 用途 |

| --- | --- | --- |

| `qwen3.5:27b` | 17 GB | 课程主力模型 |

| `qwen3.5:9b`（可选） | 6.6 GB | 对比测试用 |

---

### 3.8 首次运行模型 — CLI 对话测试

模型下载完了，激动人心的时刻到了——让我们跟 270 亿参数的大模型聊聊天！

#### 启动交互式对话



## 3.8 Ollama 启动与显存检查

```
# 启动 Qwen3.5-27B 的交互式对话
# 出现 >>> 提示符后即可输入问题
ollama run qwen3.5:27b
```

输入命令：

出现 `>>>` 提示符后，输入测试问题：

模型回复——这是你自己服务器上的 GPU 计算出来的，数据没有离开你的机房：

> 输入 `/bye` 可以退出对话模式。

### 查看显存占用

在另一个终端运行 `nvidia-smi`：

显存分析：

| 进程 | 显存占用 | 说明 |
| --- | --- | --- |
| `python3`（TTS 服务） | ~4.5 GB | 之前的 TTS 服务 |
| `ollama`（Qwen3.5-27B） | ~40.7 GB | 包含模型权重 + KV 缓存 |
| 合计 | ~45.2 GB | |
| 剩余 | ~35 GB | 还有富余空间 |

> 深入理解：你可能注意到 Ollama 占了 ~41GB 显存，但模型文件只有 17GB。这是因为推理时需要额外的 KV 缓存（Key-Value Cache）来存储对话上下文——模型需要"记住"之前说了什么才能生成连贯的回复。这是正常现象。

---

## 3.9 API 接口测试

### 为什么要测 API？

前面的 `ollama run` 是人与模型一对一聊天。但 OpenClaw 不是这样工作的——它是通过 HTTP API 调用模型的：发一个 HTTP 请求，收到一个 JSON 回复。

所以我们需要验证 Ollama 的 API 接口是否正常。

### curl 测试

```
# 通过 API 调用 Qwen3.5-27B（非流式模式）
curl http://127.0.0.1:11434/api/chat -d '{
  "model": "qwen3.5:27b",
  "messages": [{"role": "user", "content": "你好，用一句话介绍你自己"}],
  "stream": false
}'
```

每个字段的含义：

| 字段 | 值 | 说明 |
| --- | --- | --- |
| URL | `http://127.0.0.1:11434/api/chat` | Ollama API 地址：本机 11434 端口，`/api/chat` 聊天接口 |
| `-d` | POST 请求体 | 发送 JSON 格式的请求数据 |
| `model` | `"qwen3.5:27b"` | 指定使用哪个模型 |
| `messages` | 消息数组 | `role`: `"user"` 表示用户消息 |
| `stream` | `false` | 不用流式返回，等生成完毕一次性返回 |

输入 API 调用命令：

等待模型响应（约 20-30 秒）：

收到 JSON 回复：

### 返回值关键字段

| 字段 | 含义 | 示例值 |
| --- | --- | --- |
| `model` | 使用的模型 | `qwen3.5:27b` |
| `message.role` | 回复角色 | `assistant` |
| `message.content` | 回复文本 | 模型的回答内容 |
| `done` | 是否生成完毕 | `true` |
| `eval_count` | 生成的 token 数 | 970 |
| `eval_duration` | 生成耗时（纳秒） | 约 25 秒 |

> 性能参考：270 亿参数模型在 A800 上约 39 tokens/s，相当于人类快速打字速度的 5-6 倍。

---

## 3.10 OpenClaw 对接 Ollama

现在 Ollama 和 Qwen3.5 都跑起来了，接下来是最关键的一步：让 OpenClaw 知道要用 Ollama 作为大脑。

### 编辑配置文件

OpenClaw 的所有配置都在 `~/.openclaw/openclaw.json` 中（JSON5 格式，支持注释）。

我们需要写入以下完整配置：

```
{
  // 网关配置
  gateway: { mode: "local" },

  // 模型配置
  models: {
    providers: {
      ollama: {
        baseUrl: "http://127.0.0.1:11434",  // Ollama API 地址
        apiKey: "ollama-local",              // 本地不需要真实 Key，随便填
        api: "ollama",                       // ★ 必须用 Ollama 原生 API 模式
        models: [{
          id: "qwen3.5:27b",               // 模型 ID（与 ollama list 一致）
          name: "Qwen3.5 27B",             // 显示名称
          reasoning: false,                 // Ollama 对接时关闭思考模式
          input: ["text"],                  // 输入类型
          cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },
          contextWindow: 262144,            // 256K 上下文
          maxTokens: 32768                  // 最大输出 32K
        }]
      }
    }
  },

  // Agent 配置
  agents: {
    defaults: {
      model: { primary: "ollama/qwen3.5:27b" }  // 默认模型
    }
  }
}
```

### 配置项逐段解释

`models.providers.ollama` 段：

| 配置项 | 值 | 含义 | 注意事项 |
| --- | --- | --- | --- |
| `baseUrl` | `http://127.0.0.1:11434` | Ollama API 地址 | 端口 11434 是 Ollama 默认端口 |
| `apiKey` | `"ollama-local"` | API 密钥 | 本地不需要认证，但字段不能为空 |
| `api` | `"ollama"` | API 模式 | ⚠️ 必须用原生模式，见下方说明 |
| `models[0].id` | `"qwen3.5:27b"` | 模型 ID | 必须与 `ollama list` 中名称完全一致 |
| `contextWindow` | `262144` | 上下文窗口 | 256K tokens |
| `cost` | 全部为 `0` | 费用 | 本地模型零成本 |

`agents` 段：

| 配置项 | 值 | 含义 |
| --- | --- | --- |
| `model.primary` | `"ollama/qwen3.5:27b"` | 默认模型，格式为 provider名/模型id |

### 三个重要提醒

> ⚠️ 提醒 1：`api` 必须用 `"ollama"`，不要用 `"openai-completions"`
>
> Ollama 同时支持原生 API 和 OpenAI 兼容 API。这里必须用 `ollama`（原生模式）。
>
> **原因**：OpenAI 兼容模式下，Ollama 的工具调用（`function calling`）不稳定——Agent 调用工具时会莫名其妙断掉。原生模式经过 OpenClaw 团队专门适配，工具调用完全可靠。
>
> 这个坑有多隐蔽：用 OpenAI 兼容模式时，普通对话完全正常，只有在 Agent 尝试调用工具（比如执行 `nvidia-smi`）时才会出问题——你可能测了半天才发现。

> ⚠️ 提醒 2：`models` 字段必须是数组
>
> `models` 的值是 `[{...}]`（数组），不能写成 `{...}`（对象）。

> ⚠️ 提醒 3：不能用 `openclaw config set` 逐项设置
>
> 如果你尝试 `openclaw config set models.providers.ollama.baseUrl xxx` 逐项设置，会报错。必须直接编辑 JSON 文件，一次性写入完整配置。

---

## 3.11 启动 OpenClaw Gateway

### Gateway 是什么？

如果 Ollama 是"大脑"，那 Gateway 就是"神经中枢"——连接大脑和外界的桥梁。

| 职责 | 说明 |
| --- | --- |
| 接收消息 | 来自 Dashboard、飞书等所有渠道 |
| 调度 Agent | 把消息交给 Agent，Agent 调用模型思考后回复 |
| 管理会话 | 记住对话上下文，实现连续对话 |
| 工具调度 | Agent 需要执行命令时，Gateway 负责调度 |

### 启动 Gateway

```
# 用 screen 在后台启动 Gateway
# screen 让你可以随时查看日志，比 nohup 更方便
screen -dmS openclaw bash -c 'source ~/.bashrc && openclaw gateway'
```

| 部分 | 含义 |
| --- | --- |
| `screen -dmS openclaw` | 创建名为 openclaw 的后台 screen 会话 |
| `bash -c '...'` | 在会话中执行命令 |
| `source ~/.bashrc` | 加载环境变量（确保 `nvm`、`Node.js` 可用） |
| `openclaw gateway` | 启动 Gateway |

输入启动命令：

> 为什么用 `screen` 而不是 `nohup`？
>
> Ollama 用 `nohup` 是因为它的日志不太需要实时看。Gateway 不同——它的日志对排查问题非常有用。用 `screen` 可以随时 `screen -r openclaw` 进去看日志，看完 Ctrl+A D 退出。

等待 3-5 秒后检查状态：

```
# 查看 Gateway 运行状态
openclaw gateway status
```

| 信息 | 值 | 含义 |
| --- | --- | --- |
| Address | `ws://127.0.0.1:18789` | Gateway 的 WebSocket 地址 |
| Status | `Running` | 正在运行 |

> ⚠️ 如果启动失败：检查 18789 端口是否被占用 `lsof -i :18789`，有旧进程就 `kill <PID>` 后重试。

---

## 3.12 SSH 端口转发 — 在本地浏览器访问远程 Dashboard

### 为什么需要端口转发？

Gateway 监听在 127.0.0.1:18789——只接受服务器本地连接。你的浏览器无法直接访问。

SSH 端口转发在你的电脑和服务器之间建一条"隧道"：

```
你的电脑浏览器                           AutoDL 服务器
┌────────────────┐                   ┌────────────────┐
│ localhost:18789 │══ SSH 隧道 ══════>│ 127.0.0.1:18789│
│ （你的电脑）    │                   │ （Gateway）     │
└────────────────┘                   └────────────────┘
```

### 建立 SSH 隧道

在你的电脑上（不是服务器上！），新开一个终端窗口：

```
# 在本地电脑执行（不是服务器上！）
# 将本地 18789 端口转发到远程 18789 端口
ssh -p 40903 -L 18789:127.0.0.1:18789 root@connect.bjb1.seetacloud.com
```

| 参数 | 含义 |
| --- | --- |
| `-p 40903` | SSH 端口（替换为你的） |
| `-L 18789:127.0.0.1:18789` | 本地 18789 → 远程 18789 |

连接成功后不要关闭这个终端——它就是隧道。

> ⚠️ 如果本地 18789 被占用，改用其他端口：`-L 28789:127.0.0.1:18789`，然后浏览器访问 `http://localhost:28789`。

### 获取 Gateway Token

```
# 在 AutoDL 服务器上运行——获取带认证 Token 的 URL
openclaw dashboard --no-open
```

复制输出的 URL 中 `token=` 后面那串字符。

---

## 3.13 Dashboard 实战 — 完整体验

### 登录 Dashboard

在浏览器中打开 `http://localhost:18789`，你会看到登录页面：

| 字段 | 填什么 | 说明 |
| --- | --- | --- |
| WebSocket URL | 保持默认 `ws://localhost:18789` | 如果用了其他端口就改 |
| Gateway Token | 粘贴刚才获取的 Token | 认证令牌 |
| `Password` | 留空 | 可选 |

输入 `Token` 后点击 Connect：

### 进入主界面

连接成功！注意顶部模型名称——Default (`qwen3.5:27b`)——就是你刚部署的本地模型。收到回复：

回复区域信息解读：

| 信息 | 示例 | 含义 |
| --- | --- | --- |
| Token 统计 | ↑15.7k ↓31 | 输入 15.7K token，生成 31 token |
| 上下文 | 6% ctx | 用了 6% 上下文窗口，还有 94% |
| 模型标识 | `qwen3.5:27b` | 确认用的是本地模型 |

---

## 3.14 实战演示：让 Agent "干活"

### 测试工具调用

在 Dashboard 中输入：

> "请在服务器上运行 `nvidia-smi` 命令，告诉我 GPU 的使用情况"

Agent 会：

1. 理解意图 → 你想看 GPU 信息
1. 调用工具 → 执行 `exec` 工具运行命令
1. 返回结果 → 用自然语言汇报 GPU 状态

点击展开查看原始输出：

这不是模拟——Agent 真的在服务器上执行了命令！

### 更多测试场景

| 你说 | Agent 会做什么 | 调用的工具 |
| --- | --- | --- |
| "帮我查看磁盘使用情况" | 执行 `df -h` | `exec` |
| "在 `/root/autodl-tmp` 创建 `hello.py`" | 创建文件并写入代码 | `write` |
| "帮我查 Ollama 下了哪些模型" | 执行 `ollama list` | `exec` |
| "读一下 `openclaw.json` 的内容" | 读取配置文件 | `read` |

> 深入理解：这一刻你应该能感受到 AI Agent 和普通聊天机器人的本质区别——它不只是说，还能做。你说一句话，它自己拆解任务、调用工具、执行命令、返回结果。这才是 Agent 的真正魅力。

---

## 3.15 Dashboard 管理功能速览

### Overview（概览页）

| 指标 | 值 | 含义 |
| --- | --- | --- |
| `STATUS` | `OK`（绿色） | Gateway 正常 |
| `COST` | `$0.00` | 本地模型零费用 |
| `SESSIONS` | `1` | 1 个活跃会话 |
| `SKILLS` | `52/52` | 52 个技能全部就绪 |

> 延伸思考：注意 `COST` `$0.00`——不管你问多少问题，费用始终为零。后面对接云端 API 时会看到费用开始跳动。

### Skills（技能页）

OpenClaw 出厂自带 52 个内置技能，包括文件读写、命令执行、网页浏览等。还有 ClawHub 技能市场的 13,700+ 社区技能可以一键安装。

---

## 本章小结

| 步骤 | 完成情况 | 关键结果 |
| --- | --- | --- |
| `zstd` 安装 | ✅ | Ollama 前置依赖 |
| Ollama 安装 | ✅ | `v0.18.1` |
| 模型存储配置 | ✅ | 指向数据盘 |
| Ollama 启动 | ✅ | GPU 检测正常，75GB 可用 |
| `Qwen3.5:27b` 下载 | ✅ | 17GB，Q4_K_M 量化 |
| CLI 对话 | ✅ | 模型正常回复 |
| API 测试 | ✅ | ~39 tokens/s |
| OpenClaw 对接 | ✅ | 原生 Ollama API 模式 |
| Gateway 启动 | ✅ | `ws://127.0.0.1:18789` |



# 验证总结与后续规划

## 已验证功能

| `SSH` 端口转发 | ✅ | 本地浏览器可访问 |

| `Dashboard` 全流程 | ✅ | 登录、对话、工具调用 |

## 当前状态与局限

恭喜！你已经拥有了一个 **能干活的 AI 智能体** ——不仅能聊天，还能在服务器上执行命令、读写文件。

但 `Ollama` 方案在高并发场景下有限（同时 1-2 个请求）。下一章，我们来看工业级推理引擎 `vLLM`——同样的模型，并发数从 1-2 跃升到 19。

> `实用建议`：在进入 `Part 4` 之前，花 5 分钟在 `Dashboard` 里多试几个指令，感受一下 `Agent` 的能力边界。这个体验会帮你更好地理解下一章 `vLLM` 方案的价值。
