# 从零入门多模态大模型数据合成

> 来源：[https://datawhaler.feishu.cn/docx/GqGFd9gYto1NhZxBFD9c9XvJnhF](https://datawhaler.feishu.cn/docx/GqGFd9gYto1NhZxBFD9c9XvJnhF)

# 学习第一步：报名赛事！(点击即可跳转)

> 赛事链接：https://tianchi.aliyun.com/competition/entrance/532251

> 直接报名即可，然后可以尝试自己阅读赛题，

> **尝试理解和学习赛事要求**

![](img/6f3f7e335cef9ea98243e71e178a7e3d.png)

![](img/bd7c74b6a534b03bff86a28319bbf90e.png)

> 天池Better Synth多模态大模型数据合成挑战赛 是 Datawhale 2024 年 AI 夏令营第四期的学习活动——

+   适合想 入门并实践 多模态大模型、数据处理、模型训练 的学习者参与

>

> **温馨提示**：

> 本赛事需要耗费大量的算力，

> 跑通此baseline至少需要**120G内存**以上的**A10/4090**，

> 整个流程可能需要5小时，大家请自行寻找算力，预留好时间，

> 下面我们会以**autoDL平台**为例——

>

> [从零入门多模态大模型数据合成-大家评论版](https://datawhaler.feishu.cn/wiki/MRIhw663ViJMv3k81X5cMp0VnQd)各种排坑指南 & 全网编辑的版本可以查看：

>

> **学习内容提要**：跑通官方给定 数据处理 + 模型训练&微调 + 模型评测 baseline(需充值或自备算力)，学习Data-Juicer数据处理系统，探索多模态大模型的训练数据合成方案。

内容速览

# **300 分钟** baseline指南！

> **教程贡献者说：**

> 我们整个学习活动，将带你 ——

+   **跑通最简单的Baseline**(相信我，当前baseline已经写到最简单了，但需要的算力和学习实在不是常人&免费方案能hold住的)

+   **了解竞赛核心考察点和难点**

+   精读Baseline与了解进阶思路

>

> 当前环节目标： **300分钟 **跑通 **“天池Be**tter Synth多模态大模型数据合成挑战赛” ！！！！

>

> 千里之行，始于足下，从这里，开启你的 AI 学习之旅吧！

> 记得在每次完成学习后，写笔记打卡哟~ 及时总结沉淀能让学习事半功倍~

## Step1：在autoDL创建实例！（点击即可跳转）

> 链接：https://www.autodl.com/console/instance/list

> 选择合适的配置租赁，搜索对应镜像，然后启动！

> autodl镜像链接(社区镜像)：datawhalechina/self-llm/Better-Synth

>

> 注意：镜像中已经安装好了相关运行环境，如果不使用该镜像的话，需要自行通过install.sh安装环境

>

> **温馨提示**：由于baseline运行需求，我们需要

+   额外扩充磁盘空间，至少需要增加100G

+   在后面的模型训练和评测阶段，需要跑大概3小时，至少需要100G+以上的内存

+   方案一：使用单卡120G内存的实例规格（下面以这个规格为例）

+   方案二：使用双卡，每张卡80G内存的实例规格(可以自行尝试)

+   还可自行寻找其他同等算力环境，需要自行通过install.sh安装环境

>

> 需提前充值大概10~20元，否则无法创建实例，以及避免由于欠费被停止实例。

>

> **提前给你打个预防针**：这个baseline可能让你十分痛苦地需要反复配置环境，而且可能在你没注意的地方跑失败导致后续一系列的问题

![](img/1db54cc2322288b0ea677b998640a8bf.png)

![](img/e1ae5f064477da237b5ce4a2e3331959.png)

![](img/f216e640e5eb4a6bfd95dd24b0835812.png)

## Step2：300分钟 跑通baseline！

### 改为“无卡模式开机”

![](img/e540c05bca1cb5b05d3eee0e13ce8ac6.png)

![](img/1092a8c1c2defda045a3dd8c4e2680c5.png)

![](img/04e007422057589db33092809a99236a.png)

![](img/f11793c85171e59558ed38774af664e9.png)

+   如果使用的不是上面指定的镜像，还需要运行 如下命令安装相关环境

```
conda create -n dj python=3.10

conda init bash  
source ~/.bashrc

bash install.sh #大概需要1小时
```

> 如果遇到【  colect  Building wheel for flash-attn(setup.p)】的问题

1.  下载下面的 flash_attn-xxx 文件，

1.  上传到 /root/miniconda3/envs/dj 的目录里,

1.  然后终端里，cd到对应路径里，运行命令：

pip install flash_attn-2.6.3+cu118torch2.0cxx11abiFALSE-cp310-cp310-linux_x86_64.whl

>

> PS：听说 pip install flash-attn --no-build-isolation 命令可能也可以

<https://datawhaler.feishu.cn/sync/HwcSdGAius0EDCb50WrczM2Gngi>

### 下载赛事所需文件包

> 包括：baseline、必要的开发者套件等，等他下

> 复制如下命令，在终端中执行：

```
conda activate dj
cd autodl-tmp/
git clone https://www.modelscope.cn/datasets/Datawhale/better_synth_baseline_autoDL.git
```

![](img/66c06f37ffce18ae0d9503243dc282b6.png)

### **下载多模态base模**型及数据集 & 相关依赖软件（大概需要60分钟）

> 工具、基础模型 和相关数据集等

1.  安装必要工具

> 复制如下命令，在终端中执行，期间需要 输入一个 Y，再按一个回车

```
apt update
apt install axel zip file
pip install modelscope
```

![](img/8f55aecd7e5872120d0f42a4fc2a58f7.png)

1.  下载模型和相关数据集（约50分钟左右，网速好也会很快～）

> 执行完成以上步骤后 ，复制如下命令，在终端中执行

```
cd better_synth_baseline_autoDL
bash download.sh  ###大概需要50分钟
```

![](img/1cb465bca980c5ae929a30c52e075a41.png)

![](img/09a22ad5c9993e4b69b7d7a8e12c7571.png)

1.  下载BLIP图片描述模型**（约20分钟左右）**

> 执行完成以上步骤后 ，复制如下命令，在终端中执行

```
python download_blip.py
```

![](img/22948f4242d7933bc1b698e9c1df77f1.png)

![](img/dd60a1955f28a3995e9e4be30781563b.png)

# 接下来正式进入数据合成和模型训练环节！！！

### 数据处理与合成**（大概需要1小时）**

> 记住！！！从现在开始，每分每秒都算钱！！！！

> 千万不要让机器闲着！！！！跑完了就下一步！！！

> 跑通了就赶紧关机！！！！

1.  关机，重新带卡开机！

![](img/2b4dca4cf62885365fcd73ae84c4d917.png)

![](img/6ad5b985b79f60fdd5ef266b819103cd.png)

1.  **开始进行basel**ine的核心——数据处理与合成（大概需要1小时）

> 复制如下命令，在终端中执行

```
conda activate dj
cd autodl-tmp/better_synth_baseline_autoDL
dj-process --config ./image_split_10.yaml
dj-process --config ./image_captioning_10.yaml
```

![](img/7cbfbb5b7077a6c3811d0cf6c0a05e78.png)

### 执行模型训练 & 推理评测**（大概需要3～4小时）**

1.  安装 flash-attn

> (如果使用的不是指定镜像，运行成功过 bash install.sh，不需要进行此步骤)

> 复制如下命令，在终端中执行

```
cd toolkit/training
pip install -e .
pip install flash-attn --no-build-isolation
cd ..
```

1.  开始训练 & 推理评测！（**大概需要3~4小时**）

> 复制如下命令，在终端中执行

```
bash train_mgm_2b_stag**e_one_c**ard.sh   ### 大概需要3小时
```

![](img/91164b2547863f0428a1e5845fc0eff2.png)

![](img/9aaf5d234b5fa342c4338d69708bd973.png)

![](img/40506a8b99ccde39a70aebfa7adead45.png)

![](img/adbcb79a73c7e2e782d38aca552385b6.png)

![](img/0ffa3338aee8138062b8dc97c42272ff.png)

### 打包构建符合提交要求的zip文件

> 复制如下命令，在终端执行，然后下载submit.zip文件！

```
cd ../submit

cp -r /root/autodl-tmp/better_synth_baseline_autoDL/solution .

cp -r /root/autodl-tmp/better_synth_baseline_autoDL/output/eval_results output/

cp -r /root/autodl-tmp/better_synth_baseline_autoDL/output/train.sh output/

cp /root/autodl-tmp/better_synth_baseline_autoDL/output/training_dirs/MGM-2B-Pretrain-*/pretrain.log output/training_dirs/MGM-2B-Pretrain-image_recaption/

cp /root/autodl-tmp/better_synth_baseline_autoDL/output/training_dirs/MGM-2B-Finetune-*/finetuning.log output/training_dirs/MGM-2B-Finetune-image_recaption/


zip -r submit.zip solution output
```

![](img/f81de4606bef8c91c299e349ccdda19c.png)

![](img/da15b6a45e1d79cf7f1e1ec1579409a2.png)

## Step3：在官网提交结果(submit.zip文件)

> 链接：https://tianchi.aliyun.com/competition/entrance/532251/submission/1335

> **评分大概需要 10 分钟～ ****上传结果后可以在 **[**ranklist**](https://tianchi.aliyun.com/competition/entrance/532251/rankingList)** 处看到评分结果哟！**（预期分数在0.5分左右😉~）

![](img/f8d186e83928ed78d10fa590311d7d29.png)

![](img/372fdd4d9fd7697448db6726e28e82dc.png)

![](img/b2d177f3cb76ac53adfa12aaaf833307.png)

## 别忘了关机！！！！！

![](img/5a32d9eebaf4832ebbe6319277c4da33.png)

# 🎉 恭喜你拿下了属于自己的第一个分数！

## 👍🏻 快来给自己点赞！

## 🛎️ 别忘记打卡&在群里接龙！

![](img/ba6ce89443ff254c06683a812efa8988.png)

## 📅 然后迎接后续的挑战吧！

> **教程贡献者说：**

> 这个赛题对想要入门多模态语言模型的小伙伴们是一个不错的尝试。对于现阶段的发展来说，可能数据的质量才是模型性能表现的关键，而数据处理又是一个很复杂又很贵的事情，因此是否能够在本次比赛中掌握一些技巧和灵感来运用到之后的工作和模型训练中，是一个不错的点。

> 接下来我们会基于这个赛题进行更多的知识点学习，会涉及到：

1.  简单的Linux操作系统下的环境配置和安装

1.  Data Jucier 与 Mini-Gemini 的学习

1.  数据处理的进阶技巧和模型训练阶段的技巧等

>

> **更多详情见如下学习规划表：**

| ## 从零入门**多模态大模型数据合成** |  |  |  |
| --- | --- | --- | --- |
| **Task** | **Task 描述** | **相关知识点** | **学习安排** |
| **Task 1：5天** |  | 1.  赛事环境配置 &amp; 跑通baseline | 开营直播 |
| **Task 2：****5****天** |  | 1.  Data Jucier Playground | 赛事官方直播分享 |
| **Task 3：****5****天** |  | 1.  官方赛事解读 |  |
|  |  |  |  |

> 打怪难度(1~3⭐️)：⭐️⭐️⭐️

1.  熟悉赛题背景，跑通**baseline1**

1.  熟悉开发者套件结构和最终的提交形式

1.  写笔记，完成**Task1 **打卡

> 打怪难度(1~3⭐️)：⭐️⭐️

1.  熟悉Data-Juicer官方文档和示例

1.  精读并尝试在baseline1的基础上进行改进和提升、在群里分享思考

1.  写笔记，完成 **Task2 **打卡

> **打怪难度：⭐️⭐️**

1.  学习**官方直播**、在群里分享思考

1.  思考更多上分技巧，参与**上分思路交流会**

1.  写笔记，完成 **Task3 **打卡

### 祝大家上分顺利，学习愉快~ 多在群里提问和交流哟！

+   赛事链接：[**天池Better Synth多模态**](https://tianchi.aliyun.com/competition/entrance/532251)大模型数据合成挑战赛（点击跳转赛事官网）

+   提问前建议先查看** **[ QA & 其他彩蛋](https://datawhaler.feishu.cn/wiki/OyA2w7JwFiydNtkh3hccyRZunsb)**是否早已收录你的**问题~ （QA文档可任意编辑，助教和教程贡献者会不定时查看）

+   更多学习活动问题可见：** **[从零入门多模态大模型数据合成-学习者手册](https://datawhaler.feishu.cn/wiki/YdjtwmO8jiPckDkbERPcYgsDnAe)

# 🍰 下面正式来了解一下赛题！

### 你知道刚刚发生了什么吗？

> 刚刚我们跑通了Baseline，想必你一定很好奇这背后发生了什么吧？

> 在学习的过程中，不单单要知道如何做，更要知道为什么要这样做。这样我们在遇到同类型的问题时，才能举一反三。

> 一般情况下，拿到一个赛题之后，我们需要明确：

> 1）这是一个什么场景下的赛题；

> 2）这个赛题要解决什么问题。

> 深度思考这些问题，既能帮助我们更好地解决相关问题，更能让我们在后续遇到相关问题时，更好地完成迁移。

## 赛题解读

| ## 天池Better Synth多模态大模型数据合成挑战赛 |  |
| --- | --- |
| **赛事链接** | https://tianchi.aliyun.com/s/46544cccd6a0744b9c43d58cb6d97dee |
| **赛事任务** | 基于赛方提供的 **候选种子数据集 **，使用** ****Data-Juicer**等工具实现**数据合成与清洗**，构建 **预训练（模态**间对齐）阶段的数据集，赛事最终目标是——提升多模态大模型的 图像理解能力 |
| **数据集等** | **本次赛事**要求在**统一的框架和参数设置**下进行模型训练和任务评测，公平对比数据导致的性能差异，因此—— |
| **评分规则介绍** | **分数越高越好** |
| **任务提交格式说明** |  |
| **赛程安排** |  |
| 赛事奖励 | +   **冠军（第1名）**：30000元人民币奖金，颁发获奖证书 |
| **赛事其他要点** | #### 赛事目标 |

+   **用于数据处理的 ****solution/****方案 **及 **评测结果**，

+   辅以训练过程脚本用于确认训练的真实性

> **数据合成**：指使用已有数据或模型生成新的数据样本的过程。

> **数据清洗**：指在数据处理过程中检测和修复数据中错误、不完整性或不一致性的过程。数据清洗的目的是确保数据集的质量和一致性。

> **Data-Juicer**：一个集成了数据分析和可视化工具的一站式大模型数据处理系统，赛事中可用于辅助数据分析、筛选和生成

> **图像理解任务**：指通过计算机视觉技术和机器学习算法对图像进行深入分析和理解的过程。这些任务旨在从图像中提取和推断有意义的信息，例如识别对象、场景、行为，以及进行更高级别的推理和理解。

![](img/aafa6970caf189a5b75d2af1585bf535.png)

1.  需要使用 [data-juicer](https://github.com/modelscope/data-juicer) 基于**种子数据集**进行数据处理与合成

线上赛与线下赛将提供**不同的种子数据集**，并且线下赛将**基于线上赛参与排名的模型**继续训练。

线上赛与线下赛过程中种子数据集与模型训练的信息差异如下表所示：

![](img/5c91b85c0ac2d82ee87b76400a378eb4.png)

1.  **数**据集产出流程中必须包含“合成”的过程，未包含的方案会被认为是无效方案。

![](img/9bac6817593d27ecc7c00cfca32c5fb3.png)

![](img/abbd93c8a06aa4b38d08e7568a3872d2.png)

![](img/5b99738b224860fc2b0325d664a0b347.png)

+   processed_data.jsonl需为标准的JSONL格式，例如：

```
{&quot;images&quot;: [&quot;images/00237/002375592.jpg&quot;], &quot;text&quot;: &quot;&lt;image&gt;\nadorable pink and gray elephant themed party favour boxes with tissue fillers &lt;|__dj__eoc|&gt;&quot;, &quot;id&quot;: &quot;002375592&quot;}
{&quot;images&quot;: [&quot;images/00199/001999195.jpg&quot;], &quot;text&quot;: &quot;&lt;image&gt;\nbreccinano adult dog food for all ages with turkey, lamb and venisi &lt;|__dj__eoc|&gt;&quot;, &quot;id&quot;: &quot;001999195&quot;}
...
```

**线上赛：**

+   榜单成绩为模型**性能相对提升**的平均得分：

+   使用**TextVQA****和 ****MMBench**** **评测，取平均分

+   需要自行评测后提交评测结果

+   基准为官方baseline（使用随机采样数据训练）

+   更多评估方法细节详见 [本次赛事具体是如何评分的？](https://datawhaler.feishu.cn/wiki/PVmkwDClQiKbmOk1e7scYo2Pndd#C2U9dDZyhodcv3x0kFvcn8CBnbc)

**线下赛：**

线下赛会同时考虑合成数据方法的速度 和 评测提分

+   会扩展 **测评集 & 数据集**

+   同样以 **官方提供的合成算子速度**为 baseline

+   计算**相对提升**，并与评测任务的**平均提升得分**

> 假设评测任务的平均提升得分为M倍，

> 合成数据方法的平均速度提升得分为 N 倍，

> 则按照 M 按 0.02 区间进行分桶后，

> 桶内按照 M*N 的方式进行得分计算排名。

**线上赛：提交****submit.zip****文件**

+   需包含：数据处理方案、训练及推理脚本、训练日志、推理结果

+   文件打包结构如下图(不可更改)：

![](img/7c097ef835bdbb83619a7fe4f7cce621.png)

**线下赛：提交****submit.zip****文件，直接****提交模型自动评测**

+   需包含：数据处理与合成方案、处理与合成后的新数据集、训练及推理脚本、模型、推理结果

+   文件打包结构如下图(不可更改)：

![](img/8c5d7b7c82fd4820c679497e48173c9c.png)

> solution/目录要求：

+   在readme.txt中详细说明算法设计和执行流程，用于代码审核

+   任何第三方 pip 依赖需要在 requirements.txt文件中完整列出，复现前会自动安装

+   根据需要自由添加更多代码文件和 **Data-Juicer** 执行文件

> 必要文件缺失或**标准工具** (train.sh非可编辑部分）被篡改，评测将不会进行

> 如果评测失败，大概率是提交格式不符合要求，请按照上图规定的格式提交

**线上赛：TOP10晋级线下赛**

+   即日起—**2024年8月23日18:00**，UTC+8

+   在天池平台提交推理结果进行测评

+   每天提供 **6** 次实时评测

+   需进行代码审核，**截止8月26日18:00**

+   提交成绩对应的完整提交文件：代码、日志、训练模型、合成数据和readme等

+   晋级名单将于**8月28日**前公布

**线下赛：**“天池AI奇点技术闭门会”

+   2024年9月2日9:00—**2024年9月6日**

+   在北京清华大学（暂定）以训练营和技术沙龙方式举行

+   获奖名单于**2024年9月6日**予以公示

**Data-Juicer:**

一个一站式**多模态**数据处理的python库，旨在为大语言模型 (LLM) 提供更高质量、更丰富、更易“消化”的数据。

[Data-Juice](http://8.138.149.181/)r在线试用： Playground。

**TensorRT-LLM ：**

一个大模型推理加速库

https://github.com/NVIDIA/TensorRT-LLM?spm=a2c22.12281976.0.0.c1202c2bxZVSWx

## 本次赛事具体是如何评分的？

> 赛事是通过两个多模态模型常用的benchmark进行综合打分

1.  **TextVQA**：该benchmark中，图像对应文本的问题回答数据集

1.  **MMBench**：该benchmark是一个针对大规模多模态模型的新型评估基准。随着视觉语言模型在感知和推理能力方面的显著进步，如何有效地评估这些模型成为了一个主要难题。传统基准如VQAv2和COCO Caption提供了定量性能测量，但在细粒度能力和鲁棒性评估指标方面存在不足。而像OwlEval这样的主观性基准虽然能够全面评价模型能力，但其可扩展性差且易受偏见影响。MMBench设计了一套综合的评估流水线，包含两大核心元素

+   精心构建的超越现有同类基准的数据集，该数据集包括2,974个经过细致挑选的问题，覆盖了20种不同类型的细粒度技能

+   引入了创新的CircularEval策略，并结合使用ChatGPT技术来将模型生成的自由格式预测转化为预定义选项，以实现对模型预测的更可靠评估。

![](img/bd49bcb643623725b6ec7e2acd71bf86.png)

### 赛事规定提交的材料是这些——

> 线上赛只需要提交solution/，训练脚本，训练日志（pretrain.log和finetune.log），以及评测推理结果

+   请将数据处理方案、训练及推理脚本、训练日志、推理结果打包成一个 zip 文件，上传至天池平台进行评测。

+   为保证提交的规范性，务必遵循以下文件打包结构并提交以下所需的文件，请勿添加额外的顶级目录。

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

+   下载基础模型，种子/微调/评测数据集

+   基础模型与微调数据集均存放于训练目录中指定位置

+   种子数据集存放于input目录

+   评测数据集存放于toolkit/eval目录

```
bash download.sh # 数据集 + 多模态模型 Gemma-2B，这一步可能时间较长，大约1个小时
```

下载完成后依然会在终端中显示如下信息 Done

![](img/557f03b1b73fec9f527a7f17329611ab.png)

+   比赛要求使用** **[**data-juicer**](https://github.com/modelscope/data-juicer)** **基于上一步中下载的**种子数据集**进行数据处理与合成

+   数据处理与合成相关代码请存放于solution目录中，并在solution/requirements.txt中添加所需的依赖

+   最后也请在solution/readme中详细介绍所使用的数据处理与合成方案

+   处理后的数据集需按照如下结构存放：

```
📦 output/
├── 📂 processed_data/
│       ├── 📂 synthetic_images/ (if any)
│       └── 📄 processed_data.jsonl
│ ...
```

+   processed_data.jsonl需为标准的JSONL格式，例如：

```
{&quot;images&quot;: [&quot;images/00237/002375592.jpg&quot;], &quot;text&quot;: &quot;&lt;image&gt;\nadorable pink and gray elephant themed party favour boxes with tissue fillers &lt;|__dj__eoc|&gt;&quot;, &quot;id&quot;: &quot;002375592&quot;}
{&quot;images&quot;: [&quot;images/00199/001999195.jpg&quot;], &quot;text&quot;: &quot;&lt;image&gt;\nbreccinano adult dog food for all ages with turkey, lamb and venisi &lt;|__dj__eoc|&gt;&quot;, &quot;id&quot;: &quot;001999195&quot;}
...
```

在本次的 baseline 中，我们采用最简单的数据处理方法 recaptioning 来作为一个示例

由于可能无法直连 HuggingFace 的原因需要提前下载 blip2-opt-2.7b 的图片描述模型

```
# model_download.py
from modelscope import snapshot_download

model_dir = snapshot_download(&#x27;goldsj/blip2-opt-2.7b&#x27;, cache_dir=&#x27;/root/autodl-tmp/downloads&#x27;, revision=&#x27;master&#x27;)
```

终端执行

```
python model_download.py
```

![](img/5ab4a02de5dadf4bdfff9c0f883dda83.png)

注意这里的 cache_dir 需要改成想要保存的目录

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

注意其中的 hf_img2seq 需要换成之前下载好的本地模型所在的目录，比如 /root/autodl-tmp/downloads/goldsj/blip2-opt-2___7b

得到 image_captioning.yaml 文件如下

```

dataset_path: input/pretrain_stage_1_10k/mgm_pretrain_stage_1_10k.jsonl
export_path: output/image_captioning_output/res_10k.jsonl

process:
  - image_captioning_mapper:
      hf_img2seq: &#x27;/root/autodl-tmp/downloads/goldsj/blip2-opt-2___7b&#x27;  # You can replace this path to a local downloaded HF model
      keep_original_sample: false  # we only need the recaptioned captions

```

然后使用 data-jucier 来进行数据处理

```
# recaption the 10k data
dj-process --config solution/image_captioning.yaml
```

![](img/5c7b1ec77a4b99ac0aa3b4ff887c0c72.png)

## 参考资料

## 更多QA详见[ QA & 其他彩蛋](https://datawhaler.feishu.cn/wiki/OyA2w7JwFiydNtkh3hccyRZunsb)！

> [ QA & 其他彩蛋](https://datawhaler.feishu.cn/wiki/OyA2w7JwFiydNtkh3hccyRZunsb)任何想法都可以在学习群里交流或在  进行填写哟~

> 本教程主要由 [姜舒凡](https://github.com/Tsumugii24)、 、 、  写作贡献，

> 融合了多位助教的内测体验和宝贵建议。

> 如果你觉得对你有价值和启发，请在下面点赞和评论哟~

>

### 另外别忘了写笔记打卡呀！！~

### 及时思考、沉淀和产出能让你的学习事半功倍！

>

> **注：本文采用 ****署名—非商业性使用—**相同方式共享 4.0 协议国际版 许可协议进行许可。