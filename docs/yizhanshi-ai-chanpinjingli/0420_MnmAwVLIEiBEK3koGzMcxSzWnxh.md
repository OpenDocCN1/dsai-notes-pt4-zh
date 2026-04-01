# TensorFlow 高级 API - Keras

> 来源：[https://v11enp9ok1h.feishu.cn/docx/UtqKdrZ2MorK6VxgFoccBH9Knof](https://v11enp9ok1h.feishu.cn/docx/UtqKdrZ2MorK6VxgFoccBH9Knof)

Keras 是一个用 Python 编写的高级神经网络 API，它能够以 TensorFlow, CNTK 或 Theano 作为后端运行。Keras 的设计理念是用户友好、模块化和易扩展。

### Keras 的主要特点

1.  **简单易用**：提供直观一致的接口，适合快速原型设计

1.  **模块化**：神经网络层、损失函数、优化器等都是可插拔的模块

1.  **易扩展**：可以轻松添加新模块来表达新的研究想法

1.  **支持多后端**：可以无缝运行在 TensorFlow, CNTK 或 Theano 上

---

## Keras 核心概念

### 模型 (Model)

Keras 的核心数据结构是模型，模型是组织神经网络层的方式。Keras 提供了两种主要的模型：

+   **Sequential 模型**：层的线性堆叠

+   **Functional API**：构建复杂模型的有向无环图

### 层 (Layer)

层是 Keras 的基本构建块，每个层接收输入数据，进行某种计算后输出结果。Keras 提供了多种预定义层：

+   核心层：Dense, Activation, Dropout 等

+   卷积层：Conv2D, MaxPooling2D 等

+   循环层：LSTM, GRU 等

+   其他：Embedding, BatchNormalization 等

### 激活函数 (Activation Function)

激活函数决定神经元的输出，常用的有：

+   ReLU (Rectified Linear Unit)

+   Sigmoid

+   Tanh

+   Softmax (多分类问题)

---

## Keras 基本工作流程

### 定义模型

## 实例

```
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense

model = Sequential([
    Dense(64, activation=&#x27;relu&#x27;, input_shape=(784,)),
    Dense(64, activation=&#x27;relu&#x27;),
    Dense(10, activation=&#x27;softmax&#x27;)
])
```

### 编译模型

## 实例

```
model.compile(optimizer=&#x27;adam&#x27;,
              loss=&#x27;categorical_crossentropy&#x27;,
              metrics=[&#x27;accuracy&#x27;])
```

### 训练模型

## 实例

```
model.fit(x_train, y_train, 
          epochs=5, 
          batch_size=32)
```

### 评估模型

## 实例

```
loss_and_metrics = model.evaluate(x_test, y_test, batch_size=128)
```

### 进行预测

## 实例

```
classes = model.predict(x_test, batch_size=128)
```

## Keras 常用层详解

### Dense 全连接层

## 实例

```
Dense(units, 
      activation=None, 
      use_bias=True, 
      kernel_initializer=&#x27;glorot_uniform&#x27;, 
      bias_initializer=&#x27;zeros&#x27;)
```

+   units：正整数，输出空间的维度

+   activation：激活函数

+   use_bias：是否使用偏置向量

+   kernel_initializer：权重矩阵的初始化器

+   bias_initializer：偏置向量的初始化器

### Conv2D 二维卷积层

## 实例

```
Conv2D(filters, 
       kernel_size, 
       strides=(1, 1), 
       padding=&#x27;valid&#x27;, 
       activation=None)
```

+   filters：卷积核的数目

+   kernel_size：卷积核的尺寸

+   strides：卷积步长

+   padding：填充方式 ('valid' 或 'same')

### LSTM 长短期记忆网络层

## 实例

```
LSTM(units, 
     activation=&#x27;tanh&#x27;, 
     recurrent_activation=&#x27;hard_sigmoid&#x27;, 
     return_sequences=False)
```

+   units：正整数，输出空间的维度

+   activation：激活函数

+   recurrent_activation：循环步的激活函数

+   return_sequences：是否返回完整序列

## Keras 模型保存与加载

### 保存整个模型

## 实例

```
model.save(&#x27;my_model.h5&#x27;)  # 保存架构、权重和训练配置
```

### 仅保存架构

## 实例

```
json_string = model.to_json()  # 保存为JSON
yaml_string = model.to_yaml()  # 保存为YAML
```

### 仅保存权重

## 实例

```
model.save_weights(&#x27;my_model_weights.h5&#x27;)
```

### 加载模型

## 实例

```
from tensorflow.keras.models import load_model

model = load_model(&#x27;my_model.h5&#x27;)  # 加载完整模型
```

## Keras 回调函数

回调函数是在训练过程中特定时间点被调用的函数，用于：

+   模型检查点

+   提前停止

+   学习率调整

+   日志记录等

### 常用回调函数

## 实例

```
from tensorflow.keras.callbacks import ModelCheckpoint, EarlyStopping

callbacks = [
    ModelCheckpoint(filepath=&#x27;best_model.h5&#x27;, monitor=&#x27;val_loss&#x27;, save_best_only=True),
    EarlyStopping(monitor=&#x27;val_loss&#x27;, patience=3)
]

model.fit(x_train, y_train, 
          epochs=10, 
          callbacks=callbacks, 
          validation_data=(x_val, y_val))
```

## Keras 实践示例：MNIST 手写数字识别

## 实例

```
from tensorflow.keras.datasets import mnist
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense, Dropout, Flatten
from tensorflow.keras.layers import Conv2D, MaxPooling2D
from tensorflow.keras.utils import to_categorical

# 加载数据
(x_train, y_train), (x_test, y_test) = mnist.load_data()

# 数据预处理
x_train = x_train.reshape(60000, 28, 28, 1).astype(&#x27;float32&#x27;) / 255
x_test = x_test.reshape(10000, 28, 28, 1).astype(&#x27;float32&#x27;) / 255
y_train = to_categorical(y_train, 10)
y_test = to_categorical(y_test, 10)

# 构建模型
model = Sequential([
    Conv2D(32, kernel_size=(3, 3), activation=&#x27;relu&#x27;, input_shape=(28, 28, 1)),
    Conv2D(64, (3, 3), activation=&#x27;relu&#x27;),
    MaxPooling2D(pool_size=(2, 2)),
    Dropout(0.25),
    Flatten(),
    Dense(128, activation=&#x27;relu&#x27;),
    Dropout(0.5),
    Dense(10, activation=&#x27;softmax&#x27;)
])

# 编译模型
model.compile(loss=&#x27;categorical_crossentropy&#x27;,
              optimizer=&#x27;adam&#x27;,
              metrics=[&#x27;accuracy&#x27;])

# 训练模型
model.fit(x_train, y_train,
          batch_size=128,
          epochs=12,
          verbose=1,
          validation_data=(x_test, y_test))

# 评估模型
score = model.evaluate(x_test, y_test, verbose=0)
print(&#x27;Test loss:&#x27;, score[0])
print(&#x27;Test accuracy:&#x27;, score[1])
```

## Keras 进阶技巧

### 自定义层

## 实例

```
from tensorflow.keras import backend as K
from tensorflow.keras.layers import Layer

class MyLayer(Layer):
    def __init__(self, output_dim, **kwargs):
        self.output_dim = output_dim
        super(MyLayer, self).__init__(**kwargs)
    
    def build(self, input_shape):
        self.kernel = self.add_weight(name=&#x27;kernel&#x27;,
                                     shape=(input_shape[1], self.output_dim),
                                     initializer=&#x27;uniform&#x27;,
                                     trainable=True)
        super(MyLayer, self).build(input_shape)
    
    def call(self, x):
        return K.dot(x, self.kernel)
    
    def compute_output_shape(self, input_shape):
        return (input_shape[0], self.output_dim)
```

### 自定义损失函数

## 实例

```
**from**** tenso**rflow.ker**as**** im**port** backe**nd as K

def custom_loss(y_true, y_pred):
    return K.mean(K.square(y_pred - y_true), axis=-1)

model.compile(optimizer=&#x27;adam&#x27;, loss=custom_loss)
```

### 学习率调度

## 实例

```
from tensorflow.keras.callbacks import LearningRateScheduler

def scheduler(epoch, lr):
    if epoch &lt; 10:
        return lr
    else:
        return lr * K.exp(-0.1)

callback = LearningRateScheduler(scheduler)
model.fit(x_train, y_train, epochs=15, callbacks=[callback])
```

## Keras 常见问题与解决方案

1.  过拟合问题

+   增加 Dropout 层

+   使用 L1/L2 正则化

+   增加训练数据

+   使用数据增强

1.  训练速度慢

+   增加批量大小

+   使用更简单的模型

+   尝试不同的优化器

+   使用 GPU 加速

1.  梯度消失/爆炸

+   使用 BatchNormalization

+   使用适当的权重初始化

+   使用 ReLU 等非饱和激活函数

+   使用梯度裁剪

---

## 总结

Keras 作为 TensorFlow 的高级 API，提供了简单直观的接口来构建和训练深度学习模型。通过本文，你应该已经掌握了：

1.  Keras 的核心概念和基本工作流程

1.  常用层的使用方法

1.  模型的保存与加载

1.  回调函数的使用

1.  实际项目中的应用示例

1.  进阶技巧和常见问题解决方案

Keras 的强大之处在于它的灵活性和易用性，使得深度学习模型的开发变得更加高效。随着实践的深入，你将能够构建更加复杂的神经网络模型来解决各种实际问题。