
# GPT-OSS高效微调实战

## Part 3.Unsloth快速上手指南

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/OwnFd80KIoNHWSxplMRcRMw7nLe](https://kq4b3vgg5b.feishu.cn/docx/OwnFd80KIoNHWSxplMRcRMw7nLe)

### 课程说明

+ 体验课内容节选自《2025大模型Agent智能体开发实战》(秋招冲刺班)完整版付费课程

体验课时间有限，若想深度学习大模型技术，欢迎大家报名由我主讲的《2025大模型Agent智能体开发实战》(秋招冲刺班)

在进行了充足的准备工作后，接下来正式开始进行微调。

### 七、Unsloth基本使用方法介绍

`Unsloth`是一个集模型调用和高效微调为一体的框架，在开始进行模型微调前，我们可以先尝试借助`Unsloth`进行模型调用。需要注意的是，`Unsloth`的使用难度远比一般的微调框架简单，在Jupyter中即可完成模型微调，且微调结束后还可以直接进行模型调用，并支持在Jupyter中进行模型权重合并与导出，非常便捷。

#### 1.模型导入与调用流程

首先进行模型导入：

```
import os
os.environ["CUDA_VISIBLE_DEVICES"] = "1"
```

> 由于当前实验环境是多卡环境，而动态量化模型只支持单卡运行，因此这里先设置接下来运行的GPU编号。

```
from unsloth import FastLanguageModel
import torch
```

```
🦥 Unsloth: Will patch your computer to enable 2x faster free finetuning.


/root/miniconda3/envs/unsloth/lib/python3.12/site-packages/tqdm/auto.py:21: TqdmWarning: IProgress not found. Please update jupyter and ipywidgets. See https://ipywidgets.readthedocs.io/en/stable/user_install.html
  from .autonotebook import tqdm as notebook_tqdm


🦥 Unsloth Zoo will now patch everything to make training faster!
```

```
max_seq_length = 8192
dtype = None
load_in_4bit = False
```

```
model, tokenizer = FastLanguageModel.from_pretrained(
    model_name = "./gpt-oss-20b",
    max_seq_length = max_seq_length,
    dtype = dtype,
    load_in_4bit = load_in_4bit,
)
```

```
---------------------------------------------------------------------------

NameError                                 Traceback (most recent call last)

Cell In[1], line 1
----> 1 model, tokenizer = FastLanguageModel.from_pretrained(
      2     model_name = "./gpt-oss-20b",
      3     max_seq_length = max_seq_length,
      4     dtype = dtype,
      5     load_in_4bit = load_in_4bit,
      6 )


NameError: name 'FastLanguageModel' is not defined
```

导入完成后即可查看模型基本情况，包括模型结构和分词器信息等：

```
model
```

```
GptOssForCausalLM(
  (model): GptOssModel(
    (embed_tokens): Embedding(201088, 2880, padding_idx=199999)
    (layers): ModuleList(
      (0-23): 24 x GptOssDecoderLayer(
        (self_attn): GptOssAttention(
          (q_proj): Linear(in_features=2880, out_features=4096, bias=True)
          (k_proj): Linear(in_features=2880, out_features=512, bias=True)
          (v_proj): Linear(in_features=2880, out_features=512, bias=True)
          (o_proj): Linear(in_features=4096, out_features=2880, bias=True)
        )
        (mlp): GptOssMLP(
          (router): GptOssTopKRouter()
          (experts): Mxfp4GptOssExperts()
        )
        (input_layernorm): GptOssRMSNorm((2880,), eps=1e-05)
        (post_attention_layernorm): GptOssRMSNorm((2880,), eps=1e-05)
      )
    )
    (norm): GptOssRMSNorm((2880,), eps=1e-05)
    (rotary_emb): GptOssRotaryEmbedding()
  )
  (lm_head): Linear(in_features=2880, out_features=201088, bias=False)
)
```

> 需要注意，此时模型还没有LoRA层。

```
tokenizer
```

```
PreTrainedTokenizerFast(name_or_path='./gpt-oss-20b', vocab_size=199998, model_max_length=1000000000000000019884624838656, is_fast=True, padding_side='left', truncation_side='right', special_tokens={'bos_token': '<|startoftext|>', 'eos_token': '<|return|>', 'pad_token': '<|endoftext|>', clean_up_tokenization_spaces=False, added_tokens_decoder={
        199998: AddedToken('<|startoftext|>', rstrip=False, lstrip=False, single_word=False, normalized=False, special=True),
        199999: AddedToken('<|endoftext|>', rstrip=False, lstrip=False, single_word=False, normalized=False, special=True),
        200000: AddedToken('<|reserved_200000|>', rstrip=False, lstrip=False, single_word=False, normalized=False, special=True),
        200001: AddedToken('<|reserved_200001|>', rstrip=False, lstrip=False, single_word=False, normalized=False, special=True),
        200002: AddedToken('<|return|>', rstrip=False, lstrip=False, single_word=False, normalized=False, special=True),
        200003: AddedToken('<|constrain|>', rstrip=False, lstrip=False, single_word=False, normalized=False, special=True),
        200004: AddedToken('<|reserved_200004|>', rstrip=False, lstrip=False, single_word=False, normalized=False, special=True),
        200005: AddedToken('<|channel|>', rstrip=False, lstrip=False, single_word=False, normalized=False, special=True),
        200006: AddedToken('<|start|>', rstrip=False, lstrip=False, single_word=False, normalized=False, special=True),
        200007: AddedToken('<|end|>', rstrip=False, lstrip=False, single_word=False, normalized=False, special=True),
        200008: AddedToken('<|message|>', rstrip=False, lstrip=False, single_word=False, normalized=False, special=True),
        200009: AddedToken('<|reserved_200009|>', rstrip=False, lstrip=False, single_word=False, normalized=False, special=True),
        200010: AddedToken('<|reserved_200010|>', rstrip=False, lstrip=False, single_word=False, normalized=False, special=True),
        200011: AddedToken('<|reserved_200011|>', rstrip=False, lstrip=False, single_word=False, normalized=False, special=True),
        200012: AddedToken('<|call|>', rstrip=False, lstrip=False, single_word=False, normalized=False, special=True),
        200013: AddedToken('<|reserved_200013|>', rstrip=False, lstrip=False, single_word=False, normalized=False, special=True),
        200014: AddedToken('<|reserved_200014|>', rstrip=False, lstrip=False, single_word=False, normalized=False, special=True),
        200015: AddedToken('<|reserved_200015|>', rstrip=False, lstrip=False, single_word=False, normalized=False, special=True),
        200016: AddedToken('<|reserved_200016|>', rstrip=False, lstrip=False, single_word=False, normalized=False, special=True),
        200017: AddedToken('<|reserved_200017|>', rstrip=False, lstrip=False, single_word=False, normalized=False, special=True),
        200018: AddedToken('<|endofprompt|>', rstrip=False, lstrip=False, single_word=False, normalized=False, special=True),
}
)
```

#### 显存占用

此时模型约占用显存16G：

```
gpu_stats = torch.cuda.get_device_properties(0)
start_gpu_memory = round(torch.cuda.max_memory_reserved() / 1024 / 1024 / 1024, 3)
max_memory = round(gpu_stats.total_memory / 1024 / 1024 / 1024, 3)
print(f"GPU = {gpu_stats.name}. Max memory = {max_memory} GB.")
print(f"{start_gpu_memory} GB of memory reserved.")
```

```
GPU = NVIDIA H800 PCIe. Max memory = 79.19 GB.
15.467 GB of memory reserved.
```

#### 开启对话

然后即可尝试进行对话。借助`Unsloth`进行模型调用总共需要两个步骤，其一是借助`apply_chat_template`进行分词同时输入对话相关参数，其二则是借助`generate`进行文本创建。一次基本对话流程如下所示：

```
messages = [
    {"role" : "user", "content" : "你好，好久不见！"}
]
```

```
text = tokenizer.apply_chat_template(
    messages,
    tokenize = False,
    add_generation_prompt = True,
    return_tensors = "pt",
    reasoning_effort = "low", 
)
```

此时`text`就是加载了gpt-oss内置提示词模板之后的字符串。据此也能看出gpt-oss内置提示词模板的特殊字符：

```
text
```

```
'<|start|>system<|message|>You are ChatGPT, a large language model trained by OpenAI.\nKnowledge cutoff: 2024-06\nCurrent date: 2025-08-21\n\nReasoning: low\n\n# Valid channels: analysis, commentary, final. Channel must be included for every message.<|end|><|start|>user<|message|>你好，好久不见！<|end|><|start|>assistant'
```

然后进行分词：

```
inputs = tokenizer(text, return_tensors="pt").to("cuda")
```

```
inputs
```
