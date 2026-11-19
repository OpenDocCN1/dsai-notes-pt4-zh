
# Qwen-3高效微调实战（上）

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/SpdPdCwpcoNaLqxktiac1ff4nhg](https://kq4b3vgg5b.feishu.cn/docx/SpdPdCwpcoNaLqxktiac1ff4nhg)

## 课程说明

体验课内容节选自《2025大模型Agent智能体开发实战》(5月班)完整版付费课程

体验课时间有限，若想深度学习大模型技术，欢迎大家报名由我主讲的《2025大模型Agent智能体开发实战》(5月班)

![](img/114dd678e05e76567d07efe84afb0856.png)

![](img/5bdab2ece6cdf31d4e44b12fb1492974.png)

此外，公开课全套学习资料，已上传至网盘

链接: https://pan.baidu.com/s/1RPjCf3oatnN8paq8H2ioUg 提取码: 5g8z

需要更系统深入学习大模型可扫码⬆️添加助教咨询喔～

---

## 一、大模型微调技术快速入门

### 1. 微调基础概念介绍

#### 1.1 微调基本概念

所谓大模型微调，指的在已有的大规模预训练模型基础上，通过对标注数据进行训练，进一步优化模型的表现，以适应特定任务或场景的需求。不同于RAG或者Agent技术，通过搭建工作流来优化模型表现，微调是通过修改模型参数来优化模型能力，是一种能够让模型“永久”掌握某种能力的方法。

#### 1.2 全量微调与高效微调

而从方法的大类上来划分，微调又可以划分为全量微调：带入全部数据进行微调，和高效微调：只带入部分数据进行微调。毫无疑问，全量微调是一种算力消耗更大、但对模型的能力改造更为彻底的方法，而高效微调则更类似一种“四两拨千斤”的方法，通过修改模型部分参数，来调整模型整体能力。

#### 1.3 模型微调的优劣势分析

尽管模型微调能够通过修改模型参数的方式，永久的修改模型的能力，但这其实是一把双刃剑，如果处理不当，很可能造成模型原始能力的灾难性遗忘、即会导致模型原始能力丢失，对于复杂模型尤其如此。而为了能够满足微调最初目标，我们必须小心谨慎的设计模型微调数据集和微调训练流程，并经过反复多次训练验证，得到一个最佳模型。

#### 1.4 高效微调与`LoRA`、`QLoRA`

尽管全量微调可以对模型的能力进行深度改造，但要带入模型全部参数进行训练，需要消耗大量的算力，且有一定的技术门槛。相比之下，在绝大多数场景中，如果我们只想提升模型某个具体领域的能力，那高效微调会更加合适。尽管在2020年前后，深度学习领域诞生了很多高效微调的方法，但现在适用于大模型的最主流的高效微调方法只有一种——`LoRA`。

`LoRA`（Low-Rank Adaptation）微调是一种参数高效的微调方法，旨在通过引入低秩矩阵来减少微调时需要调整的参数数量，从而显著降低显存和计算资源的消耗。具体来说，`LoRA` 微调并不直接调整原始模型的所有参数，而是通过在某些层中插入低秩的适配器（Adapter）层来进行训练。

#### `LoRA`的原理

- 在标准微调中，我们会修改模型的所有权重，而在 `LoRA` 中，只有某些低秩矩阵（适配器）被训练和调整。这意味着原始模型的参数保持不变，只是通过少量的新参数来调整模型的输出。
- 低秩矩阵的引入可以在显存和计算能力有限的情况下，依然有效地对大型预训练模型进行微调，从而让 `LoRA` 成为显存较小的设备上的理想选择。

#### `LoRA`的优势

1. **显存优化：** 只需要调整少量的参数（适配器），显著减少了显存需求，适合显存有限的GPU。
1. **计算效率：** 微调过程中的计算负担也更轻，因为减少了需要调整的参数量。
1. **灵活性：** 可以与现有的预训练模型轻松结合使用，适用于多种任务，如文本生成、分类、问答等。

`QLoRA`（Quantized Low-Rank Adaptation）则是 `LoRA` 的一个扩展版本，它结合了 `LoRA` 的低秩适配器和量化技术。`QLoRA` 进一步优化了计算效率和存储需求，特别是在极端显存受限的环境下。与 `LoRA` 不同的是，`QLoRA` 会将插入的低秩适配器层的部分权重进行量化（通常是量化为 `INT4` 或 `INT8`），在保持性能的同时显著降低模型的存储和计算需求。

- **核心思想：** 在 `LoRA` 的基础上加入量化技术，减少权重表示的位数，从而降低显存和计算需求。`QLoRA` 结合了低秩适配器和量化的优点，能够在显存有限的设备上进行更高效的微调。
- **量化：** 通过将模型权重量化为低精度（如 `INT4`），减少内存占用，并提高推理和训练速度。
- 优势：
  - 在显存非常有限的情况下仍能进行微调。
  - 可以处理更大规模的模型。
  - 适合用于边缘设备和需要低延迟推理的场景。

`LoRA` 与 `QLoRA` 二者对比如下

| 特性 | `LoRA` | `QLoRA` |
| --- | --- | --- |
| **核心技术** | 低秩适配器（Low-Rank Adapters） | 低秩适配器 + 量化技术（Low-Rank Adapters + Quantization） |
| **适用场景** | 显存受限，但设备性能较好 | 极限显存受限或需要快速推理的设备 |
| **计算效率** | 提高计算效率，减少调整的参数数量 | 进一步提升效率，减少内存使用并加快推理速度 |
| **量化技术** | 无量化 | 将权重量化为低精度（如 `INT4` 或 `INT8`） |
| **内存消耗** | 较低，但不如 `QLoRA` 低 | 显著降低内存消耗，适合更小的设备 |
| **训练复杂度** | 较简单，适用于大多数微调场景 | 需要更多的量化和适配工作，但适合超大模型和设备受限场景 |

> 微调原理参考：【入门】大语言模型常用微调框架介绍|https://www.bilibili.com/video/BV1Yc411g78a/

![](img/a81a66f8e0321c0ee286a5e158f3a227.png)

### 2. 高效微调的应用场景

在实际大模型应用场景中，高效微调主要用于以下四个方面：

- **对话风格微调**：高效微调可以用于根据特定需求调整模型的对话风格。例如，针对客服系统、虚拟助理等场景，模型可以通过微调来适应不同的 语气、礼貌程度 或 回答方式，从而在与用户互动时提供更符合要求的对话体验。通过微调少量的参数（例如对话生成的策略、情感表达等），可以使模型表现出更具针对性和个性化的风格。
- **知识灌注**：知识灌注是指将外部知识或领域特定的信息快速集成到已有的预训练模型中。通过高效微调，模型可以更好地学习新领域的专有知识，而无需重新从头开始训练。例如，对于法律、医疗等专业领域，可以使用少量的标注数据对预训练模型进行微调，帮助模型理解特定行业的术语、规则和知识，进而提升专业领域的问答能力。
- **推理能力提升**：高效微调还可以用于提升大模型的推理能力，尤其是在处理更复杂推理任务时。通过微调，模型能够更加高效地理解长文本、推理隐含信息，或者从数据中提取逻辑关系，进而在多轮推理任务中提供更准确的答案。这种微调方式可以帮助模型在解答复杂问题时，提高推理准确性并减少错误。
- **Agent能力（`Function calling`能力、或者`MCP`能力）提升：**在多任务协作或功能调用场景中，高效微调能够显著提升模型的Agent能力，使得模型能够有效地与其他系统进行交互、调用外部`API`或执行特定任务。通过针对性微调，模型可以学会更精准的功能调用策略、参数解析和操作指令，从而在自动化服务、智能助手或机器人控制等领域表现得更加高效和智能。

### 3. 主流微调工具介绍

在入手学习大模型微调时，首先推荐功能层次封装层次较高的微调四套工具：`unsloth`、`LLaMA-Factory`、`ms-SWIFT`和`ColossalAI`。除此之外，也可以借助更加底层的库，如`peft`、`LoRA`、`transformers`等实现高效微调。对于初学者来说，首先使用现成工具来进行微调，四种工具基本说明如下。

#### 3.1 `unsloth`

![](img/70b662068efd7fa0f020714598dc16f3.png)

- `unsloth` GitHub主页：https://github.com/unslothai/unsloth

`unsloth` 是一个专为大型语言模型（LLM）设计的动态量化与微调框架，旨在提高微调效率并减少显存占用。 它通过手动推导计算密集型数学步骤并手写 GPU 内核，实现了无需硬件更改即可显著加快训练速度。

![](img/25be9c77dd30e761e48c5bf1e5962a42.png)

`unsloth` 与 HuggingFace 生态兼容，可以很容易地`transformers`、`peft`、`trl` 等库结合，以实现模型的监督微调（SFT）和直接偏好优化（DPO），仅需模型的加载方式，无需对现有训练代码进行修改。

- Unsloth动态量化模型：https://unsloth.ai/blog/dynamic-v2

![](img/5292f0de11b8f1d30bfcce8a336f19fe.png)

Unsloth 的动态量化方法，特别是其最新的 Dynamic 2.0 版本，旨在在尽量减少性能损失的同时显著压缩大型语言模型（LLMs）的体积。对于 Qwen3 模型，尤其是 4-bit 动态量化版本，现有的评测显示其性能下降非常有限，甚至在某些任务上与原始模型相当。具体评测结果如图所示：

![](img/a386014146026f020ca52e060d12e9ec.png)

而目前，关于 Qwen3 模型在 4-bit 动态量化下的具体性能下降数据尚不全面。然而，根据最近的一项研究，Qwen3 模型在4bit动态量化时，仅损失不到1%的性能。

![](img/01f1417059866740e7a04d74f07e5c49.png)

> 论文地址：https://arxiv.org/abs/2505.02214

这也使得Unsloth的动态量化模型成为个人配置下的最佳微调工具。

不过需要注意的是，动态量化由利也有弊，其好处在于可以极大程度压缩模型运行所需占用的显存大小，同时几乎不损失性能，但问题在于动态量化的模型，无论是推理还是微调，只能单卡运行，这就使得其吞吐量有限，无法在一台物理机上实现多GPU并行从而扩大吞吐量。

主要功能点：

- **高效微调：** `unsloth` 通过深度优化，使 LLM 的微调速度提高 2-5 倍，显存使用量减少约 80%，且准确度无明显下降。
- **广泛的模型支持：** 目前支持的模型包括目前各类主流模型，用户可以根据需求适合的模型进行微调。
- **兼容性：** `unsloth` 与 HuggingFace生态系统兼容，用户可以轻松将其与 `transformers`、`peft`、`l` 等库结合，实现模型的监督微调（SFT）和直接偏好优化（DPO），仅需修改模型的加载方式，无需对现有训练代码进行过多修改。
- **内存优化：** 通过 4 位和 16 位的 `QLoRA`/`LoRA` 微调，`unsloth` 显著了显存占用，使得在资源受限的环境中也能大的微调。

`unsloth`核心优势：

- **显著提升微调效率：** 相比传统方法，Unsloth采用独家4bit动态量化技术，能够在更短的时间内完成微调任务，节省时间成本。
- **降低硬件要求：** 通过优化显存使用，用户可以在显存较小的 GPU 上进行大模型的微调，降低了硬件门槛。
- **开源免费：** Unsloth 提供开源版本，用户可以在 Google Colab 或 Kaggle Notebooks 上免费试用，方便上手体验。

总的来说，`unsloth` 为大型语言模型的微调提供了高效、低成本的解决方案，适合希望在有限资源下进行模型微调的开发者和研究人员。

#### 3.2 `LLama-Factory`

![](img/3857577a610d4885bcd6db4030affbad.png)

- `LLaMA-Factory` GitHub主页：https://github.com/hiyouga/LLaMA-Factory

`LLaMA-Factory` 是一个统一且高效的微调框架，旨在为超过 100 种大型语言模型（LLMs）和视觉语言模型（VLMs）提供便捷的微调支持。 用户能够灵活地定制模型以适应各种下游任务。

主要功能和特点：

- **广型支持：** `LLaMA-Factory` 支持对 100 多`LLMs` 和 `VLMs` 进行微调，包括最新的模型版本，如 Llama 3、GLM-4、Mistral Small、PaliGemma2 等。
- **高效的微调方法：** 框架集成了多`nk Adaptation`）、`QRA`（Quantized `LoRA`）等，以提高训练速度并减少显存占用。
- **多模态任务支持：** 除了传统的文本任务外，`LLaMA-Factory` 还支频识别、音频理解等多种任务类型。
- **实验监控：** 提供了丰富的实验监控工具，如 `LlamaBoard`、`TensorBoard`、`Wandb`、`MLflow`、练过程。
- **快速：** 框架提供了类似 OpenAI 风格的 API、`Gradio` UI 和命令行界面，并结合 `vLLM` worker，实现了高效的推理能力。

#### 3.3 `ms-SWIFT`

![](img/ecfa84e11f5143ed803ee0478c6bd6c5.png)

- `ms-SWIFT` GitHub项目主页：https://github.com/modelscope/swift

`ms-swift`（Scalable lightWeight Infrastructure for Fine-Tuning）是由魔搭社区（ModelScope）开发的高效微调和部署框架，旨在为研究人员和开发者提供一站式的大模型与多模态大模型的训练、推理、评测、量化和部署解决方案。 的模型支持：** `ms-swift` 支持超过 450 种大型模型（LLMs）和 150 多种多模态大模型（MLLMs）的训练和部署**，包括最新的模型版本，如 Qwen2.5、InternLM3、GLM4、Llama3.3、Mistral、DeepSeek-R1、Yi1.5、Baichuan2、Gemma2 等，以及多模态模型如 Qwen2.5-VL、Qwen2-Audio、Llama3.2-Vision、Llava、InternVL2.5 等。

- **多样化的训练技术：** 框架集`oRA`、`Llama-Pro`、`LonoRA`、`GaLore`、`Q-GaLore`、`LoRA+`、`LISA`、`DoRA`、`FourierFt`、`ReFT`、`UnSloth` 和 `Liger` 等，满足不同的微调需求。
- **轻量级微调：** 支持多种轻量级微调方法，如 `LoRA`、`QLoRA`、`DoLLaMAPro`、`Adapt`、`GaLore`、`Q-Galore`、`LISA`、`UnSloth`、`Liger-Kernel` 等，降低显存和计算资源的消耗。
- **分布式训练：** 支持分布式数据并行（`DDP`）、`DeepSpeed ZeRO2`/`ZeRO3`、`FSDP` 等技术，提升推理加速：** 提供 `BNBWQ`、`GPTQ`、`AQLM`、`H🐧`、`EETQ` 等量化方法，并支持使用 `vLLM` 和 `LMDeploy` 对推理、评测和部署 支持图像、视频和语音等多种模态型训练，涵盖 VQA、Caption、OCR、Grounding 等任务。
- **用户友好的界面：** 提供基于 `Gradio` 的 We和量化操作，简化了大模型的全链路流程。

#### 3.4 `ColossalAI`

- `ColossalAI` GitHub项目主页：https://github.com/hpcaitech/ColossalAI

![](img/3fee6a7de1f0bdd610284e6b9be1fd7c.png)

`Colossal-AI` 是一个高效的分布式人工智能训练系统，旨在最大化提升人工智能训练效率，同时最小化训练成本。作为深度学习框架的内核，`Colossal-AI` 提供了自动超高维并行、大规模优化库、自适应任务调度、内存优化以及最新模型复现等前沿技术。与英伟达的 `Megatron-LM` 相比，`Colossal-AI` 仅需一半数量的 GPU 即可完成 `GPT-3` 训练，半小时内预训练 `ViT-Base/32`，并在两天内训练完 15 亿参数的 `GPT` 模型。此外，`Colossal-AI` 提供了多种并行技术，如数据并行、流水线并行和张量并行，以加速模型训练。 该项目自开源以来，迅速登上 GitHub 热榜，成为解放 AI 生产力的最佳选择。

并且，`ColossalAI`也是目前唯一支持`DeepSeek R1`非量化模型高效微调的框架，仅需4个节点、8卡`A100`服务器即可完成`DeepSeek R1`高效微调。

![](img/4ec9f4dfe42a7884ff28ec5bec313de0.png)

> 注一：若是强化学习训练，则推荐`veRL`和`OpenRLHF`等框架。

> 注二：其他更底层微调框架推荐

| 框架 | 优势 | 适用场景 |
| --- | --- | --- |
| `Hugging Face` | 高度兼容，易用，文档丰富 | 一般 NLP 任务，模型选择丰富 |
| `LoRA` | 显存节省，减少微调计算量 | 显存有限的设备，微调大规模模型 |
| `PEFT` | 高效微调，低计算开销 | 资源有限的环境，适合大规模预训练模型的微调 |
| `DeepSpeed` | 大规模分布式训练，显存优化 | 超大规模训练，多卡分布式训练 |
| `AdapterHub` | 低资源消耗，快速微调 | 多任务微调，资源有限的环境 |
| `Alpaca-LoRA` | 生成任务优化，`LoRA` 技术结合 | 对话生成、文本生成 |
| `FastChat` | 对话系统微调，快速集成 | 对话生成任务，尤其是对 `ChatGPT` 等模型微调 |
| `FairScale` | 大规模分布式训练优化，自动化优化 | 多卡分布式训练，大规模微调 |

### 4. 模型性能评估框架`EvalScope`



# EvalScope 模型评估框架介绍

+ 项目地址：`https://github.com/modelscope/evalscope`

![](img/2512e879227c37054d6948a47712673d.png)

`EvalScope` 是由阿里巴巴魔搭社区（ModelScope）推出的一款开源模型评估框架，旨在为大语言模型（LLM）和多模态模型提供统一、系统化的性能评估方案。该框架具备高度的自动化和可扩展性，适用于研究机构、工业界以及模型开发者在模型验证与性能对比场景中的广泛需求。

`EvalScope` 的核心功能和特点包括：

1.  **丰富的评测基准覆盖**：框架内置多种权威评测数据集，涵盖中英文通用知识问答（如 MMLU、CMMLU、C-Eval）、数学推理（如 GSM8K、MATH）、常识判断（如 HellaSwag、ARC）、代码生成（如 HumanEval）等多个方向，支持对模型能力进行多维度评估。

1.  **多样的评估模式支持**：`EvalScope` 提供三种灵活的评估模式，包括单模型评估模式（Single）、基于基线的两两对比模式（Pairwise-Baseline）、以及全模型两两对比模式（Pairwise-All），可满足从快速诊断到全面对比的不同使用场景。

1.  **统一的模型接入接口**：框架对不同类型的模型提供统一的调用方式，兼容 HuggingFace、本地部署模型及 API 远程调用，支持标准的 `generate` 与 `chat` 接口，大大降低了模型集成的复杂度。

1.  **评估流程高度自动化**：`EvalScope` 实现了评测任务的全自动执行，包括客观题自动打分、复杂问题使用评审模型辅助判定结果等，支持批量评估与日志记录，极大提升了评估效率与结果一致性。

1.  **完善的性能与能力可视化工具**：框架支持生成详细的评估报告和图表，展示模型在不同任务维度下的表现，便于开发者进行横向对比和性能分析。

1.  **多后端与评测能力扩展**：`EvalScope` 可集成多个评测后端，如 OpenCompass、VLMEvalKit、RAGEval 等，支持从单模态到多模态、从语言建模到 RAG 端到端评测的全链路能力。

1.  **支持部署性能测试**：除评估模型能力外，`EvalScope` 还提供服务端推理性能测试工具，涵盖吞吐量、响应时延等关键指标，帮助开发者评估模型的部署实用性。

公开课中我们将借助 `EvalScope` 进行模型微调前后的性能对比测试。

![](img/7e1ab6822a3c9f2b882f2ac0bb884f81.png)

## 5. 微调所需软硬件环境说明

大模型微调属于大模型进阶类技术，不同于普通的模型对话或搭建基础应用，微调往往需要一定的软硬件条件支持。

+ 大模型微调所需硬件一览

硬件方面，不同尺寸模型、不同精度微调时所需显存如下：

![](img/0a1f3de7b3fbd78a0db0675b403e053b.png)

> 更多大模型基础硬件配置要求，详见公开课《【2025版】大模型GPU硬件配置保姆级指南》|https://www.bilibili.com/video/BV1VFqZYSEoj

+ Qwen3 全系列模型微调所需显存一览

以下是 Qwen3 系列模型高效微调显存占用预估表：

| 模型名称 | 参数量 | FP16 微调显存占用 | 4-bit 动态量化微调显存占用 | 备注 |

| --- | --- | --- | --- | --- |

| Qwen3-0.6B | 0.6B | ~1.2 GB | ~0.5 GB | 可在低端 GPU 或 CPU 上运行 |

| Qwen3-1.7B | 1.7B | ~3.4 GB | ~1.5 GB | 适合入门级部署 |

| Qwen3-4B | 4B | ~8.0 GB | ~3.5 GB | 适合中等规模任务 |

| Qwen3-8B | 8B | ~16.0 GB | ~7.0 GB | 需要高端消费级 GPU |

| Qwen3-14B | 14B | ~28.0 GB | ~12.0 GB | 可在单张 RTX 4090 上微调 |

| Qwen3-30B-A3B (MoE) | 激活参数约 3B | ~85.0 GB | 暂不支持 | 激活部分专家参数，资源需求较高 |

| Qwen3-32B | 32B | ~65.0 GB | ~32.0 GB | 需要 A100/H100 或多卡并行 |

| Qwen3-235B-A22B (MoE) | 激活参数约 22B | ~600 GB | 暂不支持 | 超大模型，适合企业级部署，需高端服务器支持 |

公开课中将采用 H800 GPU 服务器为大家展示 Qwen3 32B 4bit 动态量化模型的高效微调流程，同学们可以根据自己实际硬件条件，选取不同模型进行微调，更换不同模型仅需要下载不同模型权重即可，其他流程完全一致。

> 注1：CPU 不能进行微调；

> 注2：目前 MoE 模型只支持 4bit 普通量化微调，暂不支持动态量化微调。

![](img/4696a4cbf77ef0ff061e9eb7e1280cd3.png)

+ 操作系统选择

而操作系统方面，由于绝大多数工业场景下微调会涉及多卡微调，目前只有 Linux 系统对 `DeepSpeed` 和其他多卡并行加速库支持较好，因此绝大多数工业场景下都会使用 Ubuntu 操作系统或 CentOS 操作系统。本节公开课我们以 Ubuntu 系统为例来进行高效微调。

若无相关软件环境，本节公开课的相关代码也可以在 Windows 下运行（本节微调示例不涉及多卡并行）。但若想体验更加真实的工业场景下的微调流程，也可以考虑在 AutoDL 上租赁显卡并配置 Ubuntu 服务器来完成操作，例如租赁 3090 显卡，每小时仅需 1.5 元，可以微调 Qwen3-14B 4bit 动态量化模型，运行两小时即可得到结果，仅需不到 5 元即可完成训练：

![](img/1094e869ebefdb3aae8771a89e8af53f.png)

> AutoDL 相关操作详见公开课：《AutoDL 快速入门与 GPU 租赁使用指南》|https://www.bilibili.com/video/BV1bxB7YYEST/

![](img/799556d6d2f10c4d2fc7655d4300b897.png)

## 6. 公开课课件领取

👇扫码即可领取全部课程资料👇

![](img/e0373f2de4e5a6012d779c30a1f67bd3.png)

![](img/147443e988f2431278960f52c000989b.png)

---

## 【专项提升】如何准备微调数据集

### Part 1. 原理篇

如何创建和选取模型微调数据集，是决定模型微调效果成败的最关键因素，截止目前，已经诞生了各类不同的微调框架和海量的微调数据集，在绝大多数情况下，我们只需要选择不同的微调框架并搭配不同的数据集即可。但伴随着模型能力越来越复杂，包括现阶段很多模型具备了 Function calling 功能，甚至是具备了推理或者混合推理能力，此时如果希望进行一些复杂功能模型的微调，例如围绕 Qwen3 模型进行 Function calling 能力微调、同时还需保留其混合推理能力，此时很多公开数据集或许就无法满足要求了。此外，如果我们希望给模型进行特定领域的知识关注，或者提升模型对于特殊工具组的工具调用准确率，此时就需要手动创建微调数据集了。

而要手动合并或者创建微调数据集，就必须深入了解微调数据集构造背后的原理。本小节内容，就为大家详细介绍创建微调数据集背后的底层原理。

+ 本节课件领取

![](img/c07fcfd98fa97ff1c7d35e8443ea3e1d.png)

#### 1. 模型内置特殊字符及提示词模板

其实最快速了解构造数据集的方法，是从模型底层原理入手。对于当代大模型来说，普遍需要通过一些特殊字符来标记用户的不同类型输入、系统提示词、以及工具调用或者多模态输入等。而在实际对话过程中，模型对于用户的输入输出是这么进行识别的（以 Qwen3 为例），一次简答的问答，模型的真实输入和输出如下所示：

![](img/a4a3a5353219f95c67e5c3b0f3f66881.png)

其中 `<|im_start|>` 代表文本开始，而 `user` 则代表消息身份，用于构建多轮对话，而 `<|im_end|>` 则代表文本结束，即用户输入结束，而 `<|im_start|>` 代表新一段文本开始，`assistant` 代表接下来由模型创建消息，而 `<|im_end|>` 同样代表模型创建消息的结束。

而模型其实是通过这样一组特殊字符标记来规范自己的行为，判断当前消息类型，以及通过输出特殊标记来确定停止时间。对于绝大多数模型，我们可以在模型的 `tokenizer_config.json` 中看到完整的特殊标记符（以及系统提示词模板）：

![](img/5625f9b2d722ec0f45673125d1683faf.png)

![](img/10881a2c62d35ce3066692d909bb2325.png)

而在实际微调过程中，我们都知道需要有监督的数据集、也就是需要输入 QA 对来进行微调。以著名的 alpaca_zh 中文微调数据集来说，其基本格式如下：

![](img/8f2c98522b6e05c351f99802e042926e.png)

其中的 `input` 和 `output` 就是输入和输出。例如如下对话：

![](img/95e7316cde77344124c0a8351cfa4830.png)

就可以表示为下列 json 格式数据集：

```
  {
    &quot;instruction&quot;: &quot;&quot;,
    &quot;input&quot;: &quot;输入：你好。&quot;,
    &quot;output&quot;: &quot;输出：你好，有什么可以帮到你的？&quot;
  },
```

而在真实的微调过程中，如果是针对 Qwen3 进行微调，微调脚本会将这条数据集（无论什么格式）转化为如下格式：

```
&lt;|im_start|&gt;user\n你好&lt;|im_end|&gt;\n&lt;|im_start|&gt;assistant\n你好，有什么可以帮到你的？&lt;|im_end|&gt;
```

而在实际训练过程中，模型就会根据 `assistant` 前的内容，学习 `assistant` 后面的输出内容。

#### 2. 带有系统提示和 Function calling 微调数据集格式

在很多场景下，我们还会发现一些带有 `instruction` 字段的微调数据集，那 `instruction` 字段是如何带入到微调过程中的呢？

![](img/2654aacb40a45680cf606e068af3f96c.png)

答案非常简单，还是依靠特殊字符。例如有一个对话内容如下：

+ 系统提示词（`instruction`）：你是一名助人为乐的助手。

+ 用户输入（`input`）：你好，好久不见。

+ 助手回复（`output`）：是的呀，好久不见，最近有什么有趣的事情要和我分享么？

此时模型的输入和输出如下：

```
&lt;|im_start|&gt;system
你是一名助人为乐的助手。&lt;|im_end|&gt;
&lt;|im_start|&gt;user
你好，好久不见。&lt;|im_end|&gt;
&lt;|im_start|&gt;assistant
是的呀，好久不见，最近有什么有趣的事情要和我分享么？&lt;|im_end|&gt;
```

即会通过 `<|im_start|>system...<|im_end|>` 来标记系统提示词。实际进行微调时，模型会根据 `assistant` 为界，学习 `assistant` 之前的文本输入情况下应该如何输出。

更进一步的，如果对话过程中带入了 Function calling，此时首先模型会读取提前准备好的 tool schema（也可能是自动生成的，例如 MCP 即可自动创建 tool schema）：

```
tool_schema = [{
    &quot;name&quot;: &quot;get_weather&quot;,
    &quot;description&quot;: &quot;查询指定城市的天气信息&quot;,
    &quot;parameters&quot;: {
        &quot;type&quot;: &quot;object&quot;,
        &quot;properties&quot;: {
            &quot;location&quot;: {
                &quot;type&quot;: &quot;string&quot;,
                &quot;description&quot;: &quot;要查询天气的城市名称&quot;
            }
        },
        &quot;required&quot;: [&quot;location&quot;]
    }
}]
```

而假设我们的对话内容如下：

+ 系统提示词（`instruction`）：你是一名助人为乐的助手。当用户查询天气的时候，请调用 `get_weather` 函数进行天气信息查询。

+ 用户输入（`input`）：你好，请帮我查询下北京天气。

+ 助手回复（`output`）：{"name": "get_weather", "arguments": {"location": "北京"}}

> 此时回复内容就是一条 Function call message

而此时模型真实的输入和输出内容如下：

```
&lt;|im_start|&gt;system
你是一名助人为乐的助手。当用户查询天气的时候，请调用 `get_weather` 函数进行天气信息查询。

# Tools

You may call one or more functions to assist with the user query.

You are provided with function signatures within &lt;tools&gt;&lt;/tools&gt; XML tags:
&lt;tools&gt;
{&quot;name&quot;: &quot;get_weather&quot;, &quot;description&quot;: &quot;查询指定城市的天气信息&quot;, &quot;parameters&quot;: {&quot;type&quot;: &quot;object&quot;, &quot;properties&quot;: {&quot;location&quot;: {&quot;type&quot;: &quot;string&quot;, &quot;description&quot;: &quot;要查询天气的城市名称&quot;}}, &quot;required&quot;: [&quot;location&quot;]}}
&lt;/tools&gt;

For each function call, return a json object with function name and arguments within &lt;tool_call&gt;&lt;/tool_call&gt; XML tags:
&lt;tool_call&gt;
{&quot;name&quot;: &lt;function-name&gt;, &quot;arguments&quot;: &lt;args-json-object&gt;}
&lt;/tool_call&gt;&lt;|im_end|&gt;
&lt;|im_start|&gt;user
你好，请帮我查询下北京天气。&lt;|im_end|&gt;
&lt;|im_start|&gt;assistant
&lt;tool_call&gt;
{&quot;name&quot;: &quot;get_weather&quot;, &quot;arguments&quot;: {&quot;location&quot;: &quot;北京&quot;}}
&lt;/tool_call&gt;&lt;|im_end|&gt;
```

接下来在进行训练时，模型同样根据 `assistant` 前的内容，学习 `assistant` 后面的输出内容。不过需要注意的是，由于高效微调调整的参数量较少，因此只能优化模型的 Function calling 能力，并不能从无到有让模型学会 Function calling。

#### 3. 带有思考过程的微调数据集结构

而如果是带有思考链，则一个简单的问答数据如下：

![](img/4ad3cca74dd6a2c3775068bbd5ae15ca.png)

+ 系统提示词（`instruction`）：你是一名助人为乐的助手。

+ 用户输入（`input`）：你好，好久不见。

+ 助手回复（`output`）：好的，用户发来“你好，好久不见！”，我需要回应。首先，用户可能希望得到亲切的回应，所以应该用友好的语气。/n是的呀，好久不见，最近有什么有趣的事情要和我分享么？

此时模型真实的内部输入和输出结果如下：

```
&lt;|im_start|&gt;system
你是一名助人为乐的助手。&lt;|im_end|&gt;
&lt;|im_start|&gt;user
你好，好久不见。&lt;|im_end|&gt;
&lt;|im_start|&gt;assistant
&lt;think&gt;
好的，用户发来“你好，好久不见！”，我需要回应。首先，用户可能希望得到亲切的回应，所以应该用友好的语气。
&lt;/think&gt;

是的呀，好久不见，最近有什么有趣的事情要和我分享么？&lt;|im_end|&gt;
```

模型同样根据 `assistant` 前的内容，学习 `assistant` 后面的输出内容。也就是说，所谓的思考过程，本质上其实是一种文本响应格式，通过模型训练而来。

最后难度升级，假设是带有思考过程、系统提示词的 Function calling 流程呢？此时一次对话的基本数据结构如下：

![](img/35174d3a7900452d38c44e5251ed4930.png)

内容如下：

+ 系统提示词（`instruction`）：你是一名助人为乐的助手。当用户查询天气的时候，请调用 `get_weather` 函数进行天气信息查询。



+ **用户输入（`input`）**：你好，请帮我查询下北京天气。

+ **助手回复（`output`）**：好的，用户问北京今天的天气，我应该尝试调用工具`get_weather`，并将参数设置为北京。`/n{"name": "get_weather", "arguments": {"location": "北京"}}`

而此时模型的真实输入和输出如下：

```
<|im_start|>system
你是一名助人为乐的助手。当用户查询天气的时候，请调用get_weather函数进行天气信息查询。

# Tools

You may call one or more functions to assist with the user query.

You are provided with function signatures within <tools></tools> XML tags:
<tools>
{"name": "get_weather", "description": "查询指定城市的天气信息", "parameters": {"type": "object", "properties": {"location": {"type": "string", "description": "要查询天气的城市名称"}}, "required": ["location"]}}
</tools>

For each function call, return a json object with function name and arguments within <tool_call></tool_call> XML tags:
<tool_call>
{"name": "<function-name>", "arguments": <args-json-object>}
</tool_call>
<|im_end|>
<|im_start|>user
你好，请帮我查询下北京天气。<|im_end|>
<|im_start|>assistant
<think>
好的，用户问北京今天的天气，我应该尝试调用工具 get_weather，并将参数设置为北京。
</think>

<tool_call>
{"name": "get_weather", "arguments": {"location": "北京"}}
</tool_call><|im_end|>
```

模型同样根据assistant前的内容，学习assistant后面的输出内容。由此可见，模型拥有不同功能的背后，其实源于不同格式的训练数据集的训练。而对于`Qwen3`这种模型来说，同时拥有`Function calling`、混合推理等功能，属于功能非常复杂的模型了。在实际微调过程中，稍有不慎就会令其丧失原有能力。

## 4. Qwen3混合推理模型构造微调数据集基本方法

在了解了微调数据集结构背后的基本原理后，接下来的问题是应该如何构造微调数据集呢？一般来说我们可以在`huggingface`、`ModelScope`或`llama-factory`中挑选合适的数据集，并根据实际情况进行组装。例如围绕`Qwen3`模型的高效微调，为了确保其仍然保留混合推理能力，我们可以考虑在微调数据集中加入如普通对话数据集`FineTome`（https://huggingface.co/datasets/mlabonne/FineTome-100k），以及带有推理字段的数学类数据集`OpenMathReasoning`（https://huggingface.co/datasets/nvidia/OpenMathReasoning），并围绕这两个数据集进行拼接，从而在确保能提升模型的数学能力的同时，保留非推理的功能。同时还需要在持续微调训练过程中不断调整`COT`数学数据集和普通文本问答数据集之间的配比，以确保模型能够在提升数学能力的同时，保留混合推理的性能。

![](img/d24fc4034e32272411a07d0b7d37caab.png)

此外，更多关于高效微调数据集选取和创建的相关内容，欢迎报名我主讲的《2025大模型Agent智能体开发实战》(5月班)：https://ix9mq.xetslk.com/s/3u765N。

![](img/4f7f9b1ba7819fe22f29f74923dc95bb.png)

![](img/eae84b346edc272dc0f0937b157b548e.png)

**其中将会有更加详细完整的微调全流程实战讲解**，详细信息扫码添加助教，回复“大模型”，即可领取课程大纲&查看课程详情⬆️

---

## 二、Qwen3模型高效微调环境准备

接下来开始进行模型高效微调前的环境准备工作。需要注意的是，为了更好的复现企业级模型微调全流程，本次公开课将需要使用如下四项工具：

+ **【必须】`Unsloth`**：高效微调框架，必须安装使用；

+ **【可选】`vLLM`**：模型调度框架，用于验证微调后模型效果，也可以使用`ollama`或者其他调度框架进行模型微调后效果验证；

+ **【可选】`EvalScope`**：模型评测框架，用于对比微调前后模型性能，也可以通过人工观察进行评估；

+ **【可选】`wandb`**：模型训练数据在线记录工具，用于保存模型训练过程中损失之的变化情况，并监控服务器硬件数据；

若想尽快完成微调，可以只安装`Unsloth`即可，若希望完整执行完微调、过程监督和效果测试各环节，则需要完整安装完各框架工具。

### 1. Unsloth安装部署

由于要安装多个项目，因此建议创建虚拟环境以避免依赖冲突。这里首先为`Unsloth`创建虚拟环境：

```
conda create --name unsloth python=3.11
conda init
source ~/.bashrc
conda activate unsloth
```

然后在虚拟环境中安装Jupyter及Jupyter Kernel：

```
conda install jupyterlab
conda install ipykernel
python -m ipykernel install --user --name unsloth --display-name "Python unsloth"
```

然后使用如下命令安装`Unsloth`：

```
pip install --upgrade --force-reinstall --no-cache-dir unsloth unsloth_zoo
```

![](img/54113218e614d7b114b71eebc726d74d.png)

安装完成后在任意Jupyter中选择`unsloth` kernel，即可进入对应的虚拟环境进行代码编写：

![](img/f91e7921d64b3e0c7baafb52bf71b4d9.png)

可以输入如下代码进行测试

```
from unsloth import FastLanguageModel
import torch
```

![](img/9b03dff474801a9bc93493ec227bab04.png)

### 2. vLLM安装部署流程

#### 2.1 vLLM安装部署

接下来继续安装`vLLM`，若是在AutoDL上租赁的服务器，则可以在默认环境中进行安装，其他服务器上建议创建虚拟环境进行安装。虚拟环境创建流程如下（AutoDL不用）：

```
conda create --name vllm python=3.11
conda init
source ~/.bashrc
conda activate vllm
```

`vllm`主要提供后台的模型调用服务，因此不用安装Jupyter Kernel。

接下来继续进行`vllm`安装：

```
pip install bitsandbytes>=0.45.3
pip install --upgrade vllm
```

> 注：`bitsandbytes`是为了适配4bit动态量化模型调用的库

#### 2.2 Qwen3模型权重下载

接下来进行模型权重下载，更多`Qwen3`模型全尺寸模型本地`ollama`、`vllm`、`llama.cpp`调用及部署方法，详见公开课：https://www.bilibili.com/video/BV1qVG9z4ELJ/

![](img/87ac89e32c9a9c28c0b8f5c083ec5d51.png)

及赋范大模型技术社区文档：

![](img/5c6e69bd522e69e93567845502ce3564.png)

![](img/042e249505634f95456437e8ead8fa15.png)

![](img/739f91d52be196de5708afb9a9ab9e26.png)

而要下载4bit动态量化的`Unsloth`模型，则可以在魔搭社区上搜索`Qwen3-unsloth-bnb-4bit`：

![](img/4a46a3a888ff08c45ba10d40d39e431d.png)

选择模型下载即可。需要注意的是，有时我们会遇到这两种不同的模型，其中带有`Unsloth`标志的是动态量化模型，而不带`unsloth`则是普通量化模型，需要注意区分。

![](img/98c5a5b046c5015dad2deccfda33e42e.png)

> 注，目前最新版`vLLM`已支持`Unsloth`动态量化模型，目前`Unsloth`团队已完成dense模型优化，MoE模型兼容`vLLM`版目前还未上线。即目前`vLLM`只支持`unsloth/Qwen3-1.7B-unsloth-bnb-4bit`、`nsloth/Qwen3-4B-unsloth-bnb-4bit`、`unsloth/Qwen3-8B-unsloth-bnb-4bit`、`unsloth/Qwen3-14B-unsloth-bnb-4bit`、`unsloth/Qwen3-32B-unsloth-bnb-4bit`四款模型。

然后安装魔搭社区工具并进行模型下载，以`Qwen3-32B-unsloth-bnb-4bit`为例（注，不同模型均可完成微调流程，大家根据自己硬件配置选择即可）：

https://www.modelscope.cn/models/unsloth/Qwen3-32B-unsloth-bnb-4bit/files

![](img/325711efc1053e7767dcabbf803f1488.png)

使用如下命令进行下载：

```
pip install modelscope
modelscope download --model unsloth/Qwen3-32B-unsloth-bnb-4bit --local_dir ./Qwen3-32B-unsloth-bnb-4bit
```

![](img/44684f8d4c2edfec6d52353198ee4a02.png)

下载后模型权重如图所示：

![](img/512d509c6a9d47542b26a24af216bef7.png)

![](img/0d395e2c0776ee883e4c70af4f309224.png)

#### 2.3 借助vllm进行模型调用

下载完模型权重后，接下来即可测试模型调用流程了，首先开启`vllm`服务，需要注意的是，`Unsloth`的动态量化模型只支持单卡调用，最低22G显存即可运行。因此这里是设置的单卡运行`vLLM`：

```
# cd /root/autodl-tmp/Qwen3
vllm serve ./Qwen3-32B-unsloth-bnb-4bit --enable-auto-tool-choice --tool-call-parser hermes
```

> 此外，更多关于`vLLM`调用`qwen3`参数说明，详见：https://qwen.readthedocs.io/en/latest/deployment/vllm.html#

开启后执行效果如下：

![](img/096e2800f9617be399ee2a2546abc045.png)

实际显存占用如下：

![](img/875f3dde7acc66f56fe91db0b1dbfbbe.png)

> 注，`vllm`会占用显卡的80%以上显存。实际调用32B 4bit动态量化模型时，最低22G显存即可调用，最低37G显存即可微调。

接下来则可在代码环境中进行调用测试：

```
from openai import OpenAI

openai_api_key = "EMPTY"
openai_api_base = "http://localhost:8000/v1"

client = OpenAI(
    api_key=openai_api_key,
    base_url=openai_api_base,
)

messages = [
    {"role": "user", "content": "你好，好久不见！"}
]

response = client.chat.completions.create(
    model="./Qwen3-32B-unsloth-bnb-4bit",
    messages=messages,
)

print(response.choices[0].message.content)
```

![](img/d9f276bd86e20d4d3c5e86f7eb1c5ebe.png)

### 3. EvalScope安装部署流程

#### 3.1 EvalScope安装流程

接下来进行`EvalScope`安装部署，项目官网：https://github.com/modelscope/evalscope

![](img/ba726c8df74edd376144d1592a6dfe1f.png)

为了避免和`Unsloth`的依赖产生冲突，`EvalScope`需要再单独创建虚拟环境：

```
conda create --name evalscope python=3.11
conda init
source ~/.bashrc
conda activate evalscope

conda install jupyterlab
conda install ipykernel
python -m ipykernel install --user --name evalscope --display-name "Python evalscope"
```

然后安装对应的库

```
pip install evalscope                # 安装 Native backend (默认)
# 额外选项
pip install 'evalscope[opencompass]'   # 安装 OpenCompass backend
pip install 'evalscope[vlmeval]'       # 安装 VLMEvalKit backend
pip install 'evalscope[rag]'           # 安装 RAGEval backend
pip install 'evalscope[perf]'          # 安装 模型压测模块 依赖
pip install 'evalscope[app]'           # 安装 可视化 相关依赖

# 或可以直接输入all，安装全部模块
# pip install 'evalscope[all]'           # 安装所有 backends (Native, OpenCompass, VLMEvalKit, RAGEval)
```

> 源码安装：

```
git clone https://github.com/modelscope/evalscope.git
cd evalscope/

pip install omegaconf

pip install -e .                  # 安装 Native backend
# 额外选项
pip install -e '.[opencompass]'   # 安装 OpenCompass backend
pip install -e '.[vlmeval]'       # 安装 VLMEvalKit backend
pip install -e '.[rag]'           # 安装 RAGEval backend
pip install -e '.[perf]'          # 安装 模型压测模块 依赖
pip install -e '.[app]'           # 安装 可视化 相关依赖
pip install -e '.[all]'           # 安装所有 backends (Native, OpenCompass, VLMEvalKit, RAGEval)
```

#### 3.2 借助EvalScope进行压力测试

紧接着，我们就可以尝试进行模型的压力测试，可以测试当前4bit动态量化模型在单卡H800上，由`vllm`调度框架驱动时的实际性能表现：



```
evalscope perf \
    --url "http://127.0.0.1:8000/v1/chat/completions" \
    --parallel 5 \
    --model ./Qwen3-32B-unsloth-bnb-4bit \
    --number 20 \
    --api openai \
    --dataset openqa \
    --stream
```

![](img/cb31c1a688184acef85a1e895c6f4d4a.png)

![](img/6bbcd4d539dff2c5f87b436281bae6d8.png)

输出结果保存在：`outputs/20250505_164404/Qwen3-32B-unsloth-bnb-4bit`:

![](img/57a12b2244332eba276bf0087fb42760.png)

然后即可在`summary.json`中查看压测的benchmark

```
{
    "Time taken for tests (s)": 632.0044,
    "Number of concurrency": 5,
    "Total requests": 20,
    "Succeed requests": 20,
    "Failed requests": 0,
    "Output token throughput (tok/s)": 45.5503,
    "Total token throughput (tok/s)": 46.4759,
    "Request throughput (req/s)": 0.0316,
    "Average latency (s)": 142.0679,
    "Average time to first token (s)": 0.2191,
    "Average time per output token (s)": 0.0994,
    "Average input tokens per request": 29.25,
    "Average output tokens per request": 1439.4,
    "Average package latency (s)": 0.0985,
    "Average package per request": 1439.4,
    "Expected number of requests": 20,
    "Result DB path": "outputs/20250505_164404/Qwen3-32B-unsloth-bnb-4bit/benchmark_data.db"
}
```

Benchmarking summary:

![](img/b5f463058bed33a32c62eefd4c7a79f4.png)

Percentile results:

![](img/18e489803a00d37070deeee33ea9b495.png)

压测结果解释：

### 📊 压测核心指标解读

| 指标项 | 含义 | 解读 |

| --- | --- | --- |

| `Time taken for tests (s)` | 总测试耗时 | 632 秒，表示本次压测运行了约 10 分钟。 |

| `Number of concurrency` | 并发数 | 使用了 5 个并发请求同时测试，表明模型支持一定程度的并发推理能力。 |

| `Total requests` | 总请求数 | 总共向模型发起了 20 次推理请求。 |

| `Succeed requests` / `Failed requests` | 成功与失败数 | 全部成功（20/20），说明模型稳定性良好。 |

### 🚀 吞吐性能

| 指标项 | 含义 | 解读 |

| --- | --- | --- |

| `Output token throughput (tok/s)` | 每秒生成 token 数 | 45.55 tok/s，生成速度在 4bit 动态量化大模型中属于正常偏低水平。 |

| `Total token throughput (tok/s)` | 总吞吐（包括输入+输出） | 46.48 tok/s，说明输入 token 占比较小，推理主要时间在输出部分。 |

| `Request throughput (req/s)` | 每秒处理请求数 | 0.0316 req/s，即大约 31 秒才完成一次请求（见下方 latency）。适用于高吞吐非交互式任务。 |

### ⏱️ 延迟分析

| 指标项 | 含义 | 解读 |

| --- | --- | --- |

| `Average latency (s)` | 单次请求平均耗时 | 142 秒，即每个请求耗时超过 2 分钟，说明单次生成量很大。 |

| `Average time to first token (s)` | 首 token 延迟 | 0.2191 秒，响应启动速度非常快，体现了量化模型的推理启动效率。 |

| `Average time per output token (s)` | 每个生成 token 的平均耗时 | 0.0994 秒，略高，意味着模型速度偏慢，可能与显存带宽或激活参数有关。 |

### 📦 Token 分布与批处理

| 指标项 | 含义 | 解读 |

| --- | --- | --- |

| `Average input tokens per request` | 每次输入平均 token 数 | 29.25，说明输入 `prompt` 很短。 |

| `Average output tokens per request` | 每次输出平均 token 数 | 1439.4，生成文本非常长，这解释了高 latency 与低 req/s。 |

| `Average package latency (s)` | 批处理延迟 | 0.0985 秒，表示模型每次接受请求到开始处理之间延迟极短。 |

| `Average package per request` | 每次请求中 token 包含数 | 同为 1439.4，表明未使用分片或 `chunk` 输出，模型按完整长度生成。 |

### 优点

+ 模型非常稳定（20 次请求 100% 成功）
+ 首 token 响应迅速（0.21 秒），表明部署结构良好
+ 适合长文本生成类任务

### 缺点

+ 整体吞吐较低（45 tok/s），属于 32B 大模型的正常范围
+ 单次推理耗时较长（每次平均 142 秒），适用于非实时场景。

### 3.3 进行模型性能评估

接下来即可进一步进行模型性能评估。这里我们先尝试对其进行初始状态下的性能评估，然后等微调结束后，再进行新一轮的评估，进而对比微调前后模型性能变化情况。这里需要在`Jupyter`中选择`evalscope` kernel：

![](img/74f545ffc5aa0f3669f77c9b8252060b.png)

然后运行如下代码：

【可选】数据集构造代码：

```
from evalscope.collections import CollectionSchema, DatasetInfo, WeightedSampler
from evalscope.utils.io_utils import dump_jsonl_data

schema = CollectionSchema(name='Qwen3', datasets=[
    CollectionSchema(name='English', datasets=[
        DatasetInfo(name='mmlu_pro', weight=1, task_type='exam', tags=['en'], args={'few_shot_num': 0}),
        DatasetInfo(name='mmlu_redux', weight=1, task_type='exam', tags=['en'], args={'few_shot_num': 0}),
        DatasetInfo(name='ifeval', weight=1, task_type='instruction', tags=['en'], args={'few_shot_num': 0}),
    ]),
    CollectionSchema(name='Chinese', datasets=[
        DatasetInfo(name='ceval', weight=1, task_type='exam', tags=['zh'], args={'few_shot_num': 0}),
        DatasetInfo(name='iquiz', weight=1, task_type='exam', tags=['zh'], args={'few_shot_num': 0}),
    ]),
    CollectionSchema(name='Code', datasets=[
        DatasetInfo(name='live_code_bench', weight=1, task_type='code', tags=['en'], args={'few_shot_num': 0, 'subset_list': ['v5_v6'], 'extra_params': {'start_date': '2025-01-01', 'end_date': '2025-04-30'}}),
    ]),
    CollectionSchema(name='Math & Science', datasets=[
        DatasetInfo(name='math_500', weight=1, task_type='math', tags=['en'], args={'few_shot_num': 0}),
        DatasetInfo(name='aime24', weight=1, task_type='math', tags=['en'], args={'few_shot_num': 0}),
        DatasetInfo(name='aime25', weight=1, task_type='math', tags=['en'], args={'few_shot_num': 0}),
        DatasetInfo(name='gpqa', weight=1, task_type='knowledge', tags=['en'], args={'subset_list': ['gpqa_diamond'], 'few_shot_num': 0})
    ])
])

# get the mixed data
mixed_data = WeightedSampler(schema).sample(100000000)  # set a large number to ensure all datasets are sampled
# dump the mixed data to a jsonl file
dump_jsonl_data(mixed_data, 'outputs/qwen3_test.jsonl')
```

创建好的完整数据集已上传至课件网盘：

![](img/d6e30320fa94dba653291bcbfb65c4d8.png)

![](img/1f88941a238a8ac87081dcf3558fca16.png)

![](img/d3e4f1ca0cc86bbca4cde41e7b8a58b8.png)

评测代码：

```
from evalscope import TaskConfig, run_task
task_cfg = TaskConfig(
    model='./Qwen3-32B-unsloth-bnb-4bit',
    api_url='http://127.0.0.1:8000/v1/chat/completions',
    eval_type='service',
    datasets=[
        'data_collection',
    ],
    dataset_args={
        'data_collection': {
            'dataset_id': 'modelscope/EvalScope-Qwen3-Test',
            'filters': {'remove_until': '</think>'}  # 过滤掉思考的内容
        }
    },
    eval_batch_size=128,
    generation_config={
        'max_tokens': 30000,  # 最大生成token数，建议设置为较大值避免输出截断
        'temperature': 0.6,  # 采样温度 (qwen 报告推荐值)
        'top_p': 0.95,  # top-p采样 (qwen 报告推荐值)
        'top_k': 20,  # top-k采样 (qwen 报告推荐值)
        'n': 1,  # 每个请求产生的回复数量
    },
    timeout=60000,  # 超时时间
    stream=True,  # 是否使用流式输出
    limit=2000,  # 设置为2000条数据进行测试
)

run_task(task_cfg=task_cfg)
```

这里采用了EvalScope专门为Qwen3准备的`modelscope/EvalScope-Qwen3-Test`数据集进行评测，会围绕模型的推理、指令跟随、代理能力和多语言支持方面能力进行测试，该数据包含`mmlu_pro`、`ifeval`、`live_code_bench`、`math_500`、`aime24`等各著名评估数据集。

数据集地址：https://modelscope.cn/datasets/modelscope/EvalScope-Qwen3-Test/summary

![](img/80acbd1c3f3e113d25b27059a957d253.png)

评测运行结果如下：

![](img/29d72001b4a5cdf42a164aac3f88e86d.png)

![](img/79d394794986884993be4336b57d3969.png)

此时后台显示如下：

![](img/0e48737cb2dfb9ed16fa1b03f574b615.png)

查看模型能力评测结果

```
evalscope app
```

![](img/2d2b4a8c3aa453a63f34027c23c930ce.png)

对评测结果进行可视化：

![](img/2206453852ec720455a767675675d1c6.png)

![](img/ccab2bd8d9fb45275550382bbe21159d.png)

![](img/d17be66a1cdb46688ea08360572d026f.png)

### 4. wandb安装与注册

#### 4.1 wandb基本说明

在大规模模型训练中，我们往往需要监控和分析大量的训练数据，而WandB可以帮助我们实现这一目标。它提供了以下几个重要的功能：

**实时可视化**：WandB可以实时展示训练过程中关键指标的变化，如损失函数、学习率、训练时间等。通过这些可视化数据，我们能够直观地了解模型的训练进展，快速发现训练中的异常或瓶颈。

**自动记录与日志管理**：WandB会自动记录每次实验的参数、代码、输出结果，确保实验结果的可追溯性。无论是超参数的设置，还是模型的架构调整，WandB都能够帮助我们完整保留实验记录，方便后期对比与调优。

**支持中断与恢复训练**：在长时间的预训练任务中，系统中断或需要暂停是常见的情况。通过WandB的checkpoint功能，我们可以随时恢复训练，从上次中断的地方继续进行，避免数据和时间的浪费。



**多实验对比**：当我们尝试不同的模型配置或超参数时，`WandB`允许我们在多个实验之间轻松进行对比分析，帮助我们选择最优的模型配置。

**团队协作**：`WandB`还支持团队协作，多个成员可以共同查看实验结果，协同调试模型。这对研究和项目开发中团队的合作非常有帮助。

## 4.2 `wandb`注册与使用

`wandb`官网：`https://wandb.ai/site`

![](img/c2ec64ffe557e06a079a709c780e64d5.png)

![](img/2ab7a64eaf6eed3980b6b025bca19d04.png)

![](img/1f15e6657accb803ab59c5464907030e.png)

然后即可在令行中输入如下代码安装`wandb`：

```
pip install wandb
```

![](img/5ca6708bbf97d588f112c4e35ec962a5.png)

接下来在`unsloth`微调前，我们即可设置`wandb`进行微调记录，并可在对应网站上观察到训练过程如下：

![](img/4ea3a9d1a2a43869b1f95b6cd00f4472.png)

## 三、`Qwen3`混合推理模型微调数据集准备

在搭建好了基础环境后，接下来即可开始着手准备`Qwen3`微调数据集了。围绕`Qwen3`模型的高效微调，为了确保其仍然保留混合推理能力，我们可以考虑在微调数据集中加入如普通对话数据集`FineTome`（`https://huggingface.co/datasets/mlabonne/FineTome-100k`），以及带有推理字段的数学类数据集`OpenMathReasoning`（`https://huggingface.co/datasets/unsloth/OpenMathReasoning-mini`），并围绕这两个数据集进行拼接，从而在确保能提升模型的数学能力的同时，保留非推理的功能。同时还需要在持续微调训练过程中不断调整`COT`数学数据集和普通文本问答数据集之间的配比，以确保模型能够在提升数学能力的同时，保留混合推理的性能。

### 1. `OpenMathReasoning` 数据集（AIMO-2）

+   数据集地址：`https://huggingface.co/datasets/unsloth/OpenMathReasoning-mini`

![](img/215ee152c13a114764e23043a4c572bf.png)

**数据集介绍：**`OpenMathReasoning` 是为 AI Mathematical Olympiad - Progress Prize 2（AIMO-2）竞赛开发的高质量数学推理数据集。该数据集包含：

+   540,000 道独特的高质量数学问题，涵盖代数、组合、几何和数论等领域；

+   3,200,000 条详细的长推理解答；

+   1,700,000 条工具集成推理（`Tool-Integrated Reasoning`）解答，结合了代码执行与推理过程；

+   生成式解答选择（`GenSelect`） 方法，用于从多个候选解答中选择最优解。

该数据集旨在推动 AI 在复杂数学推理任务中的能力提升，已被用于训练在多个数学推理基准上表现优异的模型。

+   数据集格式

`OpenMathReasoning`包含`expected_answer`、`problem`和`generated_solution`三个核心字段，分别代表问题答案、问题和模型思考过程。例如其中一条数据内容如下：

问题：

```
设有一组共 NN 个球，由 CC 种颜色组成，其中每种颜色的球的数量分别为 n1,n2,…,nCn_1, n_2, \ldots, n_C（满足 n1+n2+…+nC=Nn_1 + n_2 + \ldots + n_C = N）。如果从中随机抽取 AA 个球（其中 A≤NA \leq N），那么抽到的球中恰好有 a1,a2,…,aCa_1, a_2, \ldots, a_C 个分别对应每种颜色（满足 a1+a2+…+aC=Aa_1 + a_2 + \ldots + a_C = A），这种情况发生的概率是多少？
```

答案：

思考过程：

```
好的，也就是说我要计算这样一个概率：从总共 $N$ 个球中随机抽取 $A$ 个球，其中球分为 $C$ 种颜色，要求抽到的每种颜色球的数量恰好是 $a_1, a_2, \ldots, a_C$。嗯，我们来想想该怎么求解这个问题。

首先，概率问题通常会涉及**组合数**。概率的一般公式是：
**有利情况数 ÷ 所有可能情况总数**。

在本题中，“有利情况”就是指：恰好抽到 $a_1$ 个颜色1的球，$a_2$ 个颜色2的球，依此类推，一直到颜色 $C$。
而“所有可能情况”则是：从 $N$ 个球中任意抽取 $A$ 个球的方式，不考虑颜色分布。

我们可以这样分解这个问题：

* 从 $N$ 个球中任意抽取 $A$ 个球的总方式数是组合数：

  $$
  C(N, A) = \binom{N}{A} = \frac{N!}{A!(N - A)!}
  $$

* 而有利情况则是：从每种颜色中选出指定数量的球。比如从 $n_1$ 个颜色1的球中选出 $a_1$ 个，从 $n_2$ 个颜色2的球中选出 $a_2$ 个，依此类推。
  由于每种颜色之间的选择是相互独立的，所以所有组合方式的总数就是各个颜色组合数的乘积：

  $$
  \binom{n_1}{a_1} \times \binom{n_2}{a_2} \times \cdots \times \binom{n_C}{a_C}
  ```

不过，要注意前提条件必须满足：

* 每个 $a_i \leq n_i$，也就是说不能从某种颜色中选出比它实际数量更多的球；
* 同时 $a_1 + a_2 + \cdots + a_C = A$，总共抽取的球数必须正确。

如果这些条件满足，那么这种组合方式就是有效的。如果不满足，比如某个 $a_i &gt; n_i$，则该组合数为0，概率也自然为0，这是符合逻辑的。

其实这正是一个**多项超几何分布（multivariate hypergeometric distribution）**问题。

超几何分布描述的是：从有限总体中不放回地抽取样本，得到某个类别成功次数的概率。多项超几何分布是其推广，适用于多个类别的情况，正好契合这里的“多种颜色”的设定。

该分布的概率公式就是：

$$
P = \frac{\binom{n_1}{a_1} \cdot \binom{n_2}{a_2} \cdots \binom{n_C}{a_C}}{\binom{N}{A}}
$$

---

### 最终结论：

当从 $N$ 个球（含有 $C$ 种颜色，每种颜色分别有 $n_1, n_2, \ldots, n_C$ 个球）中随机抽取 $A$ 个球时，恰好抽中每种颜色 $a_1, a_2, \ldots, a_C$ 个球的概率为：

$$
\boxed{
P = \frac{\prod_{i=1}^{C} \binom{n_i}{a_i}}{\binom{N}{A}}
}
$$

这个公式就是多项超几何分布的概率表达式，分子为每种颜色的有利组合方式之乘积，分母为所有可能抽取 $A$ 个球的方式总数。
```

### 2. `FineTome-100k` 数据集（Maxime Labonne）

+   数据集地址：`https://huggingface.co/datasets/mlabonne/FineTome-100k`

![](img/61269a769574171a56b1de6052ec7aea.png)

**数据集简介：**`FineTome-100k` 是由 Maxime Labonne 创建的高质量多轮对话数据集，采用 `ShareGPT` 风格，适用于大语言模型的微调。该数据集特点包括：

+   100,000 条多轮对话样本；

+   数据以 `JSONL` 格式存储，每条记录包含一个 `conversations` 字段，记录对话的完整历史；

+   对话格式类似于 `ShareGPT`，适合训练模型进行多轮对话；

+   可转换为 Hugging Face 通用的多轮对话格式，以适配不同的训练框架。

![](img/0e7321f2efacfef7a43aeb7fa2edcd0f.png)

![](img/adfdf805bcdc1d95f37f3daf158ec3de.png)

接下来即可上手使用`Unsloth`进行高效微调了。

---

+   体验课内容节选自[《2025大模型Agent智能体开发实战》(5月班)]完整版付费课程

体验课时间有限，若想深度学习大模型技术，欢迎大家报名由我主讲的《2025大模型Agent智能体开发实战》(5月班)

![](img/3fbbc0d8a6e8a80d2dbedb710cb81cd8.png)

5月班目前上新特惠中，合购还有更多优惠哦~详细信息扫码添加助教，回复“大模型”，即可领取课程大纲&查看课程详情👇

![](img/10ec7c9cf6d200bb1b8a6c6151ebdf33.png)
