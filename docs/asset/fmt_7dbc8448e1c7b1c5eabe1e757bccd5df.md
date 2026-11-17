
# GLM-4开源模型零门槛部署与调用指南

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/X1aedswSmoUCUIxo6Pfc0bsAnWh](https://kq4b3vgg5b.feishu.cn/docx/X1aedswSmoUCUIxo6Pfc0bsAnWh)

## GLM-4开源大模型快速入门教程

### 1.ModelScope在线算力与在线环境获取指南

登录魔搭社区：https://www.modelscope.cn/home ，点击注册：

![](img/9599b74d2aaf13e699cb170720cf3cc7.png)

输入账号密码完成注册：

![](img/1922fdd2676629a470a8ef0505fdf6a2.png)

注册完成后，点击个人中心，点击绑定阿里云账号：

在跳转页面中选择登录阿里云，未注册阿里云也可以在当前页面注册：

点击授权：

![](img/1b3cbae23060547413335708f494653d.png)

绑定完成后，点击左侧“我的Notebook”，即可查看当前账号获赠算力情况。对于首次绑定阿里云账号的用户，都会赠送永久免费的CPU环境（8核32G内存）和36小时限时使用的GPU算力（32G内存+24G显存）。这里的GPU算力会根据实际使用情况扣除剩余时间，总共36小时的使用时间完全足够进行前期各项实验。

![](img/5cf5741fd44c08b9ed08999da0686df8.png)

> 关于大模型算力问题，[《大模型技术实战课》（第3期）](https://appZe9inzwc2314.h5.xiaoeknow.com)正式课程会详细介绍各类不同项目所需算力、本地硬件采购建议、以及在线GPU算力获取方法等，帮助大家一站式解决大模型硬件问题。

接下来启动GPU在线算力环境，选择方式二、点击启动：

![](img/df5fe93fcfc5009e6a5bd7aea179339f.png)

稍等片刻即可完成启动，并点击查看`Notebook`：

![](img/e4ffb0db13c74b797a2c7941d95fb78c.png)

即可接入在线`NoteBook`编程环境：

当前`NoteBook`编程环境和`Colab`类似（谷歌提供的在线编程环境），可以直接调用在线算力来完成编程工作，并且由于该服务由`ModelScope`提供，因此当前`NoteBook`已经完成了`CUDA`、`PyTorch`、`Tensorflow`环境配置，并且已经预安装了大模型部署所需各种库，如`Transformer`库、`vLLM`库、`modelscope`库等，并且当前`NoteBook`运行环境是`Ubuntu`操作系统，我们可以通过`Jupyter`中的`Terminal`功能对`Ubuntu`系统进行操作：

进入到命令行界面：

输入`nvidia-smi`，查看当前GPU情况：

该功能也是我们操作远程`Ubuntu`系统的核心功能。

> 目前来说，绝大多数大模型开发项目都需要`Linux`操作系统，不同于大家常用的[Windows图形化操作系统，](https://appZe9inzwc2314.h5.xiaoeknow.com)大多数`Linux`操作系统都是以命令行操作为主，而在《大模型技术实战课》（第3期）课程中，我们也会提供完整详细的`Ubuntu`操作系统安装与使用教学。

此外，`ModelScope` `NoteBooko`还可以一键拉取`ModelScope`上发布的模型或项目，直接在云端环境进行运行和实验。这个点击+号开启新的导航页：

并在导航页下方点击模型库：

即可选择任意模型文档，进行尝试运行：

![](img/80c9c180b1e89151cd751d5d225fedbd.png)

例如我们点击选择`CodeQwen1.5-7B-Chat`，一个基于`Qwen1.5-7B`微调得到的代码模型。点击即可获得一个新的`Jupyter`文件，包含了该模型的说明文档和运行代码（也就是该模型在`ModelScope`上的`readme`文档）：

而如果想要下载某个`Jupyter`文件到本地，只需要选择文件点击右键、选择`Download`，即可通过浏览器将项目文件下载到本地：

![](img/551f398bba52c16039d099095439541e.png)

当然，这里需要注意的是，哪怕当前在线编程环境已经做了适配，但并不一定满足所有`ModelScope`中模型运行要求，既并非每个拉取的`Jupyter`文件都可以直接运行。当前体验课只把`ModelScope`视作在线编程环境，并不会直接`Copy`项目文件代码进行运行。不过无论如何，`ModelScope Notebook`还是为初学者提供了非常友好的、零基础即可入手尝试部署大模型的绝佳实践环境。

接下来我们就借助`ModelScope Notebook`来完成`GLM-4`开源大模型私有化部署。

+ 智谱AI主页：https://www.zhipuai.cn/

+ `Github`主页：https://github.com/THUDM/GLM-4/tree/main

+ `ModelScope`主页：https://www.modelscope.cn/models/ZhipuAI/glm-4-9b-chat/summary

![](img/74b09ee145fe3d39e7f9d05c6b39fbf4.png)

### 2.`GLM-4-9B-chat`模型下载与运行

+ 借助`modelscope`进行模型下载

注，其他环境下需要按照如下方式提前准备好其他相关库，魔搭社区在线环境无需进行下列安装过程。

```
!pip install modelscope
```
