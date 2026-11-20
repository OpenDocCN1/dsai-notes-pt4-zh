
- **Google**：在 Google Cloud Console 中设置 API 配额和预算提醒

`OpenClaw` 本身可以通过 `CLI` 查看模型状态：

```
openclaw models status           # 查看当前模型状态
openclaw models list             # 列出所有可用模型
```

## Token 限制

不同模型有不同的上下文窗口和输出限制：

| 模型 | 上下文窗口 | 最大输出 |
| --- | --- | --- |
| `GPT-5.2` | 128K tokens | 32,768 tokens |
| `Claude Sonnet 4.6` | 200K tokens | 64,000 tokens |
| `Gemini 3.1 Pro` | 1M tokens | 65,536 tokens |

> `OpenClaw` 会自动处理上下文窗口限制，你不需要手动管理。如果对话太长，`OpenClaw` 会自动压缩或截断历史消息。

## 省钱技巧

1. 简单任务用便宜模型（`GPT-5.2-mini`、`Haiku`）

2. 设置合理的 `maxTokens`，避免模型输出过长

3. 用 `OpenRouter` 自动选择最便宜的提供商

4. 高频任务考虑用本地模型（`Ollama`）

5. 开启用量追踪，定期检查费用

---

## 自定义模型接入

> ⏭️ **小白可跳过** — 这部分面向高级用户，新手用默认配置就够了

如果你有自己部署的模型服务，只要兼容 `OpenAI API` 格式，就能接入 `OpenClaw`。

### 兼容 `OpenAI API` 的服务

很多本地推理框架都兼容 `OpenAI API` 格式（比如 `vLLM`、`text-generation-webui`、`LocalAI` 等）。配置方法：

```
{
  models: {
    providers: {
      custom: {
        baseUrl: "http://your-server:8000/v1",
        apiKey: "your-key-if-needed",
        // api: "openai-chat",  // API 协议类型
        models: [
          {
            id: "my-custom-model",
            name: "My Custom Model",
            contextWindow: 32768,
            maxTokens: 4096,
          },
        ],
      },
    },
  },
}
```

然后就可以用了：

```
openclaw models set "custom/my-custom-model"
```

### 多个自定义服务

可以配置多个自定义提供商，只需用不同的名字：

```
{
  models: {
    providers: {
      "gpu-server-1": {
        baseUrl: "http://192.168.1.100:8000/v1",
        api: "openai-chat",  // API 协议类型
      },
      "gpu-server-2": {
        baseUrl: "http://192.168.1.101:8000/v1",
        api: "openai-chat",
      },
    },
  },
}
```

---

## 常见配置问题排查

### 问题：`API Key` 无效

```
Error: Invalid API key provided
```

确认 `Key` 没有多余的空格或换行，没有过期或被撤销，用的是正确提供商的 `Key`。用 `openclaw models status` 测试。

### 问题：连接超时

```
Error: Request timed out
```

检查网络连接。在中国大陆可能需要代理（`export HTTPS_PROXY="http://127.0.0.1:7890"`）。

### 问题：模型不存在

```
Error: Model not found
```

确认模型 `ID` 拼写正确，确认你的账号有权限使用该模型（有些模型需要单独申请）。用 `openclaw models list` 查看可用模型。

### 问题：`Ollama` 连接失败

```
Error: Connection refused to http://127.0.0.1:11434
```

**排查步骤：**

```
# 1. 确认 `Ollama` 正在运行
ollama list

# 2. 如果没运行，启动它
ollama serve

# 3. 如果是远程 `Ollama`，确认防火墙允许 11434 端口
```

### 问题：本地模型太慢

1. 用更小的模型（`7B` 比 `70B` 快很多）

2. 如果有 `NVIDIA GPU`，确认 `Ollama` 在用 `GPU`（`ollama ps` 查看）

3. 减少 `maxTokens` 限制输出长度

4. 考虑用 `vLLM` 替代 `Ollama`（吞吐量更高）

### 问题：费用超出预期

1. 开启用量追踪（在 `~/.openclaw/openclaw.json` 中配置 `usageTracking`）

2. 设置每日限额

3. 检查是否有 `Agent` 在用昂贵的模型做简单任务

4. 把高频任务切换到便宜模型

---

## 下一步

模型配好了！去 `05. 消息平台集成` 连接你的聊天软件！
