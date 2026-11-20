
```python
###### Pytorch卷积层输出大小验证
import torch
import torch.nn as nn
import torch.autograd as autograd
# With square kernels and equal stride
# output_shape: height = (50-3)/2+1 = 24.5，卷积向下取整，所以 height=24.
m = nn.Conv2d(16, 33, 3, stride=2)
# # non-square kernels and unequal stride and with padding
# m = nn.Conv2d(16, 33, (3, 5), stride=(2, 1), padding=(4, 2))  # 输出shape: torch.Size([20, 33, 28, 100])
# # non-square kernels and unequal stride and with padding and dilation
# m = nn.Conv2d(16, 33, (3, 5), stride=(2, 1), padding=(4, 2), dilation=(3, 1))  # 输出shape: torch.Size([20, 33, 26, 100])
input = autograd.Variable(torch.randn(20, 16, 50, 100))
output = m(input)

print(output.shape)  # 输出shape: torch.Size([20, 16, 24, 49])
```

# 卷积神经网络

卷积神经网络一般由卷积层、汇聚层和全连接层构成。

## 汇聚层

通常当我们处理图像时，我们希望逐渐降低隐藏表示的空间分辨率、聚集信息，这样随着我们在神经网络中层叠的上升，每个神经元对其敏感的感受野（输入）就越大。

汇聚层(Pooling Layer)也叫子采样层(Subsampling Layer)，其作用不仅是进降低卷积层对位置的敏感性，同时降低对空间降采样表示的敏感性。

与卷积层类似，汇聚层运算符由一个固定形状的窗口组成，该窗口根据其步幅大小在输入的所有区域上滑动，为固定形状窗口(有时称为汇聚窗口)遍历的每个位置计算一个输出。然而，不同于卷积层中的输入与卷积核之间的互相关计算，汇聚层不包含参数。相反，池运算是确定性的，我们通常计算汇聚窗口中所有元素的最大值或平均值。这些操作分别称为最大汇聚层(maximum pooling)和平均汇聚层(average pooling)。

在这两种情况下，与互相关运算符一样，汇聚窗口从输入张量的左上⻆开始，从左往右、从上往下的在输入张量内滑动。在汇聚窗口到达的每个位置，它计算该窗口中输入子张量的最大值或平均值。计算最大值或平均值是取决于使用了最大汇聚层还是平均汇聚层。

比如最大汇聚层，其计算输入中区域的最大值。

![](img/6d874781eb63b8699a28bcee647afd66.png)

### 汇聚层 API

Pytorch 框架中对应的聚合层 api 如下：

```
class torch.nn.MaxPool2d(kernel_size, stride=None, padding=0, dilation=1, return_indices=False, ceil_mode=False)
```

主要参数解释：

+   `kernel_size`(int or tuple)：max pooling 的窗口大小。

+   `stride`(int or tuple, optional)：max pooling的窗口移动的步长。默认值是`kernel_size`。

+   `padding`(int or tuple, optional)：默认值为 0，即不填充像素。输入的每一条边补充 0 的层数。

+   `dilation`：滑动窗中各元素之间的距离。

+   `ceil_mode`：默认值为 `False`，即上述公式默认向下取整，如果设为 `True`，计算输出信号大小的时候，公式会使用向上取整。

> Pytorch 中池化层默认 `ceil_mode = False`，而 Caffe 只实现了 `ceil_mode = True` 的计算方式。

```python
import torch
import torch.nn as nn
import torch.autograd as autograd
# 大小为3，步幅为2的正方形窗口池
m = nn.MaxPool2d(kernel_size=3, stride=2, padding=1)
# pool of non-square window
input = autograd.Variable(torch.randn(20, 16, 50, 32))
output = m(input)
print(output.shape)  # torch.Size([20, 16, 25, 16])
```

# 卷积神经网络结构

一个典型的卷积网络结构是由卷积层、汇聚层、全连接层交叉堆叠而成。如下图所示：

![](img/1b95011d8e31e82dfaa532a024c26bb4.png)

一个简单的 CNN 网络连接图如下所示：

![](img/56c7ec300ba048fa31c37e12ab831e97.png)

目前，卷积网络的整体结构趋向于使用更小的卷积核(比如 和 ) 以及更深的结构(比如层数大于 50)

另外，由于卷积层的操作性越来越灵活（同样可完成减少特征图分辨率），汇聚层的作用越来越小，因此目前的卷积神经网络逐渐趋向于全卷积网络。

![](img/f92bacd26d14611bb8f1fba645d2b166.png)
