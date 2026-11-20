
下面，我们可能想快速对微调后的模型效果进行简单的验证，可以在 `Webui` 中通过检查点路径，加载这个适配器，并且重新加载模型，注意检查点路径填写微调后的输出目录，模型路径还填写微调前基础模型的路径：

![](img/d89b587df078ab85c80cabd7eb0155e1.png)

效果验证（注意调一下最大生成长度，默认的 1024 比较小，非常容易截断）：

作为对比，可以直接去掉上面的检查点路径，重新加载（先卸载）模型，就可以得到原始模型的回答结果：

### 模型合并和导出

以上的使用方式只适用于临时测试，后续使用微调后的模型我们不可能每次都同时加载一个适配器和模型，所以为了后续方便使用，我们需要将原始模型和 `Lora` 适配器进行合并，然后导出。

我们重新把微调输出目录填入检查点路径，然后重新加载模型，并设定一个导出目录（这里可以设定模型输出后的量化等级、以及一键上传 `Hugging Face` 等等，大家可以选择设置）：

导出成功后，我们进入导出目录，可以看到合并后的模型文件：

```
cd /root/autodl-tmp/save/Qwen2.5-7B-Instruct-Security
```

然后我们清空检查点路径，将合并后的模型目录填入模型路径，重新加载模型：

![](img/24c538b5973eb02edf3a7d9c9ce4647e.png)

重新测试，可以得到和之前一样的效果：

### 本地调用微调后的模型

下面我们要考虑在本地部署并通过 `API` 的方式调用微调后的模型，目前两个主流的本地部署模型的方案分别是 `Ollama` 和 `VLLM`：

> `Ollama` 是一款专注于本地部署的轻量级大模型工具，以 “开箱即用” 为核心理念，支持全平台，一键安装后即可通过命令行快速启动模型。它内置 1700 + 主流模型库（如 `Llama`、`Qwen`、`Mistral`），自动下载优化后的量化版本（如 `int4` 量化），显存占用降低 50%，`14B` 模型仅需 11GB 显存即可运行。`Ollama` 提供类 `ChatGPT` 的交互界面，支持流式输出和 `JSON` 响应，适合个人开发者快速验证模型能力、本地测试或对隐私敏感的场景（如医疗数据处理），且兼容 `CPU` 推理（需 16GB + 内存）和 `GPU` 加速，硬件门槛极低。最新版本已支持视觉模型，可处理多模态任务。

![](img/be3aebbd4413fee4a889a96c2b264be7.png)

> `VLLM` 是专为高性能推理设计的企业级框架，基于 `PyTorch` 构建，创新性引入 `PagedAttention` 技术，将 `KV Cache` 分块存储，显存利用率提升 30%，支持 4K 以上长文本推理。其动态批处理技术实现高吞吐量（如 `Llama-8B` 在 `H100` 上可达 5000+ tokens/s），是 `Ollama` 的 5 倍，并原生支持多 `GPU` 张量并行，可在 8 台 `H100` 服务器上部署 `70B` 模型，支撑日均 1 亿次请求，响应延迟 <500ms。`VLLM` 兼容 `HuggingFace` 模型，提供 `Prometheus` 监控和自动故障恢复机制，适合电商搜索、智能客服等高并发场景，以及多模态任务。

![](img/d7659bf20e4651eff9ad137abb2b5601.png)

+ 选 `Ollama`：需求为快速部署、低硬件门槛、交互式对话，例如个人开发者测试模型、小型团队搭建内部工具。

+ 选 `VLLM`：需求为高性能推理、高并发处理、多 `GPU` 扩展，例如电商平台支撑亿级请求、科研机构处理大规模数据，或需定制化模型参数的场景。

### 通过 vLLM 调用

在 `LLaMA Factory` 中，提供了 `llamafactory-cli api` 命令来帮助我们快速将模型部署为 `API` 调用，通过指定 `--infer_backend vllm` 参数可以启用 `VLLM` 推理（如果不指定，默认使用的是 `Huggingface` 推理引擎）：

```
API_PORT=6006 API_MODEL_NAME=security llamafactory-cli api \
    --model_name_or_path /root/autodl-tmp/models/security1011
    --template qwen \
    --infer_backend vllm \
    --vllm_enforce_eager
```

#### 环境变量

+ `API_PORT=6006`：设定 `API` 服务要监听的端口号为 6006。

+ `API_MODEL_NAME=security`：给 `API` 服务所使用的模型设定一个自定义名称，如果不指定默认会使用 `gpt-3.5-turbo`。

#### 命令行参数

+ `--model_name_or_path /root/autodl-tmp/save/Qwen2.5-7B-Instruct-Security`：指定模型的路径。

+ `--template qwen`：指定对话模板为 `Qwen` 这样能确保输入输出的格式和 `Qwen` 模型相适配。

+ `--infer_backend vllm`：把推理后端设置为 `vLLM`。

+ `--vllm_enforce_eager`：强制 `vLLM` 用动态图模式，这种模式在调试或者需要确定性执行顺序时非常有用。

当服务输出以下内容时表示成功启动：

![](img/438369b8653a9b81f51d70592db92d4c.png)

我们通过 `http://localhost:6006/docs` 可以访问部署后的 `API` 接口文档（可以看到是符合 `OPEN AI` 格式的）：

我们尝试获取模型列表，可以看到返回了我们指定好的 `security` 模型：

![](img/e614e2a0aef0f0706b0e8cf4a634a6e9.png)

我们可以尝试在一些聊天客户端添加自定义提供商：

测试调用：

![](img/cc6ad697cbe9d249ba540562b8126f04.png)

### 通过 Ollama 调用

<https://wxa.wxs.qq.com/tmpl/mv/base_tmpl.html>

`Ollama` 是建立在 `llama.cpp` 开源推理引擎基础上的大模型推理工具框架，其主要支持运行 `GGUF` 格式的模型。

![](img/3bec68d25e7d42b0db153c886f86d55a.png)

> `GGUF`（`GGML Universal File`）是专为大型语言模型（`LLM`）设计的文件格式，其优化了数据的存储方式，减少了存储空间的占用，这对于包含大量参数的大型模型尤为重要。

相比于默认的 `BF16` 精度（每个参数占用 16 位（2 字节）），`4BIT` 量化（每个参数仅占用 4 位（0.5 字节））可以将模型推理的显存占用降低 75%。所以使用 `Ollama` 运行 `7B` 的模型大概仅需要 4G 显存（考虑到其他开销实际可能更大一点）。但是量化后的模型也就意味着会损失一定精度，这也是为什么大家在使用 `Ollama` 运行模型总觉得会比和其他方式运行模型变得 “更傻” 的原因。

`LLaMA Factory` 默认并没有将模型转换为 `GGUF` 的方法，我们需要自己 `llama.cpp` 的仓库，使用其中的转换脚本：

```
git clone https://github.com/ggerganov/llama.cpp.git
cd llama.cpp/gguf-py
pip install --editable .
```

下载并安装好依赖后，我们执行其中的脚本，将模型目录指定为我们刚刚导出的合并好的目录：

```
python convert_hf_to_gguf.py /root/autodl-tmp/save/Qwen2.5-7B-Instruct-Security --outtype q8_0
```

开始转换：

转换成功后，将在微调后合并的模型目录下生产一个 `GGUF` 文件（`/root/autodl-tmp/save/Qwen2.5-7B-Instruct-Security/Qwen2.5-7B-Instruct-Security-F16.gguf`）：

![](img/f111d562e0ce0f0c0141ba0cff55d129.png)

然后我们在本地安装 `Ollama`：

```
curl -fsSL https://ollama.com/install.sh | sh
```

安装完成后，启动 `Ollama`：

```
OLLAMA_MODELS=/root/autodl-tmp/ollama ollama serve
```

然后我们将刚刚导出好的 `GGUF` 文件给定 `Ollama` 进行注册（注意这里指定的是 `Modelfile` 文件，而不是 `GGUF` 文件）：

```
OLLAMA_MODELS=/root/autodl-tmp/ollama ollama create security -f /root/autodl-tmp/save/Qwen2.5-7B-Instruct-Security/Modelfile
```

然后我们就可以使用 `Ollama` 运行模型了：

```
ollama run security
```

![](img/a09857d15f3ac54340dd7ec8b25eab9d.png)

### 对比模型微调的效果

为了方便验证和对比模型微调后的效果，我们在 `Easy Dataset` 提供的 `Playground` 进行测试，这里我们使用 `VLLM` 部署的服务（不损失精度、推理速度更快），我们在模型配置中添加这个模型。这里只有接口地址、模型名称是要准确填写的，提供商、`API` 密钥可以随意填写：

![](img/444d1561849cc1a18665e62b8ea1f810.png)

然后这里我们使用硅基流动提供的 `Qwn2.5-7B-Instruct` 作为基础模型，和微调后的模型（`Security`）进行对比：

![](img/e22d48b30c91d8af0ef6bafd6a527a9d.png)

以下是从多个角度的详细对比：

![](img/819998092cd92797d5ee83c1b0d7d1a9.png)

从对比可以得出结论，相比微调前的基础模型（`Qwen2.5-7B-Instruct`），模型微调后：

+ 在数据集内部的特定问题上，具备推理能力，并且回答结果更丰富，说明学习到了数据集内部的知识；

+ 在 Web 安全领域的特定问题上（不在数据集内），具备推理能力，并且回答结果更丰富，说明模型的泛化能力较好，没有过拟合；

+ 在 Web 安全整体的知识体系上，具备推理能力，并且回答结果更丰富，说明模型能够系统性整合 Web 安全领域的知识框架，形成结构化的知识输出体系，同时具备跨知识点的关联推理能力，可基于底层逻辑对复杂知识体系进行演绎构建；

+ 在非 Web 安全的某些其他领域上，具备推理能力，说明模型在聚焦 Web 安全领域专业能力强化的同时，未丧失基础模型的跨领域知识迁移能力，能够基于微调形成的推理框架对非专业领域问题进行逻辑拆解，保持了基础模型的知识泛化性与多领域适应性。
