
# GPT-OSS使用指南

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/GcaDdGbVRoL50Rx1wVUcNVHHnch](https://kq4b3vgg5b.feishu.cn/docx/GcaDdGbVRoL50Rx1wVUcNVHHnch)

![](img/3a87ee257e27d603c60096adfd0c2344.png)

欢迎来到`gpt-oss`系列，OpenAI 的开放权重模型专为强大的推理、代理任务和多功能开发人员用例而设计。

此次OpenAI发布了两种开放模型：

+   `gpt-oss-120b` — 适用于生产、通用、高推理用例，适合单个 H100 GPU（117B 总参数，其中 51亿个激活参数）

+   `gpt-oss-20b` — 适用于较低延迟以及本地或特殊用例（21B总参数，其中 36 亿个激活参数）

> 注意：`gpt-oss`模型基于 `Harmony 响应格式`进行训练，用于定义对话结构、生成推理输出和构建函数调用。如果你不是直接使用 GPT-OSS，而是通过 API 或 HuggingFace、Ollama 或 vLLM 等提供程序使用，则无需担心这一点，因为你的推理解决方案会自行处理格式。

## ✨模型亮点

+   宽松的 Apache 2.0 许可证： 自由构建，不受版权限制或专利风险 - 非常适合实验、定制和商业部署。

+   **可调节的推理力度**： 根据具体用例和延迟需求轻松调整推理力度（低、中、高）。

+   **完整的思维链**： 完全访问模型的推理过程，从而更轻松地进行调试并增强对输出的信任。并且它不会向最终用户展示。

+   **可微调：** 通过参数微调完全根据您的特定用例定制模型。

+   **Agentic 功能：** 使用模型的本机功能进行函数调用、 网页浏览 、 Python 代码执行和结构化输出。

+   **原生 MXFP4 量化**： 模型使用原生 MXFP4 精度针对 MoE 层进行训练，使得 `gpt-oss-120b` 在单个 H100 GPU 上运行，而 `gpt-oss-20b` 模型在 16GB 内存内运行。

## ✔️推理示例

### 1、Transformers库

可以将 `gpt-oss-120b` 和 `gpt-oss-20b` 与 `Transformers` 结合使用。如果使用 `Transformers` 聊天模板，它将自动应用 `Harmony 响应格式`。如果直接使用 `model.generate` ，则需要使用聊天模板手动应用 `Harmony` 格式，或者使用 `openai-harmony` 包。

+   首先，安装必要的依赖项来配置环境：

```
pip install -U transformers kernels torch 
```

+   设置完成后，可以通过运行以下代码片段来继续运行模型：

```
from transformers import pipeline
import torch

model_id = "openai/gpt-oss-20b"

pipe = pipeline(
    "text-generation",
    model=model_id,
    torch_dtype="auto",
    device_map="auto",
)

messages = [
    {"role": "user", "content": "Explain quantum mechanics clearly and concisely."},
]

outputs = pipe(
    messages,
    max_new_tokens=256,
)
print(outputs[0]["generated_text"][-1])
```

或者，可以通过 `Transformers Serve` 运行模型来启动与 OpenAI 兼容的 Web 服务器：

```
transformers serve
transformers chat localhost:8000 --model-name-or-path openai/gpt-oss-20b
```

点击这里可以看到：将 `gpt-oss` 与 `Transformers` 结合使用的更多信息。

### 2、vLLM

`vLLM` 建议使用 `uv` 进行 Python 依赖项管理。你可以使用 `vLLM` 启动与 OpenAI 兼容的 Web 服务器。以下命令将自动下载模型并启动服务器。

```
uv pip install --pre vllm==0.10.1+gptoss \
    --extra-index-url https://wheels.vllm.ai/gpt-oss/ \
    --extra-index-url https://download.pytorch.org/whl/nightly/cu128 \
    --index-strategy unsafe-best-match

vllm serve openai/gpt-oss-20b
```

也可以按照以下详细步骤进行操作：

#### 安装vLLM

`vLLM` 建议使用`uv`来管理你的 Python 环境。这将有助于你根据环境选择正确的实现。要创建新的虚拟环境并安装 `vLLM`，请运行：

```
uv venv --python 3.12 --seed
source .venv/bin/activate
uv pip install vllm --torch-backend=auto
```

#### 启动服务器并下载模型

`vLLM` 提供了一个`serve`命令，该命令将自动从 HuggingFace 下载模型并在 `localhost:8000` 上启动与 OpenAI 兼容的服务器。根据所需的模型大小，在服务器的终端会话中运行以下命令。

```
# For 20B
vllm serve openai/gpt-oss-20b

# For 120B
vllm serve openai/gpt-oss-120b
```

#### 使用API

`vLLM` 公开了与chat完成兼容的 API和与response兼容的 API，因此你无需进行太多更改即可使用 OpenAI SDK。以下是一个 Python 示例：

```
from openai import OpenAI

client = OpenAI(
    base_url="http://localhost:8000/v1",
    api_key="EMPTY"
)

result = client.chat.completions.create(
    model="openai/gpt-oss-20b",
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "Explain what MXFP4 quantization is."}
    ]
)

print(result.choices[0].message.content)

response = client.responses.create(
    model="openai/gpt-oss-120b",
    instructions="You are a helfpul assistant.",
    input="Explain what MXFP4 quantization is."
)

print(response.output_text)
```

如果你以前使用过 OpenAI SDK，这个代码你应该会非常熟悉，并且你现有的代码也可以通过更改基本 URL 来工作。

#### 使用工具（函数调用）

`vLLM`支持函数调用并赋予模型浏览功能。

函数调用通过`response`和`chat`完成 API 进行。

通过`chat`完成调用函数的示例：

```
tools = [
    {
        "type": "function",
        "function": {
            "name": "get_weather",
            "description": "Get current weather in a given city",
            "parameters": {
                "type": "object",
                "properties": {"city": {"type": "string"}},
                "required": ["city"]
            },
        },
    }
]

response = client.chat.completions.create(
    model="openai/gpt-oss-120b",
    messages=[{"role": "user", "content": "What's the weather in Berlin right now?"}],
    tools=tools
)

print(response.choices[0].message)
```

由于模型可以作为思路链 (CoT) 的一部分执行工具调用，因此将 API 返回的推理返回到后续工具调用中非常重要，你可以在其中提供答案，直到模型得出最终答案。

#### Agents SDK 集成

`gpt-oss` 与 OpenAI 的 Agents SDK 可以一起使用吗？

这两个 Agents SDK 都允许你覆盖 OpenAI 基础客户端，使其指向自托管模型的 `vLLM`。或者，对于 Python SDK，你也可以使用 `LiteLLM` 集成来代理到 `vLLM`。

以下是 Python Agents SDK 示例：

```
uv pip install openai-agents
```

```
import asyncio
from openai import AsyncOpenAI
from agents import Agent, Runner, function_tool, OpenAIResponsesModel, set_tracing_disabled

set_tracing_disabled(True)

@function_tool
def get_weather(city: str):
    print(f"[debug] getting weather for {city}")
    return f"The weather in {city} is sunny."


async def main(model: str, api_key: str):
    agent = Agent(
        name="Assistant",
        instructions="You only respond in haikus.",
        model=OpenAIResponsesModel(
            model="openai/gpt-oss-120b",
            openai_client=AsyncOpenAI(
                base_url="http://localhost:8000/v1",
                api_key="EMPTY",
            ),
        )
        tools=[get_weather],
    )

    result = await Runner.run(agent, "What's the weather in Tokyo?")
    print(result.final_output)

if __name__ == "__main__":
    asyncio.run(main())
```

#### 使用vLLM直接采样

除了使用 `vllm serve` 作为 API 服务器运行 `vLLM` 之外，还可以使用 `vLLM` Python 库直接控制推理。

如果直接使用 `vLLM` 进行采样，请务必确保输入提示符合 `Harmony 响应格式` ，否则模型将无法正常运行。可以使用 `openai-harmony` SDK 来实现这一点。

```
uv pip install openai-harmony
```

之后，可以使用 `Harmony` 对 `vLLM` 的生成函数生成的标记进行编码和解析。

```
import json
from openai_harmony import (
    HarmonyEncodingName,
    load_harmony_encoding,
    Conversation,
    Message,
    Role,
    SystemContent,
    DeveloperContent,
)

from vllm import LLM, SamplingParams

# --- 1) Render the prefill with Harmony ---
encoding = load_harmony_encoding(HarmonyEncodingName.HARMONY_GPT_OSS)

convo = Conversation.from_messages(
    [
        Message.from_role_and_content(Role.SYSTEM, SystemContent.new()),
        Message.from_role_and_content(
            Role.DEVELOPER,
            DeveloperContent.new().with_instructions("Always respond in riddles"),
        ),
        Message.from_role_and_content(Role.USER, "What is the weather like in SF?"),
    ]
)

prefill_ids = encoding.render_conversation_for_completion(convo, Role.ASSISTANT)

# Harmony stop tokens (pass to sampler so they won't be included in output)
stop_token_ids = encoding.stop_tokens_for_assistant_action()

# --- 2) Run vLLM with prefill ---
llm = LLM(
    model="openai/gpt-oss-120b",
    trust_remote_code=True,
)

sampling = SamplingParams(
    max_tokens=128,
    temperature=1,
    stop_token_ids=stop_token_ids,
)

outputs = llm.generate(
    prompt_token_ids=[prefill_ids],   # batch of size 1
    sampling_params=sampling,
)

# vLLM gives you both text and token IDs
gen = outputs[0].outputs[0]
text = gen.text
output_tokens = gen.token_ids  # <-- these are the completion token IDs (no prefill)

# --- 3) Parse the completion token IDs back into structured Harmony messages ---
entries = encoding.parse_messages_from_completion_tokens(output_tokens, Role.ASSISTANT)

# 'entries' is a sequence of structured conversation entries (assistant messages, tool calls, etc.).
for message in entries:
    print(f"{json.dumps(message.to_dict())}")
```

### 3、Ollama

如果想要在消费类硬件上运行 `gpt-oss` ，则可以在安装 Ollama 后运行以下命令来使用 Ollama。



# gpt-oss 模型使用指南

## gpt-oss-20b
`ollama pull gpt-oss:20b`
`ollama run gpt-oss:20b`

## gpt-oss-120b
`ollama pull gpt-oss:120b`
`ollama run gpt-oss:120b`

Ollama 支持 `gpt-oss` 的两种模型尺寸：

+ `gpt-oss-20b`
+ 此为较小型号
+ 最好配备 ≥16GB VRAM 或 统一内存
+ 非常适合高端消费级 GPU 或 Apple Silicon Mac

+ `gpt-oss-120b`
+ 此为更大的全尺寸模型
+ 最好配备 ≥60GB VRAM 或 统一内存
+ 非常适合多 GPU 或强大的工作站设置

## 详细步骤

### 安装 Ollama
在 Ollama 官网下载适合你的设备的 Ollama 客户端：https://ollama.com/download

### 拉取模型
直接通过以下代码拉取你想要的模型：

```
# For 20B
ollama pull gpt-oss:20b

# For 120B
ollama pull gpt-oss:120b
```

### 与 gpt-oss 模型聊天
你可以在应用程序或终端中发起聊天：

```
ollama run gpt-oss:20b
```

Ollama 采用了模拟 `OpenAI Harmony` 格式的开箱即用的聊天模板。输入您的消息并开始对话。

### 使用 API
Ollama 公开了一个与 `Chat` 完成兼容的 `API`，因此无需进行太多更改即可使用 `OpenAI SDK`。以下是一个 `Python` 示例：

```
from openai import OpenAI

client = OpenAI(
    base_url="http://localhost:11434/v1",  # Local Ollama API
    api_key="ollama"                       # Dummy key
)

response = client.chat.completions.create(
    model="gpt-oss:20b",
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "Explain what MXFP4 quantization is."}
    ]
)

print(response.choices[0].message.content)
```

如果你以前使用过 `OpenAI SDK`，这段代码你应该不会陌生。或者你也可以直接在 `python` 或者 `JavaScript` 中使用 `Ollama SDK`。

### 使用工具（函数调用）
Ollama 可以：

+ 调用函数
+ 使用内置浏览器工具（在应用程序中）

通过 `Chat` 完成调用函数的示例：

```
tools = [
    {
        "type": "function",
        "function": {
            "name": "get_weather",
            "description": "Get current weather in a given city",
            "parameters": {
                "type": "object",
                "properties": {"city": {"type": "string"}},
                "required": ["city"]
            },
        },
    }
]

response = client.chat.completions.create(
    model="gpt-oss:20b",
    messages=[{"role": "user", "content": "What's the weather in Berlin right now?"}],
    tools=tools
)

print(response.choices[0].message)
```

由于模型可以作为思路链（`CoT`）的一部分执行工具调用，因此将 `API` 返回的推理返回到后续工具调用中非常重要，你可以在其中提供答案，直到模型得出最终答案。

### Responses API 解决方法
Ollama 尚不支持原生 `Responses API`。

如果你确实想使用 `Responses API`，可以使用 `Hugging Face` 的 `Responses.js` proxy 将聊天完成转换为 `Responses API`。

### Agents SDK 集成
`gpt-oss` 与 `OpenAI` 的 `Agents SDK` 可以一起使用吗？这两个 `Agents SDK` 都允许你覆盖 `OpenAI` 基础客户端，使用 `Chat Completions` 或 `Responses.js` 代理将本地模型指向 `Ollama`。或者，也可以使用内置功能将 `Agents SDK` 指向第三方模型。

+ `Python`：使用 `LiteLLM` 通过 `LiteLLM` 代理到 `Ollama`
+ `TypeScript`：将 `AI SDK` 与 `ollama` 适配器结合使用

以下是使用 `LiteLLM` 的 `Python Agents SDK` 示例：

```
import asyncio
from agents import Agent, Runner, function_tool, set_tracing_disabled
from agents.extensions.models.litellm_model import LitellmModel

set_tracing_disabled(True)

@function_tool
def get_weather(city: str):
    print(f"[debug] getting weather for {city}")
    return f"The weather in {city} is sunny."


async def main(model: str, api_key: str):
    agent = Agent(
        name="Assistant",
        instructions="You only respond in haikus.",
        model=LitellmModel(model="ollama/gpt-oss:120b", api_key=api_key),
        tools=[get_weather],
    )

    result = await Runner.run(agent, "What's the weather in Tokyo?")
    print(result.final_output)

if __name__ == "__main__":
    asyncio.run(main())
```

## ❇️参考实现
https://github.com/openai/gpt-oss

`gpt-oss` 模型官网 GitHub 网页提供了一系列参考实现：

### Inference 推理
+ `torch` — 一个未优化的 `PyTorch` 实现，仅用于教育目的。由于未优化，至少需要 4 个 `H100` 处理器
+ `triton` — 使用 `PyTorch` 和 `Triton` 的更优化实现，包括使用 `CUDA` 图形和基本缓存
+ `metal` — 用于在 `Apple Silicon` 硬件上运行模型的 `Metal` 特定实现

### Tools 工具
+ `browser` —— 模型训练所用的浏览器工具的参考实现
+ `python` — 模型训练所用的 `python` 工具的无状态参考实现

### Client examples 示例
+ `chat` — 一个基本的终端聊天应用程序，使用 `PyTorch` 或 `Triton` 实现以及 `Python` 和浏览器工具进行推理
+ `responses_api` — 一个与 `Responses API` 兼容的服务器示例，它实现了浏览器工具以及其他与 `Responses` 兼容的功能

## 🎈基础设置
### 环境要求
+ 在 `macOS` 上：安装 `Xcode CLI` 工具 --> `xcode-select --install`
+ 在 `Linux` 上：这些参考实现需要 `CUDA`
+ 在 `Windows` 上：这些参考实现尚未在 `Windows` 上测试。如果您想在本地运行模型，请尝试使用 `Ollama` 等解决方案。

### 安装
[如果你想](https://pypi.org/project/gpt-oss/) 尝试任何代码，你可以直接从 `PyPI` 安装它：

```
# 如果你只想要工具
pip install gpt-oss
# 如果你想尝试 torch 实现
pip install gpt-oss[torch]
# 如果你想尝试 triton 实现
pip install gpt-oss[triton]
```

如果你想修改代码或尝试 `metal` 实现，需要在本地设置项目：

```
git clone https://github.com/openai/gpt-oss.git
pip install -e .[metal]
```

## 🟢下载模型
可以直接从 `Hugging Face CLI` 下载 `Hugging Face Hub` 中的模型权重：

```
# gpt-oss-120b
huggingface-cli download openai/gpt-oss-120b --include "original/*" --local-dir gpt-oss-120b/

# gpt-oss-20b
huggingface-cli download openai/gpt-oss-20b --include "original/*" --local-dir gpt-oss-20b/
```

## ✔️参考 PyTorch 实现
`OpenAI` 在 `gpt_oss/torch/model.py` 中包含了一个低效的 `PyTorch` 参考实现。此代码使用基本的 `PyTorch` 运算符来展示确切的模型架构，并在 `MoE` 中略微增加了对张量并行的支持，以便更大的模型可以使用此代码运行（例如，在 `4xH100` 或 `2xH200` 上）。在此实现中，`OpenAI` 将所有权重上溯到 `BF16` 并在 `BF16` 上运行模型。

### 安装依赖项
```
pip install -e .[torch]
```

### 运行
```
# On 4xH100:
torchrun --nproc-per-node=4 -m gpt_oss.generate gpt-oss-120b/original/
```

## ✔️参考 Triton 实现（单 GPU）
`OpenAI` 还提供了一个优化的参考实现，它使用支持 `MXFP4` 的优化的 `triton MoE` 内核。它还对注意力机制代码进行了一些优化，以降低内存成本。要运行此实现，需要安装 `triton` 和 `torch` 的 Nightly 版本。此版本可在单个 `80GB` 的 `gpt-oss-120b` `GPU` 上运行。

### 第一步运行
```
# You need to install triton from source to use the triton implementation
git clone https://github.com/triton-lang/triton
cd triton/
pip install -r python/requirements.txt
pip install -e . --verbose --no-build-isolation

# Install the gpt-oss triton implementation
pip install -e .[triton]
```

### 第二步运行
```
# On 1xH100
export PYTORCH_CUDA_ALLOC_CONF=expandable_segments:True
python -m gpt_oss.generate --backend triton gpt-oss-120b/original/
```

如果遇到 `torch.OutOfMemoryError` 请确保打开可扩展分配器，以避免从检查点加载权重时崩溃。

## ✔️参考 Metal 实现
此外，`OpenAI` 还提供了 `Metal` 在 `Apple Silicon` 上运行的参考实现。此实现尚未投入生产，但与 `PyTorch` 实现一致。

在 `Apple Silicon` 设备上运行 `.[metal]` 安装时，该实现将自动编译：

```
pip install -e .[metal]
```

要进行推理，需要首先使用以下命令将 `Hugging Face` 中的 `SafeTensor` 权重转换为正确的格式：

```
python gpt_oss/metal/scripts/create-local-model.py -s <model_dir> -d <output_file>
```

或者下载预先转换的权重：

```
huggingface-cli download openai/gpt-oss-120b --include "metal/*" --local-dir gpt-oss-120b/metal/
huggingface-cli download openai/gpt-oss-20b --include "metal/*" --local-dir gpt-oss-20b/metal/
```

然后测试运行：

```
python gpt_oss/metal/examples/generate.py gpt-oss-20b/metal/model.bin -p "why did the chicken cross the road?"
```

## 🔯Harmony format & tools
此次，除了发布 `gpt-oss` 模型之外，`OpenAI` 还发布了一个新的聊天格式库 `harmony` 以便与该模型进行交互。

`gpt-oss` 模型基于 `Harmony` 响应格式进行训练，用于定义对话结构、生成推理输出和构建函数调用。如果你不是直接使用 `gpt-oss`，而是通过 `API` 或 `Ollama` 等提供商使用，则无需担心这一点，因为你的推理解决方案会自行处理格式。如果你正在构建自己的推理解决方案，你可以参考这个指南来了解提示格式：https://cookbook.openai.com/articles/openai-harmony

`OpenAI` 还为该模型提供了两个系统工具：浏览和 `Python` 容器。工具的具体实现请参见 `gpt_oss/tools`。

## ✴️Clients
### 终端 Chat
终端聊天应用程序是一个基本示例，展示了如何将 `Harmony` 格式与 `PyTorch`、`Triton` 和 `vLLM` 实现结合使用。它还提供了 `Python` 和浏览器工具作为可选工具。



# gpt-oss 使用指南

## 命令行接口

```
usage: python -m gpt_oss.chat [-h] [-r REASONING_EFFORT] [-a] [-b] [--show-browser-results] [-p] [--developer-message DEVELOPER_MESSAGE] [-c CONTEXT] [--raw] [--backend {triton,torch,vllm}] FILE

Chat example

positional arguments:
  FILE                  Path to the SafeTensors checkpoint

options:
  -h, --help            show this help message and exit
  -r REASONING_EFFORT, --reasoning-effort REASONING_EFFORT
                        Reasoning effort (default: low)
  -a, --apply-patch     Make apply_patch tool available to the model (default: False)
  -b, --browser         Use browser tool (default: False)
  --show-browser-results
                        Show browser results (default: False)
  -p, --python          Use python tool (default: False)
  --developer-message DEVELOPER_MESSAGE
                        Developer message (default: )
  -c CONTEXT, --context CONTEXT
                        Max context length (default: 8192)
  --raw                 Raw mode (does not render Harmony encoding) (default: False)
  --backend {triton,torch,vllm}
                        Inference backend (default: triton)
```

torch 和 triton 的实现分别需要 `gpt-oss-120b/original/` 和 `gpt-oss-20b/original/` 下的原始检查点。而 vLLM 分别使用 `gpt-oss-120b/` 和 `gpt-oss-20b/` 根目录下 Hugging Face 转换后的检查点。

## Responses API

OpenAI还提供了一个示例 Responses API server。该服务器并未实现 Responses API 的所有功能和事件，但应该兼容大多数基本用例，可以为任何构建自己服务器的人提供灵感。

你可以使用以下推理后端启动此服务器：

+   `triton` — 使用 `triton` 实现
+   `metal` — 仅在 Apple Silicon 上使用 `metal` 实现
+   `ollama` — 使用 Ollama `/api/generate` API 作为推理解决方案
+   `vllm` — 使用你安装的 `vllm` 版本进行推理
+   `transformers` — 使用你安装的 `Transformers` 版本执行本地推理

## Codex

支持将 [`codex`](https://github.com/openai/codex) 作为 gpt-oss 的客户端。要运行 20b 版本，需要将其设置为 `~/.codex/config.toml`：

```
disable_response_storage = true
show_reasoning_content = true

[model_providers.local]
name = "local"
base_url = "http://localhost:11434/v1"

[profiles.oss]
model = "gpt-oss:20b"
model_provider = "local"
```

此功能适用于任何监听 11434 端口的 chat completions-API 兼容服务器，例如 `ollama`。

启动服务器并将 codex 指向 oss 模型：

```
ollama run gpt-oss:20b
codex -p oss
```

## Tools

### 浏览器搜索

两种 gpt-oss 模型都经过训练，具备使用 `browser` 工具进行浏览的能力，该工具提供以下三种方法：

+   `search` 关键词
+   `open` 以打开特定页面
+   `find` 页面上的内容

要启用浏览器工具，必须将定义放入 Harmony 格式提示符的 system 消息中。如果你的工具实现了完整接口，你可以使用 `with_browser()` 方法，或者使用 `with_tools()` 修改定义。例如：

```
import datetime
from gpt_oss.tools.simple_browser import SimpleBrowserTool
from gpt_oss.tools.simple_browser.backend import ExaBackend
from openai_harmony import SystemContent, Message, Conversation, Role, load_harmony_encoding, HarmonyEncodingName

encoding = load_harmony_encoding(HarmonyEncodingName.HARMONY_GPT_OSS)

# Exa backend requires you to have set the EXA_API_KEY environment variable
backend = ExaBackend(
    source="web",
)
browser_tool = SimpleBrowserTool(backend=backend)

# create a basic system prompt
system_message_content = SystemContent.new().with_conversation_start_date(
    datetime.datetime.now().strftime("%Y-%m-%d")
)

# if you want to use the browser tool
if use_browser_tool:
    # enables the tool
    system_message_content = system_message_content.with_tools(browser_tool.tool_config)
    # alternatively you could use the following if your tool is not stateless
    system_message_content = system_message_content.with_browser()

# construct the system message
system_message = Message.from_role_and_content(Role.SYSTEM, system_message_content)

# create the overall prompt
messages = [system_message, Message.from_role_and_content(Role.USER, "What's the weather in SF?")]
conversation = Conversation.from_messages(messages)

# convert to tokens
token_ids = encoding.render_conversation_for_completion(conversation, Role.ASSISTANT)

# perform inference
# ...

# parse the output
messages = encoding.parse_messages_from_completion_tokens(output_tokens, Role.ASSISTANT)
last_message = messages[-1]
if last_message.recipient.startswith("browser"):
  # perform browser call
  response_messages = await browser_tool.process(last_message)

  # extend the current messages and run inference again
  messages.extend(response_messages)
```

为了控制上下文窗口的大小，此工具使用一个可与模型交互的可滚动文本窗口。因此，它可能会获取页面的前 50 行，然后滚动到接下来的 20 行。该模型还经过训练，可以在其答案中使用此工具的引用。

为了提高性能，该工具会缓存请求，以便模型无需重新加载页面即可重新访问页面的不同部分。因此，你应该为每个请求创建一个新的浏览器实例。

### Python

该模型[经过训练，使用 Python](https://github.com/openai/harmony) 工具执行计算和其他操作，作为其思路链的一部分。在训练过程中，该模型使用了一个有状态的工具，这使得在 CoT 循环之间运行工具更加容易。然而，本参考实现使用的是无状态模式。因此，`PythonTool` 定义了自己的工具描述，以覆盖 openai-harmony 中的定义。

> 此实现运行在一个较为宽松的 Docker 容器中，这在诸如即时注入之类的情况下可能会出现问题。这只是一个示例，你应该考虑在生产环境中实现自己的容器限制。

要启用 Python 工具，必须将定义放入 Harmony 格式提示符的 system 消息中。如果你的工具实现了完整接口，可以使用 `with_python()` 方法，也可以使用 `with_tools()` 修改定义。例如：

```
import datetime
from gpt_oss.tools.python_docker.docker_tool import PythonTool
from openai_harmony import SystemContent, Message, Conversation, Role, load_harmony_encoding, HarmonyEncodingName

encoding = load_harmony_encoding(HarmonyEncodingName.HARMONY_GPT_OSS)

python_tool = PythonTool()

# create a basic system prompt
system_message_content = SystemContent.new().with_conversation_start_date(
    datetime.datetime.now().strftime("%Y-%m-%d")
)

# if you want to use the python tool
if use_python_tool:
    # enables the tool making sure that the prompt gets set with the stateless tool description
    system_message_content = system_message_content.with_tools(python_tool.tool_config)
    # alternatively you could use the following if your tool is not stateless
    system_message_content = system_message_content.with_python()

# construct the system message
system_message = Message.from_role_and_content(Role.SYSTEM, system_message_content)

# create the overall prompt
messages = [system_message, Message.from_role_and_content(Role.USER, "What's the square root of 9001?")]
conversation = Conversation.from_messages(messages)

# convert to tokens
token_ids = encoding.render_conversation_for_completion(conversation, Role.ASSISTANT)

# perform inference
# ...

# parse the output
messages = encoding.parse_messages_from_completion_tokens(output_tokens, Role.ASSISTANT)
last_message = messages[-1]
if last_message.recipient == "python":
  # perform python call
  response_messages = await python_tool.process(last_message)

  # extend the current messages and run inference again
  messages.extend(response_messages)
```

### 应用补丁

`apply_patch` 可用于在本地创建、更新或删除文件。

## 其他细节

### 精确格式

OpenAI发布了原生量化支持的模型。具体来说，使用 MXFP4 作为 MoE 层中的线性投影权重。将 MoE 张量分为两部分存储：

+   `tensor.blocks` 存储实际的 fp4 值。将每两个值打包成一个 `uint8` 值。
+   `tensor.scales` 存储块缩放比例。所有 MXFP4 张量的块缩放都在最后一个维度上进行。

所有其他张量均采用 BF16 格式。因此，也建议使用 BF16 作为模型的激活精度。
