
# Deploy002 模型部署

> 来源：[https://v11enp9ok1h.feishu.cn/docx/PBVYdgJjbofLJuxy44gc9rMKnwd](https://v11enp9ok1h.feishu.cn/docx/PBVYdgJjbofLJuxy44gc9rMKnwd)

## 动静转换：Torch上更进一步

![](img/5dbc8fae4ba4c598de2e991c21a16e3e.png)

虽说我们在001一节讲了很多torch的好处，torch的优点。最核心的就是torch使用了动态图组网。使用动态组网的好处是。可以使用更偏向python语法的格式对模型进行定义。下面就给大家一个常见的网络：

```
import torch
import torch.nn as nn
import torch.nn.functional as F

class SimpleNet(nn.Module):
    def __init__(self, input_size, hidden_size, num_classes):
        super(SimpleNet, self).__init__()
        self.fc1 = nn.Linear(input_size, hidden_size)
        self.fc2 = nn.Linear(hidden_size, hidden_size)
        self.fc3 = nn.Linear(hidden_size, num_classes)
    
    def forward(self, x):
        x = F.relu(self.fc1(x))
        x = F.relu(self.fc2(x))
        x = self.fc3(x)
        return x

# 示例使用
input_size = 784  # 例如，对于MNIST数据集
hidden_size = 128
num_classes = 10

model = SimpleNet(input_size, hidden_size, num_classes)
print(model)
```

大家很容易开心的写出这样代码，但问题是，使用动态图模式。不可避免的带来了一系列问题。对于一个动态图来说，面临了以下三点问题：

1. 性能：
   + 动态图在每次执行时都需要重新构建计算图，这可能导致额外的开销。
   + 静态图只需构建一次，然后可以重复高效执行。

2. 优化难度：
   + 动态图难以进行全局优化，因为图结构在运行时可能会改变。
   + 静态图允许更多的编译时优化，如内存分配优化、算子融合等。

3. 内存使用：
   + 动态图可能需要更多的运行时内存，因为它需要保持 Python 解释器和相关对象的活跃状态。
   + 静态图可以更有效地管理内存，尤其是在推理阶段。

所以从torch开始，我们第一步要做的就是动转静。拿到静态图才能更好的做整体性能上的优化！

## 美好的愿景：ONNX

ONNX，全称Open Neural Network Exchange，是人工智能领域中一个引人入胜的故事。它的诞生源于一个美好的愿景：在纷繁复杂的深度学习世界中架起一座沟通的桥梁。

2017年的硅谷，各大科技巨头都在人工智能领域奋力拼搏。Facebook（现在的Meta）和Microsoft这两个看似竞争对手的公司，却因为一个共同的梦想走到了一起。他们希望打破AI框架之间的壁垒，让不同平台上训练的模型能够自由迁移。就这样，ONNX项目应运而生。

听起来ONNX是不同模型间完美的桥梁，最后聚合到ONNX完成推理是很开心和能接受的事情。但是听起来越完美的事情就面临越多的问题，首先是对ONNX来说。ONNX模型在某些情况下可能比原生框架的模型运行得慢。这主要是因为ONNX作为一个中间表示，可能无法充分利用特定硬件或框架的优化特性。想象一下，它就像是一个通用翻译器，虽然能够让不同语言的人交流，但可能会损失一些语言中的微妙之处和效率。

除此之外，AI领域发展的太快。ONNX并不一定能很好的表示torch中各种各样的算子，导致模型转换成ONNX失败。谈回之前简单的网络，我们如何把它转换成ONNX形式呢？请看：

```
# 将模型转换为ONNX格式
import torch.onnx

# 创建一个示例输入张量
dummy_input = torch.randn(1, input_size)

# 指定ONNX文件的输出路径
output_path = "simple_net.onnx"

# 导出模型到ONNX
torch.onnx.export(model,               # 要转换的模型
                  dummy_input,         # 模型的输入样例
                  output_path,         # 输出的ONNX文件路径
                  export_params=True,  # 存储训练好的参数权重
                  opset_version=11,    # ONNX算子集版本
                  do_constant_folding=True,  # 是否执行常量折叠优化
                  input_names=['input'],   # 输入节点的名称
                  output_names=['output'], # 输出节点的名称
                  dynamic_axes={'input' : {0 : 'batch_size'},    # 批处理维度动态
                                'output' : {0 : 'batch_size'}})

print(f"Model has been converted to ONNX and saved as {output_path}")
```

## 厂家的秘方：OpenVINO、TensorRT

不同厂家都有自己的推理秘制配方：推理引擎。这种趋势反映了AI领域的激烈竞争和快速创新。每家公司都希望在这场技术革命中占据有利地位，而自研推理引擎成为了关键战略。

这种做法的核心原因在于硬件差异化和性能优化。不同公司拥有各自独特的硬件架构，如英特尔的CPU、NVIDIA的GPU或谷歌的TPU。为了充分发挥这些硬件的潜力，定制化的推理引擎成为必然选择。这些引擎能够针对特定硬件进行深度优化，实现最佳的性能和效率。这其中，我将为大家简单介绍两种。分别是OpenVINO和TensorRT。

### OpenVINO

![](img/208985393cd5144b27ce7c4f48fa156a.png)

让我们先将目光投向OpenVINO。它的故事始于英特尔的实验室，在那里，一群充满激情的工程师梦想着如何让人工智能的力量触手可及。2018年，OpenVINO正式诞生，其名字中的"VINO"代表"Visual Inference and Neural network Optimization"，寓意着它要为视觉智能和神经网络优化开辟一条康庄大道。

OpenVINO可在英特尔®硬件上扩展计算机视觉和非视觉工作负载，从而最大限度地提高性能。它通过从边缘到云的高性能，人工智能和深度学习推理来加速应用程序。

### 关于OpenVINO的模型转换

```
import subprocess
import sys

def convert_onnx_to_openvino(onnx_model_path, output_dir):
    cmd = [
        sys.executable,  # 使用当前Python解释器
        "-m", "mo",  # 调用model optimizer
        "--input_model", onnx_model_path,
        "--output_dir", output_dir,
        "--data_type", "FP32"
    ]
    
    subprocess.run(cmd, check=True)
    print(f"Model has been converted to OpenVINO IR format and saved in {output_dir}")

# 使用示例
onnx_model_path = "simple_net.onnx";
output_dir = "openvino_model";

convert_onnx_to_openvino(onnx_model_path, output_dir)
```

这个转换过程和ONNX很像，在OpenVINO具体执行流程里分为反序列化，输入定义和前向执行几方面。

### TensorRT

![](img/c097120f518f5c0ce8d10c3dd33c4c3e.png)

与此同时，在硅谷的另一端，NVIDIA的工程师们也在编织着自己的AI梦想。2017年，TensorRT横空出世，它的名字中的"RT"代表"Runtime"，彰显了它对高性能推理的执着追求。

TensorRT就像是一位技艺精湛的魔法师，它能够将庞大复杂的神经网络模型变成小巧高效的推理引擎。它的法术可以让模型在NVIDIA的GPU上飞驰，实现令人瞠目的低延迟和高吞吐量。想象一下，它就像是给AI装上了火箭推进器，让智能决策的速度突破音障。

TensorRT可用于对超大规模数据中心，嵌入式平台或自动驾驶平台进行推理加速。TensorRT现已能支持TensorFlow，Caffe，Mxnet，Pytorch等几乎所有的深度学习框架，将TensorRT和NVIDIA的GPU结合起来，能在几乎所有的框架中进行快速和高效的部署推理。但可惜，TensorRT是一个闭源的库。

### 关于TensorRT模型的转换

我们一般会给TensorRT的模型叫为`engine`，我们可以使用`trt`提供的命令行工具，`trtexec`进行转换

```
trtexec --onnx=simple_net.onnx --saveEngine=simple_net.trt --explicitBatch
```

## 推理引擎：类似的执行流程

+ OpenVINO模型部署分为两个部分：模型优化器和推理引擎。

模型优化器将训练好的模型转换为推理引擎可以识别的中间表达–IR文件，并在转换过程中对模型进行优化。推理引擎接受经过模型优化器转换并优化的网络模型，为Intel的各种计算设备提供高性能的神经网络推理运算。

+ TensorRT模型部署也是分为两个部分：`build`和`deployment` 。

`build`：这个阶段主要完成模型转换，将不同框架的模型转换到TensorRT。模型转换时会完成前述优化过程中的层间融合，精度校准。这一步的输出是一个针对特定GPU平台和网络模型的优化过的TensorRT模型，这个TensorRT模型可以序列化存储到磁盘或内存中。存储到磁盘中的文件称之为 `plan file`。

`deployment`：将上面一个步骤中的`plan`文件首先反序列化，并创建一个 `runtime engine`，然后就可以输入数据（比如测试集或数据集之外的图片），然后输出分类向量结果或检测结果。

## 写在最后

模型部署以加速为最终目的，首先就会抛弃易用性。这里特指静态图，在固定的范围内做极致的优化。除了模型上的优化，不同硬件厂商更会在贴近不同硬件上做各种底层上的优化。以获得在特定芯片上极致的性能。请期待后续部署教程吧～
