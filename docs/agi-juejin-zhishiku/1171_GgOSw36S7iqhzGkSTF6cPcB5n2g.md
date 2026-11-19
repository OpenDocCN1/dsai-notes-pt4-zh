
# 简单 Transformer 模型与文本分类（Keras & PyTorch）

> 来源：[https://agijuejin.feishu.cn/docx/LOEMddyCWoIyZuxX23IccYqYnnf](https://agijuejin.feishu.cn/docx/LOEMddyCWoIyZuxX23IccYqYnnf)

> 作者：王几行XING

> 原文：[https://zhuanlan.zhihu.com/p/705334174](https://zhuanlan.zhihu.com/p/705334174)

## 案例简介

此 Keras 官方案例展示了如何使用`Transformer`模型来进行文本分类任务。`Transformer`模型是由Vaswani等人在2017年提出的，用于处理自然语言处理任务的强大模型架构。

案例网址：[Keras documentation: Text classification with Transformer](https://link.zhihu.com/?target=https%3A//keras.io/examples/nlp/text_classification_with_transformer/)

### 数据集准备

+ 使用IMDb数据集进行情感分类任务。该数据集包含50000条电影评论，分别标记为正面或负面情感。

+ 数据集分为训练集和测试集，每个部分包含25000条评论。

+ 文本数据被分词并转化为整数序列，每个整数表示词汇表中的一个词。

### 模型构建

+ 构建一个包含自定义`TransformerBlock`的模型，用于处理文本输入并进行分类。

+ `TransformerBlock`包括多头注意力层、前馈神经网络层、层归一化和`Dropout`层。

+ 使用嵌入层将输入文本序列转化为词向量表示。

+ 使用全局平均池化层将`Transformer`的输出转换为固定大小的向量。

+ 添加全连接层和`Softmax`激活函数进行二分类。

### 模型编译和训练

+ 使用`Adam`优化器和交叉熵损失函数进行模型编译。

+ 模型在训练集上进行训练，并在验证集上监控性能。

+ 使用`EarlyStopping`回调函数，在验证性能不再提升时提前停止训练。

### 模型评估

+ 在测试集上评估模型性能，计算准确率等指标。

## 第一步，环境准备

```
!pip install -q -U keras
!pip install -q -U tensorflow
## Setup
import keras
import tensorflow as tf
from keras import layers
```

## 第二步，数据准备

```
vocab_size = 20000
maxlen = 200
(x_train, y_train), (x_val, y_val) = keras.datasets.imdb.load_data(num_words=vocab_size)

print(len(x_train), 'Training sequences')
print(len(x_val), 'Validation sequences')
x_train = keras.utils.pad_sequences(x_train, maxlen=maxlen)
x_val = keras.utils.pad_sequences(x_val, maxlen=maxlen)

# Downloading data from https://storage.googleapis.com/tensorflow/tf-keras-datasets/imdb.npz
# 17464789/17464789 ━━━━━━━━━━━━━━━━━━━━ 0s 0us/step
# 25000 Training sequences
# 25000 Validation sequences
```

查看一下数据：

![](img/65acad91e270c16f984ef34837110c23.png)

说明这里的数据已经完成了`tokenized`，而不是原始的文本数据。

## 第三步，定义`Transformer`层

```
# 从 TensorFlow 导入必要的模块
from tensorflow.keras import layers
from tensorflow import keras

# 定义自定义层 TransformerBlock，继承自 layers.Layer
class TransformerBlock(layers.Layer):
    # 构造函数，用于初始化 TransformerBlock
    def __init__(self, embed_dim, num_heads, ff_dim):
        super(TransformerBlock, self).__init__()  # 调用父类（layers.Layer）的构造函数

        # 初始化多头注意力层
        self.att = layers.MultiHeadAttention(num_heads=num_heads, key_dim=embed_dim)

        # 初始化前馈神经网络 (FFN) 作为一个顺序模型
        self.ffn = keras.Sequential([
            layers.Dense(ff_dim, activation="relu"),  # 第一层 Dense，使用 ReLU 激活函数
            layers.Dense(embed_dim),  # 第二层 Dense，没有激活函数
        ])

        # 初始化两个层归一化层，用于正则化输入
        self.layernorm1 = layers.LayerNormalization(epsilon=1e-6)
        self.layernorm2 = layers.LayerNormalization(epsilon=1e-6)

        # 初始化两个 Dropout 层，用于防止过拟合
        self.dropout1 = layers.Dropout(0.1)  # Dropout 比率为 0.1
        self.dropout2 = layers.Dropout(0.1)  # Dropout 比率为 0.1

    # 定义前向传播方法 call
    def call(self, inputs, training):
        # 应用多头注意力层于输入数据
        attn_output = self.att(inputs, inputs)

        # 在注意力输出上应用 Dropout，training 参数用于指示是否在训练模式下运行
        attn_output = self.dropout1(attn_output, training=training)

        # 将原始输入与注意力输出相加，然后进行层归一化
        out1 = self.layernorm1(inputs + attn_output)

        # 将归一化后的输出传入前馈神经网络
        ffn_output = self.ffn(out1)

        # 在前馈网络输出上应用 Dropout
        ffn_output = self.dropout2(ffn_output, training=training)

        # 最后将前馈网络的输出与归一化后的中间输出相加，再进行层归一化，得到最终输出
        return self.layernorm2(out1 + ffn_output)
```

## 第四步，模型定义

```
# 定义输入的嵌入维度、注意力头数和前馈网络的隐藏层大小
embed_dim = 32  # 每个 token 的嵌入大小
num_heads = 2  # 注意力头的数量
ff_dim = 32  # 前馈神经网络中的隐藏层大小

# 假设 maxlen 和 vocab_size 是已定义的变量，表示输入序列的最大长度和词汇表大小
maxlen = 100  # 输入序列的最大长度
vocab_size = 20000  # 词汇表大小

# 定义输入层，输入形状为 (maxlen,)，表示每个输入序列的长度
inputs = layers.Input(shape=(maxlen,))  # 表示输入是一个长度为 maxlen 的整数序列。每个整数通常是一个词在词汇表中的索引

# 定义自定义的嵌入层和位置嵌入层
# 假设 TokenAndPositionEmbedding 是一个已定义的类，用于嵌入 tokens 和位置
embedding_layer = TokenAndPositionEmbedding(maxlen, vocab_size, embed_dim)

# 将输入传递给嵌入层
x = embedding_layer(inputs)

# 定义 TransformerBlock，并将嵌入后的输入传递给它
transformer_block = TransformerBlock(embed_dim, num_heads, ff_dim)
x = transformer_block(x, training=True)  # 传递 training 参数以指示处于训练模式

# 全局平均池化层，将序列的每个时间步的表示取平均值
x = layers.GlobalAveragePooling1D()(x)

# Dropout 层，防止过拟合
x = layers.Dropout(0.1)(x)

# 全连接层，使用 ReLU 激活函数
x = layers.Dense(20, activation="relu")(x)

# 再次使用 Dropout 层
x = layers.Dropout(0.1)(x)

# 输出层，使用 softmax 激活函数，将输出分类为两个类别
outputs = layers.Dense(2, activation="softmax")(x)

# 创建 Keras 模型，指定输入和输出
model = keras.Model(inputs=inputs, outputs=outputs)

# 打印模型摘要
model.summary()
```

![](img/c327db9b48ff85e35cf134570c75e206.png)

### 代码解释

#### 定义超参数

+ `embed_dim = 32`：每个 `token` 的嵌入向量的维度。

+ `num_heads = 2`：多头注意力机制中的头数。

+ `ff_dim = 32`：前馈神经网络中的隐藏层维度。

#### 定义输入层

+ `inputs = layers.Input(shape=(maxlen,))`：定义输入层，输入形状为 `(maxlen,)`，表示每个输入序列的长度。输入是一个整数序列，每个整数通常是一个词在词汇表中的索引。

#### 定义嵌入层

+ `embedding_layer = TokenAndPositionEmbedding(maxlen, vocab_size, embed_dim)`：`TokenAndPositionEmbedding` 是一个假设已定义的类，用于将 `token` 和位置进行嵌入。这个层会将整数序列转换为嵌入向量，并添加位置信息。

#### 将输入传递给嵌入层

+ `x = embedding_layer(inputs)`：将输入传递给嵌入层，得到嵌入后的向量表示。

#### 定义 `TransformerBlock`

+ `transformer_block = TransformerBlock(embed_dim, num_heads, ff_dim)`：初始化一个 `TransformerBlock`，其中包含多头注意力和前馈神经网络。

+ `x = transformer_block(x, training=True)`：将嵌入后的输入传递给 `TransformerBlock`，并指定 `training=True` 表示在训练模式下运行。

#### 全局平均池化层

+ `x = layers.GlobalAveragePooling1D()(x)`：应用全局平均池化，将序列的每个时间步的表示取平均值，得到一个固定大小的向量。

#### `Dropout` 层

+ `x = layers.Dropout(0.1)(x)`：应用 `Dropout`，防止过拟合。`Dropout` 比率为 0.1，表示 10% 的节点会随机失活。

#### 全连接层

+ `x = layers.Dense(20, activation="relu")(x)`：应用一个全连接层，使用 `ReLU` 激活函数，输出维度为 20。

#### 再次应用 `Dropout` 层

+ `x = layers.Dropout(0.1)(x)`：再次应用 `Dropout`，防止过拟合。

#### 输出层

+ `outputs = layers.Dense(2, activation="softmax")(x)`：应用一个全连接层，使用 `softmax` 激活函数，输出维度为 2。`softmax` 会将输出转换为两个类别的概率分布。

#### 创建 Keras 模型

+ `model = keras.Model(inputs=inputs, outputs=outputs)`：使用输入层和输出层创建 Keras 模型。

#### 打印模型摘要

+ `model.summary()`：打印模型摘要，显示模型的结构和参数数量。

## 第五步，模型训练

```
# 编译模型
model.compile(
    optimizer='adam',  # 使用 Adam 优化器
    loss="sparse_categorical_crossentropy",  # 使用稀疏分类交叉熵作为损失函数
    metrics=['accuracy']  # 评估指标选择准确率
)

# 训练模型
history = model.fit(
    x_train,  # 训练数据，假设已经准备好
    y_train,  # 训练标签，假设已经准备好
    batch_size=32,  # 每个批次的样本数
    epochs=2,  # 训练整个数据集的次数（轮数）
    validation_data=(x_val, y_val)  # 验证数据和验证标签，用于在每个 epoch 结束时评估模型
)
```

### 代码解释

#### 编译模型

+ `optimizer='adam'`：使用 `Adam` 优化器，它是一种常用的优化算法，能够高效地调整模型的权重以最小化损失函数。

+ `loss="sparse_categorical_crossentropy"`：使用稀疏分类交叉熵作为损失函数。适用于标签是整数而不是 one-hot 编码的分类问题。

+ `metrics=['accuracy']`：评估模型性能的指标选择准确率，表示模型预测的准确程度。

#### 训练模型

+ `x_train`：训练数据，假设已经向量化并准备好。

+ `y_train`：训练标签，表示每个样本的真实分类。

+ `batch_size=32`：每个批次的样本数。批量梯度下降法会在每次迭代中使用 32 个样本。

+ `epochs=2`：训练整个数据集的次数。在每个 `epoch` 结束时，模型会遍历一次全部训练数据。

+ `validation_data=(x_val, y_val)`：验证数据和验证标签。在每个 `epoch` 结束时，模型会在验证数据上评估其性能，帮助监控模型是否过拟合。

### 进一步说明

#### `Adam` 优化器

`Adam`（Adaptive Moment Estimation）是一种自适应学习率优化算法。它结合了动量梯度下降和 `RMSProp` 的优点，能高效且稳定地调整学习率。

#### 稀疏分类交叉熵损失

当标签是整数（例如：类索引 0, 1, 2...）而不是 one-hot 编码时，使用 `sparse_categorical_crossentropy`。它比 `categorical_crossentropy` 更节省内存。

#### 训练过程

`model.fit` 方法将模型应用于训练数据，优化模型参数，使得损失函数最小化。通过验证数据集，可以在训练过程中监控模型的表现，避免过拟合。训练历史（`history`）包含了每个 `epoch` 的训练和验证损失及准确率，可以用来绘制训练曲线或进一步分析模型的表现。

## 完整代码 (未指定 training 模式)



```python
import tensorflow as tf
from keras import layers
```

## 1st layer for tokens
```python
class TransformerBlock(layers.Layer):
    def __init__(self, embed_dim, num_heads, ff_dim, rate=0.1):
        super().__init__()
        self.att = layers.MultiHeadAttention(num_heads=num_heads, key_dim=embed_dim)
        self.ffn = keras.Sequential(
            [layers.Dense(ff_dim, activation="relu"), layers.Dense(embed_dim),]
        )
        self.layernorm1 = layers.LayerNormalization(epsilon=1e-6)
        self.layernorm2 = layers.LayerNormalization(epsilon=1e-6)
        self.dropout1 = layers.Dropout(rate)
        self.dropout2 = layers.Dropout(rate)

    def call(self, inputs):
        attn_output = self.att(inputs, inputs)
        attn_output = self.dropout1(attn_output)
        out1 = self.layernorm1(inputs + attn_output)
        ffn_output = self.ffn(out1)
        ffn_output = self.dropout2(ffn_output)
        return self.layernorm2(out1 + ffn_output)
```

## 2nd layer for positions (index)
```python
class TokenAndPositionEmbedding(layers.Layer):
  def __init__(self, maxlen, vocab_size, embed_dim):
    super().__init__()
    self.token_emb = layers.Embedding(input_dim=vocab_size, output_dim=embed_dim)
    self.pos_emb = layers.Embedding(input_dim=maxlen, output_dim=embed_dim)

  def call(self, x):
    maxlen = tf.shape(x)[-1]
    positions = tf.range(start=0, limit=maxlen, delta=1)
    positions = self.pos_emb(positions)
    x = self.token_emb(x)
    return x+positions 
```

## Download the data
```python
vocab_size = 20000  # Only consider the top 20k words
maxlen = 200  # Only consider the first 200 words of each movie review
(x_train, y_train), (x_val, y_val) = keras.datasets.imdb.load_data(num_words=vocab_size)
print(len(x_train), "Training sequences")
print(len(x_val), "Validation sequences")
x_train = keras.utils.pad_sequences(x_train, maxlen=maxlen)
x_val = keras.utils.pad_sequences(x_val, maxlen=maxlen)
```

## Define the model
```python
embed_dim = 32  # Embedding size for each token
num_heads = 2  # Number of attention heads
ff_dim = 32  # Hidden layer size in feed forward network inside transformer

inputs = layers.Input(shape=(maxlen,))
embedding_layer = TokenAndPositionEmbedding(maxlen, vocab_size, embed_dim)
x = embedding_layer(inputs)
transformer_block = TransformerBlock(embed_dim, num_heads, ff_dim)
x = transformer_block(x)
x = layers.GlobalAveragePooling1D()(x)
x = layers.Dropout(0.1)(x)
x = layers.Dense(20, activation="relu")(x)
x = layers.Dropout(0.1)(x)
outputs = layers.Dense(2, activation="softmax")(x)

model = keras.Model(inputs=inputs, outputs=outputs)
model.summary()
```

## Train and evaluate the model
```python
model.compile(
    optimizer="adam", loss="sparse_categorical_crossentropy", metrics=["accuracy"]
)
history = model.fit(
    x_train, y_train, batch_size=32, epochs=2, validation_data=(x_val, y_val)
)
```

## PyTorch 版



```
import torch
import torch.nn as nn
import torch.nn.functional as F
from torch.utils.data import DataLoader, TensorDataset
from torch.nn.utils.rnn import pad_sequence

class TransformerBlock(nn.Module):
    def __init__(self, embed_dim, num_heads, ff_dim, dropout=0.1):
        super(TransformerBlock, self).__init__()
        self.att = nn.MultiheadAttention(embed_dim, num_heads)
        self.ffn = nn.Sequential(
            nn.Linear(embed_dim, ff_dim),
            nn.ReLU(),
            nn.Linear(ff_dim, embed_dim)
        )
        self.layernorm1 = nn.LayerNorm(embed_dim)
        self.layernorm2 = nn.LayerNorm(embed_dim)
        self.dropout1 = nn.Dropout(dropout)
        self.dropout2 = nn.Dropout(dropout)

    def forward(self, x):
        attn_output, _ = self.att(x, x, x)
        attn_output = self.dropout1(attn_output)
        out1 = self.layernorm1(x + attn_output)
        ffn_output = self.ffn(out1)
        ffn_output = self.dropout2(ffn_output)
        return self.layernorm2(out1 + ffn_output)

class TokenAndPositionEmbedding(nn.Module):
    def __init__(self, maxlen, vocab_size, embed_dim):
        super(TokenAndPositionEmbedding, self).__init__()
        self.token_emb = nn.Embedding(vocab_size, embed_dim)
        self.pos_emb = nn.Embedding(maxlen, embed_dim)

    def forward(self, x):
        maxlen = x.size(1)
        positions = torch.arange(0, maxlen, device=x.device).unsqueeze(0).expand_as(x)
        return self.token_emb(x) + self.pos_emb(positions)

# 下载数据并进行预处理
vocab_size = 20000  # 只考虑前 20k 词汇
maxlen = 200  # 只考虑每条评论的前 200 个词

from keras.datasets import imdb

(x_train, y_train), (x_val, y_val) = imdb.load_data(num_words=vocab_size)

# 填充序列并转换为 PyTorch 张量
x_train = [torch.tensor(seq, dtype=torch.long) for seq in x_train]
y_train = torch.tensor(y_train, dtype=torch.long)
x_val = [torch.tensor(seq, dtype=torch.long) for seq in x_val]
y_val = torch.tensor(y_val, dtype=torch.long)

x_train = pad_sequence(x_train, batch_first=True, padding_value=0)
x_val = pad_sequence(x_val, batch_first=True, padding_value=0)

x_train = x_train[:, :maxlen]
x_val = x_val[:, :maxlen]

train_dataset = TensorDataset(x_train, y_train)
val_dataset = TensorDataset(x_val, y_val)
train_loader = DataLoader(train_dataset, batch_size=32, shuffle=True)
val_loader = DataLoader(val_dataset, batch_size=32)

# 定义模型
class TransformerModel(nn.Module):
    def __init__(self, maxlen, vocab_size, embed_dim, num_heads, ff_dim):
        super(TransformerModel, self).__init__()
        self.embedding_layer = TokenAndPositionEmbedding(maxlen, vocab_size, embed_dim)
        self.transformer_block = TransformerBlock(embed_dim, num_heads, ff_dim)
        self.global_avg_pool = nn.AdaptiveAvgPool1d(1)
        self.dropout1 = nn.Dropout(0.1)
        self.dense1 = nn.Linear(embed_dim, 20)
        self.dropout2 = nn.Dropout(0.1)
        self.dense2 = nn.Linear(20, 2)
        
    def forward(self, x):
        x = self.embedding_layer(x).transpose(0, 1)  # Transformer expects (seq_len, batch_size, embed_dim)
        x = self.transformer_block(x)
        x = x.transpose(0, 1)
        x = self.global_avg_pool(x.permute(0, 2, 1)).squeeze(-1)
        x = self.dropout1(x)
        x = F.relu(self.dense1(x))
        x = self.dropout2(x)
        return F.log_softmax(self.dense2(x), dim=-1)

model = TransformerModel(maxlen, vocab_size, embed_dim=32, num_heads=2, ff_dim=32)
optimizer = torch.optim.Adam(model.parameters(), lr=0.001)
criterion = nn.CrossEntropyLoss()

# 训练和评估模型
def train_model(model, train_loader, val_loader, criterion, optimizer, epochs=2):
    device = torch.device('cuda' if torch.cuda.is_available() else 'cpu')
    model.to(device)
    
    for epoch in range(epochs):
        model.train()
        train_loss = 0
        correct = 0
        total = 0
        for inputs, targets in train_loader:
            inputs, targets = inputs.to(device), targets.to(device)
            optimizer.zero_grad()
            outputs = model(inputs)
            loss = criterion(outputs, targets)
            loss.backward()
            optimizer.step()
            train_loss += loss.item()
            _, predicted = torch.max(outputs, 1)
            total += targets.size(0)
            correct += predicted.eq(targets).sum().item()
        print(f'Epoch {epoch + 1}/{epochs}, Loss: {train_loss/len(train_loader)}, Accuracy: {100.*correct/total}%')
        
        model.eval()
        val_loss = 0
        correct = 0
        total = 0
        with torch.no_grad():
            for inputs, targets in val_loader:
                inputs, targets = inputs.to(device), targets.to(device)
                outputs = model(inputs)
                loss = criterion(outputs, targets)
                val_loss += loss.item()
                _, predicted = torch.max(outputs, 1)
                total += targets.size(0)
                correct += predicted.eq(targets).sum().item()
        print(f'Validation Loss: {val_loss/len(val_loader)}, Accuracy: {100.*correct/total}%')

train_model(model, train_loader, val_loader, criterion, optimizer, epochs=2)
```
