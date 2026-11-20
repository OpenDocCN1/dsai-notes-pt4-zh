
![](img/9b3dd6e16d3809125f0d091fb2c04d98.png)

### 4.1 unsloth

#### 主要功能点

+ **高效微调：** `unsloth` 通过深度优化，使 LLM 的微调速度提高 2-5 倍，显存使用量减少约 80%，且准确度无明显下降。

+ **广泛的模型支持：** 目前支持的模型包括各类主流模型，用户可以根据需求选择合适的模型进行微调。

+ **兼容性：** `unsloth` 与 Hugging Face 生态系统兼容，用户可以轻松将其与 `transformers`、`peft`、`trl` 等库结合，实现模型的监督微调（SFT）和直接偏好优化（DPO），仅需修改模型的加载方式，无需对现有训练代码进行过多修改。

+ **内存优化：** 通过 4 位和 16 位的 `QLoRA`/`LoRA` 微调，`unsloth` 显著降低了显存占用，使得在资源受限的环境中也能进行大规模微调。

#### unsloth 核心优势

+ **显著提升微调效率：** 相比传统方法，`Unsloth` 能够在更短的时间内完成微调任务，节省时间成本。

+ **降低硬件要求：** 通过优化显存使用，用户可以在显存较小的 GPU 上进行大模型的微调，降低了硬件门槛。

+ **开源免费：** `Unsloth` 提供开源版本，用户可以在 Google Colab 或 Kaggle Notebooks 上免费试用，方便上手体验。

总的来说，`unsloth` 为大型语言模型的微调提供了高效、低成本的解决方案，适合希望在有限资源下进行模型微调的开发者和研究人员。

### 4.2 LLaMA-Factory

![](img/3dde160fc89fd9db585b1e3b4a68586c.png)

+ [LLaMA-Factory GitHub](https://github.com/hiyouga/LLaMA-Factory)

LLaMA-Factory 是一个统一且高效的微调框架，旨在为超过 100 种大型语言模型（LLMs）和视觉语言模型（VLMs）提供便捷的微调支持。用户能够灵活地定制模型以适应各种下游任务。

#### 主要功能和特点

+ **广型支持：** `LLaMA-Factory` 支持对 100 多 LLMs 和 VLMs 进行微调，包括最新的模型版本，如 `Llama 3`、`GLM-4`、`Mistral Small`、`PaliGemma2` 等。

+ **高效的微调方法：** 框架集成了多种高效微调技术，如 `LoRA`（Low-Rank Adaptation）、`QLoRA`（Quantized LoRA）等，以提高训练速度并减少显存占用。

+ **多模态任务支持：** 除了传统的文本任务外，`LLaMA-Factory` 还支持图像识别、音频理解等多种任务类型，适应更广泛的多模态应用场景。

+ **实验监控：** 提供了丰富的实验监控工具，如 `LlamaBoard`、`TensorBoard`、`Wandb`、`MLflow` 等，帮助开发者实时监控训练过程，确保训练过程的高效和透明。

+ **快速：** 框架提供了类似 OpenAI 风格的 API、`Gradio UI` 和命令行界面，并结合 `vLLM` worker，实现了高效的推理能力。

### 4.3 ms-SWIFT

![](img/462eca2448ab02905fc424fa473c208f.png)

+ `ms-SWIFT` GitHub 项目主页：https://github.com/modelscope/swift

`ms-swift`（Scalable lightWeight Infrastructure for Fine-Tuning）是由魔搭社区（ModelScope）开发的高效微调和部署框架，旨在为研究人员和开发者提供一站式的大模型与多模态大模型的训练、推理、评测、量化和部署解决方案。`ms-swift` 支持超过 450 种大型模型（LLMs）和 150 多种多模态大模型（MLLMs）的训练和部署，包括最新的模型版本，如 `Qwen2.5`、`InternLM3`、`GLM4`、`Llama3.3`、`Mistral`、`DeepSeek-R1`、`Yi1.5`、`Baichuan2`、`Gemma2` 等，以及多模态模型如 `Qwen2.5-VL`、`Qwen2-Audio`、`Llama3.2-Vision`、`Llava`、`InternVL2.5` 等。

+ **多样化的训练技术：** 框架集成了 `oRA`、`Llama-Pro`、`LonoRA`、`GaLore`、`Q-GaLore`、`LoRA+`、`LISA`、`DoRA`、`FourierFt`、`ReFT`、`UnSloth` 和 `Liger` 等，满足不同的微调需求。

+ **轻量级微调：** 支持多种轻量级微调方法，如 `LoRA`、`QLoRA`、`DoLLaMAPro`、`Adapt`、`GaLore`、`Q-Galore`、`LISA`、`UnSloth`、`Liger-Kernel` 等，降低显存和计算资源的消耗。

+ **分布式训练：** 支持分布式数据并行（`DDP`）、`DeepSpeed ZeRO2`/`ZeRO3`、`FSDP` 等技术，提升推理加速：提供 `BNBWQ`、`GPTQ`、`AQLM`、`EETQ` 等量化方法，并支持使用 `vLLM` 和 `LMDeploy` 对推理、评测和部署支持图像、视频和语音等多种模型训练，涵盖 `VQA`、`Caption`、`OCR`、`Grounding` 等任务。

+ **用户友好的界面：** 提供基于 `Gradio` 的 Web 界面和量化操作，简化了大模型的全链路流程。

> 注一：若是强化学习训练，则推荐 `veRL` 和 `OpenRLHF` 等框架。

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

### 5. 模型微调所需硬件与服务器环境搭建

大模型微调属于大模型进阶类技术，不同于普通的模型对话或搭建基础应用，微调往往需要一定的软硬件条件支持。

#### 大模型微调所需硬件一览

硬件方面，不同尺寸模型、不同精度微调时所需显存如下：

![](img/cd18cb88ef6543188639ae617bde5343.png)

接下来我们将以 8B 模型为例进行高效微调，最低 INT4 情况下仅需 7G 显存即可运行。

更多大模型基础硬件配置要求，详见公开课《【2025 版】大模型 GPU 硬件配置保姆级指南》（https://www.bilibili.com/video/BV1VFqZYSEoj）。

#### 操作系统选择

而操作系统方面，由于绝大多数工业场景下微调会涉及多卡微调，目前只有 Linux 系统对 `DeepSpeed` 和其他多卡并行加速库支持较好，因此绝大多数工业场景下都会使用 Ubuntu 操作系统或 CentOS 操作系统。本节公开课我们以 Ubuntu 系统为例来进行高效微调。

若无相关软件环境，本节公开课的相关代码也可以在 Windows 下运行（本节微调示例不涉及多卡并行）。但若想体验更加真实的工业场景下的微调流程，也可以考虑在 AutoDL 上租赁显卡并配置 Ubuntu 服务器来完成操作。最小化实现微调效果，仅需单卡 3090 运行两小时即可得到结果，仅需不到 5 元即可完成训练：

![](img/be112fd13df54314a3e06b3f69d004a9.png)

AutoDL 相关操作详见公开课：《AutoDL 快速入门与 GPU 租赁使用指南》（https://www.bilibili.com/video/BV1bxB7YYEST）。

![](img/a7751103062550602ab46ed5a60527e1.png)

更多大模型微调技术介绍，欢迎报名由我主讲的《2025 大模型 Agent 智能体开发实战》（2 月 DeepSeek 强化班）https://whakv.xetslk.com/s/1tKbjV 进行更深度系统的学习哦~

## 二、DeepSeek R1 Distill 高效微调环境准备

### 1. unsloth 安装部署

`unsloth` 是推理、微调一体式框架，可以使用如下命令快速安装：

```
pip install unsloth
```

![](img/b921f43fe9faee587454d39488685700.png)

注意，这里的第二行、第三行是一条命令，只是换行了而已，需要复制在一起然后运行。

而如果是使用 AutoDL 且 Github 网速不稳，则可以使用如下命令开启 AutoDL 学术加速：

```
source /etc/network_turbo
```

然后再使用上面的安装命令进行安装。

[AutoDL 学术资源加速说明：https://www.autodl.com/docs/network_turbo/](https://www.autodl.com/docs/network_turbo/)

### 2. wandb 安装与注册

#### 2.1 wandb 基本说明

在大规模模型训练中，我们往往需要监控和分析大量的训练数据，而 `WandB` 可以帮助我们实现这一目标。它提供了以下几个重要的功能：

**实时可视化**：`WandB` 可以实时展示训练过程中关键指标的变化，如损失函数、学习率、训练时间等。通过这些可视化数据，我们能够直观地了解模型的训练进展，快速发现训练中的异常或瓶颈。

**自动记录与日志管理**：`WandB` 会自动记录每次实验的参数、代码、输出结果，确保实验结果的可追溯性。无论是超参数的设置，还是模型的架构调整，`WandB` 都能够帮助我们完整保留实验记录，方便后期对比与调优。

**支持中断与恢复训练**：在长时间的预训练任务中，系统中断或需要暂停是常见的情况。通过 `WandB` 的 `checkpoint` 功能，我们可以随时恢复训练，从上次中断的地方继续进行，避免数据和时间的浪费。

**多实验对比**：当我们尝试不同的模型配置或超参数时，`WandB` 允许我们在多个实验之间轻松进行对比分析，帮助我们选择最优的模型配置。

**团队协作**：`WandB` 还支持团队协作，多个成员可以共同查看实验结果，协同调试模型。这对研究和项目开发中团队的合作非常有帮助。

#### 2.2 wandb 注册与使用

https://wandb.ai/site

![](img/77339282db27a73c65606f0c73284539.png)

![](img/82ec2edd46383745c8845c371c746a86.png)

![](img/85e7f60ed61ddabaa0156a5a3cd6fad9.png)

然后即可在令行中输入如下代码安装 `wandb`：

```
pip install wandb
```

![](img/c4c64348a7ce619f515ee78f3892f21d.png)

接下来在 `unsloth` 微调前，我们即可设置 `wandb` 进行微调记录，并可在对应网站上观察到训练过程如下：

![](img/28d729f43912b83ed371ce6e8069fa10.png)

### 3. DeepSeek R1 模型下载

本次实验我们将同时介绍 DeepSeek R1 Qwen 蒸馏模型和 LLama 蒸馏模型的高效微调流程，因此需要提前进行模型下载。考虑到国内网络环境，我们可以使用 ModelScope 进行下载，这里首先需要安装 `ModelScope`：

```
pip install modelscope
```

#### 3.1 DeepSeek R1 Distill Qwen 7B 模型下载

ModelScope 模型地址：https://www.modelscope.cn/models/deepseek-ai/DeepSeek-R1-Distill-Qwen-7B

![](img/73a06ee229d00315e262871f687e15b4.png)

然后创建 `DeepSeek-R1-Distill-Qwen-7B` 文件夹，用于保存模型权重：

```
mkdir ./DeepSeek-R1-Distill-Qwen-7B
```

然后即可使用如下命令进行模型下载：

```
modelscope download --model deepseek-ai/DeepSeek-R1-Distill-Qwen-7B --local_dir ./DeepSeek-R1-Distill-Qwen-7B
```

![](img/6799c6effca6bc710526e39a2d28fca4.png)

下载完成后模型完整项目如下：

![](img/22a0b2bee48d09456475c6b1156eb94d.png)

#### 3.2 DeepSeek R1 Distill LLama 8B 模型下载

ModelScope 模型地址：https://www.modelscope.cn/models/deepseek-ai/DeepSeek-R1-Distill-Llama-8B

![](img/72d3a935d9b27c947f38fb3ab108b158.png)

然后创建 `DeepSeek-R1-Distill-Llama-8B` 文件夹，用于保存模型权重：

```
mkdir ./DeepSeek-R1-Distill-Llama-8B
```

然后即可使用如下命令进行模型下载：

```
modelscope download --model deepseek-ai/DeepSeek-R1-Distill-Llama-8B --local_dir ./DeepSeek-R1-Distill-Llama-8B
```

![](img/a3d7712c58667f6ef1378b9f74ecb892.png)

下载完成后模型完整项目如下：

![](img/f0aa591c2dac6014e02ad602fc5796b5.png)

两个模型下载完成后文件结构如下所示：

![](img/1a2ed8671b01b8536fbd5ac1c0ae31c1.png)

此外，也可以在课件网盘中下载模型权重：

![](img/7980bbebf7d591b8d3c2918490e761c2.png)

扫码联系⬆️助教即可领取～

---

### 4. 推理模型微调数据集下载

#### 4.1 DeepSeek R1 模型组回复结构与微调数据集结构要求

DeepSeek R1 及其蒸馏模型，推理过程的具体体现就是在回复内容中，会同时包含推理部分内容和最终回复部分内容，并且其推理部分内容会通过（一种在模型训练过程中注入的特殊标记）来进行区分。

![](img/55993954ba5bd1bc8c3ecbed580eaf86.png)

也就是说，DeepSeek R1 模型组的回复格式是一种非常特殊的格式，即包含 `think` 部分内容，也包含 `response` 部分内容。

因此，在围绕 DeepSeek R1 模型组进行微调的时候，微调数据集的回复部分文本也需要是包含推理和最终回复两部分内容，才能使得 DeepSeek R1 模型组在保持既定回复风格的同时，强化模型能力，反之则会导致指令消融问题（模型回复不再包含 `think` 部分）。

此时 `think` 部分和最终回复，共同构成有监督微调的标签。

而这种同时包含思考和结果的数据集，在推理模型大行其道的当下也并不少见，例如非常著名的数学问答数据集 `NuminaMath CoT`，就同时包含数学问题、问题的解题思路（也就是 `think` 部分）和问题最终的答案。而该数据集也是可以用于推理模型微调的数据集。

![](img/0bcd626686489d067a759e276f02a778.png)
