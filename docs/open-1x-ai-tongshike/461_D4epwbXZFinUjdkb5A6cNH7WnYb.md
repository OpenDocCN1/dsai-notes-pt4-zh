
# Task1跑通baseline-阿里云PAI-DSW版本

> 来源：[https://datawhaler.feishu.cn/docx/Ff6ddgKhUoxTO5xoKhocdx1FnKc](https://datawhaler.feishu.cn/docx/Ff6ddgKhUoxTO5xoKhocdx1FnKc)

## 学习第一步：报名赛事！(点击即可跳转)

> 赛事链接：https://tianchi.aliyun.com/competition/entrance/532251

> 直接报名即可，然后可以尝试自己阅读赛题，

> **尝试理解和学习赛事要求**

![](img/6a42cdd7f34d6a5c20dd4609333ca57b.png)

![](img/b589d43821f4eababd1d4e40babec038.png)

> 天池Better Synth多模态大模型数据合成挑战赛 是 Datawhale 2024 年 AI 夏令营第四期的学习活动——

+ 适合想 入门并实践 多模态大模型、数据处理、模型训练 的学习者参与

>

> **温馨提示**：**本赛事需要耗费大量的算力**，跑通此baseline至少需要120G内存以上的A10，整个流程可能需要5小时，大家请自备算力，预留好时间，下面我们会以阿里云为例。

>

> **学习内容提要**：使用已有大模型和技术，在限定的计算资源下合成高质量训练数据，以增强多模态大模型的图像理解能力

内容速览

## 300 分钟 baseline指南！

> **教程贡献者说：**

> 我们整个学习活动，将带你 ——

+ **跑通最简单的Baseline**(相信我，当前baseline已经写到最简单了，但需要的算力和学习实在不是常人&免费方案能hold住的)

+ **了解竞赛核心考察点和难点**

+ 精读Baseline与了解进阶思路

>

> 当前环节目标：**300分钟**跑通“天池Better Synth多模态大模型数据合成挑战赛”！

> **AutoDL版本跳转：**[从零入门多模态大模型数据合成](https://datawhaler.feishu.cn/wiki/PVmkwDClQiKbmOk1e7scYo2Pndd)

>

> 千里之行，始于足下，从这里，开启你的 AI 学习之旅吧！

> 记得在每次完成学习后，写笔记打卡哟~ 及时总结沉淀能让学习事半功倍~

## Step0：开通阿里云PAI-DSW试用

<https://datawhaler.feishu.cn/sync/DkaqdtRoIsuiyAbDBoPcacKUned>

## Step1：在魔搭社区创建PAI实例！（点击即可跳转）

> 链接：https://www.modelscope.cn/my/mynotebook/authorization

> **创建实例，粘贴镜像链接，启动！**

> 阿里云镜像链接：`dsw-registry-vpc.cn-hangzhou.cr.aliyuncs.com/pai-training-algorithm/data-juicer-better-synth:0.0.1`

> autodl镜像链接(社区镜像)：`datawhalechina/self-llm/Better-Synth`

>

> 注意：镜像中已经安装好了相关运行环境，如果不使用该镜像的话，需要自行通过`install.sh`安装环境

> 如果在上一步授权失败，可到阿里云控制台创建 & 打开实例，具体步骤详见下方子步骤

>

> **温馨提示**：由于baseline运行需求，我们需要

+ 额外扩充磁盘空间，至少需要增加50G，需0.1元每小时，

+ 在后面的模型训练和评测阶段，需要跑大概3小时，至少需要100G+以上的内存，需要至少购置阿里云188G内存的GPU规格，大概需要15元每小时

+ 还可自行寻找其他同等算力，需要自行通过`install.sh`安装环境

> 请提前充值(点击即可进入充wufaan值入口)，避免由于欠费被迫停止实例。

>

> **提前给你打个预防针**：这个baseline可能让你十分痛苦地需要反复配置环境

![](img/30935e51e87127a9c901e56321d8eac8.png)

![](img/005309b09e21140053decb958e999640.png)

![](img/97bc7e17bf3f571a3e4c7f69d58896e4.png)

![](img/aac58904f82b9e71b20162e9cc7bd0a2.png)

![](img/ac2f6d551654292179b8be809ad88f32.png)

![](img/cb1045122f1edccd5320f542261e59f5.png)

+ **如果在魔搭无法授权或点击【打开】无法**打开，可到阿里云控制台创建 & 打开实例

![](img/ef64d8c09af91cb416e21b45d6d2f470.png)

![](img/a481a713867e88c374ff08c074a178f6.png)

+ 创建实例

![](img/1b0e55fb8e771e5402dc104377047b23.png)

![](img/ef814fbe410eb610097ee0d174ec7739.png)

![](img/119648376a759fda9fa30fb8d40cbcba.png)

+ 打开实例

![](img/f125b656350588972d132842c68a4811.png)

![](img/3344b889014320a25a7159014df842d1.png)

+ 如果之前试用的额度已经过期，可使用魔搭的免费Notebook实例

![](img/01684422509a5dd5ef8f56fd92813504.png)

![](img/bf7bb6c9924edc95a70a95cedf034bf6.png)

## Step2：300分钟 跑通baseline！

1. 下载赛事所需文件包

> 包括：baseline、必要的开发者套件、基础模型 和相关数据集等

> 复制如下命令，在终端中执行：

```
git clone https://www.modelscope.cn/datasets/Datawhale/better_synth_challenge_baseline.git
```

![](img/4867bb131ff76ffbb40e946c0b554765.png)

+ 待拉取完，得到的文件目录大致如下

![](img/5735421a0ab107043d8f82d8481dc187.png)

> 可尝试使用 `ll better_synth_challenge_baseline/` 查看

![](img/f07b2fab78ede65d5ac4550d711cc21b.png)

1. **下载多模态base模型及数据集 & 相关依赖软件（大概需要60分钟）**

1. 安装必要工具

> 复制如下命令，在终端中执行

```
apt update
apt install axel zip file
pip install modelscope
```

![](img/95f0abda4fa3f0916efc4a9fdffe5bb1.png)

![](img/fd4177192e5ce753869afb783252a0aa.png)

1. 下载模型和相关数据集（约50分钟左右，网速好也会很快～）

> 执行完成以上步骤后 ，复制如下命令，在终端中执行

```
cd better_synth_challenge_baseline
bash download.sh  ###大概需要50分钟
```

![](img/30eab8c4069905263d8bceaaed9bb4ac.png)

+ 如果使用的不是上面指定的镜像，还需要运行 如下命令命令安装相关环境

```
bash install.sh
```

1. **下载BLIP图片描述模型（约20分钟左右）**

> 这个步骤运行前，请确认：

> 如果是其他环境，需要将文件中的 `'/mnt/workspace/'`

> 修改为 `'/root/autodl-tmp/'` 或

> 其他`better_synth_challenge_baseline`文件夹所在的对应的路径

> 执行完成以上步骤后 ，复制如下命令，在终端中执行

```
python download_blip.py

# 如果是在autoDL上，需要修改download_blip.py文件，将下载路径对应修改为
# cache_dir=&#x27;/root/autodl-tmp/better_synth_challenge_baseline/models&#x27;
```

## 接下来正式进入数据合成和模型训练环节！！！

1. 数据处理与合成（大概需要1小时）

> 这个步骤运行前，请确认：

1. 如果是阿里云环境，需要使用镜像：`dsw-registry-vpc.cn-hangzhou.cr.aliyuncs.com/pai-training-algorithm/data-juicer-better-synth:0.0.1`，且需要将实例配置，修改为188G内存的GPU规格

> 记得把 `hangzhou` 切换为自己当前的区域名称

1. 如果是其他环境，需要提前通过 `bash install.sh` 安装环境，

且需要将 `solution/image_captioning.yaml` 文件中的 `'/mnt/workspace/'`

修改为 `'/root/autodl-tmp/'` 或

其他`better_synth_challenge_baseline`文件夹所在的对应的路径

> 需先修改实例配置，修改为188G内存的GPU规格

+ 先关闭实例

+ 按照下图选中对应规格，

+ 然后确定、启动即可

![](img/220ab671bee5cfa305e429bd5eb63872.png)

复制如下命令，在终端中执行

```
dj-process --config solution/image_captioning.yaml
```

![](img/721b0c1a4c1496a0650f26c6b30dbd63.png)

1. 执行模型训练 & 推理测评（大概需要3～4小时）

> 这个步骤运行前，请确认：

1. 如果是阿里云环境，需要使用镜像：`dsw-registry-vpc.cn-hangzhou.cr.aliyuncs.com/pai-training-algorithm/data-juicer-better-synth:0.0.1`

1. 如果是其他环境，需要将 `toolkit/train_mgm_2b_stage_1.sh` 文件中的 `'/mnt/workspace/'`

修改为 `'/root/autodl-tmp/'` 或

其他`better_synth_challenge_baseline`文件夹所在的对应的路径

1. 如果执行过 `bash install.sh`，则无需执行下方的 `git clone https://github.com/modelscope/data-juicer.git` 命令

> 重启实例后，复制如下命令，在终端中执行

```
cd better_synth_challenge_baseline/toolkit
git clone https://github.com/modelscope/data-juicer.git
bash train_mgm_2b_stage_1.sh   ### 大概需要3小时
```

1. 打包构建符合提交要求的zip文件

> 复制如下命令，在终端执行即可，然后下载`submit.zip`文件！

```
better_synth_root_dir=/mnt/workspace/

cd submit

cp -r ${better_synth_root_dir}/better_synth_challenge_baseline/solution .

cp -r ${better_synth_root_dir}/better_synth_challenge_baseline/output/eval_results output/

cp -r ${better_synth_root_dir}/better_synth_challenge_baseline/output/train.sh output/

cp ${better_synth_root_dir}/better_synth_challenge_baseline/output/training_dirs/MGM-2B-Finetune-*/finetuning.log output/training_dirs/MGM-2B-Finetune-image_recaption/

cp ${better_synth_root_dir}/better_synth_challenge_baseline/output/training_dirs/MGM-2B-Pretrain-*/pretrain.log output/training_dirs/MGM-2B-Pretrain-image_recaption/

zip -r submit.zip solution output
```

## Step3：在官网提交结果(submit.zip文件)

> 链接：

> **评分大概需要 10 分钟～** 上传结果后可以在 [ranklist](https://tianchi.aliyun.com/competition/entrance/532251/rankingList) 处看到评分结果哟！**（预期分数在0.5分左右😉~）**

![](img/1654ac46ccf98692338b3fb30bfed64f.png)

## 关闭PAI实例！！！！（点击即可跳转）

<https://datawhaler.feishu.cn/sync/RMHtdxBnqshtNsbJHcqcMma2nLg>

## 🎉 恭喜你拿下了属于你的第一个分数！

### 👍🏻 快来给自己点赞！

### 🛎️ 别忘记打卡&在群里接龙！

![](img/f28212e467794a05e37a72aa28f0e48a.png)

### 📅 然后迎接后续的挑战吧！

> **教程贡献者说：**

> 这个赛题对想要入门多模态语言模型的小伙伴们是一个不错的尝试。对于现阶段的发展来说，可能数据的质量才是模型性能表现的关键，而数据处理又是一个很复杂又很贵的事情，因此是否能够在本次比赛中掌握一些技巧和灵感来运用到之后的工作和模型训练中，是一个不错的点。

> 接下来我们会基于这个赛题进行更多的知识点学习，会涉及到：

1. 简单的Linux操作系统下的环境配置和安装

1. Data Jucier 与 Mini-Gemini 的学习

1. 数据处理的进阶技巧和模型训练阶段的技巧等

>

> **更多详情见如下学习规划表：**

<https://datawhaler.feishu.cn/sync/VAjldPLR5sfTXQbuErlcTNqHngh>

## 🍰 下面正式来了解一下赛题！

### 你知道刚刚发生了什么吗？

> 刚刚我们跑通了Baseline，想必你一定很好奇这背后发生了什么吧？

> 在学习的过程中，不单单要知道如何做，更要知道为什么要这样做。这样我们在遇到同类型的问题时，才能举一反三。

> 一般情况下，拿到一个赛题之后，我们需要明确：

> 1）这是一个什么场景下的赛题；

> 2）这个赛题要解决什么问题。

> 深度思考这些问题，既能帮助我们更好地解决相关问题，更能让我们在后续遇到相关问题时，更好地完成迁移。

## 赛题解读

<https://datawhaler.feishu.cn/sync/XUYLdkGLys3iIqbhxj6cKvxmnSc>

`Data-Juicer`:

一个一站式**多模态**数据处理的`python`库，旨在为大语言模型 (LLM) 提供更高质量、更丰富、更易“消化”的数据。

[`Data-Juicer`](http://8.138.149.181/)r在线试用： Playground。

`TensorRT-LLM`:

一个大模型推理加速库

https://github.com/NVIDIA/TensorRT-LLM?spm=a2c22.12281976.0.0.c1202c2bxZVSWx

## 本次赛事具体是如何评分的？

> 赛事是通过两个多模态模型常用的benchmark进行综合打分

1. TextVQA：该benchmark中，图像对应文本的问题回答数据集

1. MMBench：该benchmark是一个针对大规模多模态模型的新型评估基准。随着视觉语言模型在感知和推理能力方面的显著进步，如何有效地评估这些模型成为了一个主要难题。传统基准如VQAv2和COCO Caption提供了定量性能测量，但在细粒度能力和鲁棒性评估指标方面存在不足。而像OwlEval这样的主观性基准虽然能够全面评价模型能力，但其可扩展性差且易受偏见影响。MMBench设计了一套综合的评估流水线，包含两大核心元素

+ 精心构建的超越现有同类基准的数据集，该数据集包括2,974个经过细致挑选的问题，覆盖了20种不同类型的细粒度技能

+ 引入了创新的CircularEval策略，并结合使用ChatGPT技术来将模型生成的自由格式预测转化为预定义选项，以实现对模型预测的更可靠评估。

![](img/e0eaa1434526330133f781ff8b36a858.png)

### 赛事规定提交的材料是这些——

> 线上赛只需要提交`solution/`，训练脚本，训练日志（`pretrain.log`和`finetune.log`），以及评测推理结果

+ 请将数据处理方案、训练及推理脚本、训练日志、推理结果打包成一个 `zip` 文件，上传至天池平台进行评测。

+ 为保证提交的规范性，务必遵循以下文件打包结构并提交以下所需的文件，请勿添加额外的顶级目录。



```
submit.zip
├── solution
│   ├── readme                                  ########## 介绍您的算法设计和执行流程 ##########
│   ├── requirements.txt                        ########## 第三方 pip 依赖库 ##########
│   └── ...
└── output
    ├── train.sh                                ########## 训练脚本 ##########
    ├── training_dirs
    │   ├── MGM-2B-Pretrain-xxxx                ########## 预训练好的模型 ##########
    │   │   └── pretrain.log                   ########## 预训练日志 ##########
    │   └── MGM-2B-Finetune-xxxx                ########## 微调好的模型 ##########
    │       └── finetune.log                    ########## 微调训练日志 ##########
    └── eval_results                            ########## 推理结果 ##########
        └── MGM-2B-Finetune-xxxx
```

## 来看看在速通指南里，我们具体做了什么

+ 下载基础模型，种子/微调/评测数据集  
+ 基础模型与微调数据集均存放于训练目录中指定位置  
+ 种子数据集存放于`input`目录  
+ 评测数据集存放于`toolkit/eval`目录  

```
bash download.sh # 数据集 + 多模态模型 Gemma-2B，这一步可能时间较长，大约1个小时
```

下载完成后依然会在终端中显示如下信息`Done`

![](img/eb2910a5288240df599361d48f867956.png)

+ 比赛要求使用[`data-juicer`](https://github.com/modelscope/data-juicer)基于上一步中下载的种子数据集进行数据处理与合成  
+ 数据处理与合成相关代码请存放于`solution`目录中，并在`solution/requirements.txt`中添加所需的依赖  
+ 最后也请在`solution/readme`中详细介绍所使用的数据处理与合成方案  
+ 处理后的数据集需按照如下结构存放：  

```
📦 output/
├── 📂 processed_data/
│   ├── 📂 synthetic_images/ (if any)
│   └── 📄 processed_data.jsonl
│ ...
```

+`processed_data.jsonl`需为标准的JSONL格式，例如：  

```
{&quot;images&quot;: [&quot;images/00237/002375592.jpg&quot;], &quot;text&quot;: &quot;&lt;image&gt;\nadorable pink and gray elephant themed party favour boxes with tissue fillers &lt;|__dj__eoc|&gt;&quot;, &quot;id&quot;: &quot;002375592&quot;}
{&quot;images&quot;: [&quot;images/00199/001999195.jpg&quot;], &quot;text&quot;: &quot;&lt;image&gt;\nbreccinano adult dog food for all ages with turkey, lamb and venisi &lt;|__dj__eoc|&gt;&quot;, &quot;id&quot;: &quot;001999195&quot;}
...
```

在本次的baseline中，我们采用最简单的数据处理方法`recaptioning`来作为一个示例

由于可能无法直连HuggingFace的原因需要提前下载`blip2-opt-2.7b`的图片描述模型

```
# model_download.py
from modelscope import snapshot_download

model_dir = snapshot_download(&#x27;goldsj/blip2-opt-2.7b&#x27;, cache_dir=&#x27;/root/autodl-tmp/downloads&#x27;, revision=&#x27;master&#x27;)
```

终端执行

```
python model_download.py
```

![](img/ccd6ad529a23e2c2d531cfb7251eb17b.png)

注意这里的`cache_dir`需要改成想要保存的目录

```
# prepare the recaptioning recipe
recipe = &#x27;&#x27;&#x27;
dataset_path: input/pretrain_stage_1_10k/mgm_pretrain_stage_1_10k.jsonl
export_path: output/image_captioning_output/res_10k.jsonl

process:
  - image_captioning_mapper:
      hf_img2seq: &#x27;Salesforce/blip2-opt-2.7b&#x27;  # You can replace this path to a local downloaded HF model
      keep_original_sample: false  # we only need the recaptioned captions
&#x27;&#x27;&#x27;

with open(&#x27;solution/image_captioning.yaml&#x27;, &#x27;w&#x27;) as fout:
    fout.write(recipe)
```

注意其中的`hf_img2seq`需要换成之前下载好的本地模型所在的目录，比如`/root/autodl-tmp/downloads/goldsj/blip2-opt-2___7b`

得到`image_captioning.yaml`文件如下

```
dataset_path: input/pretrain_stage_1_10k/mgm_pretrain_stage_1_10k.jsonl
export_path: output/image_captioning_output/res_10k.jsonl

process:
  - image_captioning_mapper:
      hf_img2seq: &#x27;/root/autodl-tmp/downloads/goldsj/blip2-opt-2___7b&#x27;  # You can replace this path to a local downloaded HF model
      keep_original_sample: false  # we only need the recaptioned captions
```

然后使用`data-jucier`来进行数据处理

```
# recaption the 10k data
dj-process --config solution/image_captioning.yaml
```

![](img/445fe7027869454145790b1a9f1f9996.png)

## 参考资料

## 更多QA详见[QA & 其他彩蛋](https://datawhaler.feishu.cn/wiki/OyA2w7JwFiydNtkh3hccyRZunsb)！

>[QA & 其他彩蛋](https://datawhaler.feishu.cn/wiki/OyA2w7JwFiydNtkh3hccyRZunsb)任何想法都可以在学习群里交流或在进行填写哟~

>本教程主要由[姜舒凡](https://github.com/Tsumugii24)、、、写作贡献，

>融合了多位助教的内测体验和宝贵建议。

>如果你觉得对你有价值和启发，请在下面点赞和评论哟~

>

### 另外别忘了写笔记打卡呀！！~

### 及时思考、沉淀和产出能让你的学习事半功倍！

>

> **注：本文采用 署名—非商业性使用—相同方式共享 4.0 协议国际版 许可协议进行许可。**
