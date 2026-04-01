# NLP（二）：浅谈分词

> 来源：[https://agijuejin.feishu.cn/docx/KFoMdZCdbojQRUxPDv5cEivqnTc](https://agijuejin.feishu.cn/docx/KFoMdZCdbojQRUxPDv5cEivqnTc)

> 作者：紫气东来

> 原文：[https://zhuanlan.z](https://zhuanlan.zhihu.com/p/458452872)hihu.com/p/458452...

分词（tokenization，也叫word segmentation）是一种操作，它按照特定需求，把文本切分成一个字符串序列(其元素一般称为token，或者叫词语)。对于西方屈折语的文本 ，词与词之间有空格之类的显式标志指示词的边界，但是有些固定搭配仍然需要当作一个词；而对于很多孤立语和黏着语 (如汉语、日语、越南语、藏语等) ，词与词之间没有空格，因此自动分词问题就成了计算机处理孤立语和黏着语文本时面临的首要基础性工作 。

![](img/b57b423a2f8cec4971827182cb00a510.png)

## 一、中文分词

中文分词(Chinese Word Segmentation) 指的是将一个汉字序列切分成一个个单独的词。分词就是将连续的字序列按照一定的规范重新组合成词序列的过程。

### 1.1 基本问题

分词中涉及到三个基本问题：分词规范、歧义切分和未登录词的识别。

**分词规范**

“词是什么”(词的抽象定义)及“什么是 词”(词的具体界定)，这两个基本问题有点飘忽不定，迄今拿不出一 个公认的、具有权威性的词表来。主要困难出自两个方面:一方面是单 字词与词素之间的划界，另一方面是词与短语(词组)的划界。

**歧义切分**

歧义字段在汉语文本中普遍存在，处理这类问题可能需要进行复杂的上下文语义分析，甚至韵律分析(语气、重音、停顿等)。

```
例如：“结合成” 
一种切分为:(a)结合│成;另一种切分为:(b)结│合成
```

**未登录词问题**

未登录词又称为生词(unknown word)，可以有两种解释:一是指 已有的词表中没有收录的词;二是指已有的训练语料中未曾出现过的词，又称之为集外词(out of vocabulary，OOV)。

### 1.2 分词方法

| 方法 | 典型算法 | 特点 |
| --- | --- | --- |
| 基于词典的方法 | 正向最大匹配法（从左到右）；  逆向最大匹配法（从右到左）；最小切分（每一句中切出的词数最小）双向最大匹配（进行从左到右、从右到左两次扫描） | 本质上就是字符串匹配的方法，将一串文本中的文字片段和已有的词典进行匹配，如果匹配到，则此文字片段就作为一个分词结果。但是基于词典的机械切分会遇到多种问题，最为常见的包括歧义切分问题和未登录词问题 |
| 基于统计的方法 | N-最短路径方法、基于词的n元语法模型的分词方法、由字构词的汉语分词方法、基于词感知机算法的汉语分词方法、基于字的生成式模型和区分式模型相结合的汉语分词方法等 | 基于统计的分词方法是在给定大量已经分词的文本的前提下，利用统计机器学习模型学习词语切分的规律（称为训练），从而实现对未知文本的切分。例如最大概率分词方法和最大熵分词方法等 |
| 基于深度学习的分词方法 | LSTM+CRF、BiLSTM+CRF等 | 深度学习方法为分词技术带来了新的思路，直接以最基本的向量化原子特征作为输入，经过多层非线性变换，输出层就可以很好的预测当前字的标记或下一个动作。在深度学习的框架下，仍然可以采用基于子序列标注的方式，或基于转移的方式，以及半马尔科夫条件随机场。 |

下面详细讨论 N-gram 算法及其实现：

假设我们有一个由 n 个词组成的句子  S=\left(w_{1}, w_{2}, \cdots, w_{n}\right) ，如何衡量它的概率呢？假设每一个单词  w_i 都要依赖于从第一个单词  w_1 到它之前一个单词  w_{i-1} 的影响：

p(S)=p\left(w_{1} w_{2} \cdots w_{n}\right)=p\left(w_{1}\right) p\left(w_{2} \mid w_{1}\right) \cdots p\left(w_{n} \mid w_{n-1} \cdots w_{2} w_{1}\right)

这样前后依赖过长，会导致参数空间过大和数据稀疏严重，因此引入马尔科夫假设（Markov Assumption）：一个词的出现仅与它之前的若干个词有关，即：

p\left(w_{1} \cdots w_{n}\right)=\prod p\left(w_{i} \mid w_{i-1} \cdots w_{1}\right) \approx \prod p\left(w_{i} \mid w_{i-1} \cdots w_{i-N+1}\right)

其中  i 为某个词的位置，  N 为依赖的词的个数

+   当n=1时，即一元模型（Uni-gram）：  P\left(w_{1}, w_{2}, \cdots, w_{m}\right)=\prod_{i=1}^{m} P\left(w_{i}\right)

+   当n=2时，即二元模型（Bi-gram）：  P\left(w_{1}, w_{2}, \cdots, w_{m}\right)=\prod_{i=1}^{m} P\left(w_{i}｜w_{i-1}\right)

+   当n=3时，即三元模型（Tri-gram）  P\left(w_{1}, w_{2}, \cdots, w_{m}\right)=\prod_{i=1}^{m} P\left(w_{i}｜w_{i-2}w_{i-1}\right)
N-gram的  N 可以取很高，然而现实中一般 bi-gram 和 tri-gram 就够用了。

采用极大似然估计（Maximum Likelihood Estimation，MLE）来计算各项的概率，通常计算频率来实现：

\begin{equation} \begin{aligned} p\left(w_{n} \mid w_{n-1}\right) &=\frac{C\left(w_{n-1} w_{n}\right)}{C\left(w_{n-1}\right)} \\ p\left(w_{n} \mid w_{n-1} w_{n-2}\right) &=\frac{C\left(w_{n-2} w_{n-1} w_{n}\right)}{C\left(w_{n-2} w_{n-1}\right)} \\ p\left(w_{n} \mid w_{n-1} \cdots w_{2} w_{1}\right) &=\frac{C\left(w_{1} w_{2} \cdots w_{n}\right)}{C\left(w_{1} w_{2} \cdots w_{n-1}\right)} \end{aligned} \end{equation}

以Bi-gram为例，我们有这样一个由三句话组成的语料库：

```
&lt;s&gt; I am Sam &lt;s&gt;
&lt;s&gt; Sam I am &lt;s&gt;
&lt;s&gt; I do not like eggs and ham &lt;s&gt;
```

容易统计，“I”出现了3次，“I am”出现了2次，因此能计算概率：

\begin{equation} p(a m \mid I)=\frac{2}{3} \end{equation}

同理，还能计算出如下概率：

\begin{equation} \begin{array}{ccc} p(I \mid<s>)=0.67 & p(\text { Sam } \mid a m)=0.5 & p(<s>\mid \text { Sam })=0.5 \\ p(d o \mid I)=0.33 & p(\text { not } \mid d o)=1 & p(\text { like } \mid \text { not })=1 \end{array} \end{equation}

另外再提供一个《Language Modeling with Ngrams》中的例子，Jurafsky et al., 1994 从加州一个餐厅的数据库中做了一些统计：

![](img/95760e56bc5fb2ddbf064a3eab3d1ee4.png)

据统计，  \begin{equation} p(I \mid<s>)=0.25, p(<s>\mid f o o d)=0.68 \end{equation} ，则

\begin{equation} \begin{gathered} p(<s>I \text { want chinese } f o o d<s>) =0.25 \times 0.33 \times 0.0065 \times 0.52 \times 0.68=1.896 \times 10^{-4} \end{gathered} \end{equation}

就算出了“I want chinese food”这句话的概率，由于通常较小，常用log值来代替。

假设待切分语句为：“研究生物学”，我们要怎样进行切分呢，直观的讲我们可以看出就这么一句简单的话包含了“研究”、“研究生”、“生物”、“生物学”多个词语，那么直观上我们有如下几种切分方式：

```
研究/生物学
研究生/物/学
研究/生物/学
研/究/生/物/学
```

将这些切法构建为一幅有向无环图，结点为词语，边为条件概率

![](img/37feb56b998023b7f7e79eea8f86dc9c.png)

最大化概率2-gram分词算法

```
  1、将带分词的字符串从左到右切分为  ；
  2、计算当前词与所有前驱词的概率
  3、计算该词的累计概率值，并筛选最大的累计概率则为最好的前驱点；
  4、重复步骤3，直到该字符串结束；
  5、从  开始，按照从右到左的顺序，依次将没歌词的最佳前驱词输出，即字符串的分词结束。
```

step 1. 为了计算词与词之间的概率，首先需要加载词典并计算转移概率：

```
import math
class MaxProbCut:
    def __init__(self):
        self.word_dict = {}  # 记录概率,1-gram
        self.word_dict_count = {}  # 记录词频,1-gram
        self.trans_dict = {}  # 记录概率,2-gram
        self.trans_dict_count = {}  # 记录词频,2-gram
        self.max_wordlen = 0  #词的最长长度
        self.all_freq = 0  # 所有词的词频总和,1-gram
        word_count_path = &quot;./model/word_dict.model&quot;
        word_trans_path = &#x27;./model/trans_dict.model&#x27;
        self.init(word_count_path, word_trans_path)

    # 加载词典
    def init(self, word_count_path, word_trans_path):
        self.word_dict_count = self.load_model(word_count_path)
        self.all_freq = sum(self.word_dict_count.values())  # 所有词的词频
        self.max_wordlen = max(len(key) for key in self.word_dict_count.keys())
        for key in self.word_dict_count:
            self.word_dict[key] = math.log(self.word_dict_count[key] / self.all_freq)
        #计算转移概率
        Trans_dict = self.load_model(word_trans_path)
        for pre_word, post_info in Trans_dict.items():
            for post_word, count in post_info.items():
                word_pair = pre_word + &#x27; &#x27; + post_word
                self.trans_dict_count[word_pair] = float(count)
                if pre_word in self.word_dict_count.keys():
                    self.trans_dict[key] = math.log(count / self.word_dict_count[pre_word])  # 取自然对数，归一化
                else:
                    self.trans_dict[key] = self.word_dict[post_word]

    #加载预训练模型
    def load_model(self, model_path):
        f = open(model_path, &#x27;r&#x27;)
        a = f.read()
        word_dict = eval(a)
        f.close()
        return word_dict
```

step 2. 在当前节点的所有前驱节点种，寻找最佳前驱节点

![](img/11da1186d67d3c0a3b2119098ecb3064.png)

```
    # 估算未出现的词的概率,根据beautiful data里面的方法估算，平滑算法
    def get_unknow_word_prob(self, word):
        return math.log(1.0 / (self.all_freq ** len(word)))

    # 获取候选词的概率
    def get_word_prob(self, word):
        if word in self.word_dict.keys():  # 如果字典包含这个词
            prob = self.word_dict[word]
        else:
            prob = self.get_unknow_word_prob(word)
        return prob

    #获取转移概率
    def get_word_trans_prob(self, pre_word, post_word):
        trans_word = pre_word + &quot; &quot; + post_word
        
        if trans_word in self.trans_dict_count.keys():
            trans_prob = math.log(self.trans_dict_count[trans_word] / self.word_dict_count[pre_word])
        else:
            trans_prob = self.get_word_prob(post_word)
        return trans_prob

    # 寻找node的最佳前驱节点，方法为寻找所有可能的前驱片段
    def get_best_pre_node(self, sentence, node, node_state_list):
        # 如果node比最大词长小，取的片段长度以node的长度为限
        max_seg_length = min([node, self.max_wordlen])
        pre_node_list = []  # 前驱节点列表

        # 获得所有的前驱片段，并记录累加概率
        for segment_length in range(1, max_seg_length + 1):
            segment_start_node = node - segment_length
            segment = sentence[segment_start_node:node]  # 获取片段
            pre_node = segment_start_node  # 取该片段，则记录对应的前驱节点
            if pre_node == 0:
                # 如果前驱片段开始节点是序列的开始节点，
                # 则概率为&lt;S&gt;转移到当前词的概率
                segment_prob = self.get_word_trans_prob(&quot;&lt;BEG&gt;&quot;, segment)
            else:  # 如果不是序列开始节点，按照二元概率计算
                # 获得前驱片段的前一个词
                pre_pre_node = node_state_list[pre_node][&quot;pre_node&quot;]
                pre_pre_word = sentence[pre_pre_node:pre_node]
                segment_prob = self.get_word_trans_prob(pre_pre_word, segment)

            pre_node_prob_sum = node_state_list[pre_node][&quot;prob_sum&quot;]  # 前驱节点的概率的累加值
            # 当前node一个候选的累加概率值
            candidate_prob_sum = pre_node_prob_sum + segment_prob
            pre_node_list.append((pre_node, candidate_prob_sum))

        # 找到最大的候选概率值
        (best_pre_node, best_prob_sum) = max(pre_node_list, key=lambda d: d[1])

        return best_pre_node, best_prob_sum
```

step 3 分词逻辑，即执行最大化概率2-gram分词算法

```
    #切词主函数
    def cut(self, sentence):
        sentence = sentence.strip()
        # 初始化
        node_state_list = []  # 记录节点的最佳前驱，index就是位置信息
        # 初始节点，也就是0节点信息
        ini_state = {}
        ini_state[&quot;pre_node&quot;] = -1  # 前一个节点
        ini_state[&quot;prob_sum&quot;] = 0  # 当前的概率总和
        node_state_list.append(ini_state)
        # 字符串概率为2元概率， P(a b c) = P(a|&lt;S&gt;)P(b|a)P(c|b)
        # 逐个节点寻找最佳前驱节点
        for node in range(1, len(sentence) + 1):
            # 寻找最佳前驱，并记录当前最大的概率累加值
            (best_pre_node, best_prob_sum) = self.get_best_pre_node(sentence, node, node_state_list)

            # 添加到队列
            cur_node = {}
            cur_node[&quot;pre_node&quot;] = best_pre_node
            cur_node[&quot;prob_sum&quot;] = best_prob_sum
            node_state_list.append(cur_node)
            # print(&quot;cur node list&quot;,node_state_list)

        # step 2, 获得最优路径,从后到前
        best_path = []
        node = len(sentence)  # 最后一个点
        best_path.append(node)
        while True:
            pre_node = node_state_list[node][&quot;pre_node&quot;]
            if pre_node == -1:
                break
            node = pre_node
            best_path.append(node)
        best_path.reverse()

        # step 3, 构建切分
        word_list = []
        for i in range(len(best_path) - 1):
            left = best_path[i]
            right = best_path[i + 1]
            word = sentence[left:right]
            word_list.append(word)

        return word_list
```

结果测试

```
def test():
    cuter = MaxProbCut()
    sentence = &quot;今天我不知道你为什么会这个样子&quot;
    seg_sentence = cuter.cut(sentence)
    print(&quot;original sentence: &quot; , sentence)
    print(&quot;segment result: &quot;, seg_sentence)

test()

#original sentence:  今天我不知道你为什么会这个样子
#segment result:  [&#x27;今天&#x27;, &#x27;我&#x27;, &#x27;不&#x27;, &#x27;知道&#x27;, &#x27;你&#x27;, &#x27;为什么&#x27;, &#x27;会&#x27;, &#x27;这个&#x27;, &#x27;样子&#x27;]
```

对于中文分词，有很多很成熟的工具，以下简要介绍几个：

**Jieba： **“结巴”中文分词：做最好的 Python 中文分词组件

Github地址： https:// github.com/fxsjy/jieba

```
import jieba
seg_list = jieba.cut(&quot;研究生研究生物学&quot;)  # 默认是精确模式
print(&quot;, &quot;.join(seg_list))

#研究生, 研究, 生物学
```

**SnowNLP： **SnowNLP是一个python写的类库，可以方便的处理中文文本内容。

Github地址： [https:// github.c](https://link.zhihu.com/?target=https%3A//github.com/isnowfy/snownlp)om/isnowfy/snow nlp

```
from snownlp import SnowNLP
s = SnowNLP(u&#x27;上海交通大学建于1896年，原名南洋公学&#x27;)
s.words
Out[9]: [&#x27;上海&#x27;, &#x27;交通&#x27;, &#x27;大学&#x27;, &#x27;建&#x27;, &#x27;于&#x27;, &#x27;1896&#x27;, &#x27;年&#x27;, &#x27;，&#x27;, &#x27;原&#x27;, &#x27;名&#x27;, &#x27;南洋&#x27;, &#x27;公&#x27;, &#x27;学&#x27;]
```

## 二、英文分词

在英文中，tokenize的目标是把输入的文本流，切分成一个个子串，每个子串相对有完整的语义，便于学习embedding表达和后续模型的使用。下面通过 Huggingface 的 tokenizers工具来简要讨论。

tokenize有三种粒度： word/subword/char

+   **word词 **，是最自然的语言单元。对于英文等自然语言来说，存在着天然的分隔符，比如说空格，或者是一些标点符号，对词的切分相对容易。但是对于一些东亚文字包括中文来说，就需要某种分词算法才行。顺便说一下，Tokenizers库中，基于规则切分部分，采用了spaCy和Moses两个库。如果基于词来做词汇表，由于长尾现象的存在，这个词汇表可能会超大。像Transformer XL库就用到了一个26.7万个单词的词汇表。这需要极大的embedding matrix才能存得下。embedding matrix是用于查找取用token的embedding vector的。这对于内存或者显存都是极大的挑战。常规的词汇表，一般大小不超过5万。

+   **char/字符, **也就是说，我们的词汇表里只有最基本的字符。而一般来讲，字符的数量是少量有限的。这样做的问题是，由于字符数量太小，我们在为每个字符学习嵌入向量的时候，每个向量就容纳了太多的语义在内，学习起来非常困难。

+   **subword子词级 **，它介于字符和单词之间。比如说Transformers可能会被分成Transform和ers两个部分。 这个方案平衡了词汇量和语义独立性，是相对较优的方案 。它的处理原则是， 常用词应该保持原状，生僻词应该拆分成子词以共享token压缩空间 。

**常用Tokenizer算法**

![](img/9e72708bea715faa6a98eef8e835338e.png)

### **2.1 Byte-Pair Encoding (B**PE) / Byte-level BPE

**2.1.1 BPE**

+   首先，它依赖于一种预分词器 pretokenizer 来完成初步的切分。pretokenizer 可以是简单基于空格的，也可以是基于规则的；

+   分词之后，统计每个词出现的频次供后续计算使用。例如，我们统计到了5个词的词频

> ("hug", 10), ("pug", 5), ("pun", 12), ("bun", 4), ("hugs", 5)

+   建立基础词汇表，包括所有的字符，即：

> ["b", "g", "h", "n", "p", "s", "u"]

+   根据规则，我们分别考察2-gram，3-gram的基本字符组合，把高频的ngram组合依次加入到词汇表当中，直到词汇表达到预定大小停止。比如，我们计算出ug/un/hug三种组合出现频次分别为20，16和15，加入到词汇表中。

+   最终词汇表的大小 = 基础字符词汇表大小 + 合并串的数量，比如像GPT，它的词汇表大小 40478 = 478(基础字符) + 40000（merges）。添加完后，我们词汇表变成：

> ["b", "g", "h", "n", "p", "s", "u", "ug", "un", "hug"]

实际使用中，如果遇到未知字符用<unk>代表。

2.1.2 Byte-level BPE

BPE的一个问题是，如果遇到了unicode，基本字符集可能会很大。一种处理方法是我们以一个字节为一种“字符”，不管实际字符集用了几个字节来表示一个字符。这样的话，基础字符集的大小就锁定在了256。

例如，像GPT-2的词汇表大小为50257 = 256 + <EOS> + 50000 mergers，<EOS>是句子结尾的特殊标记。

```
In [1]: from transformers import GPT2Tokenizer
In [2]: tokenizer = GPT2Tokenizer.from_pretrained(&#x27;gpt2&#x27;)
In [3]: text = &quot;Replace me by any text you&#x27;d like.&quot;
In [4]: encoded_input = tokenizer(text, return_tensors=&#x27;pt&#x27;)
In [5]: encoded_input
Out[5]: {&#x27;input_ids&#x27;: tensor([[3041, 5372,  502,  416,  597, 2420,  345, 1549,  588,   13]]), &#x27;attention_mask&#x27;: tensor([[1, 1, 1, 1, 1, 1, 1, 1, 1, 1]])}
```

### **2.2 WordPiece**

WordPiece，从名字好理解，它是一种子词粒度的tokenize算法subword tokenization algorithm，很多著名的Transformers模型，比如BERT/DistilBERT/Electra都使用了它。

它的原理非常接近BPE，不同之处在于，它在做合并的时候**，并不是每次找最高频的**组合，而是找能够 最大化训练集数据似然 的merge，即它每次合并的两个字符串A和B，应该具有最大的  \begin{equation} \frac{P(A B)}{P(A) P(B)} \end{equation} 值。合并AB之后，所有原来切成A+B两个tokens的就只保留AB一个token，整个训练集上最大似然变化量与  \begin{equation} \frac{P(A B)}{P(A) P(B)} \end{equation} 成正比。

```
In [5]: from transformers import BertTokenizer
In [6]: tokenizer = BertTokenizer.from_pretrained(&#x27;bert-base-uncased&#x27;)
In [7]: text = &quot;Replace me by any text you&#x27;d like.&quot;
In [8]: encoded_input = tokenizer(text, return_tensors=&#x27;pt&#x27;)
In [9]: encoded_input
Out[9]: {&#x27;input_ids&#x27;: tensor([[ 101, 5672, 2033, 2011, 2151, 3793, 2017, 1005, 1040, 2066, 1012,  102]]), &#x27;token_type_ids&#x27;: tensor([[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]]), &#x27;attention_mask&#x27;: tensor([[1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1]])}
```

### **2.3 Unigram**

与BPE或者WordPiece不同，Unigram的算法思想是从一个巨大的词汇表出发，再逐渐删除trim down其中的词汇，直到size满足预定义。

初始的词汇表可以采用所有预分词器分出来的词，再加上所有高频的子串。

每次从词汇表中删除词汇的原则是使预定义的损失最小。训练时，计算loss的公式为：

\begin{equation} L o s s=-\sum_{i=1}^{N} \log \left(\sum_{x \in S(x_i)} p(x)\right) \end{equation}

假设训练文档中的所有词分别为  \begin{equation} x_{1} ; x_{2} \ldots x_{N} \end{equation} ，而每个词tokenize的方法是一个集合  S(x_i) 。当一个词汇表确定时，每个词tokenize的方法集合  S(x_i) 就是确定的，而每种方法对应着一个概率  p(x) 。如果从词汇表中删除部分词，则某些词的tokenize的种类集合就会变少，  log(*) 中的求和项就会减少，从而增加整体loss。

Unigram算法每次会从词汇表中挑出使得loss增长最小的10%~20%的词汇来删除。

一般Unigram算法会与SentencePiece算法连用。

```
&gt;&gt;&gt; from transformers import AlbertTokenizer 
&gt;&gt;&gt; tokenizer = AlbertTokenizer.from_pretrained(&#x27;albert-base-v2&#x27;) 
&gt;&gt;&gt; encoded_input = tokenizer(text, return_tensors=&#x27;pt&#x27;)
&gt;&gt;&gt; encoded_input
{&#x27;input_ids&#x27;: tensor([[   2, 3934,   55,   34,  186, 1854,   42,   22,   43,  101,    9,    3]]), &#x27;token_type_ids&#x27;: tensor([[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]]), &#x27;attention_mask&#x27;: tensor([[1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1]])}
```

### 2.4 SentencePiece

SentencePiece，顾名思义，它是把一个句子看作一个整体，再拆成片段，而没有保留天然的词语的概念。一般地，它把空格space也当作一种特殊字符来处理，再用BPE或者Unigram算法来构造词汇表。

比如，XLNetTokenizer就采用了_来代替空格，解码的时候会再用空格替换回来。

目前，Tokenizers库中，所有使用了SentencePiece的都是与Unigram算法联合使用的，比如ALBERT、XLNet、Marian和T5.

```
&gt;&gt;&gt; from transformers import T5Tokenizer
&gt;&gt;&gt; tokenizer = T5Tokenizer.from_pretrained(&#x27;t5-base&#x27;)
&gt;&gt;&gt; text = &quot;Replace me by any text you&#x27;d like.&quot;
&gt;&gt;&gt; encoded_input = tokenizer(text, return_tensors=&#x27;pt&#x27;)
&gt;&gt;&gt; encoded_input
{&#x27;input_ids&#x27;: tensor([[28035,   140,    57,   136,  1499,    25,    31,    26,   114,     5,
             1]]), &#x27;attention_mask&#x27;: tensor([[1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1]])}
```

## 三、使用 SentencePiece 进行词表扩充

### 3.1 SentencePiece 的用法

**Sente**nc**ePiec**e分为两部分： 训练模型 和 使用模型 。

[训练模型主要](https://link.zhihu.com/?target=https%3A//github.com/shjwudp/shu/tree/master/books)是根据训练数据集，通过训练获得分词模型及词表，下面以名著 《水浒传》 为数据进行训练

```
spm_train --input=shuihuzhuan.txt --model_prefix=shuihuzhuan-tokenizer --vocab_size=4000 --character_coverage=0.9995 --model_type=bpe
```

参数说明：

+   --input : 训练语料文件，可以传递以逗号分隔的文件列表。文件格式为每行一个句子。 无需运行tokenizer、normalizer或preprocessor。 默认情况下，SentencePiece 使用 Unicode NFKC 规范化输入。

+   --model_prefix ：输出模型名称前缀。 训练完成后将生成 <model_name>.model 和 <model_name>.vocab 文件。

+   --vocab_size ：训练后的词表大小，例如：8000、16000 或 32000

+   --character_coverage ：模型覆盖的字符数量，对于字符集丰富的语言（如日语或中文）推荐默认值为 0.9995，对于其他字符集较小的语言推荐默认值为 1.0。

+   --model_type ：模型类型。 可选值：unigram（默认）、bpe、char 或 word 。 使用word类型时，必须对输入句子进行pretokenized。

训练日志如下：

```
trainer_interface.cc(267) LOG(INFO) Loading corpus: shuihuzhuan.txt
trainer_interface.cc(315) LOG(INFO) Loaded all 5854 sentences
trainer_interface.cc(330) LOG(INFO) Adding meta_piece: &lt;unk&gt;
trainer_interface.cc(330) LOG(INFO) Adding meta_piece: &lt;s&gt;
trainer_interface.cc(330) LOG(INFO) Adding meta_piece: &lt;/s&gt;
trainer_interface.cc(335) LOG(INFO) Normalizing sentences...
trainer_interface.cc(384) LOG(INFO) all chars count=861752
trainer_interface.cc(392) LOG(INFO) Done: 99.9501% characters are covered.
trainer_interface.cc(402) LOG(INFO) Alphabet size=3778
trainer_interface.cc(403) LOG(INFO) Final character coverage=0.999501
trainer_interface.cc(435) LOG(INFO) Done! preprocessed 5854 sentences.
trainer_interface.cc(441) LOG(INFO) Tokenizing input sentences with whitespace: 5854
trainer_interface.cc(451) LOG(INFO) Done! 6066
bpe_model_trainer.cc(166) LOG(INFO) Updating active symbols. max_freq=9371 min_freq=13
bpe_model_trainer.cc(257) LOG(INFO) Added: freq=650 size=20 all=149043 active=12605 piece=太尉
bpe_model_trainer.cc(257) LOG(INFO) Added: freq=469 size=40 all=154021 active=17583 piece=不得
bpe_model_trainer.cc(257) LOG(INFO) Added: freq=358 size=60 all=157858 active=21420 piece=我们
bpe_model_trainer.cc(257) LOG(INFO) Added: freq=316 size=80 all=161504 active=25066 piece=▁宋江
bpe_model_trainer.cc(257) LOG(INFO) Added: freq=272 size=100 all=164517 active=28079 piece=天子
bpe_model_trainer.cc(166) LOG(INFO) Updating active symbols. max_freq=271 min_freq=11
bpe_model_trainer.cc(257) LOG(INFO) Added: freq=250 size=120 all=167565 active=11128 piece=公人
bpe_model_trainer.cc(257) LOG(INFO) Added: freq=229 size=140 all=170344 active=13907 piece=却说
bpe_model_trainer.cc(257) LOG(INFO) Added: freq=209 size=160 all=173235 active=16798 piece=当时
bpe_model_trainer.cc(257) LOG(INFO) Added: freq=193 size=180 all=176117 active=19680 piece=二人
bpe_model_trainer.cc(257) LOG(INFO) Added: freq=182 size=200 all=178474 active=22037 piece=王婆
bpe_model_trainer.cc(166) LOG(INFO) Updating active symbols. max_freq=181 min_freq=10
trainer_interface.cc(507) LOG(INFO) Saving model: shuihuzhuan-tokenizer.model
trainer_interface.cc(531) LOG(INFO) Saving vocabs: shuihuzhuan-tokenizer.vocab
```

可以查看 Topk 频率的 token ：

```
&gt; head -n20 shuihuzhuan-tokenizer.vocab
&lt;unk&gt;   0
&lt;s&gt;     0
&lt;/s&gt;    0
:“      -0
。”     -1
宋江    -2
?”      -3
!”      -4
两个    -5
一个    -6
李逵    -7
武松    -8
只见    -9
如何    -10
那里    -11
去了    -12
林冲    -13
军马    -14
哥哥    -15
说道    -16
```

使用模型进行分词：

```
&gt; echo &quot;钱塘江上潮信来，今日方知我是我。&quot; | spm_encode --model=shuihuzhuan-tokenizer.model 
▁ 钱 塘 江 上 潮 信 来 , 今日 方 知 我 是 我 。
```

也可以输出为 ID 形式：

```
&gt; echo &quot;钱塘江上潮信来，今日方知我是我。&quot; | spm_encode --model=shuihuzhuan-tokenizer.model  --output_format=id
234 509 2388 251 237 2230 598 228 222 40 374 306 250 235 250 223
```

将模型转化为词表：

```
spm_export_vocab --model=shuihuzhuan-tokenizer.model --output=shuihuzhuan.vocab
```

在 python 中加载模型使用：

```
import sentencepiece as spm
sp = spm.SentencePieceProcessor()
text=&quot;鞭舞两条龙尾，棍横一串狼牙。三军看得眼睛花，二将纵横交马。使棍的军班领袖，使鞭的将种堪夸。&quot;
sp.Load(&quot;shuihuzhuan-tokenizer.model&quot;)
print(sp.EncodeAsPieces(text))

[&#x27;▁&#x27;, &#x27;鞭&#x27;, &#x27;舞&#x27;, &#x27;两&#x27;, &#x27;条&#x27;, &#x27;龙&#x27;, &#x27;尾&#x27;, &#x27;,&#x27;, &#x27;棍&#x27;, &#x27;横&#x27;, &#x27;一&#x27;, &#x27;串&#x27;, &#x27;狼&#x27;, &#x27;牙&#x27;, &#x27;。&#x27;, &#x27;三&#x27;, &#x27;军&#x27;, &#x27;看&#x27;, &#x27;得&#x27;, &#x27;眼&#x27;, &#x27;睛&#x27;, &#x27;花&#x27;, &#x27;,&#x27;, &#x27;二&#x27;, &#x27;将&#x27;, &#x27;纵&#x27;, &#x27;横&#x27;, &#x27;交&#x27;, &#x27;马&#x27;, &#x27;。&#x27;, &#x27;使&#x27;, &#x27;棍&#x27;, &#x27;的&#x27;, &#x27;军&#x27;, &#x27;班&#x27;, &#x27;领&#x27;, &#x27;袖&#x27;, &#x27;,&#x27;, &#x27;使&#x27;, &#x27;鞭&#x27;, &#x27;的&#x27;, &#x27;将&#x27;, &#x27;种&#x27;, &#x27;堪&#x27;, &#x27;夸&#x27;, &#x27;。&#x27;]
```

以上基本熟悉了 sentencepiece 训练及使用词表模型的方法，下面将尝试进行词表扩充。

### 3.2 如何扩充词表

假如我们现在使用 sentencepiece 已经通过大量数据训练得到了中文词表模型 chinese_sp.model ，现希望将该词表扩充到标准的 LLaMA 词表中。

+   首先加载标准 LLaMA 分词器，并输出其基本信息：

```
import os
os.environ[&quot;PROTOCOL_BUFFERS_PYTHON_IMPLEMENTATION&quot;]=&quot;python&quot;
from transformers import LlamaTokenizer

llama_tokenizer = LlamaTokenizer.from_pretrained(&#x27;./vicuna-13b-v1.3&#x27;)
print(len(llama_tokenizer))
print(llama_tokenizer.all_special_tokens)
print(llama_tokenizer.all_special_ids)
print(llama_tokenizer.special_tokens_map)
```

输出：

```
32000
[&#x27;&lt;s&gt;&#x27;, &#x27;&lt;/s&gt;&#x27;, &#x27;&lt;unk&gt;&#x27;, &#x27;&lt;unk&gt;&#x27;]
[1, 2, 0, 0]
{&#x27;bos_token&#x27;: &#x27;&lt;s&gt;&#x27;, &#x27;eos_token&#x27;: &#x27;&lt;/s&gt;&#x27;, &#x27;unk_token&#x27;: &#x27;&lt;unk&gt;&#x27;, &#x27;pad_token&#x27;: &#x27;&lt;unk&gt;&#x27;}
```

+   然后使用 sentencepiece 加载训练好的中文词表

```
from sentencepiece import sentencepiece_model_pb2 as sp_pb2_model
import sentencepiece as spm

chinese_sp_model = spm.SentencePieceProcessor()
chinese_sp_model.Load(&#x27;./chinese_sp.model&#x27;)
llama_spm = sp_pb2_model.ModelProto()
llama_spm.ParseFromString(llama_tokenizer.sp_model.serialized_model_proto())
chinese_spm = sp_pb2_model.ModelProto()
chinese_spm.ParseFromString(chinese_sp_model.serialized_model_proto())

# print number of tokens
print(len(chinese_sp_model))
# 20000
```

+   将中文词元加入到 LLaMA 分词器中

```
## Add Chinese tokens to LLaMA tokenizer
llama_spm_tokens_set=set(p.piece for p in llama_spm.pieces)
print(len(llama_spm_tokens_set))
print(f&quot;Before:{len(llama_spm_tokens_set)}&quot;)
for p in chinese_spm.pieces:
    piece = p.piece
    if piece not in llama_spm_tokens_set:
        new_p = sp_pb2_model.ModelProto().SentencePiece()
        new_p.piece = piece
        new_p.score = 0
        llama_spm.pieces.append(new_p)
print(f&quot;New model pieces: {len(llama_spm.pieces)}&quot;)
```

输出：

```
32000
Before:32000
New model pieces: 49953
```

+   保存处理后的新的分词器

```
output_sp_dir = &#x27;merged_tokenizer_sp&#x27;
output_hf_dir = &#x27;merged_tokenizer_hf&#x27; # the path to save Chinese-LLaMA tokenizer
os.makedirs(output_sp_dir,exist_ok=True)
with open(output_sp_dir+&#x27;/chinese_llama.model&#x27;, &#x27;wb&#x27;) as f:
    f.write(llama_spm.SerializeToString())
tokenizer = LlamaTokenizer(vocab_file=output_sp_dir+&#x27;/chinese_llama.model&#x27;)

tokenizer.save_pretrained(output_hf_dir)
print(f&quot;Chinese-LLaMA tokenizer has been saved to {output_hf_dir}&quot;)
```

+   测试并比较两个分词器的效果

```
llama_tokenizer = LlamaTokenizer.from_pretrained(&#x27;../../../vllm/ckpt/vicuna-13b-v1.3&#x27;)
chinese_llama_tokenizer = LlamaTokenizer.from_pretrained(output_hf_dir)
text=&quot;木末芙蓉花，山中发红萼。涧户寂无人，纷纷开且落。&quot;
print(&quot;Test text:\n&quot;,text)
print(f&quot;Tokenized by LLaMA tokenizer:{llama_tokenizer.tokenize(text)}&quot;)
print(f&quot;Tokenized by Chinese-LLaMA tokenizer:{chinese_llama_tokenizer.tokenize(text)}&quot;)
```

结果如下：

```
Test text:
 木末芙蓉花，山中发红萼。涧户寂无人，纷纷开且落。
Tokenized by LLaMA tokenizer:[&#x27;▁&#x27;, &#x27;木&#x27;, &#x27;&lt;0xE6&gt;&#x27;, &#x27;&lt;0x9C&gt;&#x27;, &#x27;&lt;0xAB&gt;&#x27;, &#x27;&lt;0xE8&gt;&#x27;, &#x27;&lt;0x8A&gt;&#x27;, &#x27;&lt;0x99&gt;&#x27;, &#x27;&lt;0xE8&gt;&#x27;, &#x27;&lt;0x93&gt;&#x27;, &#x27;&lt;0x89&gt;&#x27;, &#x27;花&#x27;, &#x27;，&#x27;, &#x27;山&#x27;, &#x27;中&#x27;, &#x27;发&#x27;, &#x27;红&#x27;, &#x27;&lt;0xE8&gt;&#x27;, &#x27;&lt;0x90&gt;&#x27;, &#x27;&lt;0xBC&gt;&#x27;, &#x27;。&#x27;, &#x27;&lt;0xE6&gt;&#x27;, &#x27;&lt;0xB6&gt;&#x27;, &#x27;&lt;0xA7&gt;&#x27;, &#x27;户&#x27;, &#x27;&lt;0xE5&gt;&#x27;, &#x27;&lt;0xAF&gt;&#x27;, &#x27;&lt;0x82&gt;&#x27;, &#x27;无&#x27;, &#x27;人&#x27;, &#x27;，&#x27;, &#x27;&lt;0xE7&gt;&#x27;, &#x27;&lt;0xBA&gt;&#x27;, &#x27;&lt;0xB7&gt;&#x27;, &#x27;&lt;0xE7&gt;&#x27;, &#x27;&lt;0xBA&gt;&#x27;, &#x27;&lt;0xB7&gt;&#x27;, &#x27;开&#x27;, &#x27;&lt;0xE4&gt;&#x27;, &#x27;&lt;0xB8&gt;&#x27;, &#x27;&lt;0x94&gt;&#x27;, &#x27;&lt;0xE8&gt;&#x27;, &#x27;&lt;0x90&gt;&#x27;, &#x27;&lt;0xBD&gt;&#x27;, &#x27;。&#x27;]
Tokenized by Chinese-LLaMA tokenizer:[&#x27;▁&#x27;, &#x27;木&#x27;, &#x27;末&#x27;, &#x27;芙蓉&#x27;, &#x27;花&#x27;, &#x27;，&#x27;, &#x27;山&#x27;, &#x27;中&#x27;, &#x27;发&#x27;, &#x27;红&#x27;, &#x27;萼&#x27;, &#x27;。&#x27;, &#x27;涧&#x27;, &#x27;户&#x27;, &#x27;寂&#x27;, &#x27;无人&#x27;, &#x27;，&#x27;, &#x27;纷纷&#x27;, &#x27;开&#x27;, &#x27;且&#x27;, &#x27;落&#x27;, &#x27;。&#x27;]
```

原版的小分词器分词后的词元数达到了 45，而新的扩充后的分词器只有 22，并且通过上述结果也能看出，扩充后的效果更好。

## 参考资料

[1] [W](https://link.zhihu.com/?target=https%3A//www.tokenex.com/resource-center/what-is-tokenization)hat is Tokenization? Everything You Need to Know | TokenEx

[2] [https:// zhuanlan.zhi](https://zhuanlan.zhihu.com/p/109054674)hu.com/p/10 9054674

[3] [https:// blog.csdn.net/songbinxu /article/details/80209197?depth_1-utm_sour](https://link.zhihu.com/?target=https%3A//blog.csdn.net/songbinxu/article/details/80209197%3Fdepth_1-utm_source%3Ddistribute.pc_relevant.none-task%26utm_source%3Ddistribute.pc_relevant.none-task)ce=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task

[4] [h](https://link.zhihu.com/?target=https%3A//blog.csdn.net/Zh823275484/article/details/87878512)ttps:// blog.csdn.net/Zh8232754 84/article/details/87878512

[5] [https:// github.com/liuhu](https://link.zhihu.com/?target=https%3A//github.com/liuhuanyong/WordSegment)anyong/ WordSegment

[6] [N](https://link.zhihu.com/?target=https%3A//cloud.tencent.com/developer/article/1865689)LP BERT GPT等模型中 tokenizer 类别说明详解 - 云+社区 - 腾讯云 (tencent.com)

[7] [ym](https://link.zhihu.com/?target=https%3A//github.com/ymcui/Chinese-LLaMA-Alpaca)cui/Chinese-LLaMA-Alpaca: 中文LLaMA&Alpaca大语言模型+本地CPU/GPU训练部署 (Chinese LLaMA & Alpaca LLMs) (github.com)

[8] [大模型词表扩充必备工具SentencePi](https://zhuanlan.zhihu.com/p/630696264)ece - 知乎 (zhihu.com)

[9] [【万字长文】LLaMA, Cha](https://link.zhihu.com/?target=https%3A//mp.weixin.qq.com/s/JWuzln2ltd6-5f7QbS2yUw)tGLM, BLOOM的参数高效微调实践

> No matter how hard we try to be mature, we will always be a kid when we all get hurt and cry. ---- Peter and Wendy