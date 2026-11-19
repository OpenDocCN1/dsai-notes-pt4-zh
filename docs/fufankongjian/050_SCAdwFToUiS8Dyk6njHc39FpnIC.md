
# 最新Ktransformers v0.24并发部署 DeepSeek-R1&V3模型

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/FCOqdF5BDox8mbxIXhwciGW6nsf](https://kq4b3vgg5b.feishu.cn/docx/FCOqdF5BDox8mbxIXhwciGW6nsf)

## 课程说明

体验课内容节选自《2025大模型Agent智能体开发实战》(春季班)完整版付费课程

体验课时间有限，若想深度学习大模型技术，欢迎大家报名由我主讲的《2025大模型Agent智能体开发实战》(春季班)

![](img/5862ce86f84d6d8e3a1be06a7b75641a.png)

![](img/35e3634f030e1c33b06f5167237914c7.png)

此外，公开课全套学习资料，已上传至网盘

（`https://pan.baidu.com/s/1NLniijSwld39yKGf075q2g?pwd=h632`）

需要更系统深入学习大模型可扫码⬆️添加助教咨询喔～

---

## 一、 DeepSeek-R1&V3高性能部署思路

### 1. 主流高性能部署方案

截止目前，毫无疑问，DeepSeek模型最高性价比的部署方案有两个，其一是清华大学团队提出的 Ktransformers 部署方案，

`ktransformers`：https://github.com/kvcache-ai/ktransformers

![](img/7de883e057145365b316cce97b8afeff.png)

其二则是 Unsloth 动态量化方案，

Unsloth：https://github.com/unslothai/unsloth

![](img/24d531d12cd09e1a913f85205704b30b.png)

尽管两套方案都是采用 CPU+GPU 混合推理，但有所不同的是，Ktransformers 技术方案是在 `llama.cpp` 上进行的大幅修改，从而加快了 CPU 推理速度，而 Unsloth 则主要是在模型量化层面下功夫，推出了 1.58bit 等一众量化程度较深、但同时推理性能较好的量化模型，而实际运行过程，底层仍然是使用 `llama.cpp` 进行推理。

在此前的公开课中，我们曾分别介绍过独家 KTransformers 技术实战：https://www.bilibili.com/video/BV1kyAke9EBA/

以及独家 Unsloth 动态量化部署满血 DeepSeek：https://www.bilibili.com/video/BV1oePLezEZD/

![](img/60c446145ecd99ad08b160fe902d4c15.png)

甚至详细讲解过 KTransformers+Unsloth 结合部署方案，既使用 KTransformers 来运行 Unsloth 的 1.58bit 动态量化模型《60G 内存+14G 显存运行满血 DeepSeek R1！》：https://www.bilibili.com/video/BV1AL9mY2E5a/

![](img/9bb7fafeb4ecd4c343c261831b93bf07.png)

### 2. Ktransformers v0.24 重大更新

虽然 Ktransformers 能够高效完成推理加速，但受限于此前的技术架构，导致其无法进行并发，这也是阻碍 Ktransformers 框架迈向工业化应用的核心因素。而就在 4 月 2 号的项目更新中，Ktransformers 正式推出了 v0.24 版，该版本在 0.23 基础上进行了 1 万多行的代码更新，不仅进一步提高了推理速度，而且还增加了并发功能。

新版 Ktransformers v0.24 更新公告如下：https://github.com/kvcache-ai/ktransformers/blob/main/doc/en/balance-serve.md

#### KTransformers v0.2.4 发布说明

我们非常高兴地宣布，期待已久的 KTransformers v0.2.4 现已正式发布！在这个版本中，我们对整体架构进行了重大重构，更新了超过 1 万行代码，为社区带来了备受期待的多并发支持。

本次重构借鉴了 `sglang` 的优秀架构，在 C++ 中实现了高性能的异步并发调度机制，支持如连续批处理、分块预填充（chunked prefill）等特性。由于支持并发场景下的 GPU 资源共享，整体吞吐量也在一定程度上得到了提升。

#### 1. 多并发支持

- 新增对多个并发推理请求的处理能力，支持同时接收和执行多个任务。
- 我们基于高性能且灵活的算子库 `flashinfer` 实现了自定义的 `custom_flashinfer`，并实现了可变批大小（variable batch size）的 CUDA Graph，这在提升灵活性的同时，减少了内存和 padding 的开销。
- 在我们的基准测试中，4 路并发下的整体吞吐量提升了约 130%。
- 在英特尔的支持下，我们在最新的 Xeon6 + MRDIMM-8800 平台上测试了 `KTransformers v0.2.4`。通过提高并发度，模型的总输出吞吐量从 17 tokens/s 提升到了 40 tokens/s。我们观察到当前瓶颈已转移至 GPU，使用高于 4090D 的显卡预计还可以进一步提升性能。

#### 2. 引擎架构优化

借鉴 `sglang` 的调度框架，我们通过更新约 11,000 行代码，将 KTransformers 重构为一个更清晰的三层架构，并全面支持多并发：

- `Server`（服务层）：处理用户请求，并提供兼容 OpenAI 的 API。
- `Inference Engine`（推理引擎）：负责模型推理，支持分块预填充。
- `Scheduler`（调度器）：管理任务调度与请求编排。通过 FCFS（先来先服务）方式组织排队请求，打包为批次并发送至推理引擎，从而支持连续批处理。

![](img/6de1e7208e6f925c0eca79ec6b089014.png)

#### 3. 项目结构重组

所有 C/C++ 代码现已统一归类至 `/csrc` 目录下。

#### 4. 参数调整

我们移除了一些遗留和已弃用的启动参数，简化了配置流程。未来版本中，我们计划提供完整的参数列表和详细文档，以便用户更灵活地进行配置与调试。

---

接下来我们就尝试借助 Ktransformers v0.24 进行 DeepSeek-R1 和 V3 模型的本地安装部署。

### 3. Ktransformers 运行所需硬件要求

由于 Ktransformers 是采用 CPU+GPU 混合推理，因此对内存有较大要求，以下是不同量化模型在使用 Ktransformers 运行时的基本硬件要求：

![](img/d71fae61479b1017e179601462e353ba.png)

而如果是运行 DeepSeek-R1 `1.58bit` 模型（`IQ1_S`），则需要 14G 显存 + 120G 以上内存，而如果运行 `1.73bit` 模型（`IQ1_M`），则需要 14G 显存 + 140G 以上内存，而如果是 `2.22-bit`（`IQ2_XXS`），则需要 14G 显存 + 170G 内存，`2.51-bit`（`Q2_K_XL`）则需要 14G 显存 + 200G 内存。

| 模型名称 | 精度（比特） | 显存需求 | 内存需求 |
| --- | --- | --- | --- |
| `IQ1_S` | 1.58-bit | 14GB | ≥120GB |
| `IQ1_M` | 1.73-bit | 14GB | ≥140GB |
| `IQ2_XXS` | 2.22-bit | 14GB | ≥170GB |
| `Q2_K_XL` | 2.51-bit | 14GB | ≥200GB |
| `Q4_K_M` | 4-bit | 14GB | ≥382GB |
| `FP16` | FP16 | 14GB | 1TB |

> DeepSeek 模型组本地部署本地显存占用表：

![](img/39b19b6d240b10a88293d83aeaf7f357.png)

- 更多低成本部署方案 + 硬件采购避坑指南！：https://www.bilibili.com/video/BV1K7ACewEfM/

![](img/e7d9585ccc2d2cc8a3709fdba9e73d8c.png)

![](img/a316089ef22a93269a13f8d47d5c2d5f.png)

- 公开课部署模型：本节公开课以部署 1.58bit 模型为例进行演示，对应的部署流程可以拓展到任意精度的 DeepSeek-R1 模型（只需要下载对应的模型权重和配置即可）。
- 本节公开课配置：

  - `PyTorch 2.5.1`，`Python 3.12`(`ubuntu22.04`)，`Cuda 12.4`
  - 操作系统：`Ubuntu 22.04`
  - GPU：H800 * 2（实际只使用单卡 14G 显存）
  - CPU：40 vCPU Intel(R) Xeon(R) Platinum 8458P
  - 内存：200GB DDR4
- Ktransformers 部署方案核心性能瓶颈：但凡涉及到 CPU+GPU 混合推理，核心的性能瓶颈都是 CPU 和内存性能，Ktransformers 也不例外。
- 课件代码领取：

![](img/03ef64fd060c67b10567e0e3a8af1e9a.png)

## 二、 KTransformers v0.24 部署与调用流程

### 1. KTransformers 项目入门介绍

#### 1.1 项目定位

KTransformers（发音为 “Quick Transformers”）旨在通过先进的内核优化和计算分布/并行化策略来增强你使用 `Transformers` 的体验。

KTransformers 是一个灵活、以 `Python` 为中心的框架，其核心设计理念是可扩展性。用户仅需一行代码，即可实现优化模块的集成，并享受到以下特性：

- 与 `Transformers` 兼容的接口
- 符合 OpenAI 和 `Ollama` 规范的 RESTful API
- 一个简化版的 ChatGPT 风格 Web UI（最新版已弃用）

项目定位将 KTransformers 打造成一个灵活的平台，供用户探索和实验创新的大模型推理优化技术。因此，项目支持编写自定义脚本来实现模型权重的灵活卸载。

#### 1.2 项目参考资料

- GitHub 主页：https://github.com/kvcache-ai/ktransformers
- 项目使用指南：https://kvcache-ai.github.io/ktransformers/index.html

### 2. KTransformers v0.24 部署流程

KTransformers 支持在 `Windows`、`Linux` 等操作系统下，使用源码部署或者 `docker` 工具进行部署。

考虑到更为一般的企业级应用场景，本次实验采用 `Linux` 系统作为基础环境进行演示，并采用源码部署的方法进行部署。

#### 创建虚拟环境

```
conda create --name kt python=3.11
```

#### 安装基础依赖并验证

```
# 安装依赖库
```

```
# 验证 GLIBCXX 版本（应包含 3.4.32）
```

> 注意，这里要选择自己的 `anaconda` 或者 `miniconda` 的安装路径

![](img/06c5d312643e9ad7bf918d02b8a2db59.png)

#### 安装系统依赖

```
sudo apt update
```

> 注意，以上是一行命令

![](img/67b5ab84fbd518950d8ef760f0648677.png)

#### 构建 ktransformers

```
# 若实用 AutoDL，则需先开启学术加速
```

![](img/219bce19802e52b37e850e7af27ebad4.png)

其中 `submodule update` 命令是用于进行子模块的安装，`kt` 的子模块包括 `llama.cpp`、`prometheus-cpp` 等项目。等待安装完成后可在主目录下的 `third_party` 文件夹中看到：

![](img/a5b05eef6be0687baf7e1cdc94b134c1.png)

完整库

#### 设置 NUMA

为了确保 Ktransformers 能顺利运行，我们还需要设置单 NUMA 模式运行。

这里我们需要先确认下服务器上有几个 NUMA 节点（可以简单理解为有几块 CPU），我们可以通过如下命令进行查看

```
lscpu | grep "NUMA"
```

![](img/c91315c032aff511a5ab796a518e002c.png)

此时服务器上有两个 NUMA 节点。此外，不同的服务器也可能出现其他个数节点情况。

- 若是 `NUMA = 1`，运行以下命令：

```
sudo env USE_BALANCE_SERVE=1 PYTHONPATH="$(which python)"
```

- 若是 `NUMA > 1`，则运行以下命令：

```
# 先安装 libnuma-dev
```

其中运行的 `install.sh` 脚本如下：

![](img/477226fd934efef7d46340f097629366.png)

顺利运行后看到如下信息：

![](img/b56caeb5a7ae349dea76c136268af052.png)

需要注意的是，新版 Ktransformers 整个编译过程等待时间相对较长，需要几十分钟甚至是一个小时左右，请耐心等待编译完成。完成后看到如下图所示：

![](img/e886f63891fda7c259e5b9850448e61a.png)

> 注意，如果出现类似 `Target "cmTC_abfec" requires the language dialect "CUDA20", but CMake does not know the compile flags to use to enable it.` 报错，或者在编译的第三个阶段报错，则说明 `cmake` 版本太低，此时需要进行升级：

```
pip install cmake --upgrade
```

> 然后再次进行编译即可。

#### 查看 ktransformers 版本号

一切安装完成后，即可输入如下命令查看当前安装情况

```
pip show ktransformers
```

![](img/01b310387d25c4b06f408687d188f30f.png)

### 3. DeepSeek R1 模型权重与配置文件下载

本次实验使用官方推荐的 DeepSeek R1 `UD-IQ1_S`，直接使用 `Unsloth` 压制的模型即可，模型下载地址：

- 魔搭社区下载地址：https://www.modelscope.cn/models/unsloth/DeepSeek-R1-GGUF

![](img/d18a900b76ab0f50e31c1d819f44f27f.png)

![](img/39d6aec32322b6833a4753db18181a70.png)

- HuggingFace 下载地址：https://huggingface.co/unsloth/DeepSeek-R1-GGUF

![](img/561a06a9650789a50d8ca39b31d09bb9.png)

![](img/0300c5db5af8ebfa849096bfe77a116f.png)

模型权重较大，总共约 130G 左右。若使用 `HuggingFace` 进行下载，则需要一些网络工具。

> AutoDL 学术加速方法介绍：https://www.autodl.com/docs/network_turbo/

这里推荐使用魔搭社区进行下载，流程如下：

- 【可选】借助 `screen` 持久化会话



# 模型下载与KTransformer调用流程

## DeepSeek R1 模型权重与配置文件下载

由于实际下载时间可能持续2个小时，因此最好使用`screen`开启持久化会话，避免因为关闭会话导致下载中断。

创建一个名为`kt`的会话：

```
screen -S kt
```

之后哪怕关闭了当前会话，也可以使用如下命令恢复：

```
screen -r kt
```

> 若未安装`screen`，可以使用`sudo apt install screen`命令进行安装。

### 使用魔搭社区进行下载

使用`modelscope`进行权重下载，需要先安装魔搭社区：

```
pip install modelscope
```

然后输入如下命令进行下载：

```
mkdir ./DeepSeek-R1-GGUF
```

```
modelscope download --model unsloth/DeepSeek-R1-GGUF --include 'Q4_K_M' --local_dir /root/autodl-tmp/DeepSeek-R1-GGUF
```

![](img/9bcd10b0f7eeca0d34a3b01141d0f477.png)

相关文件可以在课件网盘中领取：

![](img/10bd29223e181e6ace28fffdb8fbdbd2.png)

![](img/f71485a3682629a05d2d47f9e6b34570.png)

### 下载DeepSeek R1原版模型的配置文件

此外，根据`KTransformer`的项目要求，还需要下载DeepSeek R1原版模型的除了模型权重文件外的其他配置文件，方便进行灵活的专家加载。因此我们还需要使用`modelscope`下载DeepSeek R1模型除了模型权重（`.safetensor`）外的其他全部文件，可以按照如下方式进行下载：

```
mkdir ./DeepSeek-R1
```

```
modelscope download --model deepseek-ai/DeepSeek-R1 --exclude '*.safetensors' --local_dir /root/autodl-tmp/DeepSeek-R1
```

![](img/d695dccd75a28c51c591bc618676f26e.png)

下载后完整文件如下所示：

![](img/12b20fcdfcc0aa38ddc916e06323d3e4.png)

相关文件也可以在课程课件中领取：

![](img/1ba0262c06e44bad1b5ff143d7e70a1b.png)

![](img/76ac32a0599d3d3a554ddeca2597874f.png)

### 成果汇总

这里最终我们是下载了`DeepSeek UD-IQ1_S`模型权重和DeepSeek R1的模型配置文件，并分别保存在两个文件夹中：

+   `DeepSeek R1 UD-IQ1_S`模型权重地址：`/root/autodl-tmp/DeepSeek-R1-GGUF/DeepSeek-R1-UD-IQ1_S`

![](img/d451013e522d052edf5c0ca8e89fd557.png)

+   DeepSeek R1的模型配置文件地址：`/root/autodl-tmp/DeepSeek-R1`

![](img/9801ecfb399777cef0d0f4ce4773edc6.png)

## DeepSeek V3-0324 模型权重与配置文件下载

接下来我们继续下载`DeepSeek-V3-0324`模型权重与配置文件。不同于`R1`模型是推理模型，`V3-0324`模型是对话模型，但由于其出色的Agent能力（`Function calling`能力），`V3-0324`模型在很多场景下也有很多实际应用需求。由于`R1`和`V3`模型架构完全相同，因此`Ktransformers`也是可以调用`V3`模型来进行响应。

本次实验使用官方推荐的`DeepSeek-V3-0324 Q2_K_XL`（1.58bit模型目前不太稳定）模型，该模型也是目前最稳定的动态量化模型，需要14G显存+170G内存即可调用。

+   [魔搭社区下载地址](https://www.modelscope.cn/models/unsloth/DeepSeek-V3-0324-GGUF/summary)

![](img/d5be272f23ddfe2f043ea1a176bf3953.png)

![](img/baef9a36113ed140df9041aaa3217e60.png)

+   [HuggingFace下载地址](https://huggingface.co/unsloth/DeepSeek-V3-0324-GGUF)

![](img/0546829c7de67722fd182c2da0c1ce27.png)

![](img/c2a1b396b7f18b781b995ea57068786b.png)

### 使用魔搭社区进行下载

输入如下命令进行下载：

```
mkdir ./DeepSeek-V3-0324-GGUF
```

```
modelscope download --model unsloth/DeepSeek-V3-0324-GGUF --include 'Q4_K_M' --local_dir /root/autodl-tmp/DeepSeek-V3-0324-GGUF
```

![](img/d2526bb4167f5d8c0e31a888612f0e31.png)

![](img/7cc71081b3e2c80a0229d86cf1d5b604.png)

### 下载DeepSeek V3原版模型的配置文件

```
mkdir ./DeepSeek-V3-0324
```

```
modelscope download --model deepseek-ai/DeepSeek-V3-0324 --exclude '*.safetensors' --local_dir /root/autodl-tmp/DeepSeek-V3-0324
```

![](img/98c8d850eba9a8f5730e3cf259808ae0.png)

下载后完整文件如下所示：

![](img/c2f1618e4b4ec9d77379f62842790f4e.png)

相关文件也可以在课程课件中领取：

![](img/cd8f789ef4b6192a9e84f300b2fdcfde.png)

![](img/00de16c1cc02d84f5b2892c543d99d1f.png)

### 成果汇总

这里最终我们是下载了`DeepSeek V3 Q2_K_XL`模型权重和DeepSeek V3的模型配置文件，并分别保存在两个文件夹中：

+   `DeepSeek V3 Q2`模型权重地址：`/root/autodl-tmp/DeepSeek-V3/UD-Q2_K_XL`

![](img/b8c964c738a4d79a1deca6ff91645aff.png)

+   `DeepSeek V3 IQ2`的模型配置文件地址：`/root/autodl-tmp/DeepSeek-V3-0324`

![](img/269c609fdd15c1a3c6fb3d67f02366b4.png)

## 三、KTransformer v0.24版调用DeepSeek运行流程

在安装完成了`KTransformer v0.24`，并下载好了模型权重和相应的模型配置之后，接下来即可尝试进行调用了。`KTransformer v0.24`支持两种调用方法，分别借助`local_chat.py`进行命令行本地对话，以及利用`server/main.py`开启服务，然后在默认`10002`端口进行`OpenAI`风格的API调用。这里我们重点尝试使用后端服务模式调用`DeepSeek`模型。

### DeepSeek-R1调用流程

部署完成后，即可使用如下命令开启后端服务，并在`10002`端口实现`OpenAI`风格API调用。这里需要输入如下命令：

```
python ktransformers/server/main.py \
```

等待响应如下：

![](img/03dd97c0c76c7635ad845ea4cc2bda53.png)

然后即可在命令行输入如下命令进行调用：

```
curl -X POST http://localhost:10002/v1/chat/completions \
```

即可获得如下响应：

![](img/cfeefb90fbd1026e28966a2dc4128629.png)

当然，我们也可以直接在Jupyter中尝试对其进行调用：

```
from openai import OpenAI

base_url="http://localhost:10002/v1")
model="DeepSeek-R1",
messages=[
    {"role": "user", "content": "你好，好久不见 !"}
]
```

响应结果如下所示：

![](img/d9a1e0960e1d879a1457d8419c22d5f9.png)

### DeepSeek-V3-0324 调用流程

可使用如下命令开启后端服务，并在`10002`端口实现`OpenAI`风格API调用。这里需要输入如下命令：

```
python ktransformers/server/main.py \
```

等待响应如下：

![](img/73eb91bd06c6f34222b7dd942f4e1e19.png)

然后即可在命令行输入如下命令进行调用：

```
curl -X POST http://localhost:10002/v1/chat/completions \
```

即可获得如下响应：

![](img/ddd5a29b20f9fac254205058940cae80.png)

当然，我们也可以直接在Jupyter中尝试对其进行调用：

```
from openai import OpenAI

base_url="http://localhost:10002/v1")
model="DeepSeek-V3-0324",
messages=[
    {"role": "user", "content": "你好，好久不见!"}
]
```

响应结果如下所示：

![](img/aa325a2a16987d4341be0322b9745f12.png)
