
# 实践：给BERT补充其他特征的编码器

> 来源：https://geekbang.feishu.cn/docx/CUTOdnIgJofmI6xfaaRchE5YnCd

> 👨🏻 作者：余俊晖

> 📃 作者介绍：一名NLP炼丹师，NLP算法竞赛TOP选手

> 🛖 👸号：大模型自然语言处理

> ⏰ 收录时间：2024-05-28 17:09:42

> 🔗 原文链接：https://mp.weixin.qq.com/s?__biz=Mzg4NjI0NDg0Ng==&mid=2247483974&idx=1&sn=52990b70e5d6f5b5041bee21f33d077e&chksm=cf9dd6c7f8ea5fd11342b713ff6abd91389b41dcd0cded07b1a216803d0b9bdcd62f91ca9244#rd

![](img/9fec404ad1e689053b72541965d599b2.png)

关注作者👸号，获取更多AI知识

## 前言

特征工程是传统的机器学习基石，然而，随着BERT等预训练语言模型的发展，文本语义表示得到了极大的改善，本文将实践一种多特征编码器。

## 1、POS特征生成

### 代码

```
import spacy
nlp = spacy.load("en_core_web_sm")

def spacy_pos_tag(sentence):
    doc = nlp(sentence)
    print(list(doc.sents))
    pos = []
    for word in list(doc.sents)[0]:
        pos.append(word.tag_)
    return pos

if __name__ == '__main__':
    print(spacy_pos_tag('Iraq Blames Market Blast on Coalition .'))
```

### 输出

```
[Iraq Blames Market Blast on Coalition .]
['NNP', 'NNP', 'NNP', 'NNP', 'IN', 'NNP', '.']
```

## 2、NLP中常见的特征

+ Word特征

+ Token特征

+ Char特征

+ POS特征

## 3、数据处理

本文与ACE2005数据集为例，处理该数据的格式样例如下：

```
[
    {
        "tokens": ["Their", "military", "service", "goes", "back", "to", "the", "Vietnam", "era", "."],
        "pos": ["PRP$", "JJ", "NN", "VBZ", "RB", "TO", "DT", "NNP", "NN", "."],
        "entities": [
            {"type": "PER", "start": 0, "end": 1},
            {"type": "ORG", "start": 1, "end": 2},
            {"type": "GPE", "start": 7, "end": 8}
        ],
        "ltokens": ["WOODRUFF", "We", "know", "that", "some", "of", "the", "American", "troops", "now", "fighting",
                    "in", "Iraq", "are", "longtime", "veterans", "of", "warfare", ",", "probably", "not", "most", ",",
                    "but",
                    "some", "."],
        "rtokens": ["Others", ",", "though", ",", "are", "novices", "."]
    },
    ...
]
```

词汇表样例：

```
{
  "pos": [
    "FW",
    "VBZ",
    "PRP",
    "DT",
    "VBD",
    "IN",
    "NNS",
    ".",
    "CD",
    "``",
    "WDT",
    "''",
    "POS",
    "VBN",
    "WP",
    "RBS",
    "NNPS",
    "JJS",
    "RB",
    ":",
    "JJR",
    "PRP$",
    "WP$",
    "TO",
    "NN",
    "WRB",
    "VB",
    "-RRB-",
    "JJ",
    "MD",
    "$",
    "-LRB-",
    "VBP",
    "RP",
    "RBR",
    "LS",
    "EX",
    "NNP",
    "CC",
    "#",
    ",",
    "SYM",
    "PDT",
    "UH",
    "VBG"
  ],
  "char": [
    "o",
    "8",
    "/",
    ">",
    "^",
    "@",
    "D",
    "Y",
    "?",
    "c",
    ".",
    "Q",
    "R",
    "0",
    "e",
    "W",
    "5",
    "X",
    "p",
    "r",
    "M",
    "C",
    "|",
    "i",
    "Z",
    "l",
    "9",
    "j",
    "4",
    "$",
    "%",
    "y",
    "P",
    "s",
    "6",
    "G",
    "!",
    "m",
    "S",
    "I",
    "*",
    "h",
    ",",
    "k",
    "_",
    "a",
    "B",
    "v",
    "d",
    "N",
    "1",
    "7",
    "-",
    "q",
    "'",
    "2",
    "w",
    ":",
    "<",
    "3",
    ";",
    "t",
    "u",
    "+",
    "J",
    "&",
    "`",
    "F",
    "=",
    "V",
    "f",
    "L",
    "U",
    "T",
    "n",
    "x",
    "b",
    "K",
    "#",
    "E",
    "g",
    "H",
    "O",
    "z",
    "A"
  ],
  "type": [
    "FAC",
    "VEH",
    "ORG",
    "GPE",
    "PER",
    "WEA",
    "LOC"
  ]
}
```

## 4、编码器代码实现

### 4.1. 数据集构建代码

`self_data.py`
