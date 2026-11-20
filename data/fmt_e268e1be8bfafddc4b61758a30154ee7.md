
```
[User IP]  ::ffff:172.17.0.1
[Time]  8/18/2023, 3:06:27 AM
[Auth] admin did not provide an api key
[Proxy]  v1/chat/completions?path=v1&path=chat&path=completions
[Base Url] http://aistudio:8000
[OpenAI Route] params  { path: [ 'dashboard', 'billing', 'usage' ] }
[Auth] allowed hashed codes:  []
[Auth] got access code:
[Auth] hashed access code: d41d8cd98f00b204e9800998ecf8427e
```

从这段日志信息来看，这是一个API请求的日志，其中包含了用户IP、时间、鉴权信息、请求的URL路径等信息。下面是对这些信息的简单解读：

1. **User IP** `::ffff:172.17.0.1`: 这是用户的IP地址，`::ffff:172.17.0.1`是一个IPv6的地址，实际上表示的是IPv4地址`172.17.0.1`。

2. **Time** `8/18/2023, 3:06:27 AM`: 这是请求发生的时间。

3. **Auth** `admin did not provide an api key`: 这表明用户（在这种情况下是管理员）没有提供API密钥进行鉴权。

4. **Proxy** `v1/chat/completions?path=v1&path=chat&path=completions`: 这是被代理的请求路径。

5. **Base Url** `http://aistudio:8000`: 这是服务器的基础URL。

6. **OpenAI Route** `params { path: ['dashboard', 'billing', 'usage'] }`: 这是请求的OpenAI路由参数。

7. **Auth** `allowed hashed codes: []`: 这是允许的哈希码列表，看起来这个列表是空的。

8. **Auth** `got access code:`: 这表明获取到了访问码，但是看起来访问码是空的。

9. **Auth** `hashed access code: d41d8cd98f00b204e9800998ecf8427e`: 这是获取到的访问码的哈希值，但由于原始的访问码是空的，所以这个哈希值实际上是空字符串的MD5哈希值。

总的来说，这个日志显示了一个API请求的详细信息，但是看起来这个请求没有提供API密钥，也没有提供访问码，可能无法通过鉴权。

在模型容器这边可以看到日志

```
2023-08-18T03:05:07.580954421Z INFO:     172.17.0.1:32930 - "POST /v1/chat/completions?path=v1&path=chat&path=completions HTTP/1.1" 200 OK

2023-08-18T03:06:08.489583122Z INFO:     172.17.0.1:32932 - "POST /v1/chat/completions?path=v1&path=chat&path=completions HTTP/1.1" 200 OK

2023-08-18T03:06:31.109598989Z INFO:     172.17.0.1:32934 - "POST /v1/chat/completions?path=v1&path=chat&path=completions HTTP/1.1" 200 OK
```

这条日志信息是HTTP服务器日志的一部分，它提供了关于特定HTTP请求的信息。下面是对这条日志的解读：

+ `2023-08-18T03:06:31.109598989Z`: 这是请求发生的时间，使用的是ISO 8601的日期和时间表示法，包括了日期、时间和时区（Z表示UTC时区）。

+ `INFO`: 这是日志的级别，表示这是一条信息级别的日志，通常用于描述正常的运行情况。

+ `172.17.0.1:32934`: 这是发起请求的客户端的IP地址和端口号。

+ `"POST /v1/chat/completions?path=v1&path=chat&path=completions HTTP/1.1"`: 这是请求的详细信息，包括了HTTP的方法（POST）、请求的路径（`/v1/chat/completions`）以及路径参数（`path=v1&path=chat&path=completions`）和HTTP的版本（HTTP/1.1）。

+ `200 OK`: 这是服务器对请求的响应状态码和状态消息，200表示请求已成功处理，OK是对应的状态消息。

总的来说，这条日志告诉我们在特定的时间，来自特定IP地址和端口的客户端向服务器发起了一个POST请求，请求的路径是`/v1/chat/completions`，路径参数是`path=v1&path=chat&path=completions`，服务器已经成功处理了这个请求并返回了200 OK的响应。

正式部署可以使用以下docker-compose文件

```yaml
version: '3.1'
services:
  chatgpt-next-web:
    image: yidadaa/chatgpt-next-web
    restart: always
    environment:
      BASE_URL: http://aistudio:8000
    ports:
      - 3000:3000
```

另一个可用项目 chatbot-ui：`https://github.com/mckaywrigley/chatbot-ui`

```
docker run -e OPENAI_API_HOST=http://192.168.14.111:8000 -e OPENAI_API_KEY=sk-XXXXXXXXXXXXXXXXXXXX -p 3000:3000 ghcr.io/mckaywrigley/chatbot-ui:main
```

# Day 6 （2023-8-5）运行 ChatGLM2-6B 项目并进行微调

ChatGLM模型就不用专门介绍了，可能后续一段时间这个是最适合在国内场景用的LLM大模型，所以我测试使用容器环境运行它并进行微调，后续还会测试使用图形界面进行微调和启用流失会话API。当然这些功能后续也会在LLaMa模型上进行复现。

## 下载模型

```
git lfs install
git clone https://huggingface.co/THUDM/chatglm2-6b
```

```
chengzh@AIStudio:/mnt/c/BKF$ git clone https://huggingface.co/THUDM/chatglm2-6b
Cloning into 'chatglm2-6b'...
remote: Enumerating objects: 172, done.
remote: Counting objects: 100% (73/73), done.
remote: Compressing objects: 100% (58/58), done.
remote: Total 172 (delta 46), reused 15 (delta 15), pack-reused 99
Receiving objects: 100% (172/172), 1.94 MiB | 718.00 KiB/s, done.
Resolving deltas: 100% (84/84), done.
Filtering content: 100% (8/8), 11.63 GiB | 9.45 MiB/s, done.
```

总大小24G

```
chengzh@AIStudio:/mnt/c/BKF/chatglm2-6b$ du -sh .
24G     .
```

## 运行容器

暂时使用`llama2:base`映像

```
docker run --gpus all --ipc=host --ulimit memlock=-1 --ulimit stack=67108864 --rm -it -v `pwd`/chatglm2-6b:/app/chatglm2-6b -p 7860:7860 soulteary/llama2:base
```

在容器上下文里 clone `chatglm2-6b` 项目

```
 git clone https://github.com/THUDM/ChatGLM2-6B
 CD ChatGLM2-6B
```

安装依赖环境

```
 pip install -r requirements.txt
```

修改`web_demo.py`

```
nano web_demo.py
```

修改 6、7行的模型名称为`/app/chatglm2-6b`

108行修改为 `launch(server_name="0.0.0.0", share=False, inbrowser=True)`

```python
from transformers import AutoModel, AutoTokenizer
import gradio as gr
import mdtex2html
from utils import load_model_on_gpus

tokenizer = AutoTokenizer.from_pretrained("/app/chatglm2-6b", trust_remote_code=True) # 修改模型路径
model = AutoModel.from_pretrained("/app/chatglm2-6b", trust_remote_code=True).cuda() # 修改模型路径
# 多显卡支持，使用下面两行代替上面一行，将num_gpus改为你实际的显卡数量
# from utils import load_model_on_gpus
# model = load_model_on_gpus("THUDM/chatglm2-6b", num_gpus=2)
model = model.eval()

"""Override Chatbot.postprocess"""


def postprocess(self, y):
    if y is None:
        return []
    for i, (message, response) in enumerate(y):
        y[i] = (
            None if message is None else mdtex2html.convert((message)),
            None if response is None else mdtex2html.convert(response),
        )
    return y


gr.Chatbot.postprocess = postprocess


def parse_text(text):
    """copy from https://github.com/GaiZhenbiao/ChuanhuChatGPT/"""
    lines = text.split("\n")
    lines = [line for line in lines if line != ""]
    count = 0
    for i, line in enumerate(lines):
        if "```" in line:

            count += 1

            items = line.split('`')

            if count % 2 == 1:

                lines[i] = f'<pre><code class="language-{items[-1]}">'

            else:

                lines[i] = f'<br></code></pre>'

        else:

            if i > 0:

                if count % 2 == 1:

                    line = line.replace("`", "\`")

                    line = line.replace("<", "&lt;")

                    line = line.replace(">", "&gt;")

                    line = line.replace(" ", "&nbsp;")

                    line = line.replace("*", "&ast;")

                    line = line.replace("_", "&lowbar;")

                    line = line.replace("-", "&#45;")

                    line = line.replace(".", "&#46;")

                    line = line.replace("!", "&#33;")

                    line = line.replace("(", "&#40;")

                    line = line.replace(")", "&#41;")

                    line = line.replace("$", "&#36;")

                lines[i] = "<br>" + line

    text = "".join(lines)

    return text

def predict(input, chatbot, max_length, top_p, temperature, history, past_key_values):

    chatbot.append((parse_text(input), ""))

    for response, history, past_key_values in model.stream_chat(tokenizer, input, history, past_key_values=past_key_values,

                                                                return_past_key_values=True,

                                                                max_length=max_length, top_p=top_p,

                                                                temperature=temperature):

        chatbot[-1] = (parse_text(input), parse_text(response))

        yield chatbot, history, past_key_values

def reset_user_input():

    return gr.update(value='')

def reset_state():

    return [], [], None

with gr.Blocks() as demo:

    gr.HTML("""<h1 align="center">ChatGLM2-6B</h1>""")

    chatbot = gr.Chatbot()

    with gr.Row():

        with gr.Column(scale=4):

            with gr.Column(scale=12):

                user_input = gr.Textbox(show_label=False, placeholder="Input...", lines=10).style(
```
