# 实践：给BERT补充其他特征的编码器

> 来源：[https://geekbang.feishu.cn/docx/CUTOdnIgJofmI6xfaaRchE5YnCd](https://geekbang.feishu.cn/docx/CUTOdnIgJofmI6xfaaRchE5YnCd)

> 👨🏻 作者：余俊晖

> 📃 作者介绍：一名NLP炼丹师，NLP算法竞赛TOP选手

> 🛖 公众号：大模型自然语言处理

> ⏰ 收录时间：2024-05-28 17:09:42

> 🔗 原文链接：h[ttps://mp.weixin.q](https://mp.weixin.qq.com/s?__biz=Mzg4NjI0NDg0Ng==&mid=2247483974&idx=1&sn=52990b70e5d6f5b5041bee21f33d077e&chksm=cf9dd6c7f8ea5fd11342b713ff6abd91389b41dcd0cded07b1a216803d0b9bdcd62f91ca9244#rd)q.com/s?__biz=Mz...

![](img/9fec404ad1e689053b72541965d599b2.png)

关注作者公众号，获取更多AI知识

## **前言 **

特征工程是传统的机器学习基石，然而，随着BERT等预训练语言模型的发展，文本语义表示得到了极大的改善，本文将实践一种多特征编码器。

## ** ****1、POS特征生成 **

### ** 代码： **

```
import spacy
nlp = spacy.load(&quot;en_core_web_sm&quot;)

def spacy_pos_tag(sentence):
    doc = nlp(sentence)
    print(list(doc.sents))
    pos = []
    for word in list(doc.sents)[0]:
        pos.append(word.tag_)
    return pos

if __name__ == &#x27;__main__&#x27;:
    print(spacy_pos_tag(&#x27;Iraq Blames Market Blast on Coalition .&#x27;))

```

### ** 输出： **

```
[Iraq Blames Market Blast on Coalition .]
[&#x27;NNP&#x27;, &#x27;NNP&#x27;, &#x27;NNP&#x27;, &#x27;NNP&#x27;, &#x27;IN&#x27;, &#x27;NNP&#x27;, &#x27;.&#x27;]
```

## ** ****2、NLP中常见的特征 **

+   Word特征

+   Token特征

+   Char特征

+   POS特征

## ** ****3、数据处理 **

本文与ACE2005数据集为例，处理该数据的格式样例如下：

```
[
    {
        &quot;tokens&quot;: [&quot;Their&quot;, &quot;military&quot;, &quot;service&quot;, &quot;goes&quot;, &quot;back&quot;, &quot;to&quot;, &quot;the&quot;, &quot;Vietnam&quot;, &quot;era&quot;, &quot;.&quot;],
        &quot;pos&quot;: [&quot;PRP$&quot;, &quot;JJ&quot;, &quot;NN&quot;, &quot;VBZ&quot;, &quot;RB&quot;, &quot;TO&quot;, &quot;DT&quot;, &quot;NNP&quot;, &quot;NN&quot;, &quot;.&quot;],
        &quot;entities&quot;: [
            {&quot;type&quot;: &quot;PER&quot;, &quot;start&quot;: 0, &quot;end&quot;: 1},
            {&quot;type&quot;: &quot;ORG&quot;, &quot;start&quot;: 1, &quot;end&quot;: 2},
            {&quot;type&quot;: &quot;GPE&quot;, &quot;start&quot;: 7, &quot;end&quot;: 8}
        ],
        &quot;ltokens&quot;: [&quot;WOODRUFF&quot;, &quot;We&quot;, &quot;know&quot;, &quot;that&quot;, &quot;some&quot;, &quot;of&quot;, &quot;the&quot;, &quot;American&quot;, &quot;troops&quot;, &quot;now&quot;, &quot;fighting&quot;,
                    &quot;in&quot;, &quot;Iraq&quot;, &quot;are&quot;, &quot;longtime&quot;, &quot;veterans&quot;, &quot;of&quot;, &quot;warfare&quot;, &quot;,&quot;, &quot;probably&quot;, &quot;not&quot;, &quot;most&quot;, &quot;,&quot;,
                    &quot;but&quot;,
                    &quot;some&quot;, &quot;.&quot;],
        &quot;rtokens&quot;: [&quot;Others&quot;, &quot;,&quot;, &quot;though&quot;, &quot;,&quot;, &quot;are&quot;, &quot;novices&quot;, &quot;.&quot;]
    },
    ...
]

```

词汇表样例：

```
{
  &quot;pos&quot;: [
    &quot;FW&quot;,
    &quot;VBZ&quot;,
    &quot;PRP&quot;,
    &quot;DT&quot;,
    &quot;VBD&quot;,
    &quot;IN&quot;,
    &quot;NNS&quot;,
    &quot;.&quot;,
    &quot;CD&quot;,
    &quot;``&quot;,
    &quot;WDT&quot;,
    &quot;&#x27;&#x27;&quot;,
    &quot;POS&quot;,
    &quot;VBN&quot;,
    &quot;WP&quot;,
    &quot;RBS&quot;,
    &quot;NNPS&quot;,
    &quot;JJS&quot;,
    &quot;RB&quot;,
    &quot;:&quot;,
    &quot;JJR&quot;,
    &quot;PRP$&quot;,
    &quot;WP$&quot;,
    &quot;TO&quot;,
    &quot;NN&quot;,
    &quot;WRB&quot;,
    &quot;VB&quot;,
    &quot;-RRB-&quot;,
    &quot;JJ&quot;,
    &quot;MD&quot;,
    &quot;$&quot;,
    &quot;-LRB-&quot;,
    &quot;VBP&quot;,
    &quot;RP&quot;,
    &quot;RBR&quot;,
    &quot;LS&quot;,
    &quot;EX&quot;,
    &quot;NNP&quot;,
    &quot;CC&quot;,
    &quot;#&quot;,
    &quot;,&quot;,
    &quot;SYM&quot;,
    &quot;PDT&quot;,
    &quot;UH&quot;,
    &quot;VBG&quot;
  ],
  &quot;char&quot;: [
    &quot;o&quot;,
    &quot;8&quot;,
    &quot;/&quot;,
    &quot;&gt;&quot;,
    &quot;^&quot;,
    &quot;@&quot;,
    &quot;D&quot;,
    &quot;Y&quot;,
    &quot;?&quot;,
    &quot;c&quot;,
    &quot;.&quot;,
    &quot;Q&quot;,
    &quot;R&quot;,
    &quot;0&quot;,
    &quot;e&quot;,
    &quot;W&quot;,
    &quot;5&quot;,
    &quot;X&quot;,
    &quot;p&quot;,
    &quot;r&quot;,
    &quot;M&quot;,
    &quot;C&quot;,
    &quot;|&quot;,
    &quot;i&quot;,
    &quot;Z&quot;,
    &quot;l&quot;,
    &quot;9&quot;,
    &quot;j&quot;,
    &quot;4&quot;,
    &quot;$&quot;,
    &quot;%&quot;,
    &quot;y&quot;,
    &quot;P&quot;,
    &quot;s&quot;,
    &quot;6&quot;,
    &quot;G&quot;,
    &quot;!&quot;,
    &quot;m&quot;,
    &quot;S&quot;,
    &quot;I&quot;,
    &quot;*&quot;,
    &quot;h&quot;,
    &quot;,&quot;,
    &quot;k&quot;,
    &quot;_&quot;,
    &quot;a&quot;,
    &quot;B&quot;,
    &quot;v&quot;,
    &quot;d&quot;,
    &quot;N&quot;,
    &quot;1&quot;,
    &quot;7&quot;,
    &quot;-&quot;,
    &quot;q&quot;,
    &quot;&#x27;&quot;,
    &quot;2&quot;,
    &quot;w&quot;,
    &quot;:&quot;,
    &quot;&lt;&quot;,
    &quot;3&quot;,
    &quot;;&quot;,
    &quot;t&quot;,
    &quot;u&quot;,
    &quot;+&quot;,
    &quot;J&quot;,
    &quot;&amp;&quot;,
    &quot;`&quot;,
    &quot;F&quot;,
    &quot;=&quot;,
    &quot;V&quot;,
    &quot;f&quot;,
    &quot;L&quot;,
    &quot;U&quot;,
    &quot;T&quot;,
    &quot;n&quot;,
    &quot;x&quot;,
    &quot;b&quot;,
    &quot;K&quot;,
    &quot;#&quot;,
    &quot;E&quot;,
    &quot;g&quot;,
    &quot;H&quot;,
    &quot;O&quot;,
    &quot;z&quot;,
    &quot;A&quot;
  ],
  &quot;type&quot;: [
    &quot;FAC&quot;,
    &quot;VEH&quot;,
    &quot;ORG&quot;,
    &quot;GPE&quot;,
    &quot;PER&quot;,
    &quot;WEA&quot;,
    &quot;LOC&quot;
  ]
}
```

## ** ****4、编码器代码实现 **

### ** 4.1.数据集构建代码 **

**1、self_data.py**

```
#!/usr/bin/env python
# _*_coding:utf-8_*_
# Author   :    Junhui Yu
# Time     :    2023/2/28 16:54

import json
import logging
from typing import List, TextIO, Union
from flair.data import Corpus, FlairDataset, Sentence
from torch.utils.data import Dataset
from torch.utils.data.dataset import Subset
from tqdm import tqdm

logger = logging.getLogger(&quot;YUNLP&quot;)


class MySentence(Sentence):
    def __init__(self, text: Union[str, List[str]], entities: list[dict] = None, *args, **kargs):
        super(MySentence, self).__init__(text, *args, **kargs)
        self.entities = entities
        self._nested_pairs = None

    @property
    def sentence_tokens(self):
        return list(map(lambda a: a.text, self.tokens))

    @property
    def previous_tokens(self):
        return list(map(lambda a: a.text, self.previous_sentence().tokens))

    @property
    def next_tokens(self):
        return list(map(lambda a: a.text, self.next_sentence().tokens))


class MyDataset(FlairDataset):
    def __init__(self, file: TextIO, name: str):
        self.name = name
        data_list = json.load(file)
        self._sentences = self.format(data_list)

    def format(self, data_list: list[dict]) -&gt; list[MySentence]:
        sentences = list()
        for data in tqdm(data_list):
            sentence = MySentence(data[&#x27;tokens&#x27;])
            sentence._previous_sentence = MySentence(data[&#x27;ltokens&#x27;])
            sentence._next_sentence = MySentence(data[&#x27;rtokens&#x27;])
            sentence.add_label(&#x27;pos&#x27;, data[&#x27;pos&#x27;])
            if &#x27;tags&#x27; in data.keys():
                sentence.add_label(&#x27;tags&#x27;, data[&#x27;tags&#x27;])
            sentence.entities = data[&#x27;entities&#x27;]
            sentences.append(sentence)

        return sentences

    def add_data(self, sentences: list[Sentence]):
        self._sentences += sentences

    @property
    def sentences(self):
        return self._sentences

    def __getitem__(self, index: int) -&gt; MySentence:
        return self._sentences[index]

    def __len__(self) -&gt; int:
        return len(self._sentences)

    def is_in_memory(self) -&gt; bool:
        return True


class MyCorpus(Corpus):
    def __init__(self, dataset_name: str, concat: bool = False, max_length: int = None):
        dataset_path = f&quot;data/{dataset_name}&quot;
        try:
            super(MyCorpus, self).__init__(name=dataset_name)
        except RuntimeError:
            pass

        self._train: MyDataset = self.create_dataset(dataset_path, &#x27;train&#x27;)
        self._dev: MyDataset = self.create_dataset(dataset_path, &#x27;dev&#x27;)
        self._test: MyDataset = self.create_dataset(dataset_path, &#x27;test&#x27;)

        with open(f&quot;{dataset_path}/vocab.json&quot;, encoding=&#x27;utf-8&#x27;) as file:
            metadata = json.load(file)
        types = metadata.pop(&#x27;type&#x27;)
        types_idx = range(len(types))
        self.metadata = {
            &#x27;types2idx&#x27;: dict(zip(types, types_idx)),
            &#x27;idx2types&#x27;: dict(zip(types_idx, types)),
            &#x27;chars_list&#x27;: metadata[&#x27;char&#x27;],
            &#x27;pos_list&#x27;: metadata[&#x27;pos&#x27;]
        }

        if concat:
            self._train.add_data(self._dev.sentences)
            self._dev = None

        if max_length is not None:
            self.filter(lambda a: len(a) &lt; max_length, [&#x27;train&#x27;])

    @staticmethod
    def create_dataset(dataset_path: str, prefix: str):
        try:
            with open(f&quot;{dataset_path}/{prefix}.json&quot;, encoding=&quot;utf-8&quot;) as file:
                dataset = MyDataset(file, prefix)
        except FileNotFoundError as _:
            return None
        return dataset

    def filter(self, condition: callable, dataset_names: list[str] = None):
        if dataset_names is None:
            dataset_names = [&#x27;train&#x27;]
        for name in dataset_names:
            dataset = self.__dict__[f&#x27;_{name}&#x27;]
            self.__dict__[f&#x27;_{name}&#x27;] = self._filter(dataset, condition)
        logger.info(self)

    @staticmethod
    def _filter(dataset: MyDataset, condition: callable) -&gt; Dataset:
        empty_sentence_indices = []
        non_empty_sentence_indices = []
        index = 0

        for sentence in dataset:
            if condition(sentence):
                non_empty_sentence_indices.append(index)
            else:
                empty_sentence_indices.append(index)
            index += 1
        subset = Subset(dataset, non_empty_sentence_indices)
        return subset

    @property
    def datasets(self) -&gt; list[MyDataset]:
        datasets = map(lambda a: self.__dict__[f&#x27;_{a}&#x27;], [&#x27;train&#x27;, &#x27;dev&#x27;, &#x27;test&#x27;])
        return list(filter(lambda a: a is not None, datasets))

    @property
    def train(self) -&gt; MyDataset:
        return self._train

    @property
    def dev(self) -&gt; MyDataset:
        return self._dev

    @property
    def test(self) -&gt; MyDataset:
        return self._test

```

### ** 4.2、编码器模型代码 **

```
#!/usr/bin/env python
# _*_coding:utf-8_*_
# Author   :    Junhui Yu
# Time     :    2023/2/28 13:51

from typing import Optional
import logging
import torch
import transformers
from torch import nn, Tensor
from torch.nn.utils.rnn import pack_padded_sequence, pad_packed_sequence, pad_sequence
from torchtext import vocab
from typing import Union
import config
from self_data import MySentence

from transformers import AutoTokenizer
from transformers import PreTrainedTokenizerFast

logger = logging.getLogger(&quot;YUNLP&quot;)


def build_tokenizer(pretrain_model: str, cache_dir: str):
    return Tokenizer({
        &#x27;return_tensors&#x27;: &#x27;pt&#x27;, &#x27;padding&#x27;: True
    },
        AutoTokenizer.from_pretrained(pretrain_model, cache_dir=cache_dir, padding_side=&#x27;right&#x27;)
    )


class Tokenizer:
    xila_table = str.maketrans(
        &quot;âêîôûŷäëïöüÿñ&quot;,
        &quot;aeiouyaeiouyn&quot;
    )
    special_tokens = {
        &#x27;“&#x27;: &#x27;&quot;&#x27;,
        &#x27;”&#x27;: &#x27;&quot;&#x27;,
        &#x27;‘&#x27;: &#x27;&#x27;&#x27;,
        &#x27;’&#x27;: &#x27;&#x27;&#x27;,
        &#x27;—&#x27;: &#x27;-&#x27;,
        &#x27;…&#x27;: &#x27;...&#x27;,
        &#x27;……&#x27;: &#x27;...&#x27;,
        &#x27;�&#x27;: &#x27;?&#x27;
    }

    def __init__(self, encode_kargs: dict, tokenizer: PreTrainedTokenizerFast = None):
        self.encode_kargs = encode_kargs
        self.tokenizer = tokenizer

    def __call__(self, batch_texts: Union[list[str], list[list[str]]]):
        batch_texts = list(map(self.preprocess, batch_texts))
        return self.tokenizer(batch_texts, is_split_into_words=True, **self.encode_kargs)

    def batch_decode(self, *args, **kargs):
        batch_texts = self.tokenizer.batch_decode(*args, **kargs)
        return [text.split(&quot; &quot;) for text in batch_texts]

    @classmethod
    def preprocess(cls, raw_text: list[str]):
        text = list(map(lambda a: a.translate(cls.xila_table), raw_text))
        text = list(map(lambda a: cls.special_tokens.get(a, a), text))
        return text


class Encoder(nn.Module):
    def __init__(self, embedding_kargs: dict):
        super(Encoder, self).__init__()
        embedding_kargs.update({&#x27;cache_dir&#x27;: f&quot;./vec/&quot;})
        self.embedding = FusionEmbedding(**embedding_kargs)
        self.hidden_size = self.embedding.token2vec.pretrain.config.hidden_size
        embedding_length = self.embedding.embedding_length

        self.out_rnn = nn.GRU(embedding_length, embedding_length, bidirectional=True, batch_first=True)
        self.transforms = nn.Sequential(
            nn.Linear(2 * embedding_length, self.hidden_size),
            nn.LayerNorm(self.hidden_size),
        )

    def forward(self, batch_sentences: list[MySentence]) -&gt; tuple[Tensor, Tensor]:
        lengths = torch.as_tensor(list(map(len, batch_sentences)))

        batch_embeds, batch_masks = self.embedding(batch_sentences)
        batch_embeds = pack_padded_sequence(batch_embeds, lengths, batch_first=True, enforce_sorted=False)
        batch_embeds = pad_packed_sequence(self.out_rnn(batch_embeds)[0], batch_first=True)[0]

        batch_hiddens = self.transforms(batch_embeds)
        return batch_hiddens, batch_masks


class FusionEmbedding(nn.Module):
    def __init__(
            self,
            cache_dir: str,
            model_max_length: int,
            pretrain_model: str,
            pos_dim: Optional[int],
            char_dim: Optional[int],
            word2vec: Optional[str],
            chars_list: Optional[list[str]],
            pos_list: Optional[list[str]],
    ):
        super(FusionEmbedding, self).__init__()
        self.token2vec = Token2Vec(cache_dir, pretrain_model, model_max_length)
        self._embedding_length = self.token2vec.hidden_size

        if word2vec is not None:
            self.word2vec = Word2Vec(cache_dir, word2vec)
            self._embedding_length += self.word2vec.word_dim

        if char_dim is not None and chars_list is not None:
            self.char2vec = Char2Vec(chars_list, char_dim)
            self._embedding_length += char_dim * 2

        if pos_dim is not None and pos_list is not None:
            self.pos2vec = Pos2Vec(pos_list, pos_dim)
            self._embedding_length += pos_dim

    def forward(self, batch_sentences: list[MySentence]) -&gt; tuple[Tensor, Tensor]:
        token2vec, mask = self.token2vec(batch_sentences)
        embeds = [token2vec]

        if hasattr(self, &#x27;word2vec&#x27;):
            embeds.append(self.word2vec(batch_sentences, token2vec.device))

        if hasattr(self, &#x27;char2vec&#x27;):
            embeds.append(self.char2vec(batch_sentences))

        if hasattr(self, &#x27;pos2vec&#x27;):
            embeds.append(self.pos2vec(batch_sentences))

        batch_embeds = torch.cat(embeds, dim=-1)
        return batch_embeds, mask

    @property
    def embedding_length(self):
        return self._embedding_length


class Token2Vec(nn.Module):
    def __init__(self, cache_dir: str, pretrain_model: str, model_max_length: int):
        super(Token2Vec, self).__init__()
        self.model_max_length = model_max_length
        self.model_max_length_copy = model_max_length

        cache_dir = cache_dir + pretrain_model
        self.tokenizer = build_tokenizer(pretrain_model, cache_dir)
        self.pretrain = transformers.AutoModel.from_pretrained(pretrain_model, cache_dir=cache_dir)

    def forward(self, batch_sentences: list[MySentence]) -&gt; tuple[Tensor, Tensor]:
        batch_context = self.span_context(batch_sentences)
        lengths = list(map(len, batch_context))

        encoding = self.tokenizer(batch_context).to(next(self.parameters()).device)
        output = self.pretrain(output_hidden_states=True, **encoding)
        hidden_states = torch.stack(output.hidden_states[-4:], dim=-1)
        hidden_state = torch.mean(hidden_states, dim=-1)

        token_embeds, sub_lengths = list(), list()
        for i, length in enumerate(lengths):
            for j in range(length):
                s, e = encoding.word_to_tokens(i, j)
                token_embeds.append(hidden_state[i, s:e])
                sub_lengths.append(e - s)

        sub_lengths = torch.as_tensor(sub_lengths, device=hidden_state.device)
        token_embeds = pad_sequence(token_embeds, padding_value=0)
        token_embeds = torch.sum(token_embeds, dim=0) / sub_lengths.unsqueeze(-1)

        token_embeds = token_embeds.split(lengths, dim=0)
        token_embeds = pad_sequence(token_embeds, batch_first=True)
        return self.span_select(token_embeds, batch_sentences)

    def span_context(self, batch_sentences: list[MySentence]) -&gt; list[list[str]]:
        batch_context = list()
        for sentence in batch_sentences:
            context = sentence.sentence_tokens
            if len(context) + len(sentence.next_sentence()) &lt; self.model_max_length:
                context = context + sentence.next_tokens

                if len(sentence.previous_sentence()) + len(context) &lt; self.model_max_length:
                    context = sentence.previous_tokens + context
                    offset = len(sentence.previous_sentence())
                    sentence.start_pos, sentence.end_pos = offset, offset + len(sentence)
                else:
                    offset = self.model_max_length - len(context)
                    context = sentence.previous_tokens[-offset:] + context
                    sentence.start_pos, sentence.end_pos = offset, offset + len(sentence)
            else:
                sentence.start_pos, sentence.end_pos = 0, len(sentence)

            batch_context.append(context)
        return batch_context

    @staticmethod
    def span_select(batch_embeds: Tensor, batch_sentences: list[MySentence]) -&gt; tuple[Tensor, Tensor]:
        hiddens, mask = list(), list()
        for sentence, embeds in zip(batch_sentences, batch_embeds):
            s, e = sentence.start_pos, sentence.end_pos
            hiddens.append(embeds[s:e])
            mask.append(torch.ones(e - s, device=embeds.device, dtype=torch.bool))
        hiddens = pad_sequence(hiddens, batch_first=True)
        mask = pad_sequence(mask, padding_value=False, batch_first=True)
        return hiddens, mask

    @property
    def hidden_size(self):
        return self.pretrain.config.hidden_size

    def train(self, mode: bool = True):
        self.model_max_length = self.model_max_length_copy
        super().train(mode)

    def eval(self):
        self.model_max_length = 512
        super().eval()


class Word2Vec(nn.Module):
    def __init__(self, cache_dir: str, word2vec: str):
        super().__init__()
        self.word2vec = word2vec
        if word2vec == &#x27;glove&#x27;:
            self._word_dim = 50
            self.vectors = vocab.GloVe(name=&#x27;6B&#x27;, dim=50, cache=cache_dir + &quot;glove&quot;)

    def forward(self, batch_sentences: list[MySentence], device: str) -&gt; Tensor:
        embeds = list(map(self.get_vectors_by_tokens, batch_sentences))
        return pad_sequence(embeds, batch_first=True).to(device)

    def get_vectors_by_tokens(self, sentence: MySentence):
        if self.word2vec in [&#x27;glove&#x27;, &#x27;chinese&#x27;]:
            return self.vectors.get_vecs_by_tokens(sentence.sentence_tokens)
        elif self.word2vec == &#x27;bio&#x27;:
            indices = [self.word2idx.get(token, 0) for token in sentence.sentence_tokens]
            return self.embeds(torch.as_tensor(indices, device=self.embeds.weight.device))

    @property
    def word_dim(self):
        return self._word_dim


class Char2Vec(nn.Module):
    def __init__(self, chars_list: list[str], char_dim: int):
        super().__init__()
        self.char2idx = dict(zip(chars_list, range(len(chars_list))))
        self.char2vec = nn.Embedding(len(self.char2idx), char_dim)
        self.char_rnn = nn.GRU(char_dim, char_dim, bidirectional=True, batch_first=True)

    def forward(self, batch_sentences: list[MySentence]) -&gt; Tensor:
        device = self.char2vec.weight.device
        lengths = list(map(len, batch_sentences))

        indices, char_lengths = list(), list()
        for sentence in batch_sentences:
            for token in sentence:
                char_text = token.text
                indices.append(torch.as_tensor([self.char2idx[c] for c in char_text], device=device))
                char_lengths.append(len(char_text))

        char_embeds = self.char2vec(pad_sequence(indices))
        char_lengths = torch.as_tensor(char_lengths, device=char_embeds.device)

        char_embeds = pack_padded_sequence(char_embeds, char_lengths.cpu(), enforce_sorted=False)
        char_embeds = pad_packed_sequence(self.char_rnn(char_embeds)[0], padding_value=0)[0]
        char_embeds = torch.sum(char_embeds, dim=0) / char_lengths.unsqueeze(-1)

        char_embeds = torch.split(char_embeds, lengths, dim=0)
        return pad_sequence(char_embeds, batch_first=True)


class Pos2Vec(nn.Module):
    def __init__(self, pos_list: list[str], pos_dim: int):
        super().__init__()
        self.pos2idx = dict(zip(pos_list, range(len(pos_list))))
        self.pos2vec = nn.Embedding(len(self.pos2idx), pos_dim)

    def forward(self, batch_sentences: list[MySentence]) -&gt; tuple[Tensor, Tensor]:
        device = self.pos2vec.weight.device
        indices = list()
        for sentence in batch_sentences:
            pos_tags = sentence.get_labels(&#x27;pos&#x27;)[0].value
            pos_indices = list(map(lambda a: self.pos2idx[a], pos_tags))
            indices.append(torch.as_tensor(pos_indices, dtype=torch.long, device=device))
        return self.pos2vec(pad_sequence(indices).T)

```

## ** ****5、模型结构与测试 **

1.  编码器模型结构

```
Encoder(
  (embedding): FusionEmbedding(
    (token2vec): Token2Vec(
      (pretrain): BertModel(
        (embeddings): BertEmbeddings(
          (word_embeddings): Embedding(30522, 768, padding_idx=0)
          (position_embeddings): Embedding(512, 768)
          (token_type_embeddings): Embedding(2, 768)
          (LayerNorm): LayerNorm((768,), eps=1e-12, elementwise_affine=True)
          (dropout): Dropout(p=0.1, inplace=False)
        )
        (encoder): BertEncoder(
          (layer): ModuleList(
            (0): BertLayer(
              (attention): BertAttention(
                (self): BertSelfAttention(
                  (query): Linear(in_features=768, out_features=768, bias=True)
                  (key): Linear(in_features=768, out_features=768, bias=True)
                  (value): Linear(in_features=768, out_features=768, bias=True)
                  (dropout): Dropout(p=0.1, inplace=False)
                )
                (output): BertSelfOutput(
                  (dense): Linear(in_features=768, out_features=768, bias=True)
                  (LayerNorm): LayerNorm((768,), eps=1e-12, elementwise_affine=True)
                  (dropout): Dropout(p=0.1, inplace=False)
                )
              )
              (intermediate): BertIntermediate(
                (dense): Linear(in_features=768, out_features=3072, bias=True)
                (intermediate_act_fn): GELUActivation()
              )
              (output): BertOutput(
                (dense): Linear(in_features=3072, out_features=768, bias=True)
                (LayerNorm): LayerNorm((768,), eps=1e-12, elementwise_affine=True)
                (dropout): Dropout(p=0.1, inplace=False)
              )
            )
            (1): BertLayer(
              (attention): BertAttention(
                (self): BertSelfAttention(
                  (query): Linear(in_features=768, out_features=768, bias=True)
                  (key): Linear(in_features=768, out_features=768, bias=True)
                  (value): Linear(in_features=768, out_features=768, bias=True)
                  (dropout): Dropout(p=0.1, inplace=False)
                )
                (output): BertSelfOutput(
                  (dense): Linear(in_features=768, out_features=768, bias=True)
                  (LayerNorm): LayerNorm((768,), eps=1e-12, elementwise_affine=True)
                  (dropout): Dropout(p=0.1, inplace=False)
                )
              )
              (intermediate): BertIntermediate(
                (dense): Linear(in_features=768, out_features=3072, bias=True)
                (intermediate_act_fn): GELUActivation()
              )
              (output): BertOutput(
                (dense): Linear(in_features=3072, out_features=768, bias=True)
                (LayerNorm): LayerNorm((768,), eps=1e-12, elementwise_affine=True)
                (dropout): Dropout(p=0.1, inplace=False)
              )
            )
            (2): BertLayer(
              (attention): BertAttention(
                (self): BertSelfAttention(
                  (query): Linear(in_features=768, out_features=768, bias=True)
                  (key): Linear(in_features=768, out_features=768, bias=True)
                  (value): Linear(in_features=768, out_features=768, bias=True)
                  (dropout): Dropout(p=0.1, inplace=False)
                )
                (output): BertSelfOutput(
                  (dense): Linear(in_features=768, out_features=768, bias=True)
                  (LayerNorm): LayerNorm((768,), eps=1e-12, elementwise_affine=True)
                  (dropout): Dropout(p=0.1, inplace=False)
                )
              )
              (intermediate): BertIntermediate(
                (dense): Linear(in_features=768, out_features=3072, bias=True)
                (intermediate_act_fn): GELUActivation()
              )
              (output): BertOutput(
                (dense): Linear(in_features=3072, out_features=768, bias=True)
                (LayerNorm): LayerNorm((768,), eps=1e-12, elementwise_affine=True)
                (dropout): Dropout(p=0.1, inplace=False)
              )
            )
            (3): BertLayer(
              (attention): BertAttention(
                (self): BertSelfAttention(
                  (query): Linear(in_features=768, out_features=768, bias=True)
                  (key): Linear(in_features=768, out_features=768, bias=True)
                  (value): Linear(in_features=768, out_features=768, bias=True)
                  (dropout): Dropout(p=0.1, inplace=False)
                )
                (output): BertSelfOutput(
                  (dense): Linear(in_features=768, out_features=768, bias=True)
                  (LayerNorm): LayerNorm((768,), eps=1e-12, elementwise_affine=True)
                  (dropout): Dropout(p=0.1, inplace=False)
                )
              )
              (intermediate): BertIntermediate(
                (dense): Linear(in_features=768, out_features=3072, bias=True)
                (intermediate_act_fn): GELUActivation()
              )
              (output): BertOutput(
                (dense): Linear(in_features=3072, out_features=768, bias=True)
                (LayerNorm): LayerNorm((768,), eps=1e-12, elementwise_affine=True)
                (dropout): Dropout(p=0.1, inplace=False)
              )
            )
            (4): BertLayer(
              (attention): BertAttention(
                (self): BertSelfAttention(
                  (query): Linear(in_features=768, out_features=768, bias=True)
                  (key): Linear(in_features=768, out_features=768, bias=True)
                  (value): Linear(in_features=768, out_features=768, bias=True)
                  (dropout): Dropout(p=0.1, inplace=False)
                )
                (output): BertSelfOutput(
                  (dense): Linear(in_features=768, out_features=768, bias=True)
                  (LayerNorm): LayerNorm((768,), eps=1e-12, elementwise_affine=True)
                  (dropout): Dropout(p=0.1, inplace=False)
                )
              )
              (intermediate): BertIntermediate(
                (dense): Linear(in_features=768, out_features=3072, bias=True)
                (intermediate_act_fn): GELUActivation()
              )
              (output): BertOutput(
                (dense): Linear(in_features=3072, out_features=768, bias=True)
                (LayerNorm): LayerNorm((768,), eps=1e-12, elementwise_affine=True)
                (dropout): Dropout(p=0.1, inplace=False)
              )
            )
            (5): BertLayer(
              (attention): BertAttention(
                (self): BertSelfAttention(
                  (query): Linear(in_features=768, out_features=768, bias=True)
                  (key): Linear(in_features=768, out_features=768, bias=True)
                  (value): Linear(in_features=768, out_features=768, bias=True)
                  (dropout): Dropout(p=0.1, inplace=False)
                )
                (output): BertSelfOutput(
                  (dense): Linear(in_features=768, out_features=768, bias=True)
                  (LayerNorm): LayerNorm((768,), eps=1e-12, elementwise_affine=True)
                  (dropout): Dropout(p=0.1, inplace=False)
                )
              )
              (intermediate): BertIntermediate(
                (dense): Linear(in_features=768, out_features=3072, bias=True)
                (intermediate_act_fn): GELUActivation()
              )
              (output): BertOutput(
                (dense): Linear(in_features=3072, out_features=768, bias=True)
                (LayerNorm): LayerNorm((768,), eps=1e-12, elementwise_affine=True)
                (dropout): Dropout(p=0.1, inplace=False)
              )
            )
            (6): BertLayer(
              (attention): BertAttention(
                (self): BertSelfAttention(
                  (query): Linear(in_features=768, out_features=768, bias=True)
                  (key): Linear(in_features=768, out_features=768, bias=True)
                  (value): Linear(in_features=768, out_features=768, bias=True)
                  (dropout): Dropout(p=0.1, inplace=False)
                )
                (output): BertSelfOutput(
                  (dense): Linear(in_features=768, out_features=768, bias=True)
                  (LayerNorm): LayerNorm((768,), eps=1e-12, elementwise_affine=True)
                  (dropout): Dropout(p=0.1, inplace=False)
                )
              )
              (intermediate): BertIntermediate(
                (dense): Linear(in_features=768, out_features=3072, bias=True)
                (intermediate_act_fn): GELUActivation()
              )
              (output): BertOutput(
                (dense): Linear(in_features=3072, out_features=768, bias=True)
                (LayerNorm): LayerNorm((768,), eps=1e-12, elementwise_affine=True)
                (dropout): Dropout(p=0.1, inplace=False)
              )
            )
            (7): BertLayer(
              (attention): BertAttention(
                (self): BertSelfAttention(
                  (query): Linear(in_features=768, out_features=768, bias=True)
                  (key): Linear(in_features=768, out_features=768, bias=True)
                  (value): Linear(in_features=768, out_features=768, bias=True)
                  (dropout): Dropout(p=0.1, inplace=False)
                )
                (output): BertSelfOutput(
                  (dense): Linear(in_features=768, out_features=768, bias=True)
                  (LayerNorm): LayerNorm((768,), eps=1e-12, elementwise_affine=True)
                  (dropout): Dropout(p=0.1, inplace=False)
                )
              )
              (intermediate): BertIntermediate(
                (dense): Linear(in_features=768, out_features=3072, bias=True)
                (intermediate_act_fn): GELUActivation()
              )
              (output): BertOutput(
                (dense): Linear(in_features=3072, out_features=768, bias=True)
                (LayerNorm): LayerNorm((768,), eps=1e-12, elementwise_affine=True)
                (dropout): Dropout(p=0.1, inplace=False)
              )
            )
            (8): BertLayer(
              (attention): BertAttention(
                (self): BertSelfAttention(
                  (query): Linear(in_features=768, out_features=768, bias=True)
                  (key): Linear(in_features=768, out_features=768, bias=True)
                  (value): Linear(in_features=768, out_features=768, bias=True)
                  (dropout): Dropout(p=0.1, inplace=False)
                )
                (output): BertSelfOutput(
                  (dense): Linear(in_features=768, out_features=768, bias=True)
                  (LayerNorm): LayerNorm((768,), eps=1e-12, elementwise_affine=True)
                  (dropout): Dropout(p=0.1, inplace=False)
                )
              )
              (intermediate): BertIntermediate(
                (dense): Linear(in_features=768, out_features=3072, bias=True)
                (intermediate_act_fn): GELUActivation()
              )
              (output): BertOutput(
                (dense): Linear(in_features=3072, out_features=768, bias=True)
                (LayerNorm): LayerNorm((768,), eps=1e-12, elementwise_affine=True)
                (dropout): Dropout(p=0.1, inplace=False)
              )
            )
            (9): BertLayer(
              (attention): BertAttention(
                (self): BertSelfAttention(
                  (query): Linear(in_features=768, out_features=768, bias=True)
                  (key): Linear(in_features=768, out_features=768, bias=True)
                  (value): Linear(in_features=768, out_features=768, bias=True)
                  (dropout): Dropout(p=0.1, inplace=False)
                )
                (output): BertSelfOutput(
                  (dense): Linear(in_features=768, out_features=768, bias=True)
                  (LayerNorm): LayerNorm((768,), eps=1e-12, elementwise_affine=True)
                  (dropout): Dropout(p=0.1, inplace=False)
                )
              )
              (intermediate): BertIntermediate(
                (dense): Linear(in_features=768, out_features=3072, bias=True)
                (intermediate_act_fn): GELUActivation()
              )
              (output): BertOutput(
                (dense): Linear(in_features=3072, out_features=768, bias=True)
                (LayerNorm): LayerNorm((768,), eps=1e-12, elementwise_affine=True)
                (dropout): Dropout(p=0.1, inplace=False)
              )
            )
            (10): BertLayer(
              (attention): BertAttention(
                (self): BertSelfAttention(
                  (query): Linear(in_features=768, out_features=768, bias=True)
                  (key): Linear(in_features=768, out_features=768, bias=True)
                  (value): Linear(in_features=768, out_features=768, bias=True)
                  (dropout): Dropout(p=0.1, inplace=False)
                )
                (output): BertSelfOutput(
                  (dense): Linear(in_features=768, out_features=768, bias=True)
                  (LayerNorm): LayerNorm((768,), eps=1e-12, elementwise_affine=True)
                  (dropout): Dropout(p=0.1, inplace=False)
                )
              )
              (intermediate): BertIntermediate(
                (dense): Linear(in_features=768, out_features=3072, bias=True)
                (intermediate_act_fn): GELUActivation()
              )
              (output): BertOutput(
                (dense): Linear(in_features=3072, out_features=768, bias=True)
                (LayerNorm): LayerNorm((768,), eps=1e-12, elementwise_affine=True)
                (dropout): Dropout(p=0.1, inplace=False)
              )
            )
            (11): BertLayer(
              (attention): BertAttention(
                (self): BertSelfAttention(
                  (query): Linear(in_features=768, out_features=768, bias=True)
                  (key): Linear(in_features=768, out_features=768, bias=True)
                  (value): Linear(in_features=768, out_features=768, bias=True)
                  (dropout): Dropout(p=0.1, inplace=False)
                )
                (output): BertSelfOutput(
                  (dense): Linear(in_features=768, out_features=768, bias=True)
                  (LayerNorm): LayerNorm((768,), eps=1e-12, elementwise_affine=True)
                  (dropout): Dropout(p=0.1, inplace=False)
                )
              )
              (intermediate): BertIntermediate(
                (dense): Linear(in_features=768, out_features=3072, bias=True)
                (intermediate_act_fn): GELUActivation()
              )
              (output): BertOutput(
                (dense): Linear(in_features=3072, out_features=768, bias=True)
                (LayerNorm): LayerNorm((768,), eps=1e-12, elementwise_affine=True)
                (dropout): Dropout(p=0.1, inplace=False)
              )
            )
          )
        )
        (pooler): BertPooler(
          (dense): Linear(in_features=768, out_features=768, bias=True)
          (activation): Tanh()
        )
      )
    )
    (word2vec): Word2Vec()
    (char2vec): Char2Vec(
      (char2vec): Embedding(85, 50)
      (char_rnn): GRU(50, 50, batch_first=True, bidirectional=True)
    )
    (pos2vec): Pos2Vec(
      (pos2vec): Embedding(45, 50)
    )
  )
  (out_rnn): GRU(968, 968, batch_first=True, bidirectional=True)
  (transforms): Sequential(
    (0): Linear(in_features=1936, out_features=768, bias=True)
    (1): LayerNorm((768,), eps=1e-05, elementwise_affine=True)
  )
)
```

1.  测试代码

```
#!/usr/bin/env python
# _*_coding:utf-8_*_
# Author   :    Junhui Yu
# Time     :    2023/2/28 15:40


import logging
from flair.datasets import DataLoader
from tqdm import tqdm
import json
from feature_encoder import Encoder
from self_data import MyCorpus

logger = logging.getLogger(&quot;YUNLP&quot;)

corpus = MyCorpus(&quot;ace05&quot;, True, 128)

metadata = json.load(open(&#x27;data/ace05/vocab.json&#x27;, &#x27;r&#x27;, encoding=&#x27;utf-8&#x27;))

args = {
    &#x27;pos_dim&#x27;: 50,
    &#x27;char_dim&#x27;: 50,
    &#x27;word2vec&#x27;: &#x27;glove&#x27;,
    &#x27;chars_list&#x27;: metadata[&#x27;char&#x27;],
    &#x27;pos_list&#x27;: metadata[&#x27;pos&#x27;],
    &#x27;pretrain_model&#x27;: r&#x27;D:\BERT\bert-base-uncased&#x27;,
    &#x27;model_max_length&#x27;: 128,
}

loader = DataLoader(
    dataset=corpus.train,
    shuffle=True,
    drop_last=False,
    batch_size=1,
)

t = tqdm(loader)

encoder = Encoder(args)
print(encoder)

for data in t:
    out = encoder(data)
    print(out)
    break

```

1.  输出

```
(tensor([[[-0.6931, -0.6064, -0.3340,  ...,  0.8820,  2.0498,  0.7626],
         [-0.4846, -0.3343,  0.8345,  ...,  0.3118,  2.0231,  0.4113],
         [-1.3114, -0.7566,  0.1274,  ...,  0.2121,  1.9899,  0.1858],
         ...,
         [-0.4345,  0.6677,  0.0272,  ..., -0.1907,  1.5457, -1.8503],
         [-0.1394,  0.6579,  1.1406,  ..., -0.4034,  1.7865, -0.8288],
         [-0.7872,  0.0053,  1.2921,  ..., -1.2903,  1.2204, -0.6980]]],
       grad_fn=&lt;NativeLayerNormBackward0&gt;), tensor([[True, True, True, True, True, True, True, True, True, True]]))
```

## ** ****6、相关工具 **

bert-base：https://huggingface.co/bert-base-uncased

glove：http://nlp.stanford.edu/data/glove.6B.zip

pos embedding：spacy或者NLTK，embedding使用pytorch自带的。

## ** ****结论 **

本文实践了一种多特征的编码器，在NLP的下游任务上经过测试有性能的提升。注：具体任务具体分析，不一定完全能大幅度提升性能，但是也不会降性能。

完整代码链接：https://github.com/yujunhuics/feature_encoder