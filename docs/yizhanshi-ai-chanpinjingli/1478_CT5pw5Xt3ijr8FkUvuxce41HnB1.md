# train_chinese_gpt2.ipynb

> 来源：[https://v11enp9ok1h.feishu.cn/docx/EIVcduz8OoXNfqx2WQHcEtNwnlg](https://v11enp9ok1h.feishu.cn/docx/EIVcduz8OoXNfqx2WQHcEtNwnlg)

    &quot;&lt;img src=\&quot;https://huggingface.co/datasets/huggingface-course/documentation-images/resolve/main/en/chapter7/chunking_texts.svg\&quot;/&gt;\n&quot;,

```
{
 &quot;cells&quot;: [
  {
   &quot;cell_type&quot;: &quot;markdown&quot;,
   &quot;metadata&quot;: {
    &quot;collapsed&quot;: false
   },
   &quot;source&quot;: [
    &quot;# 加载包和处理数据\n&quot;,
    &quot;1. 我们只需要保证数据都含有content这一列（当然，数据是我处理的，肯定是有的）\n&quot;,
    &quot;2. 使用`glob`包，获得所有的数据的路径\n&quot;,
    &quot;3. 我们使用`random`包，从所有的文件路径中，随机找50个数据路径，作为训练集合的使用&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: null,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [],
   &quot;source&quot;: [
    &quot;from datasets import load_dataset, DatasetDict\n&quot;,
    &quot;from glob import glob\n&quot;,
    &quot;import random\n&quot;,
    &quot;random.seed(42)\n&quot;,
    &quot;\n&quot;,
    &quot;all_file_list = glob(pathname=\&quot;gpt2_data/*/**\&quot;)\n&quot;,
    &quot;test_file_list = random.sample(all_file_list, 50)\n&quot;,
    &quot;train_file_list = [i for i in all_file_list if i not in test_file_list]\n&quot;,
    &quot;\n&quot;,
    &quot;len(train_file_list), len(test_file_list)&quot;
   ]
  },
  {
   &quot;attachments&quot;: {},
   &quot;cell_type&quot;: &quot;markdown&quot;,
   &quot;metadata&quot;: {
    &quot;collapsed&quot;: false
   },
   &quot;source&quot;: [
    &quot;# 创建数据\n&quot;,
    &quot;1. 只要将路径放到一个字典里面。dict的key分别为`train`、`valid`，他们对应的值就是文件路径列表即可&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: null,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [],
   &quot;source&quot;: [
    &quot;raw_datasets =load_dataset(\&quot;csv\&quot;,data_files={&#x27;train&#x27;:train_file_list,&#x27;valid&#x27;:test_file_list}, cache_dir=\&quot;cache_data\&quot;)\n&quot;,
    &quot;\n&quot;,
    &quot;raw_datasets&quot;
   ]
  },
  {
   &quot;attachments&quot;: {},
   &quot;cell_type&quot;: &quot;markdown&quot;,
   &quot;metadata&quot;: {
    &quot;collapsed&quot;: false
   },
   &quot;source&quot;: [
    &quot;# Tokenizer\n&quot;,
    &quot;1. Tokenizer 是最关键的一步，因为我们处理的是中文，因此使用`bert_base_chinese`就足够了\n&quot;,
    &quot;2. 如果你的语料里面有别的语言，你也可以使用多语言。这个都无所谓的。只要保证你使用的Tokenizer能覆盖你的数据即可\n&quot;,
    &quot;3. `context_length = 512`设置你的每一个文本的最长长度，我这里设置的是512，如果你的显卡显寸小，那你可以改小一点，比如128。但是多出来的数据，并不是说直接截断不要了，而是按照`context_length`长度，不断的对文本进行截断，大概就像是下面这样的：\n&quot;,
    &quot;\n&quot;,
    &quot;\n&quot;,
    &quot;\n&quot;,
    &quot;4. 对于`gpt2`模型，需要告诉模型一句话从哪里开始，从哪里结束。因此我们需要设置`bos_token`、`eos_token`、`unk_token`\n&quot;,
    &quot;\n&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: null,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [],
   &quot;source&quot;: [
    &quot;from transformers import AutoTokenizer, AutoConfig\n&quot;,
    &quot;\n&quot;,
    &quot;context_length = 512\n&quot;,
    &quot;tokenizer = AutoTokenizer.from_pretrained(\&quot;bert-base-chinese\&quot;)\n&quot;,
    &quot;\n&quot;,
    &quot;outputs = tokenizer(\n&quot;,
    &quot;    raw_datasets[\&quot;train\&quot;][:2][\&quot;content\&quot;],\n&quot;,
    &quot;    truncation=True,\n&quot;,
    &quot;    max_length=context_length,\n&quot;,
    &quot;    return_overflowing_tokens=True,\n&quot;,
    &quot;    return_length=True,\n&quot;,
    &quot;)\n&quot;,
    &quot;\n&quot;,
    &quot;print(f\&quot;Input IDs length: {len(outputs[&#x27;input_ids&#x27;])}\&quot;)\n&quot;,
    &quot;print(f\&quot;Input chunk lengths: {(outputs[&#x27;length&#x27;])}\&quot;)\n&quot;,
    &quot;print(f\&quot;Chunk mapping: {outputs[&#x27;overflow_to_sample_mapping&#x27;]}\&quot;)&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: null,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [],
   &quot;source&quot;: [
    &quot;tokenizer.add_special_tokens(special_tokens_dict={&#x27;bos_token&#x27;: &#x27;&lt;|endoftext|&gt;&#x27;,\n&quot;,
    &quot; &#x27;eos_token&#x27;: &#x27;&lt;|endoftext|&gt;&#x27;,\n&quot;,
    &quot; &#x27;unk_token&#x27;: &#x27;&lt;|endoftext|&gt;&#x27;})&quot;
   ]
  },
  {
   &quot;attachments&quot;: {},
   &quot;cell_type&quot;: &quot;markdown&quot;,
   &quot;metadata&quot;: {
    &quot;collapsed&quot;: false
   },
   &quot;source&quot;: [
    &quot;# 说明\n&quot;,
    &quot;1. 其实，到这里，我做的东西基本上就结束了。\n&quot;,
    &quot;2. 只要查看这个链接，说明的更加清楚：[https://huggingface.co/course/zh-CN/chapter7/6?fw=pt](https://huggingface.co/course/zh-CN/chapter7/6?fw=pt),我也就是模仿这个链接的。&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: null,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [],
   &quot;source&quot;: [
    &quot;def tokenize(element):\n&quot;,
    &quot;    outputs = tokenizer(\n&quot;,
    &quot;        element[\&quot;content\&quot;],\n&quot;,
    &quot;        truncation=True,\n&quot;,
    &quot;        max_length=context_length,\n&quot;,
    &quot;        return_overflowing_tokens=True,\n&quot;,
    &quot;        return_length=True,\n&quot;,
    &quot;    )\n&quot;,
    &quot;    input_batch = []\n&quot;,
    &quot;    for length, input_ids in zip(outputs[\&quot;length\&quot;], outputs[\&quot;input_ids\&quot;]):\n&quot;,
    &quot;        if length == context_length:\n&quot;,
    &quot;            input_batch.append(input_ids)\n&quot;,
    &quot;    return {\&quot;input_ids\&quot;: input_batch}\n&quot;,
    &quot;\n&quot;,
    &quot;\n&quot;,
    &quot;tokenized_datasets = raw_datasets.map(\n&quot;,
    &quot;    tokenize, batched=True, remove_columns=raw_datasets[\&quot;train\&quot;].column_names\n&quot;,
    &quot;)\n&quot;,
    &quot;tokenized_datasets&quot;
   ]
  },
  {
   &quot;attachments&quot;: {},
   &quot;cell_type&quot;: &quot;markdown&quot;,
   &quot;metadata&quot;: {
    &quot;collapsed&quot;: false
   },
   &quot;source&quot;: [
    &quot;# 模型\n&quot;,
    &quot;1. 这里的`gpt2`模型，可不是使用别人训练好的，就是一个gpt2配置，因为我们要使用这个从头训练一个新的`gpt2`&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: null,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [],
   &quot;source&quot;: [
    &quot;from transformers import AutoTokenizer, GPT2LMHeadModel, AutoConfig\n&quot;,
    &quot;\n&quot;,
    &quot;config = AutoConfig.from_pretrained(\n&quot;,
    &quot;    \&quot;gpt2\&quot;,\n&quot;,
    &quot;    vocab_size=len(tokenizer),\n&quot;,
    &quot;    n_ctx=context_length,\n&quot;,
    &quot;    bos_token_id=tokenizer.bos_token_id,\n&quot;,
    &quot;    eos_token_id=tokenizer.eos_token_id,\n&quot;,
    &quot;)&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: null,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [],
   &quot;source&quot;: [
    &quot;model = GPT2LMHeadModel(config)\n&quot;,
    &quot;model_size = sum(t.numel() for t in model.parameters())\n&quot;,
    &quot;print(f\&quot;GPT-2 size: {model_size/1000**2:.1f}M parameters\&quot;)&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: null,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [],
   &quot;source&quot;: [
    &quot;from transformers import DataCollatorForLanguageModeling\n&quot;,
    &quot;\n&quot;,
    &quot;tokenizer.pad_token = tokenizer.eos_token\n&quot;,
    &quot;data_collator = DataCollatorForLanguageModeling(tokenizer, mlm=False)&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: null,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [],
   &quot;source&quot;: [
    &quot;out = data_collator([tokenized_datasets[\&quot;train\&quot;][i] for i in range(5)])\n&quot;,
    &quot;for key in out:\n&quot;,
    &quot;    print(f\&quot;{key} shape: {out[key].shape}\&quot;)&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: null,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [],
   &quot;source&quot;: [
    &quot;from transformers import Trainer, TrainingArguments\n&quot;,
    &quot;\n&quot;,
    &quot;args = TrainingArguments(\n&quot;,
    &quot;    output_dir=\&quot;chinese_gpt2_big\&quot;,\n&quot;,
    &quot;    per_device_train_batch_size=20,\n&quot;,
    &quot;    per_device_eval_batch_size=16,\n&quot;,
    &quot;    evaluation_strategy=\&quot;steps\&quot;,\n&quot;,
    &quot;    eval_steps=2_000,\n&quot;,
    &quot;    logging_steps=2_000,\n&quot;,
    &quot;    gradient_accumulation_steps=8,\n&quot;,
    &quot;    num_train_epochs=2,\n&quot;,
    &quot;    weight_decay=0.1,\n&quot;,
    &quot;    warmup_steps=1_000,\n&quot;,
    &quot;    lr_scheduler_type=\&quot;cosine\&quot;,\n&quot;,
    &quot;    learning_rate=5e-4,\n&quot;,
    &quot;    save_steps=2_000,\n&quot;,
    &quot;    fp16=True,\n&quot;,
    &quot;    push_to_hub=False,\n&quot;,
    &quot;)\n&quot;,
    &quot;\n&quot;,
    &quot;trainer = Trainer(\n&quot;,
    &quot;    model=model,\n&quot;,
    &quot;    tokenizer=tokenizer,\n&quot;,
    &quot;    args=args,\n&quot;,
    &quot;    data_collator=data_collator,\n&quot;,
    &quot;    train_dataset=tokenized_datasets[\&quot;train\&quot;],\n&quot;,
    &quot;    eval_dataset=tokenized_datasets[\&quot;valid\&quot;],\n&quot;,
    &quot;)&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: null,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [],
   &quot;source&quot;: [
    &quot;trainer.train()&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: null,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [],
   &quot;source&quot;: [
    &quot;          &quot;
   ]
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