
# DeepSeek Gradio 应用完整代码

```python
# deepseek_gradio_app.py
# (将上面提供的完整代码粘贴到这里)
import os
import gradio as gr
from openai import OpenAI, APIError, RateLimitError, AuthenticationError
import traceback

# --- 安全地获取 API Key ---
api_key = os.getenv("DEEPSEEK_API_KEY")

# --- 初始化 OpenAI 客户端 ---
client = None
if api_key:
    try:
        client = OpenAI(api_key=api_key, base_url="https://api.deepseek.com/v1")
        print("DeepSeek API 客户端初始化成功。")
    except Exception as e:
        print(f"初始化 OpenAI 客户端时出错: {e}")
        client = None
else:
    print("警告：环境变量 DEEPSEEK_API_KEY 未设置。")
    client = None

# --- 定义 Gradio 的核心处理函数 ---
def predict(message, history):
    if not client:
        yield "**错误**：DeepSeek API 客户端未初始化。请检查 API Key 设置。"
        return

    openai_messages = [{"role": "system", "content": "你是一个乐于助人、知识渊博的AI助手。请用清晰、简洁的语言回答问题。"}]
    openai_messages.extend(history)
    # Gradio ChatInterface 通常会将最新用户消息加入 history, 我们信任它
    # openai_messages.append({"role": "user", "content": message}) # 可能重复，注释掉

    MAX_HISTORY_MESSAGES = 15
    if len(openai_messages) > MAX_HISTORY_MESSAGES:
        openai_messages = [openai_messages[0]] + openai_messages[-(MAX_HISTORY_MESSAGES - 1):]

    print(f"\n--- 向 API 发送的消息 (共 {len(openai_messages)} 条) ---")
    print("----------------------------------------")

    try:
        stream_response = client.chat.completions.create(
            model="deepseek-chat",
            messages=openai_messages,
            stream=True,
            temperature=0.7,
            max_tokens=1024
        )

        partial_message = ""
        print("模型回复 (流式): ", end="")
        for chunk in stream_response:
            if chunk.choices and chunk.choices[0].delta and chunk.choices[0].delta.content:
                content_part = chunk.choices[0].delta.content
                partial_message += content_part
                print(content_part, end="", flush=True)
                yield partial_message # 流式返回给 Gradio

        print("\n--- 流式输出结束 ---")

    except AuthenticationError as e:
        print(f"\n认证错误: {e}")
        yield f"**错误**：认证失败。检查 API Key。"
    except RateLimitError as e:
        print(f"\n速率限制错误: {e}")
        yield f"**错误**：请求过于频繁，请稍后再试。"
    except APIError as e:
        print(f"\nAPI 错误: Status Code={e.status_code}, Response={e.response}")
        yield f"**错误**：API 返回错误 (代码: {e.status_code})。详情: {e.message}"
    except Exception as e:
        print(f"\n发生意外错误: {type(e).__name__} - {e}")
        traceback.print_exc()
        yield f"**发生意外错误**: {type(e).__name__}。请检查服务器日志。"


# --- 创建 Gradio 聊天界面组件 ---
chatbot_component = gr.Chatbot(
    label="DeepSeek 对话窗口",
    height=650,
    type='messages', # 重要：使用 OpenAI 格式
    show_copy_button=True,
    bubble_full_width=False
)

# --- 使用 gr.ChatInterface 创建完整的聊天应用 UI ---
chatbot_interface = gr.ChatInterface(
    fn=predict,
    title="🚀 DeepSeek 智能聊天助手",
    description="你好！我是基于 DeepSeek 大模型的聊天机器人。输入你的问题，开始对话吧！",
    chatbot=chatbot_component,
    examples=[
        ["你好，介绍一下你自己。"],
        ["用 Python 写一个快速排序算法。"],
        ["解释一下什么是“量子纠缠”？"],
        ["给我讲个笑话吧。"]
    ],
    cache_examples=False,
    textbox=gr.Textbox(placeholder="🤔 在这里输入您的问题...", container=False, scale=7)
    # 移除或注释掉 retry_btn, undo_btn, clear_btn, submit_btn 如果它们导致 TypeError
    # retry_btn="🔄 重试",
    # undo_btn="↩️ 撤销",
    # clear_btn="🗑️ 清除对话",
    # submit_btn="发送"
)

# --- 启动 Gradio 应用 ---
if __name__ == "__main__":
    if not client:
        print("\n" + "="*40)
        print("‼️ 严重警告：未能初始化 DeepSeek API 客户端。")
        print("   聊天功能将无法工作！")
        print("   请确保正确设置并导出 DEEPSEEK_API_KEY 环境变量。")
        print("="*40 + "\n")
    else:
        print("\n" + "="*40)
        print("✅ DeepSeek API 客户端已准备就绪。启动 Gradio 应用...")
        print("="*40 + "\n")

    chatbot_interface.launch(server_name="127.0.0.1") # 允许局域网访问     
```

## 代码关键点解析

1. `import gradio as gr`：导入 Gradio 库。

1. `predict(message, history)` 函数：这是 Gradio 应用的核心逻辑。

   + 它接收用户当前输入 `message` 和由 Gradio 自动管理的 `history`（我们配置了 `type='messages'`，所以 `history` 直接是 OpenAI 的消息格式列表）。
   + 构建 `messages`：我们添加了一个系统提示（system role），然后将 `history` 扩展进去，最后（隐式地，由 Gradio 处理）用户的最新 `message` 也会包含在内（体现在 `history` 中）。这保证了模型能理解对话的上下文。
   + 历史截断：添加了简单的逻辑来限制 `messages` 列表的最大长度，防止 API 调用因 Token 过多而失败或费用过高。
   + 调用 API（流式）：使用 `client.chat.completions.create` 并设置 `stream=True`。这与我们在 3.3 节学到的流式调用一致。
   + `yield partial_message`：这是实现 Gradio 流式输出的关键。函数不再是一次性 `return` 完整结果，而是通过 `yield` 逐步返回部分生成的文本。Gradio 会捕捉这些 `yield` 的值并动态更新聊天界面，给用户带来实时的打字机效果。
   + 错误处理：函数内包含了对常见 API 错误的捕获（认证、速率限制、API 返回错误）以及通用异常处理，并将错误信息通过 `yield` 返回给 Gradio 界面显示。

1. `gr.Chatbot(...)`：创建了一个聊天气泡样式的显示组件，通过 `type='messages'` 指定了其内部处理历史记录的格式。

1. `gr.ChatInterface(...)`：这是 Gradio 提供的一个高级便利组件，它封装了创建完整聊天界面所需的各种元素：

   + 自动使用我们定义的 `predict` 函数作为后端逻辑（`fn=predict`）。
   + 自动管理聊天历史的显示（使用我们传入的 `chatbot_component`）。
   + 自动提供文本输入框（`textbox`）。
   + 自动添加发送、重试、撤销、清除等常用按钮。
   + 可以通过 `title`、`description`、`examples` 等参数定制界面外观和行为。

1. `chatbot_interface.launch(...)`：这行代码启动一个本地的 Web 服务器来运行我们的 Gradio 应用。`server_name="0.0.0.0"` 参数使得在同一局域网内的其他设备也可以通过运行脚本的机器的 IP 地址来访问这个聊天机器人（如果防火墙允许的话）。

## 如何运行

1. **保存代码：** 将上述完整代码保存为 Python 文件，例如 `deepseek_gradio_app.py`。

1. **设置环境变量：** 再次强调，在运行脚本的终端（或环境中）必须先设置好您的 DeepSeek `API Key` 环境变量。例如：

   + Linux/macOS: `export DEEPSEEK_API_KEY='sk-xxxxxxxxxxxxxxxxxxxx'`
   + Windows CMD: `set DEEPSEEK_API_KEY=sk-xxxxxxxxxxxxxxxxxxxx`
   + Windows PowerShell: `$env:DEEPSEEK_API_KEY='sk-xxxxxxxxxxxxxxxxxxxx'`

1. **运行脚本：** 在设置好环境变量的终端中，执行 `python deepseek_gradio_app.py`。

1. **访问应用：** 终端会输出类似 `Running on local URL: http://127.0.0.1:7860` 和 `Running on public URL: ...`（如果使用了 `share=True`，或类似 `Running on http://0.0.0.0:7860` 如果使用了 `server_name="0.0.0.0"`）的信息。

1. **打开浏览器：** 在您的浏览器中访问提供的本地 URL（通常是 `http://127.0.0.1:7860` 或 `http://<你的IP地址>:7860`）。

1. **开始聊天：** 您现在应该能看到一个功能完善的聊天界面，可以开始与 DeepSeek 模型进行对话了！

![](img/3635fbde20c7f149b0425798b3e8ef04.png)

## 安全提示

+ 这个示例假设您在本地运行，并通过环境变量管理 API Key。
+ 如果您打算将此类应用部署到公共服务器或与他人分享，绝对不能将 `API Key` 硬编码在代码中。需要采用更安全的密钥管理策略，例如使用服务器端环境变量、Secrets Manager 服务或专门的配置文件（并确保该文件不被公开访问）。

> 如果您希望从互联网上临时访问您在本地运行的 Gradio 应用（例如，为了方便演示或与他人短暂分享），最关键的修改是在 `launch()` 方法中添加 `share=True` 参数。
>
> 找到代码末尾的启动部分，修改 `launch()` 方法如下：

```python
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

### 说明

1. `share=True`：当您添加这个参数后，Gradio 会尝试通过其内置的隧道服务（通常利用类似 frp 或 ngrok 的技术）创建一个临时的、公开可访问的 URL。
1. 输出：运行脚本后，除了本地 URL（`http://127.0.0.1:7860` 或 `http://0.0.0.0:7860`），终端还会输出一个公网 URL（Public URL），通常看起来像 `https://<随机字符>.gradio.live` 或类似的域名。
1. 访问：任何人只要知道这个公网 URL，就可以在浏览器中访问您的 Gradio 应用，无论他们在世界的哪个地方。

### ⚠️ 重要警告和注意事项

1. 安全性：这个生成的公网 URL 是完全公开的！任何知道该链接的人都可以访问您的应用。这意味着他们可以与您的聊天机器人交互，并消耗您的 DeepSeek `API Key` 的额度。请极其谨慎地分享这个链接，并且只分享给您信任的人。
1. 临时性：这个公网链接通常是临时的（Gradio 官方说明一般是 72 小时有效），并且在您停止本地运行的 `Python` 脚本后就会失效。它不适用于永久托管应用。
1. 性能：通过隧道访问可能会比本地访问稍慢一些。
1. `API Key` 暴露风险（间接）：虽然代码本身不暴露 `Key`，但任何能访问应用的人都在使用您的 `Key`。如果应用本身有任何（不太可能的）漏洞被利用，或者被恶意大量使用，都可能带来风险或产生高额费用。绝对不要在需要公开部署的应用中直接硬编码或以不安全方式加载 `API Key`。对于需要长期公开访问的应用，应考虑部署到云平台并实施更严格的安全措施（如用户认证、更安全的密钥管理等）。

## 总结
