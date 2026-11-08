# PyTorch 入门

> 来源：[https://gotoaiworld.feishu.cn/docx/ZvP1dAPeDoZgkgxLmC5ciMXnnxf](https://gotoaiworld.feishu.cn/docx/ZvP1dAPeDoZgkgxLmC5ciMXnnxf)

# **免费开源教材**

1.  [《PyTorch Handbook》（PyTor](https://github.com/zergtant/pytorch-handbook)ch 中文手册）：专为入门者设计的开源中文教材，GitHub 地址https://github.com/zergtant/pytorch-handbook。内容包含环境搭建、张量、自动求导、神经网络、分类器训练等核心入门知识点，教程中的案例均经过测试可正常运行，还同步适配 PyTorch 版本更新，同时翻译了官方 60 分钟快速入门教程，适合零基础快速熟悉 PyTorch 基础用法。

1.  **pytorch-book（《深度学习框架** PyTorch：入门与实践[》开源配套）：GitHub 开源仓库地址h](https://github.com/chenyuntc/pytorch-book)ttps://github.com/chenyuntc/pytorch-book，虽对应纸质书，但仓库包含书中 50% 以上文字内容和 90% 以上代码，入门章节内容几乎完整保留。内容分基础和实战两部分，基础部分讲解张量、autograd、神经网络模块等；实战部分有动漫头像生成、唐诗创作、图像描述等趣味项目，代码兼容 PyTorch 1.0+，支持 CPU 和 GPU 运行，无需买书就能完成系统学习。

1.  《动手学深度学习》（PyTorch 版）：李沐等人编写的开源教材，中文在线地址https://zh.d2l.ai/。教材与 PyTorch 深度绑定，所有案例均基于该框架实现，从张量运算到复杂神经网络搭建逐步深入，每节都配有可直接运行的 Jupyter Notebook 代码，既能学习深度学习理论，又能同步掌握 PyTorch 实操，适合结合视频课程同步学习。

1.  MorvanZhou[ PyTorch-Tutorial：GitHub 地](https://github.com/MorvanZhou/PyTorch-Tutorial)址https://github.com/MorvanZhou/PyTorch-Tutorial，是莫烦 Python 推出的简明开源教程。内容以短小精悍的案例为主，涵盖神经网络、CNN、RNN 等基础模型的 PyTorch 实现，代码注释详细，讲解风格通俗易懂，还配有对应的 B 站视频教程，适合喜欢 “边看代码边学概念” 的入门者。

# **官方核心入门教程**

1.  PyTor[c](https://pytorch.org/tutorials/beginner/deep_learning_60min_blitz.html)h 官方 60 分钟快速入门：官网地址https://pytorch.org/tutorials/beginner/deep_learning_60min_blitz.html，是官方专为新手打造的入门教程。用短短一小时的案例串联起张量、自动求导、神经网络构建、模型训练等核心知识点，步骤清晰且代码可直接复制运行，中英双语可选，是快速建立 PyTorch 知识框架的首选入门资料。

1.  PyTorch 官方基础教程合集：官网地址https://pytorch.org/tutorials/，除了快速入门外，还包含基于 TorchVision 的图像分类、迁移学习、数据加载等专项教程。教程覆盖从基础语法到实际应用的全流程，适配最新版本的 PyTorch，能帮助入门者解决环境配置、模型保存与加载等实际操作问题。

# **优质免费视频课程**

1.  李沐《动手学[深度学习》PyTorch 版：B ](https://space.bilibili.com/1567748478)站地址https://space.bilibili.com/1567748478。课程与同名开源教材高度契合，李沐老师会逐行拆解 PyTorch 代码逻辑，比如张量运算的底层逻辑、神经网络层的搭建细节等，讲解清晰系统，能解决新手写代码时的逻辑困惑，适合搭配教材同步学习。

1.  莫烦 Python PyTorch 教程：B 站可搜索观看，课程与他的 GitHub 开源教程配套。讲解风格轻松通俗，每个知识点都搭配简短的代码演示，比如用几行代码实现线性回归、简单 CNN 等，适合零基础且怕复杂理论的学习者快速上手。

1.  freeCodeCam[p 6 小时 PyTorch 全教程：YouT](https://www.youtube.com/watch?v=GIsg-ZUy0MY)ube 地址https://www.youtube.com/watch?v=GIsg-ZUy0MY，英文讲解但字幕友好。教程从张量基础到神经网络结构、模型训练技巧一站式覆盖，时长适中，案例连贯，适合想同时提升英文技术文档阅读能力的入门者。

# **入门实战仓库**

1.  **PyTorch 官方示例库**：包含图像分类、文本处理、迁移学习等多个入门级实战案例，代码简洁规范。比如基于 CIFAR - 10 数据集的图像分类案例，完整展示了数据加载、模型定义、训练与评估的全流程，适合模仿学习 PyTorch 的标准开发流程。

1.  PyTorch Examples：GitHub 地址https://github.com/pytorch/examples，官方维护的实战案例仓库。包含 MNIST 手写数字识别、GAN、RNN 文本生成等经典项目，每个案例都是独立的代码文件，结构清晰，可帮助入门者理解 PyTorch 在不同深度学习场景中的应用方式。

# PyTorch中文手册

https://github.com/zergtant/pytorch-handbook

## 书籍介绍

这是一本开源的书籍，目标是帮助那些希望和使用PyTorch进行深度学习开发和研究的朋友快速入门。

由于本人水平有限，在写此教程的时候参考了一些网上的资料，在这里对他们表示敬意，我会在每个引用中附上原文地址，方便大家参考。

深度学习的技术在飞速的发展，同时PyTorch也在不断更新，且本人会逐步完善相关内容。

## 版本说明

由于PyTorch版本更迭，教程的版本会与PyTorch版本，保持一致。

pytorch大版本更新的主要变动总结 当前版本 1.11

## 在线版本和PDF

国内的镜像，速度很快，不会被墙：https://www.pytorch.wiki/

PDF文件目前还没有找到好的生成方法，有熟悉这方面的朋友可以联系我，感激不尽

## 🐧 6群

群号：760443051

[点击链接加入群聊【PyTorch H](https://jq.qq.com/?_wv=1027&k=X4Ro6uWv)andbook 交流6群】：https://jq.qq.com/?_wv=1027&k=X4Ro6uWv

1群(985896536)已满，2群(681980831) 3群(773681699)已满 4群(884017356)已满 5群(894059877)已满

不要再加了

## 新福利

公众账号每日分享干货文章

## 说明

+   修改错别字请直接提issue或PR

+   PR时请注意版本

+   有问题也请直接提issue

感谢

## 目录

### 第一章：PyTorch 入门

1.  [PyTorch 简介](https://github.com/zergtant/pytorch-handbook/blob/master/chapter1/1.1-pytorch-introduction.md)

1.  [PyTorch 环境搭建](https://github.com/zergtant/pytorch-handbook/blob/master/chapter1/1.2-pytorch-installation.md)

1.  PyTorch 深度学习：60分钟快速入门（官方）

+   [张量](https://github.com/zergtant/pytorch-handbook/blob/master/chapter1/1_tensor_tutorial.ipynb)

+   [Autograd：自动求导](https://github.com/zergtant/pytorch-handbook/blob/master/chapter1/2_autograd_tutorial.ipynb)

+   [神经网络](https://github.com/zergtant/pytorch-handbook/blob/master/chapter1/3_neural_networks_tutorial.ipynb)

+   [训练一个分类器](https://github.com/zergtant/pytorch-handbook/blob/master/chapter1/4_cifar10_tutorial.ipynb)

+   [选读：数据并行处理（多GPU）](https://github.com/zergtant/pytorch-handbook/blob/master/chapter1/5_data_parallel_tutorial.ipynb)

1.  [相关资源介绍](https://github.com/zergtant/pytorch-handbook/blob/master/chapter1/1.4-pytorch-resource.md)

### 第二章 基础

#### 第一节 PyTorch 基础

1.  [张量](https://github.com/zergtant/pytorch-handbook/blob/master/chapter2/2.1.1.pytorch-basics-tensor.ipynb)

1.  [自动求导](https://github.com/zergtant/pytorch-handbook/blob/master/chapter2/2.1.2-pytorch-basics-autograd.ipynb)

1.  [神经网络包nn和优化器optm](https://github.com/zergtant/pytorch-handbook/blob/master/chapter2/2.1.3-pytorch-basics-nerual-network.ipynb)

1.  [数据的加载和预处理](https://github.com/zergtant/pytorch-handbook/blob/master/chapter2/2.1.4-pytorch-basics-data-loader.ipynb)

#### 第二节 深度学习基础及数学原理

[深度学习基础及数学原理](https://github.com/zergtant/pytorch-handbook/blob/master/chapter2/2.2-deep-learning-basic-mathematics.ipynb)

#### 第三节 神经网络简介

[神经网络简介](https://github.com/zergtant/pytorch-handbook/blob/master/chapter2/2.3-deep-learning-neural-network-introduction.ipynb) 注：本章在本地使用微软的Edge打开会崩溃，请使Chrome Firefox打开查看

#### 第四节 卷积神经网络

[卷积神经网络](https://github.com/zergtant/pytorch-handbook/blob/master/chapter2/2.4-cnn.ipynb)

#### 第五节 循环神经网络

[循环神经网络](https://github.com/zergtant/pytorch-handbook/blob/master/chapter2/2.5-rnn.ipynb)

### 第三章 实践

#### 第一节 logistic回归二元分类

[logistic回归二元分类](https://github.com/zergtant/pytorch-handbook/blob/master/chapter3/3.1-logistic-regression.ipynb)

#### 第二节 CNN:MNIST数据集手写数字识别

CNN:MNIST数据集手写数字识别

#### 第三节 RNN实例：通过Sin预测Cos

RNN实例：通过Sin预测Cos

### 第四章 提高

#### 第一节 Fine-tuning

[Fine-tuning](https://github.com/zergtant/pytorch-handbook/blob/master/chapter4/4.1-fine-tuning.ipynb)

#### 第二节 可视化

[visdom](https://github.com/zergtant/pytorch-handbook/blob/master/chapter4/4.2.1-visdom.ipynb)

[tensorboardx](https://github.com/zergtant/pytorch-handbook/blob/master/chapter4/4.2.2-tensorboardx.ipynb)

[可视化理解卷积神经网络](https://github.com/zergtant/pytorch-handbook/blob/master/chapter4/4.2.3-cnn-visualizing.ipynb)

#### 第三节 Fast.ai

[Fast.ai](https://github.com/zergtant/pytorch-handbook/blob/master/chapter4/4.3-fastai.ipynb)

#### 第四节 训练的一些技巧

#### 第五节 多GPU并行训练

[多GPU并行计算](https://github.com/zergtant/pytorch-handbook/blob/master/chapter4/4.5-multiply-gpu-parallel-training.ipynb)

#### 补充翻译文章：在PyTorch中使用DistributedDataParallel进行多GPU分布式模型训练

[在PyTorch中使用DistributedDataP](https://github.com/zergtant/pytorch-handbook/blob/master/chapter4/distributeddataparallel)arallel进行多GPU分布式模型训练

### 第五章 应用

#### 第一节 Kaggle介绍

[Kaggle介绍](https://github.com/zergtant/pytorch-handbook/blob/master/chapter5/5.1-kaggle.md)

#### 第二节 结构化数据

[Pytorch处理结构化数据](https://github.com/zergtant/pytorch-handbook/blob/master/chapter5/5.2-Structured-Data.ipynb)

#### 第三节 计算机视觉

Fashion MNIST 图像分类

#### 第四节 自然语言处理

#### 第五节 协同过滤

### 第六章 资源

[torchaudio](https://github.com/zergtant/pytorch-handbook/blob/master/torchaudio/intro.ipynb)

### 第七章 附录

树莓派编译安装 pytorch 1.4

transforms的常用操作总结

pytorch的损失函数总结

pytorch的优化器总结

《深度学习框架PyTorch：入门与实战》

https://github.com/chenyuntc/pytorch-book