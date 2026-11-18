
# jieba分词用法及原理

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/DKOFd8OREoGQdHxu49DckVp6npf](https://qqs7y1hozd1.feishu.cn/docx/DKOFd8OREoGQdHxu49DckVp6npf)

## 1. 概述

上篇文章分析了自然语言处理，特别是中文处理中，分词的几个主要难点。为了解决这些难点，提出了基于字符串匹配的算法和基于统计的分词算法。针对当前的几种分词引擎，对其分词准确度和速度进行了评估。jieba分词作为一个开源项目，在准确度和速度方面均不错，是我们平时常用的分词工具。本文将对jieba分词的使用方法以及原理进行讲解，便于在理解jieba分词原理的同时，加深对前文讲解的分词难点和算法的理解。

## 1.1 特点

Jieba库分词有4种模式，最常用的还是前3种

1. 精确模式：就是把一段文本精确地切分成若干个中文单词，若干个中文单词之间经过组合，就精确地还原为之前的文本。其中不存在冗余单词。

2. 全模式：将一段文本中所有可能的词语都扫描出来，可能有一段文本它可以切分成不同的模式，或者有不同的角度来切分变成不同的词语，在全模式下，Jieba库会将各种不同的组合都挖掘出来。分词后的信息再组合起来会有冗余，不再是原来的文本。

3. 搜索引擎模式：在精确模式基础上，对发现的那些长的词语，我们会对它再次切分，进而适合搜索引擎对短词语的索引和搜索。也有冗余。

4. paddle模式：利用PaddlePaddle深度学习框架，训练序列标注（双向GRU）网络模型实现分词。同时支持词性标注。paddle模式使用需安装`paddlepaddle-tiny`，`pip install paddlepaddle-tiny==1.6.1`。目前paddle模式支持`jieba v0.40`及以上版本。`jieba v0.40`以下版本，请升级`jieba`，`pip install jieba --upgrade`。

## 1.2 安装说明

代码对 `Python 2/3` 均兼容

+ 全自动安装：`easy_install jieba` 或者 `pip install jieba` / `pip3 install jieba`

+ 半自动安装：先下载 http://pypi.python.org/pypi/jieba/ ，解压后运行 `python setup.py install`

+ 手动安装：将 `jieba` 目录放置于当前目录或者 `site-packages` 目录

+ 通过 `import jieba` 来引用

+ 如果需要使用paddle模式下的分词和词性标注功能，请先安装`paddlepaddle-tiny`，`pip install paddlepaddle-tiny==1.6.1`。

## 1.3 算法

+ 基于前缀词典实现高效的词图扫描，生成句子中汉字所有可能成词情况所构成的有向无环图 (`DAG`)

+ 采用了动态规划查找最大概率路径, 找出基于词频的最大切分组合

+ 对于`未登录词`，采用了基于汉字成词能力的 `HMM` 模型，使用了 `Viterbi` 算法

## 2. jieba分词用法

jieba分词是一个开源项目，地址为：`fxsjy/jieba`: 结巴中文分词

它在分词准确度和速度方面均表现不错。其功能和用法如下。

## 2.1 分词

`jieba.cut` 方法接受四个输入参数：

+ 需要分词的字符串；

+ `cut_all` 参数用来控制是否采用全模式；

+ `HMM` 参数用来控制是否使用 `HMM` 模型；

+ `use_paddle` 参数用来控制是否使用paddle模式下的分词模式，paddle模式采用延迟加载方式，通过`enable_paddle`接口安装`paddlepaddle-tiny`，并且import相关代码；

`jieba.cut_for_search` 方法接受两个参数：

+ 需要分词的字符串；

+ 是否使用 `HMM` 模型。该方法适合用于搜索引擎构建倒排索引的分词，粒度比较细

待分词的字符串可以是 `unicode` 或 `UTF-8` 字符串、`GBK` 字符串。注意：不建议直接输入 `GBK` 字符串，可能无法预料地错误解码成 `UTF-8`

`jieba.cut` 以及 `jieba.cut_for_search` 返回的结构都是一个可迭代的 `generator`，可以使用 `for` 循环来获得分词后得到的每一个词语(`unicode`)，或者用

`jieba.lcut` 以及 `jieba.lcut_for_search` 直接返回 `list`

`jieba.Tokenizer(dictionary=DEFAULT_DICT)` 新建自定义分词器，可用于同时使用不同词典。`jieba.dt` 为默认分词器，所有全局分词相关函数都是该分词器的映射。

支持三种分词模式

```python
# encoding=utf-8
import jieba

seg_list = jieba.cut("我来到北京清华大学", cut_all=True)
print("Full Mode: " + "/ ".join(seg_list))  # 全模式

seg_list = jieba.cut("我来到北京清华大学", cut_all=False)
print("Default Mode: " + "/ ".join(seg_list))  # 精确模式

seg_list = jieba.cut("他来到了网易杭研大厦")  # 默认是精确模式
print(", ".join(seg_list))

seg_list = jieba.cut_for_search("小明硕士毕业于中国科学院计算所，后在日本京都大学深造")  # 搜索引擎模式
print(", ".join(seg_list))
```

输出为

```
【全模式】: 我/ 来到/ 北京/ 清华/ 清华大学/ 华大/ 大学

【精确模式】: 我/ 来到/ 北京/ 清华大学

【新词识别】：他, 来到, 了, 网易, 杭研, 大厦    (此处，“杭研”并没有在词典中，但是也被Viterbi算法识别出来了)

【搜索引擎模式】： 小明, 硕士, 毕业, 于, 中国, 科学, 学院, 科学院, 中国科学院, 计算, 计算所, 后, 在, 日本, 京都, 大学, 日本京都大学, 深造
```

## 2.2 添加自定义词典

+ 开发者可以指定自己自定义的词典，以便包含 `jieba` 词库里没有的词。虽然 `jieba` 有新词识别能力，但是自行添加新词可以保证更高的正确率

+ 用法：`jieba.load_userdict(file_name)` ，`file_name` 为文件类对象或自定义词典的路径

+ 词典格式和 `dict.txt` 一样，一个词占一行；每一行分三部分：词语、词频（可省略）、词性（可省略），用空格隔开，顺序不可颠倒。`file_name` 若为路径或二进制方式打开的文件，则文件必须为 `UTF-8` 编码。

+ 词频省略时使用自动计算的能保证分出该词的词频。

使用起来很简单，我们先创建一个文件，比如`user_dict.txt`，其中每一行代表一个新词，分别为词语，词频，词性。如下：

```
创新办 3 i
云计算 5
凱特琳 nz
台中
```

然后在代码中分词前，加载这个自定义词典即可。更改分词器（默认为 `jieba.dt`）的 `tmp_dir` 和 `cache_file` 属性，可分别指定缓存文件所在的文件夹及其文件名，用于受限的文件系统。

```python
jieba.load_userdict("user_dict.txt")
```

加载自定义词典的分词效果：

```
之前： 李小福 / 是 / 创新 / 办 / 主任 / 也 / 是 / 云 / 计算 / 方面 / 的 / 专家 /

加载自定义词库后：　李小福 / 是 / 创新办 / 主任 / 也 / 是 / 云计算 / 方面 / 的 / 专家 /
```

## 2.3 调整词典

+ 使用 `add_word(word, freq=None, tag=None)` 和 `del_word(word)` 可在程序中动态修改词典。

+ 使用 `suggest_freq(segment, tune=True)` 可调节单个词语的词频，使其能（或不能）被分出来。

+ 注意：自动计算的词频在使用 `HMM` 新词发现功能时可能无效。

```python
# 1 使用del_word()使得某个词语不会出现
print('/'.join(jieba.cut('如果放到post中将出错。', HMM=False)))
如果/放到/post/中将/出错/。

jieba.del_word("中将")
print('/'.join(jieba.cut('如果放到post中将出错。', HMM=False)))
如果/放到/post/中/将/出错/。

# 2 使用add_word()添加新词到字典中
print('/'.join(jieba.cut('「台中」正确应该不会被切开', HMM=False)))
「/台/中/」/正确/应该/不会/被/切开

jieba.add_word("台中")
print('/'.join(jieba.cut('「台中」正确应该不会被切开', HMM=False)))
「/台中/」/正确/应该/不会/被/切开

# 3 使用suggest_freq()调整某个词语的词频，使得其在设置的词频高是能分出，词频低时不能分出
jieba.suggest_freq('台中', True)
69
print('/'.join(jieba.cut('「台中」正确应该不会被切开', HMM=False)))
「/台中/」/正确/应该/不会/被/切开
```

## 2.4 关键词提取

关键词提取，将文本中最能表达文本含义的词语抽取出来，有点类似于论文的关键词或者摘要。关键词抽取可以采取：

### 基于TF-IDF的关键词抽取算法

目标是获取文本中词频高，也就是`TF`大的，且语料库其他文本中词频低的，也就是`IDF`大的。这样的词可以作为文本的标志，用来区分其他文本。

API函数

+ `jieba.analyse.extract_tags(sentence, topK=20, withWeight=False, allowPOS=())`

+ `sentence` 为待提取的文本

+ `topK` 为返回几个 `TF/IDF` 权重最大的关键词，默认值为 20

+ `withWeight` 为是否一并返回关键词权重值，默认值为 `False`

+ `allowPOS` 仅包括指定词性的词，默认值为空，即不筛选

+ `jieba.analyse.TFIDF(idf_path=None)` ，新建 `TFIDF` 实例，`idf_path` 为 `IDF` 频率文件

代码示例

```python
from jieba import analyse
# 引入TF-IDF关键词抽取接口
tfidf = analyse.extract_tags

# 原始文本
text = "线程是程序执行时的最小单位，它是进程的一个执行流，\
        是CPU调度和分派的基本单位，一个进程可以由很多个线程组成，\
        线程间共享进程的所有资源，每个线程有自己的堆栈和局部变量。\
        线程由CPU独立调度执行，在多CPU环境下就允许多个线程同时运行。\
        同样多线程也可以实现并发操作，每个请求分配一个线程来处理。"

# 基于TF-IDF算法进行关键词抽取
keywords = tfidf(text)
print "keywords by tfidf:"
# 输出抽取出的关键词
for keyword in keywords:
    print keyword + "/",


# 输出为：
keywords by tfidf:
线程/ CPU/ 进程/ 调度/ 多线程/ 程序执行/ 每个/ 执行/ 堆栈/ 局部变量/ 单位/ 并发/ 分派/ 一个/ 共享/ 请求/ 最小/ 可以/ 允许/ 分配/ 
```

### 基于TextRank的关键词抽取算法

1. 先将文本进行分词和词性标注，将特定词性的词（比如名词）作为节点添加到图中。

2. 出现在一个窗口中的词语之间形成一条边，窗口大小可设置为2~10之间，它表示一个窗口中有多少个词语。

3. 对节点根据入度节点个数以及入度节点权重进行打分，入度节点越多，且入度节点权重大，则打分高。

4. 然后根据打分进行降序排列，输出指定个数的关键词。

API函数

+ `jieba.analyse.textrank(sentence, topK=20, withWeight=False, allowPOS=('ns', 'n', 'vn', 'v'))` 直接使用，接口相同，注意默认过滤词性。

+ `jieba.analyse.TextRank()` 新建自定义 `TextRank` 实例

+ 算法论文：`TextRank: Bringing Order into Texts`

代码示例

```python
from jieba import analyse
# 引入TextRank关键词抽取接口
textrank = analyse.textrank

# 原始文本
text = "线程是程序执行时的最小单位，它是进程的一个执行流，\
        是CPU调度和分派的基本单位，一个进程可以由很多个线程组成，\
        线程间共享进程的所有资源，每个线程有自己的堆栈和局部变量。\
        线程由CPU独立调度执行，在多CPU环境下就允许多个线程同时运行。\
        同样多线程也可以实现并发操作，每个请求分配一个线程来处理。"

print "\nkeywords by textrank:"
# 基于TextRank算法进行关键词抽取
keywords = textrank(text)
# 输出抽取出的关键词
for keyword in keywords:
    print keyword + "/",

# 输出为：
keywords by textrank:
线程/ 进程/ 调度/ 单位/ 操作/ 请求/ 分配/ 允许/ 基本/ 共享/ 并发/ 堆栈/ 独立/ 执行/ 分派/ 组成/ 资源/ 实现/ 运行/ 处理/
```

## 2.5 词性标注

利用`jieba.posseg`模块来进行词性标注，会给出分词后每个词的词性。词性标示兼容`ICTCLAS` 汉语词性标注集，可查阅网站

API函数

+ `jieba.posseg.POSTokenizer(tokenizer=None)` 新建自定义分词器，`tokenizer` 参数可指定内部使用的 `jieba.Tokenizer` 分词器。`jieba.posseg.dt` 为默认词性标注分词器。

+ 标注句子分词后每个词的词性，采用和 `ictclas` 兼容的标记法。

+ 除了`jieba`默认分词模式，提供`paddle`模式下的词性标注功能。`paddle`模式采用延迟加载方式，通过`enable_paddle()`安装`paddlepaddle-tiny`，并且import相关代码；

代码示例

```python
import jieba.posseg as pseg
words = pseg.cut("我爱北京天安门")
for word, flag in words:
    print('%s %s' % (word, flag))

我 r        # 代词
爱 v        # 动词
北京 ns        # 名词
天安门 ns        # 名词
```

paddle模式词性标注对应表如下：

paddle模式词性和专名类别标签集合如下表，其中词性标签 24 个（小写字母），专名类别标签 4 个（大写字母）。

![](img/ab77261ee5f24d1c441b7171f66058a9.png)

## 2.6 并行分词

将文本按行分隔后，每行由一个`jieba`分词进程处理，之后进行归并处理，输出最终结果。这样可以大大提高分词速度。

原理：将目标文本按行分隔后，把各行文本分配到多个 Python 进程并行分词，然后归并结果，从而获得分词速度的可观提升

基于 python 自带的 `multiprocessing` 模块，目前暂不支持 Windows

用法：

+ `jieba.enable_parallel(4)` # 开启并行分词模式，参数为并行进程数

+ `jieba.disable_parallel()` # 关闭并行分词模式

```python
jieba.enable_parallel(4) # 开启并行分词模式，参数为并行进程数
jieba.disable_parallel() # 关闭并行分词模式
```

实验结果：在 4 核 3.4GHz Linux 机器上，对金庸全集进行精确分词，获得了 1MB/s 的速度，是单进程版的 3.3 倍。

**注意**：并行分词仅支持默认分词器 `jieba.dt` 和 `jieba.posseg.dt`。

## 2.7 Tokenize：返回词语在原文的起止位置

注意，输入参数只接受 `unicode`

### 默认模式
