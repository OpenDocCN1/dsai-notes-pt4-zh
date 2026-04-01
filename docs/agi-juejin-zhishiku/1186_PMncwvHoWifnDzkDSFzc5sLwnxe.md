# NLP（四）：seq2seq 与 attention

> 来源：[https://agijuejin.feishu.cn/docx/Co1fd8f5PodkzhxN5fjcNCANnof](https://agijuejin.feishu.cn/docx/Co1fd8f5PodkzhxN5fjcNCANnof)

> 作者：紫气东来

> 原文： [https://zhuanlan.z](https://zhuanlan.zhihu.com/p/458454244)hihu.com/p/458454...

## 一、seq2seq 原理

Seq2Seq(Sequence to Sequence)，又叫Encoder-Decoder模型，就是 一种能够根据给定的序列，通过特定的方法生成另一个序列的方法 。

Seq2Seq 主要由2部分组成：Encoder 和 Decoder （都由RNN组成）。

![](img/f2c8f80a29f283d72b2da5fcd24cbd1d.png)

**Encoder: **负责将输入序列压缩成指定长度的向量，这个向量就可以看成是这个序列的语义，这个过程称为编码，如上图，获取语义向量最简单的方式就是直接将最后一个输入的隐状态作为语义向量C。也可以对最后一个隐含状态做一个变换得到语义向量，还可以将输入序列的所有隐含状态做一个变换得到语义变量。

**Decoder: **负责根据语义向量生成指定的序列，这个过程也称为解码，如上图，最简单的方式是将encoder得到的语义变量作为初始状态输入到decoder的RNN中，得到输出序列。可以看到上一时刻的输出会作为当前时刻的输入，而且其中语义向量C只作为初始状态参与运算，后面的运算都与语义向量C无关。

decoder处理方式还有另外一种，就是语义向量C参与了序列所有时刻的运算，如下图，上一时刻的输出仍然作为当前时刻的输入，但语义向量C会参与所有时刻的运算。

![](img/1c2e66baf01af1974aefe330e6a30d2a.png)

seq2seq的用途很广，可以做机器翻译：

![](img/8e1bca622aaa1fcb74db343e66e88d7a.png)

也可以做问答系统：

![](img/6988cb1a473780eab177a38008f0d156.png)

## seq2seq 的训练

RNN是可以学习概率分布，然后进行预测，比如我们输入t时刻的数据后，预测t+1时刻的数据，比较常见的是字符预测例子或者时间序列预测。为了得到概率分布，一般会在RNN的输出层使用softmax激活函数，就可以得到每个分类的概率。(在对话生成任务中，即得到词典中每个单词出现的概率）

Softmax 在机器学习和深度学习中有着非常广泛的应用。尤其在处理多分类（C分类数 > 2）问题，分类器最后的输出单元需要Softmax 函数进行数值处理

\begin{equation} S_{i}=\frac{e^{v_ i}}{\Sigma_{i}^{C} e^{v_ i}} \end{equation}

其中  v_i 是分类器前级输出单元的输出。i 表示类别索引，总的类别个数为C,表示的是当前元素的指数与所有元素指数和的比值。Softmax 将多分类的输出数值转化为相对概率，更容易理解和比较。

![](img/18e740089e0019f26f252f2abc8a44ec.png)

对于RNN，对于某个序列，对于时刻  t ，它的词向量输出概率为  \begin{equation} P\left(x_{t} \mid x_{1}, x_{2}, \ldots, x_{t-1}\right) \end{equation} ，则softmax层每个神经元的计算如下：

\begin{equation} P\left(x_{t}, t \mid x_{1}, \ldots, x_{t-1}\right)=\frac{\exp \left(w_{t} h_{t}\right)}{\sum_{i=1}^{K} \exp \left(w_{i} h_{t}\right)} \end{equation}

其中  h_t 是当前第  t 个位置的隐含状态，它与上一时刻的状态及当前输入有关，即  \begin{equation} h_{t}=f\left(h_{t-1}, x_{t}\right) \end{equation} ；

t 表示文本词典中的第t个词对应的下标。

x_t 表示词典中第t个词；

w_t 是词权重参数。

那么整个序列的生成概率就为

\begin{equation} p(x)=\Pi_{t=1}^{T} p\left(x_{t} \mid x_{1}, \ldots, x_{t-1}\right) \end{equation}

其表示从第一个词到第T个词一次生成，产生这个词序列的概率。

![](img/0ddb72d066ff14438655fb89f57e491b.png)

而对于encoder-decoder模型，设有输入序列  \begin{equation} x_{1}, x_{2}, \ldots, x_{T} \end{equation} ，输出序列  \begin{equation} y_{1}, y_{2}, \ldots, y_{T} \end{equation} ，输入序列和输出序列的长度可能不同。

那么其实就需要根据输入序列去得到输出序列可能输出的词概率，于是有下面的条件概率，  \begin{equation} x_{1}, x_{2}, \ldots, x_{T} \end{equation} 发生的情况下，  \begin{equation} y_{1}, y_{2}, \ldots, y_{T} \end{equation} 发生的概率等于

\begin{equation} p\left(y_{1}, y_{2}, \ldots, y_{T} \mid x_{1}, x_{2}, \ldots, x_{T}\right)=\Pi_{t=1}^{T} p\left(y_{t} \mid x_{1}, \ldots, x_{t-1}, y_{1}, \ldots, y_{t-1}\right)=\Pi_{t=1}^{T} p\left(y_{t} \mid v, y_{1}, \ldots, y_{t-1}\right) \end{equation}

其中,  v 表示  \begin{equation} x_{1}, x_{2}, \ldots, x_{T} \end{equation} 对应的隐含状态向量(输入中每个词的词向量)，它其实可以等同表示输入序列（模型依次生成  \begin{equation} y_{1}, y_{2}, \ldots, y_{T} \end{equation} 的概率）。

此时，  \begin{equation} h_{t}=f\left(h_{t-1}, y_{t-1}, v\right) \end{equation} ，decode编码器中隐含状态与上一时刻状态、上一时刻输出和状态v都有关（这里不同于RNN，RNN是与当前时刻的输入相关，而decode编码器是将上一时刻的输出输入到RNN中。于是decoder的某一时刻的概率分布可用下式表示

\begin{equation} p\left(y_{t} \mid v, y_{1}, y_{2}, \ldots, y_{t-1}\right)=g\left(h_{t}, y_{t-1}, v\right) \end{equation}

所以对于训练样本，我们要做的就是在整个训练样本下，所有样本的  \begin{equation} p\left(y_{1}, y_{2}, \ldots, y_{T} \mid x_{1}, \ldots, x_{T}\right) \end{equation} 概率之和最大。对应的对数似然条件概率函数为

\begin{equation} \frac{1}{N} \sum_{n=1}^{N} \log \left(y_{n} \mid x_{n}, \theta\right) \end{equation}

使之最大化，θ则是待确定的模型参数。

## 二、基于LSTM内核的seq2seq实践

**下面通过一个**中英文日期转换**的例子来说明实现过程，如下所示**，即将中文的 年-月-日 转化为英文的 day/month/year ，源代码可参考参考资料[3]。

```
# 中文的 &quot;年-月-日&quot; -&gt; &quot;day/month/year&quot;
&quot;98-02-26&quot; -&gt; &quot;26/Feb/1998&quot;
```

Step 1. 构建用于训练的数据

其中词表由27个基本单元组成：

{'4', '6', 'Sep', 'Oct', '<GO>', 'May', '8', 'Dec', 'Feb', '2', 'Jan', 'Mar', '1', '3', 'Aug', '9', '0', '-', '<PAD>', '5', '7', 'Jul', '<EOS>', 'Nov', '/', 'Apr', 'Jun'}

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
                self.v2i[en[3:6]]] + [self.v2i[v] for v in en[6:]] + [self.v2i[&quot;&lt;EOS&gt;&quot;], ])
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

Step 2. seq2seq 类的实现

其中第一步要搭建压缩器 encoder ，有了encoder就可以将原始的词向量按顺序组装起来变成句向量。 有了这个句向量之后才能进行 decode 工作。

在seq2seq中，decoder在训练是和句子生成时是不同的。为了方便训练，尤其是在刚开始训练时，decoder的输入如果是True label，那么就能大大减轻训练难度。不管在训练时有没有预测错，下一步在decoder的输入都是正确的。

而在生产环境中预测时，真的在做翻译时，我们就希望有另一种decode的sample方式。使decoder下一步的预测基于decoder上一步的预测，而不是true label。

所以在seq2seq中，为了加快训练速度，我们一般使用的training和inference的decode方式是有所不同的。 inference的时候，那没办法，只能拿着上次decode的词作为下一步的input，我们并没有多少额外的信息，也没有真实label可以参考。 但是在training时却可以拿着label过来加强训练的有效性，达到快速收敛的效果，training如果沿着错误的方向一致错下去，是很难纠正的。 所以我们对training的预测错误视而不见，每次还是用正确的标签作为decoder输入

```
class Seq2Seq(nn.Module):
    def __init__(self, enc_v_dim, dec_v_dim, emb_dim, units, max_pred_len, start_token, end_token):
        super().__init__()
        self.units = units
        self.dec_v_dim = dec_v_dim

        # encoder
        self.enc_embeddings = nn.Embedding(enc_v_dim, emb_dim)
        self.enc_embeddings.weight.data.normal_(0, 0.1)
        self.encoder = nn.LSTM(emb_dim, units, 1, batch_first=True)

        # decoder
        self.dec_embeddings = nn.Embedding(dec_v_dim, emb_dim)
        self.dec_embeddings.weight.data.normal_(0, 0.1)
        self.decoder_cell = nn.LSTMCell(emb_dim, units)
        self.decoder_dense = nn.Linear(units, dec_v_dim)

        self.opt = torch.optim.Adam(self.parameters(), lr=0.001)
        self.max_pred_len = max_pred_len
        self.start_token = start_token
        self.end_token = end_token

    def encode(self, x):
        embedded = self.enc_embeddings(x)  # [n, step, emb]
        hidden = (torch.zeros(1, x.shape[0], self.units), torch.zeros(1, x.shape[0], self.units))
        o, (h, c) = self.encoder(embedded, hidden)
        return h, c

    def inference(self, x):
        self.eval()
        hx, cx = self.encode(x)
        hx, cx = hx[0], cx[0]
        start = torch.ones(x.shape[0], 1)
        start[:, 0] = torch.tensor(self.start_token)
        start = start.type(torch.LongTensor)
        dec_emb_in = self.dec_embeddings(start)
        dec_emb_in = dec_emb_in.permute(1, 0, 2)
        dec_in = dec_emb_in[0]
        output = []
        for i in range(self.max_pred_len):
            hx, cx = self.decoder_cell(dec_in, (hx, cx))
            o = self.decoder_dense(hx)
            o = o.argmax(dim=1).view(-1, 1)
            dec_in = self.dec_embeddings(o).permute(1, 0, 2)[0]
            output.append(o)
        output = torch.stack(output, dim=0)
        self.train()

        return output.permute(1, 0, 2).view(-1, self.max_pred_len)

    def train_logit(self, x, y):
        hx, cx = self.encode(x)
        hx, cx = hx[0], cx[0]
        dec_in = y[:, :-1]
        dec_emb_in = self.dec_embeddings(dec_in)
        dec_emb_in = dec_emb_in.permute(1, 0, 2)
        output = []
        for i in range(dec_emb_in.shape[0]):
            hx, cx = self.decoder_cell(dec_emb_in[i], (hx, cx))
            o = self.decoder_dense(hx)
            output.append(o)
        output = torch.stack(output, dim=0)
        return output.permute(1, 0, 2)

    def step(self, x, y):
        self.opt.zero_grad()
        batch_size = x.shape[0]
        logit = self.train_logit(x, y)
        dec_out = y[:, 1:]
        loss = cross_entropy(logit.reshape(-1, self.dec_v_dim), dec_out.reshape(-1))
        loss.backward()
        self.opt.step()
        return loss.detach().numpy()
```

Step 3. 训练过程

产生训练数据并执行训练过程

```
def train():
    dataset = DateData(4000)
    print(&quot;Chinese time order: yy/mm/dd &quot;, dataset.date_cn[:3], &quot;\nEnglish time order: dd/M/yyyy&quot;, dataset.date_en[:3])
    print(&quot;Vocabularies: &quot;, dataset.vocab)
    print(f&quot;x index sample:  \n{dataset.idx2str(dataset.x[0])}\n{dataset.x[0]}&quot;,
          f&quot;\ny index sample:  \n{dataset.idx2str(dataset.y[0])}\n{dataset.y[0]}&quot;)
    loader = DataLoader(dataset, batch_size=32, shuffle=True)
    model = Seq2Seq(dataset.num_word, dataset.num_word, emb_dim=16, units=32, max_pred_len=11,
                    start_token=dataset.start_token, end_token=dataset.end_token)
    for i in range(100):
        for batch_idx, batch in enumerate(loader):
            bx, by, decoder_len = batch
            loss = model.step(bx, by)
            if batch_idx % 70 == 0:
                target = dataset.idx2str(by[0, 1:-1].data.numpy())
                pred = model.inference(bx[0:1])
                res = dataset.idx2str(pred[0].data.numpy())
                src = dataset.idx2str(bx[0].data.numpy())
                print(
                    &quot;Epoch: &quot;, i,
                    &quot;| t: &quot;, batch_idx,
                    &quot;| loss: %.3f&quot; % loss,
                    &quot;| input: &quot;, src,
                    &quot;| target: &quot;, target,
                    &quot;| inference: &quot;, res,
                )


if __name__ == &quot;__main__&quot;:
    train()
```

通过训练的日志可以看到训练的结果逐步变好

```
Epoch:  0 | t:  0 | loss: 3.278 | input:  13-10-03 | target:  03/Oct/2013 | inference:  MayMayMayMayMayMayMayMayMay
Epoch:  0 | t:  70 | loss: 2.565 | input:  84-05-11 | target:  11/May/1984 | inference:  /////////
Epoch:  1 | t:  0 | loss: 2.258 | input:  80-08-01 | target:  01/Aug/1980 | inference:  1//////&lt;EOS&gt;
Epoch:  1 | t:  70 | loss: 1.771 | input:  81-10-22 | target:  22/Oct/1981 | inference:  12///200&lt;EOS&gt;
Epoch:  2 | t:  0 | loss: 1.502 | input:  76-03-16 | target:  16/Mar/1976 | inference:  11///2000
Epoch:  2 | t:  70 | loss: 1.273 | input:  07-02-17 | target:  17/Feb/2007 | inference:  10///2019
...
Epoch:  97 | t:  0 | loss: 0.000 | input:  32-10-24 | target:  24/Oct/2032 | inference:  24/Oct/2032
Epoch:  97 | t:  70 | loss: 0.000 | input:  90-06-10 | target:  10/Jun/1990 | inference:  10/Jun/1990
Epoch:  98 | t:  0 | loss: 0.000 | input:  78-12-16 | target:  16/Dec/1978 | inference:  16/Dec/1978
Epoch:  98 | t:  70 | loss: 0.000 | input:  81-07-18 | target:  18/Jul/1981 | inference:  18/Jul/1981
Epoch:  99 | t:  0 | loss: 0.000 | input:  88-06-08 | target:  08/Jun/1988 | inference:  08/Jun/1988
Epoch:  99 | t:  70 | loss: 0.000 | input:  29-11-16 | target:  16/Nov/2029 | inference:  16/Nov/2029
```

## 三、seq2seq 的 attention 机制

在上述例子中，Encoder和Decoder之间的唯一联系就是一个固定长度的语义向量C，即Encoder要将整个序列的信息压缩进一个固定长度的向量中去因为语义向量无法完全表示整个序列的信息，而且之前先输入的内容带有的信息会被后输入的信息稀释掉，输入序列越长，这个现象就越严重。这样Decoder时，没有获得输入序列足够的信息， 因此Deocder的准确度自然会降低。因此提出基于Attention的encoder-decoder模型。

![](img/7c9b3ae7fe6d41e340e6c61c66167a51.png)

即把目标句子Y中每个单词的生成过程从

\begin{equation} \begin{aligned} &\mathbf{y}_{1}=\mathbf{f}(\mathbf{C}) \\ &\mathbf{y}_{2}=\mathbf{f}\left(\mathbf{C}, \mathbf{y}_{1}\right) \\ &\mathbf{y}_{3}=\mathbf{f}\left(\mathbf{C}, \mathbf{y}_{1}, \mathbf{y}_{2}\right) \end{aligned} \end{equation}

转变成下面的形式：

\begin{equation} \begin{aligned} &\mathbf{y}_{1}=\mathbf{f 1}\left(\mathbf{C}_{1}\right) \\ &\mathbf{y}_{2}=\mathbf{f 1}\left(\mathbf{C}_{2}, \mathbf{y}_{1}\right) \\ &\mathbf{y}_{3}=\mathbf{f 1}\left(\mathbf{C}_{3}, \mathbf{y}_{1}, \mathbf{y}_{2}\right) \end{aligned} \end{equation}

而每个  C_i 可能对应着不同的源语句子单词的注意力分配概率分布。

拿机器翻译来举例说明上述过程，比如输入的是英文句子：Tom chase Jerry，Encoder-Decoder框架逐步生成中文单词：“汤姆”，“追逐”，“杰瑞”。在翻译“杰瑞”这个中文单词的时候，分心模型里面的每个英文单词对于翻译目标单词“杰瑞”贡献是相同的，很明显这里不太合理，显然“Jerry”对于翻译成“杰瑞”更重要。如果引入AM（attention model）模型的话，应该在翻译“杰瑞”的时候，体现出英文单词对于翻译当前中文单词不同的影响程度，比如给出类似下面一个概率分布值：

（Tom,0.3）(Chase,0.2)(Jerry,0.5)

目标句子中的每个单词都应该学会其对应的源语句子中单词的注意力分配概率信息。这意味着在生成每个单词  y_i 的时候，原先都是相同的中间语义表示C会替换成根据当前生成单词而不断变化的  C_i ，即

\begin{equation} \begin{aligned} &\mathrm{C}_{\text {汤姆 }}=\mathrm{g}(0.6 * \mathrm{f} 2(\text { "Tom" }), 0.2 * \mathrm{f} 2(\text { Chase }), 0.2 * \mathrm{f} 2(\text { "Jerry" })) \\ &\mathrm{C}_{\text {追逐 }}=\mathrm{g}(0.2 * \mathrm{f} 2(\text { "Tom" }), 0.7 * \mathrm{f} 2(\text { Chase }), 0.1 * \mathrm{f} 2(\text { "Jerry" })) \\ &\mathrm{C}_{\text {杰瑞 }}=\mathrm{g}(0.3 * \mathrm{f} 2(\text { "Tom" }), 0.2 * \mathrm{f} 2(\text { Chase }), 0.5 * \mathrm{f} 2(\text { "Jerry" })) \end{aligned} \end{equation}

其中，f2函数代表Encoder对输入英文单词的某种变换函数，比如如果Encoder是用的RNN模型的话，这个f2函数的结果往往是某个时刻输入xi后隐层节点的状态值；g代表Encoder根据单词的中间表示合成整个句子中间语义表示的变换函数，一般的做法中，g函数就是对构成元素加权求和，也就是下列公式：

\begin{equation} c_{i}=\sum_{j=1}^{T_{x}} \alpha_{i j} h_{j} \end{equation}

假设  C_i 中那个i就是上面的“汤姆”，那么  T_x 就是3，代表输入句子的长度，h1=f("Tom")，h2=f("Chase"),h3=f("Jerry")，对应的注意力模型权值分别是0.6,0.2,0.2，所以g函数就是个加权求和函数。

![](img/5f2c415c2566ca8922acbd9d17e376e0.png)

那么问题是这个权重  \alpha_{ij} 究竟是怎么算出来的呢？注意力分配概率分布值的通用计算过程：

![](img/c3dbc5de9316bcd4b6d86c03a31edbfd.png)

对于采用RNN的Decoder来说，如果要生成  y_i 单词，在时刻  i ，我们是可以知道在生成  y_i 之前的隐层节点  i 时刻的输出值  H_i 的，而我们的目的是要计算生成  y_i 时的输入句子单词“Tom”、“Chase”、“Jerry”对  y_i 来说的注意力分配概率分布，那么可以用i时刻的隐层节点状态  H_i 去一一和输入句子中每个单词对应的RNN隐层节点状态  h_j 进行对比，即通过函数  F(h_j,H_i) 来获得目标单词  y_i 和每个输入单词对应的对齐可能性，这个F函数在不同论文里可能会采取不同的方法，然后函数F的输出经过Softmax进行归一化就得到了符合概率分布取值区间的注意力分配概率分布数值。上面显示的是当输出单词为“汤姆”时刻对应的输入句子单词的对齐概率。绝大多数AM模型都是采取上述的计算框架来计算注意力分配概率分布信息，区别只是在F的定义上可能有所不同。

其过程可分解为4步：

1.  对encoder的所有隐藏状态计算score

1.  对分数放到softmax层计算，以得到attention distribution

1.  将每个编码器隐藏状态乘以其softmaxed的分数

1.  对向量求和。对齐向量相加产生上下文向量。 上下文向量是来自上一步的对齐向量的聚合信息。

[🎦 点击观看视频](https://vdn6.vzuu.com/HD/557ced3c-88ab-11ec-a1d3-ca70f8322bc8-v4_t10000011-wciQ2SByMO.mp4?pkey=AAVjszYxLeL9nCVv3zmvJDWfgU6-3tOhx6W1IkMw93ZR28uAYEUuIDWBcbv9lha9TuoPbkQcsPmcORkwjUmUYAzl&bu=1513c7c2&c=avc.0.0&expiration=1724165460&f=mp4&pu=da4bec50&v=ks6&pp=ChMxNDAxNjIzODY1NzM5NTc5MzkyGGMiC2ZlZWRfY2hvaWNlMhMxMzY5MDA1NjA4NTk5OTA0MjU3PXu830Q%3D&pf=Web&pt=zhihu)

![](img/8b36fd8351256a2bcb9d62bb7df975b3.png)

00:09

直观理解的话可以采用日期翻译的例子

![](img/5b8ade6cd11a297b37dc556ef055835b.png)

下面来实践带attention的seq2seq，与不带attention的相比，增加了 self.attn 变量，详情如下：

```
class Seq2Seq(nn.Module):
    def __init__(self, enc_v_dim, dec_v_dim, emb_dim, units, max_pred_len, start_token, end_token):
        super().__init__()
...
        self.attn = nn.Linear(units, units)
...

    def inference(self, x, return_align=False):
        self.eval()
        o, hx, cx = self.encode(x)  # [n, step, units], [num_layers * num_directions, n, units] * 2
        hx, cx = hx[0], cx[0]  # [n, units]
        start = torch.ones(x.shape[0], 1)  # [n, 1]
        start[:, 0] = torch.tensor(self.start_token)
        start = start.type(torch.LongTensor)
        dec_emb_in = self.dec_embeddings(start)  # [n, 1, emb_dim]
        dec_emb_in = dec_emb_in.permute(1, 0, 2)  # [1, n, emb_dim]
        dec_in = dec_emb_in[0]  # [n, emb_dim]
        output = []
        for i in range(self.max_pred_len):
            attn_prod = torch.matmul(self.attn(o), hx.unsqueeze(2))  # [n, step, 1]
            attn_weight = softmax(attn_prod, dim=1)  # [n, step, 1]
            context = torch.matmul(o.permute(0, 2, 1), attn_weight)  # [n, units, 1]
            hx, cx = self.decoder_cell(dec_in, (hx, cx))
            hc = torch.cat([context.squeeze(2), hx], dim=1)  # [n, units *2]
            result = self.decoder_dense(hc)
            result = result.argmax(dim=1).view(-1, 1)
            dec_in = self.dec_embeddings(result).permute(1, 0, 2)[0]
            output.append(result)
        output = torch.stack(output, dim=0)
        self.train()

        return output.permute(1, 0, 2).view(-1, self.max_pred_len)

    def train_logit(self, x, y):
        o, hx, cx = self.encode(x)  # [n, step, units], [num_layers * num_directions, n, units] * 2
        hx, cx = hx[0], cx[0]  # [n, units]
        dec_in = y[:, :-1]  # [n, step]
        dec_emb_in = self.dec_embeddings(dec_in)  # [n, step, emb_dim]
        dec_emb_in = dec_emb_in.permute(1, 0, 2)  # [step, n, emb_dim]
        output = []
        for i in range(dec_emb_in.shape[0]):
            attn_prod = torch.matmul(self.attn(o), hx.unsqueeze(2))  # [n, step, 1]
            attn_weight = softmax(attn_prod, dim=1)  # [n, step, 1]
            context = torch.matmul(o.permute(0, 2, 1), attn_weight)  # [n, units, 1]
            hx, cx = self.decoder_cell(dec_emb_in[i], (hx, cx))  # [n, units]
            hc = torch.cat([context.squeeze(2), hx], dim=1)  # [n, units *2]
            result = self.decoder_dense(hc)  # [n, dec_v_dim]
            output.append(result)
        output = torch.stack(output, dim=0)  # [step, n, dec_v_dim]
        return output.permute(1, 0, 2)  # [n, step, dec_v_dim]
```

## 参考资料

[1] [https:// zhuanlan.zhi](https://zhuanlan.zhihu.com/p/136559171)hu.com/p/13 6559171

[2] [https://www. jianshu.](https://link.zhihu.com/?target=https%3A//www.jianshu.com/p/b2b95f945a98)com/p/b2b95f945 a98

[3] [h](https://link.zhihu.com/?target=https%3A//mofanpy.com/tutorials/machine-learning/nlp/seq2seq/)ttps:// mofanpy.com/tutorials/m achine-learning/nlp/seq2seq/

[4] [h](https://link.zhihu.com/?target=https%3A//blog.csdn.net/lygeneral/article/details/106461591)ttps:// blog.csdn.net/lygeneral /article/details/106461591

[5] [h](https://link.zhihu.com/?target=https%3A//blog.csdn.net/malefactor/article/details/50550211)ttps:// blog.csdn.net/malefacto r/article/details/50550211

> The more you know who you are and what you want, the less you let things upset you.