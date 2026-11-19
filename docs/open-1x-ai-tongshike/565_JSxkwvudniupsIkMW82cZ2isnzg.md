
# 从零入门 AI for Science（AI+药物）

> 来源：[https://datawhaler.feishu.cn/docx/TZqPdk0OtoZPLRxMLqEchEvVnlb](https://datawhaler.feishu.cn/docx/TZqPdk0OtoZPLRxMLqEchEvVnlb)

> 从零入门 AI for Science（AI+药物）是 Datawhale 2024 年 AI 夏令营第三期的学习活动（“AI+药物”方向），基于天池平台“第二届世界科学智能大赛 生命科学赛道：siRNA药物药效预测”开展的实践学习——

+ 适合想**入门并实践深度学习、AI for Science(AI4S)、AI+生命科学**的学习者参与

> **学习内容提要**：从深度学习RNN方案逐步进阶优化，教程主体内容为特征工程&相关领域（生命科学）的特征理解

> 📢**重点提示：**

> 希望选手们由`baseline`中获取解题灵感，争取更好的成绩；如果直接使用`baseline`提交，后续审核出代码雷同情况，将被取消成绩与晋级资格。

## 小白零基础 30 分钟 速通指南

> 我们整个学习活动，将带你从跑通最简的`Baseline`，到了解竞赛通用流程、深入各个竞赛环节，精读`Baseline`与进阶实践，这些一个都不会少！

> 当前环节可让你在30分钟内，从一个完全小白的选手，跑通“siRNA药物药效预测”！

> 千里之行，始于足下，从这里，开启你的 AI 学习之旅吧！

> 记得在完成速通后，填写打卡表单抽奖哟~

> —— Datawhale贡献者团队

### Step1：报名赛事！(点击即可跳转)

> 赛事链接：`http://competition.sais.com.cn/competitionDetail/532230/format`

> **登录并报名组队~**

![](img/8452d4acde3188eb70721224d577dfcb.png)

![](img/4913f9f1cfe41a71a1ec2245a891f3ba.png)

![](img/9883030c50440ae0918bcc190b71f7bb.png)

![](img/1bf58cef42c0fa8bd9bd40780a1e7b3f.png)

<https://datawhaler.feishu.cn/sync/E8g9dSVhBshgxUbmbRHc4c8BnKd>

### Step2：下载代码和数据

> 下载下方两个文件即可

+ 模型训练的代码脚本文件

![](img/b6d476c0bf9735d43fdb93960cc535a9.png)

[`task3.2_siRNA.ipynb`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/ISCnbH27coScdPxkZp1cVp49nM2)

+ 训练模型所用到的数据集(赛题指定数据集)

![](img/a78a060ef4e8cbf347c9f053a7b63edb.png)

[`siRNA_0715.zip`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/UT0jb1tjRobqVkxu7lqcXG07nxf)

### Step3：启动魔搭GPU环境（点击即可跳转）

> 魔搭链接：`https://modelscope.cn/my/mynotebook/preset`

> **进入魔搭，选择方式二，启动！**

![](img/c145b09a5467a468ef09a54dceda3382.png)

选择方式二即可

![](img/e6efe8e572985cbec12823a829a9b9b3.png)

实例启动预计2分钟，请稍等

![](img/a4fcb54c4e5f44f00c9837dae39f04f6.png)

<https://datawhaler.feishu.cn/sync/H03Edp1Ozs86WsbAivYcxgRlnLe>

### Step4：运行`baseline`！

> 上传数据和代码文件，然后**一键运行**！

1. 新建目录

> 创建一个目录，可任意命名

![](img/47eb15c27889bd0df4085d44b61262ea.png)

1. 在刚刚创建的目录下上传代码和数据文件

> 双击打开目录，拖入代码和数据文件完成上传

![](img/19988b6a2842020b0d1f6288d44150a1.png)

![](img/f6c5337ba7deda238212819fb9d7019e.png)

1. 进入终端

![](img/dc28887f2802e1f6353079977abae493.png)

![](img/c14a6d04fb1c679f9267b8f17c5f893d.png)

1. 解压数据集

> 复制下面的命令，粘贴在终端处并回车运行

```
unzip siRNA_0715.zip
```

![](img/30a2e0da956837143436152b00a12789.png)

![](img/b86326f0351c7dc286a3bc530350a4fc.png)

1. 打开名字为`task3.2_siRNA.ipynb`的代码文件，一键运行代码！（大概需要10分钟）

![](img/487f4a6834d555cae765d42abffb7025.png)

![](img/3f3e53217786c236dd945b72c05d397f.png)

（如果时间过长，检查Step3是不是误开了CPU）

![](img/1a7217e5459cecbda4490fc55f431d5d.png)

![](img/7fa328b46fb509911d4f59422ccadb45.png)

> 等待运行完成的过程中，可以看看文件的内容，通过大模型学习代码和其中的文字，进一步了解赛题和学习代码哟~

1. 下载`submission.csv`结果文件

![](img/acb7496e91615f0e1a8403c26251d488.png)

![](img/2a49f83e46cfc85cf7e3b254945e0d0f.png)

1. 运行结束后记得关闭实例

> 在结束开发后，大家记得及时保存环境现场。

> 魔搭Notebook会在没有后续操作的1小时后自动关闭，仅保存`notebook`文件。

> 如果你有其他任何需要保存的文件，请提前下载保存到本地。

> 在确定所有东西已经备份后，最好主动关闭实例~

![](img/241a97efebe657d1245feb50fce376bd.png)

+ 如果没有【关闭实例】按钮，可以回到魔搭Notebook页(点击即可跳转)进行实例关闭

> 链接：`https://modelscope.cn/my/mynotebook/preset`

![](img/0f57e75e1b77f4f9f63989f0b193a1ab.png)

### Step5：提交文件，拿下第一个分数！(点击即可跳转官网进行提交)

> 链接：`http://competition.sais.com.cn/competitionDetail/532230/mySubmissions`

> **提交结果文件，参与排行榜评比**

![](img/b7473ca4556091b7cdc3aa7643fde778.png)

![](img/8fa0c3968dc84ae987907548b48d5fda.png)

![](img/216e90e2423812703900d538d05c8fe2.png)

![](img/26112e6a232e7c6386671b935661d1f4.png)

> **评分大概需要 1 分钟左右～**评分结束后可以在下方看到自己提交的评分结果哟！（预期分数在53分左右😉~这里为了快速演示，只使用了部分特征`['siRNA_antisense_seq', 'modified_siRNA_antisense_seq_list']`进行训练，在后续的教程中带大家了解这些不同特征的含义和关系，了解这些概念后，大家可以尝试利用更多特征进行上分）

## 🎉恭喜你拿下了属于你的第一个分数！

### 👍🏻 快来给自己点赞！

### 🛎️ 别忘记打卡&在群里接龙！

<https://datawhaler.feishu.cn/sync/AL1ed0JwusN33ObQYJKcyhgxnMg>

### 📅 然后迎接后续的挑战吧！

| 从零入门 AI for Science(AI + 药物) |  |  |  |

| --- | --- | --- | --- |

| Task | Task 描述 | 相关知识点 | 学习安排 |

| Task 1 |  | 1. 深度学习-RNN 基础方案 | 开营直播 |

| Task 2 |  | 1. 赛题背景深入理解&数据分析 | `baseline`精读直播 |

| Task 3 |  | 1. 其他模型优缺点介绍 | 上分思路交流会 |

|  |  |  |  |

> **打怪难度：简单**

1. 跑通`baseline`，拿下第一个分数！

1. 写笔记，完成Task1打卡

> **打怪难度：⭐️**

1. 精读`baseline`和领域背景知识

1. 写笔记，完成Task2打卡

> **打怪难度：⭐️⭐️**

1. 精读`baseline2`、在群里分享思考

1. 参与上分思路交流会

1. 写笔记，完成Task3打卡

### 祝大家上分顺利，学习愉快~ 多在群里提问和交流哟！

+ 赛事链接：[第二届世界科学智能大赛 生命科学赛道：siRNA药物药效预测](http://competition.sais.com.cn/competitionDetail/532230/format)(点击跳转赛事官网)

+ 提问前建议先查看[ QA & 其他彩蛋](https://datawhaler.feishu.cn/wiki/H0lHw6hpei3f9ik0C8acyotjnbe)是否早已解决并收录你的问题~（QA文档可任意编辑，助教会不定时查看）

+ 更多学习活动问题可见：[AI+药物学习者手册](https://datawhaler.feishu.cn/wiki/KIncw3WXRiuff9k5zTvc6dSSnad)

## 接下来进入到[Task1：赛题解析&背景入门](https://datawhaler.feishu.cn/wiki/Vl0lwGPztirnKikW2cScrvqnnSg)，正式了解一下赛题吧！

### 更多QA可见[ QA & 其他彩蛋](https://datawhaler.feishu.cn/wiki/H0lHw6hpei3f9ik0C8acyotjnbe)！

> [ QA & 其他彩蛋](https://datawhaler.feishu.cn/wiki/H0lHw6hpei3f9ik0C8acyotjnbe)任何想法都可以在学习群里交流或在进行填写哟~

> 别忘了打开[Task1：赛题解析&背景入门](https://datawhaler.feishu.cn/wiki/Vl0lwGPztirnKikW2cScrvqnnSg)，了解赛题！

> [Task2：深入理解赛题，入门RNN和特征工程](https://datawhaler.feishu.cn/wiki/E2egwwoWEieE0Lk0aYmccSXOnjd)果大家等不及想提分的话，还可以开始的学习！

>

> 本教程主要由写作贡献，

> 融合了多位助教的内测体验和宝贵建议。

> 如果你觉得对你有价值和启发，请在下面点赞和评论哟~

>

### 另外别忘了写笔记打卡呀！！~

### 及时思考、沉淀和产出能让你的学习事半功倍！

>

> **注：本文采用署名—非商业性使用—相同方式共享 4.0 协议国际版许可协议进行许可。
