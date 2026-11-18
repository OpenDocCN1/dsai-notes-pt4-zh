
# 加载 Darknet 权重

## 检查卷积模块与 batch_normalize

在循环过程中，我们首先检查 `convolutional` 模块是否有 `batch_normalize`（`True`）。基于此，我们加载权重。

```py
if module_type == "convolutional":
    model = self.module_list[i]
    try:
        batch_normalize = int(self.blocks[i+1]["batch_normalize"])
    except:
        batch_normalize = 0

    conv = model[0]
```

我们保持一个称为 `ptr` 的变量来追踪我们在权重数组中的位置。现在，如果 `batch_normalize` 检查结果是 `True`，则我们按以下方式加载权重：

```py
if (batch_normalize):
    bn = model[1]

    #Get the number of weights of Batch Norm Layer
    num_bn_biases = bn.bias.numel()

    #Load the weights
    bn_biases = torch.from_numpy(weights[ptr:ptr + num_bn_biases])
    ptr += num_bn_biases

    bn_weights = torch.from_numpy(weights[ptr: ptr + num_bn_biases])
    ptr += num_bn_biases

    bn_running_mean = torch.from_numpy(weights[ptr: ptr + num_bn_biases])
    ptr += num_bn_biases

    bn_running_var = torch.from_numpy(weights[ptr: ptr + num_bn_biases])
    ptr += num_bn_biases

    #Cast the loaded weights into dims of model weights. 
    bn_biases = bn_biases.view_as(bn.bias.data)
    bn_weights = bn_weights.view_as(bn.weight.data)
    bn_running_mean = bn_running_mean.view_as(bn.running_mean)
    bn_running_var = bn_running_var.view_as(bn.running_var)

    #Copy the data to model
    bn.bias.data.copy_(bn_biases)
    bn.weight.data.copy_(bn_weights)
    bn.running_mean.copy_(bn_running_mean)
    bn.running_var.copy_(bn_running_var)
```

## 加载卷积层偏置

如果 `batch_normalize` 的检查结果不是 `True`，只需要加载卷积层的偏置项。

```py
else:
    #Number of biases
    num_biases = conv.bias.numel()

    #Load the weights
    conv_biases = torch.from_numpy(weights[ptr: ptr + num_biases])
    ptr = ptr + num_biases

    #reshape the loaded weights according to the dims of the model weights
    conv_biases = conv_biases.view_as(conv.bias.data)

    #Finally copy the data
    conv.bias.data.copy_(conv_biases)
```

最后，我们加载卷积层的权重。

```py
#Let us load the weights for the Convolutional layers
num_weights = conv.weight.numel()

#Do the same as above for weights
conv_weights = torch.from_numpy(weights[ptr:ptr+num_weights])
ptr = ptr + num_weights

conv_weights = conv_weights.view_as(conv.weight.data)
conv.weight.data.copy_(conv_weights)
```

## 调用 load_weights 加载权重

该函数的介绍到此为止，你现在可以通过调用 `darknet` 对象上的 `load_weights()` 函数来加载 `Darknet` 对象中的权重。

```py
model = Darknet("cfg/yolov3.cfg")
model.load_weights("yolov3.weights")
```

通过模型构建和权重加载，我们终于可以开始进行目标检测了。未来，我们还将介绍如何利用 `objectness` 置信度阈值和非极大值抑制生成最终的检测结果。![](img/2d1c94eb4a4ba15f356c96c72092e02b-fs8.png)

*原文链接：https://medium.com/paperspace/tutorial-on-implementing-yolo-v3-from-scratch-in-pytorch-part-1-a0054d38ec78*

**本文为机器之心编译，转载请联系本👸号获得授权。**

✄------------------------------------------------

**加入机器之心（全职记者/实习生）：hr@jiqizhixin.com**

**投稿或寻求报道：editor@jiqizhixin.com**

**广告&商务合作：bd@jiqizhixin.com**
