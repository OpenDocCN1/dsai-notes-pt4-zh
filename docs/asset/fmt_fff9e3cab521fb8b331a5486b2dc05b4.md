
```
TrainOutput(global_step=233, training_loss=3.3132195984345136, metrics={'train_runtime': 4321.001, 'train_samples_per_second': 0.863, 'train_steps_per_second': 0.054, 'total_flos': 2.225441122839429e+17, 'train_loss': 3.3132195984345136, 'epoch': 0.9989281886387996})
```

训练完成后，打开 SwanLab ，可以查看训练过程中记录的参数和可视化的训练 `loss` 曲线：

![](img/7a94afb6cb7f200f495f7cc606448be4.png)

示例的训练记录公开链接如下，供参考

[图](https://swanlab.cn/@xsir/Hunyuan-A13B-Instruct-Lora/runs/5shgkkqx8ye605v51ahls/chart)表 ｜ Hunyuan-A13B-Instruct-Lora/Hunyuan-A13B-Instruct-LoRA-experiment

## 加载 lora 权重推理

得到任意 checkpoints 之后加载 lora 权重进行推理：

```python
from transformers import AutoModelForCausalLM, AutoTokenizer
import torch
from peft import PeftModel

mode_path = '/root/autodl-tmp/Tencent-Hunyuan/Hunyuan-A13B-Instruct' # 这里修改实际的模型路径
lora_path = '/root/output/Hunyuan-A13B-Instruct/checkpoint-233' # 这里改称你的 lora 输出对应 checkpoint 地址

# 加载tokenizer
tokenizer = AutoTokenizer.from_pretrained(mode_path, trust_remote_code=True)

# 加载模型
model = AutoModelForCausalLM.from_pretrained(mode_path, device_map="auto", torch_dtype=torch.bfloat16, trust_remote_code=True)

# 加载lora权重
model = PeftModel.from_pretrained(model, model_id=lora_path)
import re
messages = [
    {"role": "user", "content": "who are you?"},
]
tokenized_chat = tokenizer.apply_chat_template(messages, tokenize=True, return_tensors="pt",
                                                enable_thinking=False # Toggle thinking mode (default: True)
                                                )

outputs = model.generate(tokenized_chat.to(model.device), max_new_tokens=4096)

output_text = tokenizer.decode(outputs[0])

answer_pattern = r'<answer>(.*?)</answer>'
answer_matches = re.findall(answer_pattern, output_text, re.DOTALL)

answer_content = [match.strip() for match in answer_matches][0]
print(f"answer_content:{answer_content}\n\n")
```

```
answer_content:I'm HunYuan, Tencent's AI assistant. I'm here to help you with any questions or tasks you have. How can I assist you today?
```
