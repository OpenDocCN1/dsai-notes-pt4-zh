
# 大模型推理与深度学习介绍

## 模型推理代码示例

```python
prompt = "请问什么是深度学习？"
messages = [
    {"role": "user", "content": prompt}
]
text = tokenizer.apply_chat_template(
    messages,
    tokenize=False,
    add_generation_prompt=True
)
model_inputs = tokenizer([text], return_tensors="pt").to(device)
```

```python
generated_ids = model.generate(
    model_inputs.input_ids,
    max_new_tokens=512
)
generated_ids = [
    output_ids[len(input_ids):] for input_ids, output_ids in zip(model_inputs.input_ids, generated_ids)
]

response = tokenizer.batch_decode(generated_ids, skip_special_tokens=True)[0]
```

```
response
```

## 深度学习概念介绍

深度学习（Deep Learning）是一种人工智能领域的分支，主要研究如何使用多层神经网络从大量数据中自动提取模式、规律或特性，并将这些模式、规律或特性以预定的形式表示出来，从而实现对新数据进行预测的能力。深度学习的研究对象主要是由大量的特征向量构成的复杂数据集。在深度学习系统中，每个神经元通过一系列复杂的数学运算来计算输入数据的特征向量，并将其作为神经元的输出信息（Output Information）；而每个输出信息经过一系列复杂的数学运算之后，则会被转换为一个或多个数值型特征向量（Feature Vector），然后这些数值型特征向量（Feature Vector）将会被用来作为模型训练时的输入参数。深度学习系统中的每一层神经元通常都由一个或多层卷积层、一个或多层全连接层和一个多层池化层组成，其结构如图所示：

```python
import torch.nn as nn
# Define the input shape and number of channels.
input_shape = (28, 28))
num_channels = 3
# Define a convolutional neural network with max pooling layers.
class ConvNet(nn.Module):
    # Define the architecture of the convolutional neural network.
    super().__init__()
    # Add one convolutional layer with max pooling layers.
    convolutional_layer1 = nn.Conv(in_features=input_shape[0]], out_features=num_channels, stride=2))
    max_pooling_layer1 = nn.MaxPool1d(kernel_size=2, padding=-1)))
    convolutional_layer2 = nn.Conv(in_features=input_shape[0]], out_features=num_channels-1, stride=2))
    max_pooling_layer2 = nn.MaxPool1d(kernel_size=2, padding=-1)))
    convolutional_layer3 = nn.Conv(in_features=input_shape[0]], out_features=num_channels-2, stride=2))
    max_pooling_layer3 = nn.MaxPool1d(kernel_size=2, padding=-1)))
    dense_block_1 = nn.Linear(num_channels-2), num_classes)
    dense_block_2 = nn.Linear(num_channels-2), num_classes-1)
# Define the last fully connected layer and its corresponding output size.
output_size = num_classes
lastfully_connected_layer = dense_block_1(output_size))
```

In this example, we have defined a convolutional neural network with max pooling layers. The architecture of the network consists of five convolutional layers (with max pool layers) followed by three fully connected layers. The first and second convolutional layers each have one convolutional layer with max pooling layers and a max pooling layer applied to the output of the first convolutional layer. The third convolutional layer has no max pooling layers, and it applies a max pooling layer to the output of the previous two convolutional layers. In each of the five convolutional layers, there are multiple convolutional filters with different kernel sizes and stride values. These convolutional filters are used to extract features from the input data that can be used for training the neural network. The output of each convolutional layer is a set of extracted features or labels that represent the input data in terms of its structure and characteristics. These extracted features or labels can then be used by the last fully connected layer (the final output layer) to generate the final predictions or classifications based on the input data. In summary, deep learning networks are trained using large datasets consisting of high-dimensional feature vectors, which can be represented mathematically as a tensor with $(m+n) \times (m+n)$ elements. In these training sessions, deep neural networks learn how to extract features from input data and use those extracted features to make predictions or classifications on new data. Deep learning networks are capable of handling complex datasets consisting of high-dimensional feature vectors that can be represented mathematically as a tensor with $(m+n) \times (m+n)$ elements, and they have been successfully applied to various fields, including computer vision, natural language processing, robotics, bioinformatics, among others. Overall, deep learning networks are powerful tools for solving complex problems in various domains, thanks to their ability to handle large datasets consisting of high-dimensional feature vectors that can be represented mathematically as a tensor with $(m+n) \times (m+n)$ elements.

## 社群与更多内容

### 更多大模型技术内容学习

**扫码添加助理英英**，回复“大模型”，了解更多大模型技术详情哦👇

![](img/dc5304c9ec2052f95f7f167e336ba00a.png)

此外，**扫码回复“入群”**，即可加入**大模型技术社群：海量硬核独家技术干货内容 + 无门槛技术交流！**
