
# Datawhale人工智能培养方案V2.0

> 来源：[https://datawhaler.feishu.cn/docx/Y9CbdUWlmov6guxJw6tcNRU9nEe](https://datawhaler.feishu.cn/docx/Y9CbdUWlmov6guxJw6tcNRU9nEe)

> 当前页面的内容已不再维护，培养方案已迁移至Datawhale官网：https://www.datawhale.cn/learn/summary/40，请前往官网查阅最新版~

## 📍 版本

版本号：V2.0      发布日期：2023年9月23日

版本号：V0.9.2      发布日期：2023年4月3日

版本号：V0.9.1      发布日期：2023年3月6日

版本号：V0.9.0      发布日期：2021年12月16日

## 🍭 前言

本培养方案是Datawhale的小伙伴们多年以来的积累，以期许能构建出一套完善的人工智能课程体系，帮助初涉人工智能的你（也是曾经的我们）提供一份切实可行的学习路线。

## 🔍 使用说明

本培养方案按照专业方向进行编排，每个专业方向均包含以下几块内容：

- 🍿 故事汇：通过讲故事的方式，展现该方向从业人员一天的生活，以此来让你直观感受理应具备的知识以及知识的具体运用场景；
- 🗒 课程表：按照课程性质、课程类型列出课程，其中课程的排列顺序即为本培养方案推荐的学习顺序。

课表中的课程按照性质分为以下几类：

- 基础：基础课是该方向从业人员理应具备的基本素养，建议熟练掌握；
- 必修：必修课是该方向的专属课程，建议熟练掌握；
- 选修：选修课是该方向在日常工作中可能会用到的额外知识，可选择性学习；
- 实践：实践课是对必修课中所学知识的综合运用，可选择性学习。

诚挚欢迎任何有意向为本培养方案贡献新课程或者完善现有课程的同学加入，具体请联系Datawhale开源项目管理委员会负责人谢文睿（🛰️：at-Sm1les）；

本培养方案由Datawhale开源项目管理委员会制定并发布，获取最新版请关注🛰️👸号：Datawhale，回复“培养方案”。

## 📃 自然语言处理（Natural Language Processing，NLP）

### 🍿 故事汇

《自然语言处理算法工程师的一天》

小文，一名工作经验两年半的`NLP`算法工程师，平时最擅长使用`Ctrl`键和其他键的组合操作。

这天早上来到公司后，小文刚从书包掏出电脑准备开始摸鱼，结果`PM`（产品经理）突然就杀到工位来了，小文心想这是线上模型又出Case了？

好在是虚惊一场，原来`PM`是来拉他去参加一个新项目的启动会，在会上`PM`提出想给公司`APP`上的视频增加一个叫“节点摘要”的东西，也就是在视频的下方列出各个关键节点的内容摘要和起止时间，方便用户快速了解视频的大纲并快进到自己想看的地方。小文听后觉得这个项目有搞头，爽快地接下了这个需求。

小文回到工位后，打开电脑，打开终端，登录进`Linux`系统的[开发机，连接](https://github.com/datawhalechina/learn-python-the-smart-way-v2)上本地的`VSCode`，[开始用`Python`](https://github.com/datawhalechina/juicy-bigdata)编写批量捞取视频数据的`Hadoop`脚本。小文随机抽样了一批数据，通过摸底分析，再结合自己对需求的理解，与`PM`共同讨论制定了第一期算法所要达到的目标、排期以及算法上线的评估标准，在打破`PM`不切实际的幻想同时又给了他一些盼头。

做完这些前期准备工作后，小文便开始撰写技术方案并跟[`Leader`](https://github.com/datawhalechina/learn-nlp-with-transformers)进行汇报。凭借着扎实的`NLP`基础以及对最新技术的调研和分析，小文很快拟出了以下几套方案：

- 方案一：用信息抽取的方法直接从字幕中抽取起始时间和摘要，优点是技术难度和资源消耗均低，缺点是抽取出的摘要效果受字幕质量直接影响；
- 方案二：先用信息抽取抽取的方法抽取各个关键节点的起止时间，然后依此对字幕分段，接着用文本生成的方法对每段字幕生成摘要，优点是效果会优于方案一，缺点是技术难度和资源消耗均高于方案一；
- 方案三：仅用一个文本生成模型生成出各个关键节点的内容摘要和起止时间，优点是效果最好，且仅用了一个模型，易于维护，缺点是技术难度和资源消耗最高。

`Leader`收到小文的技术方案后，对其中的细节做了些许修订，最后综合当前的人力、资源、排期等各种因素，与小文商定的结论是以上方案的可行性都没问题，但是需要依次分阶段进行。

技术方案敲定后，小文便开始着手准备模型训练所需的数据，经过数据预处理、撰写数据标注文档、对接数据标注团队、培训标注人员、验收标注数据等一系列工作后，数据终于准备完毕。

按照计划，小文首先尝试的是方案一，不出所料，小文仅基于信息抽取领域最新的`SOTA`模型做了简单的任务适配后，便训出了一个效果达标的模型，于是开始走模型上线流程，其主要包括模型封装、性能测试、刷库上线、小流量实验，经过一段时间的小流量对比实验观测后发现实验指标符合预期，于是开始全量上线，至此小文的第一期收益已收入囊中。

由于方案一是纯抽取的方案，所以在全量上线后，`PM`也发现了很多因为字幕错字导致摘要也错字的情况，于是开始跟小文报Case。好在这一切尽在小文及其`Leader`的预期之中，小文安抚完`PM`以后便开始了算法的二期迭代，也就是开始尝试方案二。

方案二相对于方案一就没那么顺利了，由于文本生成的不稳定性，在解决了一些老问题之后又引入了一些新的问题，比如由分段独立生成摘要导致的连续两个摘要之间语义不连贯。不过作为一名练习时长两年半的`NLP`算法工程师，这对小文来说已经习以为常了，通过对Case的拆解和分析以及大量的实验，大部分问题均被小文分别用数据、模型、前处理、后处理等手段给逐个解决了，很快方案二的模型效果再次达到了上线标准，到此小文在这个项目的第二期收益也斩获到手。

经过前两期的迭代，目前线上视频摘要的用户体验已经趋于完善，但不满现状的`PM`又开始发挥了新的想象力，找到小文说：“咱们能不能每个视频生成多条不同时间线的摘要，然后结合用户画像选择性展示用户最感兴趣的那条时间线？”

小文听完后虽然认可[`PM`](https://github.com/datawhalechina/so-large-lm)的想法，且方案三正好能满足这个需求，但是经过生成模型的折磨后，小文并没有敢答应下来，因为根据他在做方案二期间的实验观察，现阶段的生成模型很难保证方案三的可行性，于是果断拒绝了`PM`的需求，继续迭代方案二，直到第二天：`OpenAI`发布`ChatGPT`……

### 🗒 课程表

[课程 | AI 学习路线](https://datawhaler.feishu.cn/wiki/FFNHwwyw7ilUs7ktS97cTYKSnUg?table=tbla3tTudeclCa5D&view=vew4aJC7c4)

| 课程性质 | 课程类型 | 课程名 | 课程资料 |
| --- | --- | --- | --- |
| 基础 | Python基础 | 聪明办法学Python | 文字教程：https://github.com/datawhalechina/learn-python-the-smart-way-v2 |
|  | 机器学习理论 | 吃瓜教程 | 文字教程：https://github.com/datawhalechina/pumpkin-book |
|  | 机器学习实践 | 西瓜书代码实战 | 文字教程：https://github.com/datawhalechina/machine-learning-toy-code |
|  | 深度学习理论 | 水很深的深度学习 | 文字教程：https://github.com/datawhalechina/unusual-deep-learning |
|  |  | 李宏毅机器学习笔记 | 文字教程：https://github.com/datawhalechina/leeml-notes |
|  | 深度学习实践 | 深入浅出`PyTorch` | 文字教程：https://github.com/datawhalechina/thorough-pytorch |
| 必修 | 自然语言处理基础 | 基于`transformers`的自然语言处理入门 | 文字教程：https://github.com/datawhalechina/learn-nlp-with-transformers |
|  | 大语言模型（`LLM`）基础 | 大语言模型（`LLM`）原理与实践 | 文字教程：https://github.com/datawhalechina/so-large-lm |
| 选修 | 强化学习基础 | 强化学习教程 | 文字教程：https://github.com/datawhalechina/easy-rl |
|  | `Pandas`基础 | `Pandas`数据处理与分析 | 文字教程：https://github.com/datawhalechina/joyful-pandas |
|  | `SQL`基础 | 奇妙的`SQL` | 文字教程：https://github.com/datawhalechina/wonderful-sql |
|  | 大数据开发基础 | 妙趣横生大数据 | 文字教程：https://github.com/datawhalechina/juicy-bigdata |
| 实践 | 大语言模型（`LLM`）实践 | `ChatGPT`原理与实践 | 文字教程：https://github.com/datawhalechina/hugging-llm |
|  |  | 面向开发者的`Prompt`工程 | 文字教程：https://github.com/datawhalechina/prompt-engineering-for-developers |
|  |  | `Prompt`从入门到应用 | 文字教程：https://linklearner.com/lesson/1/summary |
|  |  | `Prompt`手册 | 文字教程：[AI `Prompt` 工程师认证&学习指南](https://datawhaler.feishu.cn/docx/ITDod4LCjo5yoixJHkdcUDw6nrc) |
|  | 文本分类 | 零基础入门`NLP` - 新闻文本分类 | 赛题介绍：对新闻文本进行分类 |
|  | 其他 | 数据竞赛Baseline & Topline分享 | 教程地址：https://github.com/datawhalechina/competition-baseline |

## 🛍 推荐系统（Recommender System，RS）

### 🍿 故事汇

《推荐系统算法工程师的一天》

作者：罗如意

小明，一名工作经验两年半的推荐算法工程师，每天来到公司的第一件事情就是打开`BI`报表，看一下大盘核心指标走向如何，如果指标波动较大，可能会从不同用户维度、曝光内容等维度继续下钻分析是否是某个人群或者某类物料导致的波动。

如果`BI`报表中的核心指标波动符合预期，接下来会打开[`AB`](https://github.com/datawhalechina/fun-rec)实验平台，看一下前几天刚上线的一个召回模型实验效果。在网页还在加载的过程中，小明心里对于这个召回实验的预期是，点击率和[`时长`](https://github.com/datawhalechina/wonderful-sql)都涨。打开`AB`实验报表发现点击率[`指标`](https://github.com/datawhalechina/free-excel)涨了，但是时长指标提升不明显，甚至有点[`微跌`](https://github.com/datawhalechina/fun-rec)的趋势。小明对于这个实验现象感觉不符合预期，立马打开了数据分析平台，疯狂写了多个`SQL`，并把实验结果复制到`Excel`中画图分析，经过大量的分析之后发现，实验组在实验期间短文本内容曝光占比突增，并且精排对这些短文本的打分也相对较高。此时小明好想发现了什么问题，实验组召回模型更容易召回短文本，于是他继续分析这路召回为什么召回了这么多短文本的......经过了大量的样本、特征、召回策略的分析，最终找到最有可能的原因，并将其改进后，重新上线了新版的模型，等明天再继续观察一下实验，此时小明暗暗窃喜，明天指标会都涨了，这时候基本可以准备吃午饭了。

吃完午饭，午[`休`](https://github.com/datawhalechina/juicy-bigdata)了一会儿又开始继续搬砖，打开代办事项列表发现精排模型需要对于新用户进行优化，并且还挺高优的。此时小明心里已经有了初步的方案，但是为了紧跟前沿，小明首先是打开知乎看看是否有大佬分享了优化新用户的创新方法，浏览了一两个小时的知乎之后，结合自己储备的知识，确定了2-3种方案之后，开始对当前的新用户的日志数据进行分析，最终确定了一种方案。对于当前确定的方案，需要构造样本、构造特征，对于简单的基础特征直接使用`SQL`快速开发，对于复杂的特征、样本采样等会使用`Scala`写`Spark`任务，特征和样本的开发时间可能会分几天去开发，短时间开发不完。

小明看到日程里面晚上还有个产品的需求会，晚上早早的就去吃饭了，吃完饭之后小明和同时绕着园区逛了几圈消消食，离开会还有10分钟了，小明就急急忙忙的上楼带上电脑去会议室了。这次产品又有个比较简单的需求，需要考虑推荐内容的画风，首刷内容中搞笑类的内容至少要有一个，并且只能出现在第二个位置。对于这个简单的需求，小明给产品的排期为1天。

晚上开完会后，也到8点了，回到工位后梳理了一下最近要做的事情，并且按照优先级重新排序了一下另一天要做的事情，最后打开知乎看看有没有大佬总结的最新论文或者打开之前没有看完的论文接着看，9点左右就准备下班回家了。

### 🗒 课程表

[课程 | AI 学习路线](https://datawhaler.feishu.cn/wiki/FFNHwwyw7ilUs7ktS97cTYKSnUg?table=tbla3tTudeclCa5D&view=vewxgwG2ui)

| 课程性质 | 课程类型 | 课程名 | 课程资料 |
| --- | --- | --- | --- |
| 基础 | Python基础 | 聪明办法学Python | 文字教程：https://github.com/datawhalechina/learn-python-the-smart-way-v2 |
|  | `Pandas`基础 | `Pandas`数据处理与分析 | 文字教程：https://github.com/datawhalechina/joyful-pandas |
|  | `NumPy`基础 | 巨硬的`NumPy` | 文字教程：https://github.com/datawhalechina/powerful-numpy |
|  | 机器学习理论 | 吃瓜教程 | 文字教程：https://github.com/datawhalechina/pumpkin-book |
|  | 机器学习实践 | 西瓜书代码实战 | 文字教程：https://github.com/datawhalechina/machine-learning-toy-code |
|  | 深度学习理论 | 水很深的深度学习 | 文字教程：https://github.com/datawhalechina/unusual-deep-learning |
|  |  | 李宏毅机器学习笔记 | 文字教程：https://github.com/datawhalechina/leeml-notes |
|  | 深度学习实践 | 深入浅出`PyTorch` | 文字教程：https://github.com/datawhalechina/thorough-pytorch |
| 必修 | 推荐系统基础 | 有趣的推荐算法 | 文字教程：https://github.com/datawhalechina/fun-rec |
| 选修 | 集成学习基础 | 集成学习 | 文字教程：https://github.com/datawhalechina/ensemble-learning |
|  | 强化学习基础 | 强化学习教程 | 文字教程：https://github.com/datawhalechina/easy-rl |
|  | `SQL`基础 | 奇妙的`SQL` | 文字教程：https://github.com/datawhalechina/wonderful-sql |
|  | `Excel`基础 | 自由`Excel` | 文字教程：https://github.com/datawhalechina/free-excel |
|  | 大数据开发基础 | 妙趣横生大数据 | 文字教程：https://github.com/datawhalechina/juicy-bigdata |
| 实践 | 新闻推荐 | 零基础入门推荐系统 - 新闻推荐 | 赛题介绍：根据用户历史浏览点击新闻文章的数据信息预测用户未来的点击行为 |
|  | 其他 | 数据竞赛Baseline & Topline分享 | 教程地址：https://github.com/datawhalechina/competition-baseline |

## 📷 计算机视觉（Computer Vision，CV）

### 🍿 故事汇

《计算机视觉算法工程师的一天》

作者：华泽

小图，一名工作经验两年半的`CV`算法工程师，其每天的工作内容如下：



# 🤖 计算机视觉（Computer Vision，CV）

## 🍿 故事汇 《计算机视觉工程师的一天》

数据标注：首先是检查标注组新标注出的数据质量和数量，进行验收和指导，剔除掉不合格的标注，如果需求有变动则会重新沟通确定后续的标注数据类型、数据源等。若是非常简单的数据标注，小图通常会写`Python`脚本来跑一些预训练模型、调调参数推理一遍数据，也能标注出符合要求的数据。

读论文：处理完数据，小图通常会花一些时间阅读最新的论文，`CV`领域进展迅速，每天都会不断涌现出新的方法和技术，比如[目标检测领域最近又又又新](https://github.com/datawhalechina/dive-into-cv-pytorch)出了`YOLO` [`YOLOv7`、`YOLOv8`](https://github.com/datawhalechina/hugging-sd)，还有相关的`pro`、`max`、`plus`等变体版本，简单翻了翻，有结合了`Transformer`的，有结合了`Diffusion`的图像生成算法的，有使用多模态、大模型技术做的，偶尔还会出现一些从`NLP`领域借鉴的思路，先全部加入到待办事项列表中，之后有空了再详细看看，写一篇笔记，在部门组会上分享。

需求评审：客户和商务团队的成员经常会提出新的功能和需求，因此需要小图和架构师、产品等开会讨论，哪些些可以做， 哪些做不了，哪些做起来很费事费时等，最后确定一下需求优先级，商量如何进行合理地排班。

写代码：小图看了一眼bug面板和需求排班面板，需要他处理的工作如下：

1. “部分摄像头视角的画面[存在鱼眼畸变](https://github.com/datawhalechina/team-learning-cv/tree/master/ImageProcessingFundamentals)，需要[新增正畸处理](https://github.com/datawhalechina/powerful-numpy)”——使用`OpenCV`库结合`NumPy`的一些操作，跑了一批数据，调整下参数，效果很好，直接整理代码开始提测；

2. “客户反馈识别不准，漏检严重”—— 仔细一看是原来是一个祖宗级别的端侧项目，用的是`TensorFlow1`+机器学习算法+`OpenCV`来识别高空抛物的算法项目，代码一查有上千行了，这就有点棘手了，需要找同事、架构师和产品一起拉个会讨论讨论是重构还是接着填坑；

3. “内存泄露，服务器每天都要重启一次” —— 出问题的项目采用的技术方案是`PyTorch`转`TensorRT` + `C++`的`GPU`推理侧部署，内存泄漏的可疑点有很多，需要深入排查；

4. “新需求评审会 下午xxx”；

5. ...

模型训练：作为公司年度十佳员工的小图，一想到显卡集群里面的`A100`显卡晚上下班后就会闲着，他就睡不着觉，于是赶在下班前给模型训练代码加一下`PyTorch`的多机多卡并行训练、混合精度训练，以提升显卡的利用率和模型训练速度，确保第二天就能看到新模型的效果。

## 🗒 课程表

[课程 | AI 学习路线](https://datawhaler.feishu.cn/wiki/FFNHwwyw7ilUs7ktS97cTYKSnUg?table=tbla3tTudeclCa5D&view=veweidPARp)

| 课程性质 | 课程类型 | 课程名 | 课程资料 |

| --- | --- | --- | --- |

| 基础 | Python基础 | 聪明办法学Python | 文字教程：`https://github.com/datawhalechina/learn-python-the-smart-way-v2` |

|  | NumPy基础 | 巨硬的NumPy | 文字教程：`https://github.com/datawhalechina/powerful-numpy` |

|  | 机器学习理论 | 吃瓜教程 | 文字教程：`https://github.com/datawhalechina/pumpkin-book` |

|  | 机器学习实践 | 西瓜书代码实战 | 文字教程：`https://github.com/datawhalechina/machine-learning-toy-code` |

|  | 深度学习理论 | 水很深的深度学习 | 文字教程：`https://github.com/datawhalechina/unusual-deep-learning` |

|  |  | 李宏毅机器学习笔记 | 文字教程：`https://github.com/datawhalechina/leeml-notes` |

|  | 深度学习实践 | 深入浅出PyTorch | 文字教程：`https://github.com/datawhalechina/thorough-pytorch` |

| 必修 | 图像处理基础 | OpenCV | 文字教程：`https://github.com/datawhalechina/team-learning-cv/tree/master/ImageProcessingFundamentals` |

|  |  | 动手学CV | 文字教程：`https://github.com/datawhalechina/dive-into-cv-pytorch` |

|  | 图像生成基础 | Stable Diffusion教程 | 文字教程：`https://github.com/datawhalechina/hugging-sd` |

| 实践 | 语义分割 | 零基础入门语义分割-地表建筑物识别 | 赛题介绍：使用给定的航拍图像训练模型并完成地表建筑物识别 |

|  | OCR | 零基础入门CV - 街景字符编码识别 | 赛题介绍：从Google街景图像中识别出门牌号 |

|  | 综合 | 数据竞赛Baseline &amp; Topline分享 | 教程地址：`https://github.com/datawhalechina/competition-baseline` |

# 📊 大数据开发（Big Data Develop）

## 🍿 故事汇 《大数据开发工程师的一天》

作者：王嘉鹏

小鹏，一名工作经验两年半的大数据开发工程师。每天的上午是一天中最重要的日子，对于小鹏也是这样。由于运营同学有一个重要报表数据在昨天完成了上线，今天需要确保运营上午上班后能根据有效数据来做策略。早上6点，小鹏便被报警电话叫醒，打开了电脑查看原因，发现`workflow`工作流提示在数据查询时写`ads`表时出错了。

于是小鹏马上估算了下解决问题的时间，确保 8点前可以修复并产出数据时，他开始了修复。首先，他检查经过`ETL`产出数据是否和期望数据一致。确保一致后，接下来，他按照数仓建模的思路（`ads`->`dm`->`dwm`->`dwd`）倒序排查出错的原因，在排查到`dwm`时，发现是因为资源打满导致任务失败。

随后，他尝试着了改进`Hive`分区表，在上层建立二级分区的方式打散数据，以便能更有效的支持检索。同时，他进行了关键性能优化，分析了一个长时间运行的数据处理作业，尝试了调整数据分片和调整`Spark`作业的配置，从而更快的执行结果。同时他也分析了维度分布，使用提前预聚合的方法打散数据，方便之后的存储。

最终到了8点，`ads`表顺利写入，这就预示着9点时运营需要的数据能正常产出。小鹏长吐一口气，整理了下修改的思路文档，以便上午上班后和业务方同步下优化后的效率提升程度。

到了下午，又接了两个取数的需求，需要和看板数据做对比使用。小鹏分析了下需求，只需要比对数据，而无需再建立数仓，因此只需要和业务方对齐口径，写相应的`SQL`即可。

下午5点，小鹏给业务方交付了数据，检验通过。在业务方使用数据时，又提了一些边角料的需求，套用之前储备好的模板便快速搞定了，最终结束了一天的工作。

## 🗒 课程表

[课程 | AI 学习路线](https://datawhaler.feishu.cn/wiki/FFNHwwyw7ilUs7ktS97cTYKSnUg?table=tbla3tTudeclCa5D&view=vewcvLKWl3)

| 课程性质 | 课程类型 | 课程名 | 课程资料 |

| --- | --- | --- | --- |

| 基础 | Python基础 | 聪明办法学Python | 文字教程：`https://github.com/datawhalechina/learn-python-the-smart-way-v2` |

|  | SQL基础 | 奇妙的SQL | 文字教程：`https://github.com/datawhalechina/wonderful-sql` |

|  | Excel基础 | 自由Excel | 文字教程：`https://github.com/datawhalechina/free-excel` |

|  | Pandas基础 | Pandas数据处理与分析 | 文字教程：`https://github.com/datawhalechina/joyful-pandas` |

|  | NumPy基础 | 巨硬的NumPy | 文字教程：`https://github.com/datawhalechina/powerful-numpy` |

|  | 机器学习理论 | 吃瓜教程 | 文字教程：`https://github.com/datawhalechina/pumpkin-book` |

|  | 机器学习实践 | 西瓜书代码实战 | 文字教程：`https://github.com/datawhalechina/machine-learning-toy-code` |

| 必修 | 大数据开发基础 | 妙趣横生大数据 | 文字教程：`https://github.com/datawhalechina/juicy-bigdata` |

# 📈 数据分析（Data Analysis，DS）

## 🍿 故事汇 《数据分析师的一天》

小红，一名工作经验两年半的的数据分析师，每天的工作内容主要是围绕数据收集、分析和报告编写展开。早上来到公司后，小红便接到了来自业务方的需求，今天的需求是分析公司销售数据以判断市场趋势。跟业务方详细了解完数据需求和分析任务后，小红便开始进行数据收集，调用了公司内部的数据管理系统，编写相应`SQL`获取了所需的销售数据、用户行为数据和产品数据。

数据收集完成后，小红先是用`Excel`对数据进行了较为简单的初步整理和清洗，接着，小红进一步使用`Python`的`Pandas`进行更为复杂的数据预处理，如缺失值处理、异常值检测等。完成这些数据预处理工作后，小红便得到了可以进行分析的数据。

[接下来是数据分析环节](https://github.com/datawhalechina/fantastic-matplotlib)。依据业务方的需求，小红用`Matplotlib`进行数据可视化，制作了各类统计图表，如柱状图、折线图和散点图，直观地展示数据的分布和变化趋势。同时小红也使用了机器学习算法对数据进行回归和分类分析。

午餐后，小红开始编写数据分析报告。首先描述了数据收集和预处理的过程，然后详细解释了数据分析的结果，包括统计结果和图表。在报告的结论部分，小红总结了本次数据分析的主要发现，并提出了基于数据分析的建议。

报告编写完成后，小红将它发送给了业务方，并一起开会讨论了报告的内容，对部分数据进行了再次分析和解读，以确保数据分析结论的准确性。

下午的时候，小红进行了一些例行的数据更新和维护工作，保证公司的数据系统能正常运行。同时还对一些新的数据分析工具和技术进行了学习和研究，以提升自己的数据分析能力。

总的来说，通过数据分析，小红帮助公司理解了业务现状和趋势，为决策提供了有价值的信息。

## 🗒 课程表

[课程 | AI 学习路线](https://datawhaler.feishu.cn/wiki/FFNHwwyw7ilUs7ktS97cTYKSnUg?table=tbla3tTudeclCa5D&view=vewrjUtY75)

| 课程性质 | 课程类型 | 课程名 | 课程资料 |

| --- | --- | --- | --- |

| 基础 | Python基础 | 聪明办法学Python | 文字教程：`https://github.com/datawhalechina/learn-python-the-smart-way-v2` |

|  | SQL基础 | 奇妙的SQL | 文字教程：`https://github.com/datawhalechina/wonderful-sql` |

|  | Excel基础 | 自由Excel | 文字教程：`https://github.com/datawhalechina/free-excel` |

|  | Pandas基础 | Pandas数据处理与分析 | 文字教程：`https://github.com/datawhalechina/joyful-pandas` |

|  | NumPy基础 | 巨硬的NumPy | 文字教程：`https://github.com/datawhalechina/powerful-numpy` |

|  | 机器学习理论 | 吃瓜教程 | 文字教程：`https://github.com/datawhalechina/pumpkin-book` |

|  | 机器学习实践 | 西瓜书代码实战 | 文字教程：`https://github.com/datawhalechina/machine-learning-toy-code` |

| 必修 | 数据分析基础 | 动手学数据分析 | 文字教程：`https://github.com/datawhalechina/hands-on-data-analysis` |

| 选修 | 集成学习基础 | 集成学习 | 文字教程：`https://github.com/datawhalechina/ensemble-learning` |

|  | 数据可视化 | matplotlib奇遇记 | 文字教程：`https://github.com/datawhalechina/fantastic-matplotlib` |

|  |  | 极好的Plotly | 文字教程：`https://github.com/datawhalechina/wow-plotly` |

| 实践 | 分类 | 零基础入门金融风控-贷款违约预测 | 赛题介绍：根据贷款申请人的数据信息预测其是否有违约的可能 |

|  |  | 零基础入门数据挖掘-心跳信号分类预测 | 赛题介绍：根据心电图感应数据预测心跳信号 |

|  | 回归 | 零基础入门数据挖掘之二手车交易价格预测 | 赛题介绍：根据汽车类型等信息预测二手汽车的交易价格 |

|  | 综合 | 零基础入门数据分析之学术前沿趋势分析 | 赛题介绍：使用公开的arXiv论文完成对应的数据分析操作 |

|  | 其他 | 数据竞赛Baseline &amp; Topline分享 | 教程地址：`https://github.com/datawhalechina/competition-baseline` |

# 💐 致敬课程背后可爱的贡献者们

薛传雨、王复振、杨煜、闫钟峰、杨梦迪、苏鹏、徐旺华、张晋、汪健麟、杨佳达、耿远昊、郝少春、谢文睿、秦州、贾彬彬、居凤霞、马晶敏、周天烁、叶梁、胡风范、陈安东、金娟娟、老表、李玲、高立业、张文涛、李祖贤、六一、陈琰钰、杨剑砺、李运佳、皮钱超、詹好、王茂霖、李一飞、杨昱文、刘羽中、王贺、刘洋、王佳旭、金一鸣、李嘉骐、牛志康、罗如意、吴忠强、何世福、徐何军、李万业、陈锴、梁家晖、宁彦吉、田雨、赖敏材、汪志鸿、王辰玥、唐鑫、宋禹成、王琦、杨毅远、江季、陈信达、安晟、闫永强、宋志龙、沈豪、袁明坤、王程伟、张强、游璐颖、樊亮、杨皓博、康兵兵、刘太路、芙蕖、聂平、张帆、张贤、李泺秋、蔡杰、hlzhang、台运鹏、张红旭、陈海顺、聂雄伟、杰少、王鹏、骆秀韬、王洲烽、蒋志政、王嘉鹏、伊小雪、胡锐锋、长琴、黄玉琳、易华挥、邹雨衡、徐虎、刘伟鸿、Joye、魂兮、宋志学、韩颐堃、陈逸涵、仲泰、万礼行、小饭同学、诸世纪、Zhang Yixin、Sarai
