
如之前一样，我们没有初始化我们的输出张量，除非我们有要分配给它的检测结果。一旦其被初始化，我们就将后续的检测结果与它连接起来。我们使用 `write` 标签来表示张量是否初始化了。在类别上迭代的循环结束时，我们将所得到的检测结果加入到张量输出中。

```py
batch_ind = image_pred_class.new(image_pred_class.size(0), 1).fill_(ind)    
    #Repeat the batch_id for as many detections of the class cls in the image
    seq = batch_ind, image_pred_class

    if not write:
        output = torch.cat(seq,1)
        write = True
    else:
        out = torch.cat(seq,1)
        output = torch.cat((output,out)) 
```

在该函数结束时，我们会检查输出是否已被初始化。如果没有，就意味着在该 batch 的任意图像中都没有单个检测结果。在这种情况下，我们返回 0。

```py
try:
    return output
except:
    return 0
```

这部分就到此为止了。在这部分结束时，我们终于有了一个张量形式的预测结果，其中以行的形式列出了每个预测。现在还剩下：创造一个从磁盘读取图像的输入流程，计算预测结果，在图像上绘制边界框，然后展示/写入这些图像。这是下一部分要介绍的内容。

# 设计输入和输出流程

在这一部分，我们将为我们的检测器构建输入和输出流程。这涉及到从磁盘读取图像，做出预测，使用预测结果在图像上绘制边界框，然后将它们保存到磁盘上。我们也会介绍如何让检测器在相机馈送或视频上实时工作。我们将引入一些命令行标签，以便能使用该网络的各种超参数进行一些实验。接下来就开始吧。

> 注：这部分需要安装 OpenCV 3。

在我们的检测器文件中创建一个 `detector.py` 文件，在上面导入必要的库。

```py
from __future__ import division
import time
import torch 
import torch.nn as nn
from torch.autograd import Variable
import numpy as np
import cv2 
from util import *
import argparse
import os 
import os.path as osp
from darknet import Darknet
import pickle as pkl
import pandas as pd
import random 
```

## 创建命令行参数

因为 `detector.py` 是我们运行我们的检测器的文件，所以有一些可以传递给它的命令行参数会很不错，我使用了 Python 的 `ArgParse` 来做这件事。

```py
def arg_parse():
    """
    Parse arguements to the detect module

    """

    parser = argparse.ArgumentParser(description='YOLO v3 Detection Module')

    parser.add_argument("--images", dest = 'images', help = 
        "Image / Directory containing images to perform detection upon",
        default = "imgs", type = str)
    parser.add_argument("--det", dest = 'det', help = 
        "Image / Directory to store detections to",
        default = "det", type = str)
    parser.add_argument("--bs", dest = "bs", help = "Batch size", default = 1)
    parser.add_argument("--confidence", dest = "confidence", help = "Object Confidence to filter predictions", default = 0.5)
    parser.add_argument("--nms_thresh", dest = "nms_thresh", help = "NMS Threshhold", default = 0.4)
    parser.add_argument("--cfg", dest = 'cfgfile', help = 
        "Config file",
        default = "cfg/yolov3.cfg", type = str)
    parser.add_argument("--weights", dest = 'weightsfile', help = 
        "weightsfile",
        default = "yolov3.weights", type = str)
    parser.add_argument("--reso", dest = 'reso', help = 
        "Input resolution of the network. Increase to increase accuracy. Decrease to increase speed",
        default = "416", type = str)

    return parser.parse_args()

args = arg_parse()
images = args.images
batch_size = int(args.bs)
confidence = float(args.confidence)
nms_thesh = float(args.nms_thresh)
start = 0
CUDA = torch.cuda.is_available() 
```

在这些参数中，重要的标签包括 `images`（用于指定输入图像或图像目录）、`det`（保存检测结果的目录）、`reso`（输入图像的分辨率，可用于在速度与准确度之间的权衡）、`cfg`（替代配置文件）和 `weightsfile`。

## 加载网络

从这里下载 `coco.names` 文件：`https://raw.githubusercontent.com/ayooshkathuria/YOLO_v3_tutorial_from_scratch/master/data/coco.names`。这个文件包含了 COCO 数据集中目标的名称。在你的检测器目录中创建一个文件夹 `data`。如果你使用的 Linux，你可以使用以下命令实现：

```py
mkdir data
cd data
wget https://raw.githubusercontent.com/ayooshkathuria/YOLO_v3_tutorial_from_scratch/master/data/coco.names 
```

然后，将类别文件载入到我们的程序中。

```py
num_classes = 80     #For COCO
classes = load_classes("data/coco.names") 
```

`load_classes` 是在 `util.py` 中定义的一个函数，其会返回一个字典——将每个类别的索引映射到其名称的字符串。

```py
def load_classes(namesfile):
    fp = open(namesfile, "r")
    names = fp.read().split("\n")[:-1]
    return names 
```

初始化网络并载入权重。

```py
#Set up the neural network
print("Loading network.....")
model = Darknet(args.cfgfile)
model.load_weights(args.weightsfile)
print("Network successfully loaded")

model.net_info["height"] = args.reso
inp_dim = int(model.net_info["height"])
assert inp_dim % 32 == 0 
assert inp_dim > 32

#If there's a GPU availible, put the model on GPU
if CUDA:
    model.cuda()

#Set the model in evaluation mode
model.eval() 
```

## 读取输入图像

从磁盘读取图像或从目录读取多张图像。图像的路径存储在一个名为 `imlist` 的列表中。

```py
read_dir = time.time()
#Detection phase
try:
    imlist = [osp.join(osp.realpath('.'), images, img) for img in os.listdir(images)]
except NotADirectoryError:
    imlist = []
    imlist.append(osp.join(osp.realpath('.'), images))
except FileNotFoundError:
    print ("No file or directory with the name {}".format(images))
    exit() 
```

`read_dir` 是一个用于测量时间的检查点。（我们会遇到多个检查点）

如果保存检测结果的目录（由 `det` 标签定义）不存在，就创建一个。

```py
if not os.path.exists(args.det):
    os.makedirs(args.det) 
```

我们将使用 OpenCV 来加载图像。

```py
load_batch = time.time()
loaded_ims = [cv2.imread(x) for x in imlist]
```

`load_batch` 又是一个检查点。

OpenCV 会将图像载入为 numpy 数组，颜色通道的顺序为 BGR。PyTorch 的图像输入格式是（batch x 通道 x 高度 x 宽度），其通道顺序为 RGB。因此，我们在 `util.py` 中写了一个函数 `prep_image` 来将 numpy 数组转换成 PyTorch 的输入格式。

```py
def prep_image(img, inp_dim):
    """
    Prepare image for inputting to the neural network. 

    Returns a Variable 
    """

    img = cv2.resize(img, (inp_dim, inp_dim))
    img = img[:,:,::-1].transpose((2,0,1)).copy()
    img = torch.from_numpy(img).float().div(255.0).unsqueeze(0)
    return img 
```

除了转换后的图像，我们也会维护一个原始图像的列表，以及一个包含原始图像的维度的列表 `im_dim_list`。

```py
#PyTorch Variables for images
im_batches = list(map(prep_image, loaded_ims, [inp_dim for x in range(len(imlist))]))

#List containing dimensions of original images
im_dim_list = [(x.shape[1], x.shape[0]) for x in loaded_ims]
im_dim_list = torch.FloatTensor(im_dim_list).repeat(1,2)

if CUDA:
    im_dim_list = im_dim_list.cuda() 
```

## 创建 batch

```py
leftover = 0
if (len(im_dim_list) % batch_size):
    leftover = 1

if batch_size != 1:
    num_batches = len(imlist) // batch_size + leftover     
    im_batches = [torch.cat((im_batches[i*batch_size : min((i + 1)*batch_size,
        len(im_batches))])) for i in range(num_batches)] 
```

## 检测循环

我们在 batch 上迭代，生成预测结果，将我们必须执行检测的所有图像的预测张量（形状为 Dx8，`write_results` 函数的输出）连接起来。

对于每个 batch，我们都会测量检测所用的时间，即测量获取输入到 `write_results` 函数得到输出之间所用的时间。在 `write_prediction` 返回的输出中，其中一个属性是 batch 中图像的索引。我们对这个特定属性执行转换，使其现在能代表 `imlist` 中图像的索引，该列表包含了所有图像的地址。

在那之后，我们 `print` 每个检测结果所用的时间以及每张图像中检测到的目标。

如果 `write_results` 函数在 batch 上的输出是一个 `int` 值（0），也就是说没有检测结果，那么我们就继续跳过循环的其余部分。
