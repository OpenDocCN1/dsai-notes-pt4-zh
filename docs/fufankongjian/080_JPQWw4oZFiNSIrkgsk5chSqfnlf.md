
# DeepSeek知识库问答实战

> 来源：`https://kq4b3vgg5b.feishu.cn/docx/ANlgdQDGVoGsxvxYbQIcGwmHnUf`

## 课程说明

体验课内容节选自《2025大模型Agent智能体开发实战》完整版付费课程

体验课时间有限，若想深度学习大模型技术，欢迎大家报名由我主讲的《2025大模型Agent智能体开发实战》：

![](img/76112a1055b3597c4416cc4ebdc5a037.png)

![](img/018dea300d5e715f5e4de1f49b9c8bb8.png)

此外，公开课全套学习资料，已上传至网盘：

`https://pan.baidu.com/s/1Jt1uWNntM6GgwZPwAErYVg?pwd=izt9`

`https://pan.baidu.com/s/1ZmPsNEd32mVE4weURqt5Ew?pwd=7xjc`

需要更系统深入学习大模型可扫码⬆️添加助教咨询喔～

---

# 《2025大模型智能体Agent开发实战》体验课

## DeepSeek-v3企业知识库问答入门实战

## 借助GraphRAG搭建知识库问答机器人

### 一、DeepSeek-v3大模型介绍与微软GraphRAG项目介绍

#### 1. 国产开源大模型之光：DeepSeek v3

![](img/e342aaf8575b329074eee2178b03e14e.png)

DeepSeek-V3是由中国人工智能公司深度求索（DeepSeek）于2024年12月26日发布的开源大型语言模型。该模型采用混合专家（MoE）架构，拥有6710亿个总参数，每个token激活370亿参数，并在14.8万亿token上进行了预训练。在多项基准测试中，DeepSeek-V3表现出色，尤其在代码和数学任务上，超越了其他开源模型，其性能与领先的闭源模型相当。此外，模型的生成速度提升至每秒60个token，相比前一版本提高了3倍。值得注意的是，DeepSeek-V3的训练成本约为557.6万美元，显著低于同类模型，体现了高效的训练策略和资源利用。用户可以通过DeepSeek的官方网站与该模型进行交互，体验其强大的语言理解和生成能力。

![](img/2d39d1f2c280abb337f76e1530c4e1c2.png)

DeepSeek v3采用混合专家（MoE）架构，拥有6710亿个总参数，每个token激活370亿参数。

**模型架构与创新：**

+ **多头潜在注意力（MLA）：** 通过低秩压缩技术减少推理时的Key-Value缓存，提升推理效率。

+ **DeepSeekMoE架构：** 采用细粒度的专家分配和共享专家机制，实现高效训练。

+ **无辅助损失的负载均衡策略：** 通过动态调整专家偏置，确保专家负载均衡，提升模型性能。

+ **多令牌预测（MTP）：** 支持多令牌预测，提高训练信号密度，并通过推测解码加速推理。

+ **FP8混合精度训练：** 在大规模模型中验证了FP8训练的可行性，降低内存消耗和训练成本。

**训练与性能：**

+ **训练数据：** 在14.8万亿高质量token上进行预训练，模型训练过程稳定。

+ **训练成本：** 总训练成本约为557.6万美元，显著低于同类模型。

+ **性能表现：** 在多种基准测试中表现优异，尤其在代码和数学任务上，超越其他开源模型，与领先的闭源模型相当。

DeepSeek-V3的发布标志着开源模型在性能和效率上的新高度，为自然语言处理技术的进步提供了新的示例。

#### 2. 知识库问答最强检索增强技术：GraphRAG

![](img/8cb315a3d016e173f6b6cb8bdeecf485.png)

**检索增强生成（RAG）** 是一种通过结合真实世界的信息来提升大型语言模型（LLM）输出质量的技术。RAG 技术是大多数基于 LLM 的工具中的一个重要组成部分。大多数 RAG 方法使用 向量相似性 作为检索技术，我们将其称为 基线 RAG（Baseline RAG）。

**GraphRAG** 则使用 知识图谱 来在推理复杂信息时显著提升问答性能。当需要对复杂数据进行推理时，GraphRAG 展示了优于基线 RAG 的性能，特别是在 知识图谱 的帮助下。

RAG 技术在帮助 LLM 推理私有数据集方面显示了很大的潜力——例如，LLM 没有在训练时接触过的、企业的专有研究、业务文档或通信数据。基线 RAG 技术最初是为了解决这个问题而提出的，但我们观察到，在某些情况下，基线 RAG 的表现并不理想。以下是几个典型的场景：

1. 基线 RAG 很难将信息串联起来：当一个问题的答案需要通过多个不同的信息片段，并通过它们共享的属性来连接，进而提供新的综合见解时，基线 RAG 表现得很差。

2. 例如，在回答类似“如何通过现有的数据推断出新结论”这种问题时，基线 RAG 无法很好地处理这些散布在不同文档中的相关信息，它可能会遗漏一些关键联系点。

3. 基线 RAG 无法有效理解大型数据集或单一大文档的整体语义概念：当被要求在大量数据或复杂文档中进行总结、提炼和理解时，基线 RAG 往往表现不佳。

4. 例如，如果问题要求对整个文档或多篇文档的主题进行总结和理解，基线 RAG 的简单向量检索方法可能无法处理文档间的复杂关系，导致对全局语义的理解不完整。

为了应对这些挑战，技术社区正在努力开发扩展和增强 RAG 的方法。微软研究院（Microsoft Research）提出的 GraphRAG 方法，使用 LLM 基于输入语料库构建 知识图谱。这个图谱与社区总结和图谱机器学习输出结合，能够在查询时增强提示（prompt）。GraphRAG 在回答以上两类问题时，展示了 显著的改进，尤其是在 复杂信息的推理能力 和 智能性 上，超越了基线 RAG 之前应用于私有数据集的其他方法。

#### 3. 微软GraphRAG项目简介

![](img/8b7e18747b58f082b0eefaea210c8343.png)

> 项目地址：`https://github.com/microsoft/graphrag/`

**GraphRAG** 是微软研究院开发的一种先进的增强检索生成（RAG）框架，旨在提升语言模型（LLM）在处理复杂数据时的性能。与传统的 RAG 方法依赖向量相似性检索不同，GraphRAG 利用 知识图谱 来显著增强语言模型的问答能力，特别是在处理私有数据集或大型、复杂数据集时表现尤为出色。

#### 4. GraphRAG核心特点

传统的 **Baseline RAG** 方法在某些情况下表现不佳，尤其是当查询需要在不同信息片段之间建立联系时，或是当需要对大规模数据集进行整体理解时。GraphRAG 通过以下方式克服了这些问题：

+ **更好的连接信息点**：GraphRAG 能够处理那些需要从多个数据点合成新见解的任务。

+ **更全面的理解能力**：GraphRAG 更擅长对大型数据集进行全面理解，能够更好地处理复杂的抽象问题。

而借助微软开源的GraphRAG项目，我们可以快速做到以下事项：

+ **基于图的检索**：传统的 RAG 方法使用向量相似性进行检索，而 GraphRAG 引入了知识图谱来捕捉实体、关系及其他重要元数据，从而更有效地进行推理。

+ **层次聚类**：GraphRAG 使用 `Leiden` 技术进行层次聚类，将实体及其关系进行组织，提供更丰富的上下文信息来处理复杂的查询。

+ **多模式查询**：支持多种查询模式：

  + **全局搜索**：通过利用社区总结来进行全局性推理。

  + **局部搜索**：通过扩展相关实体的邻居和关联概念来进行具体实体的推理。

  + **DRIFT 搜索**：结合局部搜索和社区信息，提供更准确和相关的答案。

+ **图机器学习**：集成了图机器学习技术，提升查询响应质量，并提供来自结构化和非结构化数据的深度洞察。

+ **Prompt 调优**：提供调优工具，帮助根据特定数据和需求调整查询提示，从而提高结果质量。

#### 5. GraphRAG运行流程

**索引（Indexing）过程**

1. **文本单元切分**：将输入文本分割成 `TextUnits`，每个 `TextUnits` 是一个可分析的单元，用于提取关键信息。

2. **实体和关系提取**：使用 LLM 从 `TextUnits` 中提取实体、关系和关键声明。

3. **图构建**：构建知识图谱，使用 `Leiden` 算法进行实体的层次聚类。每个实体用节点表示，节点的大小和颜色分别代表实体的度数和所属社区。

4. **社区总结**：从下到上生成每个社区及其成员的总结，帮助全局理解数据集。

**查询（Query）过程**

索引完成后，用户可以通过不同的搜索模式进行查询：

+ **全局搜索**：当我们想了解整个语料库或数据集的整体概况时，GraphRAG 可以利用 社区总结 来快速推理和获取信息。这种方式适用于大范围问题，如某个主题的总体理解。

+ **局部搜索**：如果问题关注于某个特定的实体，GraphRAG 会向该实体的 邻居（即相关实体）扩展搜索，以获得更详细和精准的答案。

+ **DRIFT 搜索**：这是对局部搜索的增强，除了获取邻居和相关概念，还引入了 社区信息 的上下文，从而提供更深入的推理和连接。

#### Prompt 调优

为了获得最佳性能，GraphRAG 强烈建议进行 Prompt 调优，确保模型可以根据你的特定数据和查询需求进行优化，从而提供更准确和相关的答案。

#### 6. GraphRAG核心原理回顾

![](img/e39daced2c1dc581cc55118b8c0c3be6.png)

🤗miniDeepSeek-v3全部训练项目代码、数据、以及训练完的模型，都已上传至课件网盘，联系助教即可领取。

![](img/e3a82ae695b1f034909bd99814793835.png)

### 二、GraphRAG安装与Indexing检索流程实现

---

> 注，以下实验环境均为Ubuntu系统，以更好的模拟真实企业应用场景，其中大多数方法也可以直接迁移至Windows操作系统中。

### 【补充阅读】在线算力租赁平台AutoDL使用指南与虚拟环境创建方法

```python
from IPython.display import Markdown, display

with open('【补充材料】在线算力租赁平台AutoDL使用指南与虚拟环境创建方法.md', 'r') as file:
    md_content = file.read()

display(Markdown(md_content))
```

### 【补充材料】在线算力租赁平台AutoDL使用指南与虚拟环境创建方法

#### Step 1. AutoDL算力租赁流程

+ **AutoDL简介**

AutoDL是一个稳定高效的算力租赁平台，AutoDL支持一键部署基础Linux基础环境、镜像环境，同时支持镜像文件保存与迁移、跨实力读取文件等，且平台显卡较为充裕，并支持多种不同计费方式，对于大模型初学者来说是非常不错的算力租赁渠道。

![](img/b4d96990034e4d0811ce678728554d70.png)

但与此同时，AutoDL也有一定的局限，如没有公网IP、不支持Docker镜像等。不过对于本次公开课，对于不具备本地硬件环境的同学来说，AutoDL肯定是最佳算力租赁渠道，公开课所有的演示流程也将是基于AutoDL环境的操作，便于各位同学跟着公开课内容逐步手动实现。

+ **AutoDL注册与充值**

点击AutoDL官网即可注册与登录👉 `https://www.autodl.com/login`

![](img/4757f9322686005e512b96d5bf9362ba.png)

除了常规登录流程外，AutoDL还要求绑定🛰️和进行实名认证。实名认证其实是目前所有算力租赁平台的基本要求，大家按照AutoDL官网指引👉 `https://www.autodl.com/console/center/account/auth`，一步步操作即可：

+ **账户充值**

AutoDL采用的是预付费模式，即需要先在AutoDL平台上储值，然后再租赁相关算力。AutoDL充值地址👉 `https://www.autodl.com/recharge`。需要注意的是，若需要全程完成公开课的模型训练流程，则需要约50元左右算力成本，而若只想简单测试最终模型效果，则算力成本在10元以内，大家可以根据自己实际需求进行充值。

![](img/7c01a070a00c8c66ac1ccf291cfca344.png)

+ **算力租赁**

接下来就需要进行GPU租赁了，本次公开课实验需要28G-30G左右显存，同时为了给大家介绍单机多卡的模型训练方法，推荐租赁双卡3090服务器，相对来说会较为划算。此外，也可以考虑租赁更高配置的服务器，如双卡4090、双卡L20、双卡A100等，可根据实际需求进行租赁。AutoDL算力市场👉：`https://www.autodl.com/market/list`

![](img/5392817448d448a539399befc494d129.png)

这里以租赁双卡3090为例进行介绍，计费方式可选根据实际需求进行选择，然后点击3090专区，并GPU数量选择2，然后任意选择一个有闲置GPU的服务器，点击2卡可租即可：

![](img/e0ff6be362de456b3ba11b4db5e1d988.png)

然后再次确认计费方式，以及账户剩余费用是否足够支付，在基础镜像中，选择一个镜像：

> 注，这里的镜像配置和计费方式，都可以随时修改。

这里我们选择 `Pytorch 2.3.0`（即最高版本）+`Python 3.12`+`CUDA 12.1`

选择完毕后，点击立即创建即可：

+ **查看当前服务器**

创建完成后，页面会自动跳转到当前账号的在线服务器管理页面，这里可以看到刚刚租赁的服务器正在开机中：

![](img/b6e6485d68fa766e1af1466e04ea1633.png)

稍作等待，待显示运行中时，代表远程服务器已经顺利运行，接下来则需要考虑如何连接远程服务器。

#### Step 2. 使用FinalShell连接远程服务器

连接远程服务器的方式有很多种，首先服务器已预安装了JupyterLab，我们可以直接通过JupyterLab中的Terminal来连接服务器，并通过命令行的方式操作远程服务器。但由于JupyterLab中Terminal界面较为原始，对新人用户操作并不友好。因此推荐使用专门的远程终端连接软件连接远程服务器，这里推荐使用FinalShell，下图扫码添加英英助教，回复LLM，即可领取本次公开课课件：

![](img/ce2fa95d6da0b677b44c8d7d149f3b6d.png)

其中finalshell软件就在本次公开课的课件中：

![](img/878bb3e49eb281e625fdf181b73d6407.png)

下载后按照普通软件安装流程进行安装即可：

![](img/92f9bfeeb0500b237e7be012d6de0e35.png)

安装完成后，即可使用FinalShell连接远程服务器。由于接下来FinalShell连接远程服务器需要使用远程服务器的地址和端口，因此我们需要先查阅此时远程服务器的基本信息。回到AutoDL控制台页面，分别复制登陆指令和密码到任意文本编辑器中：

![](img/1a467ceb40c4a87056620c884fc41657.png)

例如此时改服务器的登录指令为（需要找到你的主机的登录指令和密码）：

```
ssh -p 24503 root@conxxx
```

密码为：

```
wxxxx
```

记录好了之后即可打开FinalShell，使用该信息连接远程服务器。接下来打开FinalShell：

![](img/8860d70509443cc0adc9b3fa5f0bc5f7.png)

首次使用时点击左上方文件夹，进入连接管理器：

![](img/7e63cf8817a2794ea456b5a286f268fb.png)

再点击创建新的连接，并在弹出的选项中选择SSH连接（Linux）：

![](img/128cb10e263fd07e577855b0250e987b.png)

此处名称可以随意填写，主机为 `conxxx`，也就是登陆指令`root@conxxx`中的 `conxxx` 部分，端口为 `24503`，也就是登录指令中`ssh -p 24503 root@conxxx`的 `24503` 部分，用户名为 `root`，也就是`root@conxxx`中的 `root`，而密码则是 `wxxxx`，也就是此前复制的密码：

![](img/6406517ba66e1cecd1db170ce1fc6560.png)

创建完成后回到连接管理器页面，双击刚刚创建的连接：

![](img/c4d55e3f22135eaaedfd90ab13478393.png)

即可进入到连接页面，此时点击接受并保存即可：

![](img/bed035c9a1c807e75fc77aa2e04d6f3f.png)

当进入到如下页面，则说明已经连接成功：

![](img/2f9f5cdaf4eb7a2282e30a563d36a773.png)

接下来即可借助FinalShell的命令行来操作远程服务器，在远程服务器上进行模型训练相关操作。

![](img/91ecdfe994b8f4af7080ce27035692a7.png)

#### Step 3. 创建虚拟环境

+ **打开finalshell，创建和服务器的shell连接**

![](img/2466961811c8164d611fe353b2092b78.png)

![](img/c3e7ea6ea3eea36e0569fa76e9f4ac71.png)



# 创建`MateConv`虚拟环境（虚拟环境名称可自定义）

```
conda create --name MateConv python=3.10
conda init
source ~/.bashrc
conda activate MateConv
```

![](img/2d3e33dfbc03e3563acaf4b7e9fd4b84.png)

## 创建Jupyter Kernel

```
conda install jupyterlab
conda install ipykernel
python -m ipykernel install --user --name MateConv --display-name "Python (MateConv)"
```

## 创建项目主目录

```
cd ~/autodl-tmp/
mkdir MateConv
```

![](img/0fdde3fef778fcdaf88c5888770564a5.png)

## 打开Jupyter

```
cd ~/autodl-tmp/MateConv
juputer lab --allow-root
```

![](img/1f2cb15585e575ee8bec63ec4e48606a.png)

## 本地连接Jupyter并查看虚拟环境kernel

当服务器开启Jupyter时，Windows环境下，本地可通过`AutoDL-SSH-Tools`进行访问，下图扫码添加英英助教，回复LLM，即可领取本次公开课课件：

![](img/cf9d6669e5eb1bb56580464acc4e9f9f.png)

其中`AutoDL-SSH-Tools`软件就在本次公开课的课件中：

![](img/b10c2c6e7504e70aeb5d9214ce561745.png)

下载完成后解压缩后双击打开`AutoDL.exe`

![](img/f349b523bb57e540280b3272ba57b741.png)

在弹出的栏页中，SSH指令和密码就是AutoDL官网上给每个实例提供的密钥：

![](img/4f53047cc68d039b8911069aede1606b.png)

![](img/b9c8b9fe6d1cf61d17e8017c513d8ff1.png)

代理端口填写`8889`（也就是`Juypter`运行端口），点击开始代理，然后点击访问即可：

![](img/cefe91d6df9a0bcf88b064d7cb2efdcd.png)

![](img/10e644c8af421d4757201788593e2ed0.png)

然后任意打开一个Kernel，选择找到刚才安装的虚拟环境即可

![](img/f988dbc9c21d7d85b900914fecf8cdfb.png)

> Mac登陆指令，打开终端并输入：（以下要填入自己的`URL`和`port`端口号）

```
ssh -CNg -L 8889:127.0.0.1:8889 root@YOUR_URL -p port
```

> 然后根据提示输入密码

接下来再次点击`finalshell`中`+`号，再次创建一个和服务器的连接：

![](img/f9f8a58d85b7462c123d0eb23350d7b3.png)

![](img/9e29df5956c0fb47dc3639a71494b7f7.png)

![](img/2cc13aa2453c843979ce8efb4ea156f8.png)

然后再次进入到虚拟环境，准备后续的操作

```
conda activate MateConv
```

![](img/10ed19775f07127b5b0330227e70d230.png)

《AutoDL公开课》：https://www.bilibili.com/video/BV1bxB7YYEST

![](img/488277b5e7c582821f0688d58e1f968d.png)

---

# GraphRAG 安装与项目创建

## Step 1. 使用`pip`安装`graphrag`

```
pip install graphrag
```

```
!pip install graphrag
```



```
Looking in indexes: http://mirrors.aliyun.com/pypi/simple
Requirement already satisfied: graphrag in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (0.5.0)
Requirement already satisfied: aiofiles<25.0.0,>=24.1.0 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from graphrag) (24.1.0)
Requirement already satisfied: aiolimiter<2.0.0,>=1.1.0 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from graphrag) (1.1.0)
Requirement already satisfied: azure-identity<2.0.0,>=1.17.1 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from graphrag) (1.19.0)
Requirement already satisfied: azure-search-documents<12.0.0,>=11.4.0 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from graphrag) (11.5.2)
Requirement already satisfied: azure-storage-blob<13.0.0,>=12.22.0 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from graphrag) (12.24.0)
Requirement already satisfied: datashaper<0.0.50,>=0.0.49 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from graphrag) (0.0.49)
Requirement already satisfied: devtools<0.13.0,>=0.12.2 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from graphrag) (0.12.2)
Requirement already satisfied: environs<12.0.0,>=11.0.0 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from graphrag) (11.2.1)
Requirement already satisfied: future<2.0.0,>=1.0.0 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from graphrag) (1.0.0)
Requirement already satisfied: graspologic<4.0.0,>=3.4.1 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from graphrag) (3.4.1)
Requirement already satisfied: json-repair<0.31.0,>=0.30.0 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from graphrag) (0.30.2)
Requirement already satisfied: lancedb<0.14.0,>=0.13.0 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from graphrag) (0.13.0)
Requirement already satisfied: matplotlib<4.0.0,>=3.9.0 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from graphrag) (3.9.2)
Requirement already satisfied: networkx<4,>=3 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from graphrag) (3.4.2)
Requirement already satisfied: nltk==3.9.1 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from graphrag) (3.9.1)
Requirement already satisfied: numpy<2.0.0,>=1.25.2 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from graphrag) (1.26.4)
Requirement already satisfied: openai<2.0.0,>=1.51.2 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from graphrag) (1.55.1)
Requirement already satisfied: pandas<3.0.0,>=2.2.3 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from graphrag) (2.2.3)
Requirement already satisfied: pyaml-env<2.0.0,>=1.2.1 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from graphrag) (1.2.1)
Requirement already satisfied: pyarrow<16.0.0,>=15.0.0 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from graphrag) (15.0.2)
Requirement already satisfied: pydantic<3.0.0,>=2.9.2 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from graphrag) (2.10.1)
Requirement already satisfied: python-dotenv<2.0.0,>=1.0.0 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from graphrag) (1.0.1)
Requirement already satisfied: pyyaml<7.0.0,>=6.0.2 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from graphrag) (6.0.2)
Requirement already satisfied: rich<14.0.0,>=13.6.0 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from graphrag) (13.9.4)
Requirement already satisfied: tenacity<10.0.0,>=9.0.0 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from graphrag) (9.0.0)
Requirement already satisfied: tiktoken<0.8.0,>=0.7.0 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from graphrag) (0.7.0)
Requirement already satisfied: typer<0.13.0,>=0.12.5 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from graphrag) (0.12.5)
Requirement already satisfied: typing-extensions<5.0.0,>=4.12.2 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from graphrag) (4.12.2)
Requirement already satisfied: umap-learn<0.6.0,>=0.5.6 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from graphrag) (0.5.7)
Requirement already satisfied: click in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from nltk==3.9.1->graphrag) (8.1.7)
Requirement already satisfied: joblib in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from nltk==3.9.1->graphrag) (1.4.2)
Requirement already satisfied: regex>=2021.8.3 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from nltk==3.9.1->graphrag) (2024.11.6)
Requirement already satisfied: tqdm in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from nltk==3.9.1->graphrag) (4.67.1)
Requirement already satisfied: azure-core>=1.31.0 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from azure-identity<2.0.0,>=1.17.1->graphrag) (1.32.0)
Requirement already satisfied: cryptography>=2.5 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from azure-identity<2.0.0,>=1.17.1->graphrag) (43.0.3)
Requirement already satisfied: msal>=1.30.0 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from azure-identity<2.0.0,>=1.17.1->graphrag) (1.31.1)
Requirement already satisfied: msal-extensions>=1.2.0 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from azure-identity<2.0.0,>=1.17.1->graphrag) (1.2.0)
Requirement already satisfied: azure-common>=1.1 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from azure-search-documents<12.0.0,>=11.4.0->graphrag) (1.1.28)
Requirement already satisfied: isodate>=0.6.0 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from azure-search-documents<12.0.0,>=11.4.0->graphrag) (0.7.2)
Requirement already satisfied: diskcache<6.0.0,>=5.6.3 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from datashaper<0.0.50,>=0.0.49->graphrag) (5.6.3)
Requirement already satisfied: jsonschema<5.0.0,>=4.21.1 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from datashaper<0.0.50,>=0.0.49->graphrag) (4.23.0)
Requirement already satisfied: asttokens<3.0.0,>=2.0.0 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from devtools<0.13.0,>=0.12.2->graphrag) (2.0.5)
Requirement already satisfied: executing>=1.1.1 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from devtools<0.13.0,>=0.12.2->graphrag) (2.1.0)
Requirement already satisfied: pygments>=2.15.0 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from devtools<0.13.0,>=0.12.2->graphrag) (2.15.1)
Requirement already satisfied: marshmallow>=3.13.0 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from environs<12.0.0,>=11.0.0->graphrag) (3.23.1)
Requirement already satisfied: POT<0.10,>=0.9 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from graspologic<4.0.0,>=3.4.1->graphrag) (0.9.5)
Requirement already satisfied: anytree<3.0.0,>=2.12.1 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from graspologic<4.0.0,>=3.4.1->graphrag) (2.12.1)
Requirement already satisfied: beartype<0.19.0,>=0.18.5 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from graspologic<4.0.0,>=3.4.1->graphrag) (0.18.5)
Requirement already satisfied: gensim<5.0.0,>=4.3.2 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from graspologic<4.0.0,>=3.4.1->graphrag) (4.3.3)
Requirement already satisfied: graspologic-native<2.0.0,>=1.2.1 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from graspologic<4.0.0,>=3.4.1->graphrag) (1.2.1)
Requirement already satisfied: hyppo<0.5.0,>=0.4.0 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from graspologic<4.0.0,>=3.4.1->graphrag) (0.4.0)
Requirement already satisfied: scikit-learn<2.0.0,>=1.4.2 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from graspologic<4.0.0,>=3.4.1->graphrag) (1.5.2)
Requirement already satisfied: scipy==1.12.0 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from graspologic<4.0.0,>=3.4.1->graphrag) (1.12.0)
Requirement already satisfied: seaborn<0.14.0,>=0.13.2 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from graspologic<4.0.0,>=3.4.1->graphrag) (0.13.2)
Requirement already satisfied: statsmodels<0.15.0,>=0.14.2 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from graspologic<4.0.0,>=3.4.1->graphrag) (0.14.4)
Requirement already satisfied: deprecation in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from lancedb<0.14.0,>=0.13.0->graphrag) (2.1.0)
Requirement already satisfied: pylance==0.17.0 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from lancedb<0.14.0,>=0.13.0->graphrag) (0.17.0)
Requirement already satisfied: requests>=2.31.0 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from lancedb<0.14.0,>=0.13.0->graphrag) (2.32.3)
Requirement already satisfied: retry>=0.9.2 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from lancedb<0.14.0,>=0.13.0->graphrag) (0.9.2)
Requirement already satisfied: attrs>=21.3.0 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from lancedb<0.14.0,>=0.13.0->graphrag) (24.2.0)
Requirement already satisfied: packaging in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from lancedb<0.14.0,>=0.13.0->graphrag) (24.1)
Requirement already satisfied: cachetools in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from lancedb<0.14.0,>=0.13.0->graphrag) (5.5.0)
Requirement already satisfied: overrides>=0.7 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from lancedb<0.14.0,>=0.13.0->graphrag) (7.4.0)
Requirement already satisfied: contourpy>=1.0.1 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from matplotlib<4.0.0,>=3.9.0->graphrag) (1.3.1)
Requirement already satisfied: cycler>=0.10 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from matplotlib<4.0.0,>=3.9.0->graphrag) (0.12.1)
Requirement already satisfied: fonttools>=4.22.0 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from matplotlib<4.0.0,>=3.9.0->graphrag) (4.55.0)
Requirement already satisfied: kiwisolver>=1.3.1 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from matplotlib<4.0.0,>=3.9.0->graphrag) (1.4.7)
Requirement already satisfied: pillow>=8 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from matplotlib<4.0.0,>=3.9.0->graphrag) (11.0.0)
Requirement already satisfied: pyparsing>=2.3.1 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from matplotlib<4.0.0,>=3.9.0->graphrag) (3.2.0)
Requirement already satisfied: python-dateutil>=2.7 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from matplotlib<4.0.0,>=3.9.0->graphrag) (2.9.0.post0)
Requirement already satisfied: anyio<5,>=3.5.0 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from openai<2.0.0,>=1.51.2->graphrag) (4.6.2)
Requirement already satisfied: distro<2,>=1.7.0 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from openai<2.0.0,>=1.51.2->graphrag) (1.9.0)
Requirement already satisfied: httpx<1,>=0.23.0 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from openai<2.0.0,>=1.51.2->graphrag) (0.27.0)
Requirement already satisfied: jiter<1,>=0.4.0 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from openai<2.0.0,>=1.51.2->graphrag) (0.7.1)
Requirement already satisfied: sniffio in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from openai<2.0.0,>=1.51.2->graphrag) (1.3.0)
Requirement already satisfied: pytz>=2020.1 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from pandas<3.0.0,>=2.2.3->graphrag) (2024.1)
Requirement already satisfied: tzdata>=2022.7 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from pandas<3.0.0,>=2.2.3->graphrag) (2024.2)
Requirement already satisfied: annotated-types>=0.6.0 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from pydantic<3.0.0,>=2.9.2->graphrag) (0.7.0)
Requirement already satisfied: pydantic-core==2.27.1 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from pydantic<3.0.0,>=2.9.2->graphrag) (2.27.1)
Requirement already satisfied: markdown-it-py>=2.2.0 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from rich<14.0.0,>=13.6.0->graphrag) (3.0.0)
Requirement already satisfied: shellingham>=1.3.0 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from typer<0.13.0,>=0.12.5->graphrag) (1.5.4)
Requirement already satisfied: numba>=0.51.2 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from umap-learn<0.6.0,>=0.5.6->graphrag) (0.60.0)
Requirement already satisfied: pynndescent>=0.5 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from umap-learn<0.6.0,>=0.5.6->graphrag) (0.5.13)
Requirement already satisfied: idna>=2.8 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from anyio<5,>=3.5.0->openai<2.0.0,>=1.51.2->graphrag) (3.7)
Requirement already satisfied: six in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from anytree<3.0.0,>=2.12.1->graspologic<4.0.0,>=3.4.1->graphrag) (1.16.0)
Requirement already satisfied: cffi>=1.12 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from cryptography>=2.5->azure-identity<2.0.0,>=1.17.1->graphrag) (1.17.1)
Requirement already satisfied: smart-open>=1.8.1 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from gensim<5.0.0,>=4.3.2->graspologic<4.0.0,>=3.4.1->graphrag) (7.0.5)
Requirement already satisfied: certifi in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from httpx<1,>=0.23.0->openai<2.0.0,>=1.51.2->graphrag) (2024.8.30)
Requirement already satisfied: httpcore==1.* in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from httpx<1,>=0.23.0->openai<2.0.0,>=1.51.2->graphrag) (1.0.2)
Requirement already satisfied: h11<0.15,>=0.13 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from httpcore==1.*->httpx<1,>=0.23.0->openai<2.0.0,>=1.51.2->graphrag) (0.14.0)
Requirement already satisfied: autograd>=1.3 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from hyppo<0.5.0,>=0.4.0->graspologic<4.0.0,>=3.4.1->graphrag) (1.7.0)
Requirement already satisfied: jsonschema-specifications>=2023.03.6 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from jsonschema<5.0.0,>=4.21.1->datashaper<0.0.50,>=0.0.49->graphrag) (2023.7.1)
Requirement already satisfied: referencing>=0.28.4 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from jsonschema<5.0.0,>=4.21.1->datashaper<0.0.50,>=0.0.49->graphrag) (0.30.2)
Requirement already satisfied: rpds-py>=0.7.1 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from jsonschema<5.0.0,>=4.21.1->datashaper<0.0.50,>=0.0.49->graphrag) (0.10.6)
Requirement already satisfied: mdurl~=0.1 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from markdown-it-py>=2.2.0->rich<14.0.0,>=13.6.0->graphrag) (0.1.2)
Requirement already satisfied: PyJWT<3,>=1.0.0 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from PyJWT[crypto]<3,>=1.0.0->msal>=1.30.0->azure-identity<2.0.0,>=1.17.1->graphrag) (2.10.0)
Requirement already satisfied: portalocker<3,>=1.4 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from msal-extensions>=1.2.0->azure-identity<2.0.0,>=1.17.1->graphrag) (2.10.1)
Requirement already satisfied: llvmlite<0.44,>=0.43.0dev0 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from numba>=0.51.2->umap-learn<0.6.0,>=0.5.6->graphrag) (0.43.0)
Requirement already satisfied: charset-normalizer<4,>=2 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from requests>=2.31.0->lancedb<0.14.0,>=0.13.0->graphrag) (3.3.2)
Requirement already satisfied: urllib3<3,>=1.21.1 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from requests>=2.31.0->lancedb<0.14.0,>=0.13.0->graphrag) (2.2.3)
Requirement already satisfied: decorator>=3.4.2 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from retry>=0.9.2->lancedb<0.14.0,>=0.13.0->graphrag) (5.1.1)
Requirement already satisfied: py<2.0.0,>=1.4.26 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from retry>=0.9.2->lancedb<0.14.0,>=0.13.0->graphrag) (1.11.0)
Requirement already satisfied: threadpoolctl>=3.1.0 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from scikit-learn<2.0.0,>=1.4.2->graspologic<4.0.0,>=3.4.1->graphrag) (3.5.0)
Requirement already satisfied: patsy>=0.5.6 in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from statsmodels<0.15.0,>=0.14.2->graspologic<4.0.0,>=3.4.1->graphrag) (1.0.1)
Requirement already satisfied: pycparser in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from cffi>=1.12->cryptography>=2.5->azure-identity<2.0.0,>=1.17.1->graphrag) (2.21)
Requirement already satisfied: wrapt in /root/miniconda3/envs/graphrag/lib/python3.11/site-packages (from smart-open>=1.8.1->gensim<5.0.0,>=4.3.2->graspologic<4.0.0,>=3.4.1->graphrag) (1.17.0)
WARNING: Running pip as the 'root' user can result in broken permissions and conflicting behaviour with the system package manager, possibly rendering your system unusable.It is recommended to use a virtual environment instead: https://pip.pypa.io/warnings/venv. Use the --root-user-action option if you know what you are doing and want to suppress this warning.
```



# 本次公开课采用 GraphRAG 最新版 v1.1.0 进行教学

## 新版特性

+ **简化项目设置**：引入了 `init` 命令，生成简化的 `settings.yml` 文件，预设核心配置，降低初始配置的复杂度。

+ **扩展命令行界面 (CLI)**：采用 `Typer` 框架，提供更丰富的内联文档和功能，使 CLI 的启动时间从平均 148 秒缩短至 2 秒。

+ **统一 API 层**：推出独立的 API 层，简化开发者集成，CLI 和加速器均基于此 API 构建，提供了与 API 交互的示例。

+ **简化数据模型**：优化数据结构，移除冗余字段，减少存储空间，提高读取速度和内存使用效率，输出的 `Parquet` 文件大小减少了 80%，总磁盘空间减少了 43%。

+ **优化向量存储**：默认在索引期间创建向量存储，避免后续处理，减少读取时间和内存占用，支持 `LanceDB` 和 `Azure AI Search`，默认使用 `LanceDB` 并写入本地数据库。

+ **扁平化代码结构**：简化代码库，减少层级深度，提升可维护性和可读性，合并工作流，减少未使用的输出，提高性能。

+ **增量摄取**：新增 `update` 命令，智能合并新内容与现有索引，减少重新索引的需求，利用 LLM 缓存机制，降低成本，加快处理速度。

这些改进旨在提升 GraphRAG 的易用性、性能和可扩展性，为开发者和用户提供更高效的工具。

![](img/5d8b51aebac8ed96103b69a686f720a0.png)

### Step 2. 创建检索项目文件夹

```
mkdir -p ./openl/input
```

![](img/1ce369d8bdb9543a7ddec98d4a575a9f.png)

![](img/d29b63448397a8236d39b46ed4945743.png)

### Step 3. 上传数据集

![](img/5ce638b5e9838ead869da17a9668461a.png)

![](img/3a499ad2f695c6f0d1a3d209b5bd5ac8.png)

> 相关数据集已同步至本节公开课的课件包，扫码添加老师助理即可领取：

![](img/0b5b0dd2874d1bb9905dd79ac85100a2.png)

### Step 4. 初始化项目文件

```
graphrag init --root ./openl
```

```
!graphrag init --root ./openl
```

```
[2KInitializing project at [35m/root/autodl-tmp/graphrag/[0m[95mopenl[0m
⠋ GraphRAG Indexer 
```

### Step 5. 修改项目配置

![](img/2c9cf684382524f9a194b992d03ae5ba.png)

打开 `.env` 文件，填写 DeepSeek API-KEY

![](img/490c84474895f014abac93a873c17158.png)

打开 `setting.yaml` 文件，填写模型名称和反向代理地址：

![](img/0a8b59e33a6d584d19fafbaf47e6d073.png)

其中反向代理地址为 `api_base: https://ai.devtool.tech/proxy/v1`

### 【可选】Step 6. 验证 API-KEY 和反向代理地址是否可以正常运行

```
from openai import OpenAI
```

```
api_key = 'your-openai-api-key'
```

```
# 实例化客户端
client = OpenAI(api_key=api_key, 
                base_url="https://ai.devtool.tech/proxy/v1")
```

```
# 调用 GPT-4o-mini 模型
response = client.chat.completions.create(
    model="gpt-4o-mini",
    messages=[
        {"role": "user", "content": "你好，好久不见!"}
    ]
)
```

```
# 输出生成的响应内容
print(response.choices[0].message.content)
```

```
你好！好久不见！你最近怎么样？有什么新鲜事分享吗？
```

然后查看当前 API-KEY 可以调用的模型：

```
models_list = client.models.list()
```

```
models_list.data
```

```
[Model(id='gpt-4o-2024-08-06', created=1722814719, object='model', owned_by='system'),
 Model(id='gpt-4o', created=1715367049, object='model', owned_by='system'),
 Model(id='o1-mini-2024-09-12', created=1725648979, object='model', owned_by='system'),
 Model(id='dall-e-2', created=1698798177, object='model', owned_by='system'),
 Model(id='gpt-4-1106-preview', created=1698957206, object='model', owned_by='system'),
 Model(id='gpt-3.5-turbo', created=1677610602, object='model', owned_by='openai'),
 Model(id='text-embedding-3-large', created=1705953180, object='model', owned_by='system'),
 Model(id='gpt-3.5-turbo-0125', created=1706048358, object='model', owned_by='system'),
 Model(id='babbage-002', created=1692634615, object='model', owned_by='system'),
 Model(id='gpt-4o-2024-11-20', created=1731975040, object='model', owned_by='system'),
 Model(id='gpt-4-turbo-preview', created=1706037777, object='model', owned_by='system'),
 Model(id='davinci-002', created=1692634301, object='model', owned_by='system'),
 Model(id='gpt-4-0125-preview', created=1706037612, object='model', owned_by='system'),
 Model(id='whisper-1', created=1677532384, object='model', owned_by='openai-internal'),
 Model(id='gpt-4o-mini', created=1721172741, object='model', owned_by='system'),
 Model(id='dall-e-3', created=1698785189, object='model', owned_by='system'),
 Model(id='gpt-4o-mini-2024-07-18', created=1721172717, object='model', owned_by='system'),
 Model(id='o1-preview', created=1725648897, object='model', owned_by='system'),
 Model(id='gpt-3.5-turbo-16k', created=1683758102, object='model', owned_by='openai-internal'),
 Model(id='o1-preview-2024-09-12', created=1725648865, object='model', owned_by='system'),
 Model(id='o1-mini', created=1725649008, object='model', owned_by='system'),
 Model(id='gpt-4o-2024-05-13', created=1715368132, object='model', owned_by='system'),
 Model(id='gpt-4o-realtime-preview', created=1727659998, object='model', owned_by='system'),
 Model(id='tts-1-hd-1106', created=1699053533, object='model', owned_by='system'),
 Model(id='gpt-4o-realtime-preview-2024-10-01', created=1727131766, object='model', owned_by='system'),
 Model(id='gpt-4', created=1687882411, object='model', owned_by='openai'),
 Model(id='gpt-4-0613', created=1686588896, object='model', owned_by='openai'),
 Model(id='text-embedding-ada-002', created=1671217299, object='model', owned_by='openai-internal'),
 Model(id='text-embedding-3-small', created=1705948997, object='model', owned_by='system'),
 Model(id='tts-1-hd', created=1699046015, object='model', owned_by='system'),
 Model(id='gpt-4-turbo', created=1712361441, object='model', owned_by='system'),
 Model(id='gpt-4-turbo-2024-04-09', created=1712601677, object='model', owned_by='system'),
 Model(id='gpt-3.5-turbo-1106', created=1698959748, object='model', owned_by='system'),
 Model(id='gpt-3.5-turbo-instruct', created=1692901427, object='model', owned_by='system'),
 Model(id='gpt-4o-audio-preview', created=1727460443, object='model', owned_by='system'),
 Model(id='gpt-4o-audio-preview-2024-10-01', created=1727389042, object='model', owned_by='system'),
 Model(id='tts-1', created=1681940951, object='model', owned_by='openai-internal'),
 Model(id='tts-1-1106', created=1699053241, object='model', owned_by='system'),
 Model(id='gpt-3.5-turbo-instruct-0914', created=1694122472, object='model', owned_by='system'),
 Model(id='chatgpt-4o-latest', created=1723515131, object='model', owned_by='system')]
```

## 2. GraphRAG 索引 Indexing 过程执行

一切准备就绪后，即可开始执行 GraphRAG 索引过程。

### Step 7. 借助 GraphRAG 脚本自动执行 indexing

```
!graphrag index --root ./openl
```



```
Logging enabled at /root/autodl-tmp/graphrag/openl/logs/indexing-engine.log

GraphRAG Indexer
├── Loading Input (InputFileType.text) - 1 files loaded (0 filtered)
🚀 create_base_text_units

├── Loading Input (InputFileType.text) - 1 files loaded (0 filtered)
Empty DataFrame
Columns: []
Index: []

├── Loading Input (InputFileType.text) - 1 files loaded (0 filtered)
├── create_base_text_units
🚀 create_final_documents

├── Loading Input (InputFileType.text) - 1 files loaded (0 filtered)
├── create_base_text_units
                                 id  ...                                       text_unit_ids
0  c546fa97b72c8b72b7efb0c1ac45cb1d  ...  [0653a697b64dd8c029503ffc22af9ec3, b1fce21a45f...
[1 rows x 5 columns]

├── Loading Input (InputFileType.text) - 1 files loaded (0 filtered)
├── create_base_text_units
├── create_final_documents
└── create_base_entity_graph
🚀 create_base_entity_graph

├── Loading Input (InputFileType.text) - 1 files loaded (0 filtered)
├── create_base_text_units
├── create_final_documents
├── create_base_entity_graph
🚀 create_final_entities

├── Loading Input (InputFileType.text) - 1 files loaded (0 filtered)
├── create_base_text_units
├── create_final_documents
├── create_base_entity_graph
                                  id  ...                                       text_unit_ids
0   22113170e6c742ba940314e15c401dc1  ...  [0653a697b64dd8c029503ffc22af9ec3, 08530861b09...
1   23e8813975cc43d7921375da3c781c3c  ...  [0653a697b64dd8c029503ffc22af9ec3, 08530861b09...
2   3c4aa4fadb5c4b3f9244a7bb00598cab  ...                  [0653a697b64dd8c029503ffc22af9ec3]
3   0ebac52d90264cef8bf563ffe894bbe5  ...                  [0653a697b64dd8c029503ffc22af9ec3]
4   d45fe94d4f2840b48ebd2564ddc85e56  ...                  [0653a697b64dd8c029503ffc22af9ec3]
5   8979a4d40b04416a91f084e057be0c98  ...                  [0653a697b64dd8c029503ffc22af9ec3]
6   5a57bda8d0534c43a2591fde9519ef2e  ...                  [0653a697b64dd8c029503ffc22af9ec3]
7   9ac71842fd5b47628a2cc43bb13b1290  ...                  [0653a697b64dd8c029503ffc22af9ec3]
8   71c5eabc1ce9490f85f9357f6b7f309b  ...                  [08530861b09098077a0ebcd53ab3ae62]
9   de19324beea8438497852259c73c452a  ...                  [08530861b09098077a0ebcd53ab3ae62]
10  67f0629e10054ba49b2d1bfd08985927  ...                  [08530861b09098077a0ebcd53ab3ae62]
[11 rows x 6 columns]

├── Loading Input (InputFileType.text) - 1 files loaded (0 filtered)
├── create_base_text_units
├── create_final_documents
├── create_base_entity_graph
├── create_final_entities
🚀 create_final_nodes

├── Loading Input (InputFileType.text) - 1 files loaded (0 filtered)
├── create_base_text_units
├── create_final_documents
├── create_base_entity_graph
├── create_final_entities
                                  id  human_readable_id  ...  x  y
0   22113170e6c742ba940314e15c401dc1                  0  ...  0  0
1   23e8813975cc43d7921375da3c781c3c                  1  ...  0  0
2   3c4aa4fadb5c4b3f9244a7bb00598cab                  2  ...  0  0
3   0ebac52d90264cef8bf563ffe894bbe5                  3  ...  0  0
4   d45fe94d4f2840b48ebd2564ddc85e56                  4  ...  0  0
5   8979a4d40b04416a91f084e057be0c98                  5  ...  0  0
6   5a57bda8d0534c43a2591fde9519ef2e                  6  ...  0  0
7   9ac71842fd5b47628a2cc43bb13b1290                  7  ...  0  0
8   71c5eabc1ce9490f85f9357f6b7f309b                  8  ...  0  0
9   de19324beea8438497852259c73c452a                  9  ...  0  0
10  67f0629e10054ba49b2d1bfd08985927                 10  ...  0  0
11  22113170e6c742ba940314e15c401dc1                  0  ...  0  0
12  23e8813975cc43d7921375da3c781c3c                  1  ...  0  0
13  3c4aa4fadb5c4b3f9244a7bb00598cab                  2  ...  0  0
14  0ebac52d90264cef8bf563ffe894bbe5                  3  ...  0  0
15  d45fe94d4f2840b48ebd2564ddc85e56                  4  ...  0  0
16  8979a4d40b04416a91f084e057be0c98                  5  ...  0  0
17  5a57bda8d0534c43a2591fde9519ef2e                  6  ...  0  0
18  9ac71842fd5b47628a2cc43bb13b1290                  7  ...  0  0
19  71c5eabc1ce9490f85f9357f6b7f309b                  8  ...  0  0
20  de19324beea8438497852259c73c452a                  9  ...  0  0
21  67f0629e10054ba49b2d1bfd08985927                 10  ...  0  0
[22 rows x 8 columns]

├── Loading Input (InputFileType.text) - 1 files loaded (0 filtered)
├── create_base_text_units
├── create_final_documents
├── create_base_entity_graph
├── create_final_entities
├── create_final_nodes
🚀 create_final_communities

├── Loading Input (InputFileType.text) - 1 files loaded (0 filtered)
├── create_base_text_units
├── create_final_documents
├── create_base_entity_graph
├── create_final_entities
├── create_final_nodes
                                     id  human_readable_id  ...      period   size
0  17bfead0-7ccc-438a-922b-5cd6deea39ea                  0  ...  2024-11-28     11
1  8eb758a5-95a9-4e42-93e2-5ac1adff8a42                  1  ...  2024-11-28      7
2  7b75cf95-4504-47a7-8b11-7c90fe66109b                  2  ...  2024-11-28      4
[3 rows x 10 columns]

├── Loading Input (InputFileType.text) - 1 files loaded (0 filtered)
├── create_base_text_units
├── create_final_documents
├── create_base_entity_graph
├── create_final_entities
├── create_final_nodes
├── create_final_communities
🚀 create_final_relationships

├── Loading Input (InputFileType.text) - 1 files loaded (0 filtered)
├── create_base_text_units
├── create_final_documents
├── create_base_entity_graph
├── create_final_entities
├── create_final_nodes
├── create_final_communities
                                 id  ...                                       text_unit_ids
0  813887b9c3f54445832a00a8d1b47264  ...  [0653a697b64dd8c029503ffc22af9ec3, 08530861b09...
1  16f369775666497b8ece38456683313e  ...                  [0653a697b64dd8c029503ffc22af9ec3]
2  6e17fd4dce3a432195f428ec5287c71d  ...                  [0653a697b64dd8c029503ffc22af9ec3]
3  121cc7bef78848b69ec89e082cb86a54  ...                  [0653a697b64dd8c029503ffc22af9ec3]
4  294c8d020d0b4b389070c7c1959f1cc7  ...                  [0653a697b64dd8c029503ffc22af9ec3]
5  6110c0c995e34a3896bbc4a0274ecc6f  ...                  [0653a697b64dd8c029503ffc22af9ec3]
6  188cab571d5c4b0197172dd2deab34a5  ...                  [0653a697b64dd8c029503ffc22af9ec3]
7  4979732df79645ed88eea6e957cab2c7  ...                  [08530861b09098077a0ebcd53ab3ae62]
8  8a02e132514440d99006833312db5a5c  ...                  [08530861b09098077a0ebcd53ab3ae62]
9  3584cb0ca80a463c8e658b17bc7ef254  ...                  [08530861b09098077a0ebcd53ab3ae62]
[10 rows x 8 columns]

├── Loading Input (InputFileType.text) - 1 files loaded (0 filtered)
├── create_base_text_units
├── create_final_documents
├── create_base_entity_graph
├── create_final_entities
├── create_final_nodes
├── create_final_communities
├── create_final_relationships
🚀 create_final_text_units

├── Loading Input (InputFileType.text) - 1 files loaded (0 filtered)
├── create_base_text_units
├── create_final_documents
├── create_base_entity_graph
├── create_final_entities
├── create_final_nodes
├── create_final_communities
├── create_final_relationships
                                 id  ...                                    relationship_ids
0  0653a697b64dd8c029503ffc22af9ec3  ...  [813887b9c3f54445832a00a8d1b47264, 16f36977566...
1  b1fce21a45f01c5ef14bafc9fe3bab1d  ...                                         None
2  08530861b09098077a0ebcd53ab3ae62  ...  [813887b9c3f54445832a00a8d1b47264, 4979732df79...
3  e97cc7c63047f6471b4beac2f375eae0  ...                                         None
[4 rows x 7 columns]

├── Loading Input (InputFileType.text) - 1 files loaded (0 filtered)
├── create_base_text_units
├── create_final_documents
├── create_base_entity_graph
├── create_final_entities
├── create_final_nodes
├── create_final_communities
├── create_final_relationships
├── create_final_text_units
🚀 create_final_community_reports

├── Loading Input (InputFileType.text) - 1 files loaded (0 filtered)
├── create_base_text_units
├── create_final_documents
├── create_base_entity_graph
├── create_final_entities
├── create_final_nodes
├── create_final_communities
├── create_final_relationships
├── create_final_text_units
                                     id  human_readable_id  ...      period   size
0  100a5af4-6579-4fdc-afba-78565d6fe062                  1  ...  2024-11-28      7
1  0e54e2ef-44ef-4bfa-8a8a-fd06f79ed338                  2  ...  2024-11-28      4
2  e527793c-b726-41be-892f-979334ae5073                  0  ...  2024-11-28     11
[3 rows x 13 columns]

├── Loading Input (InputFileType.text) - 1 files loaded (0 filtered)
├── create_base_text_units
├── create_final_documents
├── create_base_entity_graph
├── create_final_entities
├── create_final_nodes
├── create_final_communities
├── create_final_relationships
├── create_final_text_units
├── create_final_community_reports
└── generate_text_embeddings
[2024-11-28T08:20:05Z  WARN  lance::dataset] No existing dataset at /root/autodl-tmp/graphrag/openl/output/lancedb/default-text_unit-text.lance, it will be created
[2024-11-28T08:20:08Z  WARN  lance::dataset] No existing dataset at /root/autodl-tmp/graphrag/openl/output/lancedb/default-entity-description.lance, it will be created
[2024-11-28T08:20:09Z  WARN  lance::dataset] No existing dataset at /root/autodl-tmp/graphrag/openl/output/lancedb/default-community-full_content.lance, it will be created
🚀 generate_text_embeddings

├── Loading Input (InputFileType.text) - 1 files loaded (0 filtered)
├── create_base_text_units
├── create_final_documents
├── create_base_entity_graph
├── create_final_entities
├── create_final_nodes
├── create_final_communities
├── create_final_relationships
├── create_final_text_units
├── create_final_community_reports
└── generate_text_embeddings
Empty DataFrame
Columns: []
Index: []

├── Loading Input (InputFileType.text) - 1 files loaded (0 filtered)
├── create_base_text_units
├── create_final_documents
├── create_base_entity_graph
├── create_final_entities
├── create_final_nodes
├── create_final_communities
├── create_final_relationships
├── create_final_text_units
├── create_final_community_reports
└── generate_text_embeddings
🚀 All workflows completed successfully.
```



# GraphRAG 知识图谱输出结构

该命令也可以在终端内运行，结果如图：

![](img/1c27ba097c132fee058bdaed4bfc2cad.png)

运行结束后，各知识图谱相关数据集都保存在`output`文件夹中：

![](img/748b72dc25ecaffee275235476dbd34f.png)

![](img/c93d6c14d264dd0efe358bff9760b077.png)

`GraphRAG`在索引阶段构建的知识图谱的确是以 `Parquet` 格式保存的！索引阶段的输出文件中，`Parquet` 文件存储了知识图谱的各个核心组成部分，例如实体、关系、社区信息以及文本单元等。这些文件共同组成了一个完整的知识图谱。

## 索引阶段主要输出

1. 实体表（Nodes Table）

+ 文件名：`create_final_nodes.parquet`
+ 内容：知识图谱中的实体节点（例如：人、地点、组织）。
+ 包含信息：
+ 实体的名称（如 `"John Doe"`）。
+ 实体的类别（如 `"PERSON"`, `"ORGANIZATION"`, `"LOCATION"`）。
+ 与社区相关的信息（如实体所属的社区）。
+ 实体的度数（`degree`），表示该实体在图谱中的连接数。

1. 关系表（Relationships Table）

+ 文件名：`create_final_relationships.parquet`
+ 内容：知识图谱中实体之间的关系（即图谱的边）。
+ 包含信息：
+ 两个实体之间的关系描述（例如 `"works for"`, `"lives in"`）。
+ 关系的强度（数值化，用于衡量关系的显著性或重要性）。

1. 嵌入向量表（Entity Embedding Table）

+ 文件名：`create_final_entities.parquet`
+ 内容：实体的语义嵌入，用于表示实体的语义信息。
+ 用途：支持语义搜索（通过嵌入计算实体之间的相似性）。

1. 社区报告表（Community Reports Table）

+ 文件名：`create_final_community_reports.parquet`
+ 内容：社区的摘要信息。
+ 用途：支持全局搜索（通过社区信息回答关于数据集整体的问题）。

1. 文本单元表（Text Units Table）

+ 文件名：`create_final_text_units.parquet`
+ 内容：被切分的原始文本单元（`TextUnits`）。
+ 用途：将知识图谱和原始文本结合，为 `LLM` 提供上下文支持。

1. 社区表（Community Table）

+ 文件名：`create_final_Communities.parquet`
+ 内容：每个社区基本情况。

1. 文件表（Documents Table）

+ 文件名：`create_final_documents.parquet`
+ 内容：用于记录所有参与知识图谱构建的文件情况。

> 为什么用 Parquet 格式保存知识图谱？

## 为何使用 Parquet 格式

1. 高效存储：

+ 知识图谱中的数据通常是结构化的，包含大量的实体、关系、嵌入等。
+ `Parquet` 的列式存储能够显著减少磁盘占用，同时提高读取效率。

1. 快速读取：

+ 查询阶段需要快速加载实体、关系、嵌入等数据到内存中。
+ `Parquet` 支持按需加载所需的列，避免了不必要的数据读取。

1. 兼容性好：

+ `Parquet` 是一个开放的标准，广泛支持各种数据处理工具（如 `Pandas`、`Spark`、`Hadoop`）。
+ `GraphRAG` 可以在 `Python` 中使用 `Pandas` 或其他工具轻松读取这些文件。

## 3. 查看知识图谱相关表格

```python
import pandas as pd
```

文件表（Documents Table）

```python
documents_df = pd.read_parquet("./openl/output/create_final_documents.parquet")
documents_df
```

|  | id | human_readable_id | title | text | text_unit_ids |
| --- | --- | --- | --- | --- | --- |
| 0 | 2ef33abc7acb307f736e46c7f20fe87568b4e9bd6311cc... | 1 | ID3、C4.5决策树的建模流程.txt | Lesson 8.3 ID3、C4.5决策树的建模流程\nID3和C4.5作为的经典决策树算... | [4ba5acd798a419708cfe5cc86f1fcb6df907a20b1992a... |

文本单元表（TEXT UNIT Table）

```python
text_unit_df = pd.read_parquet("./openl/output/create_final_text_units.parquet")
text_unit_df
```

|  | id | human_readable_id | text | n_tokens | document_ids | entity_ids | relationship_ids |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 0 | 4ba5acd798a419708cfe5cc86f1fcb6df907a20b1992ad... | 1 | Lesson 8.3 ID3、C4.5决策树的建模流程\nID3和C4.5作为的经典决策树算... | 1200 | [2ef33abc7acb307f736e46c7f20fe87568b4e9bd6311c... | [35bbb43d-b9f4-4a4d-bd53-7059efc44056, f1535df... | [a0bbf6be-4a2c-4ad4-9e98-0e26fdf9d543, 80278b1... |
| 1 | 7a8436d7970da4b0e5c837b0a3eaaa196e6e2284c7c754... | 2 | 8961919\n然后即可算出按照如此规则进行数据集划分，最终能够减少的不纯度数值：\n# ... | 1200 | [2ef33abc7acb307f736e46c7f20fe87568b4e9bd6311c... | [35bbb43d-b9f4-4a4d-bd53-7059efc44056, f1535df... | [a0bbf6be-4a2c-4ad4-9e98-0e26fdf9d543, 80278b1... |
| 2 | 1141affdc56bf8f5092b950b57febdedb884673ec6b23d... | 3 | 4.5决策树的基本建模流程\n作为ID3的改进版算法，C4.5在ID3的基础上进行了三个方面... | 1200 | [2ef33abc7acb307f736e46c7f20fe87568b4e9bd6311c... | [35bbb43d-b9f4-4a4d-bd53-7059efc44056, f1535df... | [651dfca9-7a44-4d11-aed2-c00ce76f936d, 3ad66ad... |
| 3 | b5df95b6ec7e3aaea3f67db4914e1b2e874f3ac77fc3c8... | 4 | 集划分。\n\nC4.5的连续变量处理方法\nC4.5允许带入连续变量进行建模，并且围绕连续... | 558 | [2ef33abc7acb307f736e46c7f20fe87568b4e9bd6311c... | [35bbb43d-b9f4-4a4d-bd53-7059efc44056, f1535df... | [651dfca9-7a44-4d11-aed2-c00ce76f936d, 9ddcf83... |

实体嵌入表（ENTITIES Table）

```python
entity_embedding_df = pd.read_parquet("./openl/output/create_final_entities.parquet")
entity_embedding_df.head()
```

|  | id | human_readable_id | title | type | description | text_unit_ids |
| --- | --- | --- | --- | --- | --- | --- |
| 0 | 35bbb43d-b9f4-4a4d-bd53-7059efc44056 | 0 | ID3 | ORGANIZATION | ID3 is a classic decision tree algorithm prima... | [4ba5acd798a419708cfe5cc86f1fcb6df907a20b1992a... |
| 1 | f1535df1-c0e1-4bd4-9b18-49b05918a0aa | 1 | C4.5 | ORGANIZATION | C4.5 is an advanced decision tree algorithm an... | [4ba5acd798a419708cfe5cc86f1fcb6df907a20b1992a... |
| 2 | 9afdfaaf-37d8-4990-9406-a62f37c2d64e | 2 | CART | ORGANIZATION | CART (Classification and Regression Trees) is ... | [4ba5acd798a419708cfe5cc86f1fcb6df907a20b1992a... |
| 3 | b3651a61-5626-4162-9f1e-432e6d0e9fe5 | 3 | SKLEARN | ORGANIZATION | SKLEARN, also known as Scikit-learn, is a wide... | [4ba5acd798a419708cfe5cc86f1fcb6df907a20b1992a... |
| 4 | c6f4c391-4f5a-4d99-b8d5-6d4b24f43cc1 | 4 | NUMPY | ORGANIZATION | NumPy is a Python library used for numerical c... | [4ba5acd798a419708cfe5cc86f1fcb6df907a20b1992a... |

实体表（Nodes Table）

```python
entity_df = pd.read_parquet("./openl/output/create_final_nodes.parquet")
entity_df.head()
```

|  | id | human_readable_id | title | community | level | degree | x | y |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 0 | 35bbb43d-b9f4-4a4d-bd53-7059efc44056 | 0 | ID3 | 0 | 0 | 9 | 0 | 0 |
| 1 | f1535df1-c0e1-4bd4-9b18-49b05918a0aa | 1 | C4.5 | 1 | 0 | 11 | 0 | 0 |
| 2 | 9afdfaaf-37d8-4990-9406-a62f37c2d64e | 2 | CART | 0 | 0 | 8 | 0 | 0 |
| 3 | b3651a61-5626-4162-9f1e-432e6d0e9fe5 | 3 | SKLEARN | 0 | 0 | 3 | 0 | 0 |
| 4 | c6f4c391-4f5a-4d99-b8d5-6d4b24f43cc1 | 4 | NUMPY | 2 | 0 | 4 | 0 | 0 |

```python
entity_df
```

|  | id | human_readable_id | title | community | level | degree | x | y |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 0 | 35bbb43d-b9f4-4a4d-bd53-7059efc44056 | 0 | ID3 | 0 | 0 | 9 | 0 | 0 |
| 1 | f1535df1-c0e1-4bd4-9b18-49b05918a0aa | 1 | C4.5 | 1 | 0 | 11 | 0 | 0 |
| 2 | 9afdfaaf-37d8-4990-9406-a62f37c2d64e | 2 | CART | 0 | 0 | 8 | 0 | 0 |
| 3 | b3651a61-5626-4162-9f1e-432e6d0e9fe5 | 3 | SKLEARN | 0 | 0 | 3 | 0 | 0 |
| 4 | c6f4c391-4f5a-4d99-b8d5-6d4b24f43cc1 | 4 | NUMPY | 2 | 0 | 4 | 0 | 0 |
| 5 | f7e0cfe0-7d50-4ea6-8bfe-4f252cf985cb | 5 | ML_BASIC_FUNCTION | 2 | 0 | 1 | 0 | 0 |
| 6 | a0684f1a-0a87-45dc-b240-55b3d04f1961 | 6 | AGE | 0 | 0 | 3 | 0 | 0 |
| 7 | da7c9be6-2768-4baf-9185-5aa389fc91e7 | 7 | INCOME | 0 | 0 | 3 | 0 | 0 |
| 8 | a048ca84-9e48-4beb-a8d7-2a5b3db76da5 | 8 | STUDENT | 0 | 0 | 3 | 0 | 0 |
| 9 | 590c9b35-7d6c-4e40-914c-2059472950c0 | 9 | CREDIT_RATING | 0 | 0 | 3 | 0 | 0 |
| 10 | ed5c4958-1ab3-4acd-889e-b9034ca07f35 | 10 | GAIN RATIO | 1 | 0 | 1 | 0 | 0 |
| 11 | 9d25b7b9-7d87-4f8b-ab6a-89adf3df59be | 11 | INFORMATION GAIN | 1 | 0 | 2 | 0 | 0 |
| 12 | 36a992dd-c7d3-4c93-8574-e6ce14233088 | 12 | INFORMATION VALUE | 1 | 0 | 1 | 0 | 0 |

关系表（Relationships Table）

```python
relationships_df = pd.read_parquet("./openl/output/create_final_relationships.parquet")
relationships_df.head()
```

|  | id | human_readable_id | source | target | description | weight | combined_degree | text_unit_ids |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 0 | a0bbf6be-4a2c-4ad4-9e98-0e26fdf9d543 | 0 | ID3 | C4.5 | ID3 and C4.5 are both decision tree algorithms... | 18.0 | 20 | [4ba5acd798a419708cfe5cc86f1fcb6df907a20b1992a... |
| 1 | 80278b16-6586-491e-af78-f73b39394df9 | 1 | ID3 | CART | ID3 and CART are both decision tree algorithms... | 14.0 | 17 | [4ba5acd798a419708cfe5cc86f1fcb6df907a20b1992a... |
| 2 | 409e56e1-d48a-4384-ba51-070fd10c58c1 | 2 | ID3 | SKLEARN | ID3 is not directly supported by Sklearn, but ... | 2.0 | 12 | [4ba5acd798a419708cfe5cc86f1fcb6df907a20b1992a... |
| 3 | c54b57c8-c3bb-4aab-b8da-6a1e3ff95726 | 3 | ID3 | NUMPY | NumPy is used for entropy calculations in ID3 ... | 2.0 | 13 | [4ba5acd798a419708cfe5cc86f1fcb6df907a20b1992a... |
| 4 | 651dfca9-7a44-4d11-aed2-c00ce76f936d | 4 | C4.5 | CART | C4.5 and CART are both decision tree algorithm... | 17.0 | 19 | [4ba5acd798a419708cfe5cc86f1fcb6df907a20b1992a... |

社区表（Community Table）



```
community_df = pd.read_parquet("./openl/output/create_final_communities.parquet")
community_df.head()
```

|  | id | human_readable_id | community | parent | level | title | entity_ids | relationship_ids | text_unit_ids | period | size |
|---|---|---|---|---|---|---|---|---|---|---|---|
| 0 | b73809f5-60cc-4a86-a52b-77c8954460ea | 0 | 0 | -1 | 0 | Community 0 | [a0684f1a-0a87-45dc-b240-55b3d04f1961, 9afdfaa... | [2858cef2-8514-4bb9-a8fc-aaf4249f2a62, 2e7eee3... | [4ba5acd798a419708cfe5cc86f1fcb6df907a20b1992a... | 2025-01-08 | 7 |
| 1 | d3467630-82c0-48da-9ef2-9d2aa65fd77f | 1 | 1 | -1 | 0 | Community 1 | [f1535df1-c0e1-4bd4-9b18-49b05918a0aa, ed5c495... | [6304470a-dfd0-4a9e-9232-dc6446864dd4, 7640403... | [1141affdc56bf8f5092b950b57febdedb884673ec6b23... | 2025-01-08 | 4 |
| 2 | 2750b356-bf1c-4985-bd66-240627d193fb | 2 | 2 | -1 | 0 | Community 2 | [c6f4c391-4f5a-4d99-b8d5-6d4b24f43cc1, f7e0cfe... | [6307647a-5e6d-4a12-9081-ee72b9b7e3fb] | [4ba5acd798a419708cfe5cc86f1fcb6df907a20b1992a... | 2025-01-08 | 2 |

```
community_report_df = pd.read_parquet("./openl/output/create_final_community_reports.parquet")
community_report_df.head()
```

|  | id | human_readable_id | community | parent | level | title | summary | full_content | rank | rank_explanation | findings | full_content_json | period | size |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 0 | c81a85511b7246ac8f93a15783e26855 | 0 | 0 | -1 | 0 | Decision Tree Algorithms: ID3, C4.5, and CART | This community revolves around key decision tr... | # Decision Tree Algorithms: ID3, C4.5, and CAR... | 7.5 | The impact severity rating is high due to the ... | [{'explanation': 'ID3 is recognized as a class... | {\n "title": "Decision Tree Algorithms: ID3... | 2025-01-08 | 7 |
| 1 | 0c4efb0094a64a3abd1a184b1baab37e | 1 | 1 | -1 | 0 | C4.5 and Decision Tree Algorithms | This community is centered around the C4.5 dec... | # C4.5 and Decision Tree Algorithms\n\nThis co... | 7.5 | The impact severity rating is high due to the ... | [{'explanation': 'C4.5 represents a significan... | {\n "title": "C4.5 and Decision Tree Algori... | 2025-01-08 | 4 |
| 2 | 983b4cf9540e4ee7820611ff6f6d33ba | 2 | 2 | -1 | 0 | NumPy and ML_Basic_Function Community | This community is centered around the NumPy li... | # NumPy and ML_Basic_Function Community\n\nThi... | 7.5 | The impact severity rating is high due to the ... | [{'explanation': 'NumPy is identified as a Pyt... | {\n "title": "NumPy and ML_Basic_Function C... | 2025-01-08 | 2 |

是社区报告（Community Reports）每一行代表一个社区的摘要信息，包含了与该社区相关的标题、摘要、实体、关系等详细内容。这些信息是 GraphRAG 在索引阶段基于知识图谱生成的高层次语义总结，用于帮助回答关于数据集整体的问题（例如全局搜索）。

### 表格字段的含义

1. `id`
   + 每个社区的唯一标识符（UUID）。
   + 这是系统内部生成的，通常用于追踪社区记录。

1. `human_readable_id`
   + 更容易理解的人类可读 ID（数字化或短标识符）。
   + 通常用于直观地区分社区。

1. `community`
   + 社区的编号，代表社区的分类或标识。

1. `level`
   + 社区层级（`COMMUNITY_LEVEL`）。
   + 表示社区的聚类粒度。较低的值表示更抽象的层级，较高的值表示更具体的细分社区。

1. `title`
   + 社区的标题。
   + 对该社区的简要描述，用于表示该社区的主题核心。例如：
   + ID3 Decision Tree Algorithm Community
   + C4.5 and Sklearn Integration

1. `summary`
   + 社区的摘要。
   + 对该社区内容的进一步扩展描述，解释其主题或涵盖的主要内容。

1. `full_content`
   + 社区报告的完整内容。
   + 包括更详细的信息，可能包含标题、摘要、重要发现等。

1. `rank`
   + 社区的排名或评分。
   + 可能用来表示社区的重要性、影响力或相关性。

1. `rank_explanation`
   + 对排名的解释。
   + 例如，“影响严重性从中等到高”。

1. `findings`
   + 社区的主要发现（JSON 格式）。
   + 包含详细的解释或分析。例如：
   + ID3 算法是该社区的核心主题。
   + C4.5 是 ID3 的改进版本，结合了 Sklearn 的功能。

1. `full_content_json`
   + 社区完整内容的 JSON 表示。
   + 如果需要以结构化方式处理社区内容，这个字段是关键。

1. `period`
   + 报告的时间戳或周期。
   + 在你的示例中，是 2024-11-26，表示报告生成的日期。

1. `size`
   + 社区的大小。
   + 表示该社区包含的实体或内容的数量。例如：
   + 社区 0 包含 8 个内容。
   + 社区 1 包含 2 个内容。

### 结果解释

从你的结果中可以看出：

1. 社区 0：
   + 标题是“ID3 Decision Tree Algorithm Community”。
   + 主题围绕 ID3 决策树算法。
   + 内容提到了该算法的重要性，以及其对机器学习领域的影响。
   + 社区大小为 8。

1. 社区 1：
   + 标题是“C4.5 and Sklearn Integration”。
   + 主题是 C4.5 算法和 Sklearn 的集成。
   + 该社区比社区 0 更小（仅有 2 个内容）。
   + 排名评分为 4.5，影响程度适中。

### 用途

社区报告在查询阶段可以用于：

1. 全局搜索（Global Search）：
   + 回答关于数据集整体的问题，例如“这些文档的主要主题是什么？”

1. 快速理解社区：
   + 帮助用户快速了解数据集中不同部分的主题和相关信息。

1. 可视化与调试：
   + 将这些社区报告与知识图谱结合起来，可以直观地呈现社区的结构和语义关系。

### 三、GraphRAG 问答流程

> 除了使用 Python 代码环境进行问答外，若希望快速测试问答性能，也可以直接在命令行中，借助 `graphrag` 提供的脚本命令进行快速问答，例如：`graphrag query --root ./openl/input --method local --query "请帮我介绍下 ID3 算法"` 即可完成问答。

![](img/55177fd0eeb2e6393770a2764b414e0a.png)

#### 1. 导入核心关系组

```
import os

import pandas as pd
import tiktoken

from graphrag.query.context_builder.entity_extraction import EntityVectorStoreKey
from graphrag.query.indexer_adapters import (
    read_indexer_covariates,
    read_indexer_entities,
    read_indexer_relationships,
    read_indexer_reports,
    read_indexer_text_units,
)
from graphrag.query.llm.oai.chat_openai import ChatOpenAI
from graphrag.query.llm.oai.embedding import OpenAIEmbedding
from graphrag.query.llm.oai.typing import OpenaiApiType
from graphrag.query.question_gen.local_gen import LocalQuestionGen
from graphrag.query.structured_search.local_search.mixed_context import (
    LocalSearchMixedContext,
)
from graphrag.query.structured_search.local_search.search import LocalSearch
from graphrag.vector_stores.lancedb import LanceDBVectorStore
```

##### 与索引器相关的模块

+ `read_indexer_*`

从不同的索引文件中读取数据（例如实体、关系、摘要等）。这些模块负责加载索引器生成的数据到 Python 中，供后续搜索或分析使用：

+ `read_indexer_covariates`: 读取与数据协变量（附加属性）相关的信息。

+ `read_indexer_entities`: 读取从数据中提取的实体。

+ `read_indexer_relationships`: 读取实体间的关系。

+ `read_indexer_reports`: 读取生成的社区报告摘要。

+ `read_indexer_text_units`: 读取切分后的文本单元（`TextUnits`）。

+ `store_entity_semantic_embeddings`

将实体的语义嵌入存储到向量数据库中。GraphRAG 用嵌入向量来表示实体间的语义关系。

##### 与 LLM（大型语言模型）相关的模块

+ `ChatOpenAI`

一个封装了 OpenAI 聊天模型（如 GPT 系列）的接口，允许你通过编程与 OpenAI 模型交互（如提出问题、获取回答）。

+ `OpenAIEmbedding`

用于生成文本的嵌入向量的模块，通过调用 OpenAI 的嵌入 API，将文本转换为语义向量表示。

+ `OpenaiApiType`

定义 OpenAI API 的具体类型，可能包括“聊天模型”、“嵌入模型”等。

##### 与本地搜索相关的模块

+ `LocalSearch`

GraphRAG 的本地搜索引擎，专注于通过上下文和邻近信息回答关于特定实体的问题。

+ `LocalSearchMixedContext`

允许混合使用不同上下文数据（例如实体及其邻居的关系）来丰富本地搜索的结果。

+ `LocalQuestionGen`

用于在本地搜索中生成问题的模块，帮助生成更相关的问题。

##### 与向量存储相关的模块

+ `LanceDBVectorStore`

GraphRAG 使用的向量存储解决方案之一，支持存储和检索语义嵌入向量。可以快速高效地查找与查询向量最相似的嵌入。

```
INPUT_DIR = "./openl/output"
LANCEDB_URI = f"{INPUT_DIR}/lancedb"

COMMUNITY_REPORT_TABLE = "create_final_community_reports"
ENTITY_TABLE = "create_final_nodes"
ENTITY_EMBEDDING_TABLE = "create_final_entities"
RELATIONSHIP_TABLE = "create_final_relationships"
TEXT_UNIT_TABLE = "create_final_text_units"
COMMUNITY_LEVEL = 2
```

#### 定义输入目录与文件路径

```
INPUT_DIR = "./openl/output"
LANCEDB_URI = f"{INPUT_DIR}/lancedb"
```

+ `INPUT_DIR`: 索引器输出文件的存放目录。在这里，索引器输出的路径为 `./openl/output`。

+ `LANCEDB_URI`: 存放向量存储（LanceDB 数据库）的目录路径。在 GraphRAG 中，实体嵌入向量通常被存储在 LanceDB 中，以便后续搜索时高效检索。

#### 定义数据表文件名

```
COMMUNITY_REPORT_TABLE = "create_final_community_reports"
ENTITY_TABLE = "create_final_nodes"
ENTITY_EMBEDDING_TABLE = "create_final_entities"
RELATIONSHIP_TABLE = "create_final_relationships"
COVARIATE_TABLE = "create_final_covariates"
TEXT_UNIT_TABLE = "create_final_text_units"
COMMUNITY_LEVEL = 2
```

+ `COMMUNITY_REPORT_TABLE`: 存储社区报告的文件名。社区报告是基于知识图谱生成的每个社区的摘要。

+ `ENTITY_TABLE`: 存储实体的文件名（知识图谱中的节点）。每个节点代表一个实体，例如人、地点或组织。

+ `ENTITY_EMBEDDING_TABLE`: 存储实体的嵌入向量。嵌入表示实体的语义信息，用于计算实体之间的相似性。

+ `RELATIONSHIP_TABLE`: 存储实体间关系的文件名（知识图谱中的边）。关系表示两个实体之间的交互或连接。

+ `TEXT_UNIT_TABLE`: 存储文本单元的文件名（原始文本数据被切分为的小块）。

+ `COMMUNITY_LEVEL`: 指定加载的社区层级。社区层级表示知识图谱的聚类粒度（层次化结构中的第 2 层）。



```
# read nodes table to get community and degree data
entity_df = pd.read_parquet(f"{INPUT_DIR}/{ENTITY_TABLE}.parquet")
entity_embedding_df = pd.read_parquet(f"{INPUT_DIR}/{ENTITY_EMBEDDING_TABLE}.parquet")

entities = read_indexer_entities(entity_df, entity_embedding_df, COMMUNITY_LEVEL)

# load description embeddings to an in-memory lancedb vectorstore
# to connect to a remote db, specify url and port values.
description_embedding_store = LanceDBVectorStore(
    collection_name="default-entity-description",
)
description_embedding_store.connect(db_uri=LANCEDB_URI)

print(f"Entity count: {len(entity_df)}")
entity_df.head()
```

```
Entity count: 13
```

|  | id | human_readable_id | title | community | level | degree | x | y |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 0 | 35bbb43d-b9f4-4a4d-bd53-7059efc44056 | 0 | ID3 | 0 | 0 | 9 | 0 | 0 |
| 1 | f1535df1-c0e1-4bd4-9b18-49b05918a0aa | 1 | C4.5 | 1 | 0 | 11 | 0 | 0 |
| 2 | 9afdfaaf-37d8-4990-9406-a62f37c2d64e | 2 | CART | 0 | 0 | 8 | 0 | 0 |
| 3 | b3651a61-5626-4162-9f1e-432e6d0e9fe5 | 3 | SKLEARN | 0 | 0 | 3 | 0 | 0 |
| 4 | c6f4c391-4f5a-4d99-b8d5-6d4b24f43cc1 | 4 | NUMPY | 2 | 0 | 4 | 0 | 0 |

```
relationship_df = pd.read_parquet(f"{INPUT_DIR}/{RELATIONSHIP_TABLE}.parquet")
relationships = read_indexer_relationships(relationship_df)

print(f"Relationship count: {len(relationship_df)}")
relationship_df.head()
```

```
Relationship count: 29
```

|  | id | human_readable_id | source | target | description | weight | combined_degree | text_unit_ids |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 0 | a0bbf6be-4a2c-4ad4-9e98-0e26fdf9d543 | 0 | ID3 | C4.5 | ID3 and C4.5 are both decision tree algorithms... | 18.0 | 20 | [4ba5acd798a419708cfe5cc86f1fcb6df907a20b1992a... |
| 1 | 80278b16-6586-491e-af78-f73b39394df9 | 1 | ID3 | CART | ID3 and CART are both decision tree algorithms... | 14.0 | 17 | [4ba5acd798a419708cfe5cc86f1fcb6df907a20b1992a... |
| 2 | 409e56e1-d48a-4384-ba51-070fd10c58c1 | 2 | ID3 | SKLEARN | ID3 is not directly supported by Sklearn, but ... | 2.0 | 12 | [4ba5acd798a419708cfe5cc86f1fcb6df907a20b1992a... |
| 3 | c54b57c8-c3bb-4aab-b8da-6a1e3ff95726 | 3 | ID3 | NUMPY | NumPy is used for entropy calculations in ID3 ... | 2.0 | 13 | [4ba5acd798a419708cfe5cc86f1fcb6df907a20b1992a... |
| 4 | 651dfca9-7a44-4d11-aed2-c00ce76f936d | 4 | C4.5 | CART | C4.5 and CART are both decision tree algorithm... | 17.0 | 19 | [4ba5acd798a419708cfe5cc86f1fcb6df907a20b1992a... |

```
report_df = pd.read_parquet(f"{INPUT_DIR}/{COMMUNITY_REPORT_TABLE}.parquet")
reports = read_indexer_reports(report_df, entity_df, COMMUNITY_LEVEL)

print(f"Report records: {len(report_df)}")
report_df.head()
```

```
Report records: 3
```

|  | id | human_readable_id | community | parent | level | title | summary | full_content | rank | rank_explanation | findings | full_content_json | period | size |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 0 | c81a85511b7246ac8f93a15783e26855 | 0 | 0 | -1 | 0 | Decision Tree Algorithms: ID3, C4.5, and CART | This community revolves around key decision tr... | # Decision Tree Algorithms: ID3, C4.5, and CAR... | 7.5 | The impact severity rating is high due to the ... | [{'explanation': 'ID3 is recognized as a class... | {\n "title": "Decision Tree Algorithms: ID3... | 2025-01-08 | 7 |
| 1 | 0c4efb0094a64a3abd1a184b1baab37e | 1 | 1 | -1 | 0 | C4.5 and Decision Tree Algorithms | This community is centered around the C4.5 dec... | # C4.5 and Decision Tree Algorithms\n\nThis co... | 7.5 | The impact severity rating is high due to the ... | [{'explanation': 'C4.5 represents a significan... | {\n "title": "C4.5 and Decision Tree Algori... | 2025-01-08 | 4 |
| 2 | 983b4cf9540e4ee7820611ff6f6d33ba | 2 | 2 | -1 | 0 | NumPy and ML_Basic_Function Community | This community is centered around the NumPy li... | # NumPy and ML_Basic_Function Community\n\nThi... | 7.5 | The impact severity rating is high due to the ... | [{'explanation': 'NumPy is identified as a Pyt... | {\n "title": "NumPy and ML_Basic_Function C... | 2025-01-08 | 2 |

```
text_unit_df = pd.read_parquet(f"{INPUT_DIR}/{TEXT_UNIT_TABLE}.parquet")
text_units = read_indexer_text_units(text_unit_df)

print(f"Text unit records: {len(text_unit_df)}")
text_unit_df.head()
```

```
Text unit records: 4
```

|  | id | human_readable_id | text | n_tokens | document_ids | entity_ids | relationship_ids |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 0 | 4ba5acd798a419708cfe5cc86f1fcb6df907a20b1992ad... | 1 | Lesson 8.3 ID3、C4.5决策树的建模流程\nID3和C4.5作为的经典决策树算... | 1200 | [2ef33abc7acb307f736e46c7f20fe87568b4e9bd6311c... | [35bbb43d-b9f4-4a4d-bd53-7059efc44056, f1535df... | [a0bbf6be-4a2c-4ad4-9e98-0e26fdf9d543, 80278b1... |
| 1 | 7a8436d7970da4b0e5c837b0a3eaaa196e6e2284c7c754... | 2 | 8961919\n然后即可算出按照如此规则进行数据集划分，最终能够减少的不纯度数值：\n# ... | 1200 | [2ef33abc7acb307f736e46c7f20fe87568b4e9bd6311c... | [35bbb43d-b9f4-4a4d-bd53-7059efc44056, f1535df... | [a0bbf6be-4a2c-4ad4-9e98-0e26fdf9d543, 80278b1... |
| 2 | 1141affdc56bf8f5092b950b57febdedb884673ec6b23d... | 3 | 4.5决策树的基本建模流程\n作为ID3的改进版算法，C4.5在ID3的基础上进行了三个方面... | 1200 | [2ef33abc7acb307f736e46c7f20fe87568b4e9bd6311c... | [35bbb43d-b9f4-4a4d-bd53-7059efc44056, f1535df... | [651dfca9-7a44-4d11-aed2-c00ce76f936d, 3ad66ad... |
| 3 | b5df95b6ec7e3aaea3f67db4914e1b2e874f3ac77fc3c8... | 4 | 集划分。\n\nC4.5的连续变量处理方法\nC4.5允许带入连续变量进行建模，并且围绕连续... | 558 | [2ef33abc7acb307f736e46c7f20fe87568b4e9bd6311c... | [35bbb43d-b9f4-4a4d-bd53-7059efc44056, f1535df... | [651dfca9-7a44-4d11-aed2-c00ce76f936d, 9ddcf83... |

## 2. 设置模型参数

设置模型参数

```
sd_api_key = "YOUR_DeepSeek_API_KEY"
openai_api_key = "YOUR_DeepSeek_API_KEY"
```

```
llm_model = "deepseek-chat"
embedding_model = "text-embedding-3-large"
```

```
llm_api_base = "https://api.deepseek.com"
embedding_model_api_base = "https://ai.devtool.tech/proxy/v1"
```

```
llm = ChatOpenAI(
    api_key=sd_api_key,
    model=llm_model,
    api_base=llm_api_base,
    api_type=OpenaiApiType.OpenAI,
    max_retries=20,
)

token_encoder = tiktoken.get_encoding("cl100k_base")

text_embedder = OpenAIEmbedding(
    api_key=openai_api_key,
    api_base=embedding_model_api_base,
    api_type=OpenaiApiType.OpenAI,
    model=embedding_model,
    deployment_name=embedding_model,
    max_retries=20,
)
```

### 初始化 Chat 模型

`ChatOpenAI`：

- 用于与 OpenAI 或 Azure OpenAI 的聊天模型（如 GPT 系列）交互。

**参数说明**：

- `api_key`：访问 OpenAI 或 Azure OpenAI 的密钥。
- `model`：指定 LLM 模型名称（如 gpt-4）。
- `api_base`：API 的基础路径，可以填写反向代理地址
- `api_type`：API 类型，可以是：
  - `OpenaiApiType.OpenAI`：OpenAI 直连服务。
  - `OpenaiApiType.AzureOpenAI`：Azure OpenAI 服务。
- `max_retries`：最大重试次数，以处理网络或服务中断。

### 初始化文本分词器

`tiktoken`：

- 用于对输入文本进行分词或令牌化操作。
- `cl100k_base`：一种令牌编码方式，适用于 GPT 系列模型（如 gpt-3.5-turbo 和 gpt-4）。

**用途**：

- 计算输入文本的令牌数量，确保不超过模型的上下文窗口限制。

### 初始化嵌入生成器

`OpenAIEmbedding`：

- 用于生成文本的嵌入向量。

**参数说明**：

- `api_key`：用于访问 OpenAI 嵌入 API 的密钥。
- `api_base`：API 的基础路径，可以填写反向代理地址
- `api_type`：API 类型，与 LLM 相同。
- `model`：指定嵌入模型名称，例如 text-embedding-ada-002。
- `deployment_name`：Azure 模型部署名称（如果使用 Azure）。
- `max_retries`：最大重试次数。

## 3. 构建 LocalSearch（本地搜索）搜索引擎并进行问答

创建 LocalSearch 上下文构建器

```
context_builder = LocalSearchMixedContext(
    community_reports=reports,
    text_units=text_units,
    entities=entities,
    relationships=relationships,
    covariates=None,
    entity_text_embeddings=description_embedding_store,
    embedding_vectorstore_key=EntityVectorStoreKey.ID,
    text_embedder=text_embedder,
    token_encoder=token_encoder,
)
```



本段代码的主要功能是创建本地搜索的上下文构建器（`Context Builder`）。上下文构建器的作用是整合所有加载的数据（如社区报告、文本单元、实体、关系等），为后续的本地搜索提供语义丰富的上下文。

## 参数解析（上下文构建器）

### `community_reports`

- 传入之前加载的社区报告数据（`reports`）。
- 用于补充社区级别的背景信息，在本地搜索中提供更多上下文。

### `text_units`

- 传入已加载的文本单元数据（`text_units`）。
- 提供来自原始文档的小块文本，用于回答具体问题或补充答案的上下文。

### `entities`

- 传入知识图谱中的实体（`entities`）。
- 提供实体的详细信息和属性，帮助本地搜索定位特定实体。

### `relationships`

- 传入知识图谱中的关系（`relationships`）。
- 用于描述实体之间的交互和连接，有助于构建更复杂的答案。

### `covariates`

- 传入协变量数据（`covariates`），如果索引阶段未启用协变量，设置为 `None`。
- **用途**：
- 提供附加声明性元数据或上下文信息。
- 如果协变量未启用，仍可正常运行本地搜索，但答案可能少一些附加的语义信息。

### `entity_text_embeddings`

- 传入实体嵌入的存储对象（`description_embedding_store`）。
- **用途**：
- 将实体的文本嵌入存储在向量数据库中。
- 用于通过语义搜索快速找到相关实体。

### `embedding_vectorstore_key`

- 设置嵌入向量存储的键类型。
- `EntityVectorStoreKey.ID`：
- 如果向量存储中的实体是按 ID 存储的，使用此键。
- `EntityVectorStoreKey.TITLE`：
- 如果向量存储按实体标题存储，则改为设置为 `TITLE`。

### `text_embedder`

- 传入嵌入生成器对象（`text_embedder`）。
- 用于生成文本或问题的语义嵌入，支持与存储的实体嵌入进行相似度比较。

### `token_encoder`

- 传入分词器（`token_encoder`）。
- **用途**：
- 确保生成的上下文不会超过 LLM 的上下文窗口限制。
- 有助于在构建查询时分配合理的上下文。

## 上下文构建器的作用

上下文构建器是本地搜索的核心组件，它负责将加载的数据整合成结构化的上下文，包括：

- 结合社区级摘要（`Community Reports`）：
- 提供更高层次的背景信息，用于回答主题性问题。
- 整合文本单元（`Text Units`）：
- 使用来自原始文档的文本片段，为问题构建直接的上下文。
- 集成知识图谱的实体和关系：
- 为本地搜索中的实体相关问题提供详细的语义信息。
- 利用嵌入和分词：
- 嵌入存储和分词器确保上下文生成语义准确，符合 LLM 的上下文限制。

## 创建本地搜索引擎参数组

```
local_context_params = {
    "text_unit_prop": 0.5,
    "community_prop": 0.1,
    "conversation_history_max_turns": 5,
    "conversation_history_user_turns_only": True,
    "top_k_mapped_entities": 10,
    "top_k_relationships": 10,
    "include_entity_rank": True,
    "include_relationship_weight": True,
    "include_community_rank": True,
    "return_candidate_context": True,
    "embedding_vectorstore_key": EntityVectorStoreKey.ID,
    "max_tokens": 12_000,
}

llm_params = {
    "max_tokens": 2_000,
    "temperature": 0.0,
}
```

## 参数解析（`local_context_params`）

### `text_unit_prop`

- 上下文窗口中分配给文本单元（`Text Units`）的比例（50%）。
- 控制原始文档中的文本内容在上下文中的权重。

### `community_prop`

- 上下文窗口中分配给社区报告（`Community Reports`）的比例（10%）。
- 用于为回答提供更高层次的社区背景。

### `conversation_history_max_turns`

- 上下文中包含的最大对话轮数（5轮）。
- 控制是否包括之前的对话历史，以及保留的对话轮数。

### `conversation_history_user_turns_only`

- 如果为 `True`，则只包括用户的提问，而忽略模型的回答。

### `top_k_mapped_entities`

- 从嵌入向量存储中检索的相关实体的数量（10个）。

### `top_k_relationships`

- 检索到上下文窗口中的最大关系数（10个）。

### `include_entity_rank`

- 是否在上下文中包含实体的排名（默认是根据实体的节点度数）。

### `include_relationship_weight`

- 是否在上下文中包含关系权重（例如，表示关系的重要性）。

### `include_community_rank`

- 是否在上下文中包含社区排名。这里设置为 `False`。

### `return_candidate_context`

- 如果为 `True`，返回一个数据帧集合，包含所有可能相关的实体、关系和协变量记录。这些记录的上下文中会有一列 `in_context` 表示是否被纳入上下文窗口。

### `embedding_vectorstore_key`

- 嵌入向量存储的键类型。
- 如果向量存储的标识符是实体标题而非 ID，则设置为 `EntityVectorStoreKey.TITLE`。

### `max_tokens`

- 上下文窗口允许的最大令牌数（12,000个）。
- 需要根据所用 LLM 的令牌限制调整（例如，`GPT-4` 支持 8,192 或 32,768）。

## 创建本地搜索引擎

```
search_engine = LocalSearch(
    llm=llm,
    context_builder=context_builder,
    token_encoder=token_encoder,
    llm_params=llm_params,
    context_builder_params=local_context_params,
    response_type="multiple paragraphs",
)
```

## 参数解析（`LocalSearch`）

### `llm`

- 已初始化的 LLM 对象（`ChatOpenAI`），用于生成答案。

### `context_builder`

- 上一段代码中创建的上下文构建器（`LocalSearchMixedContext`），负责生成语义丰富的上下文。

### `token_encoder`

- 分词器，用于计算上下文和生成回答的令牌数，确保不会超过 LLM 的限制。

### `llm_params`

- 用于 LLM 的参数，包括生成答案时的令牌数和温度。

### `context_builder_params`

- 上下文构建器的配置，控制上下文中每种数据的比例和数量限制。

### `response_type`

- 指定回答的格式。
- 此处为 `"multiple paragraphs"`，表示生成的回答以多段文字形式呈现。
- 可以根据需求设置为 `"single paragraph"` 或 `"prioritized list"` 等。

## 基于本地搜索的问答流程

```
result = await search_engine.asearch("请帮我介绍下ID3决策树算法")

display(Markdown(result.response))
```

## ID3决策树算法概述

ID3（`Iterative Dichotomiser 3`）是一种经典的决策树算法，主要用于分类任务。它通过信息熵（`Information Entropy`）来评估数据集划分的纯度，并选择信息增益（`Information Gain`）最大的属性进行节点分裂。ID3 算法是决策树算法家族中的基础算法，后续的 `C4.5` 和 `CART` 算法都在其基础上进行了改进和扩展 [Data: Entities (0); Reports (0); Relationships (0, 23)]。

### 核心原理

ID3 的核心思想是通过递归地选择信息增益最大的属性来构建决策树。具体步骤如下：

1. 信息熵计算：ID3 使用信息熵来衡量数据集的纯度。信息熵越低，数据集的纯度越高。对于一个数据集，信息熵的计算公式为：

$$
\text{Entropy}(S) = -\sum_{i=1}^{n} p_i \log_2(p_i)
$$

其中，$p_i$ 是数据集中第 $i$ 类样本的比例 [Data: Sources (0, 1)]。

1. 信息增益计算：信息增益用于衡量某个属性对数据集划分的贡献。信息增益越大，说明该属性对分类的贡献越大。信息增益的计算公式为：

$$
\text{Gain}(S, A) = \text{Entropy}(S) - \sum_{v \in \text{Values}(A)} \frac{|S_v|}{|S|} \text{Entropy}(S_v)
$$

其中，$S_v$ 是属性 $A$ 取值为 $v$ 时的子集 [Data: Sources (1); Entities (11)]。

1. 节点分裂：ID3 选择信息增益最大的属性作为当前节点的分裂属性，并根据该属性的不同取值将数据集划分为多个子集。这一过程递归进行，直到所有子集的纯度达到要求或没有更多属性可用 [Data: Sources (0, 1)]。

### 数据集划分与离散变量处理

ID3 只能处理离散变量，无法直接处理连续变量。如果数据集中包含连续变量，需要先对其进行离散化处理（如分箱）。ID3 通过一次展开一列的方式对数据集进行划分，这与 `CART` 算法中寻找切分点的方式不同。例如，在某个数据集中，ID3 会根据 `AGE` 列的不同取值将数据集划分为多个子集，然后计算每个子集的信息熵和信息增益 [Data: Sources (0, 1); Entities (6, 7, 8, 9); Relationships (10, 11, 12, 13)]。

### ID3 的局限性

尽管 ID3 在决策树算法的发展中具有重要地位，但它也存在一些局限性：

1. 无法处理连续变量：ID3 只能处理离散变量，连续变量需要先进行离散化处理 [Data: Entities (0); Reports (0)]。
1. 倾向于选择多分类属性：ID3 在选择分裂属性时，倾向于选择分类水平较多的属性，这可能导致模型过拟合 [Data: Sources (1); Entities (0)]。
1. 缺乏剪枝机制：ID3 没有内置的剪枝机制来防止过拟合，这使得模型在训练数据上表现良好，但在测试数据上可能表现不佳 [Data: Sources (1); Entities (0)]。
1. 不支持回归任务：ID3 仅适用于分类任务，无法处理回归问题 [Data: Entities (0)]。

### ID3 与 C4.5、CART 的关系

ID3 是决策树算法家族中的基础算法，后续的 `C4.5` 和 `CART` 算法在其基础上进行了改进：

- `C4.5`：`C4.5` 在 ID3 的基础上引入了连续变量处理、剪枝机制和信息增益比（`Gain Ratio`）等改进，解决了 ID3 的许多局限性 [Data: Entities (1); Relationships (0, 23)]。
- `CART`：`CART`（`Classification and Regression Trees`）进一步扩展了决策树的能力，支持分类和回归任务，并采用二分法进行节点分裂 [Data: Entities (2); Relationships (1, 4)]。

### 总结

ID3 作为决策树算法的奠基者，通过信息熵和信息增益构建分类模型，尽管存在一些局限性，但其核心思想为后续算法的发展提供了重要基础。`C4.5` 和 `CART` 等算法在 ID3 的基础上进行了优化，进一步提升了决策树模型的性能和适用性 [Data: Entities (0, 1, 2); Reports (0, 1); Relationships (0, 1, 4, 14)]。

## 4.构建 GlobalSearch（全局搜索）搜索引擎并进行问答

### 导入知识图谱相关内容

```
import pandas as pd
import tiktoken

from graphrag.query.indexer_adapters import (
    read_indexer_communities,
    read_indexer_entities,
    read_indexer_reports,
)
from graphrag.query.llm.oai.chat_openai import ChatOpenAI
from graphrag.query.llm.oai.typing import OpenaiApiType
from graphrag.query.structured_search.global_search.community_context import (
    GlobalCommunityContext,
)
from graphrag.query.structured_search.global_search.search import GlobalSearch
```



# 全局搜索配置与 ID3 算法解析

## 定义表名常量

```
COMMUNITY_TABLE = "create_final_communities";
COMMUNITY_REPORT_TABLE = "create_final_community_reports";
ENTITY_TABLE = "create_final_nodes";
ENTITY_EMBEDDING_TABLE = "create_final_entities";
```

## 读取数据并构建索引对象

```
community_df = pd.read_parquet(f"{INPUT_DIR}/{COMMUNITY_TABLE}.parquet")
entity_df = pd.read_parquet(f"{INPUT_DIR}/{ENTITY_TABLE}.parquet")
report_df = pd.read_parquet(f"{INPUT_DIR}/{COMMUNITY_REPORT_TABLE}.parquet")
entity_embedding_df = pd.read_parquet(f"{INPUT_DIR}/{ENTITY_EMBEDDING_TABLE}.parquet")

communities = read_indexer_communities(community_df, entity_df, report_df)
reports = read_indexer_reports(report_df, entity_df, COMMUNITY_LEVEL)
entities = read_indexer_entities(entity_df, entity_embedding_df, COMMUNITY_LEVEL)
```

## 创建 Global Search 上下文构建器

```
context_builder = GlobalCommunityContext(
    community_reports=reports,
    communities=communities,
    entities=entities,
    token_encoder=token_encoder,
)
```

代码解释如下：

### `GlobalCommunityContext`

这是一个为 `Global Search` 模式创建的上下文构建器。它负责从不同的数据源（如社区报告、实体、社区信息等）中提取相关数据，并构建一个用于查询的大范围上下文。

这个构建器帮助为整个文档集合（而不是某个特定实体）构建一个全局的知识背景，以便模型能够对广泛的问题做出响应。

### `community_reports=reports`

这里传入的是之前从文件中读取的 `reports` 数据（也就是 `COMMUNITY_REPORT_TABLE` 表格的数据）。

这些报告包含了关于不同社区的详细信息，如主题、摘要、影响力、发现等。

在全局搜索中，这些社区报告有助于构建整个数据集的高层次概览。

### `communities=communities`

`communities` 数据包含社区的结构和信息。在图谱中，社区可能代表了不同的主题、领域或相关性较高的实体群体。

这部分数据通常用于在全局搜索时进行分组和排名，比如通过社群的重要性或与查询的相关性来排序。

### `entities=entities`

`entities` 是从之前的索引步骤中提取的实体数据（来自 `ENTITY_TABLE` 表）。

这些实体（如人物、地点、事件等）可以用来扩展全局搜索的范围。它们提供了具体的名词、对象和概念，有助于为全局查询提供上下文。

注意：如果不希望在全局搜索中使用社区权重来影响排名（例如，不考虑某个社区对某个实体的影响），可以将 `entities` 参数设为 `None`。

### `token_encoder=token_encoder`

`token_encoder` 是用于对文本进行编码的工具。它将文本转化为可以输入到模型中的 token 序列。

在之前的代码中，我们已经看到 `token_encoder` 是通过 `tiktoken.get_encoding("cl100k_base")` 获取的，它的作用是把文本切分为一定的 token 数量，以便在模型中使用。

## 配置全局搜索参数

```
context_builder_params = {
    "use_community_summary": False,
    "shuffle_data": True,
    "include_community_rank": True,
    "min_community_rank": 0,
    "community_rank_name": "rank",
    "include_community_weight": True,
    "community_weight_name": "occurrence weight",
    "normalize_community_weight": True,
    "max_tokens": 12_000,
    "context_name": "Reports",
}

map_llm_params = {
    "max_tokens": 1000,
    "temperature": 0.0,
    "response_format": {"type": "json_object"},
}

reduce_llm_params = {
    "max_tokens": 2000,
    "temperature": 0.0,
}
```

## 构建全局搜索引擎

```
search_engine = GlobalSearch(
    llm=llm,
    context_builder=context_builder,
    token_encoder=token_encoder,
    max_data_tokens=12_000,
    map_llm_params=map_llm_params,
    reduce_llm_params=reduce_llm_params,
    allow_general_knowledge=False,
    json_mode=True,
    context_builder_params=context_builder_params,
    concurrent_coroutines=32,
    response_type="multiple paragraphs",
)
```

这里创建了一个 全局搜索（`Global Search`）引擎，利用前面配置的上下文构建器（`context_builder`）、语言模型（`llm`）、以及其它相关的参数进行搜索。对应参数详解如下：

### `llm`

这个参数传入的是已经配置好的语言模型（LLM），即你之前定义的 `ChatOpenAI` 模型。全局搜索引擎将使用这个模型来生成回答。

### `context_builder`

上下文构建器，之前我们已经讨论了 `GlobalCommunityContext`，它负责根据社区报告、社区等信息来构建查询的上下文。

### `token_encoder`

`token_encoder` 是用来处理文本的编码工具，通常是一个模型专用的 tokenizer。在这里，我们使用了 `tiktoken.get_encoding("cl100k_base")`，它是 OpenAI 模型的一个编码器，用来将文本转化为 token 格式。

### `max_data_tokens`

最大数据 token 数量。它控制了模型可以处理的最大上下文大小。你可以根据实际使用的模型的最大 token 限制来设置（例如，如果模型最大 token 限制是 8K，则可以设置为 5000，留一些余量）。这个设置控制了全局搜索时在上下文窗口内所使用的最大 token 数。

### `map_llm_params` 和 `reduce_llm_params`

这两个参数分别传入了映射和归约阶段的 LLM 配置（之前我们已经详细分析过这些参数）。这些参数会影响 LLM 在不同阶段生成内容的方式（例如，`max_tokens` 和 `temperature` 等）。

### `allow_general_knowledge`

这个参数控制是否允许模型在回答中加入通用知识。如果设置为 `True`，LLM 会尝试将外部知识加入到查询的结果中。这对于需要广泛知识背景的任务可能有帮助，但也有可能导致模型生成 虚假信息（hallucinations）。为了避免这个问题，默认值设置为 `False`。

### `json_mode`

这个参数控制结果的格式。如果设置为 `True`，LLM 会生成结构化的 JSON 格式输出。如果设置为 `False`，则返回的是更自由形式的文本回答。对于结构化数据的处理，通常使用 JSON 格式。

### `context_builder_params`

这是传入给上下文构建器的参数，用来进一步配置如何构建查询上下文（例如是否使用社区简要摘要、是否包含社区排名等）。我们在之前的代码分析中已经详细介绍了这些参数。

### `concurrent_coroutines`

这个参数控制并发协程的数量。全局搜索引擎支持并发处理多个查询，如果你需要同时处理多个请求，可以增加这个值。比如设置为 32，意味着最多可以同时处理 32 个查询。

### `response_type`

这个参数定义了模型生成的响应的类型和格式。它是一个自由文本的描述，指明返回的内容应该是什么样的格式。在这里，`"multiple paragraphs"` 表示模型会生成多段文字的回答，适合长篇的说明或报告。

## 执行全局搜索

```
result = await search_engine.asearch("请帮我介绍下ID3决策树算法")
```

```
display(Markdown(result.response))
```

## ID3 决策树算法介绍

ID3（Iterative Dichotomiser 3）是一种经典的决策树算法，主要用于分类任务。它通过信息熵（information entropy）来评估分裂的纯度，并专注于处理离散变量。信息熵的使用使得 ID3 能够有效地根据最具信息量的属性对数据集进行划分，从而构建决策树 [Data: Reports (0)]。

### 核心原理

ID3 的核心在于其使用信息熵来衡量数据集的混乱程度，并选择能够最大程度减少熵的属性进行分裂。这种方法确保了决策树在每一步都能选择最优的分裂点，从而提高分类的准确性。然而，ID3 仅适用于离散变量，无法直接处理连续变量 [Data: Reports (0)]。

### 局限性

尽管 ID3 在决策树算法的发展中起到了奠基作用，但它也存在一些明显的局限性。首先，ID3 容易过拟合，尤其是在处理复杂数据集时。其次，它无法直接处理连续变量，这限制了其在实际应用中的灵活性 [Data: Reports (0)]。

### 后续发展

ID3 的局限性促使了后续算法的改进。例如，C4.5 算法在 ID3 的基础上引入了对连续变量的处理、剪枝技术以及改进的属性选择标准，使其成为一个更强大且通用的算法。CART（Classification and Regression Trees）也是 ID3 的重要后继者，进一步扩展了决策树的应用范围 [Data: Reports (0)]。

### 总结

ID3 作为决策树算法的先驱，为后续算法的发展奠定了重要基础。尽管它存在一些局限性，但其核心思想——通过信息熵进行分裂——仍然是现代决策树算法的核心组成部分。通过 C4.5 和 CART 等算法的改进，ID3 的局限性得到了有效解决，进一步推动了决策树技术在机器学习领域的广泛应用 [Data: Reports (0)]。

```
result = await search_engine.asearch("请帮我介绍下ID3决策树算法")
```

```
display(Markdown(result.response))
```

ID3（Iterative Dichotomiser 3）是一种经典的决策树算法，主要用于分类任务。它通过信息熵（information entropy）来评估数据分割的纯度，并专注于处理离散变量。信息熵的使用是 ID3 算法构建决策树的核心方法，通过计算信息增益来选择最佳的分割属性，从而逐步构建决策树 [Data: Reports (0)]。

### 核心原理

ID3 算法的核心在于利用信息熵来衡量数据集的混乱程度。信息熵越低，数据集的纯度越高。算法通过计算每个属性的信息增益，选择能够最大程度减少不确定性的属性作为分割节点。这一过程递归进行，直到所有数据被正确分类或达到停止条件 [Data: Reports (0)]。

### 优点与局限性

ID3 算法在决策树的发展中具有重要的历史意义，但其也存在一些局限性。首先，ID3 只能处理离散变量，无法直接处理连续变量。其次，ID3 容易出现过拟合问题，尤其是在数据集较小或噪声较多的情况下。这些局限性促使了后续算法的改进 [Data: Reports (0, 1)]。

### 后续发展

ID3 的局限性催生了更先进的决策树算法，如 C4.5 和 CART。这些算法在 ID3 的基础上进行了改进，例如引入了连续变量的处理能力、剪枝技术（pruning）以及更复杂的属性选择标准。C4.5 和 CART 的改进不仅克服了 ID3 的不足，还进一步提升了决策树的性能和适用性 [Data: Reports (0, 1)]。

### 总结

ID3 算法作为决策树领域的开创性工作，奠定了后续算法发展的基础。尽管它存在一些局限性，但其核心思想——利用信息熵进行数据分割——仍然是决策树算法的重要组成部分。通过了解 ID3，我们可以更好地理解现代决策树算法的演进过程及其背后的原理 [Data: Reports (0, 1)]。

## 查看全局搜索调用的社区报告

```
result.context_data["reports"]
```

|  | id | title | occurrence weight | content | rank |
| --- | --- | --- | --- | --- | --- |
| 0 | 1 | C4.5 and Decision Tree Algorithms | 1.00 | # C4.5 and Decision Tree Algorithms\n\nThis co... | 7.5 |
| 1 | 0 | Decision Tree Algorithms: ID3, C4.5, and CART | 1.00 | # Decision Tree Algorithms: ID3, C4.5, and CAR... | 7.5 |
| 2 | 2 | NumPy and ML_Basic_Function Community | 0.25 | # NumPy and ML_Basic_Function Community\n\nThi... | 7.5 |

```
report_df
```

|  | id | human_readable_id | community | parent | level | title | summary | full_content | rank | rank_explanation | findings | full_content_json | period | size |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 0 | c81a85511b7246ac8f93a15783e26855 | 0 | 0 | -1 | 0 | Decision Tree Algorithms: ID3, C4.5, and CART | This community revolves around key decision tr... | # Decision Tree Algorithms: ID3, C4.5, and CAR... | 7.5 | The impact severity rating is high due to the ... | [{'explanation': 'ID3 is recognized as a class... | { "title": "Decision Tree Algorithms: ID3... | 2025-01-08 | 7 |



# 社区报告最终形成的参考文档

## C4.5 and Decision Tree Algorithms

| 1 | 0c4efb0094a64a3abd1a184b1baab37e | 1 | 1 | -1 | 0 | C4.5 and Decision Tree Algorithms | This community is centered around the C4.5 dec... | # C4.5 and Decision Tree Algorithms\n\nThis co... | 7.5 | The impact severity rating is high due to the ... | [{&#x27;explanation&#x27;: &#x27;C4.5 represents a significan... | {\n &quot;title&quot;: &quot;C4.5 and Decision Tree Algori... | 2025-01-08 | 4 |

## NumPy and ML_Basic_Function Community

| 2 | 983b4cf9540e4ee7820611ff6f6d33ba | 2 | 2 | -1 | 0 | NumPy and ML_Basic_Function Community | This community is centered around the NumPy li... | # NumPy and ML_Basic_Function Community\n\nThi... | 7.5 | The impact severity rating is high due to the ... | [{&#x27;explanation&#x27;: &#x27;NumPy is identified as a Pyt... | {\n &quot;title&quot;: &quot;NumPy and ML_Basic_Function C... | 2025-01-08 | 2 |

```
result.context_text[0]
```



# C4.5 and Decision Tree Algorithms

This community is centered around the `C4.5` decision tree algorithm, which is an advanced version of the `ID3` algorithm. `C4.5` introduces enhancements such as handling continuous variables and employing pruning techniques to prevent overfitting. The community includes related metrics like Information Gain, Information Value, and Gain Ratio, which are integral to the algorithm's functionality. Relationships within the community highlight `C4.5`'s connections to other decision tree algorithms like `CART` and its reliance on metrics for dataset splits. [Data: Entities (1)]

### C4.5's advancements over ID3

`C4.5` represents a significant evolution from the `ID3` algorithm by introducing the ability to handle continuous variables and employing pruning techniques to prevent overfitting. These enhancements make `C4.5` more robust and effective for practical applications, addressing several limitations of its predecessor. [Data: Entities (1)]

### Role of Information Gain in C4.5

Information Gain is a critical metric used by `C4.5` to measure the reduction in entropy after a dataset split. This metric is foundational in the decision tree construction process, ensuring more accurate dataset partitioning. `C4.5` builds on Information Gain by incorporating Information Value to calculate Gain Ratio, further refining the decision-making process. [Data: Entities (11), Relationships (26)]

### Importance of Information Value

Information Value is used by `C4.5` to correct Information Gain calculations, thereby improving decision tree splits. This metric plays a crucial role in enhancing the algorithm's accuracy and reliability by ensuring that the dataset splits are based on the most informative attributes. [Data: Entities (12), Relationships (24)]

### Gain Ratio as a key metric

Gain Ratio is a pivotal metric in `C4.5`, calculated by dividing Information Gain by Information Value. It guides the algorithm in evaluating and selecting dataset splits, ensuring that the decision tree is constructed in a manner that maximizes predictive accuracy. [Data: Entities (10), Relationships (25)]

### C4.5's relationship with CART

`C4.5` and `CART` are both decision tree algorithms that share similarities in handling continuous variables and utilizing Gain Ratio values for dataset splits. However, they differ in their partitioning strategies, with `C4.5` relying on information entropy and `CART` employing cut points. This relationship highlights the diversity in approaches within decision tree algorithms. [Data: Relationships (4)]

# Decision Tree Algorithms: ID3, C4.5, and CART

This community revolves around key decision tree algorithms used in machine learning, specifically `ID3`, `C4.5`, and `CART`. `ID3` serves as the foundational algorithm, with `C4.5` and `CART` offering improvements and extensions, respectively. These algorithms are interconnected through their evolutionary development and their application in machine learning tasks, particularly in classification and regression. The community also includes entities like `SKLEARN`, a machine learning library that implements `CART`, and various dataset columns (`AGE`, `INCOME`, `STUDENT`, `CREDIT_RATING`) used in calculating information gain and reducing impurity in decision tree models.

### ID3 as the foundational decision tree algorithm

`ID3` is recognized as a classic decision tree algorithm primarily used for classification tasks. It employs information entropy to evaluate the purity of splits, focusing on discrete variables. Despite its limitations, such as the tendency to overfit and inability to handle continuous variables directly, `ID3` has played a crucial role in the evolution of decision tree-based machine learning techniques. Its foundational principles have been built upon by successors like `C4.5` and `CART`, highlighting its significance in the community. [Data: Entities (0), Relationships (0)]

### C4.5's improvements over ID3

`C4.5` is an improved version of `ID3`, designed to address several limitations of its predecessor. It introduces features like continuous variable handling, pruning, and improved attribute selection criteria, making it a more robust and versatile algorithm. These enhancements have allowed `C4.5` to maintain the core principles of decision tree construction while overcoming the challenges faced by `ID3`, thereby solidifying its position in the community. [Data: Relationships (0)]

### CART's versatility in handling both classification and regression tasks

`CART` (Classification and Regression Trees) extends the capabilities of decision tree algorithms by supporting both classification and regression tasks. It builds binary trees and uses cut points for dataset partitioning, effectively handling continuous variables. `CART`'s methodology has been integrated into popular machine learning libraries like `SKLEARN`, making it a versatile and widely used algorithm in the field. [Data: Entities (2), Relationships (7)]

### SKLEARN's implementation of CART

`SKLEARN`, or `Scikit-learn`, is a widely used machine learning library that implements the `CART` algorithm for decision tree modeling. While it does not directly support `ID3` and `C4.5`, `SKLEARN`'s decision tree models are based on `CART`'s robust framework, enabling users to leverage its capabilities for predictive modeling and data analysis. This integration underscores the importance of `CART` in the community and its influence on machine learning practices. [Data: Entities (3), Relationships (7)]

### Role of dataset columns in decision tree modeling

Dataset columns such as `AGE`, `INCOME`, `STUDENT`, and `CREDIT_RATING` play a crucial role in decision tree modeling by calculating information gain and reducing impurity. These columns are used in algorithms like `ID3` to partition data effectively, highlighting their significance in the decision-making process of machine learning models. [Data: Entities (6, 7, 8, 9), Relationships (10, 11, 12, 13)]

# NumPy and ML_Basic_Function Community

This community is centered around the `NumPy` library and the `ML_Basic_Function` module, which are interconnected through their use in numerical computations for machine learning tasks. `NumPy`, being a fundamental library for numerical computations in Python, plays a crucial role in the functionality of `ML_Basic_Function`, indicating a significant dependency relationship.

### NumPy's foundational role in machine learning

`NumPy` is identified as a Python library essential for numerical computations, including those required in machine learning algorithms such as decision trees. Its widespread use and importance in data science and machine learning projects underscore its foundational role in these fields. [Data: Entities (4)]

### ML_Basic_Function's dependency on NumPy

`ML_Basic_Function`, a module or library for machine learning tasks, likely utilizes `NumPy` for numerical computations. This dependency highlights the interconnectedness of tools and libraries within the machine learning ecosystem and the pivotal role of `NumPy` in enabling complex computations. [Data: Relationships (9)]

### The significance of numerical computations in machine learning

Numerical computations are at the heart of machine learning algorithms, enabling the processing and analysis of large datasets. The reliance of `ML_Basic_Function` on `NumPy` for these computations illustrates the critical importance of efficient and reliable numerical computation libraries in advancing machine learning technologies. [Data: Entities (4, 5), Relationships (9)]

### Potential for innovation and development

The relationship between `NumPy` and `ML_Basic_Function` suggests a community with a strong foundation for innovation and development in machine learning. The integration of these tools can lead to advancements in algorithm efficiency, accuracy, and the development of new machine learning models. [Data: Relationships (9)]



## 模型调用次数和使用的token数量

```
print(
    f"LLM calls: {result.llm_calls}. Prompt tokens: {result.prompt_tokens}. Output tokens: {result.output_tokens}."
)
```

```
LLM calls: 2. Prompt tokens: 3501. Output tokens: 1006.
```
