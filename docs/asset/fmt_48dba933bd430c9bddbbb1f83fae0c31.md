
```
class Linear(nn.Linear, LoraLayer):
    # LoRA 层
    def __init__(
        self,
        in_features: int,
        out_features: int,
        r: int = 0,
        lora_alpha: int = 1,
        lora_dropout: float = 0.0,
        fan_in_fan_out: bool = False, 
        merge_weights: bool = True,
        **kwargs,
    ):
        # 继承两个基类的构造函数
        nn.Linear.__init__(self, in_features, out_features, **kwargs)
        LoraLayer.__init__(self, r=r, lora_alpha=lora_alpha, lora_dropout=lora_dropout, merge_weights=merge_weights)

        self.fan_in_fan_out = fan_in_fan_out
        # Actual trainable parameters
        if r > 0:
            # 参数矩阵 A
            self.lora_A = nn.Linear(in_features, r, bias=False)
            # 参数矩阵 B
            self.lora_B = nn.Linear(r, out_features, bias=False)
            # 归一化系数
            self.scaling = self.lora_alpha / self.r
            # 冻结原参数，仅更新 A 和 B
            self.weight.requires_grad = False
        # 初始化 A 和 B
        self.reset_parameters()
        if fan_in_fan_out:
            self.weight.data = self.weight.data.T
```

替换时，直接将原层的 `weight` 和 `bias` 复制给新的 LoRA 层，再将新的 LoRA 层分配到指定设备即可。

### （4）训练

实现了 LoRA 层的替换后，进行微调训练即可。由于在 LoRA 层中已冻结原参数，在训练中只有 `A` 和 `B` 的参数会被更新，从而实现了高效微调。训练的整体过程与原 Fine-tune 类似，此处不再赘述。由于采用了 LoRA 方式，`forward` 函数也会对应调整：

```
    def forward(self, x: torch.Tensor):
        if self.disable_adapters:
            if self.r > 0 and self.merged:
                self.weight.data -= (
                    transpose(self.lora_B.weight @ self.lora_A.weight, self.fan_in_fan_out) * self.scaling
                )
                self.merged = False

            return F.linear(x, transpose(self.weight, self.fan_in_fan_out), bias=self.bias)
        '''主要分支'''
        elif self.r > 0 and not self.merged:
            result = F.linear(x, transpose(self.weight, self.fan_in_fan_out), bias=self.bias)
            if self.r > 0:
                result += self.lora_B(self.lora_A(self.lora_dropout(x))) * self.scaling
            return result
        else:
            return F.linear(x, transpose(self.weight, self.fan_in_fan_out), bias=self.bias)
```

上述代码由于考虑到参数合并问题，有几个分支，此处我们仅阅读第二个分支即 `elif` 分支即可。基于 LoRA 的前向计算过程如前文公式所示，首先计算原参数与输入的乘积，再加上 `A`、`B` 分别与输入的乘积即可。

### 6.3.5 使用 peft 实现 LoRA 微调

`peft` 进行了很好的封装，支持我们便捷、高效地对大模型进行微调。此处以第二节的 LLM SFT 为例，简要介绍如何使用 `peft` 对大模型进行微调。如果是应用在 RLHF 上，整体思路是一致的。

首先加载所需使用库：

```
import torch.nn as nn
from transformers import AutoTokenizer, AutoModel
from peft import get_peft_model, LoraConfig, TaskType, PeftModel
from transformers import Trainer
```

其次加载原模型与原 `tokenizer`，此处和第二节一致：

```
# 加载基座模型
tokenizer = AutoTokenizer.from_pretrained(MODEL_PATH, trust_remote_code=True)
model = AutoModel.from_pretrained(
    MODEL_PATH, trust_remote_code=True
)
```

接着，设定 `peft` 参数：

```
peft_config = LoraConfig(
            task_type=TaskType.CAUSAL_LM,
            inference_mode=False,
            r=8,
            lora_alpha=32,
            lora_dropout=0.1,
        )
```

注意，对不同的模型，LoRA 参数可能有所区别。例如，对于 ChatGLM，无需指定 `target_modeules`，`peft` 可以自行找到；对于 BaiChuan，就需要手动指定。`task_type` 是模型的任务类型，大模型一般都是 `CAUSAL_LM` 即传统语言模型。

然后获取 LoRA 模型：

```
model = get_peft_model(model, peft_config)
```

此处的 `get_peft_model` 的底层操作，即为上文分析的具体实现。

最后使用 `transformers` 提供的 `Trainer` 进行训练即可，训练占用的显存就会有大幅度的降低：

```
trainer = Trainer(
    model=model,
    args=training_args,
    train_dataset= IterableWrapper(train_dataset),
    tokenizer=tokenizer
)
trainer.train()
```

如果是应用在 DPO、KTO 上，则也相同的加入 LoRA 参数并通过 `get_peft_model` 获取一个 LoRA 模型即可，其他的不需要进行任何修改。但要注意的是，LoRA 微调能够大幅度降低显卡占用，且在下游任务适配上能够取得较好的效果，但如果是需要学习对应知识的任务，LoRA 由于只调整低秩矩阵，难以实现知识的注入，一般效果不佳，因此不推荐使用 LoRA 进行模型预训练或后训练。
