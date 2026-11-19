
# 国产模型 `OpenAI` 兼容 API 速查表

## 05-model-guide

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/XhRId9IZboCHWcxisc6cCyhynle](https://kq4b3vgg5b.feishu.cn/docx/XhRId9IZboCHWcxisc6cCyhynle)

> 配套视频：《用 AI 编程工具接管一个完全陌生的开源项目》

> 作者：木羽Cheney （VibeCoding 实战课主讲老师）· 赋范大模型技术社区

> 更新日期：2026 年 4 月

> 用法：接入国产模型时，查对应行的 `Base URL` 和模型名，复制粘贴即可

---

## 使用方式

几乎所有国产大模型都兼容 `OpenAI` API 格式。你只需要改三个字段：

```
# 以 Python OpenAI SDK 为例
from openai import OpenAI

client = OpenAI(
    base_url="[从下方表格查]",     # ← 改这里
    api_key="[你的 API Key]",      # ← 改这里
)

response = client.chat.completions.create(
    model="[从下方表格查]",         # ← 改这里
    messages=[{"role": "user", "content": "你好"}]
)
```

如果你的项目用的是 `.env` 文件配置，通常是这样：

```
LLM_BASE_URL=https://dashscope.aliyuncs.com/compatible-mode/v1
LLM_API_KEY=sk-xxxxxxxxxxxxxxxx
LLM_MODEL=qwen-plus
```

---

## 一、主流模型速查表

### 通义千问（阿里云百炼）

| 字段 | 值 |
| --- | --- |
| 提供商 | 阿里云 |
| `Base URL` | https://dashscope.aliyuncs.com/compatible-mode/v1 |
| 获取 `API Key` | [百炼控制台](https://bailian.console.aliyun.com/) → 创建 `API Key` |
| 免费额度 | 新用户 7000 万 token（涵盖多个模型），有效期 3 个月 |

| 模型名 | 能力定位 | 上下文长度 | 价格（每百万 token） | 推荐场景 |
| --- | --- | --- | --- | --- |
| `qwen-plus` | 性价比之选 | 128K | 输入 0.8 元 / 输出 2 元 | **首选推荐**：日常问答、代码辅助、项目接管验证 |
| `qwen-max` | 旗舰级 | 128K | 输入 2.5 元 / 输出 10 元 | 复杂推理、长篇创作、高质量分析 |
| `qwen-turbo` | 速度优先 | 128K | 输入 0.3 元 / 输出 0.6 元 | 高并发、对速度要求高的场景 |
| `qwen-long` | 超长文本 | 1M | 输入 0.5 元 / 输出 2 元 | 整本书分析、超长对话 |
| `qwen3.5-plus` | 最新一代 | 128K | 输入 1 元 / 输出 4 元 | 多模态（支持图片+视频输入） |

> **新手推荐**：先用 `qwen-plus`，免费额度大、性价比高、能力够用。

---

### DeepSeek

| 字段 | 值 |
| --- | --- |
| 提供商 | 深度求索 |
| `Base URL` | https://api.deepseek.com/v1 |
| 获取 `API Key` | DeepSeek Platform → `API Keys` |
| 免费额度 | 注册送 10 元（约 500 万 token），⚠️ 高峰期可能暂停注册 |

| 模型名 | 能力定位 | 上下文长度 | 价格（每百万 token） | 推荐场景 |
| --- | --- | --- | --- | --- |
| `deepseek-chat` | 通用对话（`V3.2`） | 128K | 输入 2 元 / 输出 8 元 | 日常问答、代码、创作 |
| `deepseek-reasoner` | 深度推理（`R1`） | 128K | 输入 4 元 / 输出 16 元 | 数学、逻辑推理、复杂分析 |

> **优势**：编程能力极强，接近 `GPT-5` 水平；价格便宜。

> 注意：高峰期（工作日白天）可能限流，建议非高峰时段使用，或通过百炼平台间接调用。

---

### 智谱 GLM

| 字段 | 值 |
| --- | --- |
| 提供商 | 智谱 AI |
| `Base URL` | https://open.bigmodel.cn/api/paas/v4 |
| 获取 `API Key` | [智谱开放平台](https://open.bigmodel.cn/) → 个人中心 → `API Keys` |
| 免费额度 | 新用户 2000 万 token（30 天有效）+ `GLM-4.7-Flash` 永久免费 |

| 模型名 | 能力定位 | 上下文长度 | 价格（每百万 token） | 推荐场景 |
| --- | --- | --- | --- | --- |
| `glm-4.7-flash` | **完全免费** | 200K | 免费 | **零成本首选**：验证项目可用性、日常测试 |
| `glm-5-turbo` | Agent 优化 | 128K | 输入 1 元 / 输出 2 元 | Agent 工作流、工具调用 |
| `glm-5` | 旗舰级 | 128K | 输入 5 元 / 输出 10 元 | 复杂编程、长篇分析 |
| `glm-5.1` | 最强能力 | 128K | 输入 10 元 / 输出 20 元 | 最高质量需求 |

> **零成本方案**：`glm-4.7-flash` 完全免费且支持 200K 上下文，非常适合新手验证项目可用性。

---

### 月之暗面 Kimi

| 字段 | 值 |
| --- | --- |
| 提供商 | Moonshot AI |
| `Base URL` | https://api.moonshot.cn/v1 |
| 获取 `API Key` | [Moonshot 开放平台](https://platform.moonshot.cn/) → `API Key` 管理 |
| 免费额度 | 注册送 15 元 |

| 模型名 | 能力定位 | 上下文长度 | 价格（每百万 token） | 推荐场景 |
| --- | --- | --- | --- | --- |
| `moonshot-v1-8k` | 短对话 | 8K | 输入 1 元 / 输出 2 元 | 简单问答 |
| `moonshot-v1-32k` | 中等长度 | 32K | 输入 2 元 / 输出 4 元 | 文档阅读 |
| `moonshot-v1-128k` | 超长文本 | 128K | 输入 6 元 / 输出 12 元 | 长文本分析、论文阅读 |
| `kimi-2.5` | 最新旗舰 | 128K | 输入 2 元 / 输出 8 元 | 通用、推理 |

> **特长**：超长文本理解能力优秀，适合需要阅读大量文档的项目。

---

### 豆包（火山方舟）

| 字段 | 值 |
| --- | --- |
| 提供商 | 字节跳动 |
| `Base URL` | https://ark.cn-beijing.volces.com/api/v3 |
| 获取 `API Key` | [火山方舟控制台](https://console.volcengine.com/ark/) → 创建 `API Key` |
| 免费额度 | 新用户 50 万 token |

| 模型名 | 能力定位 | 上下文长度 | 价格（每百万 token） | 推荐场景 |
| --- | --- | --- | --- | --- |
| ⚠️ 需在控制台创建"接入点" | — | — | — | — |

> **特别说明**：豆包的模型名不是固定字符串，需要在火山方舟控制台创建"在线推理接入点"，获得形如 `ep-20240922143915-xxxxxx` 的接入点 ID 作为 `model` 字段。新手不推荐作为第一选择，配置步骤比其他平台多。

---

### MiniMax

| 字段 | 值 |
| --- | --- |
| 提供商 | `MiniMax` |
| `Base URL` | https://api.minimax.chat/v1 |
| 获取 `API Key` | [MiniMax 开放平台](https://platform.minimax.chat/) → `API Keys` |
| 免费额度 | 注册送额度 |

| 模型名 | 能力定位 | 上下文长度 | 价格（每百万 token） | 推荐场景 |
| --- | --- | --- | --- | --- |
| `MiniMax-Text-01` | 长文本 | 1M | 输入 1 元 / 输出 5 元 | 超长文本处理 |
| `MiniMax-M2.5` | 通用旗舰 | 256K | 输入 1.1 元 / 输出 4.4 元 | 通用对话、创作 |

> ⚠️ **域名注意**：`MiniMax` 中国站（`api.minimax.chat`）和国际站（`api.minimax.io`）的 `API Key` 不互认。确保你注册的平台和使用的 `Base URL` 匹配。

---

## 二、聚合平台（一个 Key 调多个模型）

如果你不想注册多个平台，可以用聚合平台：

### 阿里云百炼（强烈推荐）

| 字段 | 值 |
| --- | --- |
| `Base URL` | https://dashscope.aliyuncs.com/compatible-mode/v1 |
| 支持的模型 | 千问全系 + `DeepSeek` 全系 + `Kimi` + `MiniMax` + `GLM` |
| 免费额度 | 每个模型各 100 万 token，有效期 3 个月 |

**优势**：一个 `API Key` 调几乎所有主流国产模型，只需切换 `model` 字段。

**用法示例**：

```
# 用百炼调 DeepSeek
client = OpenAI(
    base_url="https://dashscope.aliyuncs.com/compatible-mode/v1",
    api_key="sk-百炼的Key"
)
response = client.chat.completions.create(
    model="deepseek-v3",  # 百炼平台上的模型名
    messages=[...]
)
```

### OpenRouter（国际聚合）

| 字段 | 值 |
| --- | --- |
| `Base URL` | https://openrouter.ai/api/v1 |
| 支持的模型 | `OpenAI` + `Claude` + `Gemini` + `DeepSeek` + 几乎所有模型 |
| 免费额度 | 部分模型免费（带 `:free` 后缀） |

> 适合需要切换不同厂商模型的场景，但延迟可能比直连稍高。

---

## 三、选型决策树

不知道该选哪个？按这个路径走：

```
你的首要需求是什么？
  │
  ├── 零成本验证 ──→ 智谱 glm-4.7-flash（永久免费，200K 上下文）
  │
  ├── 性价比最高 ──→ 通义千问 qwen-plus（7000 万免费 token）
  │
  ├── 编程能力最强 ──→ DeepSeek deepseek-chat（接近 GPT-5）
  │
  ├── 超长文本 ──→ Kimi moonshot-v1-128k 或 MiniMax-Text-01
  │
  ├── 一个平台搞定 ──→ 阿里云百炼（一个 Key 调所有模型）
  │
  └── 不想折腾 ──→ 通义千问 qwen-plus（注册简单+额度大+能力够）
```

---

## 四、常见配置问题速查

| 症状 | 可能原因 | 解决方法 |
| --- | --- | --- |
| `401 Unauthorized` | `API Key` 错误 | 去对应平台重新复制 `Key` |
| `401` 且 `Key` 确认正确 | 域名和 `Key` 不匹配 | 确认 `Base URL` 和注册平台是同一个（中国站/国际站） |
| `404 Not Found` | 模型名拼写错误 | 对照上方表格检查 `model` 字段 |
| `404` 且模型名正确 | `Base URL` 缺少路径 | 确认 `URL` 以 `/v1` 结尾 |
| `429 Too Many Requests` | 免费额度用完或并发太高 | 等一会儿或充值或换平台 |
| `400 Bad Request` | 请求参数不兼容 | 检查是否用了模型不支持的参数（如 `tools`、`response_format`） |
| 响应慢（>20s） | 模型过大或网络问题 | 换 `turbo`/`flash` 级别的小模型 |
| 中文乱码 | 编码问题 | 确认请求和响应的编码都是 UTF-8 |

---

## 五、API Key 安全提醒

| 规则 | 说明 |
| --- | --- |
| ❌ 不要提交到 Git | 把 `.env` 加入 `.gitignore` |
| ❌ 不要发到聊天群 | 别人拿你的 `Key` 用，消耗的是你的额度 |
| ❌ 不要硬编码在代码里 | 用环境变量或配置文件读取 |
| ✅ 用 `.env` 文件管理 | 本地开发标准做法 |
| ✅ 定期轮换 `Key` | 泄露了第一时间去控制台重新生成 |
| ✅ 设置用量告警 | 大多数平台支持设置消费上限 |

---

## 六、快速测试命令

拿到 `API Key` 后，用这条 `curl` 命令快速测试是否能通（替换三个占位符）：

```
curl -X POST [Base_URL]/chat/completions \
  -H "Authorization: Bearer [API_Key]" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "[模型名]",
    "messages": [{"role": "user", "content": "请用一句话介绍自己"}],
    "max_tokens": 100
  }'
```

**示例（通义千问）**：

```
curl -X POST https://dashscope.aliyuncs.com/compatible-mode/v1/chat/completions \
  -H "Authorization: Bearer sk-你的百炼Key" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "qwen-plus",
    "messages": [{"role": "user", "content": "请用一句话介绍自己"}],
    "max_tokens": 100
  }'
```

如果返回了 JSON 格式的回复，说明配置正确，可以放心接入项目了。

---

> **数据来源**：

+   [阿里云百炼模型价格](https://help.aliyun.com/zh/model-studio/model-pricing)

+   [DeepSeek API 定价](https://api-docs.deepseek.com/quick_start/pricing)

+   [智谱 AI 开放平台](https://open.bigmodel.cn/pricing)

+   2026 大模型 API 大测评

+   [百炼免费额度汇总](https://zhuanlan.zhihu.com/p/2011551559968896157)

---

更多 AI 实战教程，关注赋范大模型技术社区。

![](img/bc9fb234672a72970d45b893ce4a4089.png)
