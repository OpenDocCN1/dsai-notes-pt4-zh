
## vLLM

```
from vllm import LLM
llm = LLM("facebook/opt-13b", tensor_parallel_size=4)
output = llm.generate("San Franciso is a")
```

要运行多 GPU 服务，请在启动服务器时输入 `--tensor-parallel-size` 参数。例如，在 4 个 GPU 上运行 API 服务器：

```
python -m vllm.entrypoints.api_server --model facebook/opt-13b --tensor-parallel-size 4
```

要将 `vLLM` 扩展到单台机器之外，运行 `vLLM` 之前通过 CLI 启动 `Ray` 运行时：

```
# On head node
ray start --head
# On worker nodes
ray start --address=<ray-head-address>
```

之后，通过将 `tensor_parallel_size` 设置为 GPU 数量（即所有机器上 GPU 的总数），在头部节点上启动 `vLLM` 进程，即可在多台机器上运行推理和服务，再将 `ray` 通过 `kubeRay` 运行在 `K8S` 上，一个生产就绪的部署方案呼之欲出。

另外，不用担心，`vLLM` 和 `Ray` 为了简化开发者使用，联合开发了 `RayLLM` 项目，在后面部分会介绍到，它整合了这些功能，开发者能够很轻松在分布式环境下进行推理部署。

不仅如此，`vLLM` 还可以运行在 `Triton Server` 上，这也进一步体现了其强大的生态适配能力。具体可参考：https://github.com/triton-inference-server/tutorials/blob/main/Quick_Deploy/vLLM/README.md

## TensorRT-LLM

英伟达作为 AI 领域超一线的公司，很早就押注深度学习，在 2016 年 11 月就推出了 `TensorRT`（原名 `GPU Inference Engine (GIE)`），它是一个深度学习模型优化器和运行时库，它可以将深度学习模型转换为优化的格式，从而在英伟达 `GPU` 上实现更快的推断速度。其早期在嵌入式场景有比较广泛的应用。

![](img/aec6db93637b9abcf681946a49f65de5.png)

可以看到，`TensorRT` 本质上是一种 `Intermediate representation (IR)`，通过它可以将面向提升开发者开发效率的语言转换为面向提升目标平台执行效率的语言，`LLVM` 就是其代表之一。

![](img/2feaba08a06c9e96973a8927453494f5.png)

`TensorRT` 可以直接解析某些框架（如 `PyTorch`、`TF`）的模型或通过 `ONNX` 格式导入模型，然后通过一系列优化操作来提升模型推理速度。比如，通过自动识别可以合并的连续层，并将它们融合成一个操作。这减少了在 `GPU` 上的操作数量，从而提高了执行速度。同时，还可以做到与机器学习框架解耦。

![](img/68cab8ef81204ac5908bcc3eb3107430.png)

另一方面，随着 `transformer` 架构的模型越来越流行，英伟达为了提升 `transformer` 模型推理的性能，开发了 `FasterTransformer`，并于 2019 年开源，它包含 `Transformer` 块的高度优化版本的实现，其中包含编码器和解码器部分。使用此模块，您可以运行编码器-解码器架构模型（如：`T5`）、仅编码器架构模型（如：`BERT`）和仅解码器架构模型（如：`GPT`）的推理。`FT` 框架是用 `C++/CUDA` 编写的，依赖于高度优化的 `cuBLAS`、`cuBLASLt` 和 `cuSPARSELt` 库，可以在 `GPU` 上进行快速的 `Transformer` 推理。

但不可否认，虽然英伟达手握推理全栈 `TensorRT`、`FasterTransformer`、`Triton Server`，但在新的 AI 2.0 时代，过去优势并不能持续保持，甚至有被赶超之势，比如 `HuggingFace` 的 `TGI`，一方面英伟达拉拢 `HuggingFace`，并将其集成在自己的开发平台。

> `Hugging Face` 作为开源 AI 领域的独角兽，在 `LLM` 开发工具链和模型生态领域持续领跑。就在前几天英伟达新品发布会上，`Hugging Face` 作为英伟达新产品 `AI workbench` 的重要部件被集成，从某种意义上，这是绿色巨人对这家独角兽公司的认可，也反映了 `Hugging Face` 在模型开发领域的独到理解。

> `ully`，👸号：AI 工程化 [`Hugging Face 偷偷放大招了`](https://mp.weixin.qq.com/s?__biz=MzA5MTIxNTY4MQ==&mid=2461141343&idx=1&sn=80cc9d01ca15512edfe9edf68334340b&chksm=87396b71b04ee2674f1675a6700d7b473fb41418c50538081375d0e80fe2cf06a34237c5f326&token=239254008&lang=zh_CN#rd)，`Rust` 版本的 `ML` 框架 `Candle` 曝光

另一方面，也在不断完善自己在大模型层面的技术，特别是大模型推理。最近 9 月，英伟达将 `TensorRT` 的深度学习编译器、`FasterTransformer` 的优化内核、预处理和后处理以及多 `GPU`/多节点通信，封装在一个简单的开源 `Python` API 中并且搭配硬件（`GPU H100` 等）进行了一体优化，形成了产品级的大模型推理的方案 `TensorRT-LLM`。`TensorRT-LLM` 提供了一个易用、开源和模块化的 `Python` 应用编程接口，不需要深入的 `C++` 或 `CUDA` 专业知识，能够部署、运行、调试各种大语言模型，还能获得顶尖性能表现，以及快速定制化的功能，其在 `GPT-J 6B` 的推理性能较 `A100` 提升了 8 倍。在 `LLaMA2` 上提升了 4.6 倍。不得不说，软硬一体优化是英伟达独有的优势。

![](img/1e45640fa50870d4502c5679523a46a2.png)

官方给出了如下特点介绍：

+ 专为 `LLM` 设计：与标准的 `TensorRT` 不同，`TensorRT-LLM` 针对大型语言模型的特定需求和挑战进行了优化。
+ 集成优化：`NVIDIA` 与多家领先公司合作，将这些优化集成到了 `TensorRT-LLM` 中，以确保 `LLM` 在 `NVIDIA GPU` 上的最佳性能。
+ 模块化 `Python` API：`TensorRT-LLM` 提供了一个开源的模块化 `Python` API，使开发者能够轻松定义、优化和执行新的 `LLM` 架构和增强功能。
+ 飞行批处理（`In-flight batching`）：这是一种优化的调度技术，可以更有效地处理动态负载。它允许 `TensorRT-LLM` 在其他请求仍在进行时开始执行新请求，从而提高 `GPU` 利用率。
+ 支持新的 `FP8` 数据格式：在 `H100 GPU` 上，`TensorRT-LLM` 支持新的 `FP8` 数据格式，这可以大大减少内存消耗，同时保持模型的准确性。
+ 广泛的模型支持：`TensorRT-LLM` 包括了许多在今天生产中广泛使用的 `LLM` 的完全优化、即用版本，如 `Meta Llama 2`、`OpenAI GPT-2` 和 `GPT-3` 等。
+ 并行化和分布式推断：`TensorRT-LLM` 利用张量并行性进行模型并行化，这使得模型可以在多个 `GPU` 之间并行运行，从而实现大型模型的高效推断。
+ 优化的内核和操作：`TensorRT-LLM` 包括了针对 `LLM` 的优化内核和操作，如 `FlashAttention` 和遮蔽多头注意力等。
+ 简化的开发流程：`TensorRT-LLM` 旨在简化 `LLM` 的开发和部署过程，使开发者无需深入了解底层的技术细节。
+ 来自：https://developer.nvidia.com/blog/nvidia-tensorrt-llm-supercharges-large-language-model-inference-on-nvidia-h100-gpus/

从上面可以看到，英伟达将最近的一些优化思路都应用到了 `TensorRT-LLM` 中，其获得如此高的性能优化也不能解释。

遗憾的是，目前该工具还处于早期试用阶段，并且需要组织身份才能注册获取资格。按照官方计划，该工具将会被集成到英伟达的 `NeMo` 框架中（一款供开发者构建和训练对话式 `AI` 模型的开源框架，被集成在 `NVIDIA` 的企业级 AI 平台），普通开发者还需要再等一段时间，未来可通过 `NGC`（英伟达的 `GPU` 云服务）上的 `NeMo` 框架或 `github` 使用到 `TensorRT-LLM`。

## DeepSpeed-MII

该项目背靠 AI 领域另一个超一流玩家微软，是鼎鼎大名的 `DeepSpeed` 的一部分，核心目标便是优化大模型推理性能，降低推理成本和使用门槛。

> `DeepSpeed` 是一个由微软开发的开源深度学习优化库，旨在提高大规模模型训练的效率和可扩展性。它通过多种技术手段来加速训练，包括模型并行化、梯度累积、动态精度缩放、本地模式混合精度等。`DeepSpeed` 还提供了一些辅助工具，如分布式训练管理、内存优化和模型压缩等，以帮助开发者更好地管理和优化大规模深度学习训练任务。此外，`deepspeed` 基于 `pytorch` 构建，只需要简单修改即可迁移。`DeepSpeed` 已经在许多大规模深度学习项目中得到了应用，包括语言模型、图像分类、目标检测等等。

据官方介绍中，`MII` 提供了对数千种广泛使用的 `DL` 模型的高度优化实现的访问。与模型原始实现相比，`MII` 支持的模型实现了显著更低的延迟和成本。例如，`MII` 将 `Bloom 176B` 模型的延迟降低了 5.7 倍，同时将成本降低了 40 倍以上。同样，它将部署 `Stable Diffusion` 的延迟和成本降低了 1.9 倍。在推理优化技术上，为 `MII` 利用了 `DeepSpeed` 推理的一系列优化，如 `transformer` 的深度融合、多 `GPU` 推理的自动张量切片、`ZeroQuant` 的动态量化等技术。`MII` 支持简单快速的本地部署或者 `Azure` 云部署。

![](img/bbd6aca2dbed5c68657df6fc1fea6c9c.png)

> `DeepSpeed-Inference` 是 `DeepSpeed` 框架在推理方面的扩展。合并了张量、流水线并行以及自定义优化 `cuda` 核等并行化技术。`DeepSpeed` 提供了无缝推理模式来兼容 `DeepSpeed`、`Megatron` 和 `HuggingFace` 训练的 `Transformer` 模型。集成了模型并行技术，从而使得可以在多个 `GPU` 上进行大模型的推理。注意：`DeepSpeed` 的代码不能使用 `python` 来执行，需要通过 `deepspeed` 命令执行，如：

```
deepspeed --num_gpus 4 --master_port 60000 inference.py
```

在架构层面，`MII` 由 `DeepSpeed-Inference` 支持。根据模型类型、模型大小、批量大小和可用硬件资源，`MII` 会自动应用来自 `DeepSpeed-Inference` 的推理优化策略，以最大限度地减少延迟和提高吞吐量。它通过使用许多预先指定的模型注入策略之一来实现这一目标，这些策略允许 `MII` 和 `DeepSpeed-Inference` 识别 `PyTorch` 模型的基础架构，并将其替换为优化后的实现。这样，`MII` 就能自动地利用 `DeepSpeed-Inference` 中的大量优化策略优化数千种流行模型。

![](img/0d36e5d02191c8ff258f0c50c3195602.png)

`MII` 架构，显示 `MII` 如何在使用 `GRPC` 部署到企业内部或使用 `AML Inference` 部署到 `Microsoft Azure` 之前，使用 `DS-nference` 自动优化 `OSS` 模型。

在模型支持上，`MII` 目前已支持 50,000 多个模型，涵盖文本生成、问题解答、文本分类等一系列任务。由 `MII` 加速的模型可通过多个开源模型库获得，如 `Hugging Face`、`FairSeq`、`EluetherAI` 等。支持基于 `Bert`、`Roberta` 或 `GPT` 架构的密集模型，规模从数亿参数到数百亿参数不等。未来将支持大规模的千亿参数密集模型和稀疏模型。但遗憾的是，目前尚不支持 `llama2` 等流行的模型。

在部署上，`MII` 有两种不同的推理服务形式。第一种称为 `ds-public`，项目包含大部分 `DeepSpeed-Inference` 优化功能，也可通过 `DeepSpeed` 库使用。第二种称为 `ds-azure`，与 `Azure` 的集成更紧密，通过 `MII` 提供给所有微软 `Azure` 客户。与开源的 `PyTorch` 基线相比，两种变体都能显著降低延迟和成本，而后者还能为基于生成的工作负载提供额外的性能优势。

![](img/ceead530d0f69b5e2204e98d874e7b77.png)

`GPT` 模型的延迟比较。`MII-Azure` 比基线快达 3 倍

简单使用例子：

1）部署运行

```
# DON'T INSTALL USING pip install deepspeed-mii
# git clone https://github.com/microsoft/DeepSpeed-MII.git
# git reset --hard 60a85dc3da5bac3bcefa8824175f8646a0f12203
# cd DeepSpeed-MII && pip install .
# pip3 install -U deepspeed
# ... and make sure that you have same CUDA versions:
# python -c "import torch;print(torch.version.cuda)" == nvcc --version
import mii
mii_configs = {
    "dtype": "fp16",
    'max_tokens': 200,
    'tensor_parallel': 1,
    "enable_load_balancing": False
}
mii.deploy(task="text-generation",
           model="huggyllama/llama-13b",
           deployment_name="llama_13b_deployment",
           mii_config=mii_configs)
```

2）查询实例

```
import mii
generator = mii.mii_query_handle("llama_13b_deployment")
result = generator.query(
    {"query": ["Funniest joke ever:"]},
   do_sample=True,  max_new_tokens=200)
print(result)
```

## 模型服务层相关工具和框架

前文对模型服务层做了简单的分类，基于封装程度不同，有大量的框架和工具，笔者结合目前趋势，在每个类别中挑选了几个常见的项目和大家分享，它们各有特点，将从内向外一一介绍，最后给出一些选型建议以供参考。

![](img/6d22bc9c5854d53a513509643f198595.png)

下面介绍三款以端设备为主的推理引擎，分别是 `ggml`、`mlc-llm`、`ollama`，其核心便是降低运行门槛，让个人电脑手机等设备也能运行大模型。这也是未来商业化非常有潜力的方向。

### ggml

![](img/788041aac8988e25f852580d469c5404.png)

没有听说过 `ggml`，那也一定听说过 `llama.cpp`。`ChatGPT` 惊艳的效果让开发者为之惊讶，但大模型训练的高成本和周期让大部分开发者望而却步，今年 2 月 `meta` 开源了 `llama`，让众多开发者和中小企业为之兴奋，然而，其高昂的部署成本仍然将大部分人挡在门外。而这时候，`Georgi Gerganov` 3 月 11 日悄悄地在 `GitHub` 开源了 `llama.cpp` 项目，通过它可以让开发者在没有 `GPU` 的条件下也能运行 `LLaMA` 模型，一下子成为当时 `GitHub` 最火爆的项目，当前已经获得 4 万收藏，很多开发者成功的在 `MacBook` 甚至在树莓派上运行了 `LLaMA`。`Georgi Gerganov` 也一鼓作气在 6 月份成立了 `ggml.ai`，并且 近日 凭借其潜在的巨大商业价值进入 `AIGrant` 第二批次资助名单。

而 `ggml` 可以说是 `llama.cpp` 和 `whisper.cpp` 沉淀内化的产物，现在也成为这两个项目的内核。它是 `C++` 实现的张量库，目标是在消费级硬件上高效运行大型模型。核心优化手段就是量化，支持了 2 位、3 位、4 位、5 位、6 位和 8 位整数量化，除此之外，在编译和硬件上也做了针对性的优化，比如面向 `Apple Silicon` 的优化等。`llama.cpp` 支持当前主流的模型，包括中文大模型 `Baichuan`，且社区非常活跃，配套了很多不同语言的 `bind`，如 `java`、`python`、`go` 等，开发者可以按照需求选择。

如果想基于 `ggml` 开发 `LLM` 应用，给大家介绍一个比较常用的 `python bind` —— `ctransformers`，量化模型可以直接使用 `TheBloke`（https://huggingface.co/TheBloke）提供的模型。

下面是一个简单的示例：

1）推理：
