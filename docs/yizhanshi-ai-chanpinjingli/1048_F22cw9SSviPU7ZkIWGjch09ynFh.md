# LLaMA3_1-8B-Instruct Lora 微调

> 来源：[https://v11enp9ok1h.feishu.cn/docx/JnbCd8ZvXo5LVFxbblecwsRanle](https://v11enp9ok1h.feishu.cn/docx/JnbCd8ZvXo5LVFxbblecwsRanle)

本节[我们简要介绍如何基于 ](https://zhuanlan.zhihu.com/p/650197598)transformers、peft 等框架，对 LLaMA3_1-8B-Instruct 模型进行 Lora 微调。Lora 是一种高效微调方法，深入了解其原理可参见博客：知乎|深入浅出Lora。

这个教程会在同目录下给大家提供一个 [notebook](./04-LLaMA3_1-8B-Instruct Lora.ipynb) 文件，来让大家更好的学习。

## 环境配置

本文基础环境如下：

```
----------------
ubuntu 22.04
python 3.12
cuda 12.1
pytorch 2.3.0
----------------
```

> 本文默认学习者已安装好以上 Pytorch(cuda) 环境，如未安装请自行安装。

首先 pip 换源加速下载并安装依赖包

```
# 升级pip
python -m pip install --upgrade pip
# 更换 pypi 源加速库的安装
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple

pip install modelscope==1.16.1
pip install transformers==4.43.2
pip install accelerate==0.32.1
pip install peft==0.11.1
pip install datasets==2.20.0
```

> 考虑到部分同学配置环境可能会遇到一些问题，我们在AutoDL平台准备了LLaMA3-1的环境镜像，点击下方链接并直接创建Autodl示例即可。

> **h**ttps://www.codewithgpu.com/i/datawhalechina/self-llm/self-llm-llama3.1

在本节教程里，我们将微调数据集放置在根目录 [/dataset](../../dataset/huanhuan.json)。

## 模型下载

使用 modelscope 中的 snapshot_download 函数下载模型，第一个参数为模型名称，参数 cache_dir 为模型的下载路径。

在当前路径下新建 model_download.py 文件并在其中输入以下内容，粘贴代码后请及时保存文件，如下图所示。并运行 python model_download.py 执行下载，模型大小为 15GB，下载模型大概需要 5 分钟。

```
import torch
from modelscope import snapshot_download, AutoModel, AutoTokenizer
import os
model_dir = snapshot_download(&#x27;LLM-Research/Meta-Llama-3.1-8B-Instruct&#x27;, cache_dir=&#x27;/root/autodl-tmp&#x27;, revision=&#x27;master&#x27;)
```

> 注意：记得修改 cache_dir 为你的模型下载路径哦~

## 指令集构建

LLM 的微调一般指指令微调过程。所谓指令微调，是说我们使用的微调数据形如：

```
{
    &quot;instruction&quot;:&quot;回答以下用户问题，仅输出答案。&quot;,
    &quot;input&quot;:&quot;1+1等于几?&quot;,
    &quot;output&quot;:&quot;2&quot;
}
```

其中，instruction 是用户指令，告知模型其需要完成的任务；input 是用户输入，是完成用户指令所必须的输入内容；output 是模型应该给出的输出。

即我们的核心训练目标是让模型具有理解并遵循用户指令的能力。因此，在指令集构建时，我们应[针对我们的目标](https://github.com/KMnO4-zx/huanhuan-chat)任务，针对性构建任务指令集。例如，在本节我们使用由笔者合作开源的 Chat-甄嬛 项目作为示例，我们的目标是构建一个能够模拟甄嬛对话风格的个性化 LLM，因此我们构造的指令形如：

```
{
    &quot;instruction&quot;: &quot;你是谁？&quot;,
    &quot;input&quot;:&quot;&quot;,
    &quot;output&quot;:&quot;家父是大理寺少卿甄远道。&quot;
}
```

我们所构造的全部指令数据集在根目录下。

## 数据格式化

Lora 训练的数据是需要经过格式化、编码之后再输入给模型进行训练的，如果是熟悉 Pytorch 模型训练流程的同学会知道，我们一般需要将输入文本编码为 input_ids，将输出文本编码为 labels，编码之后的结果都是多维的向量。我们首先定义一个预处理函数，这个函数用于对每一个样本，编码其输入、输出文本并返回一个编码后的字典：

```
def process_func(example):
    MAX_LENGTH = 384    # Llama分词器会将一个中文字切分为多个token，因此需要放开一些最大长度，保证数据的完整性
    input_ids, attention_mask, labels = [], [], []
    instruction = tokenizer(f&quot;&lt;|begin_of_text|&gt;&lt;|start_header_id|&gt;system&lt;|end_header_id|&gt;\n\nCutting Knowledge Date: December 2023\nToday Date: 26 Jul 2024\n\n现在你要扮演皇帝身边的女人--甄嬛&lt;|eot_id|&gt;&lt;|start_header_id|&gt;user&lt;|end_header_id|&gt;\n\n{example[&#x27;instruction&#x27;] + example[&#x27;input&#x27;]}&lt;|eot_id|&gt;&lt;|start_header_id|&gt;assistant&lt;|end_header_id|&gt;\n\n&quot;, add_special_tokens=False)  # add_special_tokens 不在开头加 special_tokens
    response = tokenizer(f&quot;{example[&#x27;output&#x27;]}&lt;|eot_id|&gt;&quot;, add_special_tokens=False)
    input_ids = instruction[&quot;input_ids&quot;] + response[&quot;input_ids&quot;] + [tokenizer.pad_token_id]
    attention_mask = instruction[&quot;attention_mask&quot;] + response[&quot;attention_mask&quot;] + [1]  # 因为eos token咱们也是要关注的所以 补充为1
    labels = [-100] * len(instruction[&quot;input_ids&quot;]) + response[&quot;input_ids&quot;] + [tokenizer.pad_token_id]  
    if len(input_ids) &gt; MAX_LENGTH:  # 做一个截断
        input_ids = input_ids[:MAX_LENGTH]
        attention_mask = attention_mask[:MAX_LENGTH]
        labels = labels[:MAX_LENGTH]
    return {
        &quot;input_ids&quot;: input_ids,
        &quot;attention_mask&quot;: attention_mask,
        &quot;labels&quot;: labels
    }
```

LLaMA3.1 采用的Prompt Template格式如下：

```
&lt;|begin_of_text|&gt;&lt;|start_header_id|&gt;system&lt;|end_header_id|&gt;

现在你要扮演皇帝身边的女人--甄嬛&lt;|eot_id|&gt;&lt;|start_header_id|&gt;user&lt;|end_header_id|&gt;

你好呀&lt;|eot_id|&gt;&lt;|start_header_id|&gt;assistant&lt;|end_header_id|&gt;

你好，我是甄嬛，你有什么事情要问我吗？&lt;|eot_id|&gt;&lt;|start_header_id|&gt;assistant&lt;|end_header_id|&gt;
```

## 加载tokenizer和半精度模型

模型以半精度形式加载，如果你的显卡比较新的话，可以用torch.bfolat形式加载。对于自定义的模型一定要指定trust_remote_code参数为True。

```
tokenizer = AutoTokenizer.from_pretrained(&#x27;/root/autodl-tmp/LLM-Research/Meta-Llama-3.1-8B-Instruct&#x27;, use_fast=False, trust_remote_code=True)

model = AutoModelForCausalLM.from_pretrained(&#x27;/root/autodl-tmp/LLM-Research/Meta-Llama-3.1-8B-Instruct&#x27;, device_map=&quot;auto&quot;,torch_dtype=torch.bfloat16)
```

> 注意：此处要记得修改为自己的模型路径哦~

## 定义LoraConfig

LoraConfig这个类中可以设置很多参数，但主要的参数没多少，简单讲一讲，感兴趣的同学可以直接看源码。

+   task_type：模型类型

+   target_modules：需要训练的模型层的名字，主要就是attention部分的层，不同的模型对应的层的名字不同，可以传入数组，也可以字符串，也可以正则表达式。

+   r：lora的秩，具体可以看Lora原理

+   lora_alpha：Lora alaph，具体作用参见 Lora 原理

Lora的缩放是啥嘞？当然不是r（秩），这个缩放就是lora_alpha/r, 在这个LoraConfig中缩放就是4倍。

```
config = LoraConfig(
    task_type=TaskType.CAUSAL_LM, 
    target_modules=[&quot;q_proj&quot;, &quot;k_proj&quot;, &quot;v_proj&quot;, &quot;o_proj&quot;, &quot;gate_proj&quot;, &quot;up_proj&quot;, &quot;down_proj&quot;],
    inference_mode=False, # 训练模式
    r=8, # Lora 秩
    lora_alpha=32, # Lora alaph，具体作用参见 Lora 原理
    lora_dropout=0.1# Dropout 比例
)
```

## 自定义 TrainingArguments 参数

TrainingArguments这个类的源码也介绍了每个参数的具体作用，当然大家可以来自行探索，这里就简单说几个常用的。

+   output_dir：模型的输出路径

+   per_device_train_batch_size：顾名思义 batch_size

+   gradient_accumulation_steps: 梯度累加，如果你的显存比较小，那可以把 batch_size 设置小一点，梯度累加增大一些。

+   logging_steps：多少步，输出一次log

+   num_train_epochs：顾名思义 epoch

+   gradient_checkpointing：梯度检查，这个一旦开启，模型就必须执行model.enable_input_require_grads()，这个原理大家可以自行探索，这里就不细说了。

```
args = TrainingArguments(
    output_dir=&quot;./output/llama3_1_instruct_lora&quot;,
    per_device_train_batch_size=4,
    gradient_accumulation_steps=4,
    logging_steps=10,
    num_train_epochs=3,
    save_steps=100, 
    learning_rate=1e-4,
    save_on_each_node=True,
    gradient_checkpointing=True
)
```

## 使用 Trainer 训练

```
trainer = Trainer(
    model=model,
    args=args,
    train_dataset=tokenized_id,
    data_collator=DataCollatorForSeq2Seq(tokenizer=tokenizer, padding=True),
)
trainer.train()
```

## 加载 lora 权重推理

训练好了之后可以使用如下方式加载lora权重进行推理：

```
from transformers import AutoModelForCausalLM, AutoTokenizer
import torch
from peft import PeftModel

mode_path = &#x27;/root/autodl-tmp/LLM-Research/Meta-Llama-3.1-8B-Instruct&#x27;
lora_path = &#x27;/root/autodl-tmp/output/llama3_1_instruct_lora/checkpoint-100&#x27; # 这里改称你的 lora 输出对应 checkpoint 地址

# 加载tokenizer
tokenizer = AutoTokenizer.from_pretrained(mode_path, trust_remote_code=True)

# 加载模型
model = AutoModelForCausalLM.from_pretrained(mode_path, device_map=&quot;auto&quot;,torch_dtype=torch.bfloat16, trust_remote_code=True).eval()

# 加载lora权重
model = PeftModel.from_pretrained(model, model_id=lora_path)

prompt = &quot;你是谁？&quot;

messages = [
        {&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;假设你是皇帝身边的女人--甄嬛。&quot;},
        {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: prompt}
]

input_ids = tokenizer.apply_chat_template(messages, tokenize=False, add_generation_prompt=True)
model_inputs = tokenizer([input_ids], return_tensors=&quot;pt&quot;).to(&#x27;cuda&#x27;)
generated_ids = model.generate(model_inputs.input_ids,max_new_tokens=512)
generated_ids = [
    output_ids[len(input_ids):] for input_ids, output_ids in zip(model_inputs.input_ids, generated_ids)
]
response = tokenizer.batch_decode(generated_ids, skip_special_tokens=True)[0]
print(response)
```