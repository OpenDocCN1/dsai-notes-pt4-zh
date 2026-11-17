
# LLM（九）：LLaMA, Alpaca, ColossalChat 系列模型研究

> 来源：[https://agijuejin.feishu.cn/docx/IKztdgZbUopnrixu6HOcTqFHnkf](https://agijuejin.feishu.cn/docx/IKztdgZbUopnrixu6HOcTqFHnkf)
> 作者：紫气东来
> 原文：[https://zhuanlan.zhihu.com/p/618695885](https://zhuanlan.zhihu.com/p/618695885)

## 一、从 LLaMA 到 Alpaca：大模型的小训练

### 1.1 LLaMA 概要与实践

LLaMA(Large Language Model Meta AI) 是由 Meta AI 发布的一款全新的大型语言模型，共有7B、13B、33B、65B 四种版本，其模型参数如下表所示：

![](img/abe8843faeba400849da63e2718f0861.png)

与原始的 `transformer` Decoder 相比，LLaMA主要有以下改进：

+ 预归一化(Pre-normalization)[GPT3]

为了提高训练的稳定性，LLaMA对每个 `transformer` 子层的输入进行归一化，而不是对输出进行归一化。同时使用 `RMSNorm` 归一化函数。

+ SwiGLU激活函数[PaLM]

LLaMA用 `SwiGLU` 激活函数取代 `ReLU` 非线性，以提高性能。`SwiGLU` 激活函数的实现如下：

`SwiGLU(x, W, V, b, c, β) = Swish_β(xW + b) ⊗ (xV + c)`

其中 `Swish = x · σ(βx)`，`GLU(x) = σ(Wx + b) ⊗ (Vx + c)`。具体实现参见[这里](https://link.zhihu.com/?target=https%3A//github.com/huggingface/transformers/blob/main/src/transformers/activations.py%23L140)。

+ 旋转嵌入(Rotary pos)[GPTNeo]

`LLaMA` 删除了绝对位置嵌入，取而代之的是在网络的每一层添加旋转位置嵌入（`RoPE`），`RoPE`的实现参见[这里](https://link.zhihu.com/?target=https%3A//nn.labml.ai/transformers/rope/index.html)。

[当前](https://link.zhihu.com/?target=https%3A//github.com/huggingface/transformers/blob/main/src/transformers/models/llama/modeling_llama.py) `HuggingFace` 已经实现了 `LLaMA` 模型 代码，可通过以下方式直接调用：

```
from transformers import LlamaForCausalLM
USE_8BIT = True # use 8-bit quantization; otherwise, use fp16
model = LlamaForCausalLM.from_pretrained(
            "pretrained/path",
            load_in_8bit=USE_8BIT,
            torch_dtype=torch.float16,
            device_map="auto",
        )
if not USE_8BIT:
    model.half()  # use fp16
model.eval()
```

### 1.2 Alpaca 如何使用小成本训练大模型

如下图所示，Stanford的研究者使用 52K 个 `instruction-following` examples 来微调 `LLaMA` 7B 模型，从而生成了 `Alpaca` 7B。

`Alpaca` 团队使用 [self-instruct](https://link.zhihu.com/?target=https%3A//github.com/yizhongw/self-instruct) 提供的 175 个 prompts，调用 `OpenAI` 的 `text-davinci-003` 模型，利用 `OpenAI` 的模型来产生有价值的 instructions 。

![](img/441a3760ac242f5679422e81991536d0.png)

将 `OpenAI` 性能完备的模型作为 `Teacher`，来指导参数更少的 `Alpaca` 模型进行训练，大幅降低了训练成本 。其中调用 `OpenAI API` 的成本不到 500 美元，另外微调 7B 参数的 `LLaMA` 模型，使用云服务商提供的 8 块 80GB `A100` 显卡，训练 3 小时，消费不到 100 美元，因此整体成本是小于 600 美元。

## 二、Alpaca 的优化、训练及应用

### 2.1 alpaca-lora 的优化

[alpaca-lora](https://link.zhihu.com/?target=https%3A//github.com/tloen/alpaca-lora)是在 `alpaca` 的基础上把训练方式改成用 `lora` 训练，仅需要在消费级的 `GPU` 上经过数小时的训练，就可以达到和 `alpaca` 差不多的效果。

`LoRA` (`Low-Rank Adaptation`) 技术

`LoRA` 的思想很简单，即在原始 `Pretrained Weights` 旁边增加一个旁路，做一个降维再升维的操作，来模拟所谓的 `intrinsic rank` 。训练的时候固定 `Pretrained Weights` 的参数，只训练降维矩阵 `A` 与升维矩阵 `B` 。而模型的输入输出维度不变，输出时将 `BA` 与 `Pretrained Weights` 的参数叠加。用随机高斯分布初始化 `A` ，用 0 矩阵初始化 `B` ，保证训练的开始此旁路矩阵依然是 0 矩阵。

具体来看，假设预训练的矩阵为 `W_0 ∈ R^{d × k}` ，它的更新可表示为：

`W_0 + ΔW = W_0 + BA, B ∈ R^{d × r}, A ∈ R^{r × k}`

其中秩 `r ≪ min(d, k)` 。

![](img/512fb83c21c8bba408ab8168829e38ca.png)

`LoRA` 与 `Transformer` 的结合也很简单，仅在 `QKV attention` 的计算中增加一个旁路，而不动 `MLP` 模块。基于大模型的内在低秩特性，增加旁路矩阵来模拟全模型参数微调，`LoRA` 通过简单有效的方案来达成轻量微调的目的，可以将现在的各种大模型通过轻量微调变成各个不同领域的专业模型。

### 2.2 PEFT(Parameter-Efficient Fine-Tuning) 方法

随着模型变得越来越大，在消费级硬件上对模型进行全部参数的微调变得不可行。此外，为每个下游任务独立存储和部署微调模型变得非常昂贵，因为微调模型与原始预训练模型的大小相同。`PEFT` 方法旨在解决这两个问题，`PEFT` 方法仅微调少量 (额外) 模型参数，同时冻结预训练 `LLM` 的大部分参数，从而大大降低了计算和存储成本。

`HuggingFace` 开源的一个高效微调大模型的 `PEFT` 库，目前包含 `LoRA`，`Prefix Tuning`，`Prompt Tuning`，`P-Tuning` 四种算法，下面简要介绍后三种：

+ [`Prefix Tuning`](https://link.zhihu.com/?target=https%3A//aclanthology.org/2021.acl-long.353.pdf)

`Prefix Tuning` 算法是根据 下游任务 "前缀指令文本" 的所有层的 `embeding` 表示，学习到的前缀指令文本向量可以挖掘大模型的潜力去引导模型完成特定任务。

![](img/5627bbd5958f8873431af224dae52575.png)

[`P-Tuning v2`](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2110.07602.pdf) 在实际上就是 `Prefix-tuning`，在 `Prefix` 部分，每一层 `transformer` 的 `embedding` 输入需要被 `tuned`。而 `P-tuning v1` 只有 `transformer` 第一层的 `embedding` 输入需要被 `tuned`。此外，`P-Tuning v2` 还包括以下改进：

+ 移除了 `Reparameterization` 加速训练方式；

+ 采用了多任务学习优化：基于多任务数据集的 `Prompt` 进行预训练，然后再适配的下游任务。

+ 舍弃了词汇 `Mapping` 的 `Verbalizer` 的使用，重新利用 `[CLS]` 和字符标签，跟传统 `finetune` 一样利用 `cls` 或者 `token` 的输出做 `NLU`，以增强通用性，可以适配到序列标注任务。

![](img/0897afc60b1a26d787fcecca06532213.png)

+ [`P-Tuning`](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2103.10385.pdf)

`P-Tuning` 算法和 `Prefix Tuning` 的想法很相似，想通过微调 "指令文本"，让指令文本去挖掘大模型的潜力去完成特定的任务。但是 `P-Tuning` 只学习 "指令文本" 输入层 `embeding` 的表示。为了增强 "指令文本" 的连续性，采用了一个 `MLP` (`LSTM`) 的结果去 `encoding` "指令文本"。从微调参数量来看只有 0.65% 比 `Prefix Tuning` 和 `LoRA` 这些在所有层都增加参数的方法要少。

![](img/fa3942aa3f1f6bee44e60206d4df3df7.png)

+ [`Prompt Tuning`](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2104.08691.pdf)

`Prompt Tuning` 算法和 `P-Tuning` 很像，且更简单，就是是根据 下游任务 "指令文本" 输入层 `embeding` 的表示。`Prompt Tuning` 没有增加任何的层，直接使用微调指令文本 (`prompt`) 的 `embeding` 向量。

![](img/71ea7c417bff13b6c4ef86ddc1dac396.png)

### 2.3 使用 `PEFT` 训练 `alpaca-lora`

以下仅说明过程，完整代码见[这里](https://link.zhihu.com/?target=https%3A//github.com/tloen/alpaca-lora/blob/main/finetune.py)。

step 1. 模块的加载及初始化

```
import torch
import transformers
from datasets import load_dataset
from peft import LoraConfig, get_peft_model, get_peft_model_state_dict, prepare_model_for_int8_training, set_peft_model_state_dict
from transformers import LlamaForCausalLM, LlamaTokenizer

model = LlamaForCausalLM.from_pretrained(base_model, load_in_8bit=True, torch_dtype=torch.float16, device_map=device_map,)
tokenizer = LlamaTokenizer.from_pretrained(base_model)
model = prepare_model_for_int8_training(model)

config = LoraConfig(
    r=lora_r,
    lora_alpha=lora_alpha,
    target_modules=lora_target_modules,
    lora_dropout=lora_dropout,
    bias="none",
    task_type="CAUSAL_LM",
)
model = get_peft_model(model, config)
```

step 2. 准备 `data` 和 `trainer` 并进行训练

```
data = load_dataset(data_path)
trainer = transformers.Trainer(
    model=model,
    train_dataset=train_data,
    eval_dataset=val_data,
    args=transformers.TrainingArguments(
        per_device_train_batch_size=micro_batch_size,
        gradient_accumulation_steps=gradient_accumulation_steps,
        warmup_steps=100,
        num_train_epochs=num_epochs,
        learning_rate=learning_rate,
        fp16=True,
        logging_steps=10,
        optim="adamw_torch",
        evaluation_strategy="steps" if val_set_size > 0 else "no",
        save_strategy="steps",
        eval_steps=200 if val_set_size > 0 else None,
        save_steps=200,
        output_dir=output_dir,
        save_total_limit=3,
        load_best_model_at_end=True if val_set_size > 0 else False,
        ddp_find_unused_parameters=False if ddp else None,
        group_by_length=group_by_length,
        report_to="wandb" if use_wandb else None,
        run_name=wandb_run_name if use_wandb else None,
    ),
    data_collator=transformers.DataCollatorForSeq2Seq(
        tokenizer, pad_to_multiple_of=8, return_tensors="pt", padding=True
    ),
)

trainer.train(resume_from_checkpoint=resume_from_checkpoint)
```

### 2.4 更多类似模型

| model | Paper or Blog | Repo | Demo |
| --- | --- | --- | --- |
| Vicuna | [https://lmsys.org/blog/2023-03-30-vicuna/](https://link.zhihu.com/?target=https%3A//lmsys.org/blog/2023-03-30-vicuna/) | [https://github.com/lm-sys/FastChat](https://link.zhihu.com/?target=https%3A//github.com/lm-sys/FastChat) | https://chat.lmsys.org/ |



| Baize (白泽) | [2304.01196.pdf](https://arxiv.org/pdf/2304.01196.pdf) | [project-baize/baize](https://github.com/project-baize/baize) | [Baize-7B](https://huggingface.co/spaces/project-baize/Baize-7B) |
| ChatGLM | [chatglm.cn/blog](https://chatglm.cn/blog) | [THUDM/ChatGLM-6B](https://github.com/THUDM/ChatGLM-6B) | [chatglm.cn](https://chatglm.cn/) |
| Koala (考拉) | [bair.berkeley.edu/blog/2023/04/03/koala/](https://bair.berkeley.edu/blog/2023/04/03/koala/) | [koala.lmsys.org](https://koala.lmsys.org/) | [young-geng/EasyLM](https://github.com/young-geng/EasyLM) |
| WizardLM | [nlpxucan/WizardLM](https://github.com/nlpxucan/WizardLM) | [nlpxucan/WizardLM](https://github.com/nlpxucan/WizardLM) | [36e20005221d21bf.gradio.app](https://36e20005221d21bf.gradio.app/) |
| Wombat | [2304.05302.pdf](https://arxiv.org/pdf/2304.05302.pdf) | [GanjinZero/RRHF](https://github.com/GanjinZero/RRHF) | |
| BELLE | [2304.07854.pdf](https://arxiv.org/pdf/2304.07854.pdf) | [LianjiaTech/BELLE](https://github.com/LianjiaTech/BELLE) | |
| Luotuo (骆驼，Chinese) | | [LC1332/Luotuo-Chinese-LLM](https://github.com/LC1332/Luotuo-Chinese-LLM) | |
| GPT4All | [2023_GPT4All_Technical_Report.pdf](https://s3.amazonaws.com/static.nomic.ai/gpt4all/2023_GPT4All_Technical_Report.pdf) | [nomic-ai/gpt4all](https://github.com/nomic-ai/gpt4all) | |
| LMFlow | | [OptimalScale/LMFlow](https://github.com/OptimalScale/LMFlow) | [lmflow.com](https://lmflow.com/) |
| Firefly(流萤, Chinese) | | [yangjianxin1/Firefly](https://github.com/yangjianxin1/Firefly) | |
| PandaLM | | [WeOpenML/PandaLM/tree/main](https://github.com/WeOpenML/PandaLM/tree/main) | |

## 三、ColossalChat：深入体验 RLHF 在大模型中的功能

ColossalChat 是第一个基于 LLaMA 预训练模型开源完整 RLHF pipeline 实现，包括有监督数据收集、有监督微调、奖励模型训练和强化学习微调。只需要不到 100 亿个参数，就可以在大型语言模型的基础上通过 RLHF 微调达到中英文双语水平，达到与 ChatGPT 和 GPT-3.5 相当的效果，并可以进行 Demo 测试。关于 RLHF 的原理，可参考

[613315873](https://zhuanlan.zhihu.com/p/613315873)

ColossalChat 使用 `InstructionWild` 双语数据集，包含约 52K 的英语和 52K 的汉语问答，数据集通过 `OpenAI API` 接口获得。该数据集适用于微调和 RLHF 训练。通过提供高质量的数据，ColossalChat 可以实现更好的对话互动，也可以支持中文。

![](img/73cb352be283ddb159aaf82b513c0cda.png)

### 3.1 监督指令微调

使用前面提到的数据集执行有监督指令微调，以微调模型。运行 `examples/train_sft.sh` 来启动有监督的指令微调。

```
torchrun --standalone --nproc_per_node=4 train_sft.py \
    --pretrain "/path/to/LLaMa-7B/" \
    --model 'llama' \
    --strategy colossalai_zero2 \
    --log_interval 10 \
    --save_path  /path/to/Coati-7B \
    --dataset /path/to/data.json \
    --batch_size 4 \
    --accimulation_steps 8 \
    --lr 2e-5 \
    --max_datasets_size 512 \
    --max_epochs 1 \
```

### 3.2 训练奖励模型

训练奖励模型，通过手动对同一提示的不同输出进行排序来分配相应的分数，然后有监督奖励模型的训练。

运行 `examples/train_rm.sh` 开始奖励模型训练。

```
torchrun --standalone --nproc_per_node=4 train_reward_model.py \
    --pretrain "/path/to/LLaMa-7B/" \
    --model 'llama' \
    --strategy colossalai_zero2 \
    --loss_fn 'log_exp' \
    --save_path 'rmstatic.pt' \
```

### 3.3 人类反馈强化学习

在第一阶段的监督微调模型和第二阶段的奖励模型的基础上，使用强化学习算法进一步训练大型语言模型。该阶段是 RLHF 训练的核心部分，在强化学习中使用近端策略优化（PPO）算法来引入奖励信号，并生成更符合人类偏好的内容。

![](img/b7e0b51683148efc3338cde2cda9e8b7.png)

运行 `examples/train_prompts.sh`，开始使用人类反馈训练 PPO。

```
torchrun --standalone --nproc_per_node=4 train_prompts.py \
         --pretrain "/path/to/LLaMa-7B/" \
         --model 'llama' \
         --strategy colossalai_zero2 \
         --prompt_path /path/to/your/prompt_dataset \
         --pretrain_dataset /path/to/your/pretrain_dataset \
         --rm_pretrain /your/pretrain/rm/defination \
         --rm_path /your/rm/model/path
```

## 参考资料

[1] [LaMA: Open and Efficient Foundation Language Models.](https://arxiv.org/abs/2302.13971v1)
[2] [Alpaca: A Strong, Replicable Instruction-Following Model](https://crfm.stanford.edu/2023/03/13/alpaca.html)
[3] [tatsu-lab/stanford_alpaca](https://github.com/tatsu-lab/stanford_alpaca)
[4] [Stanford CRFM](https://crfm.stanford.edu/2023/03/13/alpaca.html)
[5] [tloen/alpaca-lora: Instruct-tune LLaMA on consumer hardware (github.com)](https://github.com/tloen/alpaca-lora)
[6] [LORA: LOW-RANK ADAPTATION OF LARGE LANGUAGE MODELS](https://arxiv.org/pdf/2106.09685.pdf)
[7] [ColossalAI/applications/Chat at main · hpcaitech/ColossalAI (github.com)](https://github.com/hpcaitech/ColossalAI/tree/main/applications/Chat)

> 城中桃李愁风雨，春在溪头荠菜花。—— 辛弃疾《鹧鸪天·陌上柔桑破嫩芽》
