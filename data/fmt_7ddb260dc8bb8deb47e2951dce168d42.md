
# 模型训练与测试结果

## 训练损失

```
Epoch [1/5], Loss: 0.2325
Epoch [2/5], Loss: 0.0526
Epoch [3/5], Loss: 0.0366
Epoch [4/5], Loss: 0.0273
Epoch [5/5], Loss: 0.0221
```

**解释：**损失逐渐下降表明模型在逐步收敛。

## 测试集的准确率

代码在测试集上输出最终的分类准确率，例如：

```
Test Accuracy: 98.96%
```

**解释：**模型对 MNIST 测试集的分类准确率为 98.96%，对于简单的 CNN 模型来说是一个不错的结果。

## 可视化结果

我们可以在测试数据中可视化一些样本及其预测结果。

## 代码实例

```
import torch
import torch.nn as nn
import torch.nn.functional as F
import torch.optim as optim
from torchvision import datasets, transforms
import matplotlib.pyplot as plt

# 1. 数据加载与预处理
transform = transforms.Compose([
    transforms.ToTensor(),  # 转为张量
    transforms.Normalize((0.5,), (0.5,))  # 归一化到 [-1, 1]
])

# 加载 MNIST 数据集
train_dataset = datasets.MNIST(root=&#x27;./data&#x27;, train=True, transform=transform, download=True)
test_dataset = datasets.MNIST(root=&#x27;./data&#x27;, train=False, transform=transform, download=True)

train_loader = torch.utils.data.DataLoader(dataset=train_dataset, batch_size=64, shuffle=True)
test_loader = torch.utils.data.DataLoader(dataset=test_dataset, batch_size=64, shuffle=False)

# 2. 定义 CNN 模型
class SimpleCNN(nn.Module):
    def __init__(self):
        super(SimpleCNN, self).__init__()
        # 定义卷积层
        self.conv1 = nn.Conv2d(1, 32, kernel_size=3, stride=1, padding=1)  # 输入1通道，输出32通道
        self.conv2 = nn.Conv2d(32, 64, kernel_size=3, stride=1, padding=1)  # 输入32通道，输出64通道
        # 定义全连接层
        self.fc1 = nn.Linear(64 * 7 * 7, 128)  # 展平后输入到全连接层
        self.fc2 = nn.Linear(128, 10)  # 10 个类别

    def forward(self, x):
        x = F.relu(self.conv1(x))  # 第一层卷积 + ReLU
        x = F.max_pool2d(x, 2)     # 最大池化
        x = F.relu(self.conv2(x))  # 第二层卷积 + ReLU
        x = F.max_pool2d(x, 2)     # 最大池化
        x = x.view(-1, 64 * 7 * 7) # 展平
        x = F.relu(self.fc1(x))    # 全连接层 + ReLU
        x = self.fc2(x)            # 最后一层输出
        return x

# 创建模型实例
model = SimpleCNN()

# 3. 定义损失函数与优化器
criterion = nn.CrossEntropyLoss()  # 多分类交叉熵损失
optimizer = optim.SGD(model.parameters(), lr=0.01, momentum=0.9)

# 4. 模型训练
num_epochs = 5
model.train()  # 设置模型为训练模式

for epoch in range(num_epochs):
    total_loss = 0
    for images, labels in train_loader:
        outputs = model(images)  # 前向传播
        loss = criterion(outputs, labels)  # 计算损失

        optimizer.zero_grad()  # 清空梯度
        loss.backward()  # 反向传播
        optimizer.step()  # 更新参数

        total_loss += loss.item()

    print(f&quot;Epoch [{epoch+1}/{num_epochs}], Loss: {total_loss / len(train_loader):.4f}&quot;)

# 5. 模型测试
model.eval()  # 设置模型为评估模式
correct = 0
total = 0

with torch.no_grad():  # 关闭梯度计算
    for images, labels in test_loader:
        outputs = model(images)
        _, predicted = torch.max(outputs, 1)
        total += labels.size(0)
        correct += (predicted == labels).sum().item()

accuracy = 100 * correct / total
print(f&quot;Test Accuracy: {accuracy:.2f}%&quot;)

# 6. 可视化测试结果
dataiter = iter(test_loader)
images, labels = next(dataiter)
outputs = model(images)
_, predictions = torch.max(outputs, 1)

fig, axes = plt.subplots(1, 6, figsize=(12, 4))
for i in range(6):
    axes[i].imshow(images[i][0], cmap=&#x27;gray&#x27;)
    axes[i].set_title(f&quot;Label: {labels[i]}\nPred: {predictions[i]}&quot;)
    axes[i].axis(&#x27;off&#x27;)
plt.show()
```

可视化结果展示了 6 个测试样本的实际标签与预测值，例如：

![](img/a82d6f809c0b02dad8def9ffcebe0ce2.png)

+   图片的左上角是手写数字。

+   标题部分显示了模型的预测值和真实标签。

## 注意几种错误

下载 MNIST 数据集时，由于 SSL 证书验证失败，导致无法下载数据。

```
Downloading http://yann.lecun.com/exdb/mnist/train-images-idx3-ubyte.gz
Failed to download (trying next):
&lt;urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed: unable to get local issuer certificate (_ssl.c:1129)&gt;
...
```

### 解决办法

打开终端，运行以下命令来安装/更新 SSL 证书：

```
/Applications/Python\ 3.x/Install\ Certificates.command
```

其中 `3.x` 替换为你实际安装的 Python 版本。

运行此命令后，重新运行你的 Python 代码，它应该能够成功下载 MNIST 数据集。
