
# 从零入门 AI for Science（AI+化学）

> 来源：https://datawhaler.feishu.cn/docx/D7WXd0IyZoNNUKxYxETc4w9fnEh

从零入门 AI for Science（AI+化学）是 Datawhale 2024 年 AI 夏令营第三期的学习活动，基于天池平台“第二届世界科学智能大赛 物质科学赛道：催化反应产率预测”开展的实践学习——

- 适合想 入门 并 实践 机器学习 算法、用AI解决化学&物质科学问题 的学习者参与

学习内容：教程提供的 `Baseline` 涉及 机器学习（随机森林）、深度学习 `RNN`、`Transformer` 的方案，教程会一一介绍这些方案的构建思路并给出代码实现示例，最后会简单介绍大模型解题方案（选学）

📢 重点提示：

希望选手们由 `baseline` 中获取解题灵感，争取更好的成绩；如果直接使用 `baseline` 提交，后续审核出代码雷同情况，将被取消成绩与晋级资格。

## 本文档内容速览

## 小白零基础 30 分钟 速通指南

这个部分，我们直接跑通官方给出的 `Baseline`，了解竞赛通用流程，深入各个竞赛环节，并在最后附上精读 `Baseline` 与进阶实践。

让你在 30 分钟内，从一个完全小白的选手，跑通 “化学反应产率预测挑战赛”！

千里之行，始于足下。从这里，开启你的 AI 学习之旅吧！

记得在每次完成学习后，写笔记打卡哟~ 及时总结沉淀能让学习事半功倍~

—— Datawhale贡献者团队

### Step1：报名赛事！（点击即可跳转）

赛事链接：http://competition.sais.com.cn/competitionDetail/532233/format

登录后直接报名即可~

![](img/62e1839398b242b239109da56668a40e.png)

![](img/1c60a449fafdb0925a88a334ba15893a.png)

- 没有账号需要先申请账号

![](img/d13a6069607d10afeda39175aeeab65b.png)

![](img/2bd4dd5d6a8899a87c350a893ee3111c.png)

### Step2：下载baseline代码和数据

- 将鼠标箭头悬浮在下方文件，即会出现下载按钮

资料包中有：`baseline` 代码、赛事数据。

![](img/4e52dcbe442234243c789cee6b144af1.png)

[`AI+化学baseline文件包.zip`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/BW9Ub581VoFgiyxGzKpcEDbdneg)

- （可选学习）从官网下载数据，可按下面步骤下载赛事数据

![](img/042a2a9630835a9cb201715369070eaf.png)

- 需要认证可按照下方步骤操作

![](img/baf01452ab955bd24d73d63f5b9fef08.png)

![](img/3671d71cb18b3c792fd113d5eb8b8e8e.png)

![](img/4c2b2affc3e848b6f8a766d549e00bf2.png)

![](img/912ecd0f89ec27f82c9ecfcc8e610a45.png)

### Step3：启动魔搭Notebook！（点击即可跳转）

链接：https://www.modelscope.cn/my/mynotebook/preset

直接点击启动！

![](img/bd9d61889ac4b837b78bc935f8873178.png)

点击「启动」

![](img/190d6e818e6e1243134acaadeae10d72.png)

进入 notebook

![](img/8c0f767f34a10e4cbc996cb7d49b87ab.png)

- 新用户需要先注册 & 绑定阿里云账号

![](img/189a667ec7989dd474f530911edb7e6e.png)

![](img/92a0358ff54adeb0a3d06c72c1ac95ce.png)

![](img/e51a6daae1e82663cd27c8a67ef11476.png)

![](img/c20e925f42c57149255d8a8cbdecfa38.png)

![](img/6e1acc9a89a15a2b1575b8861225392a.png)

![](img/31394cb46fdb54bb08fa82fbe76572d6.png)

- 进入环境时，可能需要登录阿里云账号，支付宝扫码登录即可

![](img/e6ef277039b0a35bb5cedabe794e4c14.png)

- 如果用户被拉黑，需添加魔搭官方钉钉群解除

![](img/f4e006f933a1c70319ee6b815c805427.png)

![](img/588f239547c49862f7065bbdf53c9326.png)

钉钉扫码加群

去魔搭钉钉售后群（用钉钉扫码），@技术人员 你的用户名 解除就可以了

- 如果之前使用过 Notebook，注意不要和之前的文件弄混

### Step4：10分钟体验一站式 baseline！

1. 上传文件

![](img/c4b88994e9baa17fb02287789d332799.png)

- 按图示新建文件夹，新建 `mp/`，并将代码和赛事数据从本地拖入到魔搭：

1. 解压文件

使用命令 `unzip AI+化学baseline文件包.zip`

![](img/95e677812d00471b847a47489701b5f7.png)

![](img/3e562a24c1a0f44530bb7719303fb162.png)

1. 打开 `baseline` 代码文件，一键运行！（大约等待 10 分钟~）

![](img/228aac2d7b92a9c69f14eb8aefd0b2cb.png)

![](img/848498d09797ff7066cc038e3293f223.png)

![](img/b2b16dc39a7dafe1862fa7d316727f19.png)

1. 执行完成后，下载 `submit.txt`！

当左侧出现 `submit.txt` 文件时，即为运行完成，可右键点击文件进行下载

![](img/4d5afe7c66316d329ecd3ed466c8f34f.png)

#### 注意：为了减少存储开销，魔搭平台在每次结束实例之后都会删除所有除 `ipynb` 的文件，所以每次退出之前记得把文件下载下来。下载只需右键文件，然后 `download` 就可以了。

### Step5：提交文件，拿下第一个分数！（点击即可跳转官网进行提交）

链接：http://competition.sais.com.cn/competitionDetail/532233/mySubmissions

![](img/7a00af68876a12bc02fa4ecdb1f64d1f.png)

![](img/37591e90256dae6c854c1fbae45c6366.png)

![](img/a77978acd983a3e09a096a28f45ce5cc.png)

注意提交次数和文件要求哦

![](img/9b04027f48c8f13d76e993589e19d81b.png)

一般提交几分钟后，可以看到自己的分数

可以看到，这个成绩还比较低。

所以，我们会在接下来的学习中深入学习并实践更加先进和高级的方法进行化学反应产率的预测。

## 🎉 恭喜你拿下了属于自己的第一个分数！

### 👍🏻 快来给自己点赞！

### 🛎️ 别忘记打卡&在群里接龙！

<https://datawhaler.feishu.cn/sync/AL1ed0JwusN33ObQYJKcyhgxnMg>

### 📅 然后准备迎接后续的挑战吧！

教程贡献者说：

AI4Science 领域和一般的 AI 领域其实方法都是一样的，大家使用的模型、数据处理手段等都差不多。主要的区别在于：Science 领域有一些自己的工具，或者数据建模手段，这些方法往往在处理需要 Science 的科学知识进行指导。

在接下来的学习中，我们会：

1. 更加深入学习化学信息学相关的知识和方法，数据处理的手段；
2. 对常用的和主流的 AI 算法，在 AI4Chemistry 进行实践；
3. 选择洞府，开启炼丹！

更多详情见如下学习规划表：

| 从零入门 AI for Science(AI + 化学) |  |  |  |
| --- | --- | --- | --- |
| Task | 学习任务 | 学习内容 | 学习安排 |
| `Task 1` |  | 1. AI + 化学 领域背景入门 | 开营直播 7月28号 19：00 |
| `Task 2` |  | 1. AI + 化学 领域主要的课题和数据特点 | 进阶 baseline 精读分享直播 |
| `Task 3` |  | 1. 深度学习进阶-Transformer 方案 | 上分思路交流会 |
| `Task 4` |  | 1. 大模型技术 方案 |  |
|  |  |  |  |

打怪难度：简单

1. 跑通 `baseline`，拿下第一个分数！
1. 写笔记，完成 `Task 1` 打卡

打怪难度：⭐️

1. 跑通 第二个 `baseline`，学习 `RNN`
1. 写笔记，完成 `Task 2` 打卡

打怪难度：⭐️⭐️

1. 跑通 第三个 `baseline`，学习 `Transformer`
1. 写笔记，完成 `Task 3` 打卡

打怪难度：⭐️⭐️

1. 尝试使用大模型技术方案解决赛题
1. 此内容为选学，没有打卡要求

祝大家上分顺利，学习愉快~ 多在群里提问和交流哟！

- 赛事链接：[第二届世界科学智能大赛 物质科学赛道：催化反应产率预测](http://competition.sais.com.cn/competitionDetail/532233/format)（点击跳转赛事官网）
- 提问前建议先查看 [QA & 其他彩蛋](https://datawhaler.feishu.cn/wiki/UdRjwR7uBibaHrkwik7cONb7nCf) 是否早已解决并收录你的问题~ （QA 文档可任意编辑，助教会不定时查看）
- 更多学习活动问题可见：[AI+物质科学 学习者手册](https://datawhaler.feishu.cn/wiki/P04Awyl7EihQ7zkQWBScyr4qn9g)

## 🍰 下面正式来了解一下赛题！

### 赛题解析

#### 天池 物质科学与智能赛道 化学反应产率预测 赛题解析

##### 任务

构建一个能够准确预测碳氮成键反应产率的预测模型。

通过对反应中所包含的反应底物、添加剂、溶剂以及产物进行合理的特征化，运用机器学习模型或者深度学习模型拟合预测反应的产率。

或者利用训练集数据对开源大语言模型进行微调以预测反应的产率。

总结：

输入：底物和条件，（`SMILES`）

输出：产率，（`float`，0-1 之间）

##### 数据

可以使用开源反应数据，若使用了开源反应数据，需要在代码审核阶段提供数据来源以及相关说明。可以使用开源的数据。但是，目前限于相关数据不是很多，且非常多和杂乱，所以我们主要还是以比赛给出的数据为准。

###### 初赛

初赛数据集仅包含碳氮成键类型反应数据，其中训练集中包含 23538 条反应数据，测试集中包含 2616 条反应数据。训练集与测试集的比例接近 9：1。每条训练数据包含 `rxnid`, `Reactant1`, `Reactant2`, `Product`, `Additive`, `Solvent`, `Yield` 字段。其中 `Reactant1`, `Reactant2`, `Product`, `Additive`, `Solvent` 字段中为对应物质的 `SMILES` 字符串，`Yield` 字段为目标字段，是经过归一化的浮点数。

##### 提交的结果文件（`submit.txt`）

`rxnid,Yield`

test1,0.5555

test2,0.6666

..., ...

##### 本次学习活动的赛题是一个典型的回归问题

机器学习一般分为两种类型的问题：回归问题和分类问题。

回归问题：即预测的结果是连续的值。

分类问题：预测的结果是离散的值。

赛题要求就是建模 `SMILES` 式子，然后模型输出到 0-1 之间的连续值。

##### 评价指标

实验真实结果与预测结果 $R^2$ 决定系数来进行评测：

![](img/6ad26d14155ce2d1fc2dcfd6ec307c92.png)

##### 来精读一下 `baseline` 代码

`baseline` 主要的流程如下：

1. **导入库**：首先，代码导入了需要用到的库，包括 `pandas`（用于数据处理和分析），`scikit-learn`（机器学习库），`rdkit`（化学信息工具）。

1. **读取数据**：代码通过使用 `pd.read_csv()` 函数从文件中读取训练集和测试集数据。

1. 使用 Morgan 分子指纹建模 `SMILES`：

- 这个过程需要调用 `rdkit` 的相关模块。然后将 `Reactant1`, `Reactant2`, `Product`, `Additive`, `Solvent` 字段的向量拼接到一起，组成一个更长的向量。

1. **使用随机森林预测结果**：

- 这里直接调用 `sklearn` 的 `RandomForestRegressor` 模块实例化一个随机森林模型，并对 `n_estimators` 等重要参数进行指定。最后使用 `model.fit(x, y)` 训练模型。模型保存在本地 `./random_forest_model.pkl`。

1. **加载模型进行预测，并将保存结果文件到本地**：

- `pkl` 文件直接使用 `pickle.load()` 加载，然后使用 `model.predict(x)` 进行预测。预测的结果保存为比赛官方指定的文件格式。

> 代码及注释见 `ipynb` 文件。

#### 关于数据分析库和机器学习库的后续学习

##### Pandas学习推荐

Datawhale 有一个开源教程项目 `Joyful Pandas`，出版图书《pandas 数据处理与分析》豆瓣链接：https://book.douban.com/subject/35998391/

文字版教程链接：https://inter.joyfulpandas.datawhale.club/Home.html

视频教程链接：https://www.bilibili.com/video/BV1tK4y177AF/

（如果大家想仔细研读的话，可以在学习过程中，依照其中的内容进行实践；还可以多多在此处留言，也许夏令营后就会展开组队学习哟~）

##### 机器学习材料推荐

Datawhale 翻译了李宏毅老师的机器学习教程，可在 https://linklearner.com/learn/summary/13 链接处学习查看

##### sciket-learn 学习材料

强烈推荐直接在官网找实例进行学习，并学会快速查找官方文档。官网：https://scikit-learn.org/stable/index.html

##### 强烈推荐 Datawhale 的学习教程，内容丰富，质量超高👍

https://github.com/datawhalechina?q=&type=all&language=&sort=

大家在本次学习过程中，可以一边将上面推荐的材料作为字典，遇到问题时通过这些材料、大模型、搜索引擎，去翻阅查看，做到理论和实践的结合，真正理解这些理论知识的价值、以及究竟要如何在真实的问题场景中使用。

![](img/ae331fddb1842e4500c287141a816493.png)

### 更多 QA 详见 [QA & 其他彩蛋](https://datawhaler.feishu.cn/wiki/ZDnFwgX0riyIXkkFRAjcJhvqnqf)！

[QA & 其他彩蛋](https://datawhaler.feishu.cn/wiki/ZDnFwgX0riyIXkkFRAjcJhvqnqf) 任何想法都可以在学习群里交流或在 进行填写哟~

如果大家等不及想提分的话，还可以直接开始后续的学习！

本教程主要由 、 、 写作贡献，

融合了多位助教的内测体验和宝贵建议。

如果你觉得对你有价值和启发，请在下面点赞和评论哟~

#### 另外别忘了写笔记打卡呀！！~

#### 及时思考、沉淀和产出能让你的学习事半功倍！

注：本文采用 署名—非商业性使用—相同方式共享 4.0 协议国际版许可协议进行许可。
