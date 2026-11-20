
# 02_trainmodel.ipynb

> 来源：[https://v11enp9ok1h.feishu.cn/docx/EFHvd7diioSqJ8xTfEvc7o1Znje](https://v11enp9ok1h.feishu.cn/docx/EFHvd7diioSqJ8xTfEvc7o1Znje)

![](img/927845bba867e2bed68748ac235bb32c.png)

[code_02_trainmodel.ipynb](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/GEZWbJOx5oC1T8x4yDbc7JoxnAH)

## 导入包

```python
from pathlib import Path
import shutil
from transformers import AutoModelForSequenceClassification, TrainingArguments, Trainer
from transformers import DataCollatorWithPadding
from transformers import AutoTokenizer
from datasets import load_dataset
import evaluate
import numpy as np
import pandas as pd
import os

metric = evaluate.load("accuracy")
```

## 设置

1. `MODEL_NAME_OR_PATH`: 大模型的名称，这里使用的是 `"hfl/chinese-roberta-wwm-ext"`
2. `NUM_LABELS`: 文本分类的类别数量，这个是按需而定，我这里分类是只有两类，因此就设置为 2
3. `MAX_LENGTH`: 每一条文本的长度，这里设置为 64。一般来说长度 64 就已经覆盖大部分场景了，128 也是可以。但是 128 对显存要求更高一点。

```python
# "bert-base-chinese"  # "distilbert-base-uncased"
MODEL_NAME_OR_PATH = "hfl/chinese-roberta-wwm-ext"
NUM_LABELS = 2
MAX_LENGTH = 64

model = AutoModelForSequenceClassification.from_pretrained(
    MODEL_NAME_OR_PATH, num_labels=NUM_LABELS)

tokenizer = AutoTokenizer.from_pretrained(MODEL_NAME_OR_PATH)
```

```python
text_dataset = load_dataset('csv', data_files={
    'train': ['data_all/data/train_data.csv'],
    'test': ['data_all/data/test_data.csv']})

def preprocess_function(examples):
    return tokenizer(examples["text"], truncation=True, max_length=MAX_LENGTH)

# 在实际工程中，会先使用`Tokenizer`把所有的文本转换成`input_ids`、`token_type_ids`、`attention_mask`，然后在训练的时候，这步就不再做了，目的是减少训练过程中cpu处理数据的时间，不给显卡休息时间。
tokenized_text = text_dataset.map(preprocess_function, batched=True)

data_collator = DataCollatorWithPadding(
    tokenizer=tokenizer, max_length=MAX_LENGTH)
```

## 模型训练设置

1. `LOGGING_DIR`: 就是一个储存日志的文件夹。
2. `MODEL_DIR`: 在模型训练的时候，会把每一阶段的模型都保存在文件夹下。

```python
LOGGING_DIR = "logging_dir"
MODEL_DIR = "model_result"
shutil.rmtree(LOGGING_DIR, ignore_errors=True)

training_args = TrainingArguments(
    output_dir=MODEL_DIR,
    overwrite_output_dir=True,
    logging_dir=LOGGING_DIR,
    learning_rate=2e-5,
    per_device_train_batch_size=32,  # 每一次batch，训练的数据的数量，如果显存高，可以32起步，如果一般，那可能就是个位数，比如2，4，8，16等。
    per_device_eval_batch_size=32,  # 在评估的时候，batch的大小，看显存大小了。
    do_eval=True,
    evaluation_strategy="steps",
    eval_accumulation_steps=50,
    eval_steps=50,
    logging_steps=50,
    save_steps=100,
    num_train_epochs=4,  #训练多少轮
    weight_decay=0.01,
    save_total_limit=3,  # 模型每`eval_steps`步，就会保存一下模型，只会保存最新的3个模型，
    jit_mode_eval=True,
    fp16=True,
    fp16_opt_level='O3',
    load_best_model_at_end=True,  # 最后，加载效果最好的模型
)
```

```python
def compute_metrics(eval_pred):
    logits, labels = eval_pred
    predictions = np.argmax(logits, axis=-1)
    return metric.compute(predictions=predictions, references=labels)

trainer = Trainer(
    model=model,
    args=training_args,
    train_dataset=tokenized_text["train"],
    eval_dataset=tokenized_text["test"],
    tokenizer=tokenizer,
    data_collator=data_collator,
    compute_metrics=compute_metrics
)

trainer.train()
```
