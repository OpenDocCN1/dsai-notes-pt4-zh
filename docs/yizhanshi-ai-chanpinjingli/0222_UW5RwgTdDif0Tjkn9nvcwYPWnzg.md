# PyTorch 快速入门

> 来源：[https://v11enp9ok1h.feishu.cn/docx/JdHqdL0ayoOpwaxx4IScSCzhnVe](https://v11enp9ok1h.feishu.cn/docx/JdHqdL0ayoOpwaxx4IScSCzhnVe)

> 在人工智能领域中，PyTorch作为一个强大且灵活的开发平台，被广泛应用于深度学习和机器学习的研究与应用中。它提供了丰富的功能和工具，使得开发人员能够快速构建、训练和部署各种类型的神经网络模型。相较于其他深度学习框架，PyTorch以其动态计算图、易于调试和灵活的API设计而备受青睐，尤其在学术研究领域广受欢迎。我们是针对InterLM实战营的小伙伴们设置的课程，带着大家快速入门PyTorch，正如下图所示，我们将从PyTorch的知识学习以代码形式进行实践，以及最后会有作业让大家巩固所学到的知识！！！

# 一、PyTorch 简介

在我们正式学习PyTorch之前，我会带大家简单了解一些什么是PyTorch和如何学习PyTorch。

## ⭐️什么是PyTorch

[PyTorch](https://pytorch.org/) 是由Facebook（也就是现在的Meta）的人工智能研究团队开发的一个开源机器学习和深度学习框架。我们可以使用 Python 代码进行操作和处理数据并编写机器学习算法。它提供了强大的GPU加速的张量计算和自动求导系统，使得深度神经网络的构建和训练更加高效和便捷。

下面是PyTorch的一个知识图谱，简单介绍了PyTor**ch以**及它的特点，因为 PyTorch 易用性的特点，我们可以快速进行开发，所以它特别适合刚入门人工智能领域的小伙伴，这也是我们为什么要把它放在基础部分。

上面简单了解了PyTorch以后，我们来介绍一下PyTorch的主要特点和优势：

1.  PyTorch 的主要特点：

1.  **动态计算图**：PyTorch使用动态计算图，这意味着模型的计算过程是根据代码的执行顺序来确定的。这种灵活性使得PyTorch非常适合进行研究和原型设计。

1.  **张量计算**：PyTorch的核心数据结构是张量，它提供了丰富的张量计算操作，使得构建和训练神经网络变得更加容易。

1.  **自动求导**：PyTorch提供了自动求导功能，可以自动计算模型的梯度，从而简化了反向传播的过程。

1.  **丰富的库和工具**：PyTorch拥有丰富的库和工具，包括计算机视觉、自然语言处理等领域的预训练模型，以及可视化工具等。

1.  PyTorch 的优势：

1.  **易用性**：PyTorch的API设计简洁直观，易于上手，特别适合初学者和研究人员使用。

1.  **灵活性**：PyTorch的动态计算图和张量计算使得模型的构建和修改变得更加灵活，可以更好地满足不同任务的需求。

1.  **性能**：PyTorch在GPU上的计算性能非常出色，可以加速模型的训练和推理过程。

1.  **社区支持**：PyTorch拥有庞大的开发者社区和丰富的学习资源，可以帮助我们快速解决遇到的问题。

## ⛳️如何学习PyTorch

了解完 [PyTorch 以后我](https://pytorch.org/docs/stable/index.html)们了解一下如何学习 PyTorch。在我们学习过程中，最重要的就是学会搜索，也是为什么我会在上面的学习架构图上加了Search的标识。这里教大家一些搜索的技巧，因为我们快速入门的课程主要以代码为主，所以最多的就是torch的方法，如果遇到了torch方法的问题可以直接将问题复制到浏览器中搜索，或者使用pytorch官方文档搜索该方法的定义，从中找出问题所在来解决问题。

因为[InternStudio](https://aicarrier.feishu.cn/wiki/IHvwwCEDpiiwFYk0uX0cz4mhnpd?from=from_copylink)们的学习方式主要是代码的形式，所以不免会遇到环境配置的问题，这里为了方便大家学习，可以使用InternStudio进行学习，有预设好的pytorch环境。关于InternStudio的使用教程你可以看 。当然你也可以使用自己本地的开发环境进行学习，网上有许多PyTorch环境配置的教程，相信你可以顺利的配置下来。

同时我们也给大家提供了Github的代码仓库，大家可以将代码仓库clone到本地或者开发机上运行我们的代码。代码都是以ipynb格式的文件，每一个代码文件都会有对应的解释说明，相信大家都可以快速的入门PyTorch😀。

# 二、PyTorch

## 环境配置

首先我们要使用开发机创建一个用于学习PyTorch的虚拟环境，我们可以使用下面两种方式进行创建：

1.  使用InternStudio内置的studio-conda命令创建

```
studio-conda pytorch-learning
```

1.  使用conda create 命令创建

```
conda create -n pytorch-learning python=3.11 -y
conda activate pytorch-learning
conda install pytorch==2.0.1 torchvision==0.15.2 torchaudio==2.0.2 pytorch-cuda=12.2 -c pytorch -c nvidia
```

当完成环境创建以后我们在JupyterLab（这里你可以使用VScode的jupyterlab也可以使用开发机上的）上面选择我们创建的虚拟环境。

创建好环境以后我们导入pytorch包，查看一下我们的pytorch版本。

```
import torch

torch.__version__
```