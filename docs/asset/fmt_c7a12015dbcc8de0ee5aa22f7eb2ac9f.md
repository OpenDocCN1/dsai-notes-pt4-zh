
## Gradio 应用组件说明

`yield partial_message`：这是实现 Gradio 流式输出的关键。函数不再是一次性 `return` 完整结果，而是通过 `yield` 逐步返回部分生成的文本。Gradio 会捕捉这些 `yield` 的值并动态更新聊天界面，给用户带来实时的打字机效果。

错误处理：函数内包含了对常见 API 错误的捕获（认证、速率限制、API 返回错误）以及通用异常处理，并将错误信息通过 `yield` 返回给 Gradio 界面显示。

1. `gr.Chatbot(...)`：创建了一个聊天气泡样式的显示组件，通过 `type='messages'` 指定了其内部处理历史记录的格式。

1. `gr.ChatInterface(...)`：这是 Gradio 提供的一个高级便利组件，它封装了创建完整聊天界面所需的各种元素：

+ 自动使用我们定义的 `predict` 函数作为后端逻辑（`fn=predict`）。

+ 自动管理聊天历史的显示（使用我们传入的 `chatbot_component`）。

+ 自动提供文本输入框（`textbox`）。

+ 自动添加发送、重试、撤销、清除等常用按钮。

+ 可以通过 `title`，`description`，`examples` 等参数定制界面外观和行为。

1. `chatbot_interface.launch(...)`：这行代码启动一个本地的 Web 服务器来运行我们的 Gradio 应用。`server_name="0.0.0.0"` 参数使得在同一局域网内的其他设备也可以通过运行脚本的机器的 IP 地址来访问这个聊天机器人（如果防火墙允许的话）。

## 如何运行

1. 保存代码：将上述完整代码保存为 Python 文件，例如 `AOAI_gradio_app.py`。

1. 设置环境变量：再次强调，在运行脚本的终端（或环境中）必须先设置好您的 AOAI API Key 环境变量。例如：

+ Linux/macOS: `export AOAI_API_KEY='sk-xxxxxxxxxxxxxxxxxxxx'`

+ Windows CMD: `set AOAI_API_KEY=sk-xxxxxxxxxxxxxxxxxxxx`

+ Windows PowerShell: `$env:AOAI_API_KEY='sk-xxxxxxxxxxxxxxxxxxxx'`

1. 运行脚本：在设置好环境变量的终端中，执行 `python AOAI_gradio_app.py`。

1. 访问应用：终端会输出类似 `Running on local URL: http://127.0.0.1:7860` 和 `Running on public URL: ...`（如果使用了 `share=True`，或类似 `Running on http://0.0.0.0:7860` 如果使用了 `server_name="0.0.0.0"`）的信息。

1. 打开浏览器：在您的浏览器中访问提供的本地 URL（通常是 `http://127.0.0.1:7860` 或 `http://<你的IP地址>:7860`）。

1. 开始聊天：您现在应该能看到一个功能完善的聊天界面，可以开始与 AOAI 模型进行对话了！

![](img/087f603a40d61bb420e9d3e14ef174a8.png)

## 安全提示

+ 这个示例假设您在本地运行，并通过环境变量管理 API Key。

+ 如果您打算将此类应用部署到公共服务器或与他人分享，绝对不能将 API Key 硬编码在代码中。需要采用更安全的密钥管理策略，例如使用服务器端环境变量、Secrets Manager 服务或专门的配置文件（并确保该文件不被公开访问）。

> 如果您希望从互联网上临时访问您在本地运行的 Gradio 应用（例如，为了方便演示或与他人短暂分享），最关键的修改是在 `launch()` 方法中添加 `share=True` 参数。

> 找到代码末尾的启动部分，修改 `launch()` 方法如下：

```
# --- 启动 Gradio 应用 ---
if __name__ == "__main__":
    # ... (之前的检查 API Key 的代码保持不变) ...

    # *** 关键修改：添加 share=True ***
    chatbot_interface.launch(
        server_name="0.0.0.0", # 保持或移除皆可，share=True 主要依赖Gradio的隧道
        share=True             # <--- 添加此参数
    )
    # 如果只想本机和公开访问，可以省略 server_name
    # chatbot_interface.launch(share=True)
```

> ### 说明

1. `share=True`：当您添加这个参数后，Gradio 会尝试通过其内置的隧道服务（通常利用类似 frp 或 ngrok 的技术）创建一个临时的、公开可访问的 URL。

1. 输出：运行脚本后，除了本地 URL（`http://127.0.0.1:7860` 或 `http://0.0.0.0:7860`），终端还会输出一个公网 URL（Public URL），通常看起来像 `https://<随机字符>.gradio.live` 或类似的域名。

1. 访问：任何人只要知道这个公网 URL，就可以在浏览器中访问您的 Gradio 应用，无论他们在世界的哪个地方。

> ### ⚠️ 重要警告和注意事项

1. 安全性：这个生成的公网 URL 是完全公开的！任何知道该链接的人都可以访问您的应用。这意味着他们可以与您的聊天机器人交互，并消耗您的 AOAI API Key 的额度。请极其谨慎地分享这个链接，并且只分享给您信任的人。

1. 临时性：这个公网链接通常是临时的（Gradio 官方说明一般是 72 小时有效），并且在您停止本地运行的 Python 脚本后就会失效。它不适用于永久托管应用。

1. 性能：通过隧道访问可能会比本地访问稍慢一些。

1. API Key 暴露风险（间接）：虽然代码本身不暴露 Key，但任何能访问应用的人都在使用您的 Key。如果应用本身有任何（不太可能的）漏洞被利用，或者被恶意大量使用，都可能带来风险或产生高额费用。绝对不要在需要公开部署的应用中直接硬编码或以不安全方式加载 API Key。对于需要长期公开访问的应用，应考虑部署到云平台并实施更严格的安全措施（如用户认证、更安全的密钥管理等）。

## 总结

这个 Gradio 应用演示了如何将我们之前学习的 AOAI API 调用知识（特别是使用 `openai` 库、处理 `messages` 历史、实现流式输出和错误处理）集成到一个交互式的 Web 应用中。这为您将来构建更复杂的基于 LLM 的应用打下了基础。

## 解析 API 响应与错误处理（基于 `openai` 库）

### 解析成功响应

### 非流式（`stream=False`）

+ 模型回复：`response.choices[0].message.content`

+ 结束原因：`response.choices[0].finish_reason`（`'stop'`，`'length'` 等）

+ Token 用量：`response.usage`（包含 `prompt_tokens`，`completion_tokens`，`total_tokens`）

### 流式（`stream=True`）

+ 通过迭代 `for chunk in stream_response:` 获取数据块。

+ 增量内容：`chunk.choices[0].delta.content`（需要检查非 `None`）。

+ 结束原因：在某个 `chunk` 中 `chunk.choices[0].finish_reason` 会变为非 `None`。

+ Token 用量：通常不易在流式迭代中直接获取完整 usage。

### 错误处理

`openai` 库会根据 HTTP 状态码和响应内容抛出特定的异常，方便捕获：

+ `openai.AuthenticationError`：API Key 无效或权限问题（对应 401/403）。

+ `openai.RateLimitError`：超出速率限制（对应 429）。

+ `openai.BadRequestError`：请求参数错误（对应 400）。

+ `openai.APIConnectionError`：网络连接问题。

+ `openai.APITimeoutError`：请求超时。

+ `openai.APIError` / `openai.APIStatusError`：其他 API 层面错误（如 5xx 服务器错误）。

+ 通用捕获：可以使用 `except openai.APIError as e:` 或更通用的 `except Exception as e:` 来捕获所有潜在错误，并通过 `e` 对象获取错误信息。

### 推荐的错误处理结构

```
from openai import OpenAI, APIError, RateLimitError, AuthenticationError # 导入特定错误类型

# ... client 初始化 ...

try:
    # ... 调用 `client.chat.completions.create(...)` ...
    pass
except AuthenticationError as e:
    print(f"认证错误: API Key 可能无效或过期。 {e}")
except RateLimitError as e:
    print(f"速率限制错误: 请求过于频繁，请稍后重试。 {e}")
    # 可以考虑在这里加入等待和重试逻辑
except APIError as e: # 捕获其他 API 相关错误 (4xx, 5xx)
    print(f"API 错误: {e.status_code} - {e.message}")
except Exception as e: # 捕获其他意外错误 (如网络问题)
    print(f"发生意外错误: {e}")
```

# 强制输出 JSON 格式（JSON Mode）

## 引言

在很多应用场景中，我们不仅仅希望模型返回自然的文本回复，更希望它能按照我们预定义的结构输出数据，例如 JSON 格式。这对于需要将模型输出直接用于程序处理、API 对接或数据存储的场景至关重要。AOAI API（与 OpenAI API 兼容）提供了“JSON Mode”功能，可以强制模型输出严格符合 JSON 语法规范的文本。

## 工作原理

要启用 JSON Mode，您需要做两件事：

1. 在 API 请求中指定参数：在调用 `client.chat.completions.create` 时，添加 `response_format={"type": "json_object"}` 参数。

1. 在提示（Prompt）中明确指示：您必须在发送给模型的 `messages` 中（通常是在 `system` 消息或最后一条 `user` 消息里）明确指示模型“必须输出 JSON 格式”或类似的要求，并且最好能描述期望的 JSON 结构（包含哪些字段）。

如果缺少其中任何一步，模型可能无法保证输出有效的 JSON。

## 编程实践：使用 Python 和 `openai` 库获取 JSON 输出

让我们来创建一个有趣的例子：请求模型想象一种生活在云端的、以彩虹为食的奇特小精灵，并用 JSON 格式描述它的关键特征。
