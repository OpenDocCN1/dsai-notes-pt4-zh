
# CLIP

> 来源：[https://v11enp9ok1h.feishu.cn/docx/BwgHdhIFaoj7iNx25TgcyKDDnRg](https://v11enp9ok1h.feishu.cn/docx/BwgHdhIFaoj7iNx25TgcyKDDnRg)

## CLIP模型是什么？它能做什么？

---

CLIP[1]全名Contrastive Language-Image Pretraining，在2021年由OpenAI提出，其核心理念为图文对比学习预训练。与传统的视觉模型不同，CLIP的预训练数据并非标注的图像数据，而是从网络上大量采集的弱监督图文对数据，即我们常说的图片和caption。CLIP收集了4亿规模的图文对数据，旨在通过预训练建模图像与文本的联系。相比传统复杂的交互式的多模态预训练，CLIP的模型极为简单，即我们常说的双塔模型，分别包括图像塔和文本塔。图像塔负责提取图像表征，一般为Vision Transformer，即常说的ViT，文本塔则负责提取文本特征，使用经典Transformer架构。双塔各自提取表征后，batch内部两两互相计算图文表征的内积，如图所示：

![](img/b80edb1e10dd6c177e68541cdeca1c92.png)

原始的CLIP模型基于英文图文语料，不能用于中文的图文表征提取场景。本项目以英文CLIP视觉侧参数和中文Roberta参数，作为模型初始化值。基于大规模原生中文图文数据，通过如下图所示的二阶段预训练策略（一阶段仅训练文本侧，二阶段同时训练），实现了CLIP模型的中文化版本。

![](img/eb7f99c56fe5cf3a7ac3b0546d9e79cd.png)

模型使用对比学习常用的InfoNCE loss进行训练，本质上即拉近正例及推远负例的距离，实现向量空间内图像与自然语言的联系。经此训练，CLIP模型具备多模态理解能力，其图像文本关联能力能应用于跨模态检索任务，以及开放域的零样本图像分类。同时，CLIP的图像塔具备强大的图像表征能力，能够广泛应用于图像生成、物体检测、分割、甚至视频领域的下游任务等。

## 为什么要推出中文CLIP？

---

推出中文CLIP的原因，在于CLIP模式证明了大规模弱监督图文数据以及大模型就能构建出强大的多模态基础模型及视觉基础模型。然而，基于英语数据训练的CLIP并不足以满足跨语言的复杂场景，尤其当面对博大精深的汉语的时候。今天谈论CLIP我们往往都在谈论其视觉表征能力，而忽略其文本塔的作用。但值得注意的是，不同语言的文本数据的图像数据均大不相同，不仅仅是语言本身的不同，更多的是不同语言的数据所包含的信息对客观现实和主观思想的反映等都大不相同。尽管当前市面上有诸如mCLIP[2]的多语言CLIP模型，但它仍不足以满足对中文的理解，甚至不足以满足对中文世界的图像的理解。例子如下(具体图片大家可以点击链接查看)：

面对`"过年喜庆对联"`这个搜索query，mCLIP返回的是圣诞相关的事物。

又或者`"周杰伦打篮球"`，返回的结果也是错误的。

不得不说，中文自然语言处理太难了，而中文视觉理解也太难了！我们需要一个中文的CLIP，扮演中文领域的多模态和视觉基础模型，实现对中文领域多模态和视觉相关应用的“赋能”。

## 中文CLIP是如何实现的？

---

2022年，通义实验室推出了中文CLIP，其方法基本与官方CLIP保持一致，保证了其易用性的同时，还实现了对大规模中文领域图文数据的学习。具体而言，我们首先收集了大规模的中文图文对数据，其中绝大部分来自于公开数据集，其中包括LAION-5B[3]，Wukong[4]，以及英文经典公开数据集如Visual Genome[5]和Microsoft COCO[6]的翻译版本，翻译来自于阿里NLP。因此，本工作保证优秀效果的同时，还保证了高可复现性，方便后续研究人员复现我们的实验效果。

而在预训练方法上，相比于经典的对比学习预训练，我们采用了两阶段对比学习预训练，方法如图所示：

![](img/098aea0a64e40a77bf7b17d4a3900262.png)

首先我们对图像塔和文本塔使用已有的预训练模型初始化，可大幅节约预训练成本加速收敛的同时，还能实现更好的效果。图像塔为ResNet50或不同规模的ViT，使用对应的CLIP的模型权重进行初始化。在第一阶段中，如左图所示，我们首先冻结图像塔的参数，通过对比学习预训练让文本塔的表征在向量空间中距离接近于已经训练好的CLIP图像塔模型的表征，此方法在谷歌工作LiT[7]及华为工作Wukong均得到有效验证。而在第二阶段，我们解除参数的冻结，让所有参数均参与训练，目标是让CLIP模型学习中文领域的图像和文本数据，学习中文领域的图像和自然语言知识。我们在消融实验中，也证明了两阶段训练方法相比于一阶段的有效性，同时也讨论了模型初始化对模型效果的影响。

目前，中文CLIP已经发布在通义实验室模型平台`ModelScope`。下文将具体介绍如何在`ModelScope`上使用此模型，实现多种类型的应用。

## 怎么用起来？

我们的中文CLIP模型提供了非常简单易用的接口。在配置好环境之后，只要通过简单的几行代码，就能够完成一个基本的图文特征提取和图文匹配计算的流程，下面我们给出详细的方法：

### 3.1 配置环境

#### 3.1.1 使用ModelScope的notebook

在我们的模型页面点击右上角的"在Notebook中打开"，根据自己当前的额度，可选使用CPU或者GPU实例，如下图：

step 1: 点击打开Notebook

![](img/d7fda7dbce6657de6c2dc3436542f041.png)

step 2: 选择实例类型

![](img/3f710c1d892b051c3e91394b2db01506.png)

ModelScope的notebook（类似`python`的notebook）已经预装了相关的环境，所以启动后可以直接使用。step 2中红框部分为环境信息：`ubuntu20.04-py37-torch1.11.0-tf1.15.5-1.6.0`，分别为操作系统、cuda版本(GPU环境)、`python`版本、PyTorch版本、TensorFlow版本和ModelScope版本。请注意CLIP运行所需的ModelScope环境版本需要高于`0.3.7`，如果发现上图红框中显示的ModelScope版本低于这一版本，在"启动"按钮旁边会有一个"更新镜像并启动"的按钮，请点击它，如下图：

![](img/325f3c935e2efe448f6fde147b05a6bc.png)

#### 3.1.2 自己动手配置

使用`ModelScope`进行推断和开发，需要配置好`ModelScope`的相关运行环境。`ModelScope`的环境管理主要是依赖基础的深度学习框架：`Pytorch`和`TensorFlow`/`Pytorch`；同时`ModelScope`集成了各个模态的模型，所以依赖相对比较复杂，因此建议使用`Conda`进行环境管理，新建一个`python`环境。详细文档见这里。

建好环境并安装基础深度学习环境后，在`shell`终端中安装多模态相关依赖库，如下所示：

```shell
pip install "modelscope[multi-modal]" -f https://modelscope.oss-cn-beijing.aliyuncs.com/releases/repo.html
# ms升级命令
# pip install --upgrade "modelscope[multi-modal]" -f https://modelscope.oss-cn-beijing.aliyuncs.com/releases/repo.html
```

在`shell`终端中进行一个简单的安装验证（会下载模型并展示进度条，默认下载路径`~/.cache/modelscope/`）：

```shell
python -c "from modelscope.pipelines import pipeline;from modelscope.utils.constant import Tasks;from modelscope.preprocessors.image import load_image;print(pipeline(task=Tasks.multi_modal_embedding).forward({'img': load_image('https://yangan2.oss-cn-beijing.aliyuncs.com/pokemon.jpeg'), 'text': '皮卡丘'}))"
```

看到有向量输出，则安装正常

### 3.2 预测图文表征 & 计算相似度

如上文所述，CLIP模型通过双塔模型的结构，来为图文计算特征向量。得到的图文特征向量，通过简单的向量内积运算，就能够计算出图文相似度。`ModelScope`中CLIP模型的推断方式主要是通过`Pipeline`进行的，仅需几行代码，就可以简单的调用CLIP模型的推理能力。下面我们分别给出提取图文表征，并进一步实现一个文本query匹配多张图片、和一张图片匹配多个候选文本的代码示例

在`python`环境中，首先，我们进行必要的`import`操作

```python
import torch
from modelscope.utils.constant import Tasks
from modelscope.pipelines import pipeline
from modelscope.preprocessors.image import load_image
```

之后，我们构建多模态表征的`ModelScope` pipeline，并载入中文CLIP模型参数（以`base`规模为例）

```python
pipeline = pipeline(task=Tasks.multi_modal_embedding,
    model='damo/multi-modal_clip-vit-base-patch16_zh') # 载入base规模CLIP模型
```

目前，我们在ModelScope上发布了4个规模的中文CLIP模型：`base`规模（`ViT-B-16`）、`large`规模（`ViT-L-14`）、`large`规模-336分辨率（`ViT-L-14@336px`）以及`huge`规模（`ViT-H-14`）。用户可以通过指定上面代码中的`model`参数来载入需要的规模，分别对应为：

+ `base`规模：`damo/multi-modal_clip-vit-base-patch16_zh`
+ `large`规模：`damo/multi-modal_clip-vit-large-patch14_zh`
+ `large`规模-336分辨率：`damo/multi-modal_clip-vit-large-patch14_336_zh`
+ `huge`规模：`damo/multi-modal_clip-vit-huge-patch14_zh`

关于这4个模型规模的更详细介绍，请参见附录。

对于一个文本query匹配多张图片，我们使用"过年喜庆对联"这个query，以及下面四张候选图片（图片文件名我们并不会用到）。其中只有过年对联这张图片真正和query相关，其他三张都是不匹配的图片，过年喜庆和对联两张图片作为高难度负例，检验模型的效果

![](img/a007cf496ee9074bed5c8873fec3b485.png)
![](img/bcd7cafe274809dc5decfac4a34fac8c.png)
![](img/d099a901be86de0d2dd3a311b80e8fd4.png)
![](img/9ea878accd4168fa173e28facd4c461f.png)

```python
load_image('https://yangan2.oss-cn-beijing.aliyuncs.com/过年对联.jpeg') # 支持示例图片url/本地图片路径 返回PIL.Image
load_image('https://yangan2.oss-cn-beijing.aliyuncs.com/圣诞装饰.png')
load_image('https://yangan2.oss-cn-beijing.aliyuncs.com/过年喜庆.jpeg')
load_image('https://yangan2.oss-cn-beijing.aliyuncs.com/对联.jpeg')
```

```python
# 准备文本query和多张候选图片
input_text = "过年喜庆对联"
input_imgs = []

# 提取图片特征，支持一张图片(`PIL.Image`)或多张图片(`List[PIL.Image]`)输入，输出归一化特征向量
img_embedding = pipeline.forward({'img': input_imgs})['img_embedding'] # 2D Tensor, [图片数, 特征维度]

# 提取文本特征，支持一条文本(`str`)或多条文本(`List[str]`)输入，输出归一化特征向量
text_embedding = pipeline.forward({'text': input_text})['text_embedding'] # 2D Tensor, [文本数, 特征维度]

# 计算图文相似度
with torch.no_grad():
    # 计算内积得到logit，考虑模型`temperature`（0.01）
    logits_per_text = (text_embedding / pipeline.model.temperature) @ img_embedding.t()
    # 根据logit计算概率分布
    probs = logits_per_text.softmax(dim=-1).cpu().numpy()

# 打印结果
print("图文相似概率分布:", probs.tolist()) 
# 图文相似概率分布: [[0.71826171875, 5.364418029785156e-07, 0.28125, 0.0006151199340820312]]
```

可以看到，模型为和文本最为相关的过年对联图片，给出了远高于其他三张图片的相似度概率，两张高难度的负例图并没有能够误导模型，这样我们就完成了一个文本query匹配图片的流程。上述代码执行中，调用`pipeline.forward()`方法后，我们也分别拿到了图片和文本的归一化特征向量（代码中的`img_embedding`和`text_embedding`），可以进一步用于其他的下游任务。

我们另外给出一个一张图片匹配多个候选文本的例子，使用下面这张示例图片，输入"杰尼龟"、"妙蛙种子"、"小火龙"、"皮卡丘"四条候选文本

![](img/340a2ed4dd7ffa8b92ce04cd91a8fc98.png)

```python
input_img = load_image('https://yangan2.oss-cn-beijing.aliyuncs.com/pokemon.jpeg') # 支持皮卡丘示例图片路径/本地图片 返回PIL.Image
```
