
# `train_chinese_gpt2.ipynb`

> 来源：`https://v11enp9ok1h.feishu.cn/docx/EIVcduz8OoXNfqx2WQHcEtNwnlg`

```
&quot;&lt;img src=\&quot;https://huggingface.co/datasets/huggingface-course/documentation-images/resolve/main/en/chapter7/chunking_texts.svg\&quot;/&gt;\n&quot;
```



# 加载包和处理数据

1. 我们只需要保证数据都含有`content`这一列（当然，数据是我处理的，肯定是有的）
2. 使用`glob`包，获得所有的数据的路径
3. 我们使用`random`包，从所有的文件路径中，随机找50个数据路径，作为训练集合的使用

```python
from datasets import load_dataset, DatasetDict
from glob import glob
import random
random.seed(42)

all_file_list = glob(pathname="gpt2_data/*/**")
test_file_list = random.sample(all_file_list, 50)
train_file_list = [i for i in all_file_list if i not in test_file_list]

len(train_file_list), len(test_file_list)
```

# 创建数据

1. 只要将路径放到一个字典里面。`dict`的`key`分别为`train`、`valid`，他们对应的值就是文件路径列表即可

```python
raw_datasets = load_dataset("csv", data_files={'train': train_file_list, 'valid': test_file_list}, cache_dir="cache_data")

raw_datasets
```

# Tokenizer

1. `Tokenizer` 是最关键的一步，因为我们处理的是中文，因此使用`bert_base_chinese`就足够了
2. 如果你的语料里面有别的语言，你也可以使用多语言。这个都无所谓的。只要保证你使用的`Tokenizer`能覆盖你的数据即可
3. `context_length = 512`设置你的每一个文本的最长长度，我这里设置的是512，如果你的显卡显寸小，那你可以改小一点，比如128。但是多出来的数据，并不是说直接截断不要了，而是按照`context_length`长度，不断的对文本进行截断，大概就像是下面这样的：

4. 对于`gpt2`模型，需要告诉模型一句话从哪里开始，从哪里结束。因此我们需要设置`bos_token`、`eos_token`、`unk_token`

```python
from transformers import AutoTokenizer, AutoConfig

context_length = 512
tokenizer = AutoTokenizer.from_pretrained("bert-base-chinese")

outputs = tokenizer(
    raw_datasets["train"][:2]["content"],
    truncation=True,
    max_length=context_length,
    return_overflowing_tokens=True,
    return_length=True,
)

print(f"Input IDs length: {len(outputs['input_ids'])}")
print(f"Input chunk lengths: {(outputs['length'])}")
print(f"Chunk mapping: {outputs['overflow_to_sample_mapping']}")
```

```python
tokenizer.add_special_tokens(special_tokens_dict={'bos_token': '<|endoftext|>',
 'eos_token': '<|endoftext|>',
 'unk_token': '<|endoftext|>'})
```

# 说明

1. 其实，到这里，我做的东西基本上就结束了。
2. 只要查看这个链接，说明的更加清楚：[https://huggingface.co/course/zh-CN/chapter7/6?fw=pt](https://huggingface.co/course/zh-CN/chapter7/6?fw=pt)，我也就是模仿这个链接的。

```python
def tokenize(element):
    outputs = tokenizer(
        element["content"],
        truncation=True,
        max_length=context_length,
        return_overflowing_tokens=True,
        return_length=True,
    )
    input_batch = []
    for length, input_ids in zip(outputs["length"], outputs["input_ids"]):
        if length == context_length:
            input_batch.append(input_ids)
    return {"input_ids": input_batch}

tokenized_datasets = raw_datasets.map(
    tokenize, batched=True, remove_columns=raw_datasets["train"].column_names
)
tokenized_datasets
```

# 模型

1. 这里的`gpt2`模型，可不是使用别人训练好的，就是一个`gpt2`配置，因为我们要使用这个从头训练一个新的`gpt2`

```python
from transformers import AutoTokenizer, GPT2LMHeadModel, AutoConfig

config = AutoConfig.from_pretrained(
    "gpt2",
    vocab_size=len(tokenizer),
    n_ctx=context_length,
    bos_token_id=tokenizer.bos_token_id,
    eos_token_id=tokenizer.eos_token_id,
)
```

```python
model = GPT2LMHeadModel(config)
model_size = sum(t.numel() for t in model.parameters())
print(f"GPT-2 size: {model_size/1000**2:.1f}M parameters")
```

```python
from transformers import DataCollatorForLanguageModeling

tokenizer.pad_token = tokenizer.eos_token
data_collator = DataCollatorForLanguageModeling(tokenizer, mlm=False)
```

```python
out = data_collator([tokenized_datasets["train"][i] for i in range(5)])
for key in out:
    print(f"{key} shape: {out[key].shape}")
```

```python
from transformers import Trainer, TrainingArguments

args = TrainingArguments(
    output_dir="chinese_gpt2_big",
    per_device_train_batch_size=20,
    per_device_eval_batch_size=16,
    evaluation_strategy="steps",
    eval_steps=2_000,
    logging_steps=2_000,
    gradient_accumulation_steps=8,
    num_train_epochs=2,
    weight_decay=0.1,
    warmup_steps=1_000,
    lr_scheduler_type="cosine",
    learning_rate=5e-4,
    save_steps=2_000,
    fp16=True,
    push_to_hub=False,
)

trainer = Trainer(
    model=model,
    tokenizer=tokenizer,
    args=args,
    data_collator=data_collator,
    train_dataset=tokenized_datasets["train"],
    eval_dataset=tokenized_datasets["valid"],
)
```

```python
trainer.train()
```
