
# 一文梳理推荐系统如何应用大模型

> 来源：[https://agijuejin.feishu.cn/docx/WHl9dOSQ8od9IGxsDbxckiH7ngd](https://agijuejin.feishu.cn/docx/WHl9dOSQ8od9IGxsDbxckiH7ngd)

> 作者：蘑菇先生学习记

> 原文：https://mp.weixin.qq.com/s/3wvh3Xlz1LcUOqJ768uAFw

大家好我是蘑菇先生。

今天给大家分享下华为诺亚方舟实验室的talk:《推荐系统如何从大语言模型中取长补短：从应用视角出发》，个人感觉很有信息量，我们组内同学也做过交流分享，相关信息也可以从综述找到："How Can Recommender Systems Benefit from Large Language Models: A Survey"，在此分享给大家。

![](img/c1703bc408e6a4734b064025a75b5285.png)

下面会围绕：背景和问题、何处运用大语言模型（特征工程、特征编码、打分排序、流程控制，共7个代表性工作）、如何运用大语言模型（如何给`LLM`注入协同信号：微调和不微调，2个代表性工作）、挑战和展望 四个部分展开介绍。由于篇幅原因，代表性工作只做简要介绍，下回会对部分代表性工作做个详细分享。

## 1、背景和问题

传统的推荐模型（Conventional Recommendation Model，`CRM`）往往以`Embedding` + 深度网络为`backbone`，通过拟合用户反馈信号提升推荐效果。`CRM`的主要特点是：

+ 模型相对较小，时间空间开销低；

+ 可以充分利用用户反馈信号；

+ 只能利用数据集内的知识；

+ 缺乏语义信息和深度意图推理。

相对的，大语言模型（`LLM`）以`Transformer`为`backbone`，通过大规模预训练语料和自监督训练提升通用语义理解和生成能力。`LLM`的主要特点是：

+ 引入外部开放世界知识，语义信号丰富；

+ 具备跨域推荐能力，适合冷启动场景；

+ 用户反馈信号缺失；

+ 计算复杂度高，难以处理海量样本。

因此，一个核心问题是 推荐模型如何从大模型中取长补短，从而提升推荐性能，优化用户体验？

为了解答这一问题，作者从应用角度出发，将该问题进一步拆解为：

+ 何处运用大语言模型（`WHERE` to adapt）

+ 如何运用大语言模型（`HOW` to adapt）

![](img/c2de7561ca44e0a5b512ef901a6e0cce.png)

下面会围绕这两个方面来展开介绍。

## 2、何处运用大语言模型：WHERE to Adapt

根据现代基于深度学习的推荐系统的流程，作者抽象出以下五个环节：

+ `数据采集`阶段：线上收集用户行为和记录，得到原始数据(`raw data`)；

+ `特征工程`阶段：对原始数据进行筛选、加工、增强，得到可供下游深度模型使用的结构化数据(`structured data`)；

+ `特征编码`阶段：对结构化数据进行编码，得到对应的稠密向量表示(`neural embeddings`)；

+ `打分排序`阶段：对候选物品进行打分排序，得到要呈现给用户的排序列表(`recommended items`)；

+ `流程控制`：作为中央控制器，把控推荐系统的整体流程。也可以细化到对排序阶段的召回、粗排、精排的控制。

大模型主要可以从 特征工程、特征编码、打分排序和流程控制 这几个部分来提升推荐系统的表现，对应的最近几年的工作如下图所示：

![](img/813cc3d062ed586041e479fa2ee796c9.png)

### 2.1、特征工程

现有工作中，`LLM`在特征工程阶段主要发挥如下作用：利用大语言模型的外部通用知识和逻辑推理能力，进行：

+ `用户理解`：利用`LLM`丰富用户画像，代表工作有`GENRE`、`PALR`、`NIR`等；

+ `内容理解`：利用`LLM`进行`item`画像抽取，代表工作有`GENRE`、`OpenRec`等；

+ `样本扩充`：利用`LLM`进行`item`生成，代表工作有`AnyPredict`、`GENRE`等。

#### 代表性工作1：GENRE(PolyU, 2023)

代表性工作`GENRE`[3]，出自香港理工大学，

同时涉及了用户理解、内容理解和样本扩充三个方向。

![](img/d9b5e3525b12cdb906f1b30ea42641e5.png)

`GENRE` 基于`GPT3.5`进行新闻推荐：

+ `内容理解：输入`新闻标题、摘要、类目信息，生成 简短内容描述；

+ `用户理解：输入用户历史行为数据`，生成用户感兴趣的 话题 和 区域；

+ `样本生成：对于`历史行为数据稀疏的用户，基于少量行为样本生成 相似的伪样本 ，用于扩充用户表征输入。

`GENRE`框架示意图：基于已生成伪样本增强用户理解，然后再生成更准确的伪样本。

![](img/bc30b2a1d74346b9595827743a4c2b77.png)

通过`prompting`增强样本的过程示意：

![](img/f01f3c43206ddc81321fdede0ab1eff1.png)

整体架构：

![](img/44f41dce4584639423cd83c8ce84b661.png)

### 2.2、特征编码

利用`LLM`的通用语义信息丰富推荐特征表示：

+ 增强文本特征(用户表征、物品表征) 表示，代表工作有`PLM-NR`、`UniSeqRep`、`VQ-Rec`、`PREC`、`U-BERT`、`UNBERT`等；

改善基于`ID`的特征表示的跨场景迁移能力。

![](img/5bdc707b817a17a710f4716b4d57fcda.png)

#### 代表性工作2：U-BERT(Tencent, AAAI 2021)

代表性工作2：`U-BERT`: Pre-training user representations for improved recommendation[4] 出自`Tencent`, `AAAI'21`，基于大模型进行用户表征学习。

+ 背景：电商推荐系统中如何使用用户和`item`侧丰富的文本特征

+ 技术方案：

+ `Review Encoder`：基于`BERT`对用户所写的评价进行语义编码；

+ `User Encoder`：基于`attention`网络对`review token`表征进行聚合；

+ 两个子任务，`rating` & `MLM`。

+ 输入：用户历史交互行为以及购买后的评价信息;

+ 预训练阶段

+ 微调阶段(训练)：增加`item`侧编码网络，拟合用户反馈数据。

`U-BERT`预训练阶段：

![](img/3f98aa61d49a63664b31e8fb2f0530f1.png)

`U-BERT`微调阶段：

![](img/2dbd350f73eae1d21b6bc51211b40719.png)

#### 代表性工作3：UniSRec(Alibaba, KDD 2022)

代表性工作3：`UniSRec`: Towards universal sequence representation learning for recommender systems[5]，出自`RUC&Alibaba`, `KDD'22`，基于`item`文本信息进行序列模型预训练，实现跨域推荐的目标(`inductive learning`)。

+ `Universal Textual Item Representation`：基于`BERT`对`item`文本信息编码，通过`MOE Adaptor`实现跨越样本的`Domain Fusion and Adaptation`；

+ `Universal Sequence Representation`：基于`MHA`对序列信息进行聚合；

+ 预训练阶段：对比学习任务，`Sequence-sequence contrastive task` & `Sequence-item contrastive task`；

+ 微调阶段（新领域）：`inductive/transductive`，`fix parameter`加速微调。

![](img/dea1899b7832a988eb06883dc85d12ec.png)

此外，`KDD2023` Amazon的`Text Is All You Need: Learning Language Representations for Sequential Recommendation`，也属于该路径下的代表性工作。

### 2.3、打分排序

打分/排序是推荐系统的核心任务，目标是得到和用户偏好相符的物品(列表)。根据如何得到最终排序列表的形式，作者将大语言模型应用于打分/排序的工作分成以下三种：

+ `物品评分任务 (Item Scoring Task)`：大语言模型对候选物品逐一评分，最后根据分数排序得到最终的排序列表；

+ `物品生成任务 (Item Generation Task)`：通过生成式的方式生成下一个物品的`ID`，或者直接生成排序列表；

+ `混合任务 (Hybrid Task)`：大语言模型天然地适合多任务场景，因此很多工作会利用大语言模型来实现多个推荐任务，其中包括评分任务和生成任务。

#### 代表性工作4：Google LLM+评分预测任务

代表性工作4：`Do LLMs Understand User Preferences? Evaluating LLMs On User Rating Prediction`(`Google`, 2023)[6]

+ 探究语言模型分别在零样本(`Zero-Shot`)，少样本(`Few-Shot`)和微调场景下的评分预测的能力；

+ 基于用户反馈数据进行`LLM`微调；

+ 结论：

+ `zero-shot` vs `statistics`：指标基本可比，说明`LLM`在推荐任务上有一定的`zero-shot`能力；

+ `zero-shot` vs `few-shot`：`few-shot`在`AUC`指标上胜出，说明`few-shot`有一定的提升作用；

+ `fine-tuned LLM` vs 传统模型：`AUC`胜出，说明`LLM`有用作推荐模型的潜力。

零样本和少样本`prompt`示例：

![](img/a16be8ff794bf2fd3ee208b5f7d3b005.png)

`LLM`微调：

![](img/025979bc36ee144964626eedc37c40a3.png)

实验结果：

![](img/348642b63a7b7783b9c328be2a3606d8.png)

#### 代表性工作5：PALR (Amazon, 2023)

`PALR`: Personalization Aware LLMs for Recommendation[7]

+ 利用用户历史交互得到用户画像，然后基于用户画像、历史交互和提前过滤得到的候选集信息生成推荐列表；

+ 经过指令微调，`PALR`在公开数据集上显著超过传统推荐模型`baseline`。

![](img/285f44204bf93d443aaf97c0d632288b.png)

`prompt`例子：

![](img/9ddef5824d85fdaa23e30e51759544ab.png)

#### 代表性工作6：P5(Recsys, 2022)

`Recommendation as language processing (rlp): A unified pretrain, personalized prompt & predict paradigm (p5)`[8]

+ 用一个统一的大语言模型在不同的推荐任务上进行预训练，针对不同任务使用不同推荐模版；

+ 用自然语言为桥梁，规范不同推荐场景的输入输出，并在跨域样本上进行预训练，具备新场景下的`zero-shot`迁移能力。

![](img/608b040652da2232148e5e711a77969e.png)

也可详见我以前的解读： `P5` | NLP模型一统推荐系统? 谈新型推荐系统建模范式

### 2.4、流程控制

#### 代表性工作7：RecLLM，Google 2023

`Is chatgpt fair for recommendation? evaluating fairness in large language model recommendation`[9]

提出了一种使用`LLM`来集成推荐系统流程各模块(检索、排序、用户画像、用户模拟)的一个对话式推荐系统路线图。

![](img/e909747638e9b5106b321f9940c160a8.png)

## 3、如何运用大语言模型

从 `训练` 和 `推理` 两个阶段出发，作者根据以下的两个维度将现有工作分为四个象限：

+ 在 `训练` 阶段，大语言模型是否需要 `微调`。这里微调的定义包含了全量微调和参数高效微调。

+ 在 `推理` 阶段，是否需要 `引入传统推荐模型 (`CRM`)`。其中，如果`CRM`知识作为一个预先过滤`candidate`的作用，则不被考虑在内。

![](img/3e797fafa8861ab21c51fcbd4786d3e6.png)

由上图可以发现两个趋势：

+ 模型：通过引入传统推荐模型（`CRM`）为语言模型注入 协同信号。

+ 数据：通过引入推荐场景的 `数据`，结合 `微调技术`，为语言模型注入 `协同信号`。

### 3.1、不微调`LLM`、引入`CRM`

研究怎么把推荐模型的信号引入`LLM`，但是不进行`LLM`的微调，代表性工作`KAR`。

#### 代表性工作8：KAR(SJTU, 2023)

`Towards Open-World Recommendation with Knowledge Augmentation from Large Language Models.`[10]

背景：`LLM`具备强大的 `语义理解和推理能力`，可以帮助推荐系统完成用户理解（推理能力）和`item`理解（理解能力）。为此提出了一种利用大语言模型开放知识辅助推荐的通用推荐框架`KAR`。

难点：

+ 大语言模型的构成差距问题(`Compositional Gap`) 无法准确回答 `复合推理` 问题，但可以准确回答 复合问题的各个子问题；

+ 用户偏好 `复杂多面`，世界知识混合海量，难以直接生成有效知识。

![](img/cf0599cdcf18e2b9137621ace6385605.png)

解法：受 `因式分解` 的启发，将开放知识生成问题动态分解为多个关键子因素，按因素分别进行用户兴趣推理和知识提取。

技术方案：

+ `知识推理和生成`

+ 基于推荐场景对于决定用户偏好，动态分解出相应的关键因素，对于用户偏好和物品外部知识分别对大语言模型提问；

+ 生成相应的兴趣推理知识和物品事实知识文本。

+ `知识适配`：

+ 所生成的文本信息内容复杂多面（500~1000 `tokens`），且存在噪声和冗余问题，推荐系统无法直接理解和利用

+ 设计多专家网络进行知识提取、压缩、映射，适配至推荐空间，输出结果鲁棒。

+ `知识利用`：

+ 将所生成的知识增强向量作为额外的特征域，结合原本数据特征，进行特征交互，输出最终结果。

效率问题：预存用户长期兴趣（或向量）和`item`知识（或向量），`LLM`不建模用户短期兴趣。

`prompt`工程：

![](img/5dab3cd72673064e146ce86549c29708.png)

系统架构：

![](img/87de6b40b740e34604897f82f9c67840.png)

实验：

![](img/4552614011d1fd2e8c69ccf42e94e9d3.png)

总结下方法创新性：

+ 有效结合大语言模型的通用世界知识与传统推荐系统的领域知识：通过`Factorization Prompting`，有效激发大语言模型针对用户兴趣的推理和知识获取能力，缓解构成差距的问题。

+ 提出混合多专家网络知识适配器，将语言模型生成的世界知识由语义空间适配至推荐空间，进行降维、噪声处理的同时保存有效信息。

+ 推理复杂度与传统推荐模型一致。

### 3.2、微调`LLM`、引入`CRM`

研究怎么把`LLM`和推荐系统模型结合起来，让`LLM`也能拥有协同信号。代表性工作`CTRL`。

#### 代表性工作9：CTRL(Huawei, 2023)

`CTRL`: Connect Tabular and Language Model for `CTR` Prediction[11]，`Huawei` 2023

背景：仅使用语言模型进行推荐目前 `效果不理想`，需要通过引入推荐场景的数据，结合微调技术，为语言模型注入协同信号。为此，华为提出了一种对齐语言模型和协同模型的框架`CTRL`。

具体技术方案：

+ `Prompt construction` ：通过7个模板把表格数据转换为文本数据。

+ 用户和物品特征：特征名+连接词+特征值。

+ 用户历史行为序列：用户的历史类型+动作连接词+历史1|历史2|历史3。

+ 采用`','`作为特征之间的分隔符；采用`'.'`作为用户信息和物品信息的分隔符。

+ `Cross-model Knowledge Alignment` ：将协同模型和语言模型的知识进行对齐。

+ 利用对比学习预训练融合两种模态的信息。

+ `Supervised Finetuning`： 在经过细粒度对比学习预训练之后，两种模态的信息已经进行充分融合。

+ 使用监督信号使协同模型适配下游任务。

+ 通过在不同的任务上微调，可以适配不同的推荐任务。

+ `Serving`：仅使用更轻量的传统模型进行`serving`，不会对`RT`造成影响。

![](img/320e93fed66530e31fb1103d7255291e.png)

实验：
