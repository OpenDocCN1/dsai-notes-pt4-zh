

![](img/798cb39a80d846f8033b9ce5a76ec589.png)

其中 `ollama` 调用的就是 `Q4_K_M` 量化版本。这里我们还是考虑下载 `Q4_K_M` 量化版本的 `GGUF` 权重进行调用，具体执行流程如下。

## 三、Ollama 本地部署与调用

### 1. 下载 GGUF 格式权重

```
cd ~/autodl-tmp
mkdir QwQ-32B-GGUF
modelscope download --model Qwen/QwQ-32B-GGUF qwq-32b-q4_k_m.gguf --local_dir ./QwQ-32B-GGUF
```

![](img/30fb51b1f410da9df02fff29624569b4.png)

下载后模型文件如图所示：

![](img/d8037ac5fa34c200836e4c1a9ce76d7a.png)

此外，也可以在课件网盘中进行下载：

![](img/323a9a98e07f2e40b3bd712fc50cf324.png)

![](img/5762d221a68d370aa7ad879d57b727f4.png)

### 2. 注册模型

然后需要创建一个 `file` 文件，用于进行 `ollama` 模型注册：

![](img/4b19cb871c506777fc140a8994943583.png)

然后在 `File` 文件中写入自定义模型 `GGUF` 权重地址：

```
FROM ./qwq-32b-q4_k_m.gguf
```

![](img/f567972b3a8d51d899613d4995484460.png)

然后将该模型加入 `Ollama` 本地模型列表：

```
cd /root/autodl-tmp/QwQ-32B-GGUF
ollama create qwq-32b -f ModelFile
```

> 使用 `ollama` 的任何命令，都要确保后台 `ollama` 处于开启状态

![](img/c0e32f39782db339215befecb99e1bd6.png)

查看模型是否注册成功

```
ollama list
```

![](img/2894e34513738a195010ffd20f2c41b3.png)

并且，如果此前修改过模型路径，则可以在新的 `ollama` 默认保存模型路径下看到新模型：

![](img/e42572f2aaa22f20eaeee414f8209261.png)

接下来即可在命令行中进行调用：

```
ollama run qwq-32b
```

![](img/6d6c1b001ed90d0a9fd6634f03216b69.png)

> 这里需要注意，尽管手动下载的 `GGUF` 格式模型权重是阿里官方发布的模型权重，但仍然会偶尔出现和 `ollama` 不兼容，导致胡言乱语的情况出现。如果希望使用 `ollama` 进行推理，优先建议使用 `ollama pull` 命令下载 `ollama` 官方的 `GGUF` 格式。

### 3. Ollama API 本地运行流程

在部署完 `ollama` 之后，即可借助 `ollama` API（也就是 `OpenAI` 风格 API）在代码环境中调用模型。

#### 导入 OpenAI 库

```
from openai import OpenAI
```

#### 实例化 OpenAI 客户端

```
client = OpenAI(
    base_url='http://localhost:11434/v1/',
    api_key='ollama',  # required but ignored
)
```

#### 创建消息

```
prompt = "在单词\"strawberry\"中，总共有几个R？"
messages = [
    {"role": "user", "content": prompt}
]
```

#### 获得回复

```
response = client.chat.completions.create(
    messages=messages,
    model='qwq-32b',
)

print(response.choices[0].message.content)
```

最终运行结果如下：

![](img/3184ddbfc842ddf7efb5d21c9dfd124a.png)

而此时显存占用约 22G 左右：

![](img/01fb52237ce44cfbd4d2e7597c56d2ad.png)

## 四、QWQ-32B 模型接入 vLLM 与推理流程

### 1. vLLM 安装与启动

接下来继续介绍 `QWQ-32B` 模型借助 `vLLM` 进行推理的完整流程。相比 `ollama`，`vLLM` 更加适合企业级高并发应用场景，但对应的，显存占用也会更高，`vLLM` 项目主页：`https://github.com/vllm-project/vllm`

![](img/7e5d28f7f7e309fe89b14698517829bd.png)

#### vLLM 安装

```
pip install vllm
```

#### DeepSeek R1 调用

目前 `vLLM` 已支持 `QWQ` 模型调用，可以在模型支持列表中查看模型关键字：`https://docs.vllm.ai/en/latest/models/supported_models.html`

![](img/d09aae096f6f46b0b072d16c42101d51.png)

接下来即可按照如下流程进行调用。

### 2. OpenAI 风格 API 响应模式

需要注意的是，`QWQ` 最高支持 `128K` 上下文长度，但随着模型上下文越长，所需要占用的显存也越大。根据测试，模型在 `32K` 上下文时，运行需要 `63G` 显存。

![](img/9bce8c5c366222ef57f1421256955193.png)

而当上下文增加到 `128K` 时，则需要将近 `150G` 显存：

![](img/9b0ebcc3d0991cdca26fd966483c3ad5.png)

因此，启动 `vLLM` 时候需要谨慎的设置最大上下文和对应的运行 GPU 数量。

#### 32K 上下文，单 GPU 运行命令

```
cd /root/autodl-tmp
vllm serve ./QwQ-32B --max-model-len 32768
```

#### 128K 上下文，双 GPU 运行命令

```
cd /root/autodl-tmp
CUDA_VISIBLE_DEVICES=0,1 vllm serve ./QwQ-32B --tensor-parallel-size 2
```

> 当前服务器为 `H800`（`80G`）双卡主机。

启动后后端如图所示：

![](img/faf83b269ce6ab39ea8dd0b5a6a37faa.png)

此时，即可在 `Jupyter` 中进行调用。

#### 导入 OpenAI 库

```
from openai import OpenAI
```

#### 实例化 OpenAI 客户端

```
openai_api_key = "EMPTY"
openai_api_base = "http://localhost:8000/v1"

client = OpenAI(
    api_key=openai_api_key,
    base_url=openai_api_base,
)
```

#### 创建消息

```
prompt = "在单词\"strawberry\"中，总共有几个R？"
messages = [
    {"role": "user", "content": prompt}
]
```

#### 获得回复

```
response = client.chat.completions.create(
    model="./QwQ-32B",
    messages=messages,
)

print(response.choices[0].message.content)
```

最终运行结果如下：

![](img/0b6dde2556751a1e485b5ae8e2bb3ca9.png)

推理结束后，后端如图所示：

![](img/28c14b6d23297d5661e89ad571763d7c.png)

![](img/21050404d57986804f9661082451705c.png)

## 五、基于 llama.cpp 的 QwQ 模型 CPU 推理

由于 `QwQ` 模型采用了和 `Qwen2.5` 系列模型完全相同的架构，因此，也是可以使用 `llama.cpp` 进行纯 `CPU` 推理或者 `CPU+GPU` 混合推理的。接下来介绍如何使用 `llama.cpp` 调用模型权重进行推理和对话。

### 1. llama.cpp 下载与编译

`llama.cpp` 项目主页：`https://github.com/ggml-org/llama.cpp`

![](img/066673f3962bcafe818d78dc4a94756e.png)

由于 `llama.cpp` 是个 `C` 语言项目，因此实际调用过程需要先构建项目，然后设置参数进行编译，然后最终创建可执行文件（类似于脚本），再运行本地大模型。借助 `llama.cpp` 可以实现纯 `CPU` 推理、纯 `GPU` 推理和 `CPU+GPU` 混合推理。

#### 依赖下载

为了能够顺利的完成 `C` 语言项目的项目创建和代码编译，首先需要先进行相关依赖的下载：

```
apt-get update
apt-get install build-essential cmake curl libcurl4-openssl-dev -y
```

这条命令安装了一些常用的构建和开发工具，具体的每个部分的含义如下：

- `build-essential`：安装一组构建必需的工具和库，包括：
  - 编译器（如 `GCC`）
  - `make` 工具
  - 其他一些常见的构建工具，确保你的系统能进行编译。
- `cmake`：安装 `CMake` 工具，它是一个跨平台的构建系统，允许你管理项目的编译过程。
- `curl`：安装 `cURL` 工具，它是一个命令行工具，用于通过 URL 发送和接收数据。它在很多开发场景中都很有用，尤其是与网络交互时。
- `libcurl4-openssl-dev`：安装 `libcurl` 库的开发版本。它是 `cURL` 的一个库文件，允许你在编程中通过 `cURL` 发送 HTTP 请求。`libcurl4-openssl-dev` 是与 `OpenSSL` 配合使用的版本，提供了 `SSL/TLS` 加密支持，用于安全的 HTTP 请求。

#### llama.cpp 源码下载

若是 `AutoDL` 服务器，可以先开启学术加速：

```
source /etc/network_turbo
```

然后再进行下载：

```
git clone https://github.com/ggerganov/llama.cpp
```

也可以直接在课件网盘中找到代码文件，直接上传服务器并解压缩：

![](img/3beb4901cc8dfbb820dba63cc4266e6b.png)

![](img/94b5ebfbab48ae7af5980ff3620ea578.png)

准备好后，即可在服务器中看到 `llama.cpp` 项目文件夹：

#### 项目构建与编译

接下来需要使用 `cmake` 来构建项目文件：

```
cmake llama.cpp -B llama.cpp/build \
    -DBUILD_SHARED_LIBS=OFF -DGGML_CUDA=ON -DLLAMA_CURL=ON
```

`cmake`：运行 `CMake` 工具，用于配置和生成构建文件。

`llama.cpp`：指定项目的源代码所在的目录。在这个例子中，`llama.cpp` 是项目的根目录。

`-B llama.cpp/build`：指定生成构建文件的目录。`-B` 参数表示构建目录，`llama.cpp/build` 是生成的构建目录。这是 `CMake` 将生成的文件存放的地方（例如 `Makefile` 或 `Ninja` 构建文件）。

同时还指定了一些编译选项：

- 禁用共享库（`-DBUILD_SHARED_LIBS=OFF`），生成静态库。
- 启用 `CUDA` 支持（`-DGGML_CUDA=ON`），以便在有 `GPU` 的情况下使用 `GPU` 加速。
- 启用 `CURL` 库支持（`-DLLAMA_CURL=ON`），以便支持网络请求。

![](img/87b34c9f90b6055ce0223214827403c8.png)

然后需要进一步进行编译：

```
cmake --build llama.cpp/build --config Release -j --clean-first --target llama-quantize llama-cli llama-gguf-split
```

`--build llama.cpp/build`：告诉 `CMake` 使用 `llama.cpp/build` 目录中的构建文件来执行构建过程。这个目录是在之前运行 `cmake llama.cpp -B llama.cpp/build` 命令时生成的，包含了所有构建所需的文件（例如 `Makefile` 或 `Ninja` 构建文件）。

`--config Release`：指定构建的配置为 `Release` 配置。

`Release` 配置通常意味着启用更多的优化，生成的程序运行速度较快，适合发布。

在 `CMake` 中，通常有两种常见的构建配置：

- `Debug`：用于调试版本，包含调试信息且没有做过多优化。
- `Release`：优化后的发布版本，去除调试信息，运行时性能更高。

`-j`：表示并行构建，允许 `CMake` 使用多个 `CPU` 核心来加速构建过程。

如果没有指定数字，`CMake` 会使用默认的并行级别，通常是可用的所有 `CPU` 核心。你也可以指定并行的作业数，例如 `-j 8` 表示使用 8 个并行作业进行编译。

`--clean-first`：表示在构建之前先清理掉之前的构建结果。这可以确保每次构建时都是从一个干净的状态开始，避免由于缓存或中间文件引起的编译错误。

如果你之前运行过构建并且有问题，或者希望重新构建而不使用任何缓存文件，这个选项非常有用。

`--target`：指定构建的目标（target）。通常，一个项目会定义多个目标（比如库、可执行文件等），通过这个参数可以告诉 `CMake` 只编译特定的目标。

`llama-quantize`：可能是与模型量化相关的目标。量化（quantization）是将模型的精度从浮点数降低到整数，从而减少内存占用和提高推理速度。

`llama-cli`：可能是一个命令行工具，用于运行模型或与用户交互。

`llama-gguf-split`：可能是一个用于拆分模型文件的目标，通常用于将一个大模型文件拆分成多个小文件，方便存储和加载。

![](img/6abb7ac259e3479ddf455ecf61ea9190.png)

#### 复制可执行文件

```
cp llama.cpp/build/bin/llama-* llama.cpp
```

将所有生成的可执行文件从构建目录 `llama.cpp/build/bin/` 复制到项目的根目录 `llama.cpp` 下。这样可以更方便地在项目根目录下执行这些可执行文件，而无需每次都进入构建目录。

在准备完成后，接下来即可进行调用和推理测试了。

### 2. 借助 llama.cpp 运行 QwQ 模型

#### 纯 CPU 推理流程【1token/s】

首先是纯 `CPU` 推理测试。此时系统只调用内存+`CPU` 进行计算，此时不会用到 `GPU`，在调用 `Q4_K_M` 模型时，实际内存占用为 `18G`。

![](img/b65e6598c79b4550aa7de4d251434d62.png)

此时模型运行门槛很低，但运行速度非常慢，只有不到 `1tokens/s`。具体推理代码实现流程如下：

```
cd ./llama.cpp

./llama-cli \
    --model /root/autodl-tmp/QwQ-32B-GGUF/qwq-32b-q4_k_m.gguf \
    --cache-type-k q4_0 \
    --threads 64 \
    --prio 2 \
    --temp 0.6 \
    --ctx-size 512 \
    --seed 3407 \
    --n-gpu-layers 0 \
    -no-cnv \
    --prompt "<|User|>你好，好久不见，请介绍下你自己。<|Assistant|>" 
```

其中命令行核心参数说明：

- `--threads`：`CPU` 核心数；
- `--ctx-size`：输出的上下文长度；
- `--n-gpu-layers`：需要卸载到 `GPU` 的层数，设置为 `0` 时代表完全使用 `CPU` 进行推理；
- `--temp`：模型温度参数；
- `-no-cnv`：不进行多轮对话；
- `--cache-type-k`：`K` 缓存量化为 `4bit`；
- `--seed`：随机数种子；

实际运行效果如下所示：

![](img/7d28abc27c9a2462295ab2bde0e0a892.png)

#### CPU+GPU 混合推理

接下来进一步尝试 `CPU+GPU` 混合推理，我们只需要合理的设置 `--n-gpu-layers` 参数，即可灵活的将模型的部分层加载到 `GPU` 上进行运行。并且无需手动设置，`llama.cpp` 会自动识别当前 `GPU` 数量以及可以分配的显存，自动将模型权重加载到各个不同的 `GPU` 上。例如，我们这里考虑将 `30` 层加载到 `GPU` 上，运行效果如下所示：

