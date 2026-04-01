# TensorFlow 实例 - 文本分类项目

> 来源：[https://v11enp9ok1h.feishu.cn/docx/CjuTdVKa5o8qFpxNr6Tc5vdxnOQ](https://v11enp9ok1h.feishu.cn/docx/CjuTdVKa5o8qFpxNr6Tc5vdxnOQ)

文本分类是自然语言处理(NLP)中的基础任务，指将文本文档自动分类到一个或多个预定义类别中。在实际应用中，文本分类被广泛用于：

+   垃圾邮件检测

+   情感分析

+   新闻分类

+   客服对话分类

+   产品评论分类

使用TensorFlow实现文本分类通常包含以下步骤：

1.  数据准备与预处理

1.  文本向量化

1.  模型构建

1.  模型训练

1.  模型评估

1.  模型部署

---

## 环境准备

在开始项目前，请确保已安装以下Python库：

```
!pip install tensorflow
!pip install numpy
!pip install pandas
!pip install matplotlib
```

导入必要的库：

## 实例

```
import tensorflow as tf
from tensorflow.keras import layers
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
```

检查TensorFlow版本：

## 实例

```
print(tf.__version__)
# 输出示例：2.8.0
```

## 数据集准备

我们将使用IMDB电影评论数据集，这是一个经典的二分类数据集，包含50,000条电影评论，标记为正面(1)或负面(0)评价。

### 加载数据集

## 实例

```
# 从TensorFlow数据集加载IMDB数据
imdb = tf.keras.datasets.imdb

# 只保留前10000个最常出现的单词
(train_data, train_labels), (test_data, test_labels) = imdb.load_data(num_words=10000)
```

### 数据探索

查看数据格式：

## 实例

```
print(&quot;训练样本数: {}, 测试样本数: {}&quot;.format(len(train_data), len(test_data)))
# 输出：训练样本数: 25000, 测试样本数: 25000

# 查看第一条评论
print(train_data[0])
# 输出：[1, 14, 22, 16, 43, 530, 973, 1622, 1385, 65, ...]
```

### 数据预处理

将整数序列转换为多热编码：

## 实例

```
def vectorize_sequences(sequences, dimension=10000):
    results = np.zeros((len(sequences), dimension))
    for i, sequence in enumerate(sequences):
        results[i, sequence] = 1.
    return results

x_train = vectorize_sequences(train_data)
x_test = vectorize_sequences(test_data)

# 将标签转换为浮点数
y_train = np.asarray(train_labels).astype(&#x27;float32&#x27;)
y_test = np.asarray(test_labels).astype(&#x27;float32&#x27;)
```

## 构建模型

### 模型架构

我们将构建一个简单的全连接神经网络：

## 实例

```
model = tf.keras.Sequential([
    layers.Dense(16, activation=&#x27;relu&#x27;, input_shape=(10000,)),
    layers.Dense(16, activation=&#x27;relu&#x27;),
    layers.Dense(1, activation=&#x27;sigmoid&#x27;)
])
```

### 模型编译

## 实例

```
model.compile(optimizer=&#x27;rmsprop&#x27;,
              loss=&#x27;binary_crossentropy&#x27;,
              metrics=[&#x27;accuracy&#x27;])
```

参数说明：

+   optimizer: 优化器，控制学习过程

+   loss: 损失函数，衡量模型预测与真实标签的差异

+   metrics: 评估指标，监控训练和测试步骤

## 训练模型

### 创建验证集

## 实例

```
x_val = x_train[:10000]
partial_x_train = x_train[10000:]
y_val = y_train[:10000]
partial_y_train = y_train[10000:]
```

### 训练过程

## 实例

```
history = model.fit(partial_x_train,
                    partial_y_train,
                    epochs=20,
                    batch_size=512,
                    validation_data=(x_val, y_val))
```

### 训练结果可视化

## 实例

```
history_dict = history.history

# 绘制训练损失和验证损失
plt.plot(history_dict[&#x27;loss&#x27;], &#x27;bo&#x27;, label=&#x27;Training loss&#x27;)
plt.plot(history_dict[&#x27;val_loss&#x27;], &#x27;b&#x27;, label=&#x27;Validation loss&#x27;)
plt.title(&#x27;Training and validation loss&#x27;)
plt.xlabel(&#x27;Epochs&#x27;)
plt.ylabel(&#x27;Loss&#x27;)
plt.legend()
plt.show()

# 绘制训练准确率和验证准确率
plt.plot(history_dict[&#x27;accuracy&#x27;], &#x27;bo&#x27;, label=&#x27;Training acc&#x27;)
plt.plot(history_dict[&#x27;val_accuracy&#x27;], &#x27;b&#x27;, label=&#x27;Validation acc&#x27;)
plt.title(&#x27;Training and validation accuracy&#x27;)
plt.xlabel(&#x27;Epochs&#x27;)
plt.ylabel(&#x27;Accuracy&#x27;)
plt.legend()
plt.show()
```

## 模型评估与预测

### 评估测试集性能

## 实例

```
results = model.evaluate(x_test, y_test)
print(results)
# 输出示例：[0.3245, 0.8732] 表示损失和准确率
```

### 进行预测

## 实例

```
predictions = model.predict(x_test)
print(predictions[0])  # 第一条测试样本的预测概率
```

## 模型优化建议

1.  **调整网络架构**：

+   增加或减少隐藏层数量

+   尝试不同的神经元数量

+   使用不同的激活函数

1.  **正则化技术**：

+   添加Dropout层防止过拟合

+   使用L1/L2正则化

1.  **优化器选择**：

+   尝试Adam、SGD等其他优化器

+   调整学习率

1.  **文本预处理改进**：

+   使用词嵌入(Embedding)代替多热编码

+   尝试预训练词向量(如Word2Vec, GloVe)

## 完整代码示例

## 实例

```
import tensorflow as tf
from tensorflow.keras import layers
import numpy as np
import matplotlib.pyplot as plt

# 加载数据
imdb = tf.keras.datasets.imdb
(train_data, train_labels), (test_data, test_labels) = imdb.load_data(num_words=10000)

# 数据预处理
def vectorize_sequences(sequences, dimension=10000):
    results = np.zeros((len(sequences), dimension))
    for i, sequence in enumerate(sequences):
        results[i, sequence] = 1.
    return results

x_train = vectorize_sequences(train_data)
x_test = vectorize_sequences(test_data)
y_train = np.asarray(train_labels).astype(&#x27;float32&#x27;)
y_test = np.asarray(test_labels).astype(&#x27;float32&#x27;)

# 构建模型
model = tf.keras.Sequential([
    layers.Dense(16, activation=&#x27;relu&#x27;, input_shape=(10000,)),
    layers.Dense(16, activation=&#x27;relu&#x27;),
    layers.Dense(1, activation=&#x27;sigmoid&#x27;)
])

# 编译模型
model.compile(optimizer=&#x27;rmsprop&#x27;,
              loss=&#x27;binary_crossentropy&#x27;,
              metrics=[&#x27;accuracy&#x27;])

# 训练模型
history = model.fit(x_train, y_train,
                   epochs=4,
                   batch_size=512,
                   validation_data=(x_test, y_test))

# 评估模型
results = model.evaluate(x_test, y_test)
print(&quot;测试损失和准确率:&quot;, results)

# 进行预测
predictions = model.predict(x_test)
print(&quot;第一条评论的预测概率:&quot;, predictions[0])
```