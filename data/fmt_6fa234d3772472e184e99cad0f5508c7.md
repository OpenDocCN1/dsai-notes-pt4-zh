
# LLM101n-CN（共建中）

> 来源：[https://v11enp9ok1h.feishu.cn/docx/EFG8dPwBXovPNpxwdJgcPkBGnDJ](https://v11enp9ok1h.feishu.cn/docx/EFG8dPwBXovPNpxwdJgcPkBGnDJ)

`<https://v11enp9ok1h.feishu.cn/sync/PaIIdslbxsIJUIbkTfPcsMCFnph>`

| **章节** | **任务ID** | **任务状态** | **小节** | **描述** | **算力奖励** |
| --- | --- | --- | --- | --- | --- |
| `ngram` | 101N0101 | S3: 已完成 | `ngram` Python 核心代码解读 | 深度解读 [EurekaLabsAI](https://github.com/EurekaLabsAI)/[ngram](https://github.com/EurekaLabsAI/ngram) 库下的核心代码：`https://github.com/EurekaLabsAI/ngram/blob/master/ngram.py` | 300-1000 |
|  | 101N0102 | S3: 已完成 | `ngram` C 核心代码解读 | 深度解读 [EurekaLabsAI](https://github.com/EurekaLabsAI)/[ngram](https://github.com/EurekaLabsAI/ngram) 库下的核心代码：`https://github.com/EurekaLabsAI/ngram/blob/master/ngram.c` | 300-1000 |
|  | 101N0103 | S3: 已完成 | `ngram` README 翻译 | 翻译：`https://github.com/EurekaLabsAI/ngram/blob/master/README.md` | 50 |
|  |  |  |  |  |  |
|  |  |  |  |  |  |
| `micrograd` | 101N0201 | S3:已完成 | `Micrograd` Python 核心代码解读 | 深度解读 [EurekaLabsAI](https://github.com/EurekaLabsAI)/[micrograd](https://github.com/EurekaLabsAI/micrograd)库下的核心代码：`https://github.com/EurekaLabsAI/micrograd/blob/master/micrograd.py` | 300-1000 |
|  | 101N0202 | S3:已完成 | `Micrograd` C 代码自发实现 | 自[行](https://github.com/EurekaLabsAI/micrograd/blob/master/micrograd.py)用C语言复现`Micrograd` python核心代码，并确保输出内容一致。（`micrograd` python核心代码：`https://github.com/EurekaLabsAI/micrograd/blob/master/micrograd.py`） | 300-1000 |
|  | 101N0203 | SQ: 已废弃 | `Micrograd` `PyTorch` 核心代码解读 | 深度解读 [EurekaLabsAI](https://github.com/EurekaLabsAI)/[micrograd](https://github.com/EurekaLabsAI/micrograd)库下的核心代码：`https://github.com/EurekaLabsAI/micrograd/blob/master/micrograd_pytorch.py` |  |
|  |  |  |  |  |  |
| `mlp` | 101N0301 | S3: 已完成 | `MLP` Python 核心代码（`pytorch`版）解读 | 深度解读 [EurekaLabsAI](https://github.com/EurekaLabsAI)/[mlp](https://github.com/EurekaLabsAI/mlp) 库下的核心代码：`https://github.com/EurekaLabsAI/mlp/blob/master/mlp_pytorch.py` | 300-1000 |
|  | 101N0302 | S3:已完成 | `MLP` Python 核心代码（`numpy`版）解读 | 深度解读 [EurekaLabsAI](https://github.com/EurekaLabsAI)/[mlp](https://github.com/EurekaLabsAI/mlp) 库下的核心代码：`https://github.com/EurekaLabsAI/mlp/blob/master/mlp_numpy.py` | 300-1000 |
|  |  |  |  |  |  |
| `tensor` | 101N0401 | S1: 未领取 | `Tensor` README 翻译 | 翻译：`https://github.com/EurekaLabsAI/tensor/blob/master/README.md` | 50 |
|  | 101N0402 | S0: 出题中 | `Tensor` Python 代码深度解读 | 解读：`https://github.com/EurekaLabsAI/tensor/blob/master/tensor1d.py` | 300-1000 |
|  | 101N0403 | S0: 出题中 | `Tensor` C 代码深度解读 | 解读：`https://github.com/EurekaLabsAI/tensor/blob/master/tensor1d.c` | 300-1000 |
|  |  |  |  |  |  |

## 作者介绍

## LLM101n:让我们构建一个Storyteller

在本课程中，我们将建立一个讲故事的人工智能大型语言模型(LLM)。携手并进，你将能够用AI创建，完善和说明小故事。我们将使用Python、C和CUDA从头开始构建一个类似于ChatGPT的web应用程序，从基础到功能俱全，只需要最低限度的计算机科学先决条件。最后，你应该对AI, llm和深度学习有一个相对深入的了解。

**Syllabus** **教学大纲**

+   [Chapter 01](https://github.com/karpathy/LLM101n/blob/master/bigram/README.md) `Bigram` Language Model (language modeling)第01章`Bigram`语言模型(语言建模)

+   [Chapter 02](https://github.com/karpathy/LLM101n/blob/master/micrograd/README.md) `Micrograd` 机器学习，反向传播

+   [Chapter 03](https://github.com/karpathy/LLM101n/blob/master/mlp/README.md)  N-gram模型(多层感知机、`matmul`、`gelu`)

+   [Chapter 04](https://github.com/karpathy/LLM101n/blob/master/attention/README.md) 注意力机制(注意力机制、`softmax`、位置编码器)

+   [Chapter 05](https://github.com/karpathy/LLM101n/blob/master/transformer/README.md) `Transformer` (`Transformer`，残差，`layernorm`, `GPT-2`)

+   [Chapter 06](https://github.com/karpathy/LLM101n/blob/master/tokenization/README.md) `Tokenization` (`minBPE`，字节对编码)

+   [Chapter 07](https://github.com/karpathy/LLM101n/blob/master/optimization/README.md) `Optimization` (`initialization`, `optimization`, `AdamW`)

+   [Chapter 08](https://github.com/karpathy/LLM101n/blob/master/device/README.md) 提速 (设备、CPU、GPU……)

+   [Chapter 09](https://github.com/karpathy/LLM101n/blob/master/precision/README.md) Need for Speed II: Precision (mixed precision training, `fp16`, `bf16`, `fp8`, ...)

+   [Chapter 10](https://github.com/karpathy/LLM101n/blob/master/distributed/README.md) Need for Speed III: Distributed (distributed optimization, `DDP`, `ZeRO`)

+   [Chapter 11](https://github.com/karpathy/LLM101n/blob/master/datasets/README.md) 数据集(数据集、数据加载、合成数据生成)

+   [Chapter 12](https://github.com/karpathy/LLM101n/blob/master/inference/README.md) 模型推理一:`kv-cache` (`kv-cache`)

+   [Chapter 13](https://github.com/karpathy/LLM101n/blob/master/quantization/README.md) 模型推理二:量化(量化)

+   [Chapter 14](https://github.com/karpathy/LLM101n/blob/master/sft/README.md) Finetuning I: `SFT` (supervised finetuning `SFT`, `PEFT`, `LoRA`, chat)第14章微调I: `SFT` (supervised Finetuning `SFT`, `PEFT`, `LoRA`, chat)

+   [Chapter 15](https://github.com/karpathy/LLM101n/blob/master/rl/README.md) Finetuning II: `RL` (reinforcement learning, `RLHF`, `PPO`, `DPO`)第15章微调II: `RL`(强化学习，`RLHF`, `PPO`, `DPO`)

+   [Chapter 16](https://github.com/karpathy/LLM101n/blob/master/deployment/README.md) `Deployment` (`API`, web app)

+   [Chapter 17](https://github.com/karpathy/LLM101n/blob/master/multimodal/README.md) `Multimodal` (`VQVAE`, diffusion transformer)

**Appendix** **附录**

+   Programming languages: Assembly, C, Python

+   Data types: `Integer`, `Float`, `String` (`ASCII`, `Unicode`, `UTF-8`)

+   `Tensor`: `shapes`, `views`, `strides`, `contiguous`, ...

+   Deep Learning frameworks: `PyTorch`, `JAX`

+   Neural Net Architecture: `GPT` (1,2,3,4), `Llama` (`RoPE`, `RMSNorm`, `GQA`), `MoE`, ...

+   Multimodal: Images, Audio, Video, `VQVAE`, `VQGAN`, diffusion

---

**Update June 25.** To clarify, the course will take some time to build. There is no specific timeline. Thank you for your interest, but please do not submit Issues/PRs.

6月25日更新。为了澄清，课程将需要一些时间来建立。没有具体的时间表。谢谢您的兴趣，但请不要提交问题/ pr。
