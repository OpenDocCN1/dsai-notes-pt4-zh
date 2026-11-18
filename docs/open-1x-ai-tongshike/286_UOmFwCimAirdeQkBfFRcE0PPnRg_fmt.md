
# Gradio实践笔记

> 来源：[https://datawhaler.feishu.cn/docx/CsMadTsCJobptBxTUNMclGXmnOd](https://datawhaler.feishu.cn/docx/CsMadTsCJobptBxTUNMclGXmnOd)

## Gradio 实现 OpenAI 流式问答机器人教程

本文将介绍如何使用`Gradio`和`OpenAI API`来实现一个流式问答机器人。通过这个教程，你将学会如何构建一个可以处理文本输入并返回连续响应的聊天机器人。

## 环境准备

在开始之前，确保你已经安装了必要的Python库。你可以使用以下命令安装`Gradio`和`OpenAI`库：

```bash
pip install gradio openai
```

## 代码结构

我们将代码分为几个部分进行详细解读：

1. 导入必要的库
2. 定义消息添加函数
3. 定义机器人响应函数
4. 创建`Gradio`界面

### 1. 导入必要的库

首先，我们需要导入`Gradio`和`OpenAI`库，以及操作系统相关的库。

```python
import os
import gradio as gr
from openai import OpenAI
```

### 2. 定义消息添加函数

`add_message`函数用于将用户的输入消息添加到聊天记录中。

```python
def add_message(history, message):
    for x in message["files"]:
        history.append(((x,), None))
    if message["text"] is not None:
        history.append((message["text"], None))
    return history, gr.MultimodalTextbox(value=None, interactive=False)
```

函数参数：

+ `history`：聊天记录。
+ `message`：用户输入的消息，是一个字符串。

函数功能：

+ 将文件或文本消息添加到聊天记录中。
+ 返回更新后的聊天记录和一个新的空的输入框。

### 3. 定义机器人响应函数

`bot`函数用于处理用户输入，并通过`OpenAI API`生成机器人响应。

```python
def bot(history):
    history[-1][1] = ""

    history_openai_format = []
    for human, assistant in history[:-1]:
        history_openai_format.append({"role": "user", "content": human})
        history_openai_format.append({"role": "assistant", "content": assistant})
    history_openai_format.append({"role": "user", "content": history[-1][0]})

    client = OpenAI(
        api_key=os.getenv("DASHSCOPE_API_KEY"),
        base_url="https://dashscope.aliyuncs.com/compatible-mode/v1",
    )
    completion = client.chat.completions.create(
        model="qwen-turbo",
        messages=history_openai_format,
        temperature=0.1,
        stream=True,
    )
    for chunk in completion:
        history[-1][1] += chunk.choices[0].delta.content
        yield history
```

函数参数：

+ `history`：聊天记录。

函数功能：

+ 从聊天记录中获取最新的用户输入（`prompt`）。
+ 拼接历史消息`history`，这是一个二维数组，每条消息包含用户输入和LLM输出。
+ 使用`OpenAI API`创建一个聊天完成请求。
+ 设置API请求的模型和消息内容。
+ 逐步接收API响应，并将内容追加到最新的消息响应中。
+ 使用`yield`逐步更新聊天记录，实现流式响应。

> 为方便对接国内大模型，我们使用了`DashScope API`代替`OpenAI API`，只需要改写`api_key`、`base_url`和`model`即可使用。

### 4. 创建Gradio界面

接下来，我们使用`Gradio`创建一个用户界面来展示聊天机器人。

```python
with gr.Blocks() as demo:
    chatbot = gr.Chatbot([], elem_id="chatbot", bubble_full_width=False)

    chat_input = gr.MultimodalTextbox(
        interactive=True,
        file_types=[],
        placeholder="Enter message or upload file...",
        show_label=False,
    )

    chat_msg = chat_input.submit(
        add_message, [chatbot, chat_input], [chatbot, chat_input]
    )
    bot_msg = chat_msg.then(bot, chatbot, chatbot, api_name="bot_response")
    bot_msg.then(lambda: gr.MultimodalTextbox(interactive=True), None, [chat_input])

demo.queue()
demo.launch()
```

步骤说明：

+ 创建一个`gr.Blocks`实例作为整个应用的容器。
+ 创建一个`Chatbot`组件来显示聊天内容。
+ 创建一个`MultimodalTextbox`组件作为用户的输入框。
+ 当用户提交消息时，调用`add_message`函数更新聊天记录。
+ 使用`then`方法链式调用`bot`函数处理并生成机器人响应。
+ 重新启用输入框供用户继续输入。
+ 最后，通过`launch()`启动`Gradio`应用。

## 完整代码

```python
import os

import gradio as gr
from openai import OpenAI


def add_message(history, message):
    for x in message["files"]:
        history.append(((x,), None))
    if message["text"] is not None:
        history.append((message["text"], None))
    return history, gr.MultimodalTextbox(value=None, interactive=False)


def bot(history):
    history[-1][1] = ""

    history_openai_format = []
    for human, assistant in history[:-1]:
        history_openai_format.append({"role": "user", "content": human})
        history_openai_format.append({"role": "assistant", "content": assistant})
    history_openai_format.append({"role": "user", "content": history[-1][0]})

    client = OpenAI(
        api_key=os.getenv("DASHSCOPE_API_KEY"),
        base_url="https://dashscope.aliyuncs.com/compatible-mode/v1",
    )
    completion = client.chat.completions.create(
        model="qwen-turbo",
        messages=history_openai_format,
        temperature=0.1,
        stream=True,
    )
    for chunk in completion:
        history[-1][1] += chunk.choices[0].delta.content
        yield history


with gr.Blocks() as demo:
    chatbot = gr.Chatbot([], elem_id="chatbot", bubble_full_width=False)

    chat_input = gr.MultimodalTextbox(
        interactive=True,
        file_types=[],
        placeholder="Enter message or upload file...",
        show_label=False,
    )

    chat_msg = chat_input.submit(
        add_message, [chatbot, chat_input], [chatbot, chat_input]
    )
    bot_msg = chat_msg.then(bot, chatbot, chatbot, api_name="bot_response")
    bot_msg.then(lambda: gr.MultimodalTextbox(interactive=True), None, [chat_input])

demo.queue()
demo.launch()
```

## 效果展示

可以看到，LLM响应是流式输出的，并且LLM在处理第二个问题“里面有哪些教程”时，理解了用户想要问的上下文是“Datawhale里面有哪些教程”。

![](img/9660bfbe5668325634779bc18d3d8e72.png)

[Screen Recording 2024-06-11 at 11.00.43.mov](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/LkMSbG3THoinaBxaDEncL7Ppnxg)

## 总结

通过上述步骤，我们成功地实现了一个基于`Gradio`和`OpenAI`的流式问答机器人。这个教程展示了如何处理用户输入并使用`OpenAI API`生成连续的响应，希望对你有所帮助。

现在，你可以根据自己的需求进一步定制和扩展这个聊天机器人，例如添加更多的对话逻辑或支持更多类型的输入。

## 参考资料

+ https://www.gradio.app/docs/gradio/chatbot
+ https://www.gradio.app/guides/creating-a-custom-chatbot-with-blocks
+ https://www.gradio.app/guides/creating-a-chatbot-fast#a-streaming-example-using-openai
+ https://help.aliyun.com/zh/dashscope/developer-reference/compatibility-of-openai-with-dashscope/
