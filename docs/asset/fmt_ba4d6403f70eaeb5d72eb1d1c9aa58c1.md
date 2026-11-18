
```py
tfidf_matrix_word_char_train = hstack((tfidf_matrix_word_train, tfidf_matrix_char_train))
tfidf_matrix_word_char_test = hstack((tfidf_matrix_word_test, tfidf_matrix_char_test))

lr_word_char = LogisticRegression(solver='sag', verbose=2)
lr_word_char.fit(tfidf_matrix_word_char_train, y_train)

y_pred_word_char = lr_word_char.predict(tfidf_matrix_word_char_test)
joblib.dump(lr_word_char, './models/lr_word_char_ngram.pkl')

pd.DataFrame(y_pred_word_char, columns=['y_pred']).to_csv('./predictions/lr_word_char_ngram.csv', index=False)
y_pred_word_char = pd.read_csv('./predictions/lr_word_char_ngram.csv')
print(accuracy_score(y_test, y_pred_word_char))
0.81423845895
```

得到了 81.4% 的准确率。该模型只加了一个整体单元，但结果比之前的两个都要好。

## 关于词袋模型

*   优点：考虑到其简单的特性，词袋模型已经很强大了，它们训练速度快，且易于理解。

*   缺点：即使 `ngram` 带有一些单词间的语境，但词袋模型无法建模序列中单词间的长期依赖关系。

现在要用到深度学习模型了。深度学习模型的表现优于词袋模型是因为深度学习模型能够捕捉到句子中单词间的顺序依赖关系。这可能要归功于循环神经网络这一特殊神经网络结构的出现了。

本文并未涵盖 `RNN` 的理论基础，但该链接（http://colah.github.io/posts/2015-08-Understanding-LSTMs/）中的内容值得一读。这篇文章来源于 Cristopher Olah 的博客，详细叙述了一种特殊的 `RNN` 模型：长短期记忆网络（`LSTM`）。

在开始之前，要先设置一个深度学习专用的环境，以便在 `TensorFlow` 上使用 `Keras`。诚实地讲，我试着在个人笔记本上运行这些代码，但考虑到数据集的大小和 `RNN` 架构的复杂程度，这是很不实际的。还有一个很好的选择是 `AWS`。我一般在 `EC2 p2.xlarge` 实例上用深度学习 `AMI`（https://aws.amazon.com/marketplace/pp/B077GCH38C?qid=1527197041958&sr=0-1&ref_=srh_res_product_title）。亚马逊 `AMI` 是安装了所有包（`TensorFlow`、`PyTorch` 和 `Keras` 等）的预先配置过的 VM 图。强烈推荐大家使用！

```py
from keras.preprocessing.text import Tokenizer
from keras.preprocessing.text import text_to_word_sequence
from keras.preprocessing.sequence import pad_sequences

from keras.models import Model
from keras.models import Sequential

from keras.layers import Input, Dense, Embedding, Conv1D, Conv2D, MaxPooling1D, MaxPool2D
from keras.layers import Reshape, Flatten, Dropout, Concatenate
from keras.layers import SpatialDropout1D, concatenate
from keras.layers import GRU, Bidirectional, GlobalAveragePooling1D, GlobalMaxPooling1D

from keras.callbacks import Callback
from keras.optimizers import Adam

from keras.callbacks import ModelCheckpoint, EarlyStopping
from keras.models import load_model
from keras.utils.vis_utils import plot_model
```

## 4. 没有预训练词嵌入的循环神经网络

`RNN` 可能看起来很可怕。尽管它们因为复杂而难以理解，但非常有趣。`RNN` 模型封装了一个非常漂亮的设计，以克服传统神经网络在处理序列数据（文本、时间序列、视频、DNA 序列等）时的短板。

`RNN` 是一系列神经网络的模块，它们彼此连接像锁链一样。每一个都将消息向后传递。强烈推荐大家从 Colah 的博客中深入了解它的内部机制，下面的图就来源于此。

![](img/3b9425c3e0a189c80a567f911b9819f3-fs8.png)

我们要处理的序列类型是文本数据。对意义而言，单词顺序很重要。`RNN` 考虑到了这一点，它可以捕捉长期依赖关系。

为了在文本数据上使用 `Keras`，我们首先要对数据进行预处理。可以用 `Keras` 的 `Tokenizer` 类。该对象用 `num_words` 作为参数，`num_words` 是根据词频进行分词后保留下来的最大词数。

```py
MAX_NB_WORDS = 80000
tokenizer = Tokenizer(num_words=MAX_NB_WORDS)

tokenizer.fit_on_texts(data['cleaned_text'])
```

当分词器适用于数据时，我们就可以用分词器将文本字符级 ngram 转换为数字序列。

这些数字表示每个单词在字典中的位置（将其视为映射）。

如下例所示：

```py
x_train[15]
'breakfast time happy time'
```

这里说明了分词器是如何将其转换为数字序列的。

```py
tokenizer.texts_to_sequences([x_train[15]])
[[530, 50, 119, 50]]
```

接下来在训练序列和测试序列中应用该分词器：

```py
train_sequences = tokenizer.texts_to_sequences(x_train)
test_sequences = tokenizer.texts_to_sequences(x_test)
```

将推文映射到整数列表中。但是由于长度不同，还是没法将它们在矩阵中堆叠在一起。还好 `Keras` 允许用 0 将序列填充至最大长度。我们将这个长度设置为 35（这是推文中的最大分词数）。

```py
MAX_LENGTH = 35
padded_train_sequences = pad_sequences(train_sequences, maxlen=MAX_LENGTH)
padded_test_sequences = pad_sequences(test_sequences, maxlen=MAX_LENGTH)
padded_train_sequences
array([[    0,     0,     0, ...,  2383,   284,      9],
       [    0,     0,     0, ...,   13,    30,     76],
       [    0,     0,     0, ...,   19,    37,  45231],
       ...,
       [    0,     0,     0, ...,   43,   502,  1653],
       [    0,     0,     0, ...,    5,  1045,   890],
       [    0,     0,     0, ..., 13748, 38750,   154]])
padded_train_sequences.shape
(1417523, 35)
```

现在就可以将数据传入 `RNN` 了。

以下是我将使用的架构的一些元素：

*   嵌入维度为 300。这意味着我们使用的 8 万个单词中的每一个都被映射至 300 维的密集（浮点数）向量。该映射将在训练过程中进行调整。

*   在嵌入层上应用 `SpatialDropout1D` 层以减少过拟合：按批次查看 35*300 的矩阵，随机删除每个矩阵中（设置为 0）的词向量（行）。这有助于将注意力不集中在特定的词语上，有利于模型的泛化。

*   双向门控循环单元（`GRU`）：这是循环网络部分。这是 `LSTM` 架构更快的变体。将其视为两个循环网络的组合，这样就可以从两个方向同时扫描文本序列：从左到右和从右到左。这使得网络在阅读给定单词时，可以结合之前和之后的内容理解文本。`GRU` 中每个网络块的输出 `h_t` 的维度即单元数，将这个值设置为 100。由于用了双向 `GRU`，因此每个 `RNN` 块的最终输出都是 200 维的。

*   双向 `GRU` 的输出是有维度的（批尺寸、时间步和单元）。这意味着如果用的是经典的 256 的批尺寸，维度将会是 (256, 35, 200)。

*   在每个批次上应用的是全局平均池化，其中包含了每个时间步（即单词）对应的输出向量的平均值。

*   我们应用了相同的操作，只是用最大池化替代了平均池化。

*   将前两个操作的输出连接在了一起。

```py
def get_simple_rnn_model():
    embedding_dim = 300
    embedding_matrix = np.random.random((MAX_NB_WORDS, embedding_dim))

    inp = Input(shape=(MAX_LENGTH, ))
    x = Embedding(input_dim=MAX_NB_WORDS, output_dim=embedding_dim, input_length=MAX_LENGTH, 
                  weights=[embedding_matrix], trainable=True)(inp)
    x = SpatialDropout1D(0.3)(x)
    x = Bidirectional(GRU(100, return_sequences=True))(x)
    avg_pool = GlobalAveragePooling1D()(x)
    max_pool = GlobalMaxPooling1D()(x)
    conc = concatenate([avg_pool, max_pool])
    outp = Dense(1, activation="sigmoid")(conc)

    model = Model(inputs=inp, outputs=outp)
    model.compile(loss='binary_crossentropy',
                  optimizer='adam',
                  metrics=['accuracy'])
    return model

rnn_simple_model = get_simple_rnn_model()
```

该模型的不同层如下所示：

```py
plot_model(rnn_simple_model, 
           to_file='./images/article_5/rnn_simple_model.png', 
           show_shapes=True, 
           show_layer_names=True)
```

![](img/ab18923040393cc9f8e767693933754f-fs8.png)

在训练期间使用了模型检查点。这样可以在每个 epoch 的最后将最佳模型（可以用准确率度量）自动存储（在硬盘上）。

```py
filepath="./models/rnn_no_embeddings/weights-improvement-{epoch:02d}-{val_acc:.4f}.hdf5"
checkpoint = ModelCheckpoint(filepath, monitor='val_acc', verbose=1, save_best_only=True, mode='max')

batch_size = 256
epochs = 2

history = rnn_simple_model.fit(x=padded_train_sequences, 
                               y=y_train, 
                               validation_data=(padded_test_sequences, y_test), 
                               batch_size=batch_size, 
                               callbacks=[checkpoint], 
                               epochs=epochs, 
                               verbose=1)

best_rnn_simple_model = load_model('./models/rnn_no_embeddings/weights-improvement-01-0.8262.hdf5')

y_pred_rnn_simple = best_rnn_simple_model.predict(padded_test_sequences, verbose=1, batch_size=2048)

y_pred_rnn_simple = pd.DataFrame(y_pred_rnn_simple, columns=['prediction'])
y_pred_rnn_simple['prediction'] = y_pred_rnn_simple['prediction'].map(lambda p: 1 if p >= 0.5 else 0)
y_pred_rnn_simple.to_csv('./predictions/y_pred_rnn_simple.csv', index=False)
y_pred_rnn_simple = pd.read_csv('./predictions/y_pred_rnn_simple.csv')
print(accuracy_score(y_test, y_pred_rnn_simple))
0.826219183127
```

准确率达到了 82.6%！这真是很不错的结果了！现在的模型表现已经比之前的词袋模型更好了，因为我们将文本的序列性质考虑在内了。

还能做得更好吗？

## 5. 用 GloVe 预训练词嵌入的循环神经网络

在最后一个模型中，嵌入矩阵被随机初始化了。那么如果用预训练过的词嵌入对其进行初始化又当如何呢？举个例子：假设在语料库中有「pizza」这个词。遵循之前的架构对其进行初始化后，可以得到一个 300 维的随机浮点值向量。这当然是很好的。这很好实现，而且这个嵌入可以在训练过程中进行调整。但你还可以使用在很大的语料库上训练出来的另一个模型，为「pizza」生成词嵌入来代替随机选择的向量。这是一种特殊的迁移学习。

使用来自外部嵌入的知识可以提高 `RNN` 的精度，因为它整合了这个单词的相关新信息（词汇和语义），而这些信息是基于大规模数据语料库训练和提炼出来的。

我们使用的预训练嵌入是 `GloVe`。

官方描述是这样的：`GloVe` 是一种获取单词向量表征的无监督学习算法。该算法的训练基于语料库全局词-词共现数据，得到的表征展示出词向量空间有趣的线性子结构。

本文使用的 `GloVe` 嵌入的训练数据是数据量很大的网络抓取，包括：

*   8400 亿个分词；
*   220 万词。

下载压缩文件要 2.03GB。请注意，该文件无法轻松地加载在标准笔记本电脑上。

`GloVe` 嵌入有 300 维。

`GloVe` 嵌入来自原始文本数据，在该数据中每一行都包含一个单词和 300 个浮点数（对应嵌入）。所以首先要将这种结构转换为 Python 字典。

```py
def get_coefs(word, *arr):
    try:
        return word, np.asarray(arr, dtype='float32')
    except:
        return None, None

embeddings_index = dict(get_coefs(*o.strip().split()) for o in tqdm_notebook(open('./embeddings/glove.840B.300d.txt')))

embed_size = 300
for k in tqdm_notebook(list(embeddings_index.keys())):
    v = embeddings_index[k]
    try:
        if v.shape != (embed_size, ):
            embeddings_index.pop(k)
    except:
        pass

embeddings_index.pop(None)
```

一旦创建了嵌入索引，我们就可以提取所有的向量，将其堆叠在一起并计算它们的平均值和标准差。

```py
values = list(embeddings_index.values())
all_embs = np.stack(values)

emb_mean, emb_std = all_embs.mean(), all_embs.std()
```
