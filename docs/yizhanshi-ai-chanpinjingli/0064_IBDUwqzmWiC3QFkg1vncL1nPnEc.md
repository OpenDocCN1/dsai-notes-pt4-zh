
# 经典案例

> 来源：[https://v11enp9ok1h.feishu.cn/docx/COyZdp0IQoUm18x318Bcc2x6nLf](https://v11enp9ok1h.feishu.cn/docx/COyZdp0IQoUm18x318Bcc2x6nLf)

<https://v11enp9ok1h.feishu.cn/sync/VztedzIoUstVPDb6RNMctK8Unie>

本章节描述了几个 `CogVLM2` 模型能够有效处理的经典案例：

## 图像理解

![](img/9bb1efbfd59fd8938b2c6363fe577c2d.png)

解释这个梗图

这个梗图是对电影《寻找梦幻岛》中的一幕的改编。在原片中，一个名叫丹尼尔的年轻男孩遇到了困难。这个梗图将电影中的场景与机器学习中的问题——模型无法泛化到分布之外的数据——进行了幽默的对比。文本“MY MODEL CANNOT GENERALIZE OUT OF DISTRIBUTION”（我的模型不能泛化到分布之外的数据）是对丹尼尔的困境的引用，而“THEN WE'LL TRAIN IT UNTIL THERE IS NOTHING LEFT OUT OF DISTRIBUTION”（然后我们将训练它，直到没有任何分布之外的数据）则是对解决机器学习中泛化问题的一种幽默且不切实际的方法。

### 挑选西瓜

Q：哪个瓜最熟？给出理由

A：根据图片，标号为9的瓜看起来最熟。它的底部有黄色，表皮上有明显的黑色斑点，这通常是成熟的迹象。

### 户型图描述

Q：请详细描述此户型图，包括各个房间的名称和大小。

A：户型图显示了一个住宅的平面布局。从左到右，上到下，我们可以看到如下房间和区域：

- 卧室C：位于左侧，面积为18.2平方米。
- 门厅：位于中间，面积为14.7平方米。
- 卧室B：位于左侧，面积为8.2平方米。
- 过道A：连接卧室B和门厅，面积为1.6平方米。
- 厨房：位于右侧，面积为7.6平方米。
- 卫生间B：位于门厅旁边，面积为4.5平方米。
- 客厅：位于右侧，面积为34.9平方米。
- 卫生间A：位于客厅旁边，面积为4.5平方米。
- 卧室A：位于左侧，面积为18.0平方米。
- 阳台A：位于右侧，面积为7.0平方米。

在户型图的左下角还有一个表示方向的罗盘，上面标注了“N”代表北。此外，图片的右下角有一个水印，显示为“房天下Fang.com”。

### 奇怪的美食

Q：请详细描述这张图片。

A：这张图片展示了一个独特的披萨，它的形状像一只乌龟。披萨的外皮是金黄色的，上面撒有绿色的香草和一些看起来像是土豆泥的块状物。乌龟的头部和四肢是用绿色的面团制作的，眼睛是两个红色的圆形点。这个披萨放在一个纸盒里，盒子的一侧有一张白色的纸。图片的右下角有一个水印，写着“👸号：手谈姬”。

这张图片展示了一个形状像乌龟的披萨，它被放置在一个纸盒里。披萨的外皮是金黄色的，上面撒有绿色的香草和一些看起来像是土豆泥的块状物。乌龟的头部和四肢也是用披萨面团制作的，呈现出绿色。整个披萨的造型非常逼真，甚至连乌龟的眼睛和壳上的纹理都做得非常细致。这个创意披萨看起来非常美味和有趣。

### 手写体解析

Q：输出这段文本。

A：亲爱的，我想告诉你，

只要我们用心

去温暖我们的每一天，

幸福就会照耀我们。

亲爱的，我想告诉你，

只要我们以行动

每天给自己一点提升，

成功就会光顾我们。

![](img/c4fafea9df5615738fcfb35550bde22b.png)

Q：输出这段文本。

A：我喜欢你.

已经很长时间了.

我一整天都在想你.

走在路上.听着音乐.

你都会忽然出现在我眼前.

稻草人·李夏怡.

### 车票扫描

Q：用json格式列出图中的关键信息。

A：

```json
{
"始发站": "无锡",
"到达站": "杭州东",
"日期": "2001年10月05日09:00开",
"金额": "45.00元",
"座位类型": "无座"
}
```

### 统计图描述

Q：用 markdown 格式整理图中的增长率曲线。

A：| 年份 | 增长率 |
|------|--------|
| 2012 | 38.10%  |
| 2013 | 1.7%   |
| 2014 | 9.1%   |
| 2015 | 11.9%  |
| 2016 | 11.1%  |
| 2017 | 9.9%   |
| 2018 | 10.1%  |
| 2019 | 18.8%  |

### 流程图描述和讲解

Q：用列表形式描述图中的关键步骤。

A：1. 将海水放入蒸馏装置中。

2. 在加热的条件下，海水蒸发，产生水蒸气。

3. 水蒸气通过冷凝器冷却并凝结成液态水。

4. 通过收集冷凝器中的纯净水，得到实验结果。

![](img/e24a54968fbbb4c58fab7cb6d4eeaccb.png)

Q：请详细描述此图像内容。

A：这是一幅展示了十二生肖的图片。图片上方有“十二生肖”四个大字，下方则是具体的十二生肖图案，每个生肖都由一个独特的图形表示。这些图形与常见的生肖符号相似，但又有所不同，可能是对传统生肖的现代艺术诠释。从左到右、从上到下，十二生肖依次为：鼠、牛、虎、兔、龙、蛇、马、羊、猴、鸡、狗、猪。

### 历史与象形文字

Q：这是什么字？字体分别是？

A：这是“鱼”字，字体分别是：左边是甲骨文，中间是金文，右边是篆书。

### 数学题解题

Q：这道题怎么做？step by step.

A：这个直角三角形的两边分别是6和8，根据勾股定理，可以计算出斜边的长度。

使用勾股定理（a² + b² = c²），其中a和b是直角三角形的两条直角边，c是斜边。我们有：

6² + 8² = c²

36 + 64 = c²

100 = c²

接下来，我们对方程两边开平方根，得到斜边的长度：

c = √100

c = 10

所以，这个直角三角形的斜边长度是10。

### 漫画分析

Q：请详细分析这张图片及其含义。

A：这张图片展示了一个关于SOTA模型的投票场景。首先，有人提出“Who wants a SOTA model?”，然后大多数人都举手表示愿意。然而，接下来的问题是“Who wants to spend 80% of their time reviewing/cleaning/filtering their data?”，只有少数人举手。图片的含义在于，虽然大多数人可能渴望使用最新的技术和模型，但他们可能不愿意投入大量的时间和精力来维护和准备数据，这可能是因为数据准备工作的复杂性和耗时性。

Q：Please analyze this image and its meaning in detail.

A：The image depicts a scenario with a group of people gathered around a podium, where a person is speaking about SOTA (State of the Art) models. The first panel shows the audience eagerly raising their hands to show their interest in the SOTA models. The second panel reveals a more nuanced perspective, showing only a few individuals raising their hands to spend 80% of their time reviewing, cleaning, or filtering their data, highlighting the disparity between the desire for the latest technology and the reality of data preparation work.

The image conveys a message about the challenges and realities faced in data science and machine learning. While many are attracted to the shiny new toys of technology (in this case, SOTA models), the actual work of preparing and cleaning data can be a significant and time-consuming barrier. It underscores the importance of not getting caught up in the hype of new technologies but also acknowledging the foundational and often unseen work that underpins them.

## 视频理解

[basketball.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/BD9nbRBlVo7GBex6Xw1cDcbPnHb)

![](img/b9c393d98fca1635795dcbd858726738.png)

[lion.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/RgnWbnt0UoyxP2x9Cakclrkanxc)
