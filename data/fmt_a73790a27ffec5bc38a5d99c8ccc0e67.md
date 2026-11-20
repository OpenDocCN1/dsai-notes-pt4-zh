
# NLP（四）：`seq2seq` 与 attention

> 来源：[https://agijuejin.feishu.cn/docx/Co1fd8f5PodkzhxN5fjcNCANnof](https://agijuejin.feishu.cn/docx/Co1fd8f5PodkzhxN5fjcNCANnof)

> 作者：紫气东来

> 原文： [https://zhuanlan.z](https://zhuanlan.zhihu.com/p/458454244)hihu.com/p/458454...

## 一、`seq2seq` 原理

`Seq2Seq`(`Sequence to Sequence`)，又叫`Encoder-Decoder`模型，就是一种能够根据给定的序列，通过特定的方法生成另一个序列的方法。

`Seq2Seq` 主要由2部分组成：`Encoder` 和 `Decoder` （都由`RNN`组成）。

![](img/f2c8f80a29f283d72b2da5fcd24cbd1d.png)

`Encoder:` 负责将输入序列压缩成指定长度的向量，这个向量就可以看成是这个序列的语义，这个过程称为编码，如上图，获取语义向量最简单的方式就是直接将最后一个输入的隐状态作为语义向量`C`。也可以对最后一个隐含状态做一个变换得到语义向量，还可以将输入序列的所有隐含状态做一个变换得到语义变量。

`Decoder:` 负责根据语义向量生成指定的序列，这个过程也称为解码，如上图，最简单的方式是将`encoder`得到的语义变量作为初始状态输入到`decoder`的`RNN`中，得到输出序列。可以看到上一时刻的输出会作为当前时刻的输入，而且其中语义向量`C`只作为初始状态参与运算，后面的运算都与语义向量`C`无关。

`decoder`处理方式还有另外一种，就是语义向量`C`参与了序列所有时刻的运算，如下图，上一时刻的输出仍然作为当前时刻的输入，但语义向量`C`会参与所有时刻的运算。

![](img/1c2e66baf01af1974aefe330e6a30d2a.png)

`seq2seq`的用途很广，可以做机器翻译：

![](img/8e1bca622aaa1fcb74db343e66e88d7a.png)

也可以做问答系统：

![](img/6988cb1a473780eab177a38008f0d156.png)

## `seq2seq` 的训练

`RNN`是可以学习概率分布，然后进行预测，比如我们输入`t`时刻的数据后，预测`t+1`时刻的数据，比较常见的是字符预测例子或者时间序列预测。为了得到概率分布，一般会在`RNN`的输出层使用`softmax`激活函数，就可以得到每个分类的概率。（在对话生成任务中，即得到词典中每个单词出现的概率）

`Softmax` 在机器学习和深度学习中有着非常广泛的应用。尤其在处理多分类（`C`分类数 > 2）问题，分类器最后的输出单元需要`Softmax` 函数进行数值处理

\begin{equation} S_{i}=\frac{e^{v_ i}}{\Sigma_{i}^{C} e^{v_ i}} \end{equation}

其中`v_i` 是分类器前级输出单元的输出。`i` 表示类别索引，总的类别个数为`C`，表示的是当前元素的指数与所有元素指数和的比值。`Softmax` 将多分类的输出数值转化为相对概率，更容易理解和比较。

![](img/18e740089e0019f26f252f2abc8a44ec.png)

对于`RNN`，对于某个序列，对于时刻`t`，它的词向量输出概率为 \begin{equation} P\left(x_{t} \mid x_{1}, x_{2}, \ldots, x_{t-1}\right) \end{equation} ，则`softmax`层每个神经元的计算如下：

\begin{equation} P\left(x_{t}, t \mid x_{1}, \ldots, x_{t-1}\right)=\frac{\exp \left(w_{t} h_{t}\right)}{\sum_{i=1}^{K} \exp \left(w_{i} h_{t}\right)} \end{equation}

其中`h_t` 是当前第`t`个位置的隐含状态，它与上一时刻的状态及当前输入有关，即 \begin{equation} h_{t}=f\left(h_{t-1}, x_{t}\right) \end{equation} ；

`t` 表示文本词典中的第`t`个词对应的下标。

`x_t` 表示词典中第`t`个词；

`w_t` 是词权重参数。

那么整个序列的生成概率就为

\begin{equation} p(x)=\Pi_{t=1}^{T} p\left(x_{t} \mid x_{1}, \ldots, x_{t-1}\right) \end{equation}

其表示从第一个词到第`T`个词一次生成，产生这个词序列的概率。

![](img/0ddb72d066ff14438655fb89f57e491b.png)

而对于`encoder-decoder`模型，设有输入序列 \begin{equation} x_{1}, x_{2}, \ldots, x_{T} \end{equation} ，输出序列 \begin{equation} y_{1}, y_{2}, \ldots, y_{T} \end{equation} ，输入序列和输出序列的长度可能不同。

那么其实就需要根据输入序列去得到输出序列可能输出的词概率，于是有下面的条件概率， \begin{equation} x_{1}, x_{2}, \ldots, x_{T} \end{equation} 发生的情况下， \begin{equation} y_{1}, y_{2}, \ldots, y_{T} \end{equation} 发生的概率等于

\begin{equation} p\left(y_{1}, y_{2}, \ldots, y_{T} \mid x_{1}, x_{2}, \ldots, x_{T}\right)=\Pi_{t=1}^{T} p\left(y_{t} \mid x_{1}, \ldots, x_{t-1}, y_{1}, \ldots, y_{t-1}\right)=\Pi_{t=1}^{T} p\left(y_{t} \mid v, y_{1}, \ldots, y_{t-1}\right) \end{equation}

其中, `v` 表示 \begin{equation} x_{1}, x_{2}, \ldots, x_{T} \end{equation} 对应的隐含状态向量(输入中每个词的词向量)，它其实可以等同表示输入序列（模型依次生成 \begin{equation} y_{1}, y_{2}, \ldots, y_{T} \end{equation} 的概率）。

此时， \begin{equation} h_{t}=f\left(h_{t-1}, y_{t-1}, v\right) \end{equation} ，`decode`编码器中隐含状态与上一时刻状态、上一时刻输出和状态`v`都有关（这里不同于`RNN`，`RNN`是与当前时刻的输入相关，而`decode`编码器是将上一时刻的输出输入到`RNN`中。于是`decoder`的某一时刻的概率分布可用下式表示

\begin{equation} p\left(y_{t} \mid v, y_{1}, y_{2}, \ldots, y_{t-1}\right)=g\left(h_{t}, y_{t-1}, v\right) \end{equation}

所以对于训练样本，我们要做的就是在整个训练样本下，所有样本的 \begin{equation} p\left(y_{1}, y_{2}, \ldots, y_{T} \mid x_{1}, \ldots, x_{T}\right) \end{equation} 概率之和最大。对应的对数似然条件概率函数为

\begin{equation} \frac{1}{N} \sum_{n=1}^{N} \log \left(y_{n} \mid x_{n}, \theta\right) \end{equation}

使之最大化，`θ`则是待确定的模型参数。

## 二、基于`LSTM`内核的`seq2seq`实践

下面通过一个中英文日期转换的例子来说明实现过程，如下所示，即将中文的 年-月-日 转化为英文的 day/month/year ，源代码可参考参考资料[3]。

```
# 中文的 &quot;年-月-日&quot; -&gt; &quot;day/month/year&quot;
&quot;98-02-26&quot; -&gt; &quot;26/Feb/1998&quot;
```

### Step 1. 构建用于训练的数据

其中词表由27个基本单元组成：

```
{'4', '6', 'Sep', 'Oct', '<GO>', 'May', '8', 'Dec', 'Feb', '2', 'Jan', 'Mar', '1', '3', 'Aug', '9', '0', '-', '<PAD>', '5', '7', 'Jul', '<EOS>', 'Nov', '/', 'Apr', 'Jun'}
```

```
from torch import nn
import torch
from torch.utils.data import DataLoader
from torch.nn.functional import cross_entropy
import numpy as np
from torch.utils.data import Dataset as tDataset
import datetime

PAD_ID = 0

class DateData(tDataset):
    def __init__(self, n):
        np.random.seed(1)
        self.date_cn = []
        self.date_en = []
        for timestamp in np.random.randint(143835585, 2043835585, n):
            date = datetime.datetime.fromtimestamp(timestamp)
            self.date_cn.append(date.strftime(&quot;%y-%m-%d&quot;))
            self.date_en.append(date.strftime(&quot;%d/%b/%Y&quot;))
        self.vocab = set(
            [str(i) for i in range(0, 10)] + [&quot;-&quot;, &quot;/&quot;, &quot;&lt;GO&gt;&quot;, &quot;&lt;EOS&gt;&quot;] + [i.split(&quot;/&quot;)[1] for i in self.date_en]
        )
        self.v2i = {v: i for i, v in enumerate(sorted(list(self.vocab)), start=1)}
        self.v2i[&quot;&lt;PAD&gt;&quot;] = PAD_ID
        self.vocab.add(&quot;&lt;PAD&gt;&quot;)
        self.i2v = {i: v for v, i in self.v2i.items()}
        self.x, self.y = [], []
        for cn, en in zip(self.date_cn, self.date_en):
            self.x.append([self.v2i[v] for v in cn])
            self.y.append([self.v2i[&quot;&lt;GO&gt;&quot;], ] + [self.v2i[v] for v in en[:3]] + [
                self.v2i[en[3:6]]] + [self.v2i[v] for v in en[6:]] + [self.v2i[&lt;EOS&gt;], ])
        self.x, self.y = np.array(self.x), np.array(self.y)
        self.start_token = self.v2i[&quot;&lt;GO&gt;&quot;]
        self.end_token = self.v2i[&quot;&lt;EOS&gt;&quot;]

    def __len__(self):
        return len(self.x)

    @property
    def num_word(self):
        return len(self.vocab)

    def __getitem__(self, index):
        return self.x[index], self.y[index], len(self.y[index]) - 1

    def idx2str(self, idx):
        x = []
        for i in idx:
            x.append(self.i2v[i])
            if i == self.end_token:
                break
        return &quot;&quot;.join(x)
```

### Step 2. `seq2seq` 类的实现

其中第一步要搭建压缩器 `encoder` ，有了`encoder`就可以将原始的词向量按顺序组装起来变成句向量。 有了这个句向量之后才能进行 `decode` 工作。

在`seq2seq`中，`decoder`在训练是和句子生成时是不同的。为了方便训练，尤其是在刚开始训练时，`decoder`的输入如果是`True label`，那么就能大大减轻训练难度。不管在训练时有没有预测错，下一步在`decoder`的输入都是正确的。

而在生产环境中预测时，真的在做翻译时，我们就希望有另一种`decode`的`sample`方式。使`decoder`下一步的预测基于`decoder`上一步的预测，而不是`true label`。

所以在`seq2seq`中，为了加快训练速度，我们一般使用的`training`和`inference`的`decode`方式是有所不同的。 `inference`的时候，那没办法，只能拿着上次`decode`的词作为下一步的`input`，我们并没有多少额外的信息，也没有真实`label`可以参考。 但是在`training`时却可以拿着`label`过来加强训练的有效性，达到快速收敛的效果，`training`如果沿着错误的方向一致错下去，是很难纠正的。 所以我们对`training`的预测错误视而不见，每次还是用正确的标签作为`decoder`输入
