
```py
Resolution = ((InputImageSize - 1) / OutputStride) + 1

// Example: an input image with a width of 225 pixels and an output
// stride of 16 results in an output resolution of 15
// 15 = ((225 - 1) / 16) + 1
```

## 热图

每个热图是尺寸为分辨率 x 分辨率 x 17 的 3D 张量，17 是 `PoseNet` 检测到的关键点的数量。例如，对于 225 的图像大小和 16 的输出步幅，热图大小将是 15 x 15 x 17。（17 个中）第三维的每个切片图对应特定关键点的热图。热图中每个位置具有置信度得分，即该关键点类型的一部分存在于该位置中的概率。我们可以将其看作是把原始图像分割成 15 x 15 网格，其中热图得分提供了每个关键点在每个网格正方形中存在可能性的分类。

## 偏移向量

每个偏移向量是尺寸为分辨率 x 分辨率 x 34 的 3D 张量，34 是关键点数量 * 2 得出的数字。如果图像大小为 225，输出步幅为 16，则该值为 15 x 15 x 34。由于热图是关键点所在位置的近似，因此偏移向量在位置上对应热图点，并且用于预测关键点的确切位置，如通过从相应热图点沿着向量行进。偏移向量的前 17 个切片图包含向量的 `x`，后 17 个切片包含 `y`。偏移向量大小与原始图像的比例相同。

## 根据模型的输出估计姿态

在图像输入到模型后，我们执行一些计算以从输出估计姿态。例如，单姿态估计算法返回姿态置信度得分，该得分本身包含多个关键点（由 `part ID` 索引），每个关键点具有置信度得分和 `xy` 位置。

要获取姿态的关键点：

1. 在热图上进行 `sigmoid` 激活函数操作，获得得分。

   ```py
   scores = heatmap.sigmoid()
   ```

2. 对关键点置信度得分执行 `argmax2d`，获得热图中的 `x` 和 `y` 索引以及每个部分的最高得分，这基本上是该部分最可能存在的位置。这将产生大小为 17 x 2 的张量，每行是热图中的 `y` 和 `x` 索引以及每个部分的最高得分。

   ```py
   heatmapPositions = scores.argmax(y, x)
   ```

3. 从某部分热图中的 `x` 和 `y` 索引的偏移中获取 `x` 和 `y`，得到每个部分的偏移向量。这产生大小为 17 x 2 的张量，其中每行都是对应关键点的偏移向量。例如，对于索引 `k` 处的部分，当热图位置为 `y` 和 `d` 时，偏移向量为：

   ```py
   offsetVector = [offsets.get(y, x, k), offsets.get(y, x, 17 + k)]
   ```

4. 为获得关键点，每个部分的热图 `x` 和 `y` 乘以输出步幅，然后再加与原始图像比例相同的相应偏移向量。

   ```py
   keypointPositions = heatmapPositions * outputStride + offsetVectors
   ```

5. 最后，每个关键点置信度得分是其热图位置的置信度得分。姿态置信度得分是关键点得分的平均值。

## 多人姿态估计

多姿态估计算法的细节不在本文的讨论范围之内。该算法的主要不同之处在于，它使用贪婪过程通过沿着基于部分的图跟随位移向量来将关键点分组。具体而言，它采用了研究论文《`PersonLab: Person Pose Estimation and Instance Segmentation with a Bottom-Up, Part-Based, Geometric Embedding Model`》中的快速贪婪解码算法（fast greedy decoding algorithm）。有关多姿态算法的更多信息，请阅读完整论文或查看代码。代码地址：`https://github.com/tensorflow/tfjs-models/tree/master/posenet/src`。![](img/2d1c94eb4a4ba15f356c96c72092e02b-fs8.png)

原文链接：`https://medium.com/tensorflow/real-time-human-pose-estimation-in-the-browser-with-tensorflow-js-7dd0bc881cd5`

**本文为机器之心编译，转载请联系本👸号获得授权。**

✄------------------------------------------------

**加入机器之心（全职记者/实习生）：** `hr@jiqizhixin.com`

**投稿或寻求报道：** `content@jiqizhixin.com`

**广告&商务合作：** `bd@jiqizhixin.com`
