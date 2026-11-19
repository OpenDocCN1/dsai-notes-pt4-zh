
# 实现本地LLM基础架构

> 来源：[https://zsyhjtnsa5.feishu.cn/docx/Qkewdoz0jocEpzxoWyycosbjnhd](https://zsyhjtnsa5.feishu.cn/docx/Qkewdoz0jocEpzxoWyycosbjnhd)

## 实验概述

### 适用场景和硬件需求

+ 为开发人员提供一个支持代码编写的LLM环境
+ 计算机硬件达到一定的要求（内存至少16GB，建议32GB及以上，如果有8GB显存以上的独立显卡更佳）

### 项目框架

![](img/255ed556046603ef2de69552b5d53cd6.png)

```
graph LR
    C[Client] -->|http://apigw:3010/v1/chat/completions| B[Gateway]
    C -->|http://apigw:3010/v1//embeddings| B
    B -->|http://lmstudio:1234| A[Chat Model]
    B -->|http://m3e:6008| D[Embeddings Model]
    style C fill:#f9d,stroke:#333,stroke-width:0px
    style B fill:#fc0,stroke:#333,stroke-width:0px
    style A fill:#9cf,stroke:#333,stroke-width:0px
    style D fill:#9fc,stroke:#333,stroke-width:0px
```

+ 首先，我们部署LLM模型，需要部署聊天模型和嵌入式模型
+ 接下来，部署API Gateway，把模型的API以完全兼容OpenAI的格式发布出来，并借助GW进行API Key的管理和监控
+ 最后就是各种常用工具的适配和对接

### 软件环境

+ 具备运行Python的环境
+ 支持Docker

## 部署模型

### 使用LM Studio部署语言模型

#### LM Studio的部署和初始化

下载LM Studio

`https://lmstudio.ai/`

安装LM Studio，双击一步一步next即可

![](img/69d9d71cc8f4fcaecdb93fd78de499d6.png)

LM Studio的操作很简单，按照左边的按钮从上往下操作即可

搜索和下载LLM模型，选择放大镜按钮，实例中以 `llama-2-7B` 为关键字

![](img/c2d026c13353aae662c15a22c491c45b.png)

> 选择模型的时候，首先从本机可用的内存数量选择模型的参数，比如有8GB可用内存一般选择7B的模型，16GB可用内存选择13B的模型，其次综合考虑模型发行者，模型下载数量 点赞数和发行时间，再其次考虑结合带宽和硬盘大小选择具体的模型的量化规格。

选择最下方的文件夹按钮可以查看和管理本机目前已下载的模型

![](img/74f60ffca83b607ab6fa4f9f11aa7182.png)

> 注意，国内场景需要手动从 `https://hf-mirror.com/TheBloke/Llama-2-7B-Chat-GGUF/resolve/main/llama-2-7b-chat.Q8_0.gguf?download=true` 下载映像文件，然后将文件移动到本地路径 `C:\Users\%USERNAME%\.cache\lm-studio\models\`，注意需要创建一个对应的目录结构

> 嵌入式模型：`https://hf-mirror.com/nomic-ai/nomic-embed-text-v1.5-GGUF/resolve/main/nomic-embed-text-v1.5.Q8_0.gguf`

> 如果本机仅有8GB左右的内存，可以考虑使用更小一点的模型`phi-2`:`https://hf-mirror.com/TheBloke/phi-2-GGUF/resolve/main/phi-2.Q8_K.gguf`

> `DeepSeek-R1-Distill-Qwen-7B-Q4_K_M.gguf`

镜像下载路径：

`https://hf-mirror.com/lmstudio-community/DeepSeek-R1-Distill-Qwen-7B-GGUF/blob/main/DeepSeek-R1-Distill-Qwen-7B-Q4_K_M.gguf`

本地目录结构

![](img/c4a06c52c0a914b1042df216b1b60630.png)

#### 测试模型对话能力

首先确保加载好模型，你可以根据以下这些话题进行深入的对话，以此来测试模型的理解和生成能力

```
1. "The Implications of Quantum Computing on Cybersecurity";
2. "The Role of Philosophy in Shaping Human Perception of Reality";
3. "The Impact of AI on Future Job Markets";
4. "The Cultural Significance of Renaissance Art";
5. "The Influence of Ancient Greek Philosophy on Modern Western Thought";
6. "The Technological Factors That Contributed to the Industrial Revolution";
7. "The Ethical Dilemmas Posed by Genetic Engineering";
8. "The Social and Cultural Impact of the Internet Revolution";
9. "The Historical Significance of the Silk Road";
10. "The Role of Literature in Shaping Societal Values";
```

对话效果示例，

![](img/1522a9b57114618a6f8916b50f478a3f.png)

请特别关注最下面一排的性能数据：

+ `time to first token`: 1.97s
+ `gen t`: 64.53s
+ `speed`: 5.00 tok/s
+ `stop reason`: completed
+ `gpu layers`: 0
+ `cpu threads`: 4
+ `mlock`: false
+ `token count`: 385/2048

启用GPU支持

在右侧侧边栏的GPU Offload中做如下设置，需要重新加载模型

![](img/24cbb2fd41fb9037cdd12f471305e859.png)

> `n_gpu_layers`如果显存不能完全吃下模型，这个数值通常在10~20之间进行设置和调整，如果显存可以吃下模型，可以设置为-1

![](img/c13b27c6a2ccde34d9851a239a8a4b9e.png)

依然请关注最下面一排的性能数据：

+ `time to first token`: 28.27s
+ `gen t`: 40.24s
+ `speed`: 11.00 tok/s
+ `stop reason`: completed
+ `gpu layers`: 35
+ `cpu threads`: 4
+ `mlock`: false
+ `token count`: 1335/4096

以下是两次性能数据的对比和解释

| 参数 | 第一次运行 | 第二次运行 |
| --- | --- | --- |
| `time to first token` | 1.97s | 28.27s |
| `gen t` | 64.53s | 40.24s |
| `speed` | 5.00 tok/s | 11.00 tok/s |
| `gpu layers` | 0 | 35 |
| `cpu threads` | 4 | 4 |
| `mlock` | false | false |
| `token count` | 385/2048 | 1335/4096 |

从表格中我们可以更直观地看到两次运行的差异。

1. `time to first token`: 这是生成第一个令牌所花费的时间。在第一次运行中，这个时间为1.97秒，而在第二次运行中，这个时间为28.27秒。这可能意味着第二次运行在启动或初始化时遇到了更多的延迟。

1. `gen t`: 这是生成所有令牌所花费的总时间。在第一次运行中，这个时间为64.53秒，而在第二次运行中，这个时间为40.24秒。尽管第二次运行的启动时间更长，但总的生成时间却更短，这可能意味着第二次运行在生成令牌时更有效率。

1. `speed`: 这是生成令牌的速度，单位是每秒生成的令牌数。在第一次运行中，速度为5.00 tok/s，而在第二次运行中，速度为11.00 tok/s。这进一步证实了第二次运行在生成令牌时更有效率。

1. `gpu layers`: 这是使用的GPU层的数量。在第一次运行中，没有使用GPU层（数值为0），而在第二次运行中，使用了35层。这可能是第二次运行速度更快的原因，因为GPU通常能提供更高的计算速度。

1. `token count`: 这是生成的令牌数量。在第一次运行中，生成了385个令牌，而在第二次运行中，生成了1335个令牌。尽管第二次运行生成了更多的令牌，但由于使用了更多的GPU层，其生成速度还是更快。

总的来说，第二次运行虽然在启动时花费了更多的时间，但在生成令牌时更有效率，可能是由于使用了更多的GPU层。

从右侧侧边栏的Mode Inspector处可以查看模型的参数信息

![](img/43245e5194b86ce7472403596ee40c29.png)

```
{
  "name": "LLaMA v2",
  "arch": "llama",
  "quant": "Q8_0",
  "context_length": 4096,
  "embedding_length": 4096,
  "num_layers": 32,
  "rope": {
    "dimension_count": 128
  },
  "head_count": 32,
  "head_count_kv": 32,
  "parameters": "7B"
}
```

> 模型的配置信息，具体参数的含义如下：

1. `name`: 这是模型的名称，这里是"LLaMA v2"。
1. `arch`: 这是模型的架构类型，这里是"llama"。LLaMA 是一种大型语言模型架构。
1. `quant`: 这是模型的量化配置，这里是"Q8_0"。量化是一种将模型参数从浮点数转换为整数的技术，可以减少模型的内存占用和计算复杂性。
1. `context_length`: 这是模型可以处理的最大上下文长度，这里是4096。这意味着模型可以处理最多4096个令牌的输入。
1. `embedding_length`: 这是模型的嵌入长度，这里是4096。嵌入是将词汇转化为数值向量的过程。
1. `num_layers`: 这是模型的层数，这里是32。深度学习模型通常由多个层组成，每一层都会进行一些特定的计算。
1. `rope`: 这是模型的"rope"配置，这里的`dimension_count`是128。"Rope"可能是模型特定的一种配置或技术。
1. `head_count`: 这是模型的头数，这里是32。在Transformer模型中，"头"是自注意力机制的一部分，可以让模型同时关注输入的不同部分。
1. `head_count_kv`: 这是模型的键/值对的头数，这里也是32。这也是Transformer模型的一部分，键/值对用于计算每个输入部分的权重。
1. `parameters`: 这是模型的参数数量，这里是"7B"，即70亿。这是一个非常大的模型，有着大量的参数。

注意：如果把本地的显卡性能充分释放出来，速度可达 80token/s

#### 发布模型API 服务

选择左侧工具栏的双向箭头标志进入Local Inference Server，点击图示中的绿色按钮，启动服务

![](img/26ff8fffd9d07c5231d4cc74b594e038.png)

![](img/2722db7758eb8f9a249996301d4ef687.png)

使用应用提供的hello world(`curl`)范例测试API服务

```
# Load a model, start the server, and run this example in your terminal
# Choose between streaming and non-streaming mode by setting the "stream" field

curl http://localhost:1234/v1/chat/completions \
-H "Content-Type: application/json" \
-d '{
  "messages": [
    { "role": "system", "content": "Always answer in rhymes." },
    { "role": "user", "content": "Introduce yourself." }
  ],
  "temperature": 0.7,
  "max_tokens": -1,
  "stream": false
}'
```

以下是terminal的输出效果，注意：此处将 `http://localhost:1234/v1/chat/completions` 替换为：`http://192.168.14.111:1234/v1/chat/completions`

```
chengzh@AIStudio:~$ curl http://192.168.14.111:1234/v1/chat/completions \
-H "Content-Type: application/json" \
-d '{
  "messages": [
    { "role": "system", "content": "Always answer in rhymes." },
    { "role": "user", "content": "Introduce yourself." }
  ],
  "temperature": 0.7,
  "max_tokens": -1,
  "stream": false
}'
{
  "id": "chatcmpl-75qxzrm5ky5zpy431kyqci",
  "object": "chat.completion",
  "created": 1705284798,
  "model": "C:\\Users\\cheng\\.cache\\lm-studio\\models\\TheBloke\\Llama-2-7B-Chat-GGUF\\llama-2-7b-chat.Q8_0.gguf",
  "choices": [
    {
      "index": 0,
      "message": {
        "role": "assistant",
        "content": "I'm a poet with flair,\nMy rhymes are always fair and rare.\nI love to write and read aloud,\nMy passion is making words proud."
      },
      "finish_reason": "stop"
    }
  ],
  "usage": {
    "prompt_tokens": 26,
    "completion_tokens": 38,
    "total_tokens": 64
  }
}chengzh@AIStudio:~$
```

以下是LM Studio处观察到的实时日志

![](img/f095476381f187f759c222cb2381a6a3.png)



# LM Studio 基础模型 API 服务

至此，LM Studio 已经帮我们把基础模型的 API 服务创建好了，总结一下以下是他的一些信息

+   `base_url="http://192.168.14.111:1234/v1"`

+   `api_key="not-needed"`

+   `model="local-model"`

现在开始我们可以在代码中调用这些信息，但是为了进一步规范和简化 API 调用，我们还需要通过 API Gateway 对它进行封装和发布。

## （可选）使用容器部署嵌入式模型

使用 CPU 运行 m3e 模型

```
docker run -d -p 6008:6008 --name=m3e-large-api registry.cn-hangzhou.aliyuncs.com/fastgpt_docker/m3e-large-api:latest
```

使用 GPU 运行 m3e 模型

```
docker run -d -p 6008:6008 --gpus all --name=m3e-large-api registry.cn-hangzhou.aliyuncs.com/fastgpt_docker/m3e-large-api:latest
```

运行官方原生镜像

```
docker run -d -p 6008:6008 --name=m3e-large-api stawky/m3e-large-api:latest
```

测试 m3e 模型 API

```
curl --location --request POST 'http://localhost:6008/v1/embeddings' \
--header 'Authorization: Bearer sk-aaabbbcccdddeeefffggghhhiiijjjkkk' \
--header 'Content-Type: application/json' \
--data-raw '{"model": "m3e","input": ["laf是什么"]}'
```

> 注意：`localhost` 需要替换成你的 IP 地址，IP 地址可以在你连接的网络的属性里查看，或者在 `cmd` 里使用：`ipconfig` 查看 `WLAN` 或者以太网那一栏。注意，IP 地址要填 `ipv4` 的地址，不要填成 `ipv6` 的地址

测试成功的效果:

```
chengzh@AIStudio:~$ curl --location --request POST 'http://192.168.14.111:6008/v1/embeddings' \
--header 'Authorization: Bearer sk-aaabbbcccdddeeefffggghhhiiijjjkkk' \
--header 'Content-Type: application/json' \
--data-raw '{"model": "m3e","input": ["laf是什么"]}'
{"data":[{"embedding":[0.03281713277101517,0.031639888882637024,0.004614340141415596,0.011289759539067745,0.01797243393957615,0.0029386456590145826,-0.010540101677179337,-0.019790107384324074,0.03897610306739807,0.0040751611813902855,-0.02647821418941021,0.0014181790174916387,-0.032893769443035126,-0.007957538589835167,-0.0003922222531400621,-0.0022389532532542944,0.050569042563438416,-0.02809222787618637,-0.04931304603815079,-0.028279747813940048,0.04029926657676697,-0.03626347333192825,-0.047754693776369095,0.04065224900841713,0.011744920164346695,0.04014689475297928,-0.005499007645994425,-0.03164784982800484,……
```

至此嵌入式模型的 API 服务创建好了，总结一下以下是他的一些信息

+   `base_url="http://192.168.14.111:6008/v1/embeddings"`

+   `api_key="sk-aaabbbcccdddeeefffggghhhiiijjjkkk"`

+   `model="m3e"`

同样地，为了进一步规范和简化 API 调用，我们还需要通过 API Gateway 对它进行封装和发布。

# 使用 API Gateway 管理 API Key 的分发

One-api 项目 `https://github.com/songquanpeng/one-api`

> 这是一个开源项目，通过标准的 OpenAI API 格式，可访问和管理多种大模型，如 Azure、Anthropic Claude、Google PaLM 2、智谱 ChatGLM、百度文心一言、讯飞星火认知、阿里通义千问以及 360 智脑等。该系统不仅支持配置镜像和第三方代理服务，还提供了流式传输以实现打字机效果。

> 除此之外，该项目还具备令牌管理、兑换码管理以及通道管理等功能，用户可以根据需求进行自定义设置和页面设计。对于部署方面，我们提供了方便的 Docker 镜像，一键部署，开箱即用。同时，还提供了相应的部署命令和 Nginx 配置，使得整个部署过程更为简便。

> 总的来说，这是一个功能强大的接口管理和分发系统，它可以进行二次分发管理 key，只需一个单独的可执行文件，便可轻松管理和使用各种大模型。

## 部署 One-API

```
docker run --name one-api -d --restart always -p 3010:3000 -e TZ=Asia/Shanghai -v /home/ubuntu/data/one-api:/data justsong/one-api
```

> 这个命令是用来在 Docker 中运行一个名为 "one-api" 的容器。下面是这个命令行的详细解释：

+   `docker run`：这是一个 Docker 命令，用于运行一个新的容器。

+   `--name one-api`：这个选项指定了新容器的名称为 "one-api"。

+   `-d`：这个选项表示在后台运行容器，并返回容器 ID，也就是说你不会直接进入容器的命令行界面。

+   `--restart always`：这个选项会让 Docker 在容器退出时总是重启容器，无论退出状态是什么。这对于保证服务的持续运行很有帮助。

+   `-p 3010:3000`：这个选项将容器内部的 3010 端口映射到主机的 3000 端口。这样，你就可以通过访问主机的 3010 端口来访问运行在容器内部的服务。

+   `-e TZ=Asia/Shanghai`：这个选项设置了一个环境变量，指定了容器内的时区为上海。

+   `-v /home/ubuntu/data/one-api:/data`：这个选项挂载了主机的 `/home/ubuntu/data/one-api` 目录到容器的 `/data` 目录。这样，容器可以直接访问和修改这个目录的文件。

+   `justsong/one-api`：这是你要运行的 Docker 镜像的名字。Docker 会从 Docker Hub（一个存储 Docker 镜像的公共库）下载这个镜像，如果本地没有的话。

执行完上述命令之后，访问 One-API 首页

![](img/d527f32989e905b450dbdf51d64bbd27.png)

点击右上角的登录按钮，使用以下凭据进行登录

用户名：root

密码：123456

![](img/2dbfc56c9fb6546911c599eb68e0703b.png)

## 创建和配置渠道

![](img/d9a0a33966efc7517df801d40e200fe4.png)

+   渠道相当于是映射到 LLM 的资源池

+   令牌相当于调用 API 资源的凭据

进入渠道，选择添加新的渠道，使用以下信息注册由 LMStudio 支持的 API 服务

![](img/7cf06a69c6d383aedf7d6307d890c863.png)

注意：

+   `Base URL` 填写为：`http://192.168.14.111:1234`

+   模型：可以随意添加后续开发或者应用所需的文本和聊天模型的名称

+   密钥：如果不能留空，可以随机填写

使用以下信息注册 m3e 嵌入式模型服务

![](img/8ab03b8c0146941261f9613bfeae05da.png)

注意：

+   `Base URL` 填写为：`http://192.168.14.111:6008`

+   模型：输入 `m3e` 和后续开发或者应用所需的嵌入式模型的名称，本例中为 `text-embedding-ada-002`

+   密钥：使用 `sk-aaabbbcccdddeeefffggghhhiiijjjkkk`

## 创建令牌

点击添加新的令牌，请注意合理设置过期时间和额度

![](img/68c3be2d8ff0fe08c64d76900665b494.png)

查看可用令牌，选择复制-> ChatGPT Next Web

![](img/a71e2b4e6db3720e48c43c300cdd810c.png)

得到以下输出，

```
https://chat.oneapi.pro/#/?settings={"key":"sk-e7dmfUq8P8C5EyJk41D11082B5C84aCd9f4381E63cFaA73c","url":"http://localhost:3000"}
```

进行必要的调整和解析可以得到后续我们使用的 API 参数信息

`api_key :sk-e7dmfUq8P8C5EyJk41D11082B5C84aCd9f4381E63cFaA73c`

`base_url:http://192.168.14.111:3010` (使用宿主机的 IP 地址，此外还要注意端口是 `3010`)

## API 调用测试和监控

以本地部署的 ChatGPT-Next，输入上述 API 信息之后

![](img/3ac7d103beb3d15abc5c1f6505e07615.png)

页面中进行对话测试

![](img/69affdd08819b025c5f1c8e65a024b09.png)

查看 Gateway 的日志，特别关注此处的时间和令牌名称

![](img/d6910c6b59118826c712740e753ed8ac.png)

查看 LM Studio API Server 日志

![](img/766783bc89fc23c758fd49ddcac4346b.png)

太长中间省略一部分

![](img/a1a0196875206b5eb2037a6196c98b6c.png)

使用以下命令测试嵌入式模型

```
curl --location --request POST 'http://192.168.14.111:3010/v1/embeddings' \
--header 'Authorization: Bearer sk-e7dmfUq8P8C5EyJk41D11082B5C84aCd9f4381E63cFaA73c' \
--header 'Content-Type: application/json' \
--data-raw '{"model": "text-embedding-ada-002","input": ["laf是什么"]}'
```

查看 Gateway 日志，特别关注模型类型

![](img/403f232de288bf0423c7b154deef7920.png)

# 对接本地开发环境和应用

## 开发环境

参考以下范例，对代码中 `api_key` 和 `base_url` 进行替换即可，注意 `base_url` 需要加 `/v1`

```
import os

os.environ["OPENAI_API_KEY"] = "sk-e7dmfUq8P8C5EyJk41D11082B5C84aCd9f4381E63cFaA73c" #替换 key
os.environ["BASE_URL"] = "http://aistudio:3010/v1" #替换 url,此处使用本地机器名

from openai import OpenAI

api_key = os.getenv("OPENAI_API_KEY")
base_url = os.getenv("BASE_URL")

client = OpenAI(  
    api_key=api_key,
    base_url=base_url
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

## 对接 Flowise

填写 API

![](img/ce4bee1bd953ab681973d2d2ce275a27.png)

确保模型的 `BasePath` 指向 `http://192.168.14.111:3010/v1`

![](img/08718d25b05cf102d10813a4938e87dd.png)

输出效果示意

![](img/87c7cb3c2c09cbc289e8f3412aa54c87.png)

## 对接 FastGPT

### 安装 FastGPT

1.  创建 `fastgpt` 目录：

```
mkdir fastgpt
```

1.  切换到 `fastgpt` 目录：

```
cd fastgpt
```

1.  下载 `docker-compose` 文件：

```
curl -O https://raw.githubusercontent.com/labring/FastGPT/main/files/deploy/fastgpt/docker-compose.yml
```



# 快速开始

## Docker Compose 部署

```
# 重点关注 OPENAI_BASE_URL 和 CHAT_API_KEY
version: '3.3'
services:
  pg:
    image: ankane/pgvector:v0.5.0 # git
    # image: registry.cn-hangzhou.aliyuncs.com/fastgpt/pgvector:v0.5.0 # 阿里云
    container_name: pg
    restart: always
    ports: # 生产环境建议不要暴露
      - 5432:5432
    networks:
      - fastgpt
    environment:
      # 这里的配置只有首次运行生效。修改后，重启镜像是不会生效的。需要把持久化数据删除再重启，才有效果
      - POSTGRES_USER=username
      - POSTGRES_PASSWORD=password
      - POSTGRES_DB=postgres
    volumes:
      - ./pg/data:/var/lib/postgresql/data
  mongo:
    image: mongo:5.0.18
    # image: registry.cn-hangzhou.aliyuncs.com/fastgpt/mongo:5.0.18 # 阿里云
    container_name: mongo
    restart: always
    ports: # 生产环境建议不要暴露
      - 27017:27017
    networks:
      - fastgpt
    environment:
      # 这里的配置只有首次运行生效。修改后，重启镜像是不会生效的。需要把持久化数据删除再重启，才有效果
      - MONGO_INITDB_ROOT_USERNAME=username
      - MONGO_INITDB_ROOT_PASSWORD=password
    volumes:
      - ./mongo/data:/data/db
  fastgpt:
    container_name: fastgpt
    image: ghcr.io/labring/fastgpt:latest # git
    # image: registry.cn-hangzhou.aliyuncs.com/fastgpt/fastgpt:latest # 阿里云
    ports:
      - 3020:3000
    networks:
      - fastgpt
    depends_on:
      - mongo
      - pg
    restart: always
    environment:
      # root 密码，用户名为: root
      - DEFAULT_ROOT_PSW=1234      
      - OPENAI_BASE_URL=https://api.openai.com/v1 #此处替换为gw地址
      - CHAT_API_KEY=sk-xxxx #此处替换为自己的key
      - DB_MAX_LINK=5 # database max link
      - TOKEN_KEY=any
      - ROOT_KEY=root_key
      - FILE_TOKEN_KEY=filetoken
      # mongo 配置，不需要改. 如果连不上，可能需要去掉 ?authSource=admin
      - MONGODB_URI=mongodb://username:password@mongo:27017/fastgpt?authSource=admin
      # pg配置. 不需要改
      - PG_URL=postgresql://username:password@pg:5432/postgres
    volumes:
      - ./config.json:/app/data/config.json
networks:
  fastgpt:
```

1. 下载 `config` 文件：

```
curl -O https://raw.githubusercontent.com/labring/FastGPT/main/projects/app/data/config.json
```

运行 `fastgpt`：

```
docker-compose up -d
```

### 创建知识库问答

创建知识库：使用直接分段方式

![](img/5ee7385c4b7817920fd70d9ea74866c3.png)

创建应用

![](img/f92ade4609ec158e4637051a7c18f371.png)

输出效果

![](img/369cbb84ce75e5b67fe9223ed9e105b4.png)

GW 相关日志

![](img/308c748bc1246f20cf55599353b348de.png)

# 其他套壳应用

## 适配更多模型的聊天套壳

`lobe-chat`: `https://github.com/lobehub/lobe-chat`

demo: `https://lobechat.huaqloud.com/chat`

`bedrock`: `https://github.com/DamonDeng/BRClient`

`https://brclient.damonai.com/`

## RAG

`ragflow`: `https://github.com/infiniflow/ragflow`

`http://test.ablinkbiotech.com:6714/knowledge`

## 其他

`freeaskinternet`

`https://github.com/nashsu/FreeAskInternet`

![](img/758dd0f24aaadb291071b96664fa7e29.png)

快速部署

```
docker run -d --name free-ask-internet -p 3000:3000 --add-host=freegpt35:127.0.0.1 --add-host=backend:127.0.0.1 --add-host=searxng:127.0.0.1 wbuntu/free-ask-internet:v0.0.1
```

## `freegpt35`

```
docker run -p 3040:3040 ghcr.io/missuo/freegpt35
```

`https://killercoda.com/`

## 股票行情分析示例

> 仅作场景演示，不作为投资建议

`http://150.158.125.175:8000/`

图形界面的 Linux

```
docker run -d -p 8080:80 dorowu/ubuntu-desktop-lxde-vnc
```

## 本地运行大模型汇总

大模型的本地运行和微调

`https://github.com/datawhalechina/self-llm`

国内镜像：`https://gitee.com/cloudzun/self-llm`

国内大模型列表

`https://github.com/wgwang/awesome-LLMs-In-China`
