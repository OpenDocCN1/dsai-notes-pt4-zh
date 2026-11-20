
# 如何调用OpenAI API Key

> 来源：[https://zsyhjtnsa5.feishu.cn/docx/LJvMdyMDQoCheuxl18jc3kTbn0a](https://zsyhjtnsa5.feishu.cn/docx/LJvMdyMDQoCheuxl18jc3kTbn0a)

## 基准代码

安装 `openai` 包，本实验手册使用是 `1.6.1` 版本

```
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple openai==1.6.1
```

确认 `openai` 版本

```
import pkg_resources

openai_version = pkg_resources.get_distribution("openai").version
print(openai_version)
```

### 基准代码1：流式输出

```
import os
from openai import OpenAI

client = OpenAI(  
    api_key="sk-xxxx",
    base_url="https://freeapi.huaqloud.com/v1"
)

stream = client.chat.completions.create(
    model="gpt-3.5-turbo",
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "证明必达格拉斯定理"}
    ],
    stream=True,
)

for chunk in stream:
    if not chunk.choices or chunk.choices[0].delta.content is None:
        continue

    print(chunk.choices[0].delta.content, end="")
print()
```

> 这段Python脚本使用了OpenAI的API，以生成来自GPT-3.5-turbo模型的回应。用户的消息是中文，要求证明勾股定理（`证明必达格拉斯定理`）。脚本设置为流式响应，意味着它会在生成回应的同时，逐块打印出来。

> 下面是对脚本的简要解释：

1. 导入 `openai` 库，这是OpenAI API的Python客户端。
2. 创建了一个 `OpenAI` 类的实例，并将API密钥和基础URL作为参数传递。
3. 使用 `client.chat.completions.create()` 方法创建了一个聊天完成。这个方法接受一个 `model` 参数，指定使用的模型（在这种情况下，是 `gpt-3.5-turbo`），以及一个 `messages` 参数，这是要发送给模型的消息列表。列表中的每条消息都是一个字典，有两个键：`role`（可以是 `system`、`user` 或 `assistant`）和 `content`（消息的内容）。
4. `stream` 参数设置为 `True`，意味着响应将被流式处理。
5. 然后，脚本进入一个循环，在生成每个块的响应时打印出来。如果一个块没有任何选择或其内容是 `None`，则跳过它。
6. 最后，使用一个空的 `print()` 语句在输出的末尾打印一个换行符。

请注意，你需要用你实际的API密钥替换 `sk-xxxx`，并用OpenAI API的实际基础URL替换 `https://freeapi.huaqloud.com/v1`。

### 基准代码2：交互式应答

```
import os
from openai import OpenAI

client = OpenAI(  
    api_key="sk-xxxx",
    base_url="https://freeapi.huaqloud.com/v1"
)

history = [
    {"role": "system", "content": "You are an intelligent assistant. You always provide well-reasoned answers that are both correct and helpful."},
    {"role": "user", "content": "Hello, introduce yourself to someone opening this program for the first time. Be concise."},
]

while True:
    response = client.chat.completions.create(
        model="gpt-3.5-turbo", 
        messages=history,
        temperature=0.7,
    )
    
    print(response.choices[0].message.content)
    history.append({"role": "assistant", "content": response.choices[0].message.content})
    history.append({"role": "user", "content": input("> ")})
```

> 这段代码使用了OpenAI的GPT-3.5-turbo模型来创建一个简单的聊天助手。下面是对代码中每个部分的详细解释：

1. 首先，你导入了 `os` 和 `openai` 模块。`os` 模块是Python的标准库，用于与操作系统交互。`openai` 模块是OpenAI的Python客户端，用于与OpenAI的API进行交互。
2. 接下来，你创建了一个 `OpenAI` 对象，该对象需要 `api_key` 和 `base_url`。这些信息用于让你的程序与OpenAI的API进行通信。
3. 你定义了一个名为 `history` 的列表，该列表包含一系列消息。每个消息都是一个字典，包含一个“角色”（可以是“系统”、“用户”或“助手”）和一个“内容”（消息的文本）。这个历史记录被用作聊天模型的输入。
4. 然后，你进入了一个无限循环。在每次迭代中，你都会向OpenAI的API发送一个请求，请求一个新的聊天完成。你使用了 `client.chat.completions.create` 方法来完成这个任务。这个方法需要一个模型名（在这个例子中是 `gpt-3.5-turbo`），一个历史消息列表，和一个“温度”参数（这个参数控制生成文本的随机性，值越高生成的文本越随机，值越低生成的文本越确定）。
5. OpenAI的API会返回一个响应，这个响应中包含了模型生成的新消息。你打印了这个新消息的内容。
6. 最后，你将新消息添加到历史记录中，并从用户那里获取新的输入，也将其添加到历史记录中。然后，循环重新开始，使用更新的历史记录生成新的响应。

这个程序将会持续运行，直到你手动停止它。在每次迭代中，它都会打印出模型的响应，并等待用户的输入。

### 注意：为了演示方便，以下所有代码的调试均使用 `Jutpyter Noetbook`，关于在 `vscode` 中使用 `Jupter Notebook` 请参照 https://code.visualstudio.com/docs/datascience/jupyter-notebooks

## 直接调用 OpenAI

### 适用场景：coding的机器肉身在国内，但是机器的灵魂可以随时在国际互联网上遨游

适配直接调用OpenAI的基准代码1：

```
import os
from openai import OpenAI

client = OpenAI(  
    api_key="sk-xxx", # 此处使用openai官方key，怎么获取本文不讨论
    # base_url="https://api.openai.com/v1" # 既然是直接访问官方站点，这个变量可以不用声明
)

stream = client.chat.completions.create(
    model="gpt-3.5-turbo",
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "证明必达格拉斯定理"}
    ],
    stream=True,
)

for chunk in stream:
    if not chunk.choices or chunk.choices[0].delta.content is None:
        continue

    print(chunk.choices[0].delta.content, end="")
print()
```

尝试调用一下官方的 `base_url`

```
import requests
import json

headers = {"Content-Type": "application/json"}
data = {"key1": "value1", "key2": "value2"}

response = requests.get('https://api.openai.com/', headers=headers, data=json.dumps(data))
print(response.text)  # 打印响应内容
```

输出如下：

```
{
    "message": "Welcome to the OpenAI API! Documentation is available at [https://platform.openai.com/docs/api-reference](https://platform.openai.com/docs/api-reference%22)"
}
```

## 使用 API Proxy 中继

### 适用场景：

+ 不能保证随时漫游的， 但是至少有那么一小会可以出去透口气使用某个托管网站创建 API Proxy
+ 有一个域名（要足够的纯净）

### 核心功能代码：

```
export default { async fetch(request) { const url = new URL(request.url); url.host = 'api.openai.com'; return fetch(url, { headers: request.headers, method: request.method, body: request.body }); }, };
```

> 这是一段JavaScript代码，它定义了一个对象并导出（`export default`）。这个对象有一个名为 `fetch` 的异步方法，该方法接收一个请求（`request`）作为参数。

> 以下是每一行代码的解释：

> 1行：`export default` 是ES6模块的语法，它表示默认导出一个对象。
> 2行：定义了一个异步函数（`async fetch(request)`），它接收一个 `request` 参数。这个参数应该是一个请求对象，包含了一个请求的所有信息，如URL、头部、方法、主体等。
> 3行：创建一个新的 `URL` 对象（`new URL(request.url)`）。这个对象的构造函数接收一个字符串参数，表示要解析的URL。然后，这个新创建的 `URL` 对象被赋值给 `url` 变量。
> 4行：将 `url.host` 的值设置为 `'api.openai.com'`。这意味着无论 `request.url` 的主机部分原本是什么，现在都被替换为 `'api.openai.com'`。
> 5行：使用全局的 `fetch` 函数发起一个新的请求。这个函数的参数包括：
> + `url`：请求的URL，已经被修改为 `'api.openai.com'`。
> + 一个对象，包含：
> + `headers`：请求的头部，直接使用了原始请求的头部（`request.headers`）。
> + `method`：请求的方法，如 `"GET"`、`"POST"` 等，直接使用了原始请求的方法（`request.method`）。
> + `body`：请求的主体，直接使用了原始请求的主体（`request.body`）。
> 6-7行：定义对象的结束。

这段代码的主要功能是，接收一个请求，修改其URL的主机部分为 `'api.openai.com'`，然后发起一个新的请求。这在某些情况下是有用的，比如当你需要通过一个代理服务器访问OpenAI的API时。

### 两种实现方法：

1. vercel实现方法：https://github.com/cloudzun/vercel-openai-deploy
2. cloudflare实现方法：https://cloud.tencent.com/developer/article/2304066

这两种方法在实现上最大的区别是，`vercel` 对 `DNS` 配置要求低，`cloudflare` 要求把 `DNS` 域名托管到它的网站上，略有些强势了。

如果不方便自己创建，可以使用某个代理：https://bridge.huaqloud.com/api

查询 `api proxy`

```
import requests
import json

headers = {"Content-Type": "application/json"}
data = {"key1": "value1", "key2": "value2"}

response = requests.get('https://bridge.huaqloud.com/api/', headers=headers, data=json.dumps(data))
print(response.text)  # 打印响应内容
```

输入一般情况下应该是：

```
{
    "message": "Welcome to the OpenAI API! Documentation is available at [https://platform.openai.com/docs/api-reference](https://platform.openai.com/docs/api-reference%22)"
}
```

适配 API Proxy 的基准代码1：

```
import os
from openai import OpenAI

client = OpenAI(  
    api_key="sk-xxx", # 此处使用openai官方key，怎么获取本文不讨论
    base_url="https://bridge.huaqloud.com/api/v1" # 此处使用api proxy地址，注意需要带上/v1
)

stream = client.chat.completions.create(
    model="gpt-3.5-turbo",
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "证明必达格拉斯定理"}
    ],
    stream=True,
)

for chunk in stream:
    if not chunk.choices or chunk.choices[0].delta.content is None:
        continue

    print(chunk.choices[0].delta.content, end="")
print()
```

套壳软件中 `OpenAI Key` 的调用（以 `NetxChat` 为例）

![](img/fb00f8d84f8691a976abfdc16184f266.png)

## 使用 Azure OpenAI

### 适用场景：拥有启用了 Azure OpenAI 服务的 Azure 订阅，在这种情况下对网络没有要求，常规联网就可以。

[官方示例](https://learn.microsoft.com/zh-cn/azure/ai-services/openai/chatgpt-quickstart?tabs=command-line%2Cpython-new&pivots=programming-language-python) 代码：
