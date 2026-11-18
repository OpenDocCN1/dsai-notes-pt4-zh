
进阶方案是在后端部署一个带有本地模型的服务器，并将其用作 API 的本地替代品。我们在这里使用 Ollama 部署的 InternLM3 为例。

1. 首先安装 [Ollama](https://ollama.com/download)

1. 设置 Ollama 后，通过在终端中键入以下命令来拉取 InternLM3 模型（这里使用 8B 的模型，大约需要 16GB 的内存，70B 的模型大约需要 64GB 以上的内存，可以根据自身的配置和需要选择不同参数的模型），Ollama 基于 llama.cpp 实现，本地 CPU 推理效率非常高（当然如果有 GPU 的话，推理效率会更高），还可以兼容 OpenAI 的接口：

   ```
   ollama pull internlm3-8b-instruct
   ```

1. 在项目目录中创建一个类似于下面的 `ModelFile`（可选）。

   ```
   FROM internlm3-8b-instruct

   # set the temperature to 1 [higher is more creative, lower is more coherent]
   PARAMETER temperature 1

   # set the system message
   SYSTEM &quot;&quot;&quot;
   &quot;&quot;&quot;
   ```

1. 接下来，创建模型（可选）：

   ```
   ollama create camel-internlm3-8b-instruct -f ./Modelfile
   ```

之后同样使用以下代码替换 `model`：`model`

   ```
   #如果出现openai.InternalServerError: Error code: 502错误可以尝试设置一下http_proxy及https_proxy
   #import os

   # os.environ[&quot;http_proxy&quot;] = &quot;http://localhost:11434/v1/&quot;
   # os.environ[&quot;https_proxy&quot;] = &quot;http://localhost:11434/v1/&quot;

   model = ModelFactory.create(
       model_platform=ModelPlatformType.OLLAMA,
       model_type=&quot;internlm3-8b-instruct&quot;,
       model_config_dict={&quot;temperature&quot;: 0.4},#可选
   )
   ```

## 使用 vLLM

vLLM 是一个高效的、面向大规模语言模型的推理库，专为大规模语言模型设计，旨在提高推理速度和减少资源消耗。vLLM 的一个核心特性是它能够在多种硬件环境下（例如单 GPU、多 GPU，甚至 CPU）高效运行，极大地降低了推理成本。

1. 首先安装 [vLLM](https://docs.vllm.ai/en/latest/getting_started/installation.html)

1. 设置 vLLM 后，启动兼容 OpenAI 的服务器，例如：

   ```
   python -m vllm.entrypoints.openai.api_server --model microsoft/Phi-3-mini-4k-instruct --api-key vllm --dtype bfloat16
   ```

之后同样使用以下代码替换 `model`：`model`

   ```
   vllm_model = ModelFactory.create(
       model_platform=ModelPlatformType.VLLM,
       model_type=&quot;microsoft/Phi-3-mini-4k-instruct&quot;,
       url=&quot;http://localhost:8000/v1&quot;, # 可选
       model_config_dict={&quot;temperature&quot;: 0.4}, #可选
   )
   ```

## Messages

### 概述

> 什么是 Agent 的 Message？

当你和朋友聊天、在网上搜索信息或是对手机语音助手说“帮我查一下天气”时，其实你都在向某个“代理者 (Agent)”发送一条“信息 (Message)”。这里的“代理者”既可以是一个人，也可以是一个能执行指令的智能程序或系统，而“信息”则是你传递的指令、问题或数据。在日常生活中，这种信息交互常常不易察觉，但在计算机科学、人工智能和自动化任务中，“Agent 的 Message”是一个至关重要的基础概念。

简单来说，Agent 的 Message 就是指系统中“智能体”或“代理者”之间互相传递的指令或数据包。就好比你给朋友发一条🛰️消息请他帮忙带杯咖啡，在智能系统中，“Agent”则是那些负责完成任务的角色，而“Message”则是他们沟通和协作的工具。当一个 Agent 收到 Message 后，会根据内容做出决策、执行任务或回复信息。

在 CAMEL 系统中，`BaseMessage` 是所有消息对象的基础类，它为对话中的每一条信息提供了统一的结构和标准化的处理方式。无论是用户输入的一段文本，还是包含图片、视频等多模态信息的数据包，都可以通过 `BaseMessage` 来统一表示和管理。

> 为什么需要统一的消息结构？

在一个对话系统中，消息可能来自多方（如用户、系统、不同类型的 Agent），且信息内容不局限于纯文本，还可能包括图像、视频甚至是自定义的元数据（`metadata`）。如果没有一个统一的基础类来约束这些消息的格式，开发者就会面临如下问题：

+   **类型繁杂且难以维护**：不同消息类型需要各自的代码逻辑和数据结构，导致系统复杂度提高。

+   **难以扩展和对接**：当需要增加新类型的消息（如引入新媒体格式或上下文信息）时，很可能需要大幅度修改原有代码。

+   **通用处理困难**：缺乏统一结构会让调试、日志记录和分析对话信息变得更加麻烦。

通过使用 `BaseMessage`，你可以：

+   将消息的创建、变形（如格式转换）和传递标准化。

+   简化对消息类型的扩展，提高代码的可维护性和可读性。

+   为后续的功能模块（如消息过滤、路由、多轮对话管理）提供一个统一的数据基础。

并且熟练掌握 `Message` 相关内容，对我们后续无论是做 RAG 应用或者模型的 Fine-tune 都非常重要！在后续章节我们会为大家介绍。

### 创建和使用 Message

在了解了 `BaseMessage` 存在的意义后，让我们直接通过实例化来看看如何创建和使用它。通过一个最小化示例，我们将掌握 `BaseMessage` 的关键属性和基本用法，再进一步扩展到多模态内容。

#### 创建 `BaseMessage` 实例的最小化示例

下面是一个最基本的代码示例，将创建一条来自用户的文本消息：

```
from camel.messages import BaseMessage
from camel.types import RoleType

# 创建一个简单的用户消息
message = BaseMessage(
    role_name=&quot;example_user&quot;,
    role_type=RoleType.USER,
    content=&quot;Hello, CAMEL!&quot;,
    meta_dict={} #添加必需的meta dict参数，即使为空也要提供，否则会报 TypeError
)

print(message)

&gt;&gt;&gt;
BaseMessage(role_name=&#x27;example_user&#x27;, role_type=&lt;RoleType.USER: &#x27;user&#x27;&gt;, meta_dict={}, content=&#x27;Hello, CAMEL!&#x27;, video_bytes=None, image_list=None, image_detail=&#x27;auto&#x27;, video_detail=&#x27;low&#x27;, parsed=None)
```

在上述示例中，我们创建了一条来自 example_user 的 `RoleType.USER` 类型消息，内容为纯文本 "Hello, CAMEL!"。这就是一个最小化的 `BaseMessage` 示例。

#### 关键属性介绍

+   `role_name`：给消息一个容易辨识的名称，如 "User"、"Assistant" 或 "System"。在更复杂的场景中，你或许会有多个用户、多个 Agent，通过 `role_name` 能帮助你追踪消息来源。

+   `role_type`：角色类型一般来自 `RoleType` 枚举，以明确此消息在对话中的身份。例如：

    +   `RoleType.USER`：表示该消息来自用户

    +   `RoleType.ASSISTANT`：表示该消息来自智能助手

+   `content`：消息的核心载体，一般是文本，也可能是解析指令、问题描述或描述性文字。

#### 简单扩展：添加多模态内容

除了纯文本外，`BaseMessage` 还支持包含图片、视频等多模态信息。这可以为你的对话系统带来更丰富的交互体验。下面的示例展示了如何向 `BaseMessage` 添加一张图片。假设你已将一张图片加载为 `PIL.Image` 对象：

`url = "https://raw.githubusercontent.com/camel-ai/camel/master/misc/logo_light.png"`

```
from PIL import Image
from io import BytesIO
import requests

# 下载一张图片并创建一个 PIL Image 对象
response = requests.get(url)
img = Image.open(BytesIO(response.content))

# 创建包含图片的用户消息
image_message = BaseMessage(
    role_name=&quot;User_with_image&quot;,
    role_type=RoleType.USER,
    content=&quot;Here is an image&quot;,
    meta_dict={},
    image_list=[img]  # 将图片列表作为参数传入
)

print(image_message)

&gt;&gt;&gt;
BaseMessage(role_name=&#x27;User_with_image&#x27;, role_type=&lt;RoleType.USER: &#x27;user&#x27;&gt;, meta_dict={}, content=&#x27;Here is an image&#x27;, video_bytes=None, image_list=[&lt;PIL.PngImagePlugin.PngImageFile image mode=RGBA size=3520x720 at 0x1DDFF8E88F0&gt;], image_detail=&#x27;auto&#x27;, video_detail=&#x27;low&#x27;, parsed=None)
```

同理，如果你有视频数据（如 `video_bytes`），也可将视频信息传入 `BaseMessage`。当你的消息中包含图片、视频等丰富媒体信息时，后续的组件（如 `ChatAgent`）便可利用这些多模态数据进行更智能和灵活的响应。

### 不同类型消息的处理

在使用 `BaseMessage` 的过程中，你不仅可以轻松创建基本的用户消息，也能够通过其内置方法快速生成其它类型（如系统消息、助手消息）的实例，并对其内容进行更新和转换。此外，`BaseMessage` 提供了多种便利的转换方法，可将消息转化为不同格式，便于对接诸如 OpenAI 等后端服务。

#### 快速生成不同类型的消息

通过 `BaseMessage` 的类方法，我们可以快捷创建出用户（User）、助手（Assistant）的消息：

```
from camel.messages import BaseMessage

# 创建用户消息
user_msg = BaseMessage.make_user_message(
    role_name=&quot;User_1&quot;,
    content=&quot;Hi, what can you do?&quot;
)

# 创建助手消息
assistant_msg = BaseMessage.make_assistant_message(
    role_name=&quot;Assistant_1&quot;,
    content=&quot;I can help you with various tasks.&quot;
)

print(&quot;User Message:&quot;, user_msg)
print(&quot;Assistant Message:&quot;, assistant_msg)
```

在上述示例中，你无需再手动指定 `role_type`，使用这些类方法即可轻松创建特定角色的消息。这样有助于在你的应用中保持代码整洁和可读性。

#### 更新消息内容

有时你需要基于某条原有的消息创建略有改动的新消息。`BaseMessage` 提供了 `create_new_instance()` 方法，使你能在保持原消息基础信息的同时，轻松更新 `content`：

```
# 基于用户消息创建一个新消息，内容稍作修改
updated_user_msg = user_msg.create_new_instance(&quot;Hi, can you tell me more about CAMEL?&quot;)
print(&quot;Updated User Message:&quot;, updated_user_msg)
```

这个方法非常有用，可以在对话过程中根据上下文动态构建消息流，而无需从头创建所有参数。

#### 将消息转换为字典格式

如果你需要查看消息内部结构，或者将消息数据传给其它系统、序列化保存，`BaseMessage` 的 `to_dict()` 方法可以直接将消息对象转化为字典结构：

```
msg_dict = assistant_msg.to_dict()
print(&quot;Message as dict:&quot;, msg_dict)
```

输出的字典中会包含消息的 `role_name`、`role_type`、`content` 等信息，使得你可以轻松与其它数据处理流程对接。

#### 适配 OpenAI 后端的消息格式

在实际应用中，你可能需要将消息传给 OpenAI 的对话接口。`BaseMessage` 提供了一组方法来将现有消息快速转化成符合 OpenAI 后端需求的格式。例如：

```
from camel.types import OpenAIBackendRole

# 将用户消息转化为 OpenAI 后端兼容的用户消息
openai_user_msg = user_msg.to_openai_message(role_at_backend=OpenAIBackendRole.USER)
print(&quot;OpenAI-compatible user message:&quot;, openai_user_msg)

# 将助手消息转化为 OpenAI 后端的助手消息
openai_assistant_msg = assistant_msg.to_openai_assistant_message()
print(&quot;OpenAI-compatible assistant message:&quot;, openai_assistant_msg)
```

通过这些方法，你可以轻松地将 `BaseMessage` 对象接入到 OpenAI 接口的调用流程中，无需手动编写繁琐的转换逻辑。

### 与 `ChatAgent` 协作

在前面的小节中，我们学习了如何创建和操作 `BaseMessage`。现在，让我们把所学的知识付诸实践，将这些消息交给 `ChatAgent`，让对话真正“活”起来。

`ChatAgent` 是 CAMEL 系统中负责对话处理与智能回应的组件。当你将 `BaseMessage` 对象传递给 `ChatAgent` 时，`ChatAgent` 将根据系统和用户消息的内容，生成具有上下文感知的回复。

#### 将文本消息直接交给 `ChatAgent` 的基本用法

如果你仅想与智能助手进行一段简单的对话，可以直接构造一个文本类型的用户消息，并使用 `ChatAgent` 的 `step()` 方法进行响应，在实际使用过程中，我们无需严格按照 `BaseMessage` 的格式来设置我们的 `message`，`ChatAgent` 会通过 `make_assistant_message` 等方法会将字符串格式的 `msg` 转换成 `BaseMessage`，我们只需要用最简便的字符串来设置我们 `message`，当然，如果你对 `role_name` 和 `role_type` 有特殊要求的话，也可以按照 `BaseMessage` 的格式来设置 `message`：
