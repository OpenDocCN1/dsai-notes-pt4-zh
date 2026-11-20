
# 从零入门CV图像竞赛(Deepfake攻防)

> 来源：`https://datawhaler.feishu.cn/docx/OnIHd4wQxoxc5vxeuWLc43hinCc`

> 从零入门CV图像竞赛(Deepfake攻防) 是 Datawhale 2024 年 AI 夏令营第二期 的学习活动（“CV图像”方向），基于蚂蚁集团举办的“外滩大会-全球Deepfake攻防挑战赛”开展的实践学习——

+ 适合想入门并实践深度学习和计算机视觉、检测识别AI生成内容、Kaggle竞赛的学习者参与
+ 只要会用电脑，就能跟着本文档内容，在30分钟内完成赛事速通体验！
+ 快来拿下你的第一个“全球Deepfake攻防挑战赛”分数！

> 让我们一起学习进阶，动手实践、写笔记打卡，
> 努力上分冲刺夏令营TOP10，获得荣誉证书和奖学金！

## 30 分钟 速通 Deepfake图像攻防！

> 整个学习活动会带领大家进行项目制学习，由浅入深，逐渐进阶。
> 当前环节可让你在30分钟内，从一个完全小白的选手，跑通“全球Deepfake攻防挑战赛”！
> 我们整个学习活动，将带你从跑通最简的Baseline，到了解竞赛通用流程、深入各个竞赛环节，精读Baseline与进阶实践，这些一个都不会少！
> 千里之行，始于足下，从这里，开启你的AI学习之旅吧！
> 记得在完成速通后，填写打卡表单抽奖哟~
> —— Datawhale贡献者团队

### Step0：注册Kaggle

> 如果有Kaggle账户，直接登录即可，无需处理此步骤

1. 使用Edge浏览器安装 `Header Editor`插件(点击即可跳转)

> 🔗链接：`https://microsoftedge.microsoft.com/addons/detail/header-editor/afopnekiinpekooejpchnkgfffaeceko`

![](img/8619fe6b50d029feac5f87a34d3ae3fd.png)

![](img/bd505fa48f3d2415101b575d4c0160b9.png)

![](img/f9c61617cd60a18f4c8c8b8a24f67aee.png)

1. 设置 `Header Editor`插件

![](img/ba5bf12946db1871c6e817d0abaeb25d.png)

![](img/9865879df99e44bf92684062e3c906b1.png)

![](img/d4b8d87345af12bae969b4721fbf7604.png)

1. 打开 `Kaggle`注册网址(点击即可跳转)

> 🔗链接：`https://www.kaggle.com/account/login?phase=emailRegister&returnUrl=%2Fcompetition%2FInclusion%E3%83%BBThe+Global+Multimedia+Deepfake+Detection`

### Step1：报名赛事！（点击即可跳转）

> 赛事链接：`https://www.kaggle.com/competitions/multi-ffdi`
> 报名问卷：`https://www.wjx.cn/vm/mh3Cxns.aspx` (必填！！！)
> 登录后直接报名即可~
> 另外别忘了填写问卷注册~

1. Kaggle报名：`https://www.kaggle.com/competitions/multi-ffdi`

![](img/0c042585737425dffe0e9dcbbfd6514c.png)

1. 填写注册报名问卷：`https://www.wjx.cn/vm/mh3Cxns.aspx`

> 友情提醒：【必填，否则参赛成绩无效！！！！】

+ 必填，根据报名规则，注册参赛才算报名成功，否则成绩无效也无法领取大赛提供的算力；
+ 提交后7个工作日审核完成，请耐心等候；
+ 未审核通过不影响大家参赛，可先行参赛。

![](img/e4e3608d9ebc52e24404890a300f41c8.png)

+ 先在Kaggle点击`join`加入比赛，然后提交报名表单，队长kaggleID是首次组队成功的用户的`username`，查看方式：kaggle注册>`setting`>`username(not editable)`

![](img/144478bfe72599267678045ead4207a9.png)

![](img/c8c47942dc475ae32c7dee8eafc25dac.png)

### Step2：20 分钟体验一站式 baseline！（点击即可跳转）

> 项目链接：`https://www.kaggle.com/code/finlay/deepfake-ffdi-baseline/`
> 食用指南如下，大概需要20分钟~

1. 复制项目内容，启动项目

![](img/b807da926b5bbfb7008215e5c7a716e9.png)

+ 新用户需验证手机号(已验证过可跳过)

![](img/86386a4a2c0549e1a62142a3d1d53826.png)

![](img/97694dc59db1753641895de6fcabf0f2.png)

![](img/0334db5458c24190c8c8c3ae17b2337c.png)

![](img/de699801907868f207954b63c4fa2ab8.png)

![](img/238c42250f8818b09aed929b995901ef.png)

1. 设置处理器配置

![](img/e54a9eaa13111297efb28ca187abec9a.png)

1. 一键运行代码~`(约15分钟)`

![](img/5233a4afb1cb1f9703ed1ff1c9535131.png)

> 等待的过程中可以仔细查看baseline的内容~
> 里面为大家进行了赛题的解析哟~

1. 运行结束后，下载`submit.csv`

> 即等待上方的 `Cancel Run` 恢复成 `⏩Run All`，且`Output`中出现`submit.csv`文件

![](img/ff4b3fc760d29adc81dd090e366811e4.png)

![](img/0a0c29d307b9336ad455bb1b17616031.png)

![](img/bd07ebc9ff482deef49c3a861df25dec.png)

![](img/42a4b48ea9aec46a9a032ec4e4e63020.png)

1. 记得关闭项目~

![](img/5df4abd2b7dad699b4fa9bafd35311ce.png)

### Step3：提交文件，拿下第一个分数！（点击即可跳转官网进行提交）

> 链接：`https://www.kaggle.com/competitions/multi-ffdi/submissions`
> 提交结果文件，参与排行榜评比

![](img/f7834a287bdace8a1d6464cd9c4f0c26.png)

![](img/aefb1f9132eae007f6ce5ecafaceae1f.png)

![](img/44c69140bcc941c70b97b3d526e4788d.png)

![](img/ab51cbebeb44d0bd38c635219e4e4913.png)

> 评分需要2分钟～评分结束后可以在下方看到自己提交的评分结果哟！

## 🎉 恭喜你拿下了属于你的第一个分数！

![](img/1f1800a98ced6062f6dbe69c6b2c2bb0.png)

### 👍🏻 快来给自己点赞！

### 🎁 然后填写表单，开启抽奖吧~祝你好运~

> 请大家提交完作品后填写下面的表单哦~
> 直接在下方填写或点击打开链接：`https://datawhaler.feishu.cn/share/base/form/shrcnm0mqlp3kHsy2oYmXGqks3e`
> 参与者还可以参与Datawhale【抽奖】哦~有机会获得Datawhale精美周边小礼品！！
> 也可以把你的分数截图转发到群里呀~

`https://datawhaler.feishu.cn/share/base/form/shrcnm0mqlp3kHsy2oYmXGqks3e?iframeFrom=docx&ccm_open=iframe`

## 📅 记住下面的学习规划表！

> **教程贡献者说：**
> 在本课程中，我们将深入探讨Deepfake技术的原理、应用案例以及检测方法。通过实践项目和案例研究，你们将有机会亲自动手，体验Deepfake技术的制作过程，并学习如何辨别Deepfake内容。
> 接下来我们会基于这个赛题进行更多的知识点学习，会涉及到：
> 1. 深度学习概念
> 1. AIGC 和 Deepfake的关系
> 1. 深度学习模型原理
> 1. 构建Deepfake攻防模型
> 1. 数据集处理与增强

> **学习规划表如下：**

| 从零入门AI竞赛(CV图像方向)-学习规划表（7天） |  |  |  |  |
| --- | --- | --- | --- | --- |
| 全球Deepfake攻防挑战赛-图像赛道 |  |  |  |  |
| **Task** | **Task 描述** | **相关知识点** | **学习文件&链接** | **学习安排** |
| **Task 1** | **目标：**跑通baseline，体验深度学习解决问题的流程，基本了解赛题要求，理解赛题场景和数据 | 1. 深度学习的任务定义 | 1. [2-3-deepfake图像Task1：速通手册](https://datawhaler.feishu.cn/wiki/Uou8w9igsibGP7kduiycCgesnOh) | 1. [**开营直播**](https://tongyi.aliyun.com/efficiency/doc/transcripts/klrbn2x3l5g4q5zy?sl=1&source=1#) |
| **Task 2** | **目标**：深入理解赛题，精读baseline代码，入门深度模型、deepfake攻防模型的代码实现 | 1. 深度学习的模型细节 | 1. [Task2知识点文档](https://datawhaler.feishu.cn/wiki/CsS2weZvgigEKok6IzycdiVunuy) | 1. [**baseline精讲直播**](https://meeting.tencent.com/v2/cloud-record/share?id=9fca4970-8f9b-406a-9c1a-bc15871247cd&from=3&is-single=false&record_type=2) |
| **Task 3** | **目标**：提高方案精度效果，了解上分具体操作 | 1. 数据集增强 | 1. [Task3知识点文档](https://datawhaler.feishu.cn/wiki/Ad0jwNK8Eis5XwksFZ7cCvb6nHh) | **进阶分享直播** |
|  |  |  |  |  |

### 祝大家上分顺利，学习愉快~ 不要害怕在群里提问和交流哟！

> 不过建议先查看 [QA & 其他彩蛋](https://datawhaler.feishu.cn/wiki/YcRLw35DhiYqBqkf4zwc35O8nOg) 是否早已解决并收录你的问题~ 然后再在群里仔细描述清楚问题进行提问哟~

> QA文档可任意编辑，可随时留言提问，我们的助教会关注，遇到感兴趣的问题还可以在上面进行回答哟~

## 🍰 再来一点学习加餐！

### 你知道刚刚发生了什么吗？

> 刚刚我们跑通了Baseline，想必你一定很好奇这背后发生了什么吧？
> 在学习的过程中，不单单要知道如何做，更需要知道为什么要这样做。这样我们在遇到同类型的问题时，才能举一反三。
> 一般情况下，拿到一个赛题之后，我们需要明确：
> 1）这是一个什么场景下的赛题；
> 2）这个赛题要解决什么问题。
> 深度思考这些问题，既能帮助我们更好地解决相关问题，更能让我们在后续遇到相关问题时，更好地完成迁移。

### 本次学习活动的关联赛题是一个 CV问题

> CV通常指的是计算机视觉（Computer Vision），它是人工智能的一个分支，专注于使计算机能够从图像或多维数据中解释和理解视觉信息。计算机视觉的应用包括图像识别、物体检测、场景重建等。
>
> 本次CV任务较为特殊，属于图像识别分类的一个分支，赛题任务是【开发并优化Deepfake图像检测模型】
> 识别通过Deepfake技术(一种人工智能算法)生成的高度逼真的图像内容，应对虚假信息、欺诈行为和隐私侵害等因Deepfake技术加剧的问题。
>
> [Datawhale](https://datawhalechina.github.io/dive-into-cv-pytorch/#/)有一个开源项目 动手学CV-Torch，以`PyTorch`为框架，强调动手实战，项目大体上会按照计算机视觉的不同领域和应用场景进行章节划分，循序渐进，带领大家走进CV的世界，以动手实战的方式，从入门到进阶。
> 学习该项目需要一定的代码基础
> 大家学有余力可以通过其中的理论知识和实战内容尝试实践上分~

### 什么是 Kaggle、Jupyter Notebook、Python、CV、深度学习？

+ `Kaggle` 是一个在线平台，提供数据科学和机器学习竞赛、数据集、工具和学习资源。
> 数据科学家和机器学习工程师可以在`Kaggle`上参与各种竞赛，解决实际问题，并分享他们的代码和模型。
> `Kaggle`上可以创建云端运行的代码环境，运行 `Jupyter Notebook` 文件

+ `Jupyter Notebook` 是一个开源的 Web 应用程序，允许用户创建和共享包含实时代码、方程、可视化和解释性文本的文档。
> 它广泛用于数据清理和转换、数值模拟、统计建模、机器学习等领域。我们的 `Baseline` 就是上传在`Kaggle`上面的一个Notebook，复制并 `start session`后我们可以运行Python代码，也可以学习到其他的人的代码。在Notebook中我们可以查看到这个代码的输入（`Input`），输出（`Output`）和日志（`Logs`）。

![](img/fec3f7bf7f6b1fde088313c0a43cb91c.png)

+ `Python` 是一种广泛使用的高级编程语言，以其清晰的语法和代码可读性而闻名。
> Python 支持多种编程范式，包括面向对象、命令式、函数式和过程式编程。它在数据科学、机器学习、Web开发、自动化脚本等领域中非常流行

+ 深度学习是机器学习的一个子领域，它使用类似于人脑中的神经网络结构的人工神经网络来模拟和解决复杂问题。深度学习在图像识别、语音识别、自然语言处理和其他许多领域中表现出色，是当前人工智能研究的热点之一

### 更多内容详见[Task1知识点文档](https://datawhaler.feishu.cn/wiki/L4kKwgPqMiGFb4kmCKecKCpxnqd)~

### QA详见：[QA & 其他彩蛋](https://datawhaler.feishu.cn/wiki/YcRLw35DhiYqBqkf4zwc35O8nOg)

> 本教程主要由 Datawhale的小伙伴们 写作贡献，经过多位助教内测和建议修改，期待你的点赞和评论～
