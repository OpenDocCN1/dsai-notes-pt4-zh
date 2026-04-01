# NLP（三）：向量化技术

> 来源：[https://agijuejin.feishu.cn/docx/BWpSdIblioawyUxOD2Kch9uOnTf](https://agijuejin.feishu.cn/docx/BWpSdIblioawyUxOD2Kch9uOnTf)

> 作者：紫气东来

> 原文：[https://zhuanlan.z](https://zhuanlan.zhihu.com/p/458453654)hihu.com/p/458453...

**词向量 **是在NLP中比较基础的一个工作，相对计算机而言，人要聪明的多，人很容易明白幸福和开心是两个比较近的词，而计算机要想了解，其实是很难的，而在现代计算机中，对语言的理解显得越来越重要，如何去表示一个词，也成为了理解语言的基础。

## 传统方法

**one-hot编码**

One-hot编码可能是最简单的一种编码方法，每个词只在对应的index置1，其他位置都是0，One-hot编码的问题在于很难做相似度计算，在大规模语料上时，One-hot编码的长度可能为几十万、几百万甚至更大，One-hot编码显然不行；

**矩阵分解方法(LSA)**

针对一个词来说，它的语义由其上下文决定。 LSA使用词-文档矩阵，矩阵通常是一个稀疏矩阵，其行代表词语、其列代表文档。词-文档矩阵表示中的值表示词在该文章出现的次数，通常，我们可以简单地通过文档的出现次数分布来表示对应的词，但是由于这个矩阵通常是比较稀疏的，我们可以利用矩阵分解，学习到对应词的低秩表示，这个表示建模了文档中词的共现关系,让相似度的计算变得更加容易。 同理，可以也可以在更小粒度上计算矩阵的构建，如设定指定窗口大小，若在该窗口内出现，则数值加一，构建好词-词共现矩阵，最终使用如svd这类的矩阵分解方法即可。 这类方法明显的弊病在于当copur过大时，计算很消耗资源，且对于未出现词或者新文档不友好。

## 经典的词向量方法

### Word2Vec算法

Word2Vec简单讲就是通过学习文本然后用词向量的方式表征词的语义信息，即通过Embedding把原先词所在空间映射到一个新的空间中去，使得语义上相似的单词在该空间内距离相近。以传统神经网络为基础的神经概率语言模型，缺点主要是计算量太大，集中体现在：隐层和输出层之间的矩阵运算和输出层上的 Softmax归一化运算上。因此Word2Vec就是针对这两点来优化神经概率语言模型的。 中两个重要的模型是： CBOW 模型和 Skip-gram 模型。

**CBOW 模型**

CBOW(continue bag-of-words) 包括三层结构：输入层、投影层和输出层。

![](img/8b0f64a95bbe1b80734a8cb4233af700.png)

**输入层： **包含2c个词的词向量。

**投影层： **对输入的2c个词向量进行累加操作。

**输出层： **输出层对应一颗哈夫曼(Huffman)树，它是以语料中出现过的词当叶子节点，以各词在语料库中出现的次数当权值构造而成。在这颗树中，叶子结点共  N 个，分别对应词典  \mathbb{D} 中的词，非叶结点  N-1 个。

![](img/fd2537726ec13d32867b3e401ecb7883.png)

由 Hierarchical Softmax 的思想，对于词典  \mathbb{D} 中的的任意词  w ，哈夫曼树中必然存在唯一一条从根节点到词  w 对应叶子节点的路径  p^w 。路径  p^w 中包含节点的个数为  l^w ，则路径  p^w 上存在  l^w-1 个分支，将每个分支看作一次二分类，那么每一次分类就对应一个概率，最后这些概率连乘得到  p(w | \text { Context }(w)) ，即

p(w | \text { Context }(w)) =\prod_{j=2}^{l^{w}} p\left(d_{j}^{w} | X_{w} ; \theta_{j-1}^{w}\right)

其中  d_{j}^{w} 表示路径  p^w 中第  j 个节点对应的编码（根节点不对应编码）；  \theta_{j}^{w} 表示路径  p^w 中第  j 个非叶子节点对应的向量；  X_{w} 表示  \text { Context }(w) 中所有词向量的叠加。

\begin{equation} \begin{aligned} p\left(d_{j}^{w} \mid X_{w} ; \theta_{j-1}^{w}\right) &= \begin{cases}\sigma\left(X_{w}^{T} \theta_{j-1}^{w}\right), & \text { if } d_{j}^{w}=0 \\ 1-\sigma\left(X_{w}^{T} \theta_{j-1}^{w}\right), & \text { otherwise }\end{cases} \\ &=\left[\sigma\left(X_{w}^{T} \theta_{j-1}^{w}\right)\right]^{1-d_{j}^{w}} \cdot\left[1-\sigma\left(X_{w}^{T} \theta_{j-1}^{w}\right)\right]^{d_{j}^{w}} \end{aligned} \end{equation}

其中  \sigma(x) = \frac{1}{1+e^{-x}} 。

通过对数极大似然化处理可得CBOW模型的目标函数为：

\begin{aligned} \mathcal{L} &=\sum_{w \in \mathcal{D}} \log \prod_{j=2}^{l^{w}}\left(\left[\sigma\left(X_{w}^{T} \theta_{j-1}^{w}\right)\right]^{1-d_{j}^{w}} \cdot\left[1-\sigma\left(X_{w}^{T} \theta_{j-1}^{w}\right)\right]^{d_{j}^{w}}\right) \\ &=\sum_{w \in \mathcal{D}} \sum_{j=2}^{l^{w}}\left(\left(1-d_{j}^{w}\right) \cdot \log \left[\sigma\left(X_{w}^{T} \theta_{j-1}^{w}\right)\right]+d_{j}^{w} \cdot \log \left[1-\sigma\left(X_{w}^{T} \theta_{j-1}^{w}\right)\right]\right) \\ &=\sum_{w \in \mathcal{D}} \sum_{j=2}^{l^{w}} \Phi\left(\theta_{j-1}^{w}, X_{w}\right) \end{aligned}

word2vec极大化目标函数使用的算法是随机梯度上升法，首先考虑  \Phi\left(\theta_{j-1}^{w}, X_{w}\right) 对  \theta_{j-1}^{w} 的梯度计算：

\begin{aligned} \frac{\partial \Phi\left(\theta_{j-1}^{w}, X_{w}\right)}{\partial \theta_{j-1}^{w}} &=\frac{\partial}{\theta_{j-1}^{w}}\left(\left(1-d_{j}^{w}\right) \cdot \log \left[\sigma\left(X_{w}^{T} \theta_{j-1}^{w}\right)\right]+d_{j}^{w} \cdot \log \left[1-\sigma\left(X_{w}^{T} \theta_{j-1}^{w}\right)\right]\right) \\ &=\left(1-d_{j}^{w}\right)\left[1-\sigma\left(X_{w}^{T} \theta_{j-1}^{w}\right)\right] X_{w}-d_{j}^{w} \sigma\left(X_{w}^{T} \theta_{j-1}^{w}\right) X_{w} \\ &=\left(\left(1-d_{j}^{w}\right)\left[1-\sigma\left(X_{w}^{T} \theta_{j-1}^{w}\right)\right]-d_{j}^{w} \sigma\left(X_{w}^{T} \theta_{j-1}^{w}\right)\right) X_{w} \\ &=\left(1-d_{j}^{w}-\sigma\left(X_{w}^{T} \theta_{j-1}^{w}\right)\right) X_{w} \end{aligned}

则  \theta_{j-1}^{w} 的更新公式为：

\theta_{j-1}^{w}:=\theta_{j-1}^{w}+\eta\left(1-d_{j}^{w}-\sigma\left(X_{w}^{T} \theta_{j-1}^{w}\right)\right) X_{w}

然后考虑  \Phi\left(\theta_{j-1}^{w}, X_{w}\right) 对  X_{w} 的梯度计算：

\begin{aligned} \frac{\partial \Phi\left(\theta_{j-1}^{w}, X_{w}\right)}{\partial X_{w}} &=\frac{\partial}{X_{w}}\left(\left(1-d_{j}^{w}\right) \cdot \log \left[\sigma\left(X_{w}^{T} \theta_{j-1}^{w}\right)\right]+d_{j}^{w} \cdot \log \left[1-\sigma\left(X_{w}^{T} \theta_{j-1}^{w}\right)\right]\right) \\ &=\left(1-d_{j}^{w}\right)\left[1-\sigma\left(X_{w}^{T} \theta_{j-1}^{w}\right)\right] \theta_{j-1}^{w}-d_{j}^{w} \sigma\left(X_{w}^{T} \theta_{j-1}^{w}\right) \theta_{j-1}^{w} \\ &=\left(\left(1-d_{j}^{w}\right)\left[1-\sigma\left(X_{w}^{T} \theta_{j-1}^{w}\right)\right]-d_{j}^{w} \sigma\left(X_{w}^{T} \theta_{j-1}^{w}\right)\right) \theta_{j-1}^{w} \\ &=\left(1-d_{j}^{w}-\sigma\left(X_{w}^{T} \theta_{j-1}^{w}\right)\right) \theta_{j-1}^{w} \end{aligned}

观察到  \Phi\left(\theta_{j-1}^{w}, X_{w}\right) 中  \theta_{j-1}^{w} 和  X_{w} 具有对称性，word2vec直接取下式方式来更新  v(\widetilde{w}) ：

v(\widetilde{w}):=v(\widetilde{w})+\eta \sum_{j=2}^{l^{w}} \frac{\partial \Phi\left(\theta_{j-1}^{w}, X_{w}\right)}{\partial X_{w}}, \widetilde{w} \in \text { Context }(w)

### Skip-gram模型

Skip-gram模型也包括输入层、投影层和输出层。

![](img/8bbf1f29df9f27eddc1755947b7f8ca0.png)

**输入层： **只含有当前样本的中心词  w 的词向量  v(w) \in \mathbb{R} ；

**投影层： **由于为恒等投影，因此该层可有可无；

**输出层： **也为一棵哈夫曼树。

![](img/ea10e5f950f8748a60091e6ad31af281.png)

在Skip-gram模型中已知当前词  w ，需要对其上下文  Context(w) 中的词进行预测，关键是条件概率函数  p(\text {Context}(w) | w) ，即：

p(\text {Context}(w) | w)=\prod_{u \in C \text {ontext}(w)} p(u | w)

同样由Hierarchical Softmax的思想，可得：

\begin{aligned} p(u | w) &=\prod_{j=2}^{l^{w}} p\left(d_{j}^{u} | v(w) ; \theta_{j-1}^{u}\right) \\ &=\prod_{j=2}^{l^{u}}\left[\sigma\left(v(w)^{T} \theta_{j-1}^{u}\right)\right]^{1-d_{j}^{u}} \cdot\left[1-\sigma\left(v(w)^{T} \theta_{j-1}^{u}\right]^{d_{j}^{u}}\right. \end{aligned}

通过极大似然化处理可得Skip-gram模型的目标函数为：

\begin{aligned} {L} &=\sum_{w \in {D}} \log \prod_{u \in C o n t e x t} \prod_{j=2}^{l^{u}}\left(\left[\sigma\left(v(w)^{T} \theta_{j-1}^{u}\right)\right]^{1-d_{j}^{u}} \cdot\left[1-\sigma\left(v(w)^{T} \theta_{j-1}^{u}\right]^{d_{j}^{u}}\right)\right.\\ &=\sum_{w \in {D}} \sum_{u \in C o n t e x t} \sum_{j=2}^{l^{u}}\left(\left(1-d_{j}^{u}\right) \cdot \log \left[\sigma\left(v(w)^{T} \theta_{j-1}^{u}\right)\right]+d_{j}^{u} \cdot \log \left[1-\sigma\left(v(w)^{T} \theta_{j-1}^{u}\right)\right]\right) \\ &=\sum_{w \in {D}} \sum_{u \in \text {Context}(w)} \sum_{j=2}^{l^{u}} {O}\left(\theta_{j-1}^{u}, v(w)\right) \end{aligned}

考虑  {O}\left(\theta_{j-1}^{u}, v(w)\right) 对  \theta_{j-1}^{u} 的梯度：

\begin{aligned} \frac{\partial {O}\left(\theta_{j-1}^{u}, v(w)\right)}{\partial \theta_{j-1}^{u}} &\left.=\frac{\partial}{\partial \theta_{j-1}^{u}}\left(\left(1-d_{j}^{u}\right) \cdot \log \left[\sigma(w)^{T} \theta_{j-1}^{u}\right)\right]+d_{j}^{u} \cdot \log \left[1-\sigma\left(v(w)^{T} \theta_{j-1}^{u}\right)\right]\right) \\ &=\left(1-d_{j-1}^{u}\right)\left[1-\sigma\left(v(w)^{T} \theta_{j-1}^{u}\right)\right] v(w)-d_{j}^{u} \sigma\left(v(w)^{T} \theta_{j-1}^{u}\right) v(w) \\ &=\left(\left(1-d_{j}^{u}\right)\left[1-\sigma\left(v(w)^{T} \theta_{j-1}^{u}\right)\right]-d_{j}^{u} \sigma\left(v(w)^{T} \theta_{j-1}^{u}\right)\right) v(w) \\ &=\left(1-d_{j}^{u}-\sigma\left(v(w)^{T} \theta_{j-1}^{u}\right)\right) v(w) \end{aligned}

则  \theta_{j-1}^{u} 的更新公式为：

\theta_{j-1}^{u}:=\theta_{j-1}^{u}+\eta\left(1-d_{j}^{u}-\sigma\left(v(w)^{T} \theta_{j-1}^{u}\right)\right) v(w)

再考虑  {O}\left(\theta_{j-1}^{u}, v(w)\right) 对  v(w) 的梯度：

\begin{aligned} \frac{\partial {O}\left(\theta_{j-1}^{u}, v(w)\right)}{\partial v(w)} &=\frac{\partial}{\partial v(w)}\left(\left(1-d_{j}^{u}\right) \cdot \log \left[\sigma\left(v(w)^{T} \theta_{j-1}^{u}\right)\right]+d_{j}^{u} \cdot \log \left[1-\sigma\left(v(w)^{T} \theta_{j-1}^{u}\right)\right]\right) \\ &=\left(1-d_{j}^{u}\right)\left[1-\sigma\left(v(w)^{T} \theta_{j-1}^{u}\right)\right] \theta_{j-1}^{u}-d_{j}^{u} \sigma\left(v(w)^{T} \theta_{j-1}^{u}\right) \theta_{j-1}^{u} \\ &=\left(\left(1-d_{j}^{u}\right)\left[1-\sigma\left(v(w)^{T} \theta_{j-1}^{u}\right)\right]-d_{j}^{u} \sigma\left(v(w)^{T} \theta_{j-1}^{u}\right)\right) \theta_{j-1}^{u} \\ &=\left(1-d_{j}^{u}-\sigma\left(v(w)^{T} \theta_{j-1}^{u}\right)\right) \theta_{j-1}^{u} \end{aligned}

则  v(w) 更新公式为：

v(w):=v(w)+\eta \sum_{u \in \text { Context }(w)} \sum_{j=2}^{l^{u}} \frac{\partial {O}\left(\theta_{j-1}^{u}, v(w)\right)}{\partial v(w)}

## 算法实现

采用认为构造的字母和数字组成的数据集，进行上述算法的实现

```
corpus = [
    # numbers
    &quot;5 2 4 8 6 2 3 6 4&quot;,
    &quot;4 8 5 6 9 5 5 6&quot;,
    &quot;1 1 5 2 3 3 8&quot;,
    &quot;3 6 9 6 8 7 4 6 3&quot;,
    &quot;8 9 9 6 1 4 3 4&quot;,
    &quot;1 0 2 0 2 1 3 3 3 3 3&quot;,
    &quot;9 3 3 0 1 4 7 8&quot;,
    &quot;9 9 8 5 6 7 1 2 3 0 1 0&quot;,

    # alphabets, expecting that 9 is close to letters
    &quot;a t g q e h 9 u f&quot;,
    &quot;e q y u o i p s&quot;,
    &quot;q o 9 p l k j o k k o p&quot;,
    &quot;h g y i u t t a e q&quot;,
    &quot;i k d q r e 9 e a d&quot;,
    &quot;o p d g 9 s a f g a&quot;,
    &quot;i u y g h k l a s w&quot;,
    &quot;o l u y a o g f s&quot;,
    &quot;o p i u y g d a s j d l&quot;,
    &quot;u k i l o 9 l j s&quot;,
    &quot;y g i s h k j l f r f&quot;,
    &quot;i o h n 9 9 d 9 f a 9&quot;,
]
```

CBOW的实现

1.  数据集构建

以  t-2, t-1, t+1, t+2 为输入，  t 为 label

```
from torch import nn
import torch
from torch.nn.functional import cross_entropy
import numpy as np
import itertools
from utils import Dataset

class Dataset:
    def __init__(self, x, y, v2i, i2v):
        self.x, self.y = x, y
        self.v2i, self.i2v = v2i, i2v
        self.vocab = v2i.keys()

    def sample(self, n):
        b_idx = np.random.randint(0, len(self.x), n)
        bx, by = self.x[b_idx], self.y[b_idx]
        return bx, by

    @property
    def num_word(self):
        return len(self.v2i)

def process_w2v_data(corpus, skip_window=2):
    all_words = [sentence.split(&quot; &quot;) for sentence in corpus]
    # groups all the iterables together and produces a single iterable as output
    all_words = np.array(list(itertools.chain(*all_words)))
    vocab, v_count = np.unique(all_words, return_counts=True)
    vocab = vocab[np.argsort(v_count)[::-1]]

    print(&quot;All vocabularies are sorted by frequency in decresing oreder&quot;)
    v2i = {v: i for i, v in enumerate(vocab)}
    i2v = {i: v for v, i in v2i.items()}

    pairs = []
    js = [i for i in range(-skip_window, skip_window + 1) if i != 0]

    for c in corpus:
        words = c.split(&quot; &quot;)
        w_idx = [v2i[w] for w in words]
        for i in range(skip_window, len(w_idx) - skip_window):
            context = []
            for j in js:
                context.append(w_idx[i + j])
            pairs.append(context + [w_idx[i]])

    pairs = np.array(pairs)
    print(&quot;5 expample pairs:\n&quot;, pairs[:5])
    x, y = pairs[:, :-1], pairs[:, -1]
    return Dataset(x, y, v2i, i2v)
```

2. 模型构建，包括向量维度、优化器、损失函数

```
class CBOW(nn.Module):
    def __init__(self, v_dim, emb_dim):
        super().__init__()
        self.v_dim = v_dim
        self.embeddings = nn.Embedding(v_dim, emb_dim)
        self.embeddings.weight.data.normal_(0, 0.1)

        self.hidden_out = nn.Linear(emb_dim, v_dim)
        self.opt = torch.optim.SGD(self.parameters(), momentum=0.9, lr=0.01)

    def forward(self, x):
        o = self.embeddings(x)  # [n, skip_window*2, emb_dim]
        o = torch.mean(o, dim=1)  # [n, emb_dim]
        return o

    def loss(self, x, y, training=None):
        embedded = self(x, training)
        pred = self.hidden_out(embedded)
        return cross_entropy(pred, y)

    def step(self, x, y):
        self.opt.zero_grad()
        loss = self.loss(x, y, True)
        loss.backward()
        self.opt.step()
        return loss.detach().numpy()
```

3. 训练函数

```
def train(model, data):
    if torch.cuda.is_available():
        device = torch.device(&quot;cuda&quot;)
        model = model.cuda()
    else:
        device = torch.device(&quot;cpu&quot;)
        model = model.cpu()
    for t in range(2500):
        bx, by = data.sample(16)
        bx, by = torch.from_numpy(bx).to(device), torch.from_numpy(by).to(device)
        loss = model.step(bx, by)
        if t % 200 == 0:
            print(f&quot;step: {t}  |  loss: {loss}&quot;)
```

4. 进行训练并可视化

```
import matplotlib.pyplot as plt

def show_w2v_word_embedding(model, data, path):
    word_emb = model.embeddings.weight.data.numpy()
    for i in range(data.num_word):
        c = &quot;blue&quot;
        try:
            int(data.i2v[i])
        except:
            c = &quot;red&quot;

        plt.text(word_emb[i, 0], word_emb[i, 1], s=data.i2v[i], color=c, weight=&quot;bold&quot;)

    plt.xlim(word_emb[:, 0].min() - 0.5, word_emb[:, 0].max() + 0.5)
    plt.ylim(word_emb[:, 1].min() - 0.5, word_emb[:, 1].max() + 0.5)
    plt.xticks(())
    plt.yticks(())
    plt.xlabel(&quot;embedding dim1&quot;)
    plt.ylabel(&quot;embedding dim2&quot;)
    plt.savefig(path, dpi=300, format=&quot;png&quot;)
    plt.show()

if __name__ == &quot;__main__&quot;:
    data = process_w2v_data(corpus, skip_window=2)
    model = CBOW(data.num_word, 2)
    train(model, data)

    show_w2v_word_embedding(model, data, &quot;cbow.png&quot;)
```

日志如下所示：

```
step: 0  |  loss: 3.6389613151550293
step: 200  |  loss: 3.203524589538574
step: 400  |  loss: 3.2657129764556885
step: 600  |  loss: 3.220423698425293
step: 800  |  loss: 2.988870620727539
step: 1000  |  loss: 2.9030673503875732
step: 1200  |  loss: 2.5622284412384033
step: 1400  |  loss: 2.645578145980835
step: 1600  |  loss: 2.196150302886963
step: 1800  |  loss: 2.2573604583740234
step: 2000  |  loss: 2.5197746753692627
step: 2200  |  loss: 2.5119199752807617
step: 2400  |  loss: 2.034820079803467
```

根据其embedding绘制其空间分布图，可见9 与字母距离较近

![](img/6474adeed61857d52fcae0c83f1e9f51.png)

**Skip-gram 实现**

与CBOW类似，只是数据处理和模型略有不同

```
def process_w2v_data(corpus, skip_window=2, method=&quot;skip_gram&quot;):
    all_words = [sentence.split(&quot; &quot;) for sentence in corpus]
    # groups all the iterables together and produces a single iterable as output
    all_words = np.array(list(itertools.chain(*all_words)))
    vocab, v_count = np.unique(all_words, return_counts=True)
    vocab = vocab[np.argsort(v_count)[::-1]]

    print(&quot;All vocabularies are sorted by frequency in decresing oreder&quot;)
    v2i = {v: i for i, v in enumerate(vocab)}
    i2v = {i: v for v, i in v2i.items()}

    pairs = []
    js = [i for i in range(-skip_window, skip_window + 1) if i != 0]

    for c in corpus:
        words = c.split(&quot; &quot;)
        w_idx = [v2i[w] for w in words]
        if method == &quot;skip_gram&quot;:
            for i in range(len(w_idx)):
                for j in js:
                    if i + j &lt; 0 or i + j &gt;= len(w_idx):
                        continue
                    pairs.append((w_idx[i], w_idx[i + j]))
        elif method.lower() == &quot;cbow&quot;:
            for i in range(skip_window, len(w_idx) - skip_window):
                context = []
                for j in js:
                    context.append(w_idx[i + j])
                pairs.append(context + [w_idx[i]])
        else:
            raise ValueError

    pairs = np.array(pairs)
    print(&quot;5 expample pairs:\n&quot;, pairs[:5])
    if method.lower() == &quot;skip_gram&quot;:
        x, y = pairs[:, 0], pairs[:, 1]
    elif method.lower() == &quot;cbow&quot;:
        x, y = pairs[:, :-1], pairs[:, -1]
    else:
        raise ValueError
    return Dataset(x, y, v2i, i2v)

class SkipGram(nn.Module):

    def __init__(self,v_dim,emb_dim):
        super().__init__()
        self.v_dim = v_dim
        self.embeddings = nn.Embedding(v_dim,emb_dim)
        self.embeddings.weight.data.normal_(0,0.1)
        self.hidden_out = nn.Linear(emb_dim,v_dim)

        self.opt = torch.optim.Adam(self.parameters(),lr=0.01)
    
    def forward(self,x,training=None, mask=None):
        # x.shape = [n,]
        o = self.embeddings(x)  # [n, emb_dim]
        return o
    
    def loss(self,x,y,training=None):
        embedded = self(x,training)
        pred= self.hidden_out(embedded)
        return cross_entropy(pred,y)
    
    def step(self,x,y):
        self.opt.zero_grad()
        loss = self.loss(x,y,True)
        loss.backward()
        self.opt.step()
        return loss.detach().numpy()
```

其结果如下：

```
step: 0  |  loss: 3.557112216949463
step: 200  |  loss: 3.3495876789093018
step: 400  |  loss: 2.5264182090759277
step: 600  |  loss: 2.787606954574585
step: 800  |  loss: 2.678286552429199
step: 1000  |  loss: 3.160764217376709
step: 1200  |  loss: 3.1824135780334473
step: 1400  |  loss: 2.823436737060547
step: 1600  |  loss: 2.750861167907715
step: 1800  |  loss: 2.4983673095703125
step: 2000  |  loss: 2.409540891647339
step: 2200  |  loss: 2.5646309852600098
step: 2400  |  loss: 2.5172579288482666
```

![](img/415ff343fc3eec9570b99c51de885359.png)

参考资料

[1]. https://github.com/yhangf/ML-NOTE

[2]. [机器是这样理解语言 - 词向量 - 自然语言处理 | 莫烦](https://link.zhihu.com/?target=https%3A//mofanpy.com/tutorials/machine-learning/nlp/intro-w2v/)Python (mofanpy.com)

> 夜雨翦春韭，新炊间黄粱。——杜甫《赠卫八处士》

编辑于 2022-02-01 21:18