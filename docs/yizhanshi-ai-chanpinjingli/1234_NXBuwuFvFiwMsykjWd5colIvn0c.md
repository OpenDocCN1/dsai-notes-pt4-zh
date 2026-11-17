# CLIP

> 来源：[https://v11enp9ok1h.feishu.cn/docx/BwgHdhIFaoj7iNx25TgcyKDDnRg](https://v11enp9ok1h.feishu.cn/docx/BwgHdhIFaoj7iNx25TgcyKDDnRg)

## CLIP模型是什么？它能做什么？

---

CLIP[1]全名Contrastive Language-Image Pretraining，在2021年由OpenAI提出，其核心理念为图文对比学习预训练。与传统的视觉模型不同，CLIP的预训练数据并非标注的图像数据，而是从网络上大量采集的弱监督图文对数据，即我们常说的图片和caption。CLIP收集了4亿规模的图文对数据，旨在通过预训练建模图像与文本的联系。相比传统复杂的交互式的多模态预训练，CLIP的模型极为简单，即我们常说的双塔模型，分别包括图像塔和文本塔。图像塔负责提取图像表征，一般为Vision Transformer，即常说的ViT，文本塔则负责提取文本特征，使用经典Transformer架构。双塔各自提取表征后，batch内部两两互相计算图文表征的内积，如图所示：

![](img/b80edb1e10dd6c177e68541cdeca1c92.png)

原始的CLIP模型基于英文图文语料，不能用于中文的图文表征提取场景。本项目以英文CLIP视觉侧参数和中文Roberta参数，作为模型初始化值。 基于大规模原生中文图文数据，通过如下图所示的二阶段预训练策略（一阶段仅训练文本侧，二阶段同时训练），实现了CLIP模型的中文化版本。

![](img/eb7f99c56fe5cf3a7ac3b0546d9e79cd.png)

模型使用对比学习常用的InfoNCE loss进行训练，本质上即拉近正例及推远负例的距离，实现向量空间内图像与自然语言的联系。经此训练，CLIP模型具备多模态理解能力，其图像文本关联能力能应用于跨模态检索任务，以及开放域的零样本图像分类。同时，CLIP的图像塔具备强大的图像表征能力，能够广泛应用于图像生成、物体检测、分割、甚至视频领域的下游任务等。

## 为什么要推出中文CLIP？

---

推出中文CLIP的原因，在于CLIP模式证明了大规模弱监督图文数据以及大模型就能构建出强大的多模态基础模型及视觉基础模型。然而，基于英语数据训练的CLIP并不足以满足跨语言的复杂场景，尤其当面对博大精深的汉语的时候。今天谈论CLIP我们往往都在谈论其视觉表征能力，而忽略其文本塔的作用。但值得注意的是，不同语言的文本数据的图像数据均大不相同，不仅仅是语言本身的不同，更多的是不同语言的数据所包含的信息对客观现实和主观思想的反映等都大不相同。尽管当前市面上有诸如mCLIP[2]的多语言CLIP模型，但它仍不足以满足对中文的理解，甚至不足以满足对中文世界的图像的理解。例子如下(具体图片大家可以点击链接查看)：

面对[“过年喜庆对联”](https://rom1504.github.io/clip-retrieval/?back=https%3A%2F%2Fknn5.laion.ai&index=laion5B&useMclip=true&query=%E8%BF%87%E5%B9%B4%E5%96%9C%E5%BA%86%E5%AF%B9%E8%81%94)这个搜索query，mCLIP返回的是圣诞相关的事物。

又或者[“周杰伦打篮球”](https://rom1504.github.io/clip-retrieval/?back=https%3A%2F%2Fknn5.laion.ai&index=laion5B&useMclip=true&query=%E5%91%A8%E6%9D%B0%E4%BC%A6%E6%89%93%E7%AF%AE%E7%90%83)，返回的结果也是错误的。

不得不说，中文自然语言处理太难了，而中文视觉理解也太难了！我们需要一个中文的CLIP，扮演中文领域的多模态和视觉基础模型，实现对中文领域多模态和视觉相关应用的“赋能”。

## 中文CLIP是如何实现的？

---

2022年，通义实验室推出了中文CLIP，其方法基本与官方CLIP保持一致，保证了其易用性的同时，还实现了对大规模中文领域图文数据的学习。具体而言，我们首先收集了大规模的中文图文对数据，其中绝大部分来自于公开数据集，其中包括LAION-5B[3]，Wukong[4]，以及英文经典公开数据集如Visual Genome[5]和Microsoft COCO[6]的翻译版本，翻译来自于阿里NLP。因此，本工作保证优秀效果的同时，还保证了高可复现性，方便后续研究人员复现我们的实验效果。

而在预训练方法上，相比于经典的对比学习预训练，我们采用了两阶段对比学习预训练，方法如图所示：

![](img/098aea0a64e40a77bf7b17d4a3900262.png)

首先我们对图像塔和文本塔使用已有的预训练模型初始化，可大幅节约预训练成本加速收敛的同时，还能实现更好的效果。图像塔为ResNet50或不同规模的ViT，使用对应的CLIP的模型权重进行初始化。在第一阶段中，如左图所示，我们首先冻结图像塔的参数，通过对比学习预训练让文本塔的表征在向量空间中距离接近于已经训练好的CLIP图像塔模型的表征，此方法在谷歌工作LiT[7]及华为工作Wukong均得到有效验证。而在第二阶段，我们解除参数的冻结，让所有参数均参与训练，目标是让CLIP模型学习中文领域的图像和文本数据，学习中文领域的图像和自然语言知识。我们在消融实验中，也证明了两阶段训练方法相比于一阶段的有效性，同时也讨论了模型初始化对模型效果的影响。

目前，中文CLIP已经发布在通义实验室模型平台ModelScope。下文将具体介绍如何在ModelScope上使用此模型，实现多种类型的应用。

## 怎么用起来？

我们的中文CLIP模型提供了非常简单易用的接口。在配置好环境之后，只要通过简单的几行代码，就能够完成一个基本的图文特征提取和图文匹配计算的流程，下面我们给出详细的方法：

## 3.1 配置环境#

### 3.1.1 使用ModelScope的notebook#

在我们的模型页面点击右上角的"在Notebook中打开"，根据自己当前的额度，可选使用CPU或者GPU实例，如下图：

step 1: 点击打开Notebook

![](img/d7fda7dbce6657de6c2dc3436542f041.png)

step 2: 选择实例类型

![](img/3f710c1d892b051c3e91394b2db01506.png)

ModelScope的notebook（类似python的notebook）已经预装了相关的环境，所以启动后可以直接使用。step 2中红框部分为环境信息：ubuntu20.04-py37-torch1.11.0-tf1.15.5-1.6.0，分别为操作系统、cuda版本(GPU环境)、python版本、PyTorch版本、TensorFlow版本和ModelScope版本。请注意CLIP运行所需的ModelScope环境版本需要高于0.3.7，如果发现上图红框中显示的ModelScope版本低于这一版本，在"启动"按钮旁边会有一个"更新镜像并启动"的按钮，请点击它，如下图：

![](img/325f3c935e2efe448f6fde147b05a6bc.png)

### 3.1.2 自己动手配置#

使用Mo[de](https://www.modelscope.cn/docs/%E7%8E%AF%E5%A2%83%E5%AE%89%E8%A3%85)lScope进行推断和开发，需要配置好ModelScope的相关运行环境。ModelScope的环境管理主要是依赖基础的深度学习框架：Pytorch和TensorFlow/Pytorch；同时ModelScope集成了各个模态的模型，所以依赖相对比较复杂，因此建议使用Conda进行环境管理，新建一个python环境。详细文档见这里。

建好环境并安装基础深度学习环境后，在shell终端中安装多模态相关依赖库，如下所示：

```
pip install &quot;modelscope[multi-modal]&quot; -f https://modelscope.oss-cn-beijing.aliyuncs.com/releases/repo.html
# ms升级命令
# pip install --upgrade &quot;modelscope[multi-modal]&quot; -f https://modelscope.oss-cn-beijing.aliyuncs.com/releases/repo.html
```

在shell终端中进行一个简单的安装验证（会下载模型并展示进度条，默认下载路径~/.cache/modelscope/）：

python -c &quot;from modelscope.pipelines import pipeline;from modelscope.utils.constant import Tasks;from modelscope.preprocessors.image import load_image;print(pipeline(task=Tasks.multi_modal_embedding).forward({&#x27;img&#x27;: load_image(&#x27;https://yangan2.oss-cn-beijing.aliyuncs.com/pokemon.jpeg&#x27;), &#x27;text&#x27;: &#x27;皮卡丘&#x27;}))&quot;

```
```

看到有向量输出，则安装正常

## 3.2 预测图文表征 & 计算相似度#

如上文所说，CLIP模型通过双塔模型的结构，来为图文计算特征向量。得到的图文特征向量，通过简单的向量内积运算，就能够计算出图文相似度。ModelScope中CLIP模型的推断方式主要是通过Pipeline进行的，仅需几行代码，就可以很简单的调用CLIP模型的推理能力。下面我们分别给出提取图文表征，并进一步实现一个文本query匹配多张图片、和一张图片匹配多个候选文本的代码示例

在python环境中，首先，我们进行必要的import操作

```
import torch
from modelscope.utils.constant import Tasks
from modelscope.pipelines import pipeline
from modelscope.preprocessors.image import load_image
```

之后，我们构建多模态表征的ModelScope pipeline，并载入中文CLIP模型参数（以base规模为例）

```
pipeline = pipeline(task=Tasks.multi_modal_embedding,
    model=&#x27;damo/multi-modal_clip-vit-base-patch16_zh&#x27;) # 载入base规模CLIP模型
```

[目前，我们在](https://modelscope.cn/models/damo/multi-modal_clip-vit-base-patch16_zh)ModelScope上[发布了4个规模](https://modelscope.cn/models/damo/multi-modal_clip-vit-large-patch14_zh)的中文CLIP模型：b[ase规模（ViT-B-16](https://www.modelscope.cn/models/damo/multi-modal_clip-vit-large-patch14_336_zh)[）、larg](https://modelscope.cn/models/damo/multi-modal_clip-vit-huge-patch14_zh)e规模（ViT-L-14）、large规模-336分辨率（ViT-L-14@336px）以及huge规模（ViT-H-14）。用户可以通过指定上面代码中的model参数来载入需要的规模，分别对应为：

+   base规模：damo/multi-modal_clip-vit-base-patch16_zh

+   large规模：damo/multi-modal_clip-vit-large-patch14_zh

+   large规模-336分辨率：damo/multi-modal_clip-vit-large-patch14_336_zh

+   huge规模：damo/multi-modal_clip-vit-huge-patch14_zh

关于这4个模型规模的更详细介绍，请参见附录。

对于一个文本query匹配多张图片，我们使用"过年喜庆对联"这个query，以及下面四张候选图片（图片文件名我们并不会用到）。其中只有过年对联这张图片真正和query相关，其他三张都是不匹配的图片，过年喜庆和对联两张图片作为高难度负例，检验模型的效果

![](img/a007cf496ee9074bed5c8873fec3b485.png)

![](img/bcd7cafe274809dc5decfac4a34fac8c.png)

![](img/d099a901be86de0d2dd3a311b80e8fd4.png)

![](img/9ea878accd4168fa173e28facd4c461f.png)

    load_image(&#x27;https://yangan2.oss-cn-beijing.aliyuncs.com/过年对联.jpeg&#x27;), # 支持示例图片url/本地图片路径 返回PIL.Image
    load_image(&#x27;https://yangan2.oss-cn-beijing.aliyuncs.com/圣诞装饰.png&#x27;), 
    load_image(&#x27;https://yangan2.oss-cn-beijing.aliyuncs.com/过年喜庆.jpeg&#x27;), 
    load_image(&#x27;https://yangan2.oss-cn-beijing.aliyuncs.com/对联.jpeg&#x27;)

```
# 准备文本query和多张候选图片
input_text = &quot;过年喜庆对联&quot;
input_imgs = [
]

# 提取图片特征，支持一张图片(PIL.Image)或多张图片(List[PIL.Image])输入，输出归一化特征向量
img_embedding = pipeline.forward({&#x27;img&#x27;: input_imgs})[&#x27;img_embedding&#x27;] # 2D Tensor, [图片数, 特征维度]

# 提取文本特征，支持一条文本(str)或多条文本(List[str])输入，输出归一化特征向量
text_embedding = pipeline.forward({&#x27;text&#x27;: input_text})[&#x27;text_embedding&#x27;] # 2D Tensor, [文本数, 特征维度]

# 计算图文相似度
with torch.no_grad():
    # 计算内积得到logit，考虑模型temperature（0.01）
    logits_per_text = (text_embedding / pipeline.model.temperature) @ img_embedding.t()
    # 根据logit计算概率分布
    probs = logits_per_text.softmax(dim=-1).cpu().numpy()

# 打印结果
print(&quot;图文相似概率分布:&quot;, probs.tolist()) 
# 图文相似概率分布: [[0.71826171875, 5.364418029785156e-07, 0.28125, 0.0006151199340820312]]
```

可以看到，模型为和文本最为相关的过年对联图片，给出了远高于其他三张图片的相似度概率，两张高难度的负例图并没有能够误导模型，这样我们就完成了一个文本query匹配图片的流程。上述代码执行中，调用pipeline.forward()方法后，我们也分别拿到了图片和文本的归一化特征向量（代码中的img_embedding和text_embedding），可以进一步用于其他的下游任务。

我们另外给出一个一张图片匹配多个候选文本的例子，使用下面这张示例图片，输入"杰尼龟"、"妙蛙种子"、"小火龙"、"皮卡丘"四条候选文本

![](img/340a2ed4dd7ffa8b92ce04cd91a8fc98.png)

input_img = load_image(&#x27;https://yangan2.oss-cn-beijing.aliyuncs.com/pokemon.jpeg&#x27;) # 支持皮卡丘示例图片路径/本地图片 返回PIL.Image

```
input_texts = [&quot;杰尼龟&quot;, &quot;妙蛙种子&quot;, &quot;小火龙&quot;, &quot;皮卡丘&quot;]

img_embedding = pipeline.forward({&#x27;img&#x27;: input_img})[&#x27;img_embedding&#x27;] # 2D Tensor, [图片数, 特征维度]
text_embedding = pipeline.forward({&#x27;text&#x27;: input_texts})[&#x27;text_embedding&#x27;] # 2D Tensor, [文本数, 特征维度]

# 计算图文相似度
with torch.no_grad():
    logits_per_image = (img_embedding / pipeline.model.temperature) @ text_embedding.t()
    probs = logits_per_image.softmax(dim=-1).cpu().numpy()

print(&quot;图文相似概率分布:&quot;, probs)
# 图文相似概率分布: [[1.182e-03 5.023e-02 5.760e-04 9.482e-01]]
```

模型同样给出了正确的预测，"皮卡丘"这条文本获得了最高的图文相似度

## 3.3 模型finetune

目前[ModelScope除](https://tianchi.aliyun.com/muge)了支持中文CLIP的推理功能之外，也支持利用下游图文对数据集，对CLIP参数进行单卡/多卡finetune。这里我们以已经在ModelScope集成的MUGE图文检索数据集为例，展示如何准备一个训练脚本，对中文CLIP进行finetune。

### 3.3.1 新建脚本 & 必要的import

例如我们新建一个训练脚本clip_train_entry.py，其中先完成必要的import操作：

```
# -*- coding: utf-8 -*-
import os

import json
import shutil

from modelscope.metainfo import Metrics, Trainers
# 指标和训练器的Python类
from modelscope.msdatasets import MsDataset
# ModelScope集成数据集的Python类
from modelscope.trainers import build_trainer
# 构建训练器的方法
from modelscope.utils.constant import ModelFile
```

### 3.3.1 准备训练超参#

在刚刚的训练脚本clip_train_entry.py中，我们接下来加入以下代码，定义一个python dict，用来指定finetune使用的模型规模、学习率、训练轮数等超参。具体超参的定义格式和含义如下：

```
finetune_cfg = \
    {
        # 指定训练使用pytorch，无需修改
        &#x27;framework&#x27;: &#x27;pytorch&#x27;, 
        # 训练多模态表征任务（中文CLIP所支持的任务），无需修改
        &#x27;task&#x27;: &#x27;multi-modal-embedding&#x27;, 
        # pipeline为多模态表征，无需修改
        &#x27;pipeline&#x27;: {&#x27;type&#x27;: &#x27;multi-modal-embedding&#x27;}, 
        # 使用的模型规模（以base为例），参见3.2和附录
        &#x27;pretrained_model&#x27;: {&#x27;model_name&#x27;: \
            &#x27;damo/multi-modal_clip-vit-base-patch16_zh&#x27;}, 
        # 图片和文本在数据集的字段名，这里以MUGE为例
        &#x27;dataset&#x27;: {&#x27;column_map&#x27;: {&#x27;img&#x27;: &#x27;image&#x27;, &#x27;text&#x27;: &#x27;query&#x27;}}, 
        # 模型和训练日志存储目录，最终ckpt将存放于$work_dir/output/目录
        &#x27;train&#x27;: {&#x27;work_dir&#x27;: &#x27;./workspace/ckpts/clip&#x27;, 
                # 多卡训练请解除下面这行注释，单卡保留
                # &#x27;launcher&#x27;: &#x27;pytorch&#x27;,
                # 训练轮数
                &#x27;max_epochs&#x27;: 1,
                # 使用混合精度训练，无需修改
                &#x27;use_fp16&#x27;: True,
                # 训练单卡batch size，根据显存实际情况决定
                &#x27;dataloader&#x27;: {&#x27;batch_size_per_gpu&#x27;: 180,
                                # 训练DataLoader使用进程数，0代表直接主进程读取数据
                                &#x27;workers_per_gpu&#x27;: 16,
                                # 是否shuffle，无需修改
                                &#x27;shuffle&#x27;: True,
                                # 丢弃每轮不够整batch的末尾数据，无需修改
                                &#x27;drop_last&#x27;: True},
                # 指定学习率warmup的过程占总步数的比例
                &#x27;lr_scheduler&#x27;: {&#x27;warmup_proportion&#x27;: 0.1},
                # 使用学习率scheduler（cosine scheduler），无需修改
                &#x27;lr_scheduler_hook&#x27;: {&#x27;type&#x27;: &#x27;LrSchedulerHook&#x27;, &#x27;by_epoch&#x27;: False},
                # 优化器类型
                &#x27;optimizer&#x27;: {&#x27;type&#x27;: &#x27;AdamW&#x27;},
                # 指定峰值学习率（warmup到峰值后最终decay到0）
                &#x27;optimizer_hparams&#x27;: {&#x27;lr&#x27;: 2.5e-05, 
                                        # 指定weight_decay，无需修改
                                        &#x27;weight_decay&#x27;: 0.001, 
                                        # adam的超参，一般无需修改
                                        &#x27;beta1&#x27;: 0.9, 
                                        &#x27;beta2&#x27;: 0.999,
                                        &#x27;eps&#x27;: 1e-08},
                # 混合精度训练相关超参，一般无需修改
                &#x27;optimizer_hook&#x27;: {&#x27;type&#x27;: &#x27;TorchAMPOptimizerHook&#x27;,
                                    &#x27;cumulative_iters&#x27;: 1,
                                    &#x27;loss_keys&#x27;: &#x27;loss&#x27;},
                # 多卡训练时，是否通过gpu通信，在global batch上计算对比学习loss，单卡不起作用
                &#x27;loss_cfg&#x27;: {&#x27;aggregate&#x27;: True},
                # 每隔多少步保存一组验证指标最优的参数
                &#x27;hooks&#x27;: [{&#x27;type&#x27;: &#x27;BestCkptSaverHook&#x27;,
                            # 中文CLIP finetune使用in-batch 文到图检索Recall@1
                            #（注意和全局文到图检索Recall@1指标不同，仅在batch内部计算Recall）
                            &#x27;metric_key&#x27;: &#x27;inbatch_t2i_recall_at_1&#x27;,
                            &#x27;by_epoch&#x27;: False,
                            &#x27;interval&#x27;: 200},
                            {&#x27;type&#x27;: &#x27;TextLoggerHook&#x27;, &#x27;interval&#x27;: 1},
                            {&#x27;type&#x27;: &#x27;IterTimerHook&#x27;},
                            # 评测间隔，可按照步数和轮数间隔指定
                            {&#x27;type&#x27;: &#x27;EvaluationHook&#x27;, &#x27;by_epoch&#x27;: False, &#x27;interval&#x27;: 200},
                            {&#x27;type&#x27;: &#x27;EvaluationHook&#x27;, &#x27;by_epoch&#x27;: True, &#x27;interval&#x27;: 1},
                            {&#x27;type&#x27;: &#x27;ClipClampLogitScaleHook&#x27;}]},
        # 验证时，计算in-batch 文到图检索Recall@1使用的batch size
        &#x27;evaluation&#x27;: {&#x27;dataloader&#x27;: {&#x27;batch_size_per_gpu&#x27;: 128,
                                    # 验证数据DataLoader使用进程数，0代表直接主进程读取数据
                                    &#x27;workers_per_gpu&#x27;: 16,
                                    &#x27;shuffle&#x27;: False,
                                    &#x27;drop_last&#x27;: True},
                    # 中文CLIP finetune使用in-batch 文到图检索Recall@1，无需修改
                    &#x27;metrics&#x27;: [{&#x27;type&#x27;: &#x27;inbatch_recall&#x27;}]},
        &#x27;preprocessor&#x27;: []}
```

### 3.3.2 执行训练任务#

在训练脚本clip_train_entry.py中，我们最后加入以下代码，初始化输出目录、获取集成的MUGE数据集、同时构建训练器

```


if __name__ == &quot;__main__&quot;:
    # 初始化输出目录，请和上面dict使用相同路径
    WORKSPACE = &#x27;./workspace/ckpts/clip&#x27;
    os.makedirs(WORKSPACE, exist_ok=True)
    config_file = os.path.join(WORKSPACE, ModelFile.CONFIGURATION)
    with open(config_file, &#x27;w&#x27;) as writer:
        json.dump(finetune_cfg, writer) # 保存训练超参

    # 指定预训练使用的CLIP规模，请和上面dict的&#x27;pretrained_model&#x27;保持一致
    pretrained_model = &#x27;damo/multi-modal_clip-vit-base-patch16_zh&#x27;
    args = dict(
        model=pretrained_model,
        work_dir=WORKSPACE,
        # 获取集成的MUGE数据集，脚本第一次执行时自动下载
        train_dataset=MsDataset.load(
            &#x27;muge&#x27;, namespace=&#x27;modelscope&#x27;, split=&#x27;train&#x27;),
        eval_dataset=MsDataset.load(
            &#x27;muge&#x27;, namespace=&#x27;modelscope&#x27;, split=&#x27;validation&#x27;),
        # CLIP验证时使用的指标，无需修改
        metrics=[Metrics.inbatch_recall],
        # 传入3.3.1定义的dict
        cfg_file=config_file)
    # 构建训练器
    trainer = build_trainer(
        name=Trainers.clip_multi_modal_embedding, default_args=args)
    # 启动训练
    trainer.train()
```

准备好训练脚本clip_train_entry.py后，我们用以下命令启动训练：

单卡训练任务：

```
export CUDA_VISIBLE_DEVICES=0;python3 clip_train_entry.py
```

多卡训练任务（请在3.3.1中解除launcher的注释）：

```
# CUDA_VISIBLE_DEVICES和WORLD_SIZE根据实际情况而定
export CUDA_VISIBLE_DEVICES=0,1,2,3,4,5,6,7;
export WORLD_SIZE=8;
# 同台机器跑多组请替换不同的端口
export port=9932;
python -m torch.distributed.launch --nproc_per_node=$WORLD_SIZE --nnodes=1 --node_rank=0 --master_port=9932 --use_env clip_train_entry.py
```

训练过程中，将打印如下的log，显示学习率、训练loss、显存占用、预估完成时间等监控信息

```
INFO:modelscope:epoch [1][2/1390]       lr: 1.799e-07, eta: 2:35:20, iter_time: 0.499, data_load_time: 0.047, memory: 29279, loss: 1.1768, logit_scale: 4.6052, global_batch_size: 180.0000
```

### 3.3.3 训练指标 & 获取finetune ckpt#

训练过程中，根据预设的验证步数/轮数间隔，将在验证集进行评测，并给出验证指标。中文CLIP finetune时，使用验证集in-batch文到图检索Recall@1作为评测指标。

```
2022-10-31 12:36:06,344 - modelscope - INFO - epoch(eval) [1][238]      memory: 30721, inbatch_t2i_recall_at_1: 0.8284, loss: 1.0088, logit_scale: 4.6046, global_batch_size: 180.0000
```

注意此指标和中文CLIP modelcard中，列出的全局文到图检索Recall@1指标不同，仅在一个batch内部进行召回和计算Recall，和全局指标并不是直接可比的。如果要计算全局文到图检索Recall@1指标，需要在全量验证集图片中，为每个文本query计算并召回最相关的图片出来。

在训练结束后，在预先指定的输出路径下，会找到一个output文件夹，其中的pytorch_model.bin文件即使finetune好的ckpt。

## 3.4 关键配置介绍

### 3.4.1 模型构建相关配置

目前我们每个规模的CLIP模型，都有对应的配置json文件。由于CLIP模型由双塔构成，所以视觉侧和文本侧各有一个json配置文件，分别命名为vision_model_config.json和text_model_config.json。这两个文件会在执行加载模型代码的时候，随着模型参数一起下载好（参见3.2部分"构建pipeline & 载入模型"代码）。这两个json配置文件，指定了模型适配分辨率、层数、隐层维度等基本结构超参。下面我们以base规模CLIP模型为例，介绍json配置中具体的超参配置项。关于现有各个CLIP模型规模的细节，请参见附录部分

模型结构配置文件，默认应位于~/.cache/modelscope/hub/damo/multi-modal_clip-vit-base-patch16_zh/文件夹。如果此目录不存在，请执行以下python命令，获取实际的模型下载文件夹

```
from modelscope.hub.snapshot_download import snapshot_download
print(snapshot_download(&#x27;damo/multi-modal_clip-vit-base-patch16_zh&#x27;)) # 将打印实际的模型下载文件夹
```

对于视觉侧，配置文件为该文件夹下vision_model_config.json文件，其json格式如下

```
{
    &quot;embed_dim&quot;: 512, # 输出的向量特征维度
    &quot;image_resolution&quot;: 224, # 适配图片分辨率，输入的原始图片会被ModelScope自动缩放到这一分辨率
    &quot;vision_layers&quot;: 12, # 视觉侧Transformer模型层数
    &quot;vision_width&quot;: 768, # 视觉侧Transformer隐层维度
    &quot;vision_patch_size&quot;: 16 # 视觉Transformer切图片patch的大小
}
```

对于文本侧，打开该文件夹下text_model_config.json文件，其json格式如下

```
{
    &quot;vocab_size&quot;: 21128, # 词表大小
    &quot;text_attention_probs_dropout_prob&quot;: 0.1, # 注意力值的神经元丢弃概率，沿袭自BERT，仅用于finetune
    &quot;text_hidden_act&quot;: &quot;gelu&quot;, # 激活函数类型
    &quot;text_hidden_dropout_prob&quot;: 0.1, # 神经元丢弃概率，沿袭自BERT，仅用于finetune
    &quot;text_hidden_size&quot;: 768, # 文本侧隐层维度
    &quot;text_initializer_range&quot;: 0.02, # 文本侧初始化分布range，沿袭自BERT
    &quot;text_intermediate_size&quot;: 3072, # 文本侧全连接层中间维度
    &quot;text_max_position_embeddings&quot;: 512, # 文本侧位置编码最大长度
    &quot;text_num_attention_heads&quot;: 12, # 文本侧注意力头数
    &quot;text_num_hidden_layers&quot;: 12, # 文本侧模型层数
    &quot;text_type_vocab_size&quot;: 2  # 文本侧type编码类型数，沿袭自BERT
}
```

## 3.5 核心代码介绍

对于中文CLIP模型来说，核心代码是如何构建模型，文[本侧的分](https://modelscope.cn/docs)词器我们沿用中文BERT分词代码；ModelScope在封装中文CLIP模型代码的基础上，完成Pipeline构建以及finetune和推理的支持。接下来会重点介绍CLIP模型实现相关的核心代码（位于ModelScope代码库modelscope/models/multi_modal/clip/路径下）。关于ModelScope框架侧的代码，可以重点参考ModelScope自身的文档中心。

### 3.5.1 文本Tokenizer#

如上，我们直接复用了中文BERT分词器代码，词表文件vocab.txt与模型参数一起在构建模型时下载（见3.2），分词器实现代码请参见modelscope/models/multi_modal/clip/bert_tokenizer.py，下面给出分词基本过程。此过程已经集成在ModelScope内部pipeline中，用户提取文本特征时无需执行这些代码，直接按照3.2的代码，直接输入原始文本即可。这里只是展示出内部工作原理：

```
from modelscope.models.multi_modal.clip.bert_tokenizer import FullTokenizer
from modelscope.hub.snapshot_download import snapshot_download
# 构建分词器
model_dir = snapshot_download(&#x27;damo/multi-modal_clip-vit-base-patch16_zh&#x27;)
vocab_file = f&#x27;{model_dir}/vocab.txt&#x27;
tokenizer = FullTokenizer(vocab_file=vocab_file)
# 进行分词
text = &quot;过年喜庆对联&quot;
context_length = 52 # 文本序列最大长度，设定为52，即除CLS与SEP之外最多50字
tokens = tokenizer.convert_tokens_to_ids(
                    tokenizer.tokenize(text)
            )[:context_length - 2]
tokens = [tokenizer.vocab[&#x27;[CLS]&#x27;]] + tokens + [tokenizer.vocab[&#x27;[SEP]&#x27;]]
print(tokens) # 按字切分并添加CLS和SEP符号，[101, 6814, 2399, 1599, 2412, 2190, 5468, 102]
```

### 3.5.2 图片侧预处理#

以下代码（参见modelscope/preprocessors/multi_modal.py）对输入的PIL.Image类型图片进行预处理，转化为torch Tensor。我们这里处理的方式和英文CLIP标准做法一致。此过程已经集成在ModelScope内部pipeline中，用户提取文本特征时无需执行这些代码，直接按照3.2的代码，用图片文件路径或者图片url，读入原始图片即可。这里只是展示出内部工作原理：


image_input = load_image(&#x27;https://yangan2.oss-cn-beijing.aliyuncs.com/pokemon.jpeg&#x27;)

```
from PIL import Image
from torchvision.transforms import Compose, Normalize, Resize, ToTensor
from modelscope.preprocessors.image import load_image

# 图片转成RGB
def _convert_to_rgb(image):
    return image.convert(&#x27;RGB&#x27;)

# 预处理算子构建函数
def image_transform(image_size=224):
    transform = Compose([
        _convert_to_rgb,
        Resize((image_size, image_size)),
        ToTensor(),
        Normalize((0.48145466, 0.4578275, 0.40821073),
                  (0.26862954, 0.26130258, 0.27577711)),
    ])
    return transform
image_resolution = 224 # 分辨率，实际执行时从3.4.1提到的视觉侧配置接收
img_preprocess = image_transform(image_resolution) # 输入分辨率，返回一个预处理算子
image_tensor = img_preprocess(image_input) # torch Tensor
```

### 3.5.3 CLIP模型实现代码#

主要实现代码请参见modelscope/models/multi[_modal/clip/mod](https://arxiv.org/abs/2010.11929)el.py。在CLIP模型视觉侧，采用经典的视觉Transformer模型结构实现，自上而下主要包含以下类定义的结构模块：

+   VisualTransformer：视觉Transformer模型的最上层模块，接收3.4.1中提到的视觉侧各项结构配置，完成图像输入多层Transformer前的embedding编码、多层Transformer计算以及最后的投影变换

+   Transformer：一个简单的Transformer实现模块，负责执行VisualTransformer中主体的多层Transformer计算部分

+   ResidualAttentionBlock：实现一层Transformer运算的模块，Transformer中将多个ResidualAttentionBlock堆叠在一起

+   QuickGELU：视觉Transformer使用的激活函数

在CLIP文本侧，我们直接沿用经典的BERT代码，定义BERT结构模块：

+   BertConfig（configuration_bert.py）：BERT相关配置项，接收3.4.1中提到的文本侧各项结构配置

+   BertModel（modeling_bert.py）：经典的BERT模型实现代码

基于视觉侧和文本侧的模型定义，model.py中的CLIP类封装双塔的模型实现，作为中文CLIP模型的模型定义模块。在CLIP类定义之上，再封装上读取预训练参数、输出向量的归一化和向ModelScope pipeline发送结果等操作，就得到了ModelScope调用CLIP模型的最上层类CLIPForMultiModalEmbedding。实际finetune和推理时，用户输入的图片和文本先通过modelscope/preprocessors/multi_modal.py中定义的CLIPPreprocessor，完成3.5.1和3.5.2的操作，得到Tensor格式图文输入数据，最后在CLIPForMultiModalEmbedding完成特征计算和结果返回。

## 效果展示

---

中文[C](https://computer-vision-in-the-wild.github.io/eccv-2022/)LIP在一系列多模态和视觉下游任务上进行了实验，以验证其有效性，其中包括电商领域的图像检索MUGE数据集（https://tianchi.aliyun.com/muge），通用领域跨模态检索Flickr30K-CN[8]和COCO-CN[9]数据集，以及20个视觉数据集的零样本图像分类（https://computer-vision-in-the-wild.github.io/eccv-2022/）。

检索任务上，中文CLIP在3个数据集的零样本场景和finetune场景上均取得显著超出此前SOTA模型的效果，如下表所示：

![](img/90022627004506dabf93234ed5e6b236.png)

![](img/c064cf9666bc147f77978abb37453c82.png)

![](img/50bf9b71612a35bdbd68c97d6f52df50.png)

实验主要采用Recall@K作为评估指标，其中具体包括R@1，R@5，R@10以及Mean Recall。上述结果反映，方法简单的CN-CLIP经过充分训练后，可表现优于同等规模的SOTA模型，并且随着规模的增大以及图像分辨率的提升，模型的效果可以进一步提升。

此外，我们还检验了中文CLIP的零样本图像分类的能力。由于当前中文领域缺乏权威的图像分类数据集，我们选择了近期微软提出的Computer Vision in the Wild中的图像分类系列，其中包括20个分类数据集，绝大多数均为经典的英文图像分类数据集，包括Caltech-101、CIFAR-10、CIFAR-100、Country211、DTD、EuroSAT、F[oo](https://computer-vision-in-the-wild.github.io/eccv-2022/)d-101、FGVC-Aircraft、GTSRB、Hateful-Memes、Kitti-Distance、Oxford-Flowers-102、Oxford-Pets、Patch-Camelyon、RESISC-45、Stanford-Cars、VOC-2007。我们使用中文CLIP参加了以该比赛为核心展开的ECCV 2022 workshop：Workshop on Computer Vision in the Wild，详见链接。我们将20个数据的标签以及用于零样本分类的prompt均通过人工翻译成中文，然后使用中文CLIP进行分类。

![](img/7371cac5aed1bf5eea6865bc4113d671.png)

其中中文CLIP随着规模的扩大，在平均分上能够超越官方提供的基线模型，并且在多个数据集上，Huge规模模型优于Large规模的baseline。由于许多数据集领域与中文数据无关、如汽车品类分类、英文文本分类、飞机品牌与型号分类、食物分类等，中文CLIP在许多数据集不占优势，但在如CIFAR-10、CIFAR-100和VOC-2007这类通用事物分类上，中文CLIP均有较为明显的优势。RN50规模近期也将发布。

## 总结

本文介绍了通义实验室近期提出的中文CLIP，包括其实现方法及实验结果，并重点介绍了中文CLIP在ModelScope上的使用方法，目前已有多个版本开源。用户可在ModelScope平台实现基于中文CLIP的图文特征提取，从而应用于跨模态检索和零样本分类等。

# 附录：中文CLIP各规模模型结构信息

![](img/432756f0c34f28154313a8d5ee4bd15c.png)