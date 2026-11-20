
# 数据预处理与文本分类准备

## 数据读取与清洗

```python
import warnings
warnings.filterwarnings("ignore")
RUN_EXAMPLES = True

True_data = pd.read_excel('true.xlsx', header=None, index_col=None)
False_data = pd.read_excel('false.xlsx', header=None, index_col=None)
data = pd.read_excel('电商商品评论.xlsx', header=None, index_col=None)
# 读取原始excel文件

df = data[~data[3].isin(False_data[3])]
# isin函数用于提取和选择指定行列，条件前加~表示isin函数的逆函数，即反向选择
# 剔除data数据中和False_data重复的评论文本,即剔除原始评论中的虚假评论

df["分类标签"] = 0
False_data["分类标签"] = 1
# 设定label，0为真实评论，1为虚假评论，即主要关注的类别

data = pd.concat([df, False_data], axis=0)
# 两张表垂直拼接在一起
data.columns = ['评论人', '日期', '评级', '评论内容', '分类标签']
data.drop(0, axis=0, inplace=True)
# 重新命名dataframe的columns名称，方便查看
data.head()
```

|  | 评论人 | 日期 | 评级 | 评论内容 | 分类标签 |
| --- | --- | --- | --- | --- | --- |
| 1 | winebibber | 已评论商品 · 2018年5月31日 | 5颗星，满分五颗星 | 当时是跟拖地机器人绑定销售，价格还算公道，一起才3k不到； | 0 |
| 2 | winebibber | 已评论商品 · 2018年5月31日 | 5颗星，满分五颗星 | 个人很喜欢的一个系列，读的过程中有时会很纠结，感觉自己也在警察和杀手的意识… | 0 |
| 3 | winebibber | 已评论商品 · 2018年5月31日 | 5颗星，满分五颗星 | 很早就了解过 [蒙台梭利的教育](https://zhida.zhihu.com/search?q=%E8%92%99%E5%8F%B0%E6%A2%AD%E5%88%A9%E7%9A%84%E6%95%99%E8%82%B2&zhida_source=entity&is_preview=1)理论，这套书的印刷及纸质还好，内容也算丰富，参考… | 0 |
| 4 | winebibber | 已评论商品 · 2018年5月31日 | 5颗星，满分五颗星 | 之前正好想给宝贝买一副太阳镜，然后就看到了这个优惠，几十块钱的价格买到这个… | 0 |
| 5 | winebibber | 已评论商品 · 2018年5月31日 | 5颗星，满分五颗星 | 入手时有活动，性价比可说比较高； | 0 |

## 词向量模型加载

```python
# 使用搜狗预训练的词向量模型
embedding = 'embedding_SougouNews.npz'

# 设置随机数种子，保证每次运行结果一致，不至于不能复现模型
np.random.seed(1)
torch.manual_seed(1)
torch.cuda.manual_seed_all(1)
torch.backends.cudnn.deterministic = True
```

下面简单说说预训练的词嵌入模块。词嵌入模块中对每一个词都进行了一个编号，读入词嵌入模型后，能够读出编号所对应的词向量，除此之外，词嵌入中还有两种特殊情况：

为了防止文本中出现词向量中不存在的词，词嵌入中还对这些不知道的词设置了一个编号，可以使用 `UNK` 字符得到相应的编号；为了防止一些词语没有达到规定的最大长度，词嵌入允许对其进行填充，填充的字符为 `PAD` ，可以使用该字符得到编号。

```python
vocab = pkl.load(open('vocab.pkl', 'rb'))  # 词表模型加载
# vocab本质上就是一个文字-数字相对应的词典，例如，可以查询“西”这个词的向量化数字
print(f"vocab['西']：{vocab['西']}")

pad_size = 32
content = list(zip(list(data["分类标签"].astype(str)), list(data["评论内容"])))
print(f"content[:5]：{content[:5]}")
UNK, PAD = '<UNK>', '<PAD>'
# 未知字，padding符号
```

vocab['西']：132
content[:5]：[('0', '当时是跟拖地机器人绑定销售，价格还算公道，一起才3k不到；'), ('0', '个人很喜欢的一个系列，读的过程中有时会很纠结，感觉自己也在警察和杀手的意识…'), ('0', '很早就了解过蒙台梭利的教育理论，这套书的印刷及纸质还好，内容也算丰富，参考…'), ('0', '之前正好想给宝贝买一副太阳镜，然后就看到了这个优惠，几十块钱的价格买到这个…'), ('0', '入手时有活动，性价比可说比较高；')]

## 向量化表示

```python
contents = []  # 初始化输入序列
for line in content:
    label, sentence = "\t".join(line).split("\t")
    # word_id存储每个字的id
    words_id = []
    # 分词器，分成每个字
    token = jieba.lcut(sentence)
    # 字的长度
    seq_len = len(token)
    if pad_size:
        # 如果字长度小于指定长度，则填充，否则截断
        if len(token) < pad_size:
            token.extend([vocab.get(PAD)] * (pad_size - len(token)))
        else:
            token = token[:pad_size]
            seq_len = pad_size
    # 将每个字映射为ID
    for word in token:
        words_id.append(vocab.get(word, vocab.get(UNK)))
    contents.append((words_id, int(label), seq_len))  # 输入序列格式为
print(contents[0])  # 即第一句话的向量化表示
```

([4760, 110, 1427, 4760, 4760, 4760, 4760, 2077, 4760, 546, 1042, 4760, 2077, 4760, 789, 4760, 4760, 3935, 4760, 4760, 4760, 4760, 4760, 4760, 4760, 4760, 4760, 4760, 4760, 4760, 4760], 0, 18)

```python
# 训练集、测试集和验证集比例为6：2：2
train_dev_data, test_data = train_test_split(contents, test_size=0.2, random_state=0)
train_data, dev_data = train_test_split(contents, test_size=0.25, random_state=0)
# 生成数据集迭代器
class DatasetIterater(object):
    def __init__(self, batches, batch_size):
        self.batch_size = batch_size
        self.batches = batches
        self.n_batches = len(batches) // batch_size
        self.residue = False  # 记录batch数量是否为整数
        if len(batches) % self.n_batches != 0:
            self.residue = True
        self.index = 0

    def _to_tensor(self, datas):
        x = torch.LongTensor([_[0] for _ in datas])
        y = torch.LongTensor([_[1] for _ in datas])

        # pad前的长度(超过pad_size的设为pad_size)
        seq_len = torch.LongTensor([_[2] for _ in datas])
        return (x, seq_len), y

    def __next__(self):
        if self.residue and self.index == self.n_batches:
            batches = self.batches[self.index * self.batch_size: len(self.batches)]
            self.index += 1
            batches = self._to_tensor(batches)
            return batches

        elif self.index >= self.n_batches:
            self.index = 0
            raise StopIteration
        else:
            batches = self.batches[self.index * self.batch_size: (self.index + 1) * self.batch_size]
            self.index += 1
            batches = self._to_tensor(batches)
            return batches

    def __iter__(self):
        return self

    def __len__(self):
        if self.residue:
            return self.n_batches + 1
        else:
            return self.n_batches

def build_iterator(dataset, batch_size):
    iter = DatasetIterater(dataset, batch_size)
    return iter

train_iter = build_iterator(train_data, 32)  # batch_size = 32, 一次训练时，喂给模型32条数据
dev_iter = build_iterator(dev_data, 32)
test_iter = build_iterator(test_data, 32)
```

### 3.2 搭建模型

设定模型的一些参数

```python
embedding_pretrained = torch.tensor(np.load("embedding_SougouNews.npz")["embeddings"].astype('float32'))  ## 预训练词向量
n_vocab = len(vocab)  # 词表大小
embed = embedding_pretrained.size(1)  # 词表大小，在运行时赋值
pad_size = 32  # 每句话处理成的长度(短填长切)
learning_rate = 5e-4
dropout = 0.1
num_head = 6
dim_model = 300
hidden = 1024
num_encoder = 2
num_classes = 2
batch_size = 32  # mini-batch大小
num_epochs = 4
class_list = ["0", "1"]  # 分类标签
```

构建模型
