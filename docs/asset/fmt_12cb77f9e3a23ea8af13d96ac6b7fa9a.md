
# LlamaFactory 一键式LLM训练、微调工具介绍与实践

> 来源：[https://agijuejin.feishu.cn/docx/Zb9HdSfA2odMLqxmOEecguvVnse](https://agijuejin.feishu.cn/docx/Zb9HdSfA2odMLqxmOEecguvVnse)

> 作者：LeonYi

> 原文：https://zhuanlan.zhihu.com/p/697773502

## 一、LlamaFactory介绍

![](img/71a6153dc9f07c445fe2f6c1a3d61f62.png)

`LlamaFactory` 是一个封装比较完善的LLM微调工具，它能够帮助用户快速地训练和微调大多数LLM模型。

Github: [https://github.com/hiyouga/LLaMA-Factory](https://link.zhihu.com/?target=https%3A//github.com/hiyouga/LLaMA-Factory)

### 1.1 简介

![](img/c963b4db409b41f054861bc93f4c6b37.png)

`LlamaFactory`主要通过`Trainer`类来实现训练流程，通过设置数据集、模型选型、训练类型、微调超参、模型保存，以及训练状态监控等信息，来开启训练。

![](img/08cde47534360ba2f0b53005f6cb2551.png)

支持的训练方法（这里的Pre-Training指的是增量预训练）

![](img/84d2758fb3faf5d9330c5f8c20e05d07.png)

`LlamaFactory`基于`PEFT`和`TRL`进行二次封装，从而可以快速开始SFT和RLHF微调。同时，引入`GaLore`和`Unsloth`等方案，能降低训练显存占用。

### 1.2 特性

+   **各种模型**: LLaMA, LLaVA, Mistral, Mixtral-MoE, Qwen, Yi, Gemma, Baichuan, ChatGLM, Phi, etc.

+   **集成训练方法**: (Continuous) pre-training, (multimodal) supervised fine-tuning, reward modeling, PPO, DPO and ORPO.

+   Scalable resources : 32-bit full-tuning, 16-bit freeze-tuning, 16-bit LoRA and 2/4/8-bit QLoRA via AQLM/AWQ/GPTQ/`LLM.int8`.

+   Advanced algorithms : `GaLore`, BAdam, DoRA, LongLoRA, LLaMA Pro, Mixture-of-Depths, LoRA+, LoftQ and Agent tuning.

+   **实用tricks**: `FlashAttention-2`, `Unsloth`, RoPE scaling, `NEFTune` and `rsLoRA`.

+   **实验监控**: `LlamaBoard`, `TensorBoard`, `Wandb`, `MLflow`, etc.

+   **推理集成**: OpenAI-style API, `Gradio` UI and CLI with `vLLM` worker.

`LlamaFactory`支持单机单卡，同时整合了`accelerate`和`deepseed`的单机多卡、多机多卡分布式训练。

**支持的模型**

| 模型名 | 模型大小 | 默认模块 | Template |

| --- | --- | --- | --- |

| Baichuan2 | 7B/13B | `W_pack` | baichuan2 |

| BLOOM | 560M/1.1B/1.7B/3B/7.1B/176B | `query_key_value` | - |

| BLOOMZ | 560M/1.1B/1.7B/3B/7.1B/176B | `query_key_value` | - |

| ChatGLM3 | 6B | `query_key_value` | chatglm3 |

| Command-R | 35B/104B | `q_proj,v_proj` | cohere |

| DeepSeek (MoE) | 7B/16B/67B | `q_proj,v_proj` | deepseek |

| Falcon | 7B/40B/180B | `query_key_value` | falcon |

| Gemma/CodeGemma | 2B/7B | `q_proj,v_proj` | gemma |

| InternLM2 | 7B/20B | `wqkv` | intern2 |

| LLaMA | 7B/13B/33B/65B | `q_proj,v_proj` | - |

| LLaMA-2 | 7B/13B/70B | `q_proj,v_proj` | llama2 |

| Mistral/Mixtral | 7B/8x7B/8x22B | `q_proj,v_proj` | mistral |

| OLMo | 1B/7B | `att_proj` | olmo |

| Phi-1.5/2 | 1.3B/2.7B | `q_proj,v_proj` | - |

| Qwen | 1.8B/7B/14B/72B | `c_attn` | qwen |

| Qwen1.5 (Code/MoE) | 0.5B/1.8B/4B/7B/14B/32B/72B | `q_proj,v_proj` | qwen |

| StarCoder2 | 3B/7B/15B | `q_proj,v_proj` | - |

| XVERSE | 7B/13B/65B | `q_proj,v_proj` | xverse |

| Yi | 6B/9B/34B | `q_proj,v_proj` | yi |

| Yuan | 2B/51B/102B | `q_proj,v_proj` | yuan |

![](img/387ac9b8a1b534891f3c56026ceb6ffb.png)

基于LlamaFactory框架进行的各种训练效率比较

![](img/20145d0a1d2211bf1aaca2483e087ab0.png)

适合进行各种LLM在不同训练方法下，效果评估对比

### 1.3 数据集信息

`LlamaFactory`配置的数据集格式。

![](img/6b5a37f2adf422b38efd77b40809c484.png)

**预训练数据集**

+   [Wiki Demo (en)](https://zhuanlan.zhihu.com/p/697773502/data/wiki_demo.txt)

+   [RefinedWeb (en)](https://link.zhihu.com/?target=https%3A//huggingface.co/datasets/tiiuae/falcon-refinedweb)

+   RedPajama V2 (en)

+   [Wikipedia (en)](https://link.zhihu.com/?target=https%3A//huggingface.co/datasets/olm/olm-wikipedia-20221220)

+   [Wikipedia (zh)](https://link.zhihu.com/?target=https%3A//huggingface.co/datasets/pleisto/wikipedia-cn-20230720-filtered)

+   [Pile (en)](https://link.zhihu.com/?target=https%3A//huggingface.co/datasets/EleutherAI/pile)

+   [SkyPile (zh)](https://link.zhihu.com/?target=https%3A//huggingface.co/datasets/Skywork/SkyPile-150B)

+   [The Stack (en)](https://link.zhihu.com/?target=https%3A//huggingface.co/datasets/bigcode/the-stack)

+   [StarCoder (en)](https://link.zhihu.com/?target=https%3A//huggingface.co/datasets/bigcode/starcoderdata)

**指令微调数据集**

+   Stanford Alpaca (en)

+   Stanford Alpaca (zh)

+   Alpaca GPT4 (en&zh)

+   Self Cognition (zh)

+   Open Assistant (multilingual)

+   [ShareGPT (zh)](https://link.zhihu.com/?target=https%3A//huggingface.co/datasets/QingyiSi/Alpaca-CoT/tree/main/Chinese-instruction-collection)

+   Guanaco Dataset (multilingual)

+   [BELLE 2M (zh)](https://link.zhihu.com/?target=https%3A//huggingface.co/datasets/BelleGroup/train_2M_CN)

+   [BELLE 1M (zh)](https://link.zhihu.com/?target=https%3A//huggingface.co/datasets/BelleGroup/train_1M_CN)

+   [BELLE 0.5M (zh)](https://link.zhihu.com/?target=https%3A//huggingface.co/datasets/BelleGroup/train_0.5M_CN)

+   BELLE Dialogue 0.4M (zh)

+   BELLE School Math 0.25M (zh)

+   BELLE Multiturn Chat 0.8M (zh)

+   [UltraChat (en)](https://link.zhihu.com/?target=https%3A//github.com/thunlp/UltraChat)

+   [LIMA (en)](https://link.zhihu.com/?target=https%3A//huggingface.co/datasets/GAIR/lima)

+   OpenPlatypus (en)

+   CodeAlpaca 20k (en)

+   Alpaca CoT (multilingual)

+   [OpenOrca (en)](https://link.zhihu.com/?target=https%3A//huggingface.co/datasets/Open-Orca/OpenOrca)

+   [SlimOrca (en)](https://link.zhihu.com/?target=https%3A//huggingface.co/datasets/Open-Orca/SlimOrca)

+   MathInstruct (en)

+   Firefly 1.1M (zh)

+   [Wiki QA (en)](https://link.zhihu.com/?target=https%3A//huggingface.co/datasets/wiki_qa)

+   [Web QA (zh)](https://link.zhihu.com/?target=https%3A//huggingface.co/datasets/suolyer/webqa)

+   [WebNovel (zh)](https://link.zhihu.com/?target=https%3A//huggingface.co/datasets/zxbsmk/webnovel_cn)

+   [Nectar (en)](https://link.zhihu.com/?target=https%3A//huggingface.co/datasets/berkeley-nest/Nectar)

+   deepctrl (en&zh)

+   [Ad Gen (zh)](https://link.zhihu.com/?target=https%3A//huggingface.co/datasets/HasturOfficial/adgen)

+   ShareGPT Hyperfiltered (en)

+   ShareGPT4 (en&zh)

+   UltraChat 200k (en)

+   AgentInstruct (en)

+   LMSYS Chat 1M (en)

+   Evol Instruct V2 (en)

+   Glaive Function Calling V2 (en)

+   [Cosmopedia (en)](https://link.zhihu.com/?target=https%3A//huggingface.co/datasets/HuggingFaceTB/cosmopedia)

+   Open Assistant (de)

+   [Dolly 15k (de)](https://link.zhihu.com/?target=https%3A//huggingface.co/datasets/mayflowergmbh/dolly-15k_de)

+   Alpaca GPT4 (de)

+   OpenSchnabeltier (de)

+   Evol Instruct (de)

+   [Dolphin (de)](https://link.zhihu.com/?target=https%3A//huggingface.co/datasets/mayflowergmbh/dolphin_de)

+   [Booksum (de)](https://link.zhihu.com/?target=https%3A//huggingface.co/datasets/mayflowergmbh/booksum_de)

+   [Airoboros (de)](https://link.zhihu.com/?target=https%3A//huggingface.co/datasets/mayflowergmbh/airoboros-3.0_de)

+   [Ultrachat (de)](https://link.zhihu.com/?target=https%3A//huggingface.co/datasets/mayflowergmbh/ultra-chat_de)

**偏好数据集**

+   [HH-RLHF (en)](https://link.zhihu.com/?target=https%3A//huggingface.co/datasets/Anthropic/hh-rlhf)

+   Open Assistant (multilingual)

+   GPT-4 Generated Data (en&zh)

+   [Orca DPO (en)](https://link.zhihu.com/?target=https%3A//huggingface.co/datasets/Intel/orca_dpo_pairs)

+   [Nectar (en)](https://link.zhihu.com/?target=https%3A//huggingface.co/datasets/berkeley-nest/Nectar)

+   [Orca DPO (de)](https://link.zhihu.com/?target=https%3A//huggingface.co/datasets/mayflowergmbh/intel_orca_dpo_pairs_de)

部分数据集的使用需要确认，推荐使用下述命令登录 Hugging Face 账户。

```
pip install --upgrade huggingface_hub
huggingface-cli login
```

### 1.4 软硬件依赖

| 必需项 | 至少 | 推荐 |

| --- | --- | --- |

| `python` | 3.8 | 3.10 |

| `torch` | 1.13.1 | 2.2.0 |

| `transformers` | 4.37.2 | 4.39.3 |

| `datasets` | 2.14.3 | 2.18.0 |

| `accelerate` | 0.27.2 | 0.28.0 |

| `peft` | 0.9.0 | 0.10.0 |

| `trl` | 0.8.1 | 0.8.1 |

| 可选项 | 至少 | 推荐 |

| --- | --- | --- |

| `CUDA` | 11.6 | 12.2 |

| `deepspeed` | 0.10.0 | 0.14.0 |

| `bitsandbytes` | 0.39.0 | 0.43.0 |

| `flash-attn` | 2.3.0 | 2.5.6 |

### 1.5 硬件依赖

* 估算值

| 方法 | 精度 | 7B | 13B | 30B | 70B | 8x7B | 8x22B |

| --- | --- | --- | --- | --- | --- | --- | --- |

| Full | AMP | 120GB | 240GB | 600GB | 1200GB | 900GB | 2400GB |

| Full | 16 | 60GB | 120GB | 300GB | 600GB | 400GB | 1200GB |

| Freeze | 16 | 20GB | 40GB | 80GB | 200GB | 160GB | 400GB |
