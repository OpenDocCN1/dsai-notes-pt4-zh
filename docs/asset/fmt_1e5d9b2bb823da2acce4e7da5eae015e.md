
```
Qwen3MoeForCausalLM(
  (model): Qwen3MoeModel(
    (embed_tokens): Embedding(151936, 2048)
    (layers): ModuleList(
      (0-47): 48 x Qwen3MoeDecoderLayer(
        (self_attn): Qwen3MoeAttention(
          (q_proj): Linear(in_features=2048, out_features=4096, bias=False)
          (k_proj): Linear(in_features=2048, out_features=512, bias=False)
          (v_proj): Linear(in_features=2048, out_features=512, bias=False)
          (o_proj): Linear(in_features=4096, out_features=2048, bias=False)
          (q_norm): Qwen3MoeRMSNorm((128,), eps=1e-06)
          (k_norm): Qwen3MoeRMSNorm((128,), eps=1e-06)
        )
        (mlp): Qwen3MoeSparseMoeBlock(
          (gate): Linear(in_features=2048, out_features=128, bias=False)
          (experts): ModuleList(
            (0-127): 128 x Qwen3MoeMLP(
              (gate_proj): Linear(in_features=2048, out_features=768, bias=False)
              (up_proj): Linear(in_features=2048, out_features=768, bias=False)
              (down_proj): Linear(in_features=768, out_features=2048, bias=False)
              (act_fn): SiLU()
            )
          )
        )
        (input_layernorm): Qwen3MoeRMSNorm((2048,), eps=1e-06)
        (post_attention_layernorm): Qwen3MoeRMSNorm((2048,), eps=1e-06)
      )
    )
    (norm): Qwen3MoeRMSNorm((2048,), eps=1e-06)
    (rotary_emb): Qwen3MoeRotaryEmbedding()
  )
  (lm_head): Linear(in_features=2048, out_features=151936, bias=False)
)
```

上面打印了 `Qwen3MoeForCausalLM` 的模型结构，可以看到里面的 `self_attn` 和 `mlp` 是两个主要的模块，因此可以考虑将这两个模块作为 LoRA 微调 的 `target_modules` ，包括 `q_proj`, `k_proj`, `v_proj`, `o_proj` 以及 `gate_proj`、`up_proj` 和 `down_proj` 。

通常我们只对 `self_attn` 模块中的 `q_proj`, `k_proj`, `v_proj`, `o_proj` 进行微调，本教程里我们也将对这四个模块进行微调演示，感兴趣的同学可以自行尝试添加对 `mlp` 中的三个 proj 模块进行微调。

## 6. 定义 `LoraConfig`

`LoraConfig` 类用于设置 LoRA 微调参数，虽然可以设置很多参数，但主要的参数没多少，简单讲一讲，感兴趣的同学可以直接看源码。

+   `task_type`：模型类型

+   `target_modules`：需要训练的模型层的名字，主要就是 attention部分的层，不同的模型对应的层的名字不同，可以传入数组，也可以字符串，也可以正则表达式。

+   `r`：LoRA的秩，具体可以看 LoRA原理。

+   `lora_alpha`：LoRA alaph ，具体作用参见 LoRA 原理。

+   `lora_dropout`: LoRA 层的 Dropout 比例，用于防止过拟合，具体作用参见 LoRA 原理。

LoRA的缩放是啥嘞？当然不是 `r`（秩），这个缩放就是 `lora_alpha/r`，在这个 `LoraConfig` 中缩放就是 4 倍。

```
from peft import LoraConfig, TaskType, get_peft_model

config = LoraConfig(
    task_type=TaskType.CAUSAL_LM,
    target_modules=["q_proj", "k_proj", "v_proj", "o_proj", "gate_proj", "up_proj", "down_proj"], # 可以自行添加更多微调的target_modules
    inference_mode=False,     # 训练模式
    r=8,                      # LoRA 秩
    lora_alpha=32,            # LoRA alaph，具体作用参见 LoRA 原理
    lora_dropout=0.1          # Dropout 比例
)
```

## 7. 自定义 `TrainingArguments` 参数

`TrainingArguments` 类用于设置微调训练过程中的配置参数，这个类的源码也介绍了每个参数的具体作用，当然大家可以来自行探索，这里就简单说几个常用的。

+   `output_dir`：模型的输出路径

+   `per_device_train_batch_size`：顾名思义 `batch_size`，批量大小

+   `gradient_accumulation_steps`: 梯度累加，如果你的显存比较小，那可以把 `batch_size` 设置小一点，梯度累加增大一些。

+   `logging_steps`：多少步，输出一次 log

+   `num_train_epochs`：顾名思义 epoch，训练轮次

+   `gradient_checkpointing`：梯度检查，这个一旦开启，模型就必须执行 `model.enable_input_require_grads()`，这个原理大家可以自行探索，这里就不细说了。

```
args = TrainingArguments(
    output_dir=&quot;./output/Qwen3_30B_A3B_lora&quot;,
    per_device_train_batch_size=16,
    gradient_accumulation_steps=1,
    logging_steps=10,
    num_train_epochs=1,
    save_steps=100,
    learning_rate=1e-4,
    save_on_each_node=True,
    gradient_checkpointing=True,
    report_to=&quot;none&quot;,
)
```

## 8. SwanLab 可视化

### SwanLab 简介

[SwanLab](https://github.com/swanhubx/swanlab) 是一个开源的模型训练记录工具，面向 AI 研究者，提供了训练可视化、自动日志记录、超参数记录、实验对比、多人协同等功能。在 SwanLab 上，研究者能基于直观的可视化图表发现训练问题，对比多个实验找到研究灵感，并通过在线链接的分享与基于组织的多人协同训练，打破团队沟通的壁垒。

### 为什么要记录训练

相较于软件开发，模型训练更像一个实验科学。一个品质优秀的模型背后，往往是成千上万次实验。研究者需要不断尝试、记录、对比，积累经验，才能找到最佳的模型结构、超参数与数据配比。在这之中，如何高效进行记录与对比，对于研究效率的提升至关重要。

### 实例化 `SwanLabCallback`

建议先在 [SwanLab 官网](https://swanlab.cn/) 注册账号，然后在训练初始化阶段选择

(2) Use an existing SwanLab account 并使用 private API Key 登录

SwanLab 与 Transformers 已经做好了集成，用法是在 `Trainer` 的 `callbacks` 参数中添加 `SwanLabCallback` 实例，就可以自动记录超参数和训练指标，简化代码如下：

```
import swanlab
from swanlab.integration.transformers import SwanLabCallback

swanlab_callback = SwanLabCallback(
    project=&quot;Qwen3-8B&quot;,
    experiment_name=&quot;Qwen3-30B-A3B-Lora&quot;
)
```

## 9. 使用 `Trainer` 训练

我们使用 `Trainer` 类来管理训练过程。`TrainingArguments` 用于设置训练参数，`Trainer` 则负责实际的训练逻辑。

```
trainer = Trainer(
    model=model,                 # 要训练的模型
    args=args,                   # 训练参数
    train_dataset=tokenized_id,  # 训练数据集
    data_collator=DataCollatorForSeq2Seq(tokenizer=tokenizer, padding=True),
    callbacks=[swanlab_callback]
    # 数据整理器
)
trainer.train()                  # 开始训练
```

看到下面的进度条即代表训练开始：

![](img/0ac6f793464475a621fc0a4c65e99f12.png)

## 10. 训练结果演示

访问可视化训练过程：`Qwen3-8B/Qwen3_30B_A3B-Lora`

在 SwanLab 上查看最终的训练结果：

可以看到在 1 个 epoch 之后，微调后的 `Qwen3-30B-A3B` 的 `loss` 降低到了不错的水平。

![](img/764a58ce7a567f0ea5ba61f53904ac24.png)

至此，你已经完成了 `Qwen3-30B-A3B` Lora 微调的训练！如果需要加强微调效果，可以尝试增加训练的数据量。

## 11. 加载 `LoRA` 权重推理

训练好了之后可以使用如下方式加载 `LoRA` 权重进行推理：

```
from transformers import AutoModelForCausalLM, AutoTokenizer
import torch
from peft import PeftModel

model_path = &#x27;Qwen/Qwen3-30B-A3B&#x27;
lora_path = &#x27;output/Qwen3_30B_A3B_lora/checkpoint-234&#x27; # 这里改称你的 lora 输出对应 checkpoint 地址

# 加载tokenizer
tokenizer = AutoTokenizer.from_pretrained(model_path, trust_remote_code=True)


# 加载模型
model = AutoModelForCausalLM.from_pretrained(model_path,
                                            device_map=&quot;auto&quot;,
                                            torch_dtype=torch.bfloat16,
                                            trust_remote_code=True).eval()

# 加载lora权重
model = PeftModel.from_pretrained(model, model_id=lora_path)
prompt = "你是谁？"
system_prompt = "现在你要扮演皇帝身边的女人--甄嬛"
print("prompt: ", prompt)
print("system_prompt: ", system_prompt)

inputs = tokenizer.apply_chat_template([{"role": "system", "content": system_prompt},
                                        {"role": "user", "content": prompt}],
                                       add_generation_prompt=True,
                                       tokenize=True,
                                       return_tensors="pt",
                                       return_dict=True
                                       ).to(model.device)  # 将 inputs 移动到模型所在的设备，确保设备一致性


gen_kwargs = {"max_length": 2500, "do_sample": True, "top_k": 1}
with torch.no_grad():
    outputs = model.generate(**inputs, **gen_kwargs)
    outputs = outputs[:, inputs['input_ids'].shape[1]:]
    print(tokenizer.decode(outputs[0], skip_special_tokens=True))
```

```
prompt:  你是谁？
system_prompt:  现在你要扮演皇帝身边的女人--甄嬛
我是甄嬛，家父是大理寺少卿甄远道。
```

> 注意修改为自己的模型路径哦~

> 如果显示 `Some parameters are on the meta device because they were offloaded to the cpu.` 的报错，需要将实例关机，重启后单独运行本条代码。
