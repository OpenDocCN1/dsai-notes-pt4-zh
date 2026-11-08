# DeepSeek+Coze！15分钟零基础上手“金句大师”项目！

> 来源：[https://datawhaler.feishu.cn/docx/B7QFdGNlQoDfv6xQ9Wtc2mlSnPh](https://datawhaler.feishu.cn/docx/B7QFdGNlQoDfv6xQ9Wtc2mlSnPh)

# 项目简介

一键生成金句+配图+金句解析。可以选择自己的风格。

下面是项目结果示例

![](img/3c8c51cb221d19d80c6c181e8f153fd6.png)

![](img/7f032431c3fedea04eaeb3ce0bfb41c4.png)

![](img/a020d3aa2034c8b0cbf5cc23bcd2f8f5.png)

![](img/ceefc3c0592680e80decc1cd9b047b4b.png)

# 为什么想做这个项目？

**我**觉得大模型的想象力很丰富，有时候说话会非常幼稚，但有时候又能创造出人类所理解不了的深邃艺术。所以用大模型来生成金句，是一件非常刺激的事情。

# 项目思路

![](img/1533aff6f1070b9fe1841226e2792366.png)

## Step1 输入

给到模型输入，输入内容为两部分：

其中count（金句数量）是必填项；fenge(语言风格)是选填项。

## Step2 金句大师！

![](img/9eb9eedf6cdd5d785e6fa5c6cdecd6ff.png)

其中系统提示词部分和用户提示词部分设计比较简单，可以细化进行提示词工程设计让效果更好。

## Step3 图像生成员！

![](img/efec874707292ec82b25834364ca1562.png)

同样的，提示词工程设计比较简单，可以进行优化！本项目只实现最基本的功能！读者也可以自己在图像生成员前面加一个提示词设计师，让效果更好！

## Step4 画板设计

画板设计如下，这里相当于是前端的一些组建拖拽。

![](img/c015d564b16eff97aef317da4eb6b9d7.png)

我们需要获得的是输出的data；需要注意的是，画板设计的输入图像为批处理中图像生产员的输出。

## Step5 输出

![](img/1e42fec3ad39e9a4c3a667c32bf2db44.png)

输出承接画板的输出即可data。然后我们可以进行测试，发现项目没问题，可以正常运行，即可完成”金句大师“项目。

## Step6 项目发布

![](img/6c7ce06d4b0e70fb865c7215b589a08d.png)

Coze平台有非常完善的发布体系，其余的平台如果想发布需要依据各个平台的文档完成授权过程。发布后等待10-15分钟即可完成。

最后还可以换上自己喜欢的背景即可完成！

![](img/3e71ee69a34afa5d7b46800790561525.png)

体验金句大师的项目链接：https://www.coze.cn/s/OkoAiiocb-E/

![](img/5fc429ffa70a77f6bfc3987bdf3d7b16.png)

**相关示例：**

![](img/8a5cbd161a4cd3165a60aed4f9ade4cb.png)

![](img/1832398fb3b3c2e0e1e8ce283f99f72f.png)

![](img/700e12c3dda5089bae8b08449251f402.png)

# 可以优化的地方？

1.金句大师的提示词工程部分可以优化。

2.图像生成员的提示词可以详细化。

3.画板部分可以优化。

4.可以做成一个应用生成UI。

5.可以考虑和📕之类的联动做到自动发布。