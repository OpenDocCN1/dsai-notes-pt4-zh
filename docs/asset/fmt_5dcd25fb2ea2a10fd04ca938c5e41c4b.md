
# Task3 baseline02 微调方案

> 来源：[https://datawhaler.feishu.cn/docx/D1egd0M64oOkO8x517fcYWzOndh](https://datawhaler.feishu.cn/docx/D1egd0M64oOkO8x517fcYWzOndh)

> **教程贡献者说：**

> 前面我们介绍了基本的`api`实现思路。为了满足题目要求，并教会大家LoRA微调的方案，在`baseline2`里面我们准备了在魔搭notebook的微调的详细教程帮助大家完成微调任务。此外还会教大家使用`vllm`加速推理，封装成`api`加速推理。我们还用到了一个提分trick，使用微调的模型进行多路投票，得到更好的结果。

## 微调介绍

### 1.1 大语言模型微调

大模型微调（Fine-tuning）是一种技术，通过在预训练的大型语言模型上使用特定数据集进行进一步训练，使模型能够更好地适应特定任务或领域。

其核心原理在于，机器学习模型只能代表其训练数据的逻辑和理解。对于未见过的数据样本，模型可能无法准确识别或理解。对于大型模型而言，它们虽然能够处理广泛的语言信息并进行流畅的对话，但在特定场景下可能无法提供准确的答案。

例如，一个通用的大型语言模型虽然包含丰富的语言信息，但在医药领域的特定问题上可能表现不佳。如果需要一个能够准确回答患者问题的医药应用，就需要为这个通用模型提供大量新的医药数据进行学习和理解。比如，当患者询问“布洛芬能否与感冒药同时服用？”时，为了确保模型能够给出正确的回答，我们需要对基础模型进行微调。

### 1.2 微调模型对于本次比赛有什么意义

1. **上下文理解提升**：微调过程中使用的特定数据集可以帮助模型更好地理解特定任务的上下文，从而在推理时能够考虑到更多的相关信息和细节。

2. **性能优化**：微调可以针对特定任务优化模型的性能，使其在处理该任务时达到更高的准确率和更低的错误率。这对于需要高可靠性和准确性的推理任务尤为重要。

3. **减少数据需求**：对于一些数据稀缺的领域或任务，微调可以在相对较少的数据量下实现较好的性能提升，因为模型已经具备了大量的通用语言知识。

4. **适应性增强**：微调使模型能够更好地适应特定用户或场景的需求，提供更加个性化和定制化的推理服务。

## 微调任务介绍

本次教程参考`self-llm`中`Qwen2-7b`的微调教程，特别鸣谢`self-llm`开发者们的大力支持。如果大家需要学习大语言模型如何封装、微调等工作可以来这个项目学习。

本次微调我们的数据使用训练集文件`round1_train_data.jsonl`，将每个问题中的子问题和答案构建成问答对。例如下方。

```
{
        "instruction": "你是一个逻辑推理专家，擅长解决逻辑推理问题。以下是一个逻辑推理的题目，形式为单项选择题。所有的问题都是（close-world assumption）闭世界假设，即未观测事实都为假。请逐步分析问题并在最后一行输出答案，最后一行的格式为\"答案是：A\"。题目如下：\n\n### 题目:\n假设您需要构建一个二叉搜索树，其中每个节点或者是一个空的节点（称为\"空节点\"），或者是一个包含一个整数值和两个子树的节点（称为\"数值节点\"）。以下是构建这棵树的规则：\n\n1. 树中不存在重复的元素。\n2. 对于每个数值节点，其左子树的所有值都小于该节点的值，其右子树的所有值都大于该节点的值。\n3. 插入一个新值到一个\"空节点\"时，该\"空节点\"会被一个包含新值的新的数值节点取代。\n4. 插入一个已存在的数值将不会改变树。\n\n请基于以上规则，回答以下选择题：\n\n### 问题:\n选择题 1：\n给定一个空的二叉搜索树，插入下列数字: [5, 9, 2, 10, 11, 3]，下面哪个选项正确描述了结果树的结构？\nA. tree(5, tree(2, tree(3, nil, nil), nil), tree(9, tree(10, nil, nil), tree(11, nil, nil)))\nB. tree(5, tree(2, nil, tree(3, nil, nil)), tree(9, nil, tree(10, nil, tree(11, nil, nil))))\nC. tree(5, tree(3, tree(2, nil, nil), nil), tree(9, nil, tree(10, tree(11, nil, nil), nil)))\nD. tree(5, nil, tree(2, nil, tree(3, nil, nil)), tree(9, tree(11, nil, nil), tree(10, nil, nil)))\n",
        "input": "",
        "output": "B"
    },
```

抽取的文件已经准备好啦，大家可以直接使用。

![](img/906caaa54721194a550d448aa5fb6c8e.png)

[an.json](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Z5L6bA2b5oB5UIxjm4KcahrTnPf)

### 2.1 环境配置

这次我们将需要的数据、模型文件、代码文件统一放到了【Datawhale](https://www.modelscope.cn/datasets/Datawhale/DW2024_Complex_reasoning_ability_assessment_qwen2-7b-lora)2024年第二届世界科学智能大赛逻辑推理赛道：复杂推理能力评估baseline2】（点击访问）仓库下。

![](img/0be713ad3c2b036b5546e0dd2495055b.png)

首先进入环境，这次我们选择[PAI-DSW](https://www.modelscope.cn/my/mynotebook/preset)（点击可跳转）的`ubuntu22.04-cuda12.1.0-py310-torch2.1.2-tf2.14.0-1.14.0`环境。

![](img/bc3359670beb12838d3bfe80bd2b0851.png)

### 2.2 数据准备

除了刚才介绍的`an`文件，这里还通过`qwen2-72b`模型在`train`数据集上跑得正确结果并分析的答案，得到了`ana.json`数据文件。

拿出一条给大家看看。

````
{
        "instruction": "你是一个逻辑推理专家，擅长解决逻辑推理问题。以下是一个逻辑推理的题目，形式为单项选择题。所有的问题都是（close-world assumption）闭世界假设，即未观测事实都为假。请逐步分析问题并在最后一行输出答案，最后一行的格式为\"答案是：A\"。题目如下：\n\n### 题目:\n假设您需要构建一个二叉搜索树，其中每个节点或者是一个空的节点（称为\"空节点\"），或者是一个包含一个整数值和两个子树的节点（称为\"数值节点\"）。以下是构建这棵树的规则：\n\n1. 树中不存在重复的元素。\n2. 对于每个数值节点，其左子树的所有值都小于该节点的值，其右子树的所有值都大于该节点的值。\n3. 插入一个新值到一个\"空节点\"时，该\"空节点\"会被一个包含新值的新的数值节点取代。\n4. 插入一个已存在的数值将不会改变树。\n\n请基于以上规则，回答以下选择题：\n\n### 问题:\n选择题 1：\n给定一个空的二叉搜索树，插入下列数字: [5, 9, 2, 10, 11, 3]，下面哪个选项正确描述了结果树的结构？\nA. tree(5, tree(2, tree(3, nil, nil), nil), tree(9, tree(10, nil, nil), tree(11, nil, nil)))\nB. tree(5, tree(2, nil, tree(3, nil, nil)), tree(9, nil, tree(10, nil, tree(11, nil, nil))))\nC. tree(5, tree(3, tree(2, nil, nil), nil), tree(9, nil, tree(10, tree(11, nil, nil), nil)))\nD. tree(5, nil, tree(2, nil, tree(3, nil, nil)), tree(9, tree(11, nil, nil), tree(10, nil, nil)))\n",
        "input": "",
        "output": "根据题目中的规则，我们依次插入数字 [5, 9, 2, 10, 11, 3] 来构建二叉搜索树。\n\n首先插入数字 5，树为：\n```
tree(5, nil, nil)
```

\n\n然后插入数字 9，由于 9 > 5，因此插入到右子树：\n```
tree(5, nil, tree(9, nil, nil))
```

\n\n接下来插入数字 2，由于 2 < 5，因此插入到左子树：\n```
tree(5, tree(2, nil, nil), tree(9, nil, nil))
```

\n\n再插入数字 10，由于 10 > 5 并且 10 > 9，所以插入到 9 的右子树：\n```
tree(5, tree(2, nil, nil), tree(9, nil, tree(10, nil, nil)))
```

\n\n继续插入数字 11，由于 11 > 5 并且 11 > 9 和 11 > 10，所以插入到 10 的右子树：\n```
tree(5, tree(2, nil, nil), tree(9, nil, tree(10, nil, tree(11, nil, nil))))
```

\n\n最后插入数字 3，由于 3 < 5 但是 3 > 2，所以插入到 2 的右子树：\n```
tree(5, tree(2, nil, tree(3, nil, nil)), tree(9, nil, tree(10, nil, tree(11, nil, nil))))
```

\n\n比较这个最终结果与给定的选项，可以看到选项 B 正确地描述了结果树的结构。\n\n答案是：B"
    },
````

![](img/cde00b7052ace5bdfe12ceb0f47b58c8.png)

[ana.json](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/AKvRbKetmozgMbx76dhcQBTYnP9)

接下来我们进入界面。

![](img/e3f047127ab0af49e6e5ff3145fb6df2.png)

请大家选择`terminal`，需要下载一下我们的数据文件。

进入后使用快捷键复制并粘贴下面的命令，按下回车。

```
git clone https://www.modelscope.cn/datasets/Datawhale/DW2024_Complex_reasoning_ability_assessment_qwen2-7b-lora.git
```

![](img/01108ec61ddda28856e6dd8ea07b9cf5.png)

跑完后双击到左边的`DW2024_Complex_reasoning_ability_assessment_qwen2-7b-lora`文件。

`baseline2`**启动顺序**

### 2.3 LoRA微调

#### LoRA介绍

LoRA（Low-Rank Adaptation）微调是一种高效的模型微调技术，特别适用于大型预训练语言模型的适应性调整。LoRA的核心思想是通过引入低秩矩阵来调整模型的权重，从而在不显著增加模型参数数量的情况下，实现对模型的微调。

##### LoRA 的优势

- 可以针对不同的下游任务构建小型 LoRA 模块，从而在共享预训练模型参数基础上有效地切换下游任务。

- LoRA 使用自适应优化器（Adaptive Optimizer），不需要计算梯度或维护大多数参数的优化器状态，训练更有效、硬件门槛更低。

- LoRA 使用简单的线性设计，在部署时将可训练矩阵与冻结权重合并，不存在推理延迟。

- LoRA 与其他方法正交，可以组合。

##### LoRA 的原理

![](img/ec486447e49a0518d7eb4efb8ac8e925.png)

- https://github.com/microsoft/LoRA?tab=readme-ov-file

- https://arxiv.org/pdf/2106.09685

- https://huggingface.co/docs/peft/quicktour

#### 代码介绍

双击`lora.ipynb`文件。

安装需要的代码运行环境。（两分钟左右）

```
!pip install modelscope==1.9.5

!pip install "transformers>=4.39.0"

!pip install streamlit==1.24.0

!pip install sentencepiece==0.1.99

!pip install transformers_stream_generator==0.0.4

!pip install datasets==2.18.0

!pip install peft==0.10.0

!pip install openai==1.17.1

!pip install tqdm==4.64.1

!pip install transformers==4.39.3

!python -m pip install setuptools==69.5.1

!pip install vllm==0.4.0.post1

!pip install nest-asyncio

!pip install accelerate

!pip install tf-keras
```

**下载模型文件，这里我们下载魔搭社区的`Qwen2-7B-Instruct`模型参数。**

```
import torch

from modelscope import snapshot_download, AutoModel, AutoTokenizer

import os

model_dir = snapshot_download('qwen/Qwen2-7B-Instruct', cache_dir='./', revision='master')
```

这一步请重启notebook

**接下来加载需要微调的环境**

```
from datasets import Dataset

import pandas as pd

from transformers import AutoTokenizer, AutoModelForCausalLM, DataCollatorForSeq2Seq, TrainingArguments, Trainer, GenerationConfig
```

**导入文件，这里记得使用`an`文件**

```
# 将JSON文件转换为CSV文件

df = pd.read_json('an.json')

ds = Dataset.from_pandas(df)
```

**加载`tokenizer`**

```
tokenizer = AutoTokenizer.from_pretrained('./qwen/Qwen2-7B-Instruct', use_fast=False, trust_remote_code=True)

tokenizer
```

**数据格式化**

`Lora`训练的数据是需要经过格式化、编码之后再输入给模型进行训练的，如果是熟悉`PyTorch`模型训练流程的同学会知道，我们一般需要将输入文本编码为`input_ids`，将输出文本编码为`labels`，编码之后的结果都是多维的向量。我们首先定义一个预处理函数，这个函数用于对每一个样本，编码其输入、输出文本并返回一个编码后的字典：

```
def process_func(example):

    MAX_LENGTH = 1800    # Llama分词器会将一个中文字切分为多个token，因此需要放开一些最大长度，保证数据的完整性

    input_ids, attention_mask, labels = [], [], []

    instruction = tokenizer(f"<|im_start|>system\n你是一个逻辑推理专家，擅长解决逻辑推理问题。<|im_end|>\n<|im_start|>user\n{example['instruction'] + example['input']}<|im_end|>\n<|im_start|>assistant\n", add_special_tokens=False)  # add_special_tokens 不在开头加 special_tokens

    response = tokenizer(f"{example['output']}", add_special_tokens=False)
```
