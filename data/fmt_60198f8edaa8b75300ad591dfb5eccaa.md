
# 基于 ChatGLM-6B 与 LoRA 的模型微调

这里项目是来着github上提供好的LORA微调方法：基于清华的 `ChatGLM-6B` + `LoRA` 进行`finetune`。

> `git clone https://github.com/mymusise/ChatGLM-Tuning.git`

![](img/2b16259fa600b1ce325fbc7ea2b1c95a.png)

## 环境准备

直接用`git`将项目`clone`到自己本地或者云服务器上，环境要求也是根据项目指定的`requirements.txt`文件安装配置环境，这里需要注意的是对GPU的`CUDA`版本要求在`11.6`以上，注意`NVIDIA`驱动和`cuDNN`版本对应安装，之前因为驱动原因踩过不少坑。

## 模型推理

`https://github.com/mymusise/ChatGLM-Tuning/blob/master/infer.ipynb`

新增推理代码`inference.py`：

```
from transformers import AutoModel
import torch

model = AutoModel.from_pretrained("THUDM/chatglm-6b", trust_remote_code=True, load_in_8bit=True, device_map='auto')

from transformers import AutoTokenizer

tokenizer = AutoTokenizer.from_pretrained("THUDM/chatglm-6b", trust_remote_code=True)

from peft import PeftModel

model = PeftModel.from_pretrained(model, "./output/")

import json

instructions = json.load(open("data/alpaca_data.json"))

answers = []
from cover_alpaca2jsonl import format_example

with torch.no_grad():
    for idx, item in enumerate(instructions[:3]):
        feature = format_example(item)
        input_text = feature['context']
        ids = tokenizer.encode(input_text)
        input_ids = torch.LongTensor([ids])
        out = model.generate(
            input_ids=input_ids,
            max_length=150,
            do_sample=False,
            temperature=0
        )
        out_text = tokenizer.decode(out[0])
        answer = out_text.replace(input_text, "").replace("\nEND", "").strip()
        item['infer_answer'] = answer
        print(out_text)
        print(f"### {idx+1}.Answer:\n", item.get('output'), '\n\n')
        answers.append({'index': idx, **item})
```

### 代码解析

这段代码主要是使用预训练的模型来生成一些文本，具体来说，它是从一个`json`文件中读取一些指令，并使用模型来生成相应的答案。下面是对每一行代码的解释：

`from transformers import AutoModel`：从`transformers`库中导入`AutoModel`类。`transformers`是一个深度学习模型库，主要用于自然语言处理（NLP）任务，如文本分类、命名实体识别、问答等。

`import torch`：导入`PyTorch`库，这是一个用于构建和训练深度学习模型的库。

`model = AutoModel.from_pretrained("THUDM/chatglm-6b", trust_remote_code=True, load_in_8bit=True, device_map='auto')`：从预训练模型`"THUDM/chatglm-6b"`加载模型。参数`trust_remote_code=True`表示信任远程代码，`load_in_8bit=True`表示以8位加载模型，`device_map='auto'`表示自动选择设备。

`from transformers import AutoTokenizer`：从`transformers`库中导入`AutoTokenizer`类。这个类用于处理文本数据，包括分词、编码等。

`tokenizer = AutoTokenizer.from_pretrained("THUDM/chatglm-6b", trust_remote_code=True)`：从预训练模型`"THUDM/chatglm-6b"`加载分词器。

`from peft import PeftModel`：从`peft`库中导入`PeftModel`类。这个类可能是一个特定的模型类，但在我知识库的截止日期（2021年9月）之前，没有这个库的信息。

`model = PeftModel.from_pretrained(model, "./output/")`：从预训练模型加载`PeftModel`模型，模型参数保存在`"./output/"`目录下。

`import json`：导入`json`库，用于处理`json`格式的数据。

`instructions = json.load(open("data/alpaca_data.json"))`：从`"data/alpaca_data.json"`文件中加载`json`数据，并赋值给`instructions`。

`answers = []`：初始化一个空列表`answers`，用于存储生成的答案。

`from cover_alpaca2jsonl import format_example`：从`cover_alpaca2jsonl`库中导入`format_example`函数。这个函数可能是用于格式化数据的。

`with torch.no_grad():`：在这个上下文管理器中，`PyTorch`不会计算和存储梯度，这可以提高模型推理速度，减少内存使用。

`for idx, item in enumerate(instructions[:3]):`：遍历`instructions`的前三个元素，`idx`是元素的索引，`item`是元素本身。

`feature = format_example(item)`：使用`format_example`函数处理`item`，返回的结果赋值给`feature`。

`input_text = feature['context']`：从`feature`中获取`'context'`字段的值，赋值给`input_text`。

`ids = tokenizer.encode(input_text)`：使用`tokenizer`对`input_text`进行编码，返回的结果赋值给`ids`。

`input_ids = torch.LongTensor([ids])`：将`ids`转换为`PyTorch`的`LongTensor`类型，并赋值给`input_ids`。

`out = model.generate(input_ids=input_ids, max_length=150, do_sample=False, temperature=0)`：使用模型对`input_ids`进行生成，生成的最大长度为`150`，不进行采样，温度为`0`。返回的结果赋值给`out`。

`out_text = tokenizer.decode(out[0])`：对`out`的第一个元素进行解码，返回的结果赋值给`out_text`。

`answer = out_text.replace(input_text, "").replace("\nEND", "").strip()`：将`out_text`中的`input_text`和`"\nEND"`替换为空，然后去除两端的空白字符，返回的结果赋值给`answer`。

`item['infer_answer'] = answer`：将`answer`赋值给`item`的`'infer_answer'`字段。

`print(out_text)`：打印`out_text`。

`print(f"### {idx+1}.Answer:\n", item.get('output'), '\n\n')`：打印`idx+1`和`item`的`'output'`字段的值。

`answers.append({'index': idx, **item})`：将一个包含`'index'`字段和`item`所有字段的字典添加到`answers`列表中。

## 运行命令

```
CUDA_VISIBLE_DEVICES=0 python inference.py
```

## 运行结果

```
> CUDA_VISIBLE_DEVICES=0 python inference.py

===================================BUG REPORT===================================
Welcome to bitsandbytes. For bug reports, please submit your error trace to: https://github.com/TimDettmers/bitsandbytes/issues
================================================================================
/home/guodong.li/virtual-venv/chatglm-lora-venv-py310-cu117/lib/python3.10/site-packages/bitsandbytes/cuda_setup/main.py:136: UserWarning: WARNING: The following directories listed in your path were found to be non-existent: {PosixPath('/opt/rh/devtoolset-7/root/usr/lib/dyninst'), PosixPath('/opt/rh/devtoolset-9/root/usr/lib/dyninst')}
  warn(msg)
CUDA SETUP: CUDA runtime path found: /usr/local/cuda-11.7/lib64/libcudart.so
CUDA SETUP: Highest compute capability among GPUs detected: 8.0
CUDA SETUP: Detected CUDA version 117
CUDA SETUP: Loading binary /home/guodong.li/virtual-venv/chatglm-lora-venv-py310-cu117/lib/python3.10/site-packages/bitsandbytes/libbitsandbytes_cuda117.so...
Overriding torch_dtype=None with `torch_dtype=torch.float16` due to requirements of `bitsandbytes` to enable model loading in mixed int8. Either pass torch_dtype=torch.float16 or don't pass this argument at all to remove this warning.
Loading checkpoint shards: 100%|██████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 8/8 [00:11<00:00,  1.39s/it]
The dtype of attention mask (torch.int64) is not bool
Instruction: Give three tips for staying healthy.
Answer: Three tips for staying healthy include: 1) eating a balanced diet, 2) getting regular exercise, and 3) getting enough rest.
### 1.Answer:
 1.Eat a balanced diet and make sure to include plenty of fruits and vegetables.
2. Exercise regularly to keep your body active and strong.
3. Get enough sleep and maintain a consistent sleep schedule.


Instruction: What are the three primary colors?
Answer: The three primary colors are red, blue, and yellow.
### 2.Answer:
 The three primary colors are red, blue, and yellow.



Instruction: Describe the structure of an atom.
Answer: An atom is a small particle of matter that contains a core of positive charge, surrounded by a cloud of negative charge. The positive charge is caused by the presence of an electron cloud, which is surrounded by an electron cloud. The negative charge is caused by the presence of an electron cloud, which is surrounded by an electron cloud. The positive and negative charges are balanced by the presence of an equal number of protons and neutrons.
### 3.Answer:
 An atom is made up of a nucleus, which contains protons and neutrons, surrounded by electrons that travel in orbits around the nucleus. The protons and neutrons have a positive charge, while the electrons have a negative charge, resulting in an overall neutral atom. The number of each particle determines the atomic number and the type of atom.
```

其中：`Answer:` 是模型的输出，`#### Answer:` 是原答案。
