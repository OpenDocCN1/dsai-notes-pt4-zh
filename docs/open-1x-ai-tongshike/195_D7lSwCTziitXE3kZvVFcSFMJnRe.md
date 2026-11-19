

# 从零入门大模型微调

> 来源：[https://datawhaler.feishu.cn/docx/OQWCdxHX6o9PqTxQ1zuc06yAnAe](https://datawhaler.feishu.cn/docx/OQWCdxHX6o9PqTxQ1zuc06yAnAe)

> 从零入门大模型微调 是 Datawhale 2024 年 AI 夏令营第四期的学习活动（“大模型技术”方向），基于讯飞开放平台“星火大模型驱动阅读理解题库构建挑战赛”开展的实践学习——

+   适合想了解如何微调、学习微调数据处理、学习通过大模型微调实现试卷QA生成 的学习者参与

> 学习内容提要：基于讯飞大模型定制训练平台和`spark-13b`微调模型，生成高考语文现代文阅读和英语阅读问题QA对

## 内容速览

## 小白零基础 30 分钟 速通指南

> 教程贡献者说：

> 我们整个学习活动，将带你从 跑通最简的`baseline`，到了解竞赛通用流程、深入各个竞赛环节，精读`baseline`与进阶实践，这些一个都不会少！

> 当前环节可让你在 30 分钟 内，从一个完全小白的选手，跑通 “星火大模型驱动阅读理解题库构建挑战赛挑战赛”！

> 千里之行，始于足下，从这里，开启你的 AI 学习之旅吧！

> 记得在每次完成学习后，写笔记打卡哟~ 及时总结沉淀能让学习事半功倍~

### Step1：报名赛事！

> 赛事链接：https://challenge.xfyun.cn/h5/detail?type=question-bank-construction&option=phb&ch=dw24_E7x9gl

> 大家需要先在赛事完成报名才能提交比赛成果哦~

![](img/56d5ec587d80329f49f94e69d33f1ee7.png)

注意：点击报名参赛（如果没登录需要大家登录一下），也请大家登录后完成个人认证以便后续顺利参赛。

![](img/e7d7bc1aff3a6a57ceca07a0428347b3.png)

### Step2：运行`baseline`：数据处理！

> 链接：https://aistudio.baidu.com/projectdetail/8225663

> 这里包含我们的数据处理及微调测试的主要代码。

1.  登录百度账号

![](img/9b9eb14250453d373354b5c082f63cb3.png)

![](img/7a4c8b2e187d031e93e95156a9a180bf.png)

1.  运行一下，启动环境

![](img/905b29a7fae4afa3445455d204401703.png)

![](img/010162bf6c7f1f19ade7a7c8410554aa.png)

![](img/2abf5d269fe57e7d7db72c2e5af6a2ec.png)

+   如果是新用户，需要完善信息：

> 填写完信息后刷新即可

![](img/e91f9389042161fd4267c62109fe141e.png)

1.  一键运行代码！下载`output.jsonl`文件！

![](img/10881fa0990f072b2f84bb92c5eec173.png)

![](img/02a9061140e90b1ea18a28900f3daad5.png)

> 到这里我们就能拿到数据处理后的文件，`output.jsonl`文件。这可是我们实现微调的宝贝！

### Step3：模型微调

> 链接：https://training.xfyun.cn/dataset/datasetIndex

> 这里我们将刚才处理好的阅读题目（`output.jsonl`）文件准备好，带上一口仙气准备微调模型！

![](img/fc6885eb68a6fbdf9a57baa0052d45ec.png)

#### 数据上传

![](img/2176d0e567c297018751c58827e6d0d7.png)

#### 等待处理

![](img/9be2554a5635c5a1a23977365630c8b4.png)

#### 模型训练

点击去训练后，我们进入模型管理页面。优惠券会赠送到大家账户，直接用即可。

![](img/87c78c6f5462d249e05eac12c166d786.png)

![](img/1609ac23369a062ec0bd1531e1c208d3.png)

一般会训练30分钟左右（目前平台推理时间过长，可能实际训练时间会更久一些有的甚至3-4h），如果排队的话可能会久一些。大家泡杯咖啡静静等待~

#### 模型发布

![](img/cf1c6e59563538dcb4e1b2ffbee0ee6f.png)

![](img/b0c9380f7fe9065af0ad403012c07a95.png)

注意：如果大家没有应用请到 https://console.xfyun.cn/app/myapp 点击创建创建一个。

![](img/5b5dc22f35cd2dedd7871c6550ce913b.png)

### Step4：模型测试

![](img/1d0a65f9d130b964e526f633c59473b4.png)

请大家正确输入

```
SPARKAI_APP_ID = ''
SPARKAI_API_SECRET = ''
SPARKAI_API_KEY = ''
serviceld=
resourceld=
```

![](img/ba499b629761493a4c8c92e3e3116eda.png)

接着运行一下notebook，点击这个代码块左边的启动按钮即可，我们能看到模型的输出。

![](img/9573cf69096277fab5b2dc148ed13cee.png)

### Step5：提交文件，拿下第一个分数！

> 链接：http://challenge.xfyun.cn/topic/info?type=question-bank-construction&option=tjjg

> 这一步我们可以将微调模型和结果上传准备跑分咯~给自己鼓鼓掌吧

点击提交结果后，需要在https://training.xfyun.cn/刚才的微调页面拿一下`resourceid`

![](img/e60f9c2254d0eab33da19c7e5e3357d9.png)

接着拿好我们的prompt：

中文部分

```
    (1)理解文中重要概念的含义
    (2)理解文中重要句子的含意
    (3)分析论点、论据和论证方法
```

英文部分

```
    (1)Understanding the main idea of the main idea.
    (2)Understand the specific information in the text.
    (3)infering the meaning of words and phrases from the context
```

准备好后按照下图填写即可（[快速跳转](http://challenge.xfyun.cn/topic/info?type=question-bank-construction&option=tjjg)）~

![](img/4c8354f7a9b3abb7ef32ff9eea2a40e2.png)

> 评分大概需要 1-2h～评分结束后可以在【提交页面最下方】处看到评分结果哟！（预期分数在71分左右😉~）（目前因为参赛选手较多评分时间过长，可能实际时间会更久一些有的甚至3-4h）

![](img/680e1394f3a4aea7c96aa950ebc49a61.png)

## 🎉 恭喜你拿下了属于自己的第一个分数！

## 👍🏻 快来给自己点赞！

## 🛎️ 别忘记打卡&在群里接龙！

![](img/f4efc551c5b9b43beed22e46d4fa2614.png)

## 📅 然后迎接后续的挑战吧！

> 教程贡献者说：

> 大家已经完成星火大模型驱动阅读理解题库构建挑战赛挑战赛的`baseline`下面我们会基于这个赛题进行更多的知识点学习，会涉及到：

> `baseline`设计思路、微调数据处理、微调的原理和流程等等

> 更多详情见如下学习规划表：

| 从零入门大模型微调 |  |  |  |

| --- | --- | --- | --- |

| Task | Task 描述 | 相关知识点 | 学习安排 |

| Task 1：3+1天 |  | 1.  讯飞微调平台使用 | 开营直播 |

| Task 2：3天 |  | 1.  `baseline`精读 | `baseline`精读直播 |

| Task 3：3天 |  | 1.  上分思路：数据扩充 | 上分思路交流会 |

| 选学辅助材料 |  | 1.  测评Prompt扩展思路 | - |

|  |  |  |  |

> 打怪难度：简单

1.  跑通 `baseline`，拿下第一个分数！

1.  写笔记，完成 `Task1` 打卡

> 打怪难度：⭐️

1.  精读`baseline`和背景知识、在群里分享思考

1.  写笔记，完成 `Task2` 打卡

> 打怪难度：⭐️⭐️

1.  精读 `baseline2`、在群里分享思考

1.  思考更多上分技巧，参与上分思路交流会

1.  写笔记，完成 `Task3` 打卡

> 打怪难度：⭐️⭐️

1.  选学部分，没有打卡要求

1.  按需学习即可

### 祝大家上分顺利，学习愉快~ 多在群里提问和交流哟！

+   赛事链接：星火大模型驱动阅读理解题库构建挑战赛(点击跳转赛事官网)

+   提问前建议先查看 [ QA & 其他彩蛋](https://datawhaler.feishu.cn/wiki/AWAQwWgPkignkUkU43zcbdfQnrh) 是否早已收录你的问题~ （QA文档可任意编辑，助教和教程贡献者会不定时查看）

+   更多学习活动问题可见： [从零入门大模型微调-学习者手册](https://datawhaler.feishu.cn/wiki/Fofkw2XmViQ7aUk4vbFce5cNnSh)

## 🍰 下面正式来了解一下赛题！

### 你知道刚刚发生了什么吗？

> 刚刚我们跑通了MVP，想必你一定很好奇这背后发生了什么吧？

> 在学习的过程中，不单单要知道如何做，更要知道为什么要这样做。这样我们在遇到同类型的问题时，才能举一反三。

> 一般情况下，拿到一个赛题之后，我们需要明确：

> 1）这是一个什么场景下的赛题；

> 2）这个赛题要解决什么问题。

> 深度思考这些问题，既能帮助我们更好地解决相关问题，更能让我们在后续遇到相关问题时，更好地完成迁移。

### 本次学习活动的赛题“星火大模型驱动阅读理解题库构建挑战赛挑战赛”是一个典型的 通过微调大模型实现QA生成问题

> 当你准备完成QA问卷设计这样的任务时都有可能遇到他们~通过本次学习你会从数据的处理到微调设计，一步步实现微调，测试模型最后让模型生成QA的过程。

## 赛题解读

| 星火大模型驱动阅读理解题库构建挑战赛 |  |

| --- | --- |

| 赛事链接 | [https://challenge.xfyun.cn/h5/detail?type=question-bank-construction&amp;option=phb&amp;ch=dw24_E7x9gl](https://challenge.xfyun.cn/h5/detail?type=question-bank-construction&amp;option=phb&amp;ch=dw24_E7x9gl) |

| 赛事任务 | 通过微调大模型根据阅读片段生成高考中英文阅读模拟题及答案，助力教育事业。 |

| 评分规则介绍 | 分数越高越好 |

| 任务提交格式说明 | 文件内容： |

| 赛程安排 | 第一阶段：7月17日——8月30日（8月30日17:00截止提交） |

| 赛事奖励 | TOP3 奖金+证书 |

| 数据集分析 | 近几年的高考语文现代阅读和英语阅读的数据集（包含原文，题目，答案）用于训练和测试 |

| 赛事要点 | 1.  对模型微调数据的理解 |

填写微调好的大模型的`resourceId`、以及对应`prompt`。

## 关于`Pandas`库和机器学习

> 在这部分介绍主要工具，并推荐1~2个相关开源项目

### `Pandas`学习推荐

> Datawhale有一个开源教程项目叫 Joyful Pandas，出版图书《pandas数据处理与分析》豆瓣链接：https://book.douban.com/subject/35998391/

> 文字版教程链接：https://inter.joyfulpandas.datawhale.club/Home.html

> 视频教程链接：https://www.bilibili.com/video/BV1tK4y177AF/

> 如果大家想仔细研读的话，可以在学习过程中，依照其中的内容进行实践；还可以多多在此处留言，也许夏令营后就会展开组队学习哟~

### 机器学习材料推荐

> Datawhale翻译了李宏毅老师的机器学习教程，可在https://linklearner.com/learn/summary/13 链接处学习查看

> 大家在本次学习过程中，可以一边将上面推荐的材料作为字典，遇到问题时通过这些材料、大模型、搜索引擎，去翻阅查看，做到理论和实践的结合，真正理解这些理论知识的价值、以及究竟要如何在真实的问题场景中使用。

![](img/ec1e65f470813d7f7107ab372401ac39.png)

## `Task2`:`baseline01`精讲（正在路上ing）学习更多吧！

## 更多QA详见 [QA & 其他彩蛋](https://datawhaler.feishu.cn/wiki/AWAQwWgPkignkUkU43zcbdfQnrh)！

> [QA & 其他彩蛋](https://datawhaler.feishu.cn/wiki/AWAQwWgPkignkUkU43zcbdfQnrh)任何想法都可以在学习群里交流或在 进行填写哟~

> 本教程主要由 写作贡献，

> 融合了多位助教的内测体验和宝贵建议。

> 如果你觉得对你有价值和启发，请在下面点赞和评论哟~

### 另外别忘了写笔记打卡呀！！~

### 及时思考、沉淀和产出能让你的学习事半功倍！

> 注：本文采用 署名—非商业性使用—相同方式共享 4.0 协议国际版许可协议进行许可。

