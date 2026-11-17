
```python
import os
import random
import numpy as np
import torch
from torch.optim import AdamW
from torch.optim.lr_scheduler import StepLR
from torch.utils.data import Dataset, DataLoader
from torch.utils.data.dataloader import default_collate
from torch.nn import CrossEntropyLoss

seed = 42  # 随机种子，影响训练的随机数逻辑，如果随机种子确定，每次训练的结果是一样的
torch.manual_seed(seed)
np.random.seed(seed)
random.seed(seed)

# 确定化cuda、cublas、cudnn的底层随机逻辑
# 否则CUDA会提前优化一些算子，产生不确定性
# 这些处理在训练时也可以不使用
os.environ["CUDA_LAUNCH_BLOCKING"] = "1"
os.environ["CUBLAS_WORKSPACE_CONFIG"] = ":16:8"
torch.use_deterministic_algorithms(True)  # Enable CUDNN deterministic mode
torch.backends.cudnn.deterministic = True
torch.backends.cudnn.benchmark = False

# torch模型都继承于torch.nn.Module
class MyModule(torch.nn.Module):
    def __init__(self, n_classes=2):
        # 优先调用基类构造
        super().__init__()
        # 单个神经元，一个linear加上一个relu激活
        self.linear = torch.nn.Linear(16, n_classes)
        self.relu = torch.nn.ReLU()

    def forward(self, tensor, label):
        # 前向过程
        output = {'logits': self.relu(self.linear(tensor))}
        if label is not None:
            # 交叉熵loss
            loss_fct = CrossEntropyLoss()
            output['loss'] = loss_fct(output['logits'], label)
        return output

# 构造一个数据集
class MyDataset(Dataset):
    # 长度是5
    def __len__(self):
        return 5

    # 如何根据index取得数据集的数据
    def __getitem__(self, index):
        return {'tensor': torch.rand(16), 'label': torch.tensor(1)}

# 构造模型
model = MyModule()
# 构造数据集
dataset = MyDataset()
# 构造dataloader， dataloader会负责从数据集中按照batch_size批量取数，这个batch_size参数就是设置给它的
# collate_fn会负责将batch中单行的数据进行padding
dataloader = DataLoader(dataset, batch_size=4, collate_fn=default_collate)
# optimizer，负责将梯度累加回原来的parameters
# lr就是设置到这里的
optimizer = AdamW(model.parameters(), lr=5e-4)
# lr_scheduler， 负责对learning_rate进行调整
lr_scheduler = StepLR(optimizer, 2)

# 3个epoch，表示对数据集训练三次
for i in range(3):
    # 从dataloader取数
    for batch in dataloader:
        # 进行模型forward和loss计算
        output = model(**batch)
        # backward过程会对每个可训练的parameters产生梯度
        output['loss'].backward()
        # 建议此时看下model中linear的grad值
        # 也就是model.linear.weight.grad
        # 将梯度累加回parameters
        optimizer.step()
        # 清理使用完的grad
        optimizer.zero_grad()
        # 调整lr
        lr_scheduler.step()
```

# Transformer结构

在2017年之后，`Transformer`结构模型几乎横扫一切统治了NLP领域，后面的CV领域和Audio领域也大放异彩。相比`LSTM`和`CNN`结构，`Transformer`结构好在哪里呢？

![](img/0adaf333afd2773881dd998d1b453e2d.png)

LLaMA2模型结构

介绍下基本结构和流程：

1. `Input`是原始句子，经过`Tokenizer`转变为`tokens`；
2. `tokens`输入模型，第一个算子是`Embedder`，`tokens`转换为`float tensor`；
3. 之后进入`layers`，每个`layers`会包含一个`attention`结构，计算`Q`和`K`的`tensor`的内积，并将内积概率化，乘以对应的`V`获得新的`tensor`；
4. `tensor`加上输入的`x`后（防止层数太深梯度消失）进入`Normalization`，对`tensor`分布进行标准化；
5. 进入`FeedForward`（`MLP`），重新进入下一`layer`；
6. 所有的`layers`计算过后，经过一个`linear`求出对`vocab`每个位置的概率。

可以看出，`Transformer`模型的基本原理是让每个文字的`Tensor`和其他文字的`Tensor`做内积（也就是cosine投影值，可以理解为文字的相关程度）。之后把这些相关程度放在一起计算各自占比，再用占比比例分别乘以对应文字的`Tensor`并相加起来，得到了一个新的`Tensor`（这个`Tensor`是之前所有`Tensor`的概率混合，可以理解为对句子所有文字的抽象）。每个文字都进行如上动作，因此生成的新的`Tensor`和之前输入的`Tensor`长度相同（比如输入十个字，计算得到的`Tensor`还是十个），在层数不断堆叠的情况下，最后的`Tensor`会越来越抽象出文字的深层次意义，用最后输出的`Tensor`去计算输出一个新的文字或分类。

## Transformer对比CNN和LSTM

+ `CNN`有局部性和平移不变性，促使模型关注局部信息。`CNN`预设了归纳偏差，这使得小样本训练可以取得较好效果，但在充分数据训练下这一效果也被`Transformer`所掩盖。并且局部性会忽略全局关系，导致某些条件下效果不佳；
+ `LSTM`的长距离记忆会导致最早的`token`被加速遗忘，并且其只能注意单侧信息导致了对句子的理解存在偏差。后来虽然引入了双向`LSTM`，但其大规模分布式训练仍然存在技术问题；
+ `Transformer`结构并不预设归纳偏差，因此需要大数据量训练才有较好效果。但其对于`token`的并行计算大大加速了推理速度，并且对分布式训练支持较好，因此在目前数据量充足的情况下反而异军突起。由于内置了`positional-embedding`，因此较好地解决了`attention`结构中的位置不敏感性。

## Encoder和Decoder

![](img/ef89ab0462fd9e6a2c141a9cf118b27d.png)

如上图所示，左边是`encoder`，右边是`decoder`。我们可以看到目前的`LLM`模型几乎都是`decoder`结构，为什么`encoder-decoder`结构模型消失了呢？有以下几个原因：

+ `encoder-decoder`模型分布式训练困难 `decoder`模型结构简单，其分布式训练相对容易，而`encoder-decoder`结构的模型由于结构复杂的多导致了训练时工程结构复杂，成本大大增加；
+ 有论文证明，`encoder-decoder`模型在参数量不断增加时不具有显著优势。在模型较小时，由于中间隐变量的存在，`decoder`部分进行交叉注意力会获得更好的效果，但随着模型增大，这些提升变得不再明显。甚至有论文猜测，`encoder-decoder`结构的收益仅仅是因为参数量翻倍；

因此，目前的模型都是`decoder`模型，`encoder-decoder`模型几乎销声匿迹。

## 延展阅读

我们可以看到，`LLaMA2`的模型特点是：

1. 没有使用`LayerNorm`，而是使用了`RMSNorm`进行预归一化
2. 使用了`RoPE`（`Rotary Positional Embedding`）
3. `MLP`使用了`SwiGLU`作为激活函数
4. `LLaMA2`的大模型版本使用了`Group Query Attention`（`GQA`）

### RMSNorm

`LayerNorm`的公式是：

y=

`RMSNorm`的开发者发现，减去均值做中心偏移意义不大，因此简化了归一化公式，最终变为：

最终在保持效果不变的情况下，计算时间提升了40%左右。

### RoPE

`BERT`模型使用的原始位置编码是`Sinusoidal Position Encoding`。该位置编码的原理非常简单：

该设计的主要好处在于：

1. 在位置编码累加到`embedding`编码的条件下，基本满足不同位置编码的内积可以模拟相对位置的数值
2. 随着相对位置增大，其位置编码的内积趋近于0
3. 具备一定的外推特性

`LLM`常用的位置编码还有`AliBi`（注意力线性偏置）。该方法不在`embedding`上直接累加位置编码，而选择在`Q*K`的结果上累加一个位置矩阵：

![](img/77ea6a8b5d56a853ad1c6da81edf747a.png)

`ALiBi`的好处在于：

1. 具备良好的外推特性
2. 相对位置数值很稳定

`RoPE`的全称是旋转位置编码(`Rotary Positional Embedding`)，该编码的推导过程和`Sinusoidal Position Encoding`的推导过程比较类似，不同之处在于后者是加性的，而前者是乘性的，因此得到的位置编码类似于：

![](img/0d1c89cb12d3e17b4e76ca136020976c.png)

或者也可以简化为：

![](img/e0efc56c921bfda1503793bfcf03458e.png)

该位置编码表示相对位置的几何意义比较明显，也就是两个向量的角度差。

该位置编码的优势在于：

1. 位置编码矩阵是单位正交阵，因此乘上位置编码后不会改变原向量模长
2. 相较于`Sinusoidal Position Encoding`具备了更好的外推特性

### SwiGLU

`SwiGLU`是`GLU`结构的变种。`GLU`是和`LSTM`原理类似，但不能接受时序数据，只能处理定长数据。而且省略了遗忘门与记忆门，只保留了输入门，`SwiGLU`是将其中的激活函数替换为了`SiLU`：

其中的表达式为：

在`SwiGLU`的论文中，作者论证了`SwiGLU`在`LOSS`收益上显著强于`ReLU`、`GeLU`、`LeakyGeLU`等其他激活方法。

### GQA

`MHA`（`Multi-head Attention`）是标准的多头注意力机制，具有`H`个`Query`、`Key`和`Value`矩阵

`MQA`（`Multi-Query Attention`，来自于论文：`Fast Transformer Decoding: One Write-Head is All You Need`）共享了注意力头之间的`KV`，只为每个头保留单独的`Q`参数，减少了显存占用。

`GQA`（`Grouped-Query Attention`，来自于论文：`GQA: Training Generalized Multi-Query Transformer Models from Multi-Head Checkpoints`）在`MQA`的基础上分成了`G`个组，组内共享`KV`。

在`Llama2`模型中，`70B`参数为了提升推理性能使用了`GQA`，其他版本没有使用这项技术。

### ChatGLM2的模型结构

![](img/44d6b696701d50b9d4e0cf5edbd41bd3.png)

`ChatGLM2`模型结构和`Llama2`的结构有一定相似之处，主要不同之处在于：

1. 在开源的`ChatGLM2`代码中没有使用`GQA`，而是使用了`MQA`
2. `QKV`为单一矩阵，在对`hidden_state`进行整体仿射后拆分为`Query`、`Key`、`Value`
3. `MLP`结构中没有使用`Up`、`Gate`、`Down`三个`Linear`加上`SwiGLU`，而是使用了`hiddensize -> 2 * ffn hiddensize`的`Up Linear`进行上采样，对`tensor`进行拆分为两个宽度为`ffnhiddensize`的`tensor`后直接输入`SiLU`，然后经过`ffnhiddensize -> hiddensize`的`Down Linear`进行下采样

# 数据的预处理

在模型训练过程中，数据及数据处理是最为重要的工作之一。在当前模型训练流程趋于成熟的情况下，数据集的好坏，是决定了该次训练能否成功的最关键因素。

在上一篇中，我们提到了模型训练的基本原理是将文字转换索引再转换为对应的向量，那么文字转为向量的具体过程是什么？

## 分词器（Tokenizer）

在`NLP`（自然语言处理）领域中，承担文字转换索引（`token`）这一过程的组件是`tokenizer`。每个模型有自己特定的`tokenizer`，但它们的处理过程是大同小异的。

首先我们安装好魔搭的模型库`modelscope`和训练框架`SWIFT`：

```
# 激活conda环境后
pip install modelscope ms-swift -U
```

我们使用“千问1.8b”模型将“杭州是个好地方”转为`tokens`的具体方式是在`python`中调用：

```
from modelscope import AutoTokenizer
tokenizer = AutoTokenizer.from_pretrained("qwen/Qwen-1_8B-Chat", trust_remote_code=True)
print(tokenizer('杭州是个好地方'))
# {'input_ids': [104130, 104104, 52801, 100371], 'token_type_ids': [0, 0, 0, 0], 'attention_mask': [1, 1, 1, 1]}
```

其中的`input_ids`就是上面我们说的文字的`token`。可以注意到`token`的个数少于实际文字的数量，这是因为在转为`token`的时候，并不是一个汉字一个`token`的，可能会将部分词语变为一个`token`，也可能将一个英文转为两部分（如词根和时态），所以`token`数量和文字数量不一定对得上。

## 模板（Template）

每种模型有其特定的输入格式，在小模型时代，这种输入格式比较简单：

```
[CLS]杭州是个好地方[SEP]
```

`[CLS]`代表了句子的起始，`[SEP]`代表了句子的终止。在`BERT`中，`[CLS]`的索引是`101`，`[SEP]`的索引是`102`，加上中间的句子部分，在`BERT`模型中整个的`token`序列是：

```
101, 100, 1836, 100, 100, 100, 1802, 1863, 102
```

我们可以看到，这个序列和上面千问的序列是不同的，这是因为这两个模型的词表不同。

在`LLM`时代，`base`模型的格式和上述的差不多，但`chat`模型的格式要复杂的多，比如千问`chat`模型的`template`格式是：

```
<|im_start|>systemYou are a helpful assistant!<|im_end|><|im_start|>userHow are you?<|im_end|><|im_start|>assistant
```

其中“You are a helpful assistant!”是`system`字段，“How are you?”是用户问题，其他的部分都是`template`的格式。

`system`字段是`chat`模型必要的字段，这个字段会以命令方式提示模型在下面的对话中遵循怎么样的范式进行回答，比如：

```
“You are a helpful assistant!”“下面你是一个警察，请按照警察的要求来审问我”“假如你是一个爱哭的女朋友，下面的对话中清扮演好这个角色”
```

`system`字段规定了模型行为准则，比如当模型作为`Agent`使用时，工具集一般也是定义在`system`中的：

```
“你是一个流程的执行者，你有如下工具可以使用：工具1：xxx，输入格式是：xxx，输出格式是：xxx，作用是：xxx工具2：xxx，输入格式是：xxx，输出格式是：xxx，作用是：xxx”
```

复杂的`template`有助于模型识别哪部分是用户输入，哪部分是自己之前的回答，哪部分是给自己的要求。

比较麻烦的是，目前各开源模型还没有一个统一的`template`标准。在`SWIFT`中，我们提供了绝大多数模型的`template`，可以直接使用：
