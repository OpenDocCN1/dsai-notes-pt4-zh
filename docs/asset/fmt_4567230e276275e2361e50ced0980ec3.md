
# (选学)Task4：利用大模型解决NLP任务

> 来源：[https://datawhaler.feishu.cn/docx/F0lydYMyFov2Zkx9yl2crgPdnBg](https://datawhaler.feishu.cn/docx/F0lydYMyFov2Zkx9yl2crgPdnBg)
>
> **教程贡献者说：**
>
> 前面我们介绍了基于 `Transformer` 模型实现在机器翻译任务上的应用
>
> 今天我们的任务是尝试使用大模型解决机器翻译任务！
>
> 你可以从中学会大模型的调用方式，任务利用大模型解决NLP任务
>
> 本赛题已明确指出训练模型过程中不能使用预训练模型，除非你使用训练集自己训练一个。因此，本部分内容仅作为选修内容，感兴趣的学习者可自行学习！

## 一、简单介绍

要使用大模型来解决此机器翻译任务，要比自己训练一个任务简单许多，主要有两种解决方案：

+   直接调用大模型 API 来解决

+   本地部署大模型进行推理

## 二、本地部署大模型进行推理

> `Qwen2-7B-Instruct` 模型需要显存 16GB +，如果没有这么大可以尝试其他小一点的模型

### 加载模型

```
import os
os.environ['HF_ENDPOINT'] = 'https://hf-mirror.com'

from transformers import AutoModelForCausalLM, AutoTokenizer
device = "cuda" # the device to load the model onto

model = AutoModelForCausalLM.from_pretrained(
    "Qwen/Qwen2-7B-Instruct",       # "/mnt/data/xuhu/qwen/Qwen2-7B-Instruct"
    torch_dtype="auto",
    device_map="auto"
)

tokenizer = AutoTokenizer.from_pretrained("/mnt/data/xuhu/qwen/Qwen2-7B-Instruct")
```

+   第一个参数：`Qwen/Qwen2-7B-Instruct` 是模型名称，如果你是下载到了本地，也可以赋予本地模型所在的地址，这里我们是从hugging face的镜像加载的模型

+   `torch_dtype`：设置模型中全部 `Linear` 层的数据格式，可以使用如下把 3232 位的模型线性层参数转换为 1616 位的模型参数，但是除线性层之外的所有参数仍为 3232 位浮点数

```
model = AutoModelForCausalLM.from_pretrained("Qwen/Qwen2-7B-Instruct", torch_dtype=torch.float16)
```

+   `device_map`：指定模型的不同部分加载到哪些设备上，"auto"是指自动分配，通常会同时使用多张GPU进行模型加载，如果想指定使用某一张如下：

```
device_map = {"": 0}  # 空字符串表示整个模型，0表示第一张GPU
```

### 加载数据

```
import pandas as pd
dc = pd.read_csv('../dataset/en-zh.dic', sep='\t', header=None).set_index(0).to_dict()[1]

from tqdm import tqdm
lines = open('../dataset/test_en.txt').readlines()
```

### 撰写prompts并加入术语词典进行推理

```
result = []
for line in tqdm(lines):
    sp_words = [x for x in line.lower().split() if x in dc.keys()]
    sp_words_meaning = [dc[x] for x in sp_words]

    sp_prompt = '文章字符为：'
    if len(sp_words) > 0:
        for x, y in zip(sp_words, sp_words_meaning): 
            sp_prompt += f'{x} 翻译为 {y}; '
    # 主要任务
    messages = [
        {"role": "system", "content": "将英文翻译为中文，不要有其他输出，直接输出翻译后的文本。保留特殊单词的翻译。"},
    ]

    # 人工的词典的规则
    if len(sp_prompt) > 0:
        messages.append({"role": "user", "content": sp_prompt})
    messages.append({"role": "user", "content": f"待翻译文本（从英文翻译为中文）：{line}"})
    
    text = tokenizer.apply_chat_template(
        messages,
        tokenize=False,
        add_generation_prompt=True
    )
    model_inputs = tokenizer([text], return_tensors="pt").to(device)
    
    generated_ids = model.generate(
        model_inputs.input_ids,
        max_new_tokens=512
    )
    generated_ids = [
        output_ids[len(input_ids):] for input_ids, output_ids in zip(model_inputs.input_ids, generated_ids)
    ]
    
    result_line = tokenizer.batch_decode(generated_ids, skip_special_tokens=True)[0]
    result.append(result_line)
    
with open('../results/qwen_submit.txt', 'w') as f:
    for line in result:
        line = line.strip().replace('\n', '')
        print(line)
        f.write(line + '\n')
```

完成代码如下：

![](img/62fca637e4cb2c73f236bd4c0380afee.png)

[`Qwen.ipynb`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/YBhrb2NyzoRdNEx7MiacQgjfnzb)

关于更多部署微调大模型的知识，可以学习：

## 三、直接调用大模型 API 来解决

这种方法更加简单，而且对于显存没有那么高的要求，你只需要一个普通的 CPU 就能跑！！！

这里我们不再给出实现代码，大家感兴趣可以参考 Datawhale 的 `llm-universe` 项目实现了ChatGPT、百度文心、讯飞星火、智谱AI等大模型 API 的调用，包括调用原生 API、封装为 `LangChain` LLM、封装为 `Fastapi` 等调用方式，prompts的设计和术语词典的使用可以参考第二部分的代码，期待你的精彩表现！！

## 你还可以随时回顾学习内容~

<https://datawhaler.feishu.cn/sync/VLmpdf8hGskXBWbJnefcN8cHnte>

> 本教程主要由  写作贡献，经过多位助教内测和建议修改，期待你的点赞和评论～
