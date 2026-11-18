
```python
input_ids = instruction["input_ids"] + response["input_ids"] + [tokenizer.pad_token_id]

attention_mask = instruction["attention_mask"] + response["attention_mask"] + [1]  # 因为eos token咱们也是要关注的所以 补充为1

labels = [-100] * len(instruction["input_ids"]) + response["input_ids"] + [tokenizer.pad_token_id]

if len(input_ids) > MAX_LENGTH:  # 做一个截断

    input_ids = input_ids[:MAX_LENGTH]

    attention_mask = attention_mask[:MAX_LENGTH]

    labels = labels[:MAX_LENGTH]

return {

    "input_ids": input_ids,

    "attention_mask": attention_mask,

    "labels": labels

}
```

调用`process_func`函数处理数据，并进行`decode`数据检查。

```
tokenized_id = ds.map(process_func, remove_columns=ds.column_names)

tokenized_id

tokenizer.decode(tokenized_id[0]['input_ids'])

tokenizer.decode(list(filter(lambda x: x != -100, tokenized_id[1]["labels"])))
```

### 打印模型结构

```
import torch

model = AutoModelForCausalLM.from_pretrained('./qwen/Qwen2-7B-Instruct', device_map="auto", torch_dtype=torch.bfloat16)

model
```

### 定义`LoraConfig`

`LoraConfig`这个类中可以设置很多参数，但主要的参数没多少，简单讲一讲，感兴趣的同学可以直接看源码。

+ `task_type`：模型类型

+ `target_modules`：需要训练的模型层的名字，主要就是`attention`部分的层，不同的模型对应的层的名字不同，可以传入数组，也可以字符串，也可以正则表达式。

+ `r`：lora的秩，具体可以看`Lora`原理

+ `lora_alpha`：`Lora` alaph，具体作用参见 `Lora` 原理

`Lora`的缩放是啥嘞？当然不是`r`（秩），这个缩放就是`lora_alpha / r`，在这个`LoraConfig`中缩放就是4倍。

```
from peft import LoraConfig, TaskType, get_peft_model

config = LoraConfig(

    task_type=TaskType.CAUSAL_LM, 

    target_modules=["q_proj", "k_proj", "v_proj", "o_proj", "gate_proj", "up_proj", "down_proj"],

    inference_mode=False, # 训练模式

    r=8, # Lora 秩

    lora_alpha=32, # Lora alaph，具体作用参见 Lora 原理

    lora_dropout=0.1# Dropout 比例

)

config
```

### 创建`PeftModel`

使用 `get_peft_model()` 函数创建一个 `PeftModel`。

它需要一个基本模型（您可以从 `Transformers` 库加载）和 `LoraConfig`，其中包含如何配置模型以使用 LoRA 进行训练的参数。

```
model = get_peft_model(model, config)

config
```

```
model.print_trainable_parameters()
```

### 自定义 `TrainingArguments` 参数

`TrainingArguments`这个类的源码也介绍了每个参数的具体作用，当然大家可以来自行探索，这里就简单说几个常用的。

+ `output_dir`：模型的输出路径

+ `per_device_train_batch_size`：顾名思义 `batch_size`

+ `gradient_accumulation_steps`: 梯度累加，如果你的显存比较小，那可以把 `batch_size` 设置小一点，梯度累加增大一些。

+ `logging_steps`：多少步，输出一次log

+ `num_train_epochs`：顾名思义 `epoch`

+ `gradient_checkpointing`：梯度检查，这个一旦开启，模型就必须执行`model.enable_input_require_grads()`，这个原理大家可以自行探索

```
args = TrainingArguments(

    output_dir="./output/Qwen2_instruct_lora",

    per_device_train_batch_size=1,

    gradient_accumulation_steps=4,

    logging_steps=10,

    num_train_epochs=1,

    save_steps=100, 

    learning_rate=1e-4,

    save_on_each_node=True,

    gradient_checkpointing=True

)
```

### 模型训练

预计10min就能跑完。

```
trainer = Trainer(

    model=model,

    args=args,

    train_dataset=tokenized_id,

    data_collator=DataCollatorForSeq2Seq(tokenizer=tokenizer, padding=True),

)
```

```
torch.backends.cuda.enable_mem_efficient_sdp(False)
```

```
trainer.train()
```

## 2.4 微调模型测试

通过模型调用生成测试结果，大家可以修改`lora_path` 看看不同的答案。

```
from transformers import AutoModelForCausalLM, AutoTokenizer

import torch

from peft import PeftModel

mode_path = './qwen/Qwen2-7B-Instruct/'

lora_path = './output/Qwen2_instruct_lora_an/checkpoint-100' # 这里改称你的 lora 输出对应 checkpoint 地址

# 加载tokenizer

tokenizer = AutoTokenizer.from_pretrained(mode_path, trust_remote_code=True)

# 加载模型

model = AutoModelForCausalLM.from_pretrained(mode_path, device_map="auto",torch_dtype=torch.float16, trust_remote_code=True).eval()

# 加载lora权重

model = PeftModel.from_pretrained(model, model_id=lora_path)

prompt = '''你是一个逻辑推理专家，擅长解决逻辑推理问题。以下是一个逻辑推理的题目，形式为单项选择题。所有的问题都是（close-world assumption）闭世界假设，即未观测事实都为假。请逐步分析问题并在最后一行输出答案，最后一行的格式为"答案是：A"。题目如下：\n\n### 题目:\n假设您需要构建一个二叉搜索树，其中每个节点或者是一个空的节点（称为"空节点"），或者是一个包含一个整数值和两个子树的节点（称为"数值节点"）。以下是构建这棵树的规则：\n\n1. 树中不存在重复的元素。\n2. 对于每个数值节点，其左子树的所有值都小于该节点的值，其右子树的所有值都大于该节点的值。\n3. 插入一个新值到一个"空节点"时，该"空节点"会被一个包含新值的新的数值节点取代。\n4. 插入一个已存在的数值将不会改变树。\n\n请基于以上规则，回答以下选择题：\n\n### 问题:\n选择题 1：\n给定一个空的二叉搜索树，插入下列数字: [5, 9, 2, 10, 11, 3]，下面哪个选项正确描述了结果树的结构？\nA. tree(5, tree(2, tree(3, nil, nil), nil), tree(9, tree(10, nil, nil), tree(11, nil, nil)))\nB. tree(5, tree(2, nil, tree(3, nil, nil)), tree(9, nil, tree(10, nil, tree(11, nil, nil))))\nC. tree(5, tree(3, tree(2, nil, nil), nil), tree(9, nil, tree(10, tree(11, nil, nil), nil)))\nD. tree(5, nil, tree(2, nil, tree(3, nil, nil)), tree(9, tree(11, nil, nil), tree(10, nil, nil)))'''

inputs = tokenizer.apply_chat_template([{"role": "user", "content": "你是一个逻辑推理专家，擅长解决逻辑推理问题。"},{"role": "user", "content": prompt}],

                                       add_generation_prompt=True,

                                       tokenize=True,

                                       return_tensors="pt",

                                       return_dict=True

                                       ).to('cuda')

gen_kwargs = {"max_length": 2500, "do_sample": True, "top_k": 1}

with torch.no_grad():

    outputs = model.generate(**inputs, **gen_kwargs)

    outputs = outputs[:, inputs['input_ids'].shape[1]:]

    print(tokenizer.decode(outputs[0], skip_special_tokens=True))
```

## 2.5 模型合并存储

这里将`lora`微调后的模型融入到原模型中。

```
# 模型合并存储

new_model_directory = "./merged_model_an"

merged_model = model.merge_and_unload()

# 将权重保存为safetensors格式的权重, 且每个权重文件最大不超过2GB(2048MB)

merged_model.save_pretrained(new_model_directory, max_shard_size="2048MB", safe_serialization=True)
```

```
!cp ./qwen/Qwen2-7B-Instruct/tokenizer.json ./merged_model_an/
```

切记这里需要重启notebook释放显存，否则影响`vllm`启动。

# vllm加速

## 3.1 vllm介绍

vLLM（Virtual Large Language Model）是一个由伯克利大学LMSYS组织开源的大规模语言模型高速推理框架。它的设计目标是在实时应用场景中大幅提升语言模型服务的吞吐量和内存使用效率。vLLM的特点包括易于使用、与`Hugging Face`等流行工具无缝集成以及高效的性能。

## 3.2 vllm服务启动

接下来请打开`start_vllm.ipynb`，执行后我们通过`vllm`的类`openai`接口成功将微调后的模型部署到8000端口。

![](img/7eb039088264ae9af949519e9ba23981.png)

## 3.3 vllm api调用

我们改写了`baseline`中的`call_qwen_api`代码，该文调用本地类`openai`的`qwen`微调模型接口。

```
def call_qwen_api(MODEL_NAME, query):

    # 这里采用dashscope的api调用模型推理，通过http传输的json封装返回结果

    client = OpenAI(

        base_url="http://localhost:8000/v1",

        api_key="sk-xxx", # 随便填写，只是为了通过接口参数校验

    )

    completion = client.chat.completions.create(

      model=MODEL_NAME,

      messages=[

                # {'role':'system','content':'你是一个解决推理任务的专家，你需要分析出问题中的每个实体以及响应关系。然后根据问题一步步推理出结果。并且给出正确的结论。'},

        {"role": "user", "content": query}

      ]

    )

    return completion.choices[0].message.content
```

# 多路LLM投票

## 4.1 思路介绍

所谓的“多路召回策略”就是指采用不同的策略、特征或者简单模型，分别召回一部分候选集，然后再把这些候选集混合在一起后供后续排序模型使用的策略。

## 4.2 实现原理

### 设计投票函数：

通过三次结果推理，将选择答案最多的结果作为最终结果：

```
def most_frequent_char(char1, char2, char3):

    # 创建一个字典来存储每个字符的出现次数

    frequency = {char1: 0, char2: 0, char3: 0}

    # 增加每个字符的出现次数

    frequency[char1] += 1

    frequency[char2] += 1

    frequency[char3] += 1

    # 找到出现次数最多的字符

    most_frequent = max(frequency, key=frequency.get)

    return most_frequent
```

### 设计多路`LLM`：

改写`process`函数，三次调用`llm`，做出现次数统计，最终返回投票数最多的结果。

```
def process_datas(datas,MODEL_NAME):

    results = []

    # 送入多线程任务

    for data in tqdm(datas, desc="Submitting tasks", total=len(datas)):

        problem = data['problem']

        for id,question in enumerate(data['questions']):

            prompt = get_prompt(problem, 

                                question['question'], 
```
