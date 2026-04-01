# TensorFlow 模型保存与加载

> 来源：[https://v11enp9ok1h.feishu.cn/docx/DKuXdsR4XoTHOVxSAjAcZUHinqg](https://v11enp9ok1h.feishu.cn/docx/DKuXdsR4XoTHOVxSAjAcZUHinqg)

在机器学习和深度学习项目中，模型的保存与加载是至关重要的环节。

TensorFlow 提供了多种方式来保存和恢复模型，使开发者能够：

+   保存训练好的模型供后续使用

+   分享模型给其他开发者

+   从检查点恢复训练

+   部署模型到生产环境

TensorFlow 2.x 主要支持三种模型保存格式：

1.  SavedModel 格式（推荐）

1.  HDF5 格式（.h5）

1.  旧版 Keras 格式

## 保存整个模型

### SavedModel 格式

SavedModel 是 TensorFlow 推荐的模型保存格式，它包含完整的模型信息：

## 实例

```
import tensorflow as tf

# 创建并训练一个简单模型
model = tf.keras.Sequential([
    tf.keras.layers.Dense(64, activation=&#x27;relu&#x27;),
    tf.keras.layers.Dense(10, activation=&#x27;softmax&#x27;)
])
model.compile(optimizer=&#x27;adam&#x27;,
              loss=&#x27;sparse_categorical_crossentropy&#x27;,
              metrics=[&#x27;accuracy&#x27;])
model.fit(x_train, y_train, epochs=5)

# 保存为SavedModel格式
model.save(&#x27;my_model&#x27;)  # 注意：没有文件扩展名
```

保存后的目录结构：

```
my_model/
├── assets/
├── variables/
│   ├── variables.data-00000-of-00001
│   └── variables.index
└── saved_model.pb
```

### HDF5 格式

HDF5 是另一种常用的模型保存格式：

## 实例

```
# 保存为HDF5格式
model.save(&#x27;my_model.h5&#x27;)  # 注意.h5扩展名
```

### 两种格式的区别

## 加载整个模型

### 从 SavedModel 加载

## 实例

```
# 从SavedModel加载
loaded_model = tf.keras.models.load_model(&#x27;my_model&#x27;)

# 验证模型
loss, acc = loaded_model.evaluate(x_test, y_test, verbose=2)
print(f&quot;Restored model, accuracy: {100*acc:.1f}%&quot;)
```

### 从 HDF5 文件加载

## 实例

```
# 从HDF5文件加载
loaded_model = tf.keras.models.load_model(&#x27;my_model.h5&#x27;)

# 验证模型
loss, acc = loaded_model.evaluate(x_test, y_test, verbose=2)
print(f&quot;Restored model, accuracy: {100*acc:.1f}%&quot;)
```

## 选择性保存与加载

### 仅保存权重

## 实例

```
# 保存权重
model.save_weights(&#x27;my_model_weights&#x27;)

# 保存为HDF5格式的权重
model.save_weights(&#x27;my_model_weights.h5&#x27;)
```

### 加载权重

## 实例

```
# 创建相同架构的模型
new_model = tf.keras.Sequential([
    tf.keras.layers.Dense(64, activation=&#x27;relu&#x27;),
    tf.keras.layers.Dense(10, activation=&#x27;softmax&#x27;)
])
new_model.compile(optimizer=&#x27;adam&#x27;,
                loss=&#x27;sparse_categorical_crossentropy&#x27;,
                metrics=[&#x27;accuracy&#x27;])

# 加载权重
new_model.load_weights(&#x27;my_model_weights&#x27;)

# 或者对于.h5文件
new_model.load_weights(&#x27;my_model_weights.h5&#x27;)
```

### 保存自定义训练循环的检查点

## 实例

```
# 创建检查点回调
checkpoint_path = &quot;training_1/cp.ckpt&quot;
cp_callback = tf.keras.callbacks.ModelCheckpoint(
    filepath=checkpoint_path,
    save_weights_only=True,
    verbose=1)

# 使用回调训练模型
model.fit(x_train, y_train,
          epochs=10,
          callbacks=[cp_callback])
```

## 模型保存与加载的最佳实践

1.  **生产环境部署**：优先使用 SavedModel 格式

1.  **跨平台共享**：HDF5 格式更通用

1.  **训练中断恢复**：使用检查点回调定期保存

1.  **自定义对象处理**：

```
model.save(&#x27;custom_model&#x27;, save_format=&#x27;tf&#x27;)
```

1.  **模型版本控制**：为不同版本的模型创建不同目录

## 常见问题与解决方案

### 自定义层/模型保存问题

## 实例

```
# 自定义层示例
class CustomLayer(tf.keras.layers.Layer):
    def __init__(self, units=32, **kwargs):
        super().__init__(**kwargs)
        self.units = units
    
    def build(self, input_shape):
        self.w = self.add_weight(
            shape=(input_shape[-1], self.units),
            initializer=&quot;random_normal&quot;,
            trainable=True)
    
    def call(self, inputs):
        return tf.matmul(inputs, self.w)
    
    def get_config(self):
        config = super().get_config()
        config.update({&quot;units&quot;: self.units})
        return config

# 使用自定义层并保存
model = tf.keras.Sequential([CustomLayer(10)])
model.compile(optimizer=&#x27;adam&#x27;, loss=&#x27;mse&#x27;)
model.save(&#x27;custom_model&#x27;)  # 会自动保存自定义层
```

### 跨版本兼容性问题

+   尽量使用相同版本的 TensorFlow 保存和加载模型

+   对于生产环境，考虑使用 TensorFlow Serving 来避免版本问题

### 大模型保存优化

## 实例

```
# 使用save_weights替代save来减少保存时间
model.save_weights(&#x27;large_model_weights.h5&#x27;)
```