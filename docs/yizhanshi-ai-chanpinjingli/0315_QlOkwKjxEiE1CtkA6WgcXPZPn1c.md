# 1.1 文本预处理

> 来源：[https://v11enp9ok1h.feishu.cn/docx/PUh0dNXBOo8AXixmkh5cEnppnGT](https://v11enp9ok1h.feishu.cn/docx/PUh0dNXBOo8AXixmkh5cEnppnGT)

> 文本预处理是自然语言处理（NLP）中的基础且关键步骤，它将原始的非结构化文本数据转化为适合机器学习模型处理的格式。

**本文将系**统**介绍**文**本预处理**的三大核心环节：文本清洗、分词和词性标注。

---

## 文本清洗：净化原始文本数据

文本清洗是预处理的第一步，目的是去除文本中的噪声数据，提高后续处理的准确性。

### 编码格式处理

不同来源的文本可能采用不同的编码格式（如UTF-8、GBK、ASCII等），统一编码是首要任务

## 实例

```
# 编码转换示例
text = &quot;示例文本&quot;.encode(&#x27;gbk&#x27;)  # 假设原始编码是GBK
text = text.decode(&#x27;gbk&#x27;).encode(&#x27;utf-8&#x27;)  # 转换为UTF-8
```

**常见编码问题解决方案：**

+   使用chardet库自动检测编码

+   统一转换为UTF-8编码

+   处理无法解码的字符（通常替换或忽略）

### 特殊字符处理

不同场景下需要处理不同类型的特殊字符：

## 实例

```
import re

# 移除HTML标签示例
text = &quot;&lt;p&gt;这是一段&lt;b&gt;HTML&lt;/b&gt;文本&lt;/p&gt;&quot;
clean_text = re.sub(r&#x27;&lt;[^&gt;]+&gt;&#x27;, &#x27;&#x27;, text)
print(clean_text)  # 输出: 这是一段HTML文本
```

### 噪声数据去除

根据具体任务需求，可能需要：

1.  去除无关信息（广告、版权声明等）

1.  处理拼写错误（使用拼写检查库）

1.  标准化数字表示（如将"1000"统一为"1,000"）

1.  统一日期格式（"2023-01-01" vs "01/01/2023"）

---

## 分词（Tokenization）：将文本分解为基本单元

分词是将连续文本分割成有意义的语言单元（token）的过程，不同语言需要不同的分词方法。

### 英文分词方法

英文分词相对简单，主要基于空格和标点分割：

## 实例

```
# 使用NLTK进行英文分词
from nltk.tokenize import word_tokenize

text = &quot;Natural Language Processing is fascinating!&quot;
tokens = word_tokenize(text)
print(tokens)  # [&#x27;Natural&#x27;, &#x27;Language&#x27;, &#x27;Processing&#x27;, &#x27;is&#x27;, &#x27;fascinating&#x27;, &#x27;!&#x27;]
```

**英文分词注意事项：**

+   处理缩写（如"I'm"→"I"+"'m"）

+   保留或合并特定短语（如"New York"作为一个token）

+   处理连字符（"state-of-the-art"）

### 中文分词技术

中文没有明显的词边界，分词更为复杂。主要方法包括：

1.  **基于词典的分词**：最大匹配法、最短路径法

1.  **基于统计的分词**：HMM、CRF等序列标注方法

1.  **基于深度学习的分词**：BiLSTM-CRF、BERT等模型

## 实例

```
# 使用jieba进行中文分词
import jieba

text = &quot;自然语言处理非常有趣&quot;
tokens = jieba.lcut(text)
print(tokens)  # [&#x27;自然语言&#x27;, &#x27;处理&#x27;, &#x27;非常&#x27;, &#x27;有趣&#x27;]
```

### 子词分词（Subword Tokenization）

解决罕见词和词表膨胀问题，常用方法：

+   Byte Pair Encoding (BPE)：通过合并高频字符对构建子词

+   **WordPiece**：类似BPE，但基于概率合并

+   Unigram Language Model：从大词表开始逐步删除低概率子词

## 实例

```
# 使用HuggingFace的tokenizer示例
from transformers import BertTokenizer

tokenizer = BertTokenizer.from_pretrained(&#x27;bert-base-chinese&#x27;)
tokens = tokenizer.tokenize(&quot;自然语言处理&quot;)
print(tokens)  # [&#x27;自&#x27;, &#x27;然&#x27;, &#x27;语&#x27;, &#x27;言&#x27;, &#x27;处&#x27;, &#x27;理&#x27;]
```

### 常用分词工具对比

## 词性标注：理解词语的语法角色

词性标注（Part-of-Speech Tagging）是为分词结果中的每个词语标注其词性类别的过程。

### 词性标注的概念

词性标注有助于：

+   理解句子结构

+   消除词义歧义

+   支持更高级的NLP任务（如句法分析）

### 常见词性体系

不同语言和工具使用不同的词性标注体系：

英文常用Penn Treebank标签集（部分）：

+   NN：名词

+   VB：动词

+   JJ：形容词

+   RB：副词

+   PRP：代词

中文常用ICTCLAS标签集（部分）：

+   n：名词

+   v：动词

+   a：形容词

+   d：副词

+   r：代词

### 自动词性标注方法

1.  **基于规则的方法**：使用手工编写的规则进行标注

1.  **基于统计的方法**：HMM、MaxEnt等模型

1.  **基于深度学习的方法**：RNN、Transformer等神经网络

## 实例

```
# 使用spaCy进行词性标注
import spacy

nlp = spacy.load(&quot;en_core_web_sm&quot;)
doc = nlp(&quot;Natural Language Processing is fascinating!&quot;)
for token in doc:
    print(token.text, token.pos_)  # 输出每个词及其词性标签
```

**词性标注的评估指标：**

+   准确率（Accuracy）

+   未知词准确率（OOV Accuracy）

+   混淆矩阵分析

---

## 实践建议

1.  **预处理流程顺序**：编码处理→文本清洗→分词→词性标注

1.  **工具选择原则**：根据语言、任务需求和性能要求选择合适工具

1.  **自定义处理**：针对特定领域可能需要自定义词典或规则

1.  **性能优化**：对于大规模文本，考虑使用并行处理或高效工具