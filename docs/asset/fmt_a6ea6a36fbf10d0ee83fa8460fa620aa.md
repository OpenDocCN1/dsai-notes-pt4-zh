
```
<|start|>system<|message|>You are ChatGPT, a large language model trained by OpenAI.
Knowledge cutoff: 2024-06
Current date: 2025-08-25

Reasoning: low

# Valid channels: analysis, commentary, final. Channel must be included for every message.
Calls to these tools must go to the commentary channel: 'functions'.<|end|><|start|>developer<|message|># Instructions

You are a deep thinking AI, you may use extremely long chains of thought to deeply consider the problem and deliberate with yourself via systematic reasoning processes to help come to a correct solution prior to answering.

# Tools

## functions

namespace functions {

// Checks the availability of a hotel for a given date range.
type is_hotel_available = (_: {
// The name of the hotel.
hotel: string,
// The city where the hotel is located.
city: string,
// The check-in date in the format "YYYY-MM-DD".
checkin: string,
// The check-out date in the format "YYYY-MM-DD".
checkout: string
}) => any;

} // namespace functions<|end|><|start|>user<|message|>I want to book a hotel in Berlin from 3/15/2022 to 3/20/2022. Is the Queens Hotel available?<|end|><|start|>assistant to=functions.is_hotel_available<|channel|>commentary json<|message|>{"hotel": "Queens Hotel", "city": "Berlin, Germany", "checkin": "2022-03-15", "checkout": "2022-03-20"}<|call|><|start|>functions.is_hotel_available to=assistant<|channel|>commentary<|message|>{"name": "is_hotel_available", "content": {"hotel": "Queens Hotel", "city": "Berlin, Germany", "checkin": "2022-03-15", "checkout": "2022-03-20", "availability": "Available", "rooms_left": 5, "price_per_night": 189, "total_price": 945, "cancellation_policy": "Flexible until 5 days before arrival", "special_offers": []}}<|end|>
```

## Step 3. LoRA 参数灌注

```
model = FastLanguageModel.get_peft_model(
    model,
    r = 8, # Choose any number > 0 ! Suggested 8, 16, 32, 64, 128
    target_modules = ["q_proj", "k_proj", "v_proj", "o_proj",
                      "gate_proj", "up_proj", "down_proj",],
    lora_alpha = 16,
    lora_dropout = 0, # Supports any, but = 0 is optimized
    bias = "none",    # Supports any, but = "none" is optimized
    # [NEW] "unsloth" uses 30% less VRAM, fits 2x larger batch sizes!
    use_gradient_checkpointing = "unsloth", # True or "unsloth" for very long context
    random_state = 3407,
    use_rslora = False,  # We support rank stabilized LoRA
    loftq_config = None, # And LoftQ
)
```

```
Unsloth: Making `model.base_model.model.model` require gradients
```

基本参数说明如下：

| 参数 | 含义 | 推荐值/当前设置 | 作用与说明 |
| --- | --- | --- | --- |
| `r` | LoRA 秩（`rank`） | `8`（可选 `8`/`16`/`32`/`64`/`128`） | 控制新增参数规模；值越大表达能力越强，但显存占用也随之上升。中文推理链推荐从 `8`/`16` 起步。 |
| `target_modules` | 注入的线性层 | `q`/`k`/`v`/`o` 投影 + `MLP` (`gate`/`up`/`down`) | 指定在哪些层挂 LoRA。覆盖注意力 + MLP → 更好适配中文句法与逻辑；若显存不足可只保留 `q`/`k`/`v`/`o`。 |
| `lora_alpha` | LoRA 缩放系数 | `16` | 放大 LoRA 分支输出，等效调整学习率。常用范围 `16`–`32`，`r` 小时可适当调高。 |
| `lora_dropout` | LoRA dropout 概率 | `0` | 控制 LoRA 分支的随机丢弃率。`0` 在 `Unsloth` 中优化过，更快更省；若过拟合可尝试 `0.05`–`0.1`。 |
| `bias` | Bias 参数训练策略 | `"none"` | 是否对注入层 bias 可训练。`"none"` 开销最小且优化最好。 |
| `use_gradient_checkpointing` | 梯度检查点 | `"unsloth"` | 节省 ~30% 显存，支持更长上下文和更大 batch；推荐开启。 |
| `random_state` | 随机种子 | `3407` | 保证实验可复现；任意固定值均可。 |
| `use_rslora` | Rank-Stabilized LoRA | `False` | 在大 `r`（≥64）时可提高稳定性；一般场景不必启用。 |
| `loftq_config` | LoftQ 量化配置 | `None` | 结合量化训练 LoRA，在 `4`/`8`bit 推理时更稳。默认关闭；若目标是低比特部署，可配置启用。 |

## Step 4. 设置微调参数

```
ds_text
```

```
Dataset({
    features: ['messages_json', 'tools_json', 'text'],
    num_rows: 200
})
```

```
from trl import SFTConfig, SFTTrainer
trainer = SFTTrainer(
    model = model,
    tokenizer = tokenizer,
    train_dataset = ds_text,
    args = SFTConfig(
        max_seq_length = 4096,
        dataset_text_field = "text",   # 你上一步 map 生成了 text 列
        packing = False,               # 一般先关掉 packing，避免拼接样本
        per_device_train_batch_size = 1,
        gradient_accumulation_steps = 4,
        warmup_steps = 5,
        num_train_epochs = 1, # Set this for 1 full training run.
        learning_rate = 2e-4,
        optim = "adamw_8bit",
        weight_decay = 0.01,
        lr_scheduler_type = "linear",
        seed = 3407,
        output_dir = "outputs",
        report_to="tensorboard",     # 📌 启用 TensorBoard
        logging_dir="./logs",        # 日志目录
        logging_strategy="steps",
        logging_steps=1,            # 记录频率
    ),
)
```

```
Unsloth: Tokenizing ["text"] (num_proc=2): 100%|██████████| 200/200 [00:01<00:00, 128.94 examples/s]
```

## Step 5. 开始微调

```
trainer_stats = trainer.train()
```

```
The tokenizer has new PAD/BOS/EOS tokens that differ from the model config and generation config. The model config and generation config were aligned accordingly, being updated with the tokenizer's values. Updated tokens: {'bos_token_id': 199998}.
==((====))==  Unsloth - 2x faster free finetuning | Num GPUs used = 1
   \   /|    Num examples = 200 | Num Epochs = 1 | Total steps = 50
O^O/ \_/ \    Batch size per device = 1 | Gradient accumulation steps = 4
\        /    Data Parallel GPUs = 1 | Total batch size (1 x 4 x 1) = 4
 "-____-"     Trainable parameters = 3,981,312 of 20,918,738,496 (0.02% trained)
/root/miniconda3/envs/unsloth/lib/python3.12/site-packages/torch/_inductor/runtime/triton_helpers.py:150: UserWarning: Logical operators 'and' and 'or' are deprecated for non-scalar tensors; please use '&' or '|' instead
  mask |= a_isnan and not b_isnan
UserWarning: Enable tracemalloc to get the object allocation traceback
/root/miniconda3/envs/unsloth/lib/python3.12/site-packages/torch/_inductor/runtime/triton_helpers.py:150: UserWarning: Logical operators 'and' and 'or' are deprecated for non-scalar tensors; please use '&' or '|' instead
  mask |= a_isnan and not b_isnan
UserWarning: Enable tracemalloc to get the object allocation traceback
/root/miniconda3/envs/unsloth/lib/python3.12/site-packages/torch/_inductor/runtime/triton_helpers.py:152: UserWarning: Logical operators 'and' and 'or' are deprecated for non-scalar tensors; please use '&' or '|' instead
  equal |= a_isnan and b_isnan
UserWarning: Enable tracemalloc to get the object allocation traceback
/root/miniconda3/envs/unsloth/lib/python3.12/site-packages/torch/_inductor/runtime/triton_helpers.py:152: UserWarning: Logical operators 'and' and 'or' are deprecated for non-scalar tensors; please use '&' or '|' instead
  equal |= a_isnan and b_isnan
UserWarning: Enable tracemalloc to get the object allocation traceback

Unsloth: Will smartly offload gradients to save VRAM!

<div>

  <progress value='50' max='50' style='width:300px; height:20px; vertical-align: middle;'></progress>
  [50/50 03:47, Epoch 1/1]
</div>
<table border="1" class="dataframe">
```
