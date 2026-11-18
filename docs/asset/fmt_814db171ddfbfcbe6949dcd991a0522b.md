
# 第一步：确认 Gateway 在运行
openclaw status

# 第二步：确认 Bot Token 正确
openclaw config get channels.telegram.botToken
# 去 @BotFather 核对 Token 是否一致

# 第三步：确认 Bot 的隐私模式
# 在 @BotFather 中：
# /mybots → 选择你的 Bot → Bot Settings → Group Privacy → Turn off
# 关闭隐私模式后，Bot 才能接收群组中的所有消息

# 第四步：检查日志
openclaw logs --limit 50 | grep telegram

# 第五步：重新连接
openclaw channels logout telegram
openclaw channels login telegram

### Q18: Telegram Bot 在群组中不回复

**现象：** Bot 在私聊中正常回复，但在群组中不响应。

**原因：** Telegram Bot 默认开启隐私模式，在群组中只能收到 `@提及` 和 `/命令`。

**解决方案：**

1. 在 `@BotFather` 中关闭隐私模式（见 Q17）
2. 或者在群组中 `@提及` Bot 来触发回复
3. 在群组配置中设置激活模式（通过聊天命令 `/activation mention` 或 `/activation always` 切换）

### Q19: Discord Bot 无法加入服务器

**现象：** 用邀请链接添加 Bot 时报错 `Missing Permissions`。

**原因：** Bot 的 OAuth2 权限配置不完整。

**解决方案：**

1. 去 Discord Developer Portal
2. 选择你的应用 → OAuth2 → URL Generator
3. 勾选以下权限：

   - `bot`
   - `applications.commands`
   - `Send Messages`
   - `Read Message History`
   - `Embed Links`
   - `Attach Files`

4. 用生成的新链接重新邀请 Bot

```
# 配置 Discord Bot Token（注意：Discord 用的字段是 token，不是 botToken）
openclaw config set channels.discord.token "your-discord-bot-token"
openclaw channels logout discord
openclaw channels login discord
```

### Q20: 消息延迟严重（发了好久才回复）

**现象：** 发消息后要等 10 秒甚至更久才收到回复。

**原因：** 延迟可能来自多个环节 — 网络、模型 API 响应时间、消息队列积压。

**解决方案：**

```
# 第一步：排查是哪个环节慢
openclaw logs --limit 20
# 看日志中的时间戳，判断延迟发生在哪里

# 第二步：测试模型 API 延迟
openclaw health
# 如果 API 延迟高，考虑换模型或换提供商

# 第三步：检查是否有消息积压
openclaw sessions list

# 第四步：优化措施
# 使用更快的模型

openclaw config set agents.defaults.model "openai/gpt-5.2-mini"
# 减少系统提示词长度（Token 越少，响应越快）
# 精简 ~/.openclaw/workspace/SOUL.md 的内容
```

### Q21: 消息发送失败，提示 "rate limited"

**现象：** 发消息时报错 `429 Too Many Requests` 或 `rate limited`。

**原因：** 消息平台有发送频率限制。WhatsApp 尤其严格，短时间内发太多消息会被限流。

**解决方案：**

```
# 消息平台的限流是平台侧限制，OpenClaw 没有内置速率限制配置
# 解决方案：
# 1. 减少消息发送频率，避免触发平台限流
# 2. 等待限流解除后自动重试
# 3. 如果是 API 限流（429），参考 Q27 配置模型故障转移
```

### Q22: 飞书（Feishu/Lark）接入后收不到消息

**现象：** 飞书 Bot 配置完成，但收不到用户消息。

**原因：** 飞书的事件订阅配置不完整，或者回调地址不可达。

**解决方案：**

```
# 第一步：确认 Gateway 的公网地址可达
curl https://your-domain.com:18789/health

# 第二步：在飞书开放平台配置事件订阅
# 事件订阅 URL：https://your-domain.com:18789/webhook/feishu
# 需要订阅的事件：im.message.receive_v1

# 第三步：检查日志
openclaw logs --limit 50 | grep feishu

# 第四步：确认 Bot 权限
# 飞书开放平台 → 应用权限 → 确保开启了：
# - 获取与发送单聊、群组消息
# - 读取用户信息
```

### Q23: 控制面板（Dashboard）打不开

**现象：** 浏览器访问 `http://localhost:18789` 显示无法连接。

**原因：** Gateway 没有运行，或者端口被防火墙拦截。

**解决方案：**

```
# 第一步：确认 Gateway 在运行
openclaw status

# 如果没运行，启动它
openclaw gateway --port 18789

# 第二步：检查端口
# Linux / macOS
lsof -i :18789
# Windows
netstat -an | findstr 18789

# 第三步：如果是远程服务器，检查防火墙
# Ubuntu
sudo ufw allow 18789
# CentOS
sudo firewall-cmd --add-port=18789/tcp --permanent
sudo firewall-cmd --reload

# 第四步：重启 Gateway（停止后重新启动，或使用聊天命令 /restart）
openclaw gateway --port 18789
```

### Q24: 同一个平台能接入多个账号吗？

**说明：** 可以。OpenClaw 支持同一平台的多个 Channel 实例。

```
# 添加第二个 WhatsApp 账号
openclaw channels add whatsapp --name "work-whatsapp"
openclaw channels add whatsapp --name "personal-whatsapp"

# 查看所有 Channel
openclaw channels list
```

每个 Channel 独立运行，有自己的配对状态和消息队列。

### Q25: 消息中的图片/文件 AI 能处理吗？

**说明：** 取决于你使用的模型。支持多模态的模型（如 `GPT-5.2`、`Claude Sonnet 4.6`）可以处理图片。

```
# 多模态能力取决于你使用的模型，不需要额外配置
# 支持多模态的模型（如 GPT-5.2、Claude Sonnet 4.6）会自动处理图片
# 文件大小限制取决于消息平台本身的限制
```

不支持多模态的模型收到图片时，会提示用户发送文字。

## 三、模型和 AI 问题

本节包含 10 个问题：

Q26: API 401 报错 | Q27: API 429 限流 | Q28: 模型回复慢 | Q29: Token 超限 | Q30: Ollama 连不上 | Q31: 本地模型质量差 | Q32: 国产模型配置 | Q33: 模型故障转移 | Q34: 控制 API 费用 | Q35: 不同对话用不同模型

### Q26: API 调用报错 401 Unauthorized

**现象：** 发消息后 AI 不回复，日志中出现 `401 Unauthorized` 或 `Invalid API Key`。

**原因：** `API Key` 无效、过期、或者余额不足。

**解决方案：**

```
# 第一步：检查 Key 是否配置正确
# 推荐通过环境变量设置：
echo $OPENAI_API_KEY
# 确认 Key 没有多余的空格或换行

# 第二步：测试 Key 是否有效
openclaw health
# 如果报错，说明 Key 有问题

# 第三步：去提供商后台检查
# OpenAI: https://platform.openai.com/account/api-keys
# Anthropic: https://console.anthropic.com/settings/keys
# 确认 Key 没有被撤销，账户余额充足

# 第四步：重新设置 Key
export OPENAI_API_KEY="sk-proj-new-key-here"
# 然后重启 Gateway
```

### Q27: 报错 429 Too Many Requests（API 限流）

**现象：** 高频使用时出现 `429` 错误，AI 间歇性不回复。

**原因：** 超过了模型提供商的 API 调用频率限制（`RPM/TPM`）。

**解决方案：**

```
# 方案一：启用模型故障转移（推荐）
# 在 ~/.openclaw/openclaw.json 中配置：
# {
#   "agents": {
#     "defaults": {
#       "model": {
#         "primary": "openai/gpt-5.2",
#         "fallbacks": ["anthropic/claude-sonnet-4-6", "openai/gpt-5.2-mini"]
#       }
#     }
#   }
# }

# 方案二：使用 OpenRouter（聚合多个提供商，限流更宽松）
export OPENROUTER_API_KEY="sk-or-xxxxx"
openclaw config set agents.defaults.model "openrouter/openai/gpt-5.2"

# 方案三：升级 API 账户等级（更高的 RPM/TPM 限制）
# 去各提供商控制台升级
```

### Q28: 模型回复很慢

**现象：** 发送消息后，AI 要等 10-30 秒甚至更久才开始回复。在终端日志中可以看到请求已发出，但响应迟迟不返回。

**原因：** 大模型本身推理就需要时间，加上网络延迟，响应时间会更长。

**解决方案：**

```
# 方案一：换用更快的小模型
openclaw config set agents.defaults.model "openai/gpt-5.2-mini"
# gpt-5.2-mini 速度是 gpt-5.2 的 3-5 倍，日常对话足够用

# 方案二：流式响应默认已启用
# OpenClaw 默认使用流式输出（边生成边发送），无需额外配置

# 方案三：减少系统提示词长度
# 精简 `SOUL.md` 中不必要的指令，精简 `MEMORY.md` 内容
openclaw agents list
# 查看当前 Agent 列表

# 方案四：使用本地模型（延迟最低）
ollama pull llama3.1:8b
openclaw config set models.providers.ollama.baseUrl "http://127.0.0.1:11434"
openclaw config set agents.defaults.model "ollama/llama3.1:8b"

# 方案五：检查网络延迟
ping api.openai.com
# 如果延迟高，考虑使用代理或换区域更近的提供商
```

### Q29: Token 超限报错（context length exceeded）

**现象：** 对话到一定长度后报错 `maximum context length exceeded` 或 `token limit`。

**原因：** 每个模型都有上下文窗口限制。对话历史 + 系统提示 + 记忆文件的总 `Token` 数超过了模型限制。

**解决方案：**

```
# 查看当前会话的 Token 使用情况
openclaw sessions list

# 方案一：使用 `/compact` 命令手动压缩上下文
# 在聊天界面中发送 `/compact`，OpenClaw 会自动压缩旧消息
# 也可以配置自动压缩阈值：
# 在 ~/.openclaw/openclaw.json 中设置：
# {
#   "agents": {
#     "defaults": {
#       "compaction": {
#         "reserveTokensFloor": 20000
#       }
#     }
#   }
# }
# reserveTokensFloor 越大，压缩越激进

# 方案二：使用上下文窗口更大的模型
# Claude Sonnet 4.6: 200K tokens
# Gemini 3.1 Pro: 1M tokens
# GPT-5.2: 128K tokens
openclaw config set agents.defaults.model "anthropic/claude-sonnet-4-6"

# 方案三：手动清理会话
openclaw sessions cleanup
# 或让 AI 总结后开新会话（在聊天界面发送 `/new`）
```

### Q30: Ollama 本地模型连不上

**现象：** 配置了 Ollama 但 OpenClaw 报错 `ECONNREFUSED` 或 `connection refused`。

**原因：** Ollama 服务没有启动，或者监听地址配置不对。

**解决方案：**

```
# 第一步：确认 Ollama 在运行
ollama serve
# 或者检查是否已经在后台运行
curl http://127.0.0.1:11434/api/tags

# 第二步：确认有可用的模型
ollama list
# 如果没有模型，先拉一个
ollama pull llama3.1:8b

# 第三步：配置 OpenClaw 连接 Ollama
openclaw config set models.providers.ollama.baseUrl "http://127.0.0.1:11434"
openclaw config set agents.defaults.model "ollama/llama3.1:8b"

# 第四步：如果 Ollama 和 OpenClaw 不在同一台机器
# 需要让 Ollama 监听所有网卡
OLLAMA_HOST=0.0.0.0 ollama serve
# 然后配置远程地址
openclaw config set models.providers.ollama.baseUrl "http://192.168.1.100:11434"
```

### Q31: 本地模型回复质量差

**现象：** 用 Ollama 跑本地模型，回复经常答非所问或质量很低。

**原因：** 小参数模型的能力有限，尤其是中文能力。

**解决方案：**

```
# 方案一：换用更大的模型（需要更多内存/显存）
ollama pull llama3.1:70b    # 需要 64GB+ 内存
ollama pull qwen2.5:14b     # 中文能力更好

# 方案二：调整模型参数（在 API 调用时传入，非配置文件）
# `temperature` 和 `topP` 等参数取决于模型提供商
# 本地模型可以在 Ollama 的 `Modelfile` 中设置默认参数
# 例如创建自定义 Modelfile：
# FROM llama3.1:8b
# PARAMETER temperature 0.7
# PARAMETER top_p 0.9

# 方案三：优化系统提示词
# 本地小模型对提示词更敏感，要写得简洁明确
# 在 `SOUL.md` 中精简指令，避免复杂的多步骤指令

# 方案四：混合策略 — 简单任务用本地模型，复杂任务用云端
# 通过模型故障转移实现：本地模型作为默认，复杂任务手动切换
# 在 ~/.openclaw/openclaw.json 中配置：
# {
#   "agents": {
#     "defaults": {
#       "model": {
#         "primary": "ollama/llama3.1:8b",
#         "fallbacks": ["openai/gpt-5.2"]
#       }
#     }
#   }
# }
```

### Q32: 怎么使用国产模型（通义千问、Kimi、智谱等）？

**解决方案：**
