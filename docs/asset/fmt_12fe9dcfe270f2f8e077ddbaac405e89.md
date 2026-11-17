
```
import os
from openai import AzureOpenAI

client = AzureOpenAI(
  azure_endpoint = "https://xxxxx.openai.azure.com/", # API端点
  api_key="xxxxx",  # API key
  api_version="2023-05-15" # API 版本
)

response = client.chat.completions.create(
    model="gpt-35-turbo", # model = "deployment_name".
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "Does Azure OpenAI support customer managed keys?"},
        {"role": "assistant", "content": "Yes, customer managed keys are supported by Azure OpenAI."},
        {"role": "user", "content": "Do other Azure AI services support this too?"}
    ]
)

print(response.choices[0].message.content)
```

> 这段代码使用了Azure版本的`OpenAI`的`GPT-3.5-turbo`模型（在这里被命名为`"gpt-35-turbo"`）来创建一个聊天助手。这个版本的`OpenAI` API与标准版本的`OpenAI` API在使用上有些许不同，但基本概念是相同的。下面是对代码中每个部分的详细解释：

1.  首先导入了`os`和`openai`模块。`os`模块是Python的标准库，用于与操作系统交互。`openai`模块是`OpenAI`的Python客户端，用于与`OpenAI`的`API`进行交互。

1.  接下来，创建了一个`AzureOpenAI`对象，该对象需要Azure的`azure_endpoint`、`api_key`和`api_version`。这些信息用于让你的程序与Azure版的`OpenAI`的`API`进行通信。

1.  使用了`client.chat.completions.create`方法来向`OpenAI`的`API`发送一个请求，请求一个新的聊天完成。这个方法需要一个模型名（在这个例子中是`"gpt-35-turbo"`），和一个历史消息列表。历史消息列表包含一系列消息，每个消息都是一个字典，包含一个“角色”（可以是“系统”、“用户”或“助手”）和一个“内容”（消息的文本）。这个历史记录被用作聊天模型的输入。

1.  `OpenAI`的`API`会返回一个响应，这个响应中包含了模型生成的新消息。你打印了这个新消息的内容。

这个代码片段是一个单次的聊天完成请求，它不会持续等待用户的输入。相反，它只是发送一个请求，打印出响应，然后结束。

### 要点：

1.  部署的名称需要和模型的名称保持一致

1.  AOAI里的模型名称一般没有小数点，比如`gpt-35-turbo`，更多有关模型的信息请参阅：Azure OpenAI 服务模型

### 适配AOAI的基准代码1：流式应答

```
import os
from openai import AzureOpenAI

client = AzureOpenAI(
  azure_endpoint = "https://xxxxx.openai.azure.com/", # API端点
  api_key="xxxxx",  # API key
  api_version="2023-05-15" # API 版本
)

stream = client.chat.completions.create(
    model="gpt-35-turbo", # 注意此处的模型名称
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "证明毕达哥拉斯定理"}
    ],
    stream=True,
)

for chunk in stream:
    if not chunk.choices or chunk.choices[0].delta.content is None:
        continue

    print(chunk.choices[0].delta.content, end="")
print()
```

### 适配AOAI的基准代码2：交互式应答

```
import os
from openai import AzureOpenAI

client = AzureOpenAI(
  azure_endpoint = "https://xxxxx.openai.azure.com/", # API端点
  api_key="xxxxx",  # API key
  api_version="2023-05-15" # API 版本
)

history = [
    {"role": "system", "content": "You are an intelligent assistant. You always provide well-reasoned answers that are both correct and helpful."},
    {"role": "user", "content": "Hello, introduce yourself to someone opening this program for the first time. Be concise."},
]

while True:
    response = client.chat.completions.create(
        model="gpt-35-turbo", 
        messages=history,
        temperature=0.7,
    )
    
    print(response.choices[0].message.content)
    history.append({"role": "assistant", "content": response.choices[0].message.content})
    history.append({"role": "user", "content": input("> ")})
```

### 套壳软件中Azure OpenAI的调用（以DIFY为例）

![](img/b65333fb3e7ddb6d4472f305d04e6cbc.png)

# 使用 API Gateway 分发管理Key

### 适用场景：

+   有专门的出海服务器或者稳定的IaaS/PaaS/CaaS托管环境，可以自主部署和持续稳定运行服务

+   需要对`API Key`进行更多的管理和配置，比如：多租户管理，额度管理以及需要把AOAI的Key转成OpenAI的key

+   推荐解决方案：使用One-API提供的[API Gateway](https://github.com/songquanpeng/one-api)：

样例：

```
https://chat.huaqloud.com/#/?settings={"key":"sk-cbsdaUEgK9a8aQvuF7891dF19c3641Ae8887Ae5a41809977","url":"https://api.huaqloud.com"}
```

### 适配API Gateway的基准代码1：

```
import os
from openai import OpenAI

client = OpenAI(  
    api_key="sk-cbsdaUEgK9a8aQvuF7891dF19c3641Ae8887Ae5a41809977", # API Key
    base_url="https://api.huaqloud.com/v1" # API Gateway 服务器地址
)

stream = client.chat.completions.create(
    model="gpt-3.5-turbo",
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "证明费马大定理"}
    ],
    stream=True,
)

for chunk in stream:
    if not chunk.choices or chunk.choices[0].delta.content is None:
        continue

    print(chunk.choices[0].delta.content, end="")
print()
```

注意：看上去是不是平淡无奇，但是在这个样例中，后台的key资源实际上是AOAI提供的。

# LLM 本地运行的平替解决方案： LM Studio

### 适用场景：

+   需要在一个完全封闭的场景中测试LLM开发

+   不需要有彪悍的GPU，但是需要有较多的内存（可用内存10G以上）以及较多的可用磁盘空间（可用磁盘空间在20GB以上），

+   同时要有一定的耐心（毕竟是模型经过一定程度的剪裁才能在普通设备运行的使用CPU进行推理）

GGUF(以前称为GGML)是一种量化方法，允许用户使用CPU来运行LLM，但也可以将其某些层加载到GPU以提高速度。虽然使用CPU进行推理通常比使用GPU慢，但对于那些在CPU或苹果设备上运行模型的人来说，这是一种非常好的格式。如果你想同时利用CPU和GPU, GGUF是一个非常好的格式。

官方网站：[LM Studio](https://lmstudio.ai/)

推荐模型：`chinese-alpaca-2-7b.Q8_0.gguf`

### 官方示例代码

```
# Chat with an intelligent assistant in your terminal
from openai import OpenAI

# Point to the local server
client = OpenAI(base_url="http://localhost:1234/v1", api_key="not-needed")

history = [
    {"role": "system", "content": "You are an intelligent assistant. You always provide well-reasoned answers that are both correct and helpful."},
    {"role": "user", "content": "Hello, introduce yourself to someone opening this program for the first time. Be concise."},
]

while True:
    completion = client.chat.completions.create(
        model="local-model", # this field is currently unused
        messages=history,
        temperature=0.7,
        stream=True,
    )

    new_message = {"role": "assistant", "content": ""}
    
    for chunk in completion:
        if chunk.choices[0].delta.content:
            print(chunk.choices[0].delta.content, end="", flush=True)
            new_message["content"] += chunk.choices[0].delta.content

    history.append(new_message)
    
    # Uncomment to see chat history
    # import json
    # gray_color = "\033[90m"
    # reset_color = "\033[0m"
    # print(f"\n{'-'*20} History dump {'-'*20}\n")
    # print(json.dumps(history, indent=2))
    # print(f"\n{'-'*55}\n{reset_color}")

    print()
    history.append({"role": "user", "content": input("> ")})
```

> 这段代码是一个使用`OpenAI`聊天模型的`Python`程序，它在你的本地终端中创建了一个聊天助手。以下是主要部分的概述：

1.  **导入模块和初始化客户端**：首先，代码从`openai`库中导入了`OpenAI`类，并实例化了一个`OpenAI`对象。这个对象指向一个本地服务器（`http://localhost:1234/v1`），并使用一个不必要的API密钥（`"not-needed"`）。

1.  **定义历史记录**：定义了一个名为`history`的列表，它包含了聊天的历史记录。列表中的每个元素都是一个包含`"role"`（角色）和`"content"`（内容）的字典。

1.  **无限循环**：然后，代码进入了一个无限循环，在循环中，程序会持续地生成聊天回复并接收用户输入。

+   **创建聊天完成**：在每次循环中，代码首先使用`client.chat.completions.create`方法创建一个聊天完成。这个方法需要一个模型名（在这个例子中是`"local-model"`，但这个字段当前未使用），一个历史消息列表，一个`"temperature"`参数（控制生成文本的随机性），以及一个`"stream"`参数（如果设置为`True`，`API`会以流的形式返回聊天完成，这意味着你可以在完成生成时获取部分结果）。

+   **处理聊天完成**：然后，代码创建了一个新的消息字典，并将其`"role"`设为`"assistant"`，`"content"`设为空字符串。接着，代码遍历了聊天完成的每一部分（每一`"chunk"`），打印出每一部分的内容，并将这些内容添加到新消息的`"content"`中。

+   **更新历史记录**：然后，代码将新消息添加到历史记录中。

+   **显示历史记录**：这一部分代码被注释掉了，但如果你取消注释，它会打印出整个聊天历史。

+   **接收用户输入**：最后，代码接收用户的输入，并将其添加到历史记录中。

这个程序会持续运行，直到你手动停止它。在每次迭代中，它都会打印出模型的回复，并等待用户的输入。

### 适配LM Studio的基准代码1：
