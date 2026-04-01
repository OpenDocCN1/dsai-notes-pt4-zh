# 02_trainmodel.ipynb

> 来源：[https://v11enp9ok1h.feishu.cn/docx/EFHvd7diioSqJ8xTfEvc7o1Znje](https://v11enp9ok1h.feishu.cn/docx/EFHvd7diioSqJ8xTfEvc7o1Znje)

![](img/927845bba867e2bed68748ac235bb32c.png)

[code_02_trainmodel.ipynb](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/GEZWbJOx5oC1T8x4yDbc7JoxnAH)

```
{
 &quot;cells&quot;: [
  {
   &quot;cell_type&quot;: &quot;markdown&quot;,
   &quot;source&quot;: [
    &quot;## 导入包&quot;
   ],
   &quot;metadata&quot;: {
    &quot;collapsed&quot;: false
   }
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: null,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [],
   &quot;source&quot;: [
    &quot;from pathlib import Path\n&quot;,
    &quot;import shutil\n&quot;,
    &quot;from transformers import AutoModelForSequenceClassification, TrainingArguments, Trainer\n&quot;,
    &quot;from transformers import DataCollatorWithPadding\n&quot;,
    &quot;from transformers import AutoTokenizer\n&quot;,
    &quot;from datasets import load_dataset\n&quot;,
    &quot;import evaluate\n&quot;,
    &quot;import numpy as np\n&quot;,
    &quot;import pandas as pd\n&quot;,
    &quot;import os\n&quot;,
    &quot;\n&quot;,
    &quot;metric = evaluate.load(\&quot;accuracy\&quot;)\n&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;markdown&quot;,
   &quot;source&quot;: [
    &quot;## 设置\n&quot;,
    &quot;1. `MODEL_NAME_OR_PATH`:大模型的名称，这里使用的是`\&quot;hfl/chinese-roberta-wwm-ext\&quot;`\n&quot;,
    &quot;2. `NUM_LABELS`:文本分类的类别数量，这个是按需而定，我这里分类是只有两类，因此就设置为2\n&quot;,
    &quot;3. `MAX_LENGTH`:每一条文本的长度，这里设置为64。一般来说长度64就已经覆盖大部分场景了，128也是可以。但是128对显存要求更高一点。&quot;
   ],
   &quot;metadata&quot;: {
    &quot;collapsed&quot;: false
   }
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: null,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [],
   &quot;source&quot;: [
    &quot;# \&quot;bert-base-chinese\&quot;  # \&quot;distilbert-base-uncased\&quot;\n&quot;,
    &quot;MODEL_NAME_OR_PATH = \&quot;hfl/chinese-roberta-wwm-ext\&quot;\n&quot;,
    &quot;NUM_LABELS = 2\n&quot;,
    &quot;MAX_LENGTH = 64\n&quot;,
    &quot;\n&quot;,
    &quot;model = AutoModelForSequenceClassification.from_pretrained(\n&quot;,
    &quot;    MODEL_NAME_OR_PATH, num_labels=NUM_LABELS)\n&quot;,
    &quot;\n&quot;,
    &quot;tokenizer = AutoTokenizer.from_pretrained(MODEL_NAME_OR_PATH)\n&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: null,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [],
   &quot;source&quot;: [
    &quot;text_dataset = load_dataset(&#x27;csv&#x27;, data_files={\n&quot;,
    &quot;    &#x27;train&#x27;: [&#x27;data_all/data/train_data.csv&#x27;],\n&quot;,
    &quot;    &#x27;test&#x27;: [&#x27;data_all/data/test_data.csv&#x27;]})\n&quot;,
    &quot;\n&quot;,
    &quot;\n&quot;,
    &quot;def preprocess_function(examples):\n&quot;,
    &quot;    return tokenizer(examples[\&quot;text\&quot;], truncation=True, max_length=MAX_LENGTH)\n&quot;,
    &quot;\n&quot;,
    &quot;# 在实际工程中，会先使用`Tokenizer`把所有的文本转换成`input_ids`,`token_type_ids`,`attention_mask`，然后在训练的时候，这步就不再做了，目的是减少训练过程中cpu处理数据的时间，不给显卡休息时间。\n&quot;,
    &quot;tokenized_text = text_dataset.map(preprocess_function, batched=True)\n&quot;,
    &quot;\n&quot;,
    &quot;data_collator = DataCollatorWithPadding(\n&quot;,
    &quot;    tokenizer=tokenizer, max_length=MAX_LENGTH)\n&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;markdown&quot;,
   &quot;source&quot;: [
    &quot;## 模型训练设置\n&quot;,
    &quot;1. `LOGGING_DIR`: 就是一个储存日志的文件夹。\n&quot;,
    &quot;2. `MODEL_DIR`: 在模型训练的时候，会把每一阶段的模型都保存在文件夹下。&quot;
   ],
   &quot;metadata&quot;: {
    &quot;collapsed&quot;: false
   }
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: null,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [],
   &quot;source&quot;: [
    &quot;\n&quot;,
    &quot;\n&quot;,
    &quot;LOGGING_DIR = \&quot;logging_dir\&quot;\n&quot;,
    &quot;MODEL_DIR = \&quot;model_result\&quot;\n&quot;,
    &quot;shutil.rmtree(LOGGING_DIR, ignore_errors=True)\n&quot;,
    &quot;\n&quot;,
    &quot;training_args = TrainingArguments(\n&quot;,
    &quot;    output_dir=MODEL_DIR,\n&quot;,
    &quot;    overwrite_output_dir=True,\n&quot;,
    &quot;    logging_dir=LOGGING_DIR,\n&quot;,
    &quot;    learning_rate=2e-5,\n&quot;,
    &quot;    per_device_train_batch_size=32,  # 每一次batch，训练的数据的数量，如果显存高，可以32起步，如果一般，那可能就是个位数，比如2，4，8，16等。\n&quot;,
    &quot;    per_device_eval_batch_size=32,  # 在评估的时候，batch的大小，看显存大小了。\n&quot;,
    &quot;    do_eval=True,\n&quot;,
    &quot;    evaluation_strategy=\&quot;steps\&quot;,\n&quot;,
    &quot;    eval_accumulation_steps=50,\n&quot;,
    &quot;    eval_steps=50,\n&quot;,
    &quot;    logging_steps=50,\n&quot;,
    &quot;    save_steps=100,\n&quot;,
    &quot;    num_train_epochs=4,  #训练多少轮\n&quot;,
    &quot;    weight_decay=0.01,\n&quot;,
    &quot;    save_total_limit=3,  # 模型每`eval_steps`步，就会保存一下模型，只会保存最新的3个模型，\n&quot;,
    &quot;    jit_mode_eval=True,\n&quot;,
    &quot;    fp16=True,\n&quot;,
    &quot;    fp16_opt_level=&#x27;O3&#x27;,\n&quot;,
    &quot;    load_best_model_at_end=True,  # 最后，加载效果最好的模型\n&quot;,
    &quot;\n&quot;,
    &quot;)\n&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: null,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [],
   &quot;source&quot;: [
    &quot;def compute_metrics(eval_pred):\n&quot;,
    &quot;    logits, labels = eval_pred\n&quot;,
    &quot;    predictions = np.argmax(logits, axis=-1)\n&quot;,
    &quot;    return metric.compute(predictions=predictions, references=labels)\n&quot;,
    &quot;\n&quot;,
    &quot;\n&quot;,
    &quot;trainer = Trainer(\n&quot;,
    &quot;    model=model,\n&quot;,
    &quot;    args=training_args,\n&quot;,
    &quot;    train_dataset=tokenized_text[\&quot;train\&quot;],\n&quot;,
    &quot;    eval_dataset=tokenized_text[\&quot;test\&quot;],\n&quot;,
    &quot;    tokenizer=tokenizer,\n&quot;,
    &quot;    data_collator=data_collator,\n&quot;,
    &quot;    compute_metrics=compute_metrics\n&quot;,
    &quot;)\n&quot;,
    &quot;\n&quot;,
    &quot;trainer.train()\n&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: null,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [],
   &quot;source&quot;: []
  }
 ],
 &quot;metadata&quot;: {
  &quot;kernelspec&quot;: {
   &quot;display_name&quot;: &quot;mynet2&quot;,
   &quot;language&quot;: &quot;python&quot;,
   &quot;name&quot;: &quot;python3&quot;
  },
  &quot;language_info&quot;: {
   &quot;codemirror_mode&quot;: {
    &quot;name&quot;: &quot;ipython&quot;,
    &quot;version&quot;: 3
   },
   &quot;file_extension&quot;: &quot;.py&quot;,
   &quot;mimetype&quot;: &quot;text/x-python&quot;,
   &quot;name&quot;: &quot;python&quot;,
   &quot;nbconvert_exporter&quot;: &quot;python&quot;,
   &quot;pygments_lexer&quot;: &quot;ipython3&quot;,
   &quot;version&quot;: &quot;3.10.8&quot;
  },
  &quot;orig_nbformat&quot;: 4,
  &quot;vscode&quot;: {
   &quot;interpreter&quot;: {
    &quot;hash&quot;: &quot;110bc624a448454d574a0cd6cc76359fd86f75739e493913b3d71c2e04f2ffdb&quot;
   }
  }
 },
 &quot;nbformat&quot;: 4,
 &quot;nbformat_minor&quot;: 2
}
```