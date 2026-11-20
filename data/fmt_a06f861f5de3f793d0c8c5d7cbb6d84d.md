
# 教程 | 如何使用纯 NumPy 代码从头实现简单的卷积神经网络

选自 KDnuggets

**作者：****Ahmed Gad**

**机器之心编译**

**参与：张倩、思源**

> 我们常使用深度学习框架构建强大的卷积神经网络，这些框架不仅能轻松调用卷积运算，同时还以矩阵乘法的方式大大提升了并行计算效率。但仅使用 `NumPy` 库创建 `CNN` 也许是理解这种网络的更好方法，本文就使用纯 `NumPy` 代码构建卷积层、`ReLU` 层和最大池化层等。

在某些情况下，使用 ML/DL 库中已经存在的模型可能会很便捷。但为了更好地控制和理解模型，你应该自己去实现它们。本文展示了如何仅使用 `NumPy` 库来实现 `CNN`。

卷积神经网络（`CNN`）是分析图像等多维信号的当前最优技术。目前已有很多库可以实现 `CNN`，如 `TensorFlow` 和 `Keras` 等。这种库仅提供一个抽象的 `API`，因此可以大大降低开发难度，并避免实现的复杂性，不过使用这种库的开发人员无法接触到一些细节，这些细节可能在实践中很重要。

有时，数据科学家必须仔细查看这些细节才能提高性能。在这种情况下，最好自己亲手构建此类模型，这可以帮助你最大程度地控制网络。因此在本文中，我们将仅使用 `NumPy` 尝试创建 `CNN`。我们会创建三个层，即卷积层（简称 `conv`）、`ReLU` 层和最大池化层。所涉及的主要步骤如下：

1. 读取输入图像。

2. 准备滤波器。

3. 卷积层：使用滤波器对输入图像执行卷积操作。

4. `ReLU` 层：将 `ReLU` 激活函数应用于特征图（卷积层的输出）。

5. 最大池化层：在 `ReLU` 层的输出上应用池化操作。

6. 堆叠卷积层、`ReLU` 层和最大池化层。

## 1. 读取输入图像

以下代码读取 `skimage` Python 库中一个已有图像，并将其转换为灰度图。

```py
import skimage.data
# Reading the image
img = skimage.data.chelsea()
# Converting the image into gray.
img = skimage.color.rgb2gray(img)
```

读取图像是第一步，因为后续步骤取决于输入大小，下图为转换后的灰度图。

![](img/4cf8b48519f08628ba0d99936ac7a65a-fs8.png)

## 2. 准备滤波器

以下代码为第一个卷积层（简称 `l1`）准备滤波器组：

```py
l1_filter = numpy.zeros((2,3,3))
```

根据滤波器的数量和每个滤波器的大小创建数组。我们有 2 个大小为 `3*3` 的滤波器，因此数组大小为 `(2=num_filters, 3=num_rows_filter, 3=num_columns_filter)`。将滤波器的尺寸选择为没有深度的 `2D` 数组，因为输入图像是灰度图且深度为 `1`。如果图像是具有 3 个通道的 `RGB`，则滤波器大小必须是 `(3, 3, 3=depth)`。

滤波器组的大小由上述 `0` 数组指定，但不是由滤波器的实际值指定。可以按如下方式覆写这些值，以检测垂直和水平边缘。

```py
l1_filter[0, :, :] = numpy.array([[[-1, 0, 1],
   [-1, 0, 1],
   [-1, 0, 1]]])
l1_filter[1, :, :] = numpy.array([[[1, 1, 1],
   [0, 0, 0],
   [-1, -1, -1]]])
```

## 3. 卷积层

在准备好滤波器之后，下一步就是用它们对输入图像执行卷积操作。下面一行使用 `conv` 函数对图像执行卷积操作：

```py
l1_feature_map = conv(img, l1_filter)
```

此类函数只接受两个参数，即图像和滤波器组，实现如下：

```py
def conv(img, conv_filter):
    if len(img.shape) > 2 or len(conv_filter.shape) > 3: # Check if number of image channels matches the filter depth.
        if img.shape[-1] != conv_filter.shape[-1]:
            print("Error: Number of channels in both image and filter must match.")
            sys.exit()
    if conv_filter.shape[1] != conv_filter.shape[2]: # Check if filter dimensions are equal.
        print('Error: Filter must be a square matrix. I.e. number of rows and columns must match.')
        sys.exit()
    if conv_filter.shape[1] % 2 == 0: # Check if filter diemnsions are odd.
        print('Error: Filter must have an odd size. I.e. number of rows and columns must be odd.')
        sys.exit()

    # An empty feature map to hold the output of convolving the filter(s) with the image.
    feature_maps = numpy.zeros((img.shape[0]-conv_filter.shape[1]+1,
        img.shape[1]-conv_filter.shape[1]+1,
        conv_filter.shape[0]))

    # Convolving the image by the filter(s).
    for filter_num in range(conv_filter.shape[0]):
        print("Filter ", filter_num + 1)
        curr_filter = conv_filter[filter_num, :] # getting a filter from the bank.
        """
        Checking if there are mutliple channels for the single filter.
        If so, then each channel will convolve the image.
        The result of all convolutions are summed to return a single feature map.
        """
        if len(curr_filter.shape) > 2:
            conv_map = conv_(img[:, :, 0], curr_filter[:, :, 0]) # Array holding the sum of all feature maps.
            for ch_num in range(1, curr_filter.shape[-1]): # Convolving each channel with the image and summing the results.
                conv_map = conv_map + conv_(img[:, :, ch_num],
                    curr_filter[:, :, ch_num])
        else: # There is just a single channel in the filter.
            conv_map = conv_(img, curr_filter)
        feature_maps[:, :, filter_num] = conv_map # Holding feature map with the current filter.
    return feature_maps # Returning all feature maps.
```

该函数首先确保每个滤波器的深度等于图像通道的数量。在下面的代码中，外部的 `if` 语句将检查通道和滤波器是否有深度。如果有，则内部 `if` 语句检查它们是否相等，如果不匹配，脚本将退出。

```py
if len(img.shape) > 2 or len(conv_filter.shape) > 3: # Check if number of image channels matches the filter depth.
    if img.shape[-1] != conv_filter.shape[-1]:
        print("Error: Number of channels in both image and filter must match.")
```

此外，滤波器的尺寸行数和列数是奇数且相等。使用以下两个 `if` 语句对其进行检查。如果不满足这些条件，脚本将退出。

```py
if conv_filter.shape[1] != conv_filter.shape[2]: # Check if filter dimensions are equal.
    print('Error: Filter must be a square matrix. I.e. number of rows and columns must match.')
    sys.exit()
if conv_filter.shape[1] % 2 == 0: # Check if filter diemnsions are odd.
    print('Error: Filter must have an odd size. I.e. number of rows and columns must be odd.')
    sys.exit()
```

如果不满足上述所有的 `if` 语句，则表示滤波器的深度适合图像，且可应用卷积操作。滤波器对图像的卷积从初始化一个数组开始，通过根据以下代码指定其大小来保存卷积的输出（即特征图）：

```py
# An empty feature map to hold the output of convolving the filter(s) with the image.
feature_maps = numpy.zeros((img.shape[0]-conv_filter.shape[1]+1,
    img.shape[1]-conv_filter.shape[1]+1,
    conv_filter.shape[0]))
```

特征图大小将与上述代码中的 `(img_rows-filter_rows+1, image_columns-filter_columns+1, num_filters)` 值相等。请注意，滤波器组中的每个滤波器都有一个输出特征图。因此将滤波器组（`conv_filter.shape[0]`）中的滤波器数量将指定为第三个参数。

```py
# Convolving the image by the filter(s).
for filter_num in range(conv_filter.shape[0]):
    print("Filter ", filter_num + 1)
    curr_filter = conv_filter[filter_num, :] # getting a filter from the bank.
    """
    Checking if there are mutliple channels for the single filter.
    If so, then each channel will convolve the image.
    The result of all convolutions are summed to return a single feature map.
    """
    if len(curr_filter.shape) > 2:
        conv_map = conv_(img[:, :, 0], curr_filter[:, :, 0]) # Array holding the sum of all feature maps.
        for ch_num in range(1, curr_filter.shape[-1]): # Convolving each channel with the image and summing the results.
            conv_map = conv_map + conv_(img[:, :, ch_num],
                curr_filter[:, :, ch_num])
    else: # There is just a single channel in the filter.
        conv_map = conv_(img, curr_filter)
    feature_maps[:, :, filter_num] = conv_map # Holding feature map with the current filter.
```

外部循环在滤波器组中的每个滤波器上进行迭代，并根据下面这行代码返回，以执行后续步骤：

```py
curr_filter = conv_filter[filter_num, :] # getting a filter from the bank.
```

如果要卷积的图像通道数大于 `1`，则滤波器深度必须与通道数量相等。在这种情况下，卷积是通过将每个图像通道与其在滤波器中对应的通道进行卷积来完成的。最后的结果加起来就是输出特征图。如果图像只有一个通道，则卷积将非常容易。此类行为由 `if-else` 块决定：

```py
if len(curr_filter.shape) > 2:
    conv_map = conv_(img[:, :, 0], curr_filter[:, :, 0]) # Array holding the sum of all feature map
    for ch_num in range(1, curr_filter.shape[-1]): # Convolving each channel with the image and summing the results.
        conv_map = conv_map + conv_(img[:, :, ch_num],
            curr_filter[:, :, ch_num])
else: # There is just a single channel in the filter.
    conv_map = conv_(img, curr_filter)
```

你可能会注意到，卷积是由名为 `conv_` 的函数实现的，该函数与 `conv` 函数不同。`conv` 函数仅接受输入图像和滤波器组，但本身不执行卷积操作，只负责将每组待卷积的输入-滤波器组传输到 `conv_` 函数上。这只是为了使代码更易于调查。下面是 `conv_` 函数的实现：
