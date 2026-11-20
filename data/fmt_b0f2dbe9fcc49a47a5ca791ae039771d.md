
# 分词算法与词表扩充

## 二、Unigram 算法

与`BPE`或者`WordPiece`不同，`Unigram`的算法思想是从一个巨大的词汇表出发，再逐渐删除`trim down`其中的词汇，直到`size`满足预定义。

初始的词汇表可以采用所有预分词器分出来的词，再加上所有高频的子串。

每次从词汇表中删除词汇的原则是使预定义的损失最小。训练时，计算`loss`的公式为：

\begin{equation} Loss=-\sum_{i=1}^{N} \log \left(\sum_{x \in S(x_i)} p(x)\right) \end{equation}

假设训练文档中的所有词分别为 \begin{equation} x_{1} ; x_{2} \ldots x_{N} \end{equation} ，而每个词`tokenize`的方法是一个集合 `S(x_i)` 。当一个词汇表确定时，每个词`tokenize`的方法集合 `S(x_i)` 就是确定的，而每种方法对应着一个概率 `p(x)` 。如果从词汇表中删除部分词，则某些词的`tokenize`的种类集合就会变少， `log(*)` 中的求和项就会减少，从而增加整体`loss`。

`Unigram`算法每次会从词汇表中挑出使得`loss`增长最小的10%~20%的词汇来删除。

一般`Unigram`算法会与`SentencePiece`算法连用。

```
>>> from transformers import AlbertTokenizer 
>>> tokenizer = AlbertTokenizer.from_pretrained('albert-base-v2') 
>>> encoded_input = tokenizer(text, return_tensors='pt')
>>> encoded_input
{'input_ids': tensor([[   2, 3934,   55,   34,  186, 1854,   42,   22,   43,  101,    9,    3]]), 'token_type_ids': tensor([[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]]), 'attention_mask': tensor([[1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1]])}
```

### 2.4 SentencePiece

`SentencePiece`，顾名思义，它是把一个句子看作一个整体，再拆成片段，而没有保留天然的词语的概念。一般地，它把空格`space`也当作一种特殊字符来处理，再用`BPE`或者`Unigram`算法来构造词汇表。

比如，`XLNetTokenizer`就采用了`_`来代替空格，解码的时候会再用空格替换回来。

目前，`Tokenizers`库中，所有使用了`SentencePiece`的都是与`Unigram`算法联合使用的，比如`ALBERT`、`XLNet`、`Marian`和`T5`。

```
>>> from transformers import T5Tokenizer
>>> tokenizer = T5Tokenizer.from_pretrained('t5-base')
>>> text = "Replace me by any text you'd like."
>>> encoded_input = tokenizer(text, return_tensors='pt')
>>> encoded_input
{'input_ids': tensor([[28035,   140,    57,   136,  1499,    25,    31,    26,   114,     5,
             1]]), 'attention_mask': tensor([[1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1]])}
```

## 三、使用 `SentencePiece` 进行词表扩充

### 3.1 `SentencePiece` 的用法

`SentencePiece`分为两部分： 训练模型 和 使用模型 。

[训练模型主要](https://link.zhihu.com/?target=https%3A//github.com/shjwudp/shu/tree/master/books)是根据训练数据集，通过训练获得分词模型及词表，下面以名著 《水浒传》 为数据进行训练

```
spm_train --input=shuihuzhuan.txt --model_prefix=shuihuzhuan-tokenizer --vocab_size=4000 --character_coverage=0.9995 --model_type=bpe
```

参数说明：

+   `--input` ：训练语料文件，可以传递以逗号分隔的文件列表。文件格式为每行一个句子。 无需运行`tokenizer`、`normalizer`或`preprocessor`。 默认情况下，`SentencePiece` 使用 `Unicode NFKC` 规范化输入。

+   `--model_prefix` ：输出模型名称前缀。 训练完成后将生成 `<model_name>.model` 和 `<model_name>.vocab` 文件。

+   `--vocab_size` ：训练后的词表大小，例如：8000、16000 或 32000

+   `--character_coverage` ：模型覆盖的字符数量，对于字符集丰富的语言（如日语或中文）推荐默认值为 0.9995，对于其他字符集较小的语言推荐默认值为 1.0。

+   `--model_type` ：模型类型。 可选值：`unigram`（默认）、`bpe`、`char` 或 `word` 。 使用`word`类型时，必须对输入句子进行`pretokenized`。

训练日志如下：

```
trainer_interface.cc(267) LOG(INFO) Loading corpus: shuihuzhuan.txt
trainer_interface.cc(315) LOG(INFO) Loaded all 5854 sentences
trainer_interface.cc(330) LOG(INFO) Adding meta_piece: <unk>
trainer_interface.cc(330) LOG(INFO) Adding meta_piece: <s>
trainer_interface.cc(330) LOG(INFO) Adding meta_piece: </s>
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

可以查看 `Topk` 频率的 `token` ：

```
> head -n20 shuihuzhuan-tokenizer.vocab
<unk>   0
<s>     0
</s>    0
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
> echo "钱塘江上潮信来，今日方知我是我。" | spm_encode --model=shuihuzhuan-tokenizer.model 
▁ 钱 塘 江 上 潮 信 来 , 今日 方 知 我 是 我 。
```

也可以输出为 `ID` 形式：

```
> echo "钱塘江上潮信来，今日方知我是我。" | spm_encode --model=shuihuzhuan-tokenizer.model  --output_format=id
234 509 2388 251 237 2230 598 228 222 40 374 306 250 235 250 223
```

将模型转化为词表：

```
spm_export_vocab --model=shuihuzhuan-tokenizer.model --output=shuihuzhuan.vocab
```

在 `python` 中加载模型使用：

```
import sentencepiece as spm
sp = spm.SentencePieceProcessor()
text="鞭舞两条龙尾，棍横一串狼牙。三军看得眼睛花，二将纵横交马。使棍的军班领袖，使鞭的将种堪夸。"
sp.Load("shuihuzhuan-tokenizer.model")
print(sp.EncodeAsPieces(text))

['▁', '鞭', '舞', '两', '条', '龙', '尾', ',', '棍', '横', '一', '串', '狼', '牙', '。', '三', '军', '看', '得', '眼', '睛', '花', ',', '二', '将', '纵', '横', '交', '马', '。', '使', '棍', '的', '军', '班', '领', '袖', ',', '使', '鞭', '的', '将', '种', '堪', '夸', '。']
```

以上基本熟悉了 `sentencepiece` 训练及使用词表模型的方法，下面将尝试进行词表扩充。

### 3.2 如何扩充词表

假如我们现在使用 `sentencepiece` 已经通过大量数据训练得到了中文词表模型 `chinese_sp.model` ，现希望将该词表扩充到标准的 `LLaMA` 词表中。

+   首先加载标准 `LLaMA` 分词器，并输出其基本信息：

```
import os
os.environ["PROTOCOL_BUFFERS_PYTHON_IMPLEMENTATION"]="python"
from transformers import LlamaTokenizer

llama_tokenizer = LlamaTokenizer.from_pretrained('./vicuna-13b-v1.3')
print(len(llama_tokenizer))
print(llama_tokenizer.all_special_tokens)
print(llama_tokenizer.all_special_ids)
print(llama_tokenizer.special_tokens_map)
```

输出：

```
32000
['<s>', '</s>', '<unk>', '<unk>']
[1, 2, 0, 0]
{'bos_token': '<s>', 'eos_token': '</s>', 'unk_token': '<unk>', 'pad_token': '<unk>'}
```

+   然后使用 `sentencepiece` 加载训练好的中文词表

```
from sentencepiece import sentencepiece_model_pb2 as sp_pb2_model
import sentencepiece as spm

chinese_sp_model = spm.SentencePieceProcessor()
chinese_sp_model.Load('./chinese_sp.model')
llama_spm = sp_pb2_model.ModelProto()
llama_spm.ParseFromString(llama_tokenizer.sp_model.serialized_model_proto())
chinese_spm = sp_pb2_model.ModelProto()
chinese_spm.ParseFromString(chinese_sp_model.serialized_model_proto())

# print number of tokens
print(len(chinese_sp_model))
# 20000
```

+   将中文词元加入到 `LLaMA` 分词器中
