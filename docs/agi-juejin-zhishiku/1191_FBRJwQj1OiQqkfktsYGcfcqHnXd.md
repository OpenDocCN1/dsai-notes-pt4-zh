# LLM（九）：LLaMA, Alpaca, ColossalChat 系列模型研究

> 来源：[https://agijuejin.feishu.cn/docx/IKztdgZbUopnrixu6HOcTqFHnkf](https://agijuejin.feishu.cn/docx/IKztdgZbUopnrixu6HOcTqFHnkf)

> 作者：紫气东来

> 原文：[https://zhuanlan.z](https://zhuanlan.zhihu.com/p/618695885)hihu.com/p/618695...

## 一、从 LLaMA 到 Alpaca：大模型的小训练

### 1.1 LLaMA 概要与实践

LLaMA(Large Language Model Meta AI) 是由 Meta AI 发布了一款全新的大型语言模型，共有7B、13B、33B、65B 四种版本，其模型参数如下表所示：

![](img/abe8843faeba400849da63e2718f0861.png)

与原始的 transformer Decoder 相比，LLaMA主要有以下改进：

+   预归一化(Pre-normalization)[GPT3]

为[了提高训练的稳定](https://link.zhihu.com/?target=https%3A//github.com/huggingface/transformers/blob/main/src/transformers/models/llama/modeling_llama.py%23L74)性，LLaMA对每个transformer子层的输入进行归一化，而不是对输出进行归一化。同时使用 RMSNorm 归一化函数。

+   SwiGLU激活函数[PaLM]

LLaMA用SwiGLU激活函数取代ReLU非线性，以提高性能。SwiGLU激活函数的实现如下：

SwiGLU⁡(x,W,V,b,c,β)=Swishβ⁡(xW+b)⊗(xV+c) \operatorname{SwiGLU}(x, W, V, b, c, \beta)=\operatorname{Swish}_\beta(x W+b) \otimes(x V+c)

其中 [  S](https://link.zhihu.com/?target=https%3A//github.com/huggingface/transformers/blob/main/src/transformers/activations.py%23L140)wish=x⋅σ(βx) \operatorname{Swish} = x \cdot \sigma(\beta x) ，   GLU(x)=σ(Wx+b)⊗(Vx+c) G L U(x)=\sigma(W x+b) \otimes(V x+c) 具体实现参见 这里 。

+   旋转嵌入(Rotary pos)[GPTNeo]

L[LaM](https://link.zhihu.com/?target=https%3A//nn.labml.ai/transformers/rope/index.html)A删除了绝对位置嵌入，取而代之的是在网络的每一层添加旋转位置嵌入（RoPE），RoPE的实现参见 这里 。

[当前 ](https://link.zhihu.com/?target=https%3A//github.com/huggingface/transformers/blob/main/src/transformers/models/llama/modeling_llama.py)HuggingFace 已经实现了 LLaMA 模型 代码 ，可通过以下方式直接调用：

```
from transformers import LlamaForCausalLM
USE_8BIT = True # use 8-bit quantization; otherwise, use fp16
model = LlamaForCausalLM.from_pretrained(
            &quot;pretrained/path&quot;,
            load_in_8bit=USE_8BIT,
            torch_dtype=torch.float16,
            device_map=&quot;auto&quot;,
        )
if not USE_8BIT:
    model.half()  # use fp16
model.eval()
```

### 1.2 Alpaca 如何使用小成本训练大模型

如下图所示，Stanford的研究者使用 52K 个 intruction-f[ollowin](https://link.zhihu.com/?target=https%3A//github.com/tatsu-lab/stanford_alpaca)g examples 来微调 LLaMA 7B 模型，从而生成了 Alpaca 7B。

Alpaca 团队使用 [self-instruct ](https://link.zhihu.com/?target=https%3A//github.com/yizhongw/self-instruct)提供的 175 个 prompts，调用 OpenAI 的 text-davinci-003 模型，利用 OpenAI 的模型来产生有价值的 instructions 。

![](img/441a3760ac242f5679422e81991536d0.png)

将 OpenAI 性能完备的模型作为 Teacher，来指导参数更少的 Alpaca 模型进行训练，大幅降低了训练成本 。其中调用 OpenAI API 的成本不到 500 美元，另外微调 7B 参数的 LLaMA 模型，使用云服务商提供的 8 块 80GB A100 显卡，训练 3 小时，消费不到 100 美元，因此整体成本是小于 600 美元。

## 二、Alpaca 的优化、训练及应用

### 2.1 alpaca-lora 的优化

[alpace-lora ](https://link.zhihu.com/?target=https%3A//github.com/tloen/alpaca-lora)是在alpaca的基础上把训练方式改成用lora训练，仅需要在消费级的GPU上经过数小时的训练，就可以达到和alpaca差不多的效果。

LoRA(Low-Rank Adaptation) 技术

[LoRA ](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2106.09685.pdf)的思想很简单，即在原始 Pretrained Weights 旁边增加一个旁路，做一个降维再升维的操作，来模拟所谓的 intrinsic rank 。训练的时候固定 Pretrained Weights 的参数，只训练降维矩阵  A 与升维矩阵  B 。而模型的输入输出维度不变，输出时将  BA 与 Pretrained Weights 的参数叠加。用随机高斯分布初始化  A ，用0矩阵初始化  B ，保证训练的开始此旁路矩阵依然是  0 矩阵。

具体来看，假设预训练的矩阵为  W_0 \in \mathbb{R}^{d \times k} ，它的更新可表示为：

W_0 + \Delta W = W_0 + BA, B \in \mathbb{R}^{d \times r}, A \in \mathbb{R}^{r \times k}

其中秩  r \ll min(d,k) 。

![](img/512fb83c21c8bba408ab8168829e38ca.png)

LoRA与Transformer的结合也很简单，仅在QKV attention的计算中增加一个旁路，而不动MLP模块。基于大模型的内在低秩特性，增加旁路矩阵来模拟全模型参数微调，LoRA通过简单有效的方案来达成轻量微调的目的，可以将现在的各种大模型通过轻量微调变成各个不同领域的专业模型。

### 2.2 PEFT(Parameter-Efficient Fine-Tuning) 方法

随着模型变得越来越大，在消费级硬件上对模型进行全部参数的微调变得不可行。此外，为每个下游任务独立存储和部署微调模型变得非常昂贵，因为微调模型与原始预训练模型的大小相同。PEFT 方法旨在解决这两个问题，PEFT 方法仅微调少量 (额外) 模型参数，同时冻结预训练 LLM 的大部分参数，从而大大降低了计算和存储成本。

[Huggi](https://link.zhihu.com/?target=https%3A//github.com/huggingface/peft)ngFace 开源的一个高效微调大模型的 PEFT 库，目前包含LoRA，Prefix Tuning，Prompt Tuning，P-Tuning 四种算法，下面简要介绍后三种：

+   [Prefix Tuning](https://link.zhihu.com/?target=https%3A//aclanthology.org/2021.acl-long.353.pdf)

Prefix Tuning 算法是根据 下游任务 "前缀指令文本" 的所有层的embeding表示，学习到的前缀指令文本向量可以挖掘大模型的潜力去引导模型完成特定任务。

![](img/5627bbd5958f8873431af224dae52575.png)

[P-Tuning v2 ](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2110.07602.pdf)在实际上就是 Prefix-tuning，在Prefix部分，每一层transformer的embedding输入需要被tuned。而P-tuning v1只有transformer第一层的embedding输入需要被tuned。此外，P-Tuning v2还包括以下改进：

+   移除了Reparamerization加速训练方式；

+   采用了多任务学习优化：基于多任务数据集的Prompt进行预训练，然后再适配的下游任务。

+   舍弃了词汇Mapping的Verbalizer的使用，重新利用[CLS]和字符标签，跟传统finetune一样利用cls或者token的输出做NLU，以增强通用性，可以适配到序列标注任务。

![](img/0897afc60b1a26d787fcecca06532213.png)

+   [P-Tuning](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2103.10385.pdf)

P-Tuning 算法和 Prefix Tuning 的想法很相似，想通过微调"指令文本",让指令文本去挖掘大模型的潜力去完成特定的任务。但是 P-Tuning 只学习 "指令文本" 输入层embeding的的表示。 为了增强 "指令文本"的连续性，采用了一个 MLP(LSTM) 的结果去encoding "指令文本"。从微调参数量来看只有 0.65% 比 Prefix Tuning 和 LoRA 这些在所有层都增加参数的方法要少。

![](img/fa3942aa3f1f6bee44e60206d4df3df7.png)

+   [Prompt Tuning](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2104.08691.pdf)

Prompt Tuning 算法和 P-Tuning 很像，且更简单，就是是根据 下游任务 "指令文本" 输入层embeding的的表示。 Prompt Tuning 没有增加任何的层，直接使用微调指令文本(prompt) 的embeding向量。

![](img/71ea7c417bff13b6c4ef86ddc1dac396.png)

### 2.3 使用 PEFT 训练 alpaca-lora

以下仅说明过程，完整代码见 [这里 ](https://link.zhihu.com/?target=https%3A//github.com/tloen/alpaca-lora/blob/main/finetune.py)。

step 1. 模块的加载及初始化

```
import torch
import transformers
from datasets import load_dataset
from peft import LoraConfig, get_peft_model, get_peft_model_state_dict, prepare_model_for_int8_training, set_peft_model_state_dict,
from transformers import LlamaForCausalLM, LlamaTokenizer 

model = LlamaForCausalLM.from_pretrained(base_model, load_in_8bit=True, torch_dtype=torch.float16, device_map=device_map,)
tokenizer = LlamaTokenizer.from_pretrained(base_model)
model = prepare_model_for_int8_training(model)

config = LoraConfig(
    r=lora_r,
    lora_alpha=lora_alpha,
    target_modules=lora_target_modules,
    lora_dropout=lora_dropout,
    bias=&quot;none&quot;,
    task_type=&quot;CAUSAL_LM&quot;,
)
model = get_peft_model(model, config)
```

step 2. 准备data 和 trainer 并进行训练

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
        optim=&quot;adamw_torch&quot;,
        evaluation_strategy=&quot;steps&quot; if val_set_size &gt; 0 else &quot;no&quot;,
        save_strategy=&quot;steps&quot;,
        eval_steps=200 if val_set_size &gt; 0 else None,
        save_steps=200,
        output_dir=output_dir,
        save_total_limit=3,
        load_best_model_at_end=True if val_set_size &gt; 0 else False,
        ddp_find_unused_parameters=False if ddp else None,
        group_by_length=group_by_length,
        report_to=&quot;wandb&quot; if use_wandb else None,
        run_name=wandb_run_name if use_wandb else None,
    ),
    data_collator=transformers.DataCollatorForSeq2Seq(
        tokenizer, pad_to_multiple_of=8, return_tensors=&quot;pt&quot;, padding=True
    ),
)

trainer.train(resume_from_checkpoint=resume_from_checkpoint)
```

### 2.4 更多类似模型

| model | Paper or Blog | Repo | Demo |
| --- | --- | --- | --- |
| Vicuna | [https:// lmsys.org/blog/](https://link.zhihu.com/?target=https%3A//lmsys.org/blog/2023-03-30-vicuna/)2023-03- 30-vicuna/ | [https:// github.c](https://link.zhihu.com/?target=https%3A//github.com/lm-sys/FastChat)om/lm-sys/FastC hat | https:// chat.lmsys.org/ |
| Baize (白泽) | [https:// arxiv.org/](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2304.01196.pdf)pdf/2304.0119 6.pdf | [https:// github.com/p](https://link.zhihu.com/?target=https%3A//github.com/project-baize/baize)roject-baiz e/baize | [h](https://link.zhihu.com/?target=https%3A//huggingface.co/spaces/project-baize/Baize-7B)ttps:// huggingface.co/spaces/p roject-baize/Baize-7B |
| ChatGLM | https:// chatglm.cn/blog | [https:// github.co](https://link.zhihu.com/?target=https%3A//github.com/THUDM/ChatGLM-6B)m/THUDM/ChatGL M-6B | https:// chatglm.cn/ |
| Koala (考拉) | [https:// bair.berkeley.edu/blog](https://link.zhihu.com/?target=https%3A//bair.berkeley.edu/blog/2023/04/03/koala/)/ 2023/04/03/koala/ | https:// koala.lmsys.org/ | [https:// github.com](https://link.zhihu.com/?target=https%3A//github.com/young-geng/EasyLM)/young-geng/E asyLM |
| WizardLM | [https:// github.com](https://link.zhihu.com/?target=https%3A//github.com/nlpxucan/WizardLM)/nlpxucan/Wiz ardLM | [https:// github.com](https://link.zhihu.com/?target=https%3A//github.com/nlpxucan/WizardLM)/nlpxucan/Wiz ardLM | [https:// 36e2000522](https://link.zhihu.com/?target=https%3A//36e20005221d21bf.gradio.app/)1d21bf.gradio.app / |
| Wombat | [https:// arxiv.org/](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2304.05302.pdf)pdf/2304.0530 2.pdf | [https:// github.c](https://link.zhihu.com/?target=https%3A//github.com/GanjinZero/RRHF)om/GanjinZero/R RHF |  |
| BELLE | [https:// arxiv.org/](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2304.07854.pdf)pdf/2304.0785 4.pdf | [https:// github.com](https://link.zhihu.com/?target=https%3A//github.com/LianjiaTech/BELLE)/LianjiaTech/ BELLE |  |
| Luotuo (骆驼，Chinese) |  | [https:// github.com/LC1332/](https://link.zhihu.com/?target=https%3A//github.com/LC1332/Luotuo-Chinese-LLM)Luotu o-Chinese-LLM |  |
| GPT4All | [https:// s3.amazonaws.com/static .nomic.ai/gp](https://link.zhihu.com/?target=https%3A//s3.amazonaws.com/static.nomic.ai/gpt4all/2023_GPT4All_Technical_Report.pdf)t4all/2023_GPT4All_Technical_Report.pdf | [https:// github.co](https://link.zhihu.com/?target=https%3A//github.com/nomic-ai/gpt4all)m/nomic-ai/gpt 4all |  |
| LMFlow |  | [https:// github.com/O](https://link.zhihu.com/?target=https%3A//github.com/OptimalScale/LMFlow)ptimalScale /LMFlow | https:// lmflow.com/ |
| Firefly(流萤, Chinese) |  | [https:// github.com/ya](https://link.zhihu.com/?target=https%3A//github.com/yangjianxin1/Firefly)ngjianxin1 /Firefly |  |
| PandaLM |  | [https:// github.com/WeOpenML](https://link.zhihu.com/?target=https%3A//github.com/WeOpenML/PandaLM/tree/main)/Pan daLM/tree/main |  |

## 三、ColossalChat：深入体验 RLHF 在大模型中的功能

[ColossalChat ](https://link.zhihu.com/?target=https%3A//github.com/hpcaitech/ColossalAI/tree/main/applications/Chat)是第一[个基于LL](https://link.zhihu.com/?target=https%3A//chat.colossalai.org/)aMA预训练模型开源完整RLHF pipline实现，包括有监督数据收集、有监督微调、奖励模型训练和强化学习微调。只需要不到100亿个参数，就可以在大型语言模型的基础上通过RLHF微调达到中英文双语水平，达到与ChatGPT和GPT-3.5相当的效果，并可以进行 Demo 测试。关于RLHF的原理，可参考

[  ](https://zhuanlan.zhihu.com/p/613315873)

ColossalChat使用 InstructionWild 双语数据集，包含约52K的英语和52K的汉语问答，数据集通过OpenAI API接口获得。该数据集适用于微调和RLHF训练。通过提供高质量的数据，ColossalChat可以实现更好的对话互动，也可以支持中文。

![](img/73cb352be283ddb159aaf82b513c0cda.png)

### 3.1 监督指令微调

使用前面提到的数据集执行有监督指令微调，以微调模型。运行examples/train_sft.sh来启动有监督的指令微调。

```
torchrun --standalone --nproc_per_node=4 train_sft.py \
    --pretrain &quot;/path/to/LLaMa-7B/&quot; \
    --model &#x27;llama&#x27; \
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

运行examples/train_rm.sh开始奖励模型训练。

```
torchrun --standalone --nproc_per_node=4 train_reward_model.py
    --pretrain &quot;/path/to/LLaMa-7B/&quot; \
    --model &#x27;llama&#x27; \
    --strategy colossalai_zero2 \
    --loss_fn &#x27;log_exp&#x27;\
    --save_path &#x27;rmstatic.pt&#x27; \
```

### 3.3 人类反馈强化学习

在第一阶段的监督微调模型和第二阶段的奖励模型的基础上，使用强化学习算法进一步训练大型语言模型。该阶段是RLHF训练的核心部分，在强化学习中使用近端策略优化（PPO）算法来引入奖励信号，并生成更符合人类偏好的内容。

![](img/b7e0b51683148efc3338cde2cda9e8b7.png)

运行examples/train_prompts.sh，开始使用人类反馈训练PPO。

```
torchrun --standalone --nproc_per_node=4 train_prompts.py \
         --pretrain &quot;/path/to/LLaMa-7B/&quot; \
         --model &#x27;llama&#x27; \
         --strategy colossalai_zero2 \
         --prompt_path /path/to/your/prompt_dataset \
         --pretrain_dataset /path/to/your/pretrain_dataset \
         --rm_pretrain /your/pretrain/rm/defination \
         --rm_path /your/rm/model/path
```

## 参考资料

[1] [L](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2302.13971v1)LaMA: Open and Efficient Foundation Language Models.

[2] [A](https://link.zhihu.com/?target=https%3A//crfm.stanford.edu/2023/03/13/alpaca.html)lpaca: A Strong, Replicable Instruction-Following Model

[3] [https:// github.com/tatsu-l](https://link.zhihu.com/?target=https%3A//github.com/tatsu-lab/stanford_alpaca)ab/st anford_alpaca

[4] [Stanford CRFM](https://link.zhihu.com/?target=https%3A//crfm.stanford.edu/2023/03/13/alpaca.html)

[5] [tloen/alpaca-lora: Instruct-tune](https://link.zhihu.com/?target=https%3A//github.com/tloen/alpaca-lora) LLaMA on consumer hardware (github.com)

[6] [LORA: LOW-RANK ADAPTATION OF L](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2106.09685.pdf)ARGE LANGUAGE MODELS

[7] [ColossalAI/applications/Chat at ](https://link.zhihu.com/?target=https%3A//github.com/hpcaitech/ColossalAI/tree/main/applications/Chat)main · hpcaitech/ColossalAI (github.com)

> 城中桃李愁风雨，春在溪头荠菜花。—— 辛弃疾《鹧鸪天·陌上柔桑破嫩芽》