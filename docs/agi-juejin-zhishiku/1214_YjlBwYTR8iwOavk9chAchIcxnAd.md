# Transformer 简单上手：文本二分类

> 来源：[https://agijuejin.feishu.cn/docx/N8O4d8OgjoW1IHxNqTlcDKWjn0d](https://agijuejin.feishu.cn/docx/N8O4d8OgjoW1IHxNqTlcDKWjn0d)

> 作者：王几行XING

> 原文： [https://zhuanlan.z](https://zhuanlan.zhihu.com/p/670830339)hihu.com/p/670830...

## 案例目标：根据影评的内容，来判断该影评是积极还是消极。二分类预测。

在过去，此类 [文本分类 ](https://www.zhihu.com/search?q=%E6%96%87%E6%9C%AC%E5%88%86%E7%B1%BB&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A670830339%7D)任务，可以典型的使用 RNN、LSTM 和 GRU 等序列模型完成。这里所谓的序列，指的是一段影评，加上最后 negative or positive 的结尾。

在有了 Transformer 这一神器之后，我们可以尝试用它。（从本案例结果来看，Transformer并没有完成超越。原因之一可能在于数据量过少。）

**需要注意的是，在使用 Transformer 和 LSTM 等序列模型进行文本分类（如情感分析）时，标签并不是放在影评数据序**列的末尾作为最后一个要预测的词汇。相反，标签是独立于文本序列的，并且在训练过程中用作监督信号。下面是这个过程的详细说明：

1.  **输入数据（影评） **：这些是模型的输入，通常是 [文本序列 ](https://www.zhihu.com/search?q=%E6%96%87%E6%9C%AC%E5%BA%8F%E5%88%97&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A670830339%7D)。在预处理阶段，这些文本序列会被转换为数字表示，比如通过词嵌入（word embeddings）。在 LSTM 网络中，这些序列作为输入被逐步处理。

1.  **标签（ **[**情感分类 **](https://www.zhihu.com/search?q=%E6%83%85%E6%84%9F%E5%88%86%E7%B1%BB&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A670830339%7D)**） **：这些是训练数据的一部分，但它们不是序列的一部分，而是独立的信息。对于情感分析，每个影评会有一个标签，比如“正面”或“负面”。这些标签在训练过程中用来告诉模型每个输入序列所对应的正确输出。

1.  **训练过程 **：在[训练 LST](https://www.zhihu.com/search?q=%E4%BA%A4%E5%8F%89%E7%86%B5%E6%8D%9F%E5%A4%B1&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A670830339%7D)M 网络时，网络会尝试学习如何[从输入的文本序](https://www.zhihu.com/search?q=%E5%8F%8D%E5%90%91%E4%BC%A0%E6%92%AD%E7%AE%97%E6%B3%95&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A670830339%7D)列中提取特征，并基于这些特征做出分类决策。网络的输出是一个分类结果（比如“正面”或“负面”），这个结果会与真实的标签进行比较，通过计算损失（如 交叉熵损失 ）来评估模型的性能。然后使用 反向传播算法 来调整网络的权重，以减少预测和真实标签之间的差异。

1.  **输出层 **：LSTM 网络的最后通常会有一个全连接层（Dense Layer），该层的输出是分类决策（例如，在情感分析中是“正面”或“负面”）。这个输出是基于整个输入序列的信息，而不是基于序列中的最后一个词汇。

小结：标签在训练 LSTM 网络时用作外部监督信号，而不是作为输入序列的一部分。这样可以让模型学习如何从整个文本序列中提取有用的信息，并做出正确的分类决策。

## 第一步，下载数据

这里我们下载并探索IMDB数据集的，它使用了TensorFlow框架。IMDB数据集包含电影[评论，这些评论](https://www.zhihu.com/search?q=%E8%87%AA%E7%84%B6%E8%AF%AD%E8%A8%80%E5%A4%84%E7%90%86&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A670830339%7D)已经被转换成了一系列数字，每个数字代表一个单词。这个数据集常用于 自然语言处理 和情感分析任务。

```
# 导入 TensorFlow 和 Keras 相关的库
import tensorflow as tf
from tensorflow import keras
from tensorflow.keras import layers
from tensorflow.keras.datasets import imdb

# 下载 IMDB 数据集
# 这里只加载了前 10,000 个最常出现的单词（基于频率）
(train_data, train_labels), (test_data, test_labels) = imdb.load_data(num_words=10000)

# 探索数据集
# 查看训练数据的形状（即数据的维度）
train_data_shape = train_data.shape
# 查看测试数据的形状
test_data_shape = test_data.shape
# 查看训练数据中的第一条评论（以数字列表的形式表示，每个数字代表一个单词）
sample_review = train_data[0]
# 查看与第一条评论对应的标签（0代表负面评论，1代表正面评论）
sample_label = train_labels[0]

# 显示训练数据和测试数据的形状
print(&quot;Training data shape:&quot;, train_data.shape)
print(&quot;Test data shape:&quot;, test_data.shape)

# 打印出第一条评论和其对应的标签
print(&quot;Sample review:&quot;, train_data[0])
print(&quot;Sample label:&quot;, train_labels[0])
```

![](img/515f6f2e26b9e671fd5ba9a5b0bef43b.png)

为了将数字展示为本文，我们需要先基于 index map 进行转换（decode）。

```
# 获取 IMDB 数据集的词汇索引映射（单词到整数索引的映射）
word_index = imdb.get_word_index()
# 生成反向词汇索引映射（整数索引到单词的映射）
# 这里将 word_index 中的键值对调换位置，创建一个新的字典
reverse_word_index = dict([(value, key) for (key, value) in word_index.items()])

# 定义一个函数，用于将编码后的评论解码为文本
# 编码评论是指使用整数代替单词的评论
def decode_review(encoded_review):
    # 将每个整数索引转换回对应的单词，如果索引不在 reverse_word_index 中，则返回 &#x27;?&#x27; 
    # 这里减去 3 是因为 0、1、2 是为 &quot;padding&quot;、&quot;start of sequence&quot; 和 &quot;unknown&quot; 保留的索引
    return &#x27; &#x27;.join([reverse_word_index.get(i - 3, &#x27;?&#x27;) for i in encoded_review])

# 打印出训练集中第一条评论的解码文本
print(decode_review(train_data[0]))  # 解码训练集中的第一条评论
```

输出的结果：

```
Downloading data from https://storage.googleapis.com/tensorflow/tf-keras-datasets/imdb_word_index.json
1641221/1641221 [==============================] - 0s 0us/step

? this film was just brilliant casting location scenery story direction everyone&#x27;s 
really suited the part they played and you could just imagine being there robert ? 
is an amazing actor and now the same being director ? father came from the same scottish 
island as myself so i loved the fact there was a real connection with this film the witty 
remarks throughout the film were great it was just brilliant so much that i bought the film 
as soon as it was released for ? and would recommend it to everyone to watch and the fly 
fishing was amazing really cried at the end it was so sad and you know what they say if you 
cry at a film it must have been good and this definitely was also ? to the two little boy&#x27;s 
that played the ? of norman and paul they were just brilliant children are often left out of 
the ? list i think because the stars that play them all grown up are such a big profile for 
the whole film but these children are amazing and should be praised for what they have done 
don&#x27;t you think the whole story was so lovely because it was true and was someone&#x27;s life after
 all that was shared with us all
```

## 第二步，定义一个 Transformer block 层

Transformer 块主要由以下部分组成：

1.  [**多头注意力层 **](https://www.zhihu.com/search?q=%E5%A4%9A%E5%A4%B4%E6%B3%A8%E6%84%8F%E5%8A%9B%E5%B1%82&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A670830339%7D)**（MultiHe**adAttention）：允许模型同时关注输入 序列的不同位置。

1.  **前馈网络 **（Feed Forward Network）：由两个全连接层组成，增加模型的处理能力。

1.  **层归一化 **（LayerNormalization）：标准化层的输出，帮助稳定和加速训练。

1.  **Dropout **：减少过拟合的风险。

1.  **残差连接 **：帮助避免深度网络中的 [梯度消失问题 ](https://www.zhihu.com/search?q=%E6%A2%AF%E5%BA%A6%E6%B6%88%E5%A4%B1%E9%97%AE%E9%A2%98&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A670830339%7D)。

```
# 定义一个 TransformerBlock 类，继承自 Keras 的 Layer 类
class TransformerBlock(layers.Layer):
    # 类的初始化方法
    def __init__(self, embed_dim, num_heads, ff_dim, rate=0.1):
        # 调用父类的初始化方法
        super().__init__()
        # 创建一个多头注意力（MultiHeadAttention）层
        self.att = layers.MultiHeadAttention(num_heads=num_heads, key_dim=embed_dim)
        # 创建一个前馈网络（Feed Forward Network），包含两个全连接层
        self.ffn = keras.Sequential(
            [layers.Dense(ff_dim, activation=&quot;relu&quot;), layers.Dense(embed_dim),]
        )
        # 创建两个层归一化（LayerNormalization）层
        self.layernorm1 = layers.LayerNormalization(epsilon=1e-6)
        self.layernorm2 = layers.LayerNormalization(epsilon=1e-6)
        # 创建两个 dropout 层，用于防止过拟合
        self.dropout1 = layers.Dropout(rate)
        self.dropout2 = layers.Dropout(rate)

    # 定义 call 方法，用于处理输入
    def call(self, inputs, training):
        # 应用多头注意力机制
        attn_output = self.att(inputs, inputs)
        # 应用第一个 dropout 层
        attn_output = self.dropout1(attn_output, training=training)
        # 应用第一个层归一化，并进行残差连接
        out1 = self.layernorm1(inputs + attn_output)
        # 通过前馈网络
        ffn_output = self.ffn(out1)
        # 应用第二个 dropout 层
        ffn_output = self.dropout2(ffn_output, training=training)
        # 应用第二个层归一化，并进行残差连接
        return self.layernorm2(out1 + ffn_output)
```

## 第三步，定义一个嵌入层

这段代码定义了一个 TokenAndPositionEmbedding 类，用于在基于 Transformer 的模型中嵌入词汇和位置信息。在 Transformer 模型中，词嵌入 （token embeddings） 用于将词汇转换成固定长度的向量，而位置嵌入（position embeddings）用于给模型提供关于单词在序列中位置的信息。这两种嵌入相加后，可以提供关于词汇内容和位置的综合信息，这对于处理序列数据，如文本，是非常重要的。

```
# 定义一个 TokenAndPositionEmbedding 类，继承自 Keras 的 Layer 类
class TokenAndPositionEmbedding(layers.Layer):
    # 类的初始化方法
    def __init__(self, maxlen, vocab_size, embed_dim):
        # 调用父类的初始化方法
        super().__init__()
        # 创建一个词嵌入层，用于将词汇标识符转换为嵌入向量
        # vocab_size 是词汇表的大小，embed_dim 是嵌入向量的维度
        self.token_emb = layers.Embedding(input_dim=vocab_size, output_dim=embed_dim)
        # 创建一个位置嵌入层，用于将位置信息转换为嵌入向量
        # maxlen 是序列的最大长度，embed_dim 是嵌入向量的维度
        self.pos_emb = layers.Embedding(input_dim=maxlen, output_dim=embed_dim)

    # 定义 call 方法，用于处理输入
    def call(self, x):
        # 获取输入序列 x 的长度
        maxlen = tf.shape(x)[-1]
        # 生成一个从 0 到 maxlen 的位置索引序列
        positions = tf.range(start=0, limit=maxlen, delta=1)
        # 通过位置嵌入层将位置索引转换为嵌入向量
        positions = self.pos_emb(positions)
        # 通过词嵌入层将输入序列 x 转换为嵌入向量
        x = self.token_emb(x)
        # 将词嵌入和位置嵌入相加，得到最终的嵌入表示
        return x + positions
```

## 第四步，数据准备

```
# 设置词汇表的大小为 20000，只考虑最常出现的 20000 个单词
vocab_size = 20000  
# 设置序列的最大长度为 200，每条评论只考虑前 200 个单词
maxlen = 200  
# 加载 IMDB 数据集，限定词汇表的大小
# x_train 和 y_train 是训练数据集，x_val 和 y_val 是验证数据集
(x_train, y_train), (x_val, y_val) = keras.datasets.imdb.load_data(num_words=vocab_size)
# 打印训练数据集的大小
print(len(x_train), &quot;Training sequences&quot;)
# 打印验证数据集的大小
print(len(x_val), &quot;Validation sequences&quot;)
# 对训练数据集中的评论进行填充或截断，使得每条评论的长度统一为 maxlen
x_train = keras.utils.pad_sequences(x_train, maxlen=maxlen)
# 对验证数据集中的评论进行填充或截断，使得每条评论的长度统一为 maxlen
x_val = keras.utils.pad_sequences(x_val, maxlen=maxlen)

# 25000 Training sequences
# 25000 Validation sequences
```

## 第五步，定义模型

**“Transforme**r layer outputs one vector for **each time step of our inpu****t sequence. H**ere, we take the mean across all time steps and use a feed-forward network on top of it to classify text.”

[以下代码定义了](https://www.zhihu.com/search?q=%E7%A5%9E%E7%BB%8F%E7%BD%91%E7%BB%9C%E6%A8%A1%E5%9E%8B&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A670830339%7D)一个使用 Transformer 块的 神经网络模型 ，适用于处理序列数据（如文本）。代码中创建了一个嵌入层、一个 Transformer 块，然后通过几个层来构建最终的模型。

```
# 设置每个 token 的嵌入维度
embed_dim = 32  
# 设置注意力机制中的头数
num_heads = 2  
# 设置 Transformer 块内前馈网络的隐藏层大小
ff_dim = 32  

# 定义模型的输入，指定输入序列的最大长度
inputs = layers.Input(shape=(maxlen,))
# 创建一个 Token 和位置嵌入层，用于处理输入序列
embedding_layer = TokenAndPositionEmbedding(maxlen, vocab_size, embed_dim)
# 将输入数据传递给嵌入层
x = embedding_layer(inputs)
# 创建一个 Transformer 块
transformer_block = TransformerBlock(embed_dim, num_heads, ff_dim)
# 将嵌入后的输入数据传递给 Transformer 块
x = transformer_block(x)
# 使用全局平均池化层，以减少维度并处理变长输入
x = layers.GlobalAveragePooling1D()(x)
# 应用 Dropout 层，用于减少过拟合
x = layers.Dropout(0.1)(x)
# 添加一个全连接层，激活函数为 relu
x = layers.Dense(20, activation=&quot;relu&quot;)(x)
# 再次应用 Dropout 层
x = layers.Dropout(0.1)(x)
# 最后一个全连接层，输出维度为 2，激活函数为 softmax，用于分类
outputs = layers.Dense(2, activation=&quot;softmax&quot;)(x)

# 创建模型，指定输入和输出
model = keras.Model(inputs=inputs, outputs=outputs)
```

## 第六步，训练和验证模型

```
model.compile(
    optimizer=&quot;adam&quot;, loss=&quot;sparse_categorical_crossentropy&quot;, metrics=[&quot;accuracy&quot;]
)
history = model.fit(
    x_train, y_train, batch_size=32, epochs=50, validation_data=(x_val, y_val)
)
```

![](img/2144bd5b2b5b9d6dfa2f23b42f3e4289.png)

![](img/5182856f520e07d114731dbb82d3eec2.png)

从上述训练历史可以看出，一个 epoch 模型就达到最优，接着训练反而降低了模型的性能。

## 第七步，用一个例子进行演示

这段代码将接收一个文本样本，处理它，然后使用您的模型进行预测。预测结果将被解释为情感类别

（例如，“Positive”或“Negative”）。您可以使用任何来自 IMDB 数据集或您自己的文本样本来测试模型。

```
# 导入 numpy 库
import numpy as np
# 从 tensorflow.keras.preprocessing.sequence 导入 pad_sequences 函数
from tensorflow.keras.preprocessing.sequence import pad_sequences

# 定义一个用于文本预处理的函数
def preprocess_text(text, tokenizer, maxlen=200):
    # 使用 tokenizer 将文本字符串转换为 token 序列
    sequences = tokenizer.texts_to_sequences([text])
    # 使用 pad_sequences 来确保所有序列的长度一致，填充或截断到 maxlen
    padded_sequence = pad_sequences(sequences, maxlen=maxlen)
    return padded_sequence

# 从 tensorflow.keras.datasets 导入 imdb 模块
from tensorflow.keras.datasets import imdb

# 重新加载 IMDB 数据集的词汇索引映射
word_index = imdb.get_word_index()
# 调整每个词汇索引，加 3 （因为有几个特殊的 token 需要插入）
word_index = {k: (v + 3) for k, v in word_index.items()}
# 添加特殊 token 到词汇索引映射
word_index[&quot;&lt;PAD&gt;&quot;] = 0  # 填充 token
word_index[&quot;&lt;START&gt;&quot;] = 1  # 序列开始 token
word_index[&quot;&lt;UNK&gt;&quot;] = 2  # 未知词汇 token
word_index[&quot;&lt;UNUSED&gt;&quot;] = 3  # 未使用的 token

# 创建一个 Tokenizer 实例
tokenizer = keras.preprocessing.text.Tokenizer()
# 设置 tokenizer 的词汇索引映射
tokenizer.word_index = word_index

# 定义一个函数，用于预测文本的情感并显示结果
def predict_and_display(text, model, tokenizer):
    # 对传入的文本进行预处理
    processed_text = preprocess_text(text, tokenizer)
    # 使用传入的模型对处理后的文本进行情感预测
    prediction = model.predict(processed_text)
    # 定义类别名称
    class_names = [&quot;Negative&quot;, &quot;Positive&quot;]
    # 根据预测结果选择类别名称
    predicted_class = class_names[np.argmax(prediction)]
    # 打印原始评论和预测的情感类别
    print(f&quot;Review: {text}\nPredicted sentiment: {predicted_class}&quot;)

# 定义一个示例文本
sample_text = &quot;This movie was fantastic! I really loved it.&quot;
# 使用定义的函数测试模型
predict_and_display(sample_text, model, tokenizer)
```

![](img/85d1ac7ad0979b4ed4c3ec87377a648f.png)

主要参考：

+   1， [Keras documentation: IMDB movie ](https://link.zhihu.com/?target=https%3A//keras.io/api/datasets/imdb/)review sentiment classification dataset ；

+   2， [K](https://link.zhihu.com/?target=https%3A//keras.io/examples/nlp/text_classification_with_transformer/)eras documentation: Text classification with Transformer